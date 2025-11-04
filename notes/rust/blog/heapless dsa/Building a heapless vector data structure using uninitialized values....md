**Title:** Building a heapless vector data structure using uninitialized values with `MaybeUninit<T>` instead of default values with `Option<T>`. Creating safe abstractions over unsafe code with safe APIs.

> **TLDR**: Heapless Vector in no_std  Rust.
> Rust stores data on the stack (fast, fixed-size), heap (growable but needs alloc), or static (program-long); for memory-tight embedded systems, use `#![no_std]` to ditch heap-alloc like `Vec<T>` and build a "heapless vector" on stack with a fixed-size array `[T; N]` tracked by `len`. Start safe with `Option<T>` (extra space for None tracking), then optimize to `MaybeUninit<T>` (exact T size, no overhead) using unsafe methods like `write()`, `as_ptr()`, `assume_init_read()`, and custom `Drop`—gains 1.5–2x speed and half memory overhead, but requires careful invariants in unsafe Rust to avoid undefined behaviour. (Generated with AI, Grok.)

In this article, I'll teach you how to build a heapless vector data structure with the example of one I already built. I'll share the performance benefits and unsafety to look out for and avoid when using `MaybeUninit<T>` over `Option<T>`. Lastly, I'll show you how to build safe APIs on top of unsafe Rust code to ensure "safe unsafe code" in Rust.

A little primer on Rust memory models before we begin. Though to fully comprehend this article you may require practical understanding of Rust memory models—what's stored where, how, and what for—here's a little primer incase you're new to this.

## Rust's Memory Model
Rust models memory in three main spaces or regions; the *stack*, the *heap*, and *static memory*.

The *stack* is a short-lived, fixed-size memory region with LIFO (Last In First Out) movement. The stack stores data of function-local variables, primitive types, function parameters, and fixed-size collections like an array. When a function is called a contiguous chunk of memory is allocated to the top of the stack known as a *frame*.

The *heap* is a dynamic, growable region of contiguous memory. Collections and data in the heap are growable and variably-sized. The heap isn't tied to the current call stack (i.e. `main` function stack frame) of the program, meaning data in the heap can live forever until explicitly deallocated.

*Static memory* is a catch-all term for several closely related regions located in the file your program is compiled into. These regions are automatically loaded into your program's memory when that program is executed. Values in static memory live for the entire execution of your program (as described by Jon Gjengset in Rust for Rustaceans). Static memory stores source code, `'static` lifetime values, and values in `static` variables. Static memory is read-only by default, therefore immutable. Mutating data in static memory is unsafe (e.g. mutating static variables using `static mut`).

How does data behave in these different memory regions? We've already seen some examples in the paragraph on *the heap*, discussing further:
- Data in the stack is allocated/deallocated automatically (LIFO order) when entering/exiting scopes (i.e. function stack frames). Access is very fast. Data size must be known at compile time. No manual management of stack data in Rust (*this is what we'll be building in this article using* `Vec<T>` *as an example*).
	- Example: `let x: i32 = 42;` – `x` lives on the stack during its scope.
- Data in the heap is allocated at runtime. Generally slower than the stack due to pointer indirection and potential fragmentation. Is managed using smart pointers (e.g. `Box<T>`, `Rc<T>`, `Arc<T>`, etc.) with Rust's ownership system ensuring safety.
	- Example: `let v = vec![1, 2, 3];` – the vector's buffer is on the heap.
- Data in static memory is allocated at compile time into the program's binary with a fixed memory address. Immutable by default, as we talked about, and mutable with unsafe code. The data's lifetime starts when the program starts and ends when the program ends.
	- Example: `static HELLO: &str = "Hello";` – `HELLO` is stored statically and accessible anywhere.

Hope you enjoyed that primer because now we move onto answering the question: why would we need a heapless vector data structure in the first place?

## Rust without the Standard Library
There's a practice in Rust known as "Rust without the standard library" communicated in code with the crate attribute `#![no_std]`. To answer our earlier question, we need to understand this practice. 

Usually when you write, build and run Rust code, you most likely do it on a platform with an operating system and a sizeable amount of both RAM memory and disk space (i.e. your laptop or remote server). And sometimes, you write Rust code that is to be executed in environments without an operating system and with limited memory and storage size. This is usually the case when you build for embedded systems, where the code is closer to the bare metal (metaphorically to mean a microcontroller or an SoC). Furthermore, in these "unorthodox" environments, such as those without an operating system, or those without the ability to dynamically allocate memory; how does Rust, as a low level systems programming language, enable us build software that runs in these environments? Through the "Rust without the standard library" (`#![no_std]`) practice. Let's elaborate further on how it works.

By default, Rust comes packaged with the standard library `std` and library preludes that bring into scope popularly-used types like `Option<T>`, `Result<T>`; traits like `Iterator`, `Debug`, `Clone`, `Drop`; functions like `std::mem::drop`, `std::mem::size_of`; and macros like `std::println!` and `std::dbg!`. What you may not know about the standard library is that it is a composite makeup of two of Rust's more fundamental libraries `core` and `alloc`. In fact, many types and functions in `std` are just re-exports from those two libraries.

`alloc` is Rust's dynamic memory allocation library. It's what makes heap allocation data types like `Box<T>` and `Vec<T>` work by either manually implementing a memory allocator with the `GlobalAlloc` trait or using the system allocator (which is usually the one dictated by the standard C library). Without `alloc`, collections, smart pointers, and dynamically-allocated strings `String` in the standard library `std` won't work.

The `core` library sits at the bottom of the Rust library pyramid and contains functionality that only depends on the Rust language itself and the hardware the Rust program is running on—meaning `core` doesn't depend on anything else. `alloc` sits on top of `core` in the Rust library hierarchy. So you can begin to see the order of library dependency:
`std -[depends on]-> alloc -[depends on]-> core`.

When we use the crate attribute `#![no_std]`, we are saying we want to change default action of Rust to instead remove access to `std` and `alloc` and only keep `core`. This ensures `std` is not compiled with source code and changes the Rust library prelude from `std::prelude` to `core::prelude`. This doesn't mean you can't access `std` in a `#![no_std]` environment, you still can using `extern crate std` but accessing `std` will have to be explicit and dependent on if the target platform (i.e. the environment where your Rust code will run) allows `std`. The same rule applies for `alloc` in the sense that Rust allows you the ability to dynamically allocate memory in a `#![no_std]` environment, that said, this action also has to be explicit and is dependent on two things:
1. you manually implementing a memory allocator if the target platform does not have one,
2. if the target platform allows implementation of a system allocator. A couple things might hinder this, one example is if there's too little memory to permit dynamic memory allocation.

Now you see, in embedded environments (i.e. microcontrollers and SoCs) where limitations on compute and memory are prevalent we must make do with data stored on the stack and static memory. We cannot use the heap memory because in such environments we lack memory allocation. That means bye-bye to flexible data structures like `Vec<T>`. Looking at what we have available by default in Rust `core`, the closest stack data structure akin to `Vec<T>` is the `core::array` (aka. array) primitive type, typed in code as `[T; N]`. But we know the limitations of `array` compared to `Vec<T>`, for example;
- `array` must be fixed-sized to `N`, `Vec<T>` is variably-sized,
- denoting an array as `[T; N]` means `T` must be `Copy` (shorthand lingo for `T` must implement the `Copy` trait),
- `array` creates a fixed-size contiguous space where each space is data stored in the stack memory, while `Vec<T>` allocates memory in the heap and returns a pointer (off on a tangent here: understanding the difference between a value, variable, and pointer in Rust greatly contributes to improving your understanding of pointers),
- plus a couple more differences that are not important to our use case otherwise would mention where applicable.

Despite these differences and limitations between `array` and `Vec<T>`, as the more similar data structures, it'd be nice to be able be create a `Vec`-like data structure that fundamentally functions like an array (therefore, without needing heap allocation) but inherits (not to be confused with OOP Inheritance as Rust doesn't do that) some functionality of `Vec<T>`—a *heapless vector* data structure. That's what we'll build next.

## Building a Heapless Vector Type
Now you understand *the what* and *the why*, let's talk about *the how*.

To build a *heapless vector* you must allocate enough memory upfront (in the form of the `N` size of elements in a `[T; N]` array)—either in static memory or in a function stack frame—for the largest number of elements you expect the vector to be able to hold, and then augment it with a `usize` that tracks how many elements it currently holds. To push to the vector, you write to the next element in the (statically sized) array and increment a variable that tracks the number of elements. If the vector's length ever reaches the static size, the next push fails.

First, we'll do this using `Option<T>` to depict allocating memory upfront using safe Rust with a minimal performance overhead compared to the next option (as time-space complexity is dependent on `N` elements). Then, using `MaybeUninit<T>` with uninitialized memory, using unsafe Rust, with better performance. In both cases, we'll work with `const` generics. Finally, let's dig into some code.

### Using `Option<T>`

```rust
#[derive(Debug)]
pub struct ArrayVec<T, const N: usize>
where
    T: Copy
{
    values: [Option<T>; N],
    len: usize,
}

impl<T, const N: usize> ArrayVec<T, N>
where
    T: Copy
{
    pub fn new() -> Self {
        ArrayVec {
            values: [None; N],
            len: 0,
        }
    }
    
    pub fn try_push(&mut self, t: T) -> Result<(), T> {
        if self.len == N {
            return Err(t);
        }
        self.values[self.len] = Some(t);
        self.len += 1;
        Ok(())
    }
}
```

`ArrayVec` is generic over both the type of its elements, `T`, and the maximum number of elements, `N`. `ArrayVec` is represented as an array of `N` optional `T`s. With `ArrayVec` generic over `const N: usize`, `N`, must be known at compile time allowing the vector to be stored on the stack while using runtime information to manage how we access the array.

Testing `ArrayVec` with some code, we can see how it works and how to interact with it.

```rust
const SIZE: usize = 5;

fn main() {
	let mut arr_vec1 = ArrayVec::<i32, SIZE>::new();
    println!("{:?}", arr_vec1); // view ArrayVec after init
    
    let mut count = 0;
    
    for i in 0..SIZE {
        count = 10 + i as i32;
        arr_vec1.try_push(count).unwrap();
    }
    println!("{:?}", arr_vec1); // view ArrayVec after pushing elements
    
    // Pushing elements beyond ArrayVec len; should return Err
    let err = arr_vec1.try_push(count + 1);
    println!("{:?}", err);
    
    // arr_vec does not change
    println!("{:?}", arr_vec1);
}
```

Running the code above returns:

```
# Stdout:
ArrayVec { values: [None, None, None, None, None], len: 0 }
ArrayVec { values: [Some(10), Some(11), Some(12), Some(13), Some(14)], len: 5 }
Err(15)
ArrayVec { values: [Some(10), Some(11), Some(12), Some(13), Some(14)], len: 5 }
```

> Notice in our code, we haven't added the `#![no_std]` crate attribute yet. We will do that in the next section on  `MaybeUninit<T>`.

We can see that `Option<T>` works as the optional type that holds a valid value when we have successfully pushed a value, `T`, and `None` when we haven't. So what is the performance overhead caused by `Option<T>` here? 

Again, working in a limited environment like that of embedded systems, we really want to be conservative with our use of storage (if any) and memory space. That means storing `None` in, for example, an array of `1_000_000` `N` values, where each `None` is byte-aligned to the size of `T` if `T` does not allow *niche optimization* for `Option<T>`, is wasteful compared to the alternative and carries performance overhead for the array, and thus our `ArrayVec` implementation.

> For brevity, I'm not going to talk about type alignment and layout in Rust but it goes a long way to help you comprehend, for example, what it means for "`None` to be byte-aligned to the size of `T`." Therefore, if you don't know about it I highly recommend you learn of it.

Niche optimization is an optimization technique implemented by the Rust compiler `rustc` on `Option<T>` if `T` cannot be null or zeroed. It's a technique where the wrapper `Option` type uses the same size as `T` by assigning it's `None` with the `0` bit. That way if `T` is 8 bytes, `Option<T>` will also be 8 bytes instead of 16 bytes.

If the previous paragraphs were tough to understand, It might help to study type alignment and layout in Rust, as well as niche optimization by the Rust Compiler (`rustc`) in more detail.

- You can experiment with the code of a **heapless vector type** with `Option<T>` on the Rust playground via the link: https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=a32ad95d9ad32d06d69bb7a612e57c4b
- You can also find the whole code on this GitHub Gist: https://gist.github.com/allwelldotdev/10630be265c9548bfb0c591767afedf2

Let's look at the more performant but unsafe alternative with `MaybeUninit<T>`.

### Using `MaybeUninit<T>`
`MaybeUninit<T>` is a `union` type, and `union` types in Rust are rare and mostly used for interfacing with `C` code through foreign function interfaces (FFI). Accessing the field of a `union` in Rust is unsafe, therefore, we'll be introducing the use of unsafe code to implement `MaybeUninit<T>`.

What is the function of `MaybeUninit<T>`, how is it useful in the concept of building a heapless vector type?

#### What does *uninitialized* mean?
According to stdlib docs, `MaybeUninit<T>` is a wrapper type to construct uninitialized instances of `T`. What does "uninitialized" mean? I'll use the example of `let` value-to-variable assignment to illustrate the meaning of initialized vs. uninitialized instances to help you *really* understand what `MaybeUninit<T>` does.

Check out the code below.

```rust
let init_var: i32 = 10; /* `init_var` is a variable that holds a 32-bit signed integer and is immediately initialized to the value of 10. */

let uninit_var; /* In contrast, `uninit_var` is a variable that, at this point,
holds nothing (actually holds what are known as "garbage bytes" - invalid,
overwritable data in memory). Meaning, `uninit_var` points to *uninitialized*
memory.

At this point of the code, in safe code, Rust would *not* allow you use
`uninit_var` in any kind of computation that requires `uninit_var` to
be initialized. The compiler will complain that `uninit_var` is uninitialized
and using it would result in unexpected/undefined behaviour (UB). To use
`uninit_var`, we must initialize it with the appropriate value. */

uninit_var: u8 = 255; /* Finally, we initialize `uninit_var` with an 8-bit
unsigned integer of value 255. Rust's powerful type inference sets `uninit_var`
to be of type `u8`. If you try to assign `uninit_var` to a value of the wrong
type, the compiler will panic. */

/* Below is another example of Rust's type inference enforcing appropriate value
assignment. */
let uninit_var: i8; /* Variable holds no value and points to unitialized memory of
type 8-bit signed integer. */
uninit_var = 255; /* ❌ ERROR: You must initialize `uninit_var` with the
appropriate value. `i8::MAX` is 127. */
```

Therefore, in a nutshell, uninitialized memory are regions that haven't been filled with valid data yet. Accessing uninitialized memory through normal types like `i32` or `String` leads to UB because the compiler assumes all values are properly initialized (e.g., references are not null, `bool`s are `true` or `false`, and padding bytes in structs are not garbage).

Here's another a more elaborate definition: `MaybeUninit<T>` lets you represent potentially uninitialized data without immediate UB. It's a `union` type with `#[repr(transparent)]`, meaning it has the *exact same size, alignment, and application binary interface (ABI)* as T—no overhead. Unlike `Option<T>`, it doesn't track initialization at runtime (no discriminant), so you must manually ensure safety via invariants (promises you make in unsafe code). Dropping a `MaybeUninit<T>` never calls `T`'s destructor; that's your job if it's initialized.

According to stdlib docs, "`MaybeUninit<T>` serves to enable unsafe code to deal with uninitialized data. It is a signal to the compiler **indicating that the data here might _not_ be initialized**. The compiler then knows to not make any incorrect assumptions or optimizations on this code. You can think of `MaybeUninit<T>` as being a bit like `Option<T>` but without any of the run-time tracking and without any of the safety checks."
Learn more about `MaybeUninit<T>` from the [stdlib docs](https://doc.rust-lang.org/std/mem/union.MaybeUninit.html).

Having understood what `MaybeUninit<T>` is, let's see it's function in building a heapless vector type.

Rewriting the earlier implementation of `ArrayVec` from `Option<T>` to `MaybeUninit<T>`:

```rust
#![no_std] // Explicity stating this crate does not use `std`

mod arrayvec {
	use core::mem::MaybeUninit;

	#[derive(Debug)]
	pub struct ArrayVec<T, const N: usize> {
		values: [MaybeUninit<T>; N],
		len: usize,
	}
	
	impl<T, const N: usize> ArrayVec<T, N> {
		/// Creates a new empty ArrayVec with an array of uninitialized `T`
		/// and `len` = 0.
		pub fn new() -> Self {
			ArrayVec {
				// values: unsafe { MaybeUninit::uninit().assume_init() },
				// Below is same as the commented code above but safer.
				values: [const { MaybeUninit::uninit() }; N],
				len: 0,
			}
		}
		
		/// Pushes `T` if all `N` elements have not been initialized;
		/// else returns `Err(T)`.
		pub fn try_push(&mut self, value: T) -> Result<(), T> {
			if self.len == N {
				return Err(value);
			}
			self.values[self.len].write(value);
			self.len += 1;
			Ok(())
		}
	}
}
```

First, notice the newly added crate attribute `#![no_std]` that signifies the code is built for an environment without support for the Rust standard library. Next, we move `ArrayVec` and its implementations into a `mod` to mimic an organized project structure and visibility. `values` is now an array of `N` maybe uninitialized values `MaybeUninit<T>`. `len` continues to track the number of initialized elements.

Following similar implementations on `ArrayVec` with `Option<T>`, now with `MaybeUninit<T>`:
- `new` introduces allocation to static memory with `const` and the `MaybeUninit::uninit()` associated function to return an array of uninitialized instances of `T` to the `values` field. In place of the safe variant we could also use the unsafe variant with `.assume_init()` on `MaybeUninit<T>` to create and assign an array of `N` uninitialized instances of `T` to the `values` field, as seen in the commented code, because it returns the same value. Let me explain.
	- When you declare a field like `values: [MaybeUninit<T>; N]` in a struct, Rust doesn't require (or perform) any explicit initialization of the array's contents to a "valid" state for `T` at construction time. Instead the array starts in a raw, uninitialized memory state (i.e., whatever **garbage bytes** happen to be on the stack at that moment), and the `MaybeUninit<T>` wrapper semantically interprets those bytes as "uninitialized" without any runtime cost or safety violation. Hence the reason I used the safe variant instead and commented the unsafe variant. Meaning the unsafe variant was safe to use in that context anyway but I chose to go with safe Rust. Other places where I use `unsafe`, I document (i.e. comment) the safety invariants that make using the `unsafe` code safe.
- `try_push` acts like `.push` on `Vec<T>`, takes in and writes `T` directly to uninitialized memory in the array (if all `N` elements have not been initialized, otherwise returns `Err(T)`) and increments `len` to track number of initialized elements.

#### Add `get` and `pop` Methods
In full vector style, let's add more useful methods to `ArrayVec` like `get`, `pop`, and a getter `len`.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	impl<T, const N: usize> ArrayVec<T, N> {
		// ...earlier code.
		
		/// Returns a reference to the element at `index` if within bounds
		/// and initialized; else returns `None`.
		///
		/// SAFETY: Unsafe internally: Assumes the first `len` slots are
		/// initialized.
		pub fn get(&self, index: usize) -> Option<&T> {
			if index >= self.len {
				return None;
			}
			unsafe { Some(&*self.values[index].as_ptr()) }
		}
		
		/// Pops the last value, if any, returning owned `T` (or `None`
		/// if empty).
		///
		/// SAFETY: Uses `.assume_init_read()` to extract (read) and mark
		/// memory as uninitialized (this is also helped by the
		/// decrementing of `self.len` for the next `try_push`).
		pub fn pop(&mut self) -> Option<T> {
			if self.len == 0 {
				return None;
			}
			self.len -= 1;
			Some(unsafe { self.values[self.len].assume_init_read() })
		}
		
		/// Getter for current length.
		pub fn len(&self) -> usize {
			self.len
		}
	}
}
```

While implementing `get` and `pop` methods on `ArrayVec`, we use `unsafe` and document safety invariants (i.e. guarantees) that describe safety rules we upheld to ensure a safe implementation of unsafe code, and we introduced methods like `.as_ptr()` and `.assume_init_read()` that allow us to reach into the *initialized instances* of `MaybeUninit<T>` and perform computation on them.

In `get` we take in an index value that points to the array data we want to get. Before we perform the get operation on the `values` array we ensure we're only getting initialized indexed values by using the `if` statement to return `None` otherwise. After returning an initialized indexed value from the `values` array, we obtain it's immutable raw pointer using `.as_ptr()`, dereference the raw pointer to access its value using `*` (this action requires `unsafe`), then return a shared reference `&` of the value in the `Some` variant.

`pop` takes a mutable reference of `self` because it mutates `self.len`, checks before popping if the array is empty (returns `None` if empty), and decrements `self.len` to track the un-initialization of formerly initialized memory caused by the `.assume_init_read()` method call on `MaybeUninit<T>`. `.assume_init_read()` is an unsafe function that acts similar to `core::ptr::read` or `std::ptr::read` in that it performs a bitwise copy of `T` whether `T` is `Copy` or not. This means if `T` is not `Copy` it moves `T`. Such that if the memory location of `T` is accessed after `.assume_init_read()` was called on that same memory location, it will result in UB (similar to a use-after-free error). This is why `.assume_init_read()` is perfect for an array pop operation on `MaybeUninit<T>` values and returns owned `T` in a `Some` variant. To ensure the use of `unsafe` code is safe, we first check the array is not empty guaranteeing it contains initialized instances of `T` that we can call `.assume_init_read()` on to extract `T`.

#### Add Destructor
Lastly, remember what I said earlier in the article when I described `MaybeUninit<T>`, I said, "dropping a `MaybeUninit<T>` never calls `T`'s destructor; that's your job if it's initialized." Using this knowledge, next, we implement `Drop` for `ArrayVec` to deallocate the `N` initialized instances of `MaybeUninit<T>` in the `values` array.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Implement Drop for ArrayVec to safely deallocate initialized elements.
	impl<T, const N: usize> Drop for ArrayVec<T, N> {
		fn drop(&mut self) {
			/* SAFETY: Explicitly drops the first `len` initialized elements.
			Therefore, no double frees. */
			for i in 0..self.len {
				unsafe {
					self.values[i].assume_init_drop();
				}
			}
		}
	}
}
```

We implement a destructor for `ArrayVec` that the compiler calls to drop (i.e. free or deallocate) `ArrayVec` when it goes out of scope.

Inside the `drop` function, we iterate through the `self.values` array and call `.assume_init_drop()` on each *initialized instance* of `MaybeUninit<T>` which drops the contained value `T` in place. The safety invariant satisfied here is we explicitly drop only the first `self.len` initialized elements, we don't touch the uninitialized `T`. Because calling `.assume_init_drop()` when `T` is not yet fully initialized causes UB.

#### Converting to a `slice`
We come quite far already but there's just a little more to do to enable us debug the values pushed into `ArrayVec`. If we try to use `ArrayVec` as it is in this moment, we'll receive an incoherent output. To help your understanding, here's a code example:

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

mod arrayvec { /* ...`ArrayVec` implementation. */ }

const CAP: usize = 5;

fn main() {
	// Create ArrayVec with 16-bit unsigned integer and array of `CAP` size.
	let mut arr_vec = ArrayVec::<u16, CAP>::new();
	let mut count: u16 = 0;
	
	// Iterate and push values into ArrayVec.
	for i in 0..(CAP - 2) {
		count = 10 + i as u16;
		arr_vec.try_push(count).unwrap();
	}
	
	// Print ArrayVec to stdout for debugging.
	std::println!("{:?}", arr_vec);
}
```

If I ran this code with `cargo run` I'd get the following output:

```
# Stdout:
ArrayVec { values: [MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>], len: 3 }
```

Notice how the size of the array is `5` yet the number of initialized elements is `3` (tracked by the `len` field of `ArrayVec`), and we cannot see the values pushed into the array instead we see `MaybeUninit<u16>`. That is precisely the reason why we need to be able to convert `ArrayVec` to a `slice` so we can peep into it's initialized values and debug what was inserted or computed in there.

> What's a `slice`? A `slice` is a primitive type in Rust that is a *dynamically-sized* view into a contiguous sequence, denoted by `[T]`. Yes a slice is dynamically-sized though that has nothing to do with heap allocation. A slice is dynamically-sized because a slice is a fat (or wide) pointer that holds information about a location in memory and a length. In other words, a slice is a view into a block of memory represented as a pointer and a length. Learn more about `slice`s in Rust via the [stdlib docs](https://doc.rust-lang.org/std/primitive.slice.html).

Using a `slice`, we can take a view into `ArrayVec` for its initialized elements. To do that, we implement methods on `ArrayVec` that allow us convert it into a slice.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Add methods to convert ArrayVec into a slice.
	impl<T, const N: usize> ArrayVec<T, N> {
		// ...earlier code.
		
		/// Return a slice using `slice::from_raw_parts()`. Returns a slice
		/// over initialized elements (i.e. first `self.len` slots).
		///
		/// SAFETY: Unsafe internally, but safe API: assumes invariant holds.
		/// Length points to valid length of init elements.
		pub fn as_slice(&self) -> &[T] {
			unsafe {
				core::slice::from_raw_parts(
					self.values.as_ptr() as *const T, self.len
				)
			}
		}
		
		/// Returns a mutable slice over initialized elements.
		///
		/// SAFETY: Length param is valid length of init elements.
		pub fn as_mut_slice(&mut self) -> &mut [T] {
			unsafe {
                core::slice::from_raw_parts_mut(
                    self.values.as_mut_ptr() as *mut T, self.len
                )
            }
		}
	}
}
```

`core::slice::from_raw_parts()` and `core::slice::from_raw_parts_mut()` are unsafe functions therefore you must call them inside `unsafe` blocks. Both take in a raw pointer (immutable and mutable respectively) and the length of the slice which corresponds, in our case, to the length of initialized elements which we track with `self.len`.

Modifying the `main()` function from before slightly (as seen below) we'll be able to debug the values pushed in.

```rust
// ...earlier code.

fn main() {
	/* ...earlier code.
	After creating ArrayVec and pushing values into it.
	See similar earlier code for this missing section. */
	
	// Repeat initial ArrayVec print.
	std::println!("{:?}", arr_vec);

	// Print ArrayVec *now as slice* to stdout for debugging.
	std::println!("---\nInit values: {:?}", arr_vec.as_slice());
}
```

Returns:

```
# Stdout:
ArrayVec { values: [MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>, MaybeUninit<u16>], len: 3 }
---
Init values: [10, 11, 12]
```

As you can see, now we can properly debug values pushed or computed in `ArrayVec`.

Let's conclude with a couple final tests on `ArrayVec` to showcase its function.

#### Testing
Let's test our heapless vector type so far.

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

mod arrayvec { /* ...`ArrayVec` implementation. */ }

const CAP: usize = 5;

fn main() {
	let mut arr_vec = ArrayVec::<i32, CAP>::new();
	std::println!("{:?}", arr_vec); // View init ArrayVec

	let mut count;

	// Push a few elements
	for i in 0..(CAP - 2) {
		count = 1 + i as i32;
		arr_vec.try_push(count).unwrap()
	}
	std::println!("{:?}", arr_vec); // View pushed elements

	// Return elements in initialized index.
	let arr_els = arr_vec.as_slice();
	std::println!("---\nInit values: {:?}", arr_els);

	// Pop from MaybeUninit ArrayVec; if uninit, return None.
	std::println!("---\nArrayVec `len` before pop: {}", arr_vec.len());
	std::println!("Popped value: {:?}", arr_vec.pop());
	std::println!("ArrayVec `len` after pop: {}", arr_vec.len());

	// TEST: Add more elements beyond `CAP` size for ArrayVec;
	// `try_push` should escape and return with Err.
	let arr_len = arr_vec.len();
	count = *arr_vec.get(arr_len - 1).unwrap(); /* I can deref the pointer
	for the value here without moving the value because I know it's a
	primitive value which enables bitwise copy. */
	let mut arr_err_els: ArrayVec<Result<(), i32>, CAP> =
		ArrayVec::new();

	for _ in arr_len..(CAP * 2) {
		count += 1;
		if let Err(value) = arr_vec.try_push(count) {
			arr_err_els.try_push(Err(value)).unwrap();
		}
	}
	std::println!("---\nFilled ArrayVec: {:?}", arr_vec.as_slice());
	std::println!("Err beyond ArrayVec: {:?}", arr_err_els.as_slice());
}
```

Returns:

```
ArrayVec { values: [MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>], len: 0 }
ArrayVec { values: [MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>, MaybeUninit<i32>], len: 3 }
---
Init values: [1, 2, 3]
---
ArrayVec `len` before pop: 3
Popped value: Some(3)
ArrayVec `len` after pop: 2
---
Filled ArrayVec: [1, 2, 3, 4, 5]
Err beyond ArrayVec: [Err(6), Err(7), Err(8), Err(9), Err(10)]
```

## In conclusion
We've come a long way to learn how to build a heapless vector data structure using uninitialized values. From understanding how Rust models memory regions, to learning about how to configure Rust for unorthodox or constrained environments using the Rust without the Standard Library `#![no_std]` practice, to building a heapless vector type `ArrayVec` using `Option<T>`, and then to optimizing `ArrayVec` further by replacing `Option<T>` default values with `MaybeUninit<T>` uninitialized values in the array.

In optimizing `ArrayVec` to `MaybeUninit<T>` from `Option<T>`, here are some of the advantages and tradeoffs.

**Advantages**
- **Performance:**
	- **Reduced memory footprint**. Exact `size_of::<T>()` per element/index in array (no discriminant overhead); halves space for non-niche types like `i32` (4 bytes vs. 8 bytes), cutting cache misses in large arrays.
	- **Faster Access & No Branching**. Direct `ptr` reads/writes via `unsafe { Some(&*self.values[index].as_ptr()) }`; skips `Option`'s runtime `is_some()` checks. According to benchmark tests, this yields 1.5–2x speed in loops (e.g., 23ms vs. 62ms for 100M elements in an array).
- **Ergonomic**:
	- **Simpler hot-path code**. Cleaner push/pop without enum wrapping/unwrapping.
- **Other**:
	- **Heapless/Embedded Fit**. Zero-cost abstraction for `#![no_std]`; enables const init and SIMD-friendly uninit masking, ideal for real-time systems.

**Tradeoffs**
- **Safety risk**. Introduces unsafe code; UB if invariants violated (e.g., reading uninit slots)—needs Miri testing vs. `Option`'s compile-time guards.
- **Ergonomic: More boilerplate.** Manual drops (with `assume_init_drop()`) and init tracking (with `self.len`); less intuitive for beginners than `Option`'s automatic discriminant and destructor (`Drop`).
- **Maintenance overhead**. Needing explicit lifetime management (with `impl<T, const N: usize> Drop for ArrayVec<T, N>`).

This effort shows you can build sophisticated data structures using only the stack and static memory, omitting heap allocation. This kind of data structure is useful in embedded systems programming where heap allocation is often unavailable.

- You can experiment with the code of a **heapless vector type** with `MaybeUninit<T>` on the Rust playground via the link (PS. In the code file, you'll see I've added more features to `ArrayVec` like the ability for iteration and so on): https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=7b80863ed153627d1d86a6ee41a42167
- You can also find the whole code on this GitHub Gist: https://gist.github.com/allwelldotdev/39c817ca8d40fe5aeb808eb6301a18ff

In another article, I'll extend this knowledge by sharing how to make `ArrayVec` and its fundamental types (`&ArrayVec` and `&mut ArrayVec`) iterable (i.e. how to use `ArrayVec` in a `for` loop in Rust and more). Stay connected for that!

---
👏🏾 Kudos for finishing the article.

Hi there!  I'm Allwell, a passionate Rust developer currently working from Lagos, Nigeria. You can connect with me on X ([`@allwelldotdev`](https://x.com/allwelldotdev/)) or LinkedIn ([Allwell Agwu-Okoro](https://linkedin.com/in/allwelldotdev/)). Let's build and learn Rust together.


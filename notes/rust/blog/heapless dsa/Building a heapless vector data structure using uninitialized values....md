**Title:** Building a heapless vector data structure using uninitialized values with `MaybeUninit<T>` instead of default values with `Option<T>`. Creating safe abstractions over unsafe code with safe APIs.

In this article, I'll teach you how to build a heapless vector data structure with the example of one I already built. I'll share the performance benefits and unsafety to look out for and avoid when using `MaybeUninit<T>` over `Option<T>`. Lastly, I'll show you how to build safe APIs on top of unsafe Rust code to ensure "safe unsafe code" in Rust.

A little primer on Rust memory models before we begin. Though to fully comprehend this article you may require practical understanding of Rust memory models--what's stored where, how, and what for--here's a little primer incase you're new to this.

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

The `core` library sits at the bottom of the Rust library pyramid and contains functionality that only depends on the Rust language itself and the hardware the Rust program is running on--meaning `core` doesn't depend on anything else. `alloc` sits on top of `core` in the Rust library hierarchy. So you can begin to see the order of library dependency:
`std -[depends on]-> alloc -[depends on]-> core`.

When we use the crate attribute `#![no_std]`, we are saying we want to change default action of Rust to instead remove access to `std` and `alloc` and only keep `core`. This ensures `std` is not compiled with source code and changes the Rust library prelude from `std::prelude` to `core::prelude`. This doesn't mean you can't access `std` in a `#![no_std]` environment, you still can using `extern crate std` but accessing `std` will have to be explicit and dependent on if the target platform (i.e. the environment where your Rust code will run) allows `std`. The same rule applies for `alloc` in the sense that Rust allows you the ability to dynamically allocate memory in a `#![no_std]` environment, that said, this action also has to be explicit and is dependent on two things:
1. you manually implementing a memory allocator if the target platform does not have one,
2. if the target platform allows implementation of a system allocator. A couple things might hinder this, one example is if there's too little memory to permit dynamic memory allocation.

Now you see, in embedded environments (i.e. microcontrollers and SoCs) where limitations on compute and memory are prevalent we must make do with data stored on the stack and static memory. We cannot use the heap memory because in such environments we lack memory allocation. That means bye-bye to flexible data structures like `Vec<T>`. Looking at what we have available by default in Rust `core`, the closest stack data structure akin to `Vec<T>` is the `core::array` (aka. array) primitive type, typed in code as `[T; N]`. But we know the limitations of `array` compared to `Vec<T>`, for example;
- `array` must be fixed-sized to `N`, `Vec<T>` is variably-sized,
- denoting an array as `[T; N]` means `T` must be `Copy` (shorthand lingo for `T` must implement the `Copy` trait),
- `array` creates a fixed-size contiguous space where each space is data stored in the stack memory, while `Vec<T>` allocates memory in the heap and returns a pointer (off on a tangent here: understanding the difference between a value, variable, and pointer in Rust greatly contributes to improving your understanding of pointers),
- plus a couple more differences that are not important to our use case otherwise would mention where applicable.

Despite these differences and limitations between `array` and `Vec<T>`, as the more similar data structures, it'd be nice to be able be create a vec-like data structure that fundamentally functions like an array (therefore, without needing heap allocation) but inherits (not to be confused with OOP Inheritance as Rust doesn't do that) some functionality of `Vec<T>`--a *heapless vector* data structure. That's what we'll build next.

## Building a Heapless Vector Type
Now you understand *the what* and *the why*, let's talk about *the how*.

To build a *heapless vector* you must allocate enough memory upfront (in the form of the `N` size of elements in a `[T; N]` array)--either in static memory or in a function stack frame--for the largest number of elements you expect the vector to be able to hold, and then augment it with a `usize` that tracks how many elements it currently holds. To push to the vector, you write to the next element in the (statically sized) array and increment a variable that tracks the number of elements. If the vector's length ever reaches the static size, the next push fails.

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

```bash
ArrayVec { values: [None, None, None, None, None], len: 0 }
ArrayVec { values: [Some(10), Some(11), Some(12), Some(13), Some(14)], len: 5 }
Err(15)
ArrayVec { values: [Some(10), Some(11), Some(12), Some(13), Some(14)], len: 5 }
```

> Notice in our code, we haven't added the `#![no_std]` crate attribute yet. We will do that in the next section on  `MaybeUninit<T>`.

We can see that `Option<T>` works as the optional type that holds a valid value when we have successfully pushed a value, `T`, and `None` when we haven't. So what is the performance overhead caused by `Option<T>` here? 

Again, working in a limited environment like that of embedded systems, we really want to be conservative with our use of storage (if any) and memory space. That means storing `None` in, for example, an array of `1_000_000` `N` values, where each `None` is byte-aligned to the size of `T` if `T` does not allow *niche optimization* for `Option<T>`, is wasteful compared to the alternative and carries performance overhead for the array, and thus our `ArrayVec` implementation.

> For brevity, I'm not going to talk about type alignment and layout in Rust but it goes a long way to help you comprehend, for example, what it means for "`None` to be byte-aligned to the size of `T`." Therefore, if you don't know about it I highly recommend you learn of it.

> Niche optimization is an optimization technique implemented by the Rust compiler `rustc` on `Option<T>` if `T` cannot be null or zeroed. It's a technique where the wrapper `Option` type uses the same size as `T` by assigning it's `None` with the `0` bit. That way if `T` is 8 bytes, `Option<T>` will also be 8 bytes instead of 16 bytes.

If the previous paragraph was tough to understand, It might help to study type alignment and layout in Rust, as well as niche optimization in more detail.

You can experiment with the code for `Option<T>` on the Rust playground via the link: https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=a32ad95d9ad32d06d69bb7a612e57c4b

You can also find the whole code on this GitHub Gist: https://gist.github.com/allwelldotdev/10630be265c9548bfb0c591767afedf2

Let's look at the more performant but unsafe alternative with `MaybeUninit<T>`.

### Using `MaybeUninit<T>`
`MaybeUninit<T>` is a `union` type, and `union` types in Rust are rare and mostly used for interfacing with `C` code in foreign function interfaces (FFI). Accessing the field of a `union` in Rust is unsafe, therefore, we'll be introducing the use of unsafe code to implement `MaybeUninit<T>`.

What is the function of `MaybeUninit<T>`, how is it useful in the concept of building a heapless vector type?

According to stdlib docs, `MaybeUninit<T>` is a wrapper type to construct uninitialized instances of `T`. What does "uninitialized" mean? I'll use the example of `let` value-to-variable assignment to illustrate the meaning of initialized and uninitialized instances to help you *really* understand what `MaybeUninit<T>` does.

```rust
let init_var: i32 = 10; /* `init_var` is a variable that holds a 32-bit signed integer and is immediately initialized to the value of 10. */

let uninit_var; /* In contrast, `uninit_var` is a variable that, at this point,
holds nothing (actually holds what are known as "garbage bytes" - invalid,
overwritable data in memory). Meaning, `uninit_var` points to *uninitialized*
memory.

At this point of the code, in safe code, Rust would not allow you use
`uninit_var` in any kind of computation that requires `uninit_var` to
be initialized. The compiler to complain that `uninit_var` is uninitialized,
using it would result in unexpected/undefined behaviour (UB). To use
`uninit_var`, we must initialize it with appropriate value. */

uninit_var: u8 = 255; /* Finally, we initialize `uninit_var` with an 8-bit
unsigned integer of value 255. Rust powerful type inference sets `uniniti_var`
to be of type `u8`. If you try to assign `uninit_var` to a value of the wrong
type, the compiler will error. */

/* Below is another example of Rust's type inference enforcing appropriate value
assignment. */
let uninit_var: i8; /* Variable holds no value, points to unitialized memory of
type 8-bit signed integer. */
uninit_var = 255; /* ❌ ERROR: You must initialize `uninit_var` with the
appropriate value. `i8::MAX` is 127. */
```

Therefore, in a nutshell, uninitialized memory are regions that haven't been filled with valid data yet. Accessing uninitialized memory through normal types like `i32` or `String` leads to UB because the compiler assumes all values are properly initialized (e.g., references are not null, `bool`s are `true` or `false`, and padding bytes in structs are not garbage).

Another definition that fits with our context is: `MaybeUninit<T>` lets you represent potentially uninitialized data without immediate UB. It's a `union` type with `#[repr(transparent)]`, meaning it has the *exact same size, alignment, and ABI* as T—no overhead. Unlike Option, it doesn't track initialization at runtime (no discriminant), so you must manually ensure safety via invariants (promises you make in unsafe code). Dropping a MaybeUninit never calls T's destructor; that's your job if it's initialized.

According to stdlib docs, "`MaybeUninit<T>` serves to enable unsafe code to deal with uninitialized data. It is a signal to the compiler **indicating that the data here might _not_ be initialized**. The compiler then knows to not make any incorrect assumptions or optimizations on this code. You can think of `MaybeUninit<T>` as being a bit like `Option<T>` but without any of the run-time tracking and without any of the safety checks."
Learn more about `MaybeUninit<T>` from the [stdlib docs](https://doc.rust-lang.org/std/mem/union.MaybeUninit.html).

Having understood what `MaybeUninit<T>` is, let's see it's function in building a heapless vector type.

We rewrite our earlier implementation of `ArrayVec` from using `Option<T>` to `MaybeUninit<T>`:

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
		/// Creates a new empty ArrayVec with and array of uninitialized `T`
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

First, notice the newly added crate attribute `#![no_std]` that signifies this source code should be built for an environment that does not support the Rust standard library. Next, we move `ArrayVec` and its impls into a `mod` to mimic an organized project structure and visibility. `values` is now an array of `N` maybe uninitialized type-values `MaybeUninit<T>`. `len` continues to track number of initialized elements.

Following similar implementations on `ArrayVec` with `Option<T>`, with `MaybeUninit<T>`;
- `new` introduces allocation to static memory with `const` and the `MaybeUninit::uninit()` associated function to return an array of uninitialized instances of `T` to the `values` field. Instead of the safe variant we could have used `unsafe` with `.assume_init()` on `MaybeUninit<T>` to create and assign an array of `N` uninitialized instances of `T` to the `values` field, as seen in the commented code, because it returns the same value. Let me explain.
	- When you declare a field like `values: [MaybeUninit<T>; N]` in a struct, Rust doesn't require (or perform) any explicit initialization of the array's contents to a "valid" state for `T` at construction time. Instead the array starts in a raw, uninitialized memory state (i.e., whatever **garbage bytes** happen to be on the stack at that moment), and the `MaybeUninit<T>` wrapper semantically interprets those bytes as "uninitialized" without any runtime cost or safety violation. Hence why, I used the safe variant instead and commented the unsafe variant. Meaning the unsafe variant was safe to use in that context. Other places where I use `unsafe`, I document (i.e. comment) the safety invariants that make using the `unsafe` code safe.
- `try_push` acts like `.push` on `Vec<T>`, takes in and writes `T` directly to uninitialized memory in the array (if all `N` elements have not been initialized, otherwise returns `Err(T)`) and increments `len`.

In full vector style, let's add more useful methods to `ArrayVec` like `get`, `pop`, and a getter `len`.

```rust
// ...earlier code.

mod arrayvec {
	impl<T, const N: usize> ArrayVec<T, N> {
		// ...earlier code.
		
		/// Returns a reference to the element at `index` if within bounds
		/// and initialized.
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
		/// memory as uninitialized.
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

...

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


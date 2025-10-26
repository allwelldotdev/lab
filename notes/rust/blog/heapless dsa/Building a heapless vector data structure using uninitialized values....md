**Title:** Building a heapless vector data structure using uninitialized values with `MaybeUninit<T>` instead of using default values with `Option<T>`. Creating safe abstractions over unsafe code with safe APIs.

In this article, I'll teach you how to build a heapless vector data structure with the example of one I already built. I'll share the performance benefits and unsafety to look out for and avoid when using `MaybeUninit<T>` over `Option<T>`. Lastly, I'll show you how to build safe APIs on top of unsafe Rust code to ensure "safe unsafe code" in Rust.

A little primer on Rust memory models before we begin. Though to fully comprehend this article you may require practical understanding of Rust memory models--what's stored where, how, and what for--here's a little primer incase you're new to this.

## Rust's Memory Model
Rust models memory in three main spaces or regions; the *stack*, the *heap*, and *static memory*.

The *stack* is a short-lived, fixed-size memory region with LIFO movement (Last In First Out). The stack stores data of local variables, primitive types, function parameters, and fixed-size collections like an array. When a function is called a contiguous chunk of memory is allocated to the top of the stack known as a *frame*.

The *heap* is a dynamic, growable region of contiguous memory. Collections and data in the heap do not have to be fixed-size. The size-bound is unneeded as data can grow to fit runnable requirements. The heap isn't tied to the current call stack (i.e. `main` function stack frame) of the program. Data in the heap can live forever until explicitly deallocated.

*Static memory* is really a catch-all term for several closely related regions located in the file your program is compiled into. These regions are automatically loaded into your program's memory when that program is executed. Values in static memory live for the entire execution of your program (as described by Jon Gjengset in Rust for Rustaceans). Static memory stores source code, `'static` values, and values in `static` variables. Static memory is read-only by default, therefore immutable. Mutating data in static memory is unsafe (e.g. mutating static variables using `static mut`).

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

Now you see, in embedded environments (i.e. microcontrollers and SoCs) where limitations on compute and memory are prevalent we must make do with data stored on the stack and static memory. We cannot use the heap memory because in such environments we lack memory allocation. That means bye-bye to useful data structures like `Vec<T>`. Looking at what we have available by default in Rust `core`, the closest stack data structure akin to `Vec<T>` is the `core::array` (aka. array) primitive type, typed in code as `[T; N]`. But we know the limitations of `array` compared to `Vec<T>`, for example;
- `array` must be fixed-sized to `N`, `Vec<T>` is variably-sized,
- denoting an array as `[T; N]` means `T` must be `Copy` (shorthand lingo for `T` must implement the `Copy` trait),
- `array` creates a fixed-size contiguous space where each space is data stored in the stack memory, while `Vec<T>` allocates memory in the heap and returns a pointer (off on a tangent here: understanding the difference between a value, variable, and pointer in Rust greatly contributes to improving your understanding of pointers),
- plus a couple more differences that are not important to our use case otherwise would mention where applicable.

Despite these differences and limitations between `array` and `Vec<T>`, as the more similar data structures, it'd be nice to be able be create a vec-like data structure that fundamentally functions like an array (therefore, without needing heap allocation) but inherits (not to be confused with OOP Inheritance as Rust doesn't do that) some functionality of `Vec<T>`--a *heapless vector* data structure. That's what we'll build next.

## Building a Heapless Vector Type
Now you understand *the what* and *the why*, let's talk about *the how*.

To build a *heapless vector* we must allocate memory upfront in the form of the `N` size of elements in `[T; N]`






**Title:** Building a heapless vector data structure using uninitialized values with `MaybeUninit<T>` instead of using default values with `Option<T>`. Building safe APIs atop unsafe code.

In this article, I'll teach you how to build a heapless vector data structure from an example of one I've already built. I'll share the performance benefits and unsafety to look out for and avoid when using `MaybeUninit<T>` over `Option<T>`. Lastly, I'll show you how to build safe APIs on top of unsafe Rust code to ensure safe unsafe code.

A little primer before we begin on Rust memory models. Though, to fully comprehend the article you'd need to have a solid understanding of Rust memory models--what's stored where, how, and what for--here's a little primer incase you're new to this.

Rust models memory as three main spaces; the *stack*, the *heap*, and *static* memory.

The *stack* is a short-lived, fixed-size memory with a LIFO movement (Last In First Out). The stack stores data of primitive types, function calls, and fixed-size collections like an array. When a function is called a contiguous chunk of memory is allocated to the top of the stack known as a *frame*.

The *heap* is a dynamic, growable region of contiguous memory. Collections and data in the heap does not have to be fixed-size, the size-bound is unneeded as the data can grow to fit runnable requirements (thus making data allocation easier). The heap isn't tied to the current call stack (i.e. `main` function stack frame) of the program. Data in the heap can live forever until it is explicitly deallocated.

*Static memory* is really a catch-all term for several closely related regions located in the file your program is compiled into. These regions are automatically loaded into your program's memory when that program is executed. Values in static memory live for the entire execution of your program (as described by Jon Gjengset in Rust for Rustaceans). Static memory stores source code, `'static` values, and values in `static` variables. Static memory is meant to be read-only therefore immutat
**Title:** Building a heapless vector data structure using uninitialized values with `MaybeUninit<T>` instead of using default values with `Option<T>`. Building safe APIs atop unsafe code.

In this article, I'll teach you how to build a heapless vector data structure from an example of one I've already built. I'll share the performance benefits and unsafety to look out for and avoid when using `MaybeUninit<T>` over `Option<T>`. Lastly, I'll show you how to build safe APIs on top of unsafe Rust code to ensure safe unsafe code.

A little primer before we begin on Rust memory models. Though, to fully comprehend the article you'd need to have a solid understanding of Rust memory models--what's stored where, how, and what for--here's a little primer incase you're new to this.

Rust models memory as three main spaces, the stack, the heap, and static memory. The stack is fixed memory
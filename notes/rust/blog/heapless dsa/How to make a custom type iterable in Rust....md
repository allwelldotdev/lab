**Title**: How To Make A Custom Type Iterable In Rust.
**Sub Title**: Learn how to make your custom type implement the `IntoIterator`, `Iterator`, and `FromIterator` trait.

In my former article, [How to build a Heapless Vector using `MaybeUninit<T>` for Better Performance](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance), I taught how to build a heapless vector data structure to enable a `Vec`-like data structure without heap allocation for use in embedded systems where heap allocation is unlikely. The heapless vector `ArrayVec` is a wrapper type over a fixed-sized array that utilizes uninitialized, stack, and static memory to mimic the functionality of a variably-sized `Vec<T>` with methods like `try_push()`, `get()`, and `as_slice()` to convert the type into a slice with pointer references to its array values.

In this article, we'll learn how to iterate through `ArrayVec`. As a custom type, we will implement iterator traits on `ArrayVec` that make it iterable (e.g. usable in a `for` loop).

These are the traits we'll learn about and implement:
- `IntoIterator`
- `Iterator`
- `FromIterator`
- `Extend`

Let's describe what these traits mean and do in Rust and why they're important in making a type iterable.

## Iterator Traits
### `IntoIterator`
The `IntoIterator` trait is the foundation for making your custom type (in our case, `ArrayVec`) iterable in `for` loops (e.g., `for item in &vec { ... }`) and compatible with iterator methods like `map`, `filter`, etc. It defines how to convert your type *into* an iterator, with an associated `Item` type (such as `T` for by-value, `&T` for by-reference, or `&mut T` for by-mutable-reference) and `IntoIter` type (the actual iterator type of your custom type, also known as *concrete iterator*).

Let's elaborate a little further on the meaning of the `IntoIterator::IntoIter` associated type and the *concrete iterator* it points to: To make `ArrayVec` iterable, we implement `IntoIterator` on `ArrayVec` and set the `IntoIter` associated type to something like `ArrayVecIntoIter`. Why? Because when we call `into_iter()` on `ArrayVec` it returns, `ArrayVecIntoIter`, an iterator (i.e. a type that implements the `Iterator` trait). How do we make `ArrayVecIntoIter` an iterator? By creating a new type, `ArrayVecIntoIter`, that mirrors iterable and utility fields on `ArrayVec` and implements `Iterator`. So we make `ArrayVecIntoIter` an iterator by implementing the required `next` method on it, along with other provided methods if we need them. That way, when you call `into_iter()` on `ArrayVec` and get `ArrayVecIntoIter`, you can continuously call `next()` on `ArrayVecIntoIter` to iterate through the values in `ArrayVec`. That, in a nutshell, is how you make `ArrayVec` iterable using `IntoIterator`.

Before putting all this into practice, below is the code that implements `ArrayVec` and it's functionality (as built in [the previous article](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance)). After going through the code and understanding it, we'll start making `ArrayVec` iterable.

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

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

const CAP: usize = 5;

fn main() {
	{
		// A:
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
}
```
<small>Figure 1: Complete former implementation of `ArrayVec`.</small>

Having understood the implementation and functionality of `ArrayVec`, next we implement `IntoIterator` on `ArrayVec`.

#### Implementing `IntoIterator` for `ArrayVec`
`ArrayVec` has two methods, `as_slice` and `as_mut_slice`, that return a slice of the array. A cool thing about `slice` types is they implement methods, `iter` and `iter_mut`, that let you return an iterator over the slice elements; immutably `&` or mutably `&mut` referenced, respectively. Let's start by implementing `iter` and `iter_mut` methods on `ArrayVec` to return iterators from slices of `ArrayVec`.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	impl<T, const N: usize> ArrayVec<T, N> {
		// ...earlier code.
		
		/// Use slice iterator for immutable iteration of ArrayVec.
		pub fn iter(&self) -> core::slice::Iter<'_, T> {
			self.as_slice.iter()
		}
		
		/// Use slice iterator for mutable iteration of ArrayVec.
		pub fn iter_mut(&mut self) -> core::slice::IterMut<'_, T> {
			self.as_mut_slice.iter_mut()
		}
	}
}
```
***Figure 2: Converting slices of `ArrayVec` into iterators.***

With this implementation in place, when we call `ArrayVec::iter` or `ArrayVec::iter_mut`, we'll get a type that is `Iterator` and allows us iterate through elements of `ArrayVec`.

Three things must happen before we can successfully implement `IntoIterator` on `ArrayVec`:
1. Provide `IntoIterator::IntoIter` associated type for `ArrayVec` by creating `ArrayVecIntoIter` type.
2. Implement `Iterator` for `ArrayVecIntoIter`.
3. Implement `Drop` for `ArrayVecIntoIter`. *You'll see why we do this later.*

**1. Create `ArrayVecIntoIter` Type.**

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Build out iterator type for ArrayVec as ArrayVecIntoIter<T, N>
    // Consuming iterator (by-value): Moves out owned T.
    // But will provide iterators for fundamental types: & and &mut
    #[derive(Debug)]
    pub struct ArrayVecIntoIter<T, const N: usize> {
        values: [MaybeUninit<T>; N],
        len: usize,
        index: usize,
    }
}
```
***Figure 3: Creating `ArrayVecIntoIter`—`ArrayVec`'s iterator.***

Notice `ArrayVecIntoIter` is similar in structure to `ArrayVec`, only difference being the `index` field. This field would be used to track how many initialized elements in `values: [MaybeUninit<T>; N]` array have been iterated over.

**2. Implement `Iterator` for `ArrayVecIntoIter`.**

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Implement Iterator trait on ArrayVecIntoIter making it an iterator.
    impl<T, const N: usize> Iterator for ArrayVecIntoIter<T, N> {
        type Item = T;

        fn next(&mut self) -> Option<Self::Item> {
            if self.index >= self.len {
                return None;
            }
            let i = self.index; 
            self.index += 1;
            // SAFETY: i < len, so slot is init
            Some(unsafe { self.values[i].assume_init_read() })
        }

        fn size_hint(&self) -> (usize, Option<usize>) {
            let remaining = self.len.saturating_sub(self.index);
            (remaining, Some(remaining))
        }
    }
}
```
***Figure 4: Implementing `Iterator` for `ArrayVecIntoIter`.***

`Iterator::Item` associated type for `ArrayVecIntoIter<T, N>` is `T`. We setup two methods, `next` and `size_hint`. `next` is a required `Iterator` method and we set it up to return `Option<Self::Item>` in other words `Option<T>` for `ArrayVecIntoIter<T, N>`.

In `next`, we check if the iterator index tracker (`self.index`) value equates to the vector initialized element tracker (`self.len`), if yes, we return `None` immediately because that means, we've reached the end of iteration. That way we create a safe API over unsafe code by not reading uninitialized values (which is undefined behaviour UB). `.assume_init_read()` reads the initialized values and returns them owned. This is why we get the return type of `Option<Self::Item>`. `Self::Item`, which is `T`, is an owned value.

> Review the [previous article](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance#heading-add-get-and-pop-methods) to understand, in detail, what `.assume_init_read()` does as it dereferences and reads the value behind the `MaybeUninit<T>`.

`size_hint` as the name suggests is a method that computes and returns bounds on the remaining length of the iterator. It returns a tuple where the first element is the lower bound, and the second element is the upper bound. To learn more about `size_hint`, see the [stdlib docs on `Iterator` methods](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.size_hint). We use the value retrieved from this method later on (I'll call your attention back to this then).

With these two methods implemented, `ArrayVecIntoIter` is now an iterator for `ArrayVec`.

**3. Implement `Drop` for `ArrayVecIntoIter`.**

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Implement Drop trait to safely deallocate initialized elements
    // from ArrayVecIntoIter<T, N> MaybeUninit<T> values
    impl<T, const N: usize> Drop for ArrayVecIntoIter<T, N> {
        fn drop(&mut self) {
            // Drop remaining init elements (from index to len)
            // SAFETY: Invariant holds for those slots.
            for i in self.index..self.len {
                unsafe {
                    self.values[i].assume_init_drop();
                }
            }
            // Uninit slots auto-drop as MaybeUninit: meaning as "garbage"
            // bytes they are overwritten by the next write.
        }
    }
}
```
***Figure 5: Implementing `Drop` for `ArrayVecIntoIter`.***

Next, we implement `Drop` for `ArrayVecIntoIter` to deallocate initialized elements that have yet to be iterated over, if any. Hence why in the `for` loop, we iterate over `usize` range of `self.index..self.len`. In a scenario where we've iterated through all initialized elements (`self.index == self.len`) then `.assume_init_drop()` will not be called (therefore no double-frees).

> **Why is it important though to implement `Drop` for `ArrayVecIntoIter` especially since we already implemented `Drop` for `ArrayVec`?**
>
> The reason is because when we convert `ArrayVec` into its iterator `ArrayVecIntoIter`, fields of `ArrayVec` will be moved in the `<ArrayVec as IntoIterator>::into_iter` method into similar fields in `ArrayVecIntoIter`. Therefore, we need to, as we did for `ArrayVec`, manually drop the uninitialized values now moved to `ArrayVecIntoIter`. More on this later.

Now, we can successfully implement `IntoIterator` on `ArrayVec`.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Finally, implement IntoIterator for ArrayVec; returns
    // an iterator (ArrayVecIntoIter).
    impl<T, const N: usize> IntoIterator for ArrayVec<T, N> {
        type Item = T;
        type IntoIter = ArrayVecIntoIter<T, N>;

        fn into_iter(self) -> Self::IntoIter {
            // Wrap `self` inside ManuallyDrop so ArrayVec Drop cannot
            // be called on ArrayVec. Prevents double-free.
            // Drop will be handled by ArrayVecIntoIter.
            let this = ManuallyDrop::new(self);
            // SAFETY: Read (bitwise copy) into `values` and `len`,
            // since `self` is consumed by function call; `this` will not
            // be used again.
            let values = unsafe { ptr::read(&this.values) };
            let len = unsafe { ptr::read(&this.len) };
            
            ArrayVecIntoIter {
                values,
                len,
                index: 0,
            }
        }
    }
}
```
***Figure 6: Implementing `IntoIterator` for `ArrayVec`.***

First observation is that `into_iter` consumes `self` and returns an iterator: `Self::IntoIter` (i.e. `<ArrayVec as IntoIterator>::IntoIter`) which points to `ArrayVecIntoIter`. This is precisely why we first needed to create the iterator, `ArrayVecIntoIter`, for `ArrayVec`.

As `self` is consumed, if we tried to perform a simpler operation by transferring ownership of `self`'s values, the compiler would have panicked with an error. Below is an example.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	impl<T, const N: usize> IntoIterator for ArrayVec<T, N> {
		type Item = T;
        type IntoIter = ArrayVecIntoIter<T, N>;

        fn into_iter(self) -> Self::IntoIter {
			ArrayVecIntoIter {
				values: self.values,
				len: self.len,
				index: 0,
			}
        }
	}
}
```
***Figure 7: Rewriting code in Figure 6 in a simpler operation.***

Panics with error:

```bash
  Compiling playground v0.0.1 (/playground)
error[E0509]: cannot move out of type `arrayvec::ArrayVec<T, N>`, which implements the `Drop` trait
   --> src/main.rs:190:25
    |
190 |                 values: self.values,
    |                         ^^^^^^^^^^^
    |                         |
    |                         cannot move out of here
    |                         move occurs because `self.values` has type `[MaybeUninit<T>; N]`, which does not implement the `Copy` trait

For more information about this error, try `rustc --explain E0509`.
error: could not compile `playground` (bin "playground") due to 1 previous error
```
***Figure 8: Compiler error panic from running code in Figure 7.***

This error states that the compiler cannot move out of `self` because `self` (`ArrayVec`) implements `Drop`.

Remember, in the `Drop` implementation of `ArrayVec`, we iterated through a range that had `self.len` as the upper bound, and within the iteration we called `.assume_init_drop()` on the initialized elements of the `self.values` array. Accessing fields of `ArrayVec` in `drop` is the reason why the error code in *Figure 8* occurs in a simple operation in *Figure 7*.

Because `ArrayVec` implements `Drop` and, in the implementation, accesses its fields, when we attempt to move out of `self` in `<ArrayVec as IntoIterator>::into_iter` the compiler panics saying, "Hey, `self.values` will be moved out here which means by the time I call `drop` on `ArrayVec` which is `self` when `into_iter` finishes running, in `<ArrayVec as Drop>::drop` I'll be accessing fields of `self` that have had their values moved therefore causing undefined behaviour UB. I cannot allow memory safety errors and UB therefore this is an error and you have to fix this code." Just imagine the compiler was a real person and had this conversation with you.

The way to fix that error is the reason for the implementation in *Figure 7*: by wrapping `self` (i.e. `ArrayVec`) in `core::mem::ManuallyDrop<T>`.

`ManuallyDrop` wraps `ArrayVec` to suppress its `Drop` impl during the transfer, then we `unsafe`-read fields, using `core::ptr::read` (similar in function to `MaybeUninit::assume_init_read`), into `ArrayVecIntoIter` (the iterator, which takes full ownership). Then we implement `Drop` for `ArrayVecIntoIter` to deallocate owned values. Finally, `ArrayVecIntoIter`'s `Drop` and `next` handle dropping iterator consumed/unconsumed elements correctly—no leaks, no doubles.

---

A lot has been explained already but we've only just iterated over owned values of `ArrayVec`. We also need to be able to iterate over borrowed values. Usually known as fundamental types (`&` and `&mut`). Let's do that next.

#### Implementing `IntoIterator` for `&ArrayVec` and `&mut ArrayVec`
This is simpler than enabling iteration for owned values. Here, we simply call the `iter` and `iter_mut` methods on `ArrayVec` and return a slice iterator.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Implement IntoIterator for fundamental types of ArrayVec: & and &mut.
    // By reference: yields &T
    impl<'a, T, const N: usize> IntoIterator for &'a ArrayVec<T, N> {
        type Item = &'a T;
        type IntoIter = core::slice::Iter<'a, T>;

        fn into_iter(self) -> Self::IntoIter {
            self.as_slice().iter()
        }
    }

    // By mutable reference: yields &mut T
    impl<'a, T, const N: usize> IntoIterator for &'a mut ArrayVec<T, N> {
        type Item = &'a mut T;
        type IntoIter = core::slice::IterMut<'a, T>;

        fn into_iter(self) -> Self::IntoIter {
            self.as_mut_slice().iter_mut()
        }
    }
}
```
***Figure 9: Implementing `IntoIterator` on fundamental types of `ArrayVec`.***

As you can see, it's a much simpler implementation to that of owned values. In real-word use cases, iterating over borrowed values or values that exist in the buffer, in embedded contexts usually fulfills 80% of your needs. Since in embedded contexts, due to limited compute resources, consumption (or in Rust terms, moving) of values is discouraged while reusing the buffer is encouraged.

#### Testing
With the previous implementations, `ArrayVec` is now iterable in a `for` loop. Let's test it.

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

mod arrayvec { /* ...`ArrayVec` implementation. */ }

const CAP: usize = 5;

fn main() {
	// { /* A: ...earlier code */ }
	{
		// B:
		// Iterating over ArrayVec through fundamental types: & and &mut
		
		// Shared iteration
		let mut arr_vec = ArrayVec::<u8, CAP>::new();
        let mut count;
        for i in 0..CAP {
            count = 1 + i as u8;
            arr_vec.try_push(count).unwrap(); // Init values on ArrayVec
        }
        std::println!("---");
        count = 0;
        for i in &arr_vec {
            std::println!("ArrayVec at index {}: {}", count, i);
            count += 1;
        }
        
        // Mutable iteration
        for value in &mut arr_vec {
            *value += 10;
        }
        std::println!("---\nMutated ArrayVec: {:?}", arr_vec.as_slice());
	}
	
	{
		// C:
		// Iterating over owned values of ArrayVec by calling `into_iter`
		let mut arr_vec = ArrayVec::<u8, CAP>::new();
        let mut count;
        for i in 0..(CAP - 1) {
            count = 1 + i as u8;
            arr_vec.try_push(count).unwrap();
        }
        let mut arr_iter = arr_vec.into_iter(); // move arr_vec
        // std::println!("{:?}", arr_vec); // should return move error
        /* Above code confirms `impl IntoIterator for ArrayVec` safety
        invariants.
        */
        std::println!("---\n{:?}", arr_iter); /* View created
        ArrayVecIntoIter. See `len` and `index` fields.
        */
        
        let mut arr_vec2 = ArrayVec::<Option<u8>, CAP>::new();
        loop { /* Loop through calls to `next()` to iterate through
        ArrayVecIntoInter.
        */
            match arr_iter.next() {
                Some(val) => arr_vec2.try_push(Some(val)).unwrap(),
                None => {
                    arr_vec2.try_push(None).unwrap();
                    break;
                }
            }
        }
        std::println!("{:?}", arr_iter); /* Review `len` and `index` fields.
        Notice index incr caused by calling `next()` on ArrayVecIntoIter.
        */
        std::println!("{:?}", arr_vec2.as_slice());
	}
}
```
***Figure 10: Testing the iteration of `ArrayVec` in a `for` loop for fundamental types and owned values.***

In Scope `B`, we test for iteration through fundamentals types. While in Scope `C`, we test for iteration over owned values.

After running the code in *Figure 10* with `cargo run`, we get:

```bash
# ...earlier output.
---
ArrayVec at index 0: 1
ArrayVec at index 1: 2
ArrayVec at index 2: 3
ArrayVec at index 3: 4
ArrayVec at index 4: 5
---
Mutated ArrayVec: [11, 12, 13, 14, 15]
---
ArrayVecIntoIter { values: [MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>], len: 4, index: 0 }
ArrayVecIntoIter { values: [MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>, MaybeUninit<u8>], len: 4, index: 4 }
[Some(1), Some(2), Some(3), Some(4), None]
```
***Figure 11: Terminal return from running code in Figure 10.***

In Scope `C`, when we call `into_iter()` on `arr_vec`, `arr_vec` is moved yet there's no panic for a `Drop` on `arr_vec` because we wrapped `arr_vec` in a `ManuallyDrop<T>` in the `<ArrayVec as IntoIterator>::into_iter` method. Because the consumption of `self`, the invariants in `into_iter` are upheld because `arr_vec` can no longer be used after the move. This is one of the awesome characteristics of the Rust programming language. Its ability to enforce memory safety through types and the borrow checker.

Some other things you also see in play after the test like the `index` field of the `ArrayVec` iterator (`ArrayVecIntoIter`).

---

We've learned about `IntoIterator` and `Iterator`, and how to implement them. Next, we'll look at `FromIterator` and `Extend`.

### `FromIterator`
The `FromIterator` trait, when implemented on your custom type, allows you to fill your custom *collection* type with output from another iterator. It does this by showing your custom *collection* type how to fill itself with outputs from another iterator, of course this is implemented in code.

A simple way to understand this is the `collect` method on `T`, where `T: Iterator`. The `collect` method *collects* outputs of an iterator into a collection type (like `Vec<T>`, etc.). The `collect` method actually uses the `FromIterator` trait behind the scenes by calling the `FromIterator::from_iter` method on the *collection* type it's collecting the iterator outputs into, thereby filling the type.

Seeing the code will help you understand it better.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	impl<T, const N: usize> FromIterator<T> for ArrayVec<T, N> {
		fn from_iter<I>(iter: I) -> Self
		where
			I: IntoIterator<Item = T>
		{
			let mut arr_vec = Self::new();
			let iter = iter.into_iter();
			
			// Optimize: If hinted size > N, take only N to skip
			// early access.
			let (lower, _) = iter.size_hint();
			if lower > N {
				for item in iter.take(N) {
                    let _ = arr_vec.try_push(item); /* Won't fail as
                    len < N.
                    */
                }
                return arr_vec;
			}
			
			// General case: Push until full (truncates excess).
            for item in iter {
                let _ = arr_vec.try_push(item); /* Err(item) dropped
                if full.
                */
            }
            arr_vec
		}
	}
}
```
***Figure 12: Implementing `FromIterator` for `ArrayVec`.***

The `FromIterator` implementation is a little more generic compared to the `IntoIterator` impl for `ArrayVec`. Here, `FromIterator` is generic over `T` which means the output of the iterator must be the same type as the elements inserted in the `ArrayVec` collection.

We see the use of the `size_hint` method as it's used to check the lower bound of the iterator (`iter`); if more than `N`, we take `N` capacity from it otherwise we simply iterate through `iter`, fill and return `Self`.

#### Testing
Testing `FromIterator` will be the shortest test because it's s straight forward, simpler process, but one that will help you understand how `collect` works under the hood.

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

mod arrayvec { /* ...`ArrayVec` implementation. */ }

const CAP: usize = 5;

fn main() {
	// { /* A: ...earlier code */ }
	// { /* B: ...earlier code */ }
	// { /* C: ...earlier code */ }
	{
		// D:
		// Test FromIterator implementation with iterators on ArrayVec.
		
		// Using collect:
        let arr_vec = (-10..10).collect::<ArrayVec<i8, 15>>();
        std::println!("---\n{:?}", arr_vec.as_slice());
        
        // Using from_iter:
        let arr_vec = ArrayVec::<_, 5>::from_iter(-3..5 as i8); /* Using
        type inference for `T` in `ArrayVec<T, N>` helped by type cast
        on iterator.
        */
        std::println!("{:?}", arr_vec.as_slice());
	}
}
```
***Figure 13: Testing `FromIterator` implementation on `ArrayVec`.***

Returns:

```bash
# ...earlier code.
---
[-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4]
[-3, -2, -1, 0, 1]
```
***Figure 14: Terminal return from running code in Figure 13.***

Notice in *Figure 13* that `from_iter` can be used either indirectly through `collect` or directly as an associated function (i.e. `<YourCustomType as FromIterator>::from_iter`).

### `Extend`
`Extend` is a shorter implementation compared to `FromIterator` that also benefits from `IntoIterator`. `Extend` is pretty intuitive for its use in that it enables you extend a collection with the contents of an iterator.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	impl<T, const N: usize> Extend<T> for ArrayVec<T, N> {
        fn extend<I: IntoIterator<Item = T>>(&mut self, iter: I) {
            for item in iter { // Stops iterating when `iter` is consumed
                if let Err(_) = self.try_push(item) { /* break if
                `self` is full
                */
                    break;
                }
            }
        }
    }
}
```
***Figure 15: Implementing `Extend` for `ArrayVec`.***

The `Extend::extend` method takes a mutable reference of self (`&mut self`) and a type that is iterable (`iter`), iterates through `iter` and pushes its items into `self`. Pretty straight forward right? Told you.

Before testing `Extend`, I want to add a functionality to `ArrayVec` that allows us to see which elements in the array are initialized, represented as `Some(T)`, and which are uninitialized, represented as `None`. If you recognized it, you're right. It's similar to the function of the `as_slice` method in `ArrayVec` but slightly different in that it doesn't add uninitialized slots to the slice (as it shouldn't). With this new function, which I want to call `show_init`, we'll be able to see both initialized and uninitialized slots of the `ArrayVec` fixed-size array, as `Some(T)` and `None` respectively.

> To understand why we're trying to see the initialized and uninitialized values of `ArrayVec`, see [the previous article](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance#heading-using-maybeuninit) to learn more.

The only caveat of `show_init` is that it only works where `T` in `ArrayVec<T, N>` is copyable (`T: Copy`). This is the norm for primitive types but not more so for heap-alloc values. Fortunately for us, we're working in a no_std/embedded context where heap-alloc values are mostly prohibited. The reason for this constraint on `show_init` is because within its implementation, we dereference `T` to perform a bitwise copy of its value to another memory location. If `T` is not copyable (i.e. `T: Clone`), `T` will be moved, which is UB and foundational to use-after-free or double-free errors. We want to avoid that and enforce only copyable types in our code, therefore we add a trait bound in a where clause, `T: Copy`.

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	// Implement these only where `T` is `Copy`.
    /* Better this way because in some impls we dereference `T`;
    if `T` is not `Copy` we'll be creating a use-after-free or
    double-free which is UB.
    */
	impl<T, const N: usize> ArrayVec<T, N>
	where
		T: Copy,
	{
		/// Returns Self with a view of initialized and uninitialized
        /// accesses of memory.
		pub fn show_init<'a>(&self, other: &'a mut ArrayVec<Option<T>, N>)
		-> &'a [Option<T>]
		{
			let mut count = 0;
            
            for item in self {
                let _ = other.try_push(Some(*item)); /* deref `T` to copy
                primitive value.
                */
                count += 1;
            }
            while count < N {
                let _ = other.try_push(None);
                count += 1;
            }
            other.as_slice()
		}
	}
}
```
***Figure 16: Adding `show_init` method to `ArrayVec` for improved testing utility.***

I won't go into the depth of explaining lifetimes in Rust and why `show_init` is generic for the lifetime `'a` , but I'll say this much: the lifetime of the return type of `show_init` is valid for as long as the owned type passed into the `other` argument is valid (in other words, for as long as the lifetime of `T` in `other: &mut T` is valid).

> To fully understand lifetimes in Rust, I recommend reading [the lifetime chapter in TRPL Book](https://rust-book.cs.brown.edu/ch10-03-lifetime-syntax.html) and [lifetime variance in the Rustonomicon](https://doc.rust-lang.org/nomicon/subtyping.html).

#### Testing
With all this in place, let's test our `Extend` implementation.

```rust
#![no_std]
extern crate std;
use arrayvec::ArrayVec;

mod arrayvec { /* ...`ArrayVec` implementation. */ }

const CAP: usize = 5;

fn main() {
	// { /* A: ...earlier code */ }
	// { /* B: ...earlier code */ }
	// { /* C: ...earlier code */ }
	// { /* D: ...earlier code */ }
	{
		// E:
		// Test Extend implementation with iterators on ArrayVec.
        
        /* Testing for two scenarios;
        
        1. `arr_vec1` has more CAP (array "capacity" - N) than `arr_vec2`
        has elements to fill it. Meaning, `arr_vec1` will retain spare CAP.
        
        2. `arr_vec1` has less CAP (array "capacity" - N) than `arr_vec2`
        has elements to fill it. Meaning, `arr_vec1` will max it's CAP
        without extending all of `arr_vec2`s elements.
        */
		
		// Scenario 1:
		type ArrayVecCap10<T> = ArrayVec<T, 10>;
        type ArrayVecCap5<T> = ArrayVec<T, 5>;
        
        let mut arr_vec1: ArrayVecCap10<u8> = ArrayVec::new();
        let mut count;
        for i in 0..3 { // Add elements to `arr_vec1`.
            count = 1 + i as u8;
            arr_vec1.try_push(count).unwrap();
        }
        
        let mut arr_vec2: ArrayVecCap5<u8> = ArrayVec::new();
        for i in 0..2 { // Add elements to `arr_vec2`.
            count = 1 + i as u8;
            arr_vec2.try_push(count).unwrap();
        }
        
        arr_vec1.extend(arr_vec2); // Extend moves `arr_vec2`.
        
        let mut empty_arr_vec: ArrayVecCap10<Option<u8>> = ArrayVec::new();
        std::println!("---\n{:?}", arr_vec1.show_init(&mut empty_arr_vec));
        drop(empty_arr_vec); /* Choosing to explicity drop (or free, or
        deallocate) the stack memory consumed by `empty_arr_vec` here
        as it's no longer in use.
        */
        
        // Scenario 2:
        let mut arr_vec1: ArrayVecCap5<i8> = ArrayVec::new();
        for i in -2..0 { arr_vec1.try_push(i as i8).unwrap(); }
        
        let mut arr_vec2: ArrayVecCap10<i8> = ArrayVec::new();
        for i in -5..0 { arr_vec2.try_push(i as i8).unwrap(); }
        
        arr_vec1.extend(arr_vec2); // Extend moves `arr_vec2`.
        
        let mut empty_arr_vec: ArrayVecCap5<Option<i8>> = ArrayVec::new();
        std::println!("{:?}", arr_vec1.show_init(&mut empty_arr_vec));
        /* Don't need to explicity drop `empty_arr_vec` here as before
        because this is the end of the scope, and the Rust Compiler (rustc)
        will call the drop method in ArrayVec's destructor `Drop`
        to drop `empty_arr_vec` implicity.
        */
	}
}
```
***Figure 17: Testing the implementation of `Extend` for `ArrayVec`, and a few other examples.***

Returns:

```bash
# ...earlier output.
---
[Some(1), Some(2), Some(3), Some(1), Some(2), None, None, None, None, None]
[Some(-2), Some(-1), Some(-5), Some(-4), Some(-3)]
```
***Figure 18: Terminal return from running code in Figure 17.***

We introduce type aliasing in Rust using `ArrayVecCap10<T>` and `ArrayVecCap5<T>`, reducing repeatability, improving readability.

In `Scenario 1`, we extend `arr_vec1` with `arr_vec2` but not fill it up, leaving some slots uninitialized, therefore `show_init` returns a shared slice displaying `Some(u8)` for initialized slots and `None` for uninitialized slots, as planned and implemented. All's good.

In `Scenario 2`, we do the opposite extending `arr_vec1` with `arr_vec2` filling it up but not iterating over all the items of `arr_vec2` as `arr_vec1`'s capacity buffer had been reached. Showing our implementation of `Extend::extend` works.

Test successful.

## In conclusion
I wasn't planning for this article to be 5,000+ words (as [the previous article](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance)), well, here we are 😅, but I'm happy I divulged as much useful information as possible, here, to help you improve your understanding of iterators in Rust. If you read till the end give yourself a pat on the back, and thank you for reading my content.

This article ends the experiment of building heapless data structures and learning from them. There is a well known Rust crate that takes the concepts we've implemented here and in the previous article to build and offer battle-tested heapless data structures useful in no_std and embedded environments. The Rust crate is called [`heapless`](https://crates.io/crates/heapless).

- You can find and play around with the full code on Rust Playground via this link: [https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=7b80863ed153627d1d86a6ee41a42167](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=7b80863ed153627d1d86a6ee41a42167)
- You can also find the whole code on this GitHub Gist: [https://gist.github.com/allwelldotdev/39c817ca8d40fe5aeb808eb6301a18ff](https://gist.github.com/allwelldotdev/39c817ca8d40fe5aeb808eb6301a18ff)

---

👏🏾 Kudos for finishing the article.

Hi there! I'm Allwell, a passionate Rust developer currently working from Lagos, Nigeria. You can connect with me on X ([`@allwelldotdev`](https://x.com/allwelldotdev/)) or LinkedIn ([Allwell Agwu-Okoro](https://linkedin.com/in/allwelldotdev/)). Let's build and learn Rust together.









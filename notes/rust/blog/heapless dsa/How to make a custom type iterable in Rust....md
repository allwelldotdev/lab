**Title**: How To Make A Custom Type Iterable In Rust.
**Sub Title**: Learn how to make your custom type implement the `IntoIterator`, `Iterator`, and `FromIterator` trait. The differences between the traits and how they add the behaviour of `into_iter()`, `iter()`, and `iter_mut()` to your custom type.

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

Let me simplify the `IntoIter` associated type of the `IntoIterator` trait and a snippet from the former paragraph: "the actual iterator type of your custom type." For example, to make `ArrayVec` iterable, we implement `IntoIter` on `ArrayVec` and set the `IntoIter` associated type to something like `ArrayVecIntoIter`. Why? Because when we call `into_iter()` on `ArrayVec` it returns `ArrayVecIntoIter`, an iterator (i.e. a type that implements the `Iterator` trait). How do we make `ArrayVecIntoIter` an iterator? By creating a new type, `ArrayVecIntoIter`, that mirrors important iterable and utility fields on `ArrayVec` and implements `Iterator`. So we make `ArrayVecIntoIter` an iterator by implementing the required `next` method on it, along with other provided methods if we need them. That way, when you call `into_iter()` on `ArrayVec` and get `ArrayVecIntoIter`, you can continuously call `next()` on `ArrayVecIntoIter` to iterate through the values held in `ArrayVec`. That's, in a nutshell, how you make `ArrayVec` iterable using `IntoIterator`.

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

Having understood the implementation and functionality of `ArrayVec`, next we implement `IntoIterator` on `ArrayVec`.

#### Implementing `IntoIterator` on `ArrayVec`
`ArrayVec` has two methods, `as_slice` and `as_mut_slice`, that return a slice of the array. A cool thing about `slice` types is they implement methods, `iter` and `iter_mut`, that let you return an iterator over the slice elements; immutably `&` and mutably `&mut` referenced, respectively. Let's start by implementing `iter` and `iter_mut` methods on `ArrayVec` to return iterators from slices of `ArrayVec`.

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

`Iterator::Item` associated type for `ArrayVecIntoIter<T, N>` is `T`. We setup two methods, `next` and `size_hint`. `next` is a required `Iterator` method and we set it up to return `Option<Self::Item>` in other words `Option<T>` for `ArrayVecIntoIter<T, N>`.

In `next`, we check if the iterator index tracker (`self.index`) value equates to the vector initialized element tracker (`self.len`), if yes, we return `None` immediately because that means, we've reached the end of iteration. That way we create a safe API over unsafe code by not reading uninitialized values (which is undefined behaviour UB). `.assume_init_read()` reads the initialized values and returns them owned. This is why we get the return type of `Option<Self::Item>`. `Self::Item`, which is `T`, is an owned value.

> Review the [previous article](https://allwell.hashnode.dev/how-to-build-a-heapless-vector-using-maybeuninitt-for-better-performance#heading-add-get-and-pop-methods) to understand, in detail, what `.assume_init_read()` does as it dereferences and reads the value behind the `MaybeUninit<T>`.

`size_hint` as a the name suggests is a method that computes and returns bounds on the remaining length of the iterator. It returns a tuple where the first element is the lower bound, and the second element is the upper bound. I won't go into detail about size hint but if you want to learn more about it, see the [stdlib docs on `Iterator` methods](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.size_hint). We use the value retrieved from this method later on (I'll call your attention back to this then).

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

Next, we implement `Drop` for `ArrayVecIntoIter` to deallocate initialized elements that have yet to be iterated over, if any. Hence why in the `for` loop, we iterate over `usize` range of `self.index..self.len`. In a scenario where we've iterated through all initialized elements (`self.index == self.len`) then `.assume_init_drop()` will not be called (therefore no double-frees).

> **Why is it important though to implement `Drop` for `ArrayVecIntoIter` especially since we already implemented `Drop` for `ArrayVec`?**
>
> The answer or reason is because when we convert `ArrayVec` into its iterator `ArrayVecIntoIter`, fields of `ArrayVec` will be moved in the `<ArrayVec as IntoIterator>::into_iter` method into similar fields in `ArrayVecIntoIter`. Therefore, we need to, as we did for `ArrayVec`, manually drop the uninitialized values now moved to `ArrayVecIntoIter`. More on this later.

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

First observation is that `into_iter` consumes `self` and returns and iterator: `Self::IntoIter` which is `<ArrayVec as IntoIterator>::IntoIter` which points to `ArrayVecIntoIter`. This is precisely why we first needed to create the iterator for `ArrayVec`—`ArrayVecIntoIter`.

As `self` is consumed, if we tried perform a simpler operation by transferring ownership of `self`'s values the compiler would have panicked with the following error:

```rust
// ...earlier code.

mod arrayvec {
	// ...earlier code.
	
	
}
```

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



Starting off with easier implementations, we'll implement `IntoIterator` on fundamental types of `ArrayVec`, that is, `&ArrayVec` and `&mut ArrayVec`.
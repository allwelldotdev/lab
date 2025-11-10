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
	}
}

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
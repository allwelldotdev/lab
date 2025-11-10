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

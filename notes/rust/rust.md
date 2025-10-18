## Things to do to land a Rust role
1. know your craft
	1. this means knowing the fundamentals of rust (Ownership, Borrowing, Error Handling, Project Structure)
	2. also means knowing some of the advanced features like async Rust, macros and so on
	3. know how to write rust code in an idiomatic way, because you're gonna be working in a collaborative team of Rust developers
2. have the experience and confidence to build production grade Rust software
	1. building a complete piece of software you deploy somewhere and other people use is completely different from following tutorials or doing online exercises
	2. there are two benefits to having this experience:
		1. you'll feel comfortable and confident in interviews,
		2. more importantly, you'll have something to put on your resume to get the interview in the first place
3. have the right connections
	1. have a community of experts and enthusiasts who can help you along your learning journey,
	2. having access to rust recruiters who can advocate for you and help you land a rust role,
	3. and knowing developers who already have rust roles who can give you referrals
## Where to start learning Rust

> Starting from the groundup use [The Rust Book](https://doc.rust-lang.org/book/) provided by The Rust Foundation.

> Moving to [Rustlings](https://github.com/rust-lang/rustlings): small exercises to get used to reading and writing Rust code.

> Then, [Rust by Example](https://doc.rust-lang.org/rust-by-example/). RBE is a collection of runnable examples that illustrate various Rust concepts and standard libraries.

> You may also check out [Rust Adventure](https://www.rustadventure.dev/) if you'd fancy a paid subscription of $30/month for a more researched and structured Rust learning track.

---
## Google Searches on New Topics

#### What's the difference between a *bit* and a *byte*, and their relationship with the `String` & `char` type in Rust?

- A *bit* is the smallest unit of data in computing, a binary value of either `0` or `1`.
- A *byte* is a sequence of **8 bits** (e.g. the binary sequence `01000001` represents the uppercase letter 'A' in ASCII, and takes up one byte of memory). This is a fundamental unit of storing data.
- A *hexadecimal* (base-16) is a way to express binary data in a more compact and human-readable format.
	- Using 16 as its base, with digits `0-9` and `A-F` (where `A=10`, `B=11`, `C=12`, `D=13`, `E=14`, `F=15`).
	- Each hexadecimal digit corresponds to four binary digits (a nibble), making it efficient for representing memory addresses, colors, and other binary data.
		- For example: the character 'H' is encoded as `0x48` (1 byte); while '🌍' is encoded as `0xF09F8C8D` (4 bytes). See link for more info.
- A `char` is a single Unicode scalar value, which is **always 4 bytes** in size.
- A `String` is a growable, heap allocated, UTF-8 encoded sequence of bytes.

---
#### Is overloading operators similar to polymorphism in python and rust?

Operator overloading is a specific form of polymorphism, often referred to as ad-hoc polymorphism. This applies to both Python and Rust, though their implementations differ.

Polymorphism is a broad concept in object-oriented programming that allows objects of different classes to be treated as objects of a common type. This can manifest in various ways, including:

- **Subtype polymorphism (or inclusion polymorphism):**
	- Where a subclass object can be used wherever a superclass object is expected.
- **Parametric polymorphism (or generics):**
	- Where code is written to work with a variety of types specified as parameters.    
- **Ad-hoc polymorphism (or overloading):**
	- Where a function or operator can have different implementations depending on the types of its arguments.    

Operator Overloading as Ad-hoc Polymorphism:

- **Python:**
	- Python achieves operator overloading by defining special methods within a class (e.g., `__add__` for the `+` operator, `__mul__` for `*`). When an operator is used with instances of a class that defines these special methods, Python calls the corresponding method, providing a specific implementation for that operator based on the types involved. This is a clear example of ad-hoc polymorphism, as the `+` operator, for instance, behaves differently for numbers, strings, or custom objects, depending on how it's overloaded.
- **Rust:**
	- Rust implements operator overloading through traits in the `std::ops` module (e.g., `Add` for `+`, `Mul` for `*`). To overload an operator for a custom type, one must implement the corresponding trait for that type. This also falls under ad-hoc polymorphism, as the behavior of an operator is defined specifically for certain types by implementing a trait.

In essence, while polymorphism is a general principle, operator overloading is a specific mechanism that realizes ad-hoc polymorphism by allowing operators to exhibit different behaviors based on the types they operate on.

---
#### What are *invariants* in programming?

In programming, an invariant is a condition or property that is always true for a specific part of a program's state, or for a particular data structure, at certain defined points during its execution. It represents a fundamental truth or assumption about the system that must consistently hold.

Key aspects of invariants in programming:

- **Always True (at specific points):**
	- While an invariant might be temporarily violated during the execution of an operation (e.g., within a function that modifies a data structure), it must be restored to a true state before the operation completes and control is returned to the calling code. This ensures that external users of the code or data structure always observe the invariant holding true.
- **Ensuring Correctness:**
	- Invariants are crucial for ensuring the correctness and reliability of software. By defining what must always be true, developers can reason about the behavior of their code and identify potential bugs if an invariant is violated.
- **Guiding Design and Implementation:**
	- Invariants help guide the design of data structures and algorithms. They define the constraints and relationships that must be maintained, influencing how operations are implemented to preserve these conditions.

- **Types of Invariants:**
	- **Class Invariants:** Conditions that are always true for every instance of a particular class. For example, in a `BankAccount` class, an invariant might be that the balance is never negative.
    - **Loop Invariants:** Conditions that are true at the beginning of every iteration of a loop, and often also at the end of every iteration. They are fundamental for proving the correctness of iterative algorithms.
    - **Data Structure Invariants:** Properties that must always hold true for a specific data structure to maintain its integrity and functionality (e.g., in a sorted list, elements are always in ascending order).

**Example:**
Consider a `Stack` data structure. A key invariant for a `Stack` is that it follows a Last-In, First-Out (LIFO) principle. This means that the element most recently added is always the first one to be removed. Any `push` or `pop` operation must maintain this invariant for the `Stack` to function correctly.

---
#### What is the Halting Problem?
*Discovered when studying The Rust Book, chapter 15.5.*

The Halting Problem is ==a fundamental concept in computer science that asks whether it's possible to create a general algorithm that can determine if any given program will eventually halt (finish running) or run forever (an infinite loop)==. The answer, proven by Alan Turing, is no; such a general algorithm cannot exist. 

Here's a breakdown:

- **The Problem:**
	- Given a program and its input, the halting problem asks if we can create a universal "halting checker" program that can always correctly predict whether the given program will stop or run forever. 
- **The Solution:**
	- Turing proved that this "halting checker" program is impossible to create [according to Wikipedia](https://en.wikipedia.org/wiki/Halting_problem). 
- **Proof by Contradiction:**
	- The proof typically uses a technique called "proof by contradiction". It assumes such a halting checker program exists and then demonstrates that this assumption leads to a logical contradiction. 
- **Why it Matters:**
	- The Halting Problem highlights the limits of what computers can do and the inherent limitations of algorithms. It's a key concept in understanding the nature of computation and decidability in mathematics. 
- **Analogy:**
	- Imagine a program that analyzes other programs and predicts if they'll halt. If you could build such a program, you could then create another program that does the opposite of what the analyzer predicts for itself. This would lead to a contradiction, proving the analyzer (and thus the halting checker) cannot exist.
- **Who solved the Halting Problem?**
	- [Alan Turing](https://www.udiprod.com/halting-problem/#:~:text=The%20video%20informally%20presents%20Alan,(i.e.%2C%20via%20computation).)
		- The video informally presents Alan Turing’s halting theorem from 1936. This is a highly influential result which plays an important role in computer science and in some branches of mathematics. It shows that some problems cannot be solved algorithmically (i.e., via computation).

---
## Rust GitHub Gists

- Heapless vector type
	- See gist link: https://gist.github.com/rust-play/a32ad95d9ad32d06d69bb7a612e57c4b
	- With `MaybeUninit<T>` for uninitialized elements.
		- See gist link: https://gist.github.com/rust-play/862fe7541fb5d626aa4463da0edc68cb
		- See personal gist public link: https://gist.github.com/allwelldotdev/9f0b30dabea98b1ff22fbfc3ea774d8e
		- 
- Niche Optimization
	- See gist link: https://gist.github.com/rust-play/92cc73f49556dfffba09477f78c201d6

---
## More Q&A

##### Why do structs derive both the `Clone` and `Copy` trait as in the example below with the `AccessLogger` struct?

```rust
use std::ops::Deref;

#[derive(Clone, Copy)]
struct AccessLogger(i32);

impl Deref for AccessLogger {
	type Target = i32;
	fn deref(&self) -> &Self::Target {
		println!("deref");
		&self.0
	}
}

fn main() {
	let n = AccessLogger(-1);
	let x = *n + 1;
	let n2 = n;
	println!("{} {}", x, *n)
}
```

When a struct derives both `Clone` and `Copy`, it's because `Copy` has `Clone` as a supertrait - meaning any type that implements `Copy` must also implement `Clone`.


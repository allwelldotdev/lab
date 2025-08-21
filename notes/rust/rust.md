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

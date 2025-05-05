## JavaScript Fundamentals
### Linking a JavaScript File
to link a JS file in HTML, add `script` tag to the bottom of the `body` tag of a HTML file:
```html
<body>
...
<script src='script.js'></script>
</body>
```
### `let`, `const`, `var`
these keywords are used to defined variables in JS
the difference between `let`, `const`, and `var` keyword is:
- `let`
	- defines mutable variables
	- can define an empty variable
	  ```js
		let emptyVariable // emptyVariable is undefined
		emptyVariable = 10
		```
	- is block-scoped (variable defined by `let` keyword is block-scoped)
- `const`
	- defines immutable variables
	- not used to define empty variables, as that variable, if defined empty, will remain empty (immutable)
- `var`
	- deprecated in recent ES6+ versions of JS
	- defined mutable variables, like `let`
	- is function-scoped, unlike `let`
### Type Conversion and Type Coercion
- **Type Conversion:** Explicitly converting a value from one type to another using functions like `Number()`, `String()`, or `Boolean()`. Example: `Number("5")` returns `5`.
- **Type Coercion:** JavaScript automatically converts types during operations. Example: `"5" * 2` coerces string `"5"` to number `5`, resulting in `10`. Coercion can lead to unexpected results, like `"5" + 2` becoming `"52"`.
### Truthy and Falsy Values
- **Truthy Values:** Values that evaluate to `true` in a boolean context. Examples: `1`, `"hello"`, `[]`, `{}`,` true`.
- **Falsy Values:** Values that evaluate to `false` in a boolean context. Examples: `0`, `""`, `null`, `undefined`, `NaN`, `false`.

> Why does an empty list `[]` or object `{}` evaluate to `true`? Aren't they falsy values?
> 
> In JavaScript, an empty array `[]` and an empty object `{}` are truthy values, not falsy, because they are valid, non-null objects. JavaScript evaluates any non-null, non-undefined object as `true` in a boolean context, regardless of whether it contains elements or properties.
> 
> The falsy values are specifically: `false`, `0`, `-0`, `""`, `null`, `undefined`, `NaN`, and `BigInt(0)`. Since `[]` and `{}` don't fall into these categories, they evaluate to `true`.
> 
> This can be counterintuitive since they "feel" empty (and would be falsy values in Python, but their existence as objects makes them truthy.
> 
> For example:
```js
if ([]) console.log("true"); // Outputs "true"
if ({}) console.log("true"); // Outputs "true"
```

### Equality Operators: `==` vs. `===`
- **`==` (Loose Equality)**: Compares values after type coercion, converting operands to the same type. Example: `5 == "5"` is `true`.
- **`===` (Strict Equality)**: Compares values and types without coercion. Example: `5 === "5"` is `false`.

### Logical Operators
JavaScript has three main logical operators that evaluate expressions and return boolean or other values based on their logic. Here's a concise explanation:

1. **Logical AND (`&&`)**: Returns `true` if both operands are truthy; otherwise, returns `false`. If the first operand is falsy, it short-circuits and returns that value without evaluating the second operand. Non-boolean values return the first falsy value or the last value if both are truthy. For example:
	- `true && false` -> `false`
	- `5 && "hello"` -> `"hello"`
	- `0 && true` -> `0` (short-circuits)
2. **Logical OR (`||`)**: Returns `true` if at least one operand is truthy; otherwise, returns `false`. If the first operand is truthy, it short-circuits and returns that value without evaluating the second operand. Non-boolean values return the first truthy value or the last value if both are falsy. For example:
	- `true || false` -> `true`
	- `0 || "hello"` -> `"hello"`
	- `null || 0` -> `0` (both falsy)
3. **Logical NOT (`!`)**: Unary operator that converts its operand to a boolean and returns the opposite. Truthy values become `false`, falsy values become `true`. For example:
	- `!true` -> `false`
	- `!0` -> `true`
	- `!!"hello"` -> `true` (double NOT converts to boolean)

**Key Notes**: 
- Logical operators often involve type coercion, as they evaluate truthy/falsy values (e.g., `[]` is truthy, `0` is falsy).
- Short-circuiting makes `&&` and `||` useful for control flow or default values (e.g., `let x = value || "default"`).
- Precedence: `!` > `&&` > `||`. Use parentheses for clarity if needed.

These operators are commonly used in conditionals, assignments, and expressions to control logic flow.

### The `switch` Statement
The **switch statement** in JavaScript evaluates an expression and executes code blocks based on matching case values. It uses strict equality (`===`) for comparisons.
```js
switch (expression) {
	case value1:
		// Code
		break;
	case value2:
		// Code
		break;
	default:
		// Code if no match
}
```
- `break` prevents fall-through to the next case.
- `default` runs if no case matches.

### Statements and Expressions
- **Statement**: A complete instruction that performs an action, like declaring a variable or controlling flow. Example: `let x = 5;` or `if (true) {}`.
- **Expression**: A code fragment that evaluates to a value. Example: `5 + 3` or `x * 2`. Expressions can be part of statements.

### The Conditional (Ternary) Operator
The **ternary operator** (`?:`) in JavaScript is a concise way to write conditional expressions. It takes three operands: `condition ? valueIfTrue : valueIfFalse`.

Example: `let result = (age > 18) ? "Adult" : "Minor";`

It evaluates the condition and returns `valueIfTrue` if `true`, or `valueIfFalse` if `false`.

---
### Activating Strict Mode
Strict mode in JavaScript is activated by adding `"use strict";` at the start of a script or function. It enforces stricter parsing and error handling, disallowing certain actions like undeclared variables, duplicate parameters, or deleting undeletable properties. Example:
```js
"use strict";
x = 5; // Error: x is not declared
```

### Functions
- Reusable blocks of code that perform a task or return a value.
- Defined using `function` keyword, arrow syntax (`=>`), or function expressions.
- Can accept parameters and have a return statement.
Example:
```js
function add(a, b) { return a + b; } // Function Declaration
const subtract = function(a, b) { return a - b; } // Function Expression
const multiply = (a, b) => a * b; // Arrow Function
```

#### **Types of Functions**:
1. Named Functions
2. Anonymous Functions
3. Immediately Functions or Immediately Invoked Function Expressions (IIFE)

#### **What are Immediately Invoked Functions?**
**Immediately Invoked Function Expressions (IIFE)**: Functions defined and executed immediately after creation. They are wrapped in parentheses and followed by () to invoke them. Used to create a private scope and avoid polluting the global namespace. For examples:
```js
(function() {
	console.log("Runs immediately!";)
})();
```
Output: `Runs immediately!`

Can take parameters:
```js
(function(name) {
	console.log(`Hello, ${name}!`);
})("Alice");
```
Output; `Hello, Alice!`

#### **Difference between Function Declaration, Expression, and Arrow Functions**:
- **Function Declaration**: Defined with `function` keyword, hoisted, can be called before definition. Example: `function add(a, b) { return a + b; }`
- **Function Expression**: Function assigned to a variable, not hoisted, can be anonymous. Example: `const multiply = function(a, b) { return a * b; }`
- **Arrow Function**: Concise syntax using `=>`, not hoisted, no own `this` or `arguments`, ideal for short functions. Example: `const divide = (a, b) => a / b;`

##### **What do you mean by "hoisted" and "no own `this` or `arguments`?**
- **Hoisted**: In JavaScript, function declarations and variable declarations (with `var`) are moved to the top of their scope during compilation, allowing them to be used before their definition. Function declarations are fully hoisted (name and body), so you can call them anywhere in the scope. Function expressions and arrow functions are not hoisted, as only the variable declaration (if `var`) is hoisted, not the function assignment. For example:
```js
console.log(add(2, 3)); // Works: 5
function add(a, b) { return a + b; } // Hoisted

console.log(multiply(2, 3)); // Error: multiply is not a function
const multiple = function(a, b) { return a * b; }; // Not hoisted
```
- **No own `this` or `arguments`**: Arrow functions do not have their own `this` binding or `arguments` object. Instead, they inherit `this` from the surrounding (lexical) scope and cannot access a local `arguments` array. This makes them unsuitable for methods needing dynamic `this` (like object methods) or functions relying on `arguments`. For example:
```js
const obj = {
	value: 42,
	arrow: () => console.log(this.value), // Inherits global this (undefined in strict mode)
	regular: function() { console.log(this.value); } // Refers to obj
};
obj.arrow(); // undefined
obj.regular(); // 42

function regularFn() { console.log(arguments); } // Logs arguments array
const arrowFn = () => console.log(arguments); // ReferenceError: arguments is not defined
regularFn(1, 2, 3); // [1, 2, 3]
```

### Arrays
- Ordered, zero-indexed lists for storing multiple values (of any type).
- Created with `[]` or `new Array()`.
- Support methods like `push()`, `pop()`, `slice()`, and `forEach()`.
For example:
```js
let arr = [1, "hello", true];
arr.push(2); // [1, "hello", true, 2]
```
#### **Basic Array Operations (Methods)**
- **`indexOf`**: Returns the index value of element in array. For example:
```js
arr.indexOf("hello"); // 2
arr.indexOf("hi"); // Returns: -1, because element "hi" does not exist in array
```
- **`includes()`**: Returns a boolean if argument/element is in array. For example:
```js
let arr = [1, "hello", true];
arr.includes("hello"); // true
arr.includes(2); // false
```
- **`push()`**: Adds one or more elements to the end of an array. Returns new length. For example:
```js
let arr = [1, 2, 3]
arr.push(4); // [1, 2, 3, 4]
```
- **`pop()`**: Removes and returns the last element of an array. For example:
```js
let arr = [1, 2, 3]
arr.pop(); // [1, 2]; returns 3
```
- **`shift()`**: Removes and returns the first element of an array. For example:
```js
let arr = [1, 2, 3]
arr.shift(); // [2, 3]; returns 1
```
- **`unshift()`**: Adds one or more elements to the start of an array. Returns new length. For example:
```js
let arr = [1, 2, 3]
arr.unshift(0); // [0, 1, 2, 3]
```
- **`forEach()`**: Executes a function for each array element. For example:
```js
arr.forEach(num => console.log(num)); // Logs each element
```
- **`map()`**: Creates a new array with results of calling a function on each element. For example:
```js
let arr = [1, 2, 3]
arr.map(num => num * 2); // [2, 4, 6]
```
- **`filter()`**: Creates a new array with elements that pass a test function. For example:
```js
let arr = [1, 2, 3]
arr.filter(num => num > 1); // [2, 3]
```
- **`slice()`**: Returns a shallow copy of a portion of an array (start, end indices). For example:
```js
let arr = [1, 2, 3, 4]
arr.slice(1, 3); // [2, 3]
```
- **`splice()`**: Adds/removes elements at a specific index, modifying the array. Returns removed elements. For example:
```js
let arr = [1, 2, 3]
arr.splice(1, 1, 'a'); // [1, 'a', 3]
```

> **1. Does `-1` refer to the last element in a JavaScript array like in Python?**
> 
> No. JavaScript arrays are zero-indexed, and negative indices are not supported for direct access. To access the last element, you can use `array[array.length - 1]` or methods like `at(-1)` (introduced in ES2022).
> 
> In Python, `list[-1]` accesses the last element, but JavaScript requires explicit length calculation or `at()` for this behavior.
> 
> For example:
```js
let arr = [1, 2, 3];
console.log(arr[arr.length - 1]); // 3 (last element)
console.log(arr.at(-1)); // 3 (last element, modern JS)
console.log(arr[-1]); // undefined (negative indices don’t work)
```

> **2. Using `slice()` to select everything from a starting index to the end of the array**
> 
> The `slice(start, end)` method returns a shallow copy of a portion of an array from `start` (inclusive) to `end `(exclusive). To select everything from a starting index to the end, use `slice(start)` without specifying an `end` parameter. This implicitly includes all elements until the array’s end. For example:
```js
let arr = [1, 2, 3, 4, 5];
console.log(arr.slice(2)); // [3, 4, 5] (from index 2 to end)
console.log(arr.slice(1)); // [2, 3, 4, 5] (from index 1 to end)
```
> You can also use negative indices with `slice()`, where `-n` counts from the end:
```js
console.log(arr.slice(-2)); // [4, 5] (last 2 elements)
```
> The original array remains unchanged.

**3. Elaborating on `splice()`**

The splice(start, deleteCount, ...items) method modifies an array by removing, replacing, or adding elements at a specified index. It’s more versatile (and destructive) than slice(), as it changes the original array and returns an array of removed elements.

- **Parameters**:
    - `start`: Index where modification begins (can be negative to count from the end).
    - `deleteCount`: Number of elements to remove starting from `start.` If `0`, no elements are removed.
    - `...items` (optional): Elements to insert at `start` after removal.
- **Return Value**: An array of the removed elements (empty if none removed).
- **Key Features**:
    - Modifies the original array in place.
    - Can delete, insert, or replace elements in one operation.
    - Useful for dynamic array manipulation.

```js
// Removing elements
let arr = [1, 2, 3, 4];
let removed = arr.splice(1, 2); // Start at index 1, remove 2 elements
console.log(arr); // [1, 4] (original array modified)
console.log(removed); // [2, 3] (removed elements)

// Adding elements
let arr = [1, 2, 3];
arr.splice(1, 0, 'a', 'b'); // Start at index 1, remove 0, add 'a', 'b'
console.log(arr); // [1, 'a', 'b', 2, 3]

// Replacing elements
let arr = [1, 2, 3];
arr.splice(1, 1, 'x'); // Start at index 1, remove 1, add 'x'
console.log(arr); // [1, 'x', 3]

// Using negative indices
let arr = [1, 2, 3, 4];
arr.splice(-2, 1, 'y'); // Start at second-to-last, remove 1, add 'y'
console.log(arr); // [1, 2, 'y', 4]
```

**Key Notes**:

- `splice()` is memory-efficient for small changes but can be costly for large arrays due to shifting elements.
- Unlike `slice(),` it’s not used for copying but for direct array modification.
- Be cautious with `deleteCount`; if omitted, it removes all elements from `start` to the end.

These distinctions make `splice()` a powerful tool for array manipulation, while `slice()` is better for non-destructive extraction.

### Objects
- Data structures storing key-value pairs. Keys are strings (or symbols), values can be any type (numbers, strings, arrays, functions, etc.).
- Created using `{}` or `new Object()`.
Example:
```js
let obj = {name: "Alice", age: 25}
```
Objects are mutable, reference-based, and support methods and properties.

> Use **arrays** for ordered data and **objects** for unstructured data.

#### **Dot vs. Bracket Notation**
Key values are also known as 'property' values of objects. Using these key values we can get an object property.

- **Dot Notation**: Accesses object properties using a dot and the property name as an identifier. Faster, cleaner, but only works with valid identifier names (e.g., no spaces or special characters).
```js
const obj = {name: "Alice"}
obj.name // "Alice"
```
- **Bracket Notation**: Accesses properties using square brackets and a string or expression. More flexible, handles dynamic keys or invalid identifiers.
```js
const obj = {name: "Alice", "full name": "Alice Smith"}
obj["full name"] // "Alice Smith"
```

> For more about dot vs. bracket notation, see GrokAI's explanation: https://grok.com/share/bGVnYWN5_725acdde-4991-44dd-9464-ad6e6af78391

**Key Differences**:

- **Use Case**: Dot notation is preferred for static, valid identifiers due to simplicity. Bracket notation is used for dynamic keys, invalid identifiers, or when the property name is computed.
- **Flexibility**: Bracket notation can handle any key, while dot notation is restricted to valid identifiers.
- **Performance**: Dot notation is marginally faster, but the difference is negligible in most cases.

```js
let obj = { name: "Alice", "full name": "Alice Smith" };
console.log(obj.name); // Dot: "Alice"
console.log(obj["name"]); // Bracket: "Alice"
console.log(obj["full name"]); // Bracket: "Alice Smith"
// console.log(obj.full name); // SyntaxError with dot
let key = "name";
console.log(obj[key]); // Bracket with variable: "Alice"
```

#### **Object Methods**
Objects can have methods (also known as functions) as value or keys (or properties) in them. See example:
```js
const obj = {
	firstName: "Allwell",
	lastName: "Agwu-Okoro",
	age: 30,
	calcBirthYear: function() {
		return 2025 - this.age;
	}
}
obj.calcBirthYear() // Returns: 1995
```

> For more advanced built-in JS object methods, see [GrokAI](https://grok.com/share/bGVnYWN5_9ea7c4d6-610b-44dd-b861-902dc6b76b67). See link content below:

- **`Object.keys(obj)`**: Returns an array of an object's enumerable property names.
	- Use: Iterate over or inspect object keys.
	- Example:
	  ```js
		let obj = { name: "Alice", age: 25 };
		console.log(Object.keys(obj)); // ["name", "age"]
		```
- **`Object.values(obj)`**: Returns an array of an object's enumerable property values.
	- Use: Access all values without needing keys.
	- Example:
	  ```js
		let obj = { name: "Alice", age: 25 };
		console.log(Object.values(obj)); // ["Alice", 25]
		```
- **`Object.entries(obj)`**: Returns an array of an object's enumerable key-value pairs as `[key, value]` arrays.
	- Use: Iterate over both keys and values.
	- Example:
	  ```js
		let obj = { name: "Alice", age: 25 };
		console.log(Object.entries(obj)); // [["name", "Alice"], ["age", 25]]
		for (let [key, value] of Object.entries(obj)) {
			console.log(`${key}: ${value}`); // name: Alice, age: 25
		}
		```
- **`Object.hasOwnProperty(prop)`**: Checks if an object has a specific property as its own (not inherited).
	- Use: Verify property existence safely.
	- Example:
	  ```js
		let obj = { name: "Alice" };
		console.log(obj.hasOwnProperty("name")); // true
		console.log(obj.hasOwnProperty("toString")); // false (inherited)
		```
- **`Object.assign(target, ...sources)`**: Copies enumerable properties from source objects to a target object.
	- Use: Merge objects or create shallow copies.
	- Example:
	  ```js
		let obj1 = { name: "Alice" };
		let obj2 = { age: 25 };
		let merged = Object.assign({}, obj1, obj2);
		console.log(merged); // { name: "Alice", age: 25 }
		```
- **`Object.freeze(obj)`**: Freezes an object, preventing new properties from being added or existing ones from being modified or deleted.
	- Use: Ensure immutability.
	- Example:
	  ```js
		let obj = { name: "Alice" };
		Object.freeze(obj);
		obj.name = "Bob"; // No effect
		console.log(obj); // { name: "Alice" }
		```
- **`Object.create(proto)`**: Creates a new object with the specified prototype object.
	- Use: Set up inheritance or create objects with specific prototypes.
	- Example:
	  ```js
		let proto = { greet: () => "Hello" };
		let obj = Object.create(proto);
		console.log(obj.greet()); // "Hello"
		```

**Notes**:

- These methods are static (called on the `Object` constructor) except `hasOwnProperty`, which is an instance method.
- Use `Object.keys`, `values`, and `entries` for iteration or data extraction.
- `Object.assign` and `freeze` are handy for managing object state.
- Always consider whether you need to handle inherited properties (use `hasOwnProperty` or `in` operator for checks).

### Iteration: The `for` Loop
A for loop in JavaScript iterates over a block of code a specified number of times. Syntax: `for (initialization; condition; increment/decrement) { code }`.

- **Initialization**: Runs once before the loop starts (e.g., let i = 0).  
- **Condition**: Checked before each iteration; loop continues if true (e.g., i < 5).  
- **Increment/Decrement**: Runs after each iteration (e.g., i++).

Example:
```js
for (let i = 0; i < 3; i++) {
	console.log(i); // Outputs: 0, 1, 2
}
```

### Iteration: The `while` Loop
A `while` loop in JavaScript repeatedly executes a block of code as long as a specified condition evaluates to `true`.

Syntax:
```js
while (condition) {
	// code
}
```

Example:
```js
let i = 0;
while (i < 3) {
	console.log(i); // Outputs: 0, 1, 2
	i++;
}
```

The loop stops when the condition becomes `false`. If the condition never changes, it can cause an infinite loop.

## Data Structures, Modern Operators and Strings
### The Nullish Coalescing Operator (`??`)
The **nullish coalescing operator (`??`)** returns the right operand if the left operand is `null` or `undefined`, otherwise returns the left operand.

Example:
```js
null ?? 5 // returns `5`
0 ?? 5 // returns `0`
```

It ignores falsy values like `0`, `""`, or `false`, unlike the `||` operator.

### Logical Assignment Operators
Logical assignment operators in JavaScript combine logical operations with assignment. They evaluate the right-hand operand based on the left-hand operand's value and assign the result to the left-hand operand. Introduced in ES2021, they are:

- **Logical AND Assignment (`&&=`)**: Assigns the right-hand value to the left-hand variable only if the left-hand value is truthy. Example: `a &&= b` is equivalent to `a = a && b`.
- **Logical OR Assignment (`||=`)**: Assigns the right-hand value to the left-hand variable only if the left-hand value is falsy. Example: `a ||= b` is equivalent to `a = a || b`.
- **Nullish Coalescing Assignment (`??=`)**: Assigns the right-hand value to the left-hand variable only if the left-hand value is `null` or `undefined`. Example: `a ??= b` is equivalent to `a = a ?? b`.

Examples:
```js
let x = 0;
x &&= 10; // x remains 0 (0 is falsy, so 10 isn't assigned)
x ||= 5;  // x becomes 5 (0 is falsy, so 5 is assigned)
x ??= 20; // x remains 5 (5 is neither null nor undefined)
```

If you ever need to assign a value to an object that's already defined, then use the logical AND operator assignment. For instance:
```js
const school = {
	name: 'Britarch',
	type: 'Montessori'
}
school.location &&= 'New location' // this doesn't assign any value

const university = {
	name: 'UNILAG',
	type: 'Tetiary',
	location: 'Akure'
}
university.location &&= 'Lagos' // Returns: { location: 'Lagos' }
```

### Optional Chaining `?.`
Optional chaining (`?.`) allows safe access to nested object properties or methods without throwing errors if a property is `null` or `undefined`. It short-circuits and returns `undefined` if the chain breaks. 

Example: `obj?.prop?.nested` returns `undefined` if `obj` or `prop` is `null`/`undefined`, instead of crashing.

### Maps
**Maps** in JavaScript are collections of key-value pairs where keys can be any data type (not just strings like objects). They maintain insertion order and offer methods like `set()`, `get()`, `has()`, `delete()`, and `clear()`.

**Key Features**:
- Keys can be primitives, objects, or functions.
- Size accessible via `size` property.
- Iterable using `for...of` or `forEach`.

Example:
```js
let map = new Map();
// set keys and values in the Map
map.set("key1", "value1").set(42, "number");
console.log(map.get("key1")); // "value1"
console.log(map.get(42)); // "number"
console.log(map.size); // 2

// another way to set values in a Map is:
let secondMap = new Map([
	["key1", "value1"],
	[42, "number"]
])
```

> We can convert an object to a map, like so:
```js
// Convert object to map
const newMap = new Map(Object.entries(oldObject));
```

```js
// Converting a map to an array
console.log([...oldMap]);
// Converting map keys into an array
console.log([...oldMap.keys()]);
// Converting map values into an array
console.log([...oldMap.values()]);
```

## Working with Arrays
### `find` Method


## Object-Oriented Programming (OOP)
- Object-oriented programming (OOP) is a programming paradigm based on the concept of objects
- we use objects to **model** (describe) real-world or abstract features
- objects may contain data (properties) and code (methods). By using objects, we pack **data and the corresponding behaviour** into one block
- in OOP, objects are **self-contained** pieces/blocks of code
- objects are building blocks of applications, and **interact** with one another
- interactions happen through a **public interface** (API): methods that the code **outside** of the object can access and use to communicate with the object;
- OOP was developed with the goal of organizing code, to make it **more flexible and easier to maintain** (avoid "spaghetti code").

### Four Fundamental OOP Principles
these principles guide you in your creation of classes. they are:
1. Abstraction
2. Encapsulation
3. Inheritance
4. Polymorphism

#### **Abstraction**
Hiding away low-level implementations of a class or blueprint that do not matter to the end user of the class or blueprint.

![js abstraction](../assets/Pasted%20image%2020250504114801.png)

#### **Encapsulation**
Keeping certain properties and methods **private** inside the class. This way, they are **not accessible outside the class**. Some methods can be **exposed** as a public interface (API).

![js encapsulation](../assets/Pasted%20image%2020250504114936.png)

#### **Inheritance**
Making all properties and methods or a certain (parent) class **available to a child class**, forming a hierarchical relationship between classes. This allows us to **reuse common logic** and to model real-world relationships.

![js inheritance](../assets/Pasted%20image%2020250504115201.png)

#### **Polymorphism**
A child class can **overwrite** a method it inherited from a parent class [it's more complex than that, but enough for now].

![js polymorphism](../assets/Pasted%20image%2020250504115502.png)

### OOP in JavaScript: Prototypes
#### **Classical OOP: Classes**
- objects (instances) are **instantiated** from a class, which functions like a blueprint;

![classical oop](../assets/Pasted%20image%2020250504120214.png)

#### **OOP in JS: Prototypes**
- objects are **linked** to a prototype object
- prototypal inheritance: the prototype contains methods (behaviour) that are **accessible to all objects linked to that prototype**
- in other programming languages like Python, a class inherits from another class. In JavaScript, an instance (object) inherits from a class (or prototype)
- behaviour is **delegated** to the linked prototype object

![oop in js - prototypes](../assets/Pasted%20image%2020250504120737.png)

### 3 Ways of Implementing Prototypal Inheritance in JavaScript
how do we create prototypes or link objects to prototypes? how can we create new objects, without having classes?

1. Constructor Functions
	- technique to create objects from a function
	- this is how built-in objects like arrays, maps or sets are actually implemented
2. ES6 Classes
	- modern alternative to constructor function syntax
	- "syntactic sugar" behind the scenes, ES6 classes work exactly like constructor functions
	- ES6 classes do **NOT** behave like classes in "classical OOP"
3. `Object.create()`
	- the easiest and most straightforward way of linking an object to a prototype object

### Prototypes

> Never create methods inside constructor functions, like this:
> 
> Instead, use prototypal inheritance.
```js
const Person = function (firstName, birthYear) {
	// Instance properties
	this.firstName = firstName;
	this.birthYear = birthYear;
	
	// Never do this:
	this.calcAge = function () {
		console.log(2037 - this.birthYear);
	}
}
```

**What happens when the `new` operator is called?**
1. An empty object is created `{}`
2. `this` keyword in constructor function call is set to the new object
3. The new object is linked (`__proto__` property) to the constructor function's prototype property
4. The new object is returned from the constructor function call

### The Prototype Chain

![prototype chain](../assets/Pasted%20image%2020250504125406.png)

### ES6 Classes
- classes can be represented through an expression or declaration
- when methods are defined inside classes they are available as object prototypes to the instances of the class instead of as a `hasOwnProperty === true` object of the instance


```js
// Class expression
const PersonCl = class {
	...
}

// Class declaration
class PersonCl {
	// Constructor function, called with `new` keyword
	constructor(firstName, birthYear) {
		this.firstName = firstName;
		this.birthYear = birthYear;
	}

	// Methods
	calcAge() {
		console.log(2037 - this.birthYear);
	}

	greet() {
		console.log(`Hey ${this.firstName}`);
	}
}

const jessica = new PersonCl('Jessica', 1996);

// Showing that instance prototype is the same as the class prototype object
console.log(jessica.__proto__ === PersonCl.prototype); // Returns: true
```

**Some things to NOTE about classes:**
1. classes are NOT hoisted
2. classes are first-class citizens
3. classes are executed in strict mode

#### **Setters and Getters**
This is how getters and setters work for any object in JavaScript:

```js
const account = {
	owner: "Jonas",
	movements: [200, 530, 120, 300],

	// Getter
	get latest() {
		return this.movements.slice(-1).pop();
	},

	// Setter
	set latest(mov) {
		this.movements.push(mov);
	},
};

// Testing Getter
console.log(account.latest); // Returns: 300

// Testing Setter
account.latest = 50;
console.log(account.movements); // Returns: [200, 530, 120, 300, 50]
```

In classes:

```js
class PersonCl {
	constructor(firstName, birthYear, fullName) {
		this.firstName = firstName;
		this.birthYear = birthYear;
		this.fullName = fullName;
	}

	calcAge() {
		console.log(2037 - this.birthYear);
	}

	greet() {
		console.log(`Hey ${this.firstName}`);
	}

	// If setting a property that already exists
	set fullName(name) { // Getters and Setters are perfect for validation
		if (name.includes(' ')) this._fullName = name;
		else alert(`${name} is not a full name!`);
	}

	get fullName() {
		return this._fullName;
	}
}
```

#### **Static Methods**
static methods are only available on the constructor function. they are not available anywhere else. This is different from instance methods, which are available on each instance of the class or object.

Example:
```js
class PersonCl {
	constructor(firstName, birthYear, fullName) {
		this.firstName = firstName;
		this.birthYear = birthYear;
		this.fullName = fullName;
	}

	calcAge() {
		console.log(2037 - this.birthYear);
	}

	greet() {
		console.log(`Hey ${this.firstName}`);
	}

	// If setting a property that already exists
	set fullName(name) { // Getters and Setters are perfect for validation
		if (name.includes(' ')) this._fullName = name;
		else alert(`${name} is not a full name!`);
	}

	get fullName() {
		return this._fullName;
	}

	static hey() {
		console.log('Hey there 👋🏾');
	}
}
```

#### **`Object.create()`**
This is another way of creating objects. Unlike ES6 classes and through using constructor functions, `Object.create()` lets you perform object inheritance - create an instance object that inherits properties and methods from another object (usually specified as one of the arguments in the `Object.create()` function.

Example:
```js
const PersonProto = {
	calcAge() {
		console.log(2037 - this.birthYear);
	},
};

const steven = Object.create(PersonProto);
console.log(steven);
steven.name = 'Steven';
steven.birthYear = 2002;
steven.calcAge();

console.log(steven.__proto__ === PersonProto);
```

- Think of `Object.create()` as linking directly the prototype (class/object attributes, properties, and methods NOT the constructor function attributes/objects/properties) of an object or class to another object or class.
- Making the object or class which `Object.create()` has been called on to become the inherited object or class of the passed in prototype.
- This is otherwise known in JavaScript as *Prototypal Inheritance*.

##### MANUALLY INITIALIZE OBJECTS AND PROPERTIES OR ATTRIBUTES ON `Object.create()` OBJECTS

To manually add objects and properties or attributes on `Object.create()` objects, we do the following:

```js
// Instead of doing this...
const steven Object.create(PersonProto);
steven.name = 'Steven';
steven.birthYear = 2002;
steven.calcAge(); // Returns: 2038 - 2002 = 35

// Do this...
const PersonProto = {
	calcAge() {
		console.log(2037 - this.birthYear);
	},
	init(firstName, birthYear) {
		this.firstName = firstName;
		this.birthYear = birthYear;
	},
};

const sarah = Object.create(PersonProto);
sarah.init('Sarah', 1979);
sarah.calcAge(); // Returns: 2037 - 1979 = 58
```

### Inheritance between Classes: Constructor Functions
We use `Object.create()` to create prototypal inheritance between objects or classes built with constructor functions.

![constructor functions](../assets/Pasted%20image%2020250505090819.png)

```js
// Creating class object via Constructor Functions
const Person = function (firstName, birthYear) {
	this.firstName = firstName;
	this.birthYear = birthYear;
};

// Person prototype method
Person.prototype.calcAge = function () {
	console.log(2037 - this.birthYear);
};

// Student constructor function
const Student = function (firstName, birthYear, course) {
	this.firstName = firstName;
	this.birthYear = birthYear;
	this.course = course;
};

// Student prototype method
Student.prototype.introduce = function () {
	console.log(`My name is ${this.firstName} and I study ${this.course}`);
}

const mike = new Student('Mike', 2020, 'Computer Science');
mike.introduce(); // Calls inherited function 'introduce' from Student prototype
```

If we wanted to make `Student` object inherit from `Person` object, we cannot do this:
```js
// Student constructor function
const Student = function (firstName, birthYear, course) {
	Person(firstName, birthYear); // this will not work
	// the above code will not work because Person object is a constructor
	// function and therefore will need the 'new' keyword at the beginning
	// of it to return;
	// Otherwise, it will return 'undefined'. And the 'this' keyword will
	// not work well with 'undefined'.
	this.course = course;
};

// The next step...

// Student constructor function
const Student = function (firstName, birthYear, course) {
	new Person(firstName, birthYear); // yet this will not work either, why?
	// because we need to set the 'this' keyword to call on the Student
	// object. For this to work, we do the next step.
	this.course = course;
};

// The right way...

// Student constructor function
const Student = function (firstName, birthYear, course) {
	Person.call(this, firstName, birthYear); // this call the Person
	// constructor function on the 'this' keyword of Student.
	this.course = course;
};
```

This is the idea of the *Prototype Chain* for inheritance between classes.

![prototype chain in constructor functions](../assets/Pasted%20image%2020250505092600.png)

To make `Student` inherit from `Person` we use the `Object.create()` function. Like so:

```js
// Creating class object via Constructor Functions
const Person = function (firstName, birthYear) {
	this.firstName = firstName;
	this.birthYear = birthYear;
};

// Person prototype method
Person.prototype.calcAge = function () {
	console.log(2037 - this.birthYear);
};

// Student constructor function
const Student = function (firstName, birthYear, course) {
	this.firstName = firstName;
	this.birthYear = birthYear;
	this.course = course;
};

Student.prototype = Object.create(Person.prototype); // this works
// Student.prototype = Person.prototype; // this will not work because it
// doesn't create a prototype chain between 'Student' and 'Person'

// Student prototype method
Student.prototype.introduce = function () {
	console.log(`My name is ${this.firstName} and I study ${this.course}`);
}

const mike = new Student('Mike', 2020, 'Computer Science');
mike.introduce(); // Calls inherited function 'introduce' from Student prototype
mike.calcAge(); // 'mike' object now has access to the 'Person' prototype method

console.log(mike.__proto__); // Returns: Points to 'Person' constructor function
// but is 'Student' prototype (needs to point to 'Student' constructor func)
// see fix below
console.log(mike.__proto__.__proto__); // Returns: 'Person' prototype

// Fix: make 'Student' prototype point to 'Student' constructor function
Student.prototype.constructor = Student

// Now, 'mike.__proto__' points to 'Student' constructor function

console.log(mike instanceof Student); // Returns: true
console.log(mike instanceof Person); // Returns: true
console.log(mike instanceof Object); // Returns: true
```

![correct prototypal inheritance initiated between Student and Person objects](../assets/Pasted%20image%2020250505093132.png)

> To learn more about inheritance between classes via constructor functions, see [Udemy notes](https://www.udemy.com/course/the-complete-javascript-course/learn/lecture/22649085?start=797#notes).

### Inheritance between Classes via ES6 Classes
In ES6 classes, to make the `Student` class inherit from the `Person` class, we use the `extends` keyword. Like so:

```js
class StudentCl extends PersonCl {
	constructor(fullName, birthYear, course) {
		// Always needs to happen first. Why?
		// So the 'this' keyword would be available
		super(fullName, birthYear); // Calls the constructor function of the
		// parent class, which in this case is 'PersonCl'
		this.course = course;
	}

	introduce() {
		console.log(`My name is ${this.fullName} and I study ${this.course}`);
	}
}

const martha = new StudentCl('Martha Jones', 2012, 'Computer Science');
martha.introduce(); // this works as an instance method
martha.calcAge(); // this works as an inherited class method (inherited from
// 'PersonCl' class object).
```

> Quick one: Simply using the `extends` keyword already performs the class inheritance in the case of ES6 classes without the need to write the constructor function.
> The constructor function is usually written if you want to specify or add additional methods to the `StudentCl` class or overwrite existing attributes or methods already in the `PersonCl` parent class. For example:
```js
class StudentCl extends PersonCl

const martha = new StudentCl('Martha Jones', 2012); // Now, 'martha' is an
// instance of the 'StudentCl' class, which inherits from 'PersonCl' class.
```

Now, let's override one of the instance methods of the parent class `calcAge()`.

```js
class StudentCl extends PersonCl {
	constructor(fullName, birthYear, course) {
		super(fullName, birthYear);
		this.course = course;
	}

	introduce() {
		console.log(`My name is ${this.fullName} and I study ${this.course}`);
	}

	// Overriding the 'calcAge()' instance method in the 'PersonCl' parent class
	calcAge() {
		console.log(`I'm ${2037 - this.birthYear} years old, but as a student I feel more like ${2037 - this.birthYear + 10}`);
	}
}

const martha = new StudentCl('Martha Jones', 2012, 'Computer Science');

martha.calcAge(); // Returns 'calcAge()' method in 'StudentCl' class which
// overrides 'calcAge()' method in 'PersonCl' class.
```

### Inheritance between Classes via `Object.create()`
Performing class inheritance using the `Object.create()` method draws from the knowledge of using `Object.create()` to create classes. See our [last example](javascript.md#**`Object.create()`**) on `Object.create()`.

```js
const PersonProto = {
	calcAge() {
		console.log(2037 - this.birthYear);
	},

	init(firstName, birthYear) {
		this.firstName = firstName;
		this.birthYear = birthYear;
	},
};

const steven = Object.create(PersonProto); // 'steven' inherits from
// 'PersonProto' class

const StudentProto = Object.create(PersonProto); // 'StudentProto' inherits from
// 'PersonProto' class
const jay = Object.create(StudentProto); // 'jay' inherits from 'StudentProto'
// class

// Now, let's set up an 'init' method in StudentProto so we don't have to
// manually set the properties of 'jay'

StudentProto.init = function (firstName, birthYear, course) {
	PersonProto.init.call(this, firstName, birthYear);
	this.course = course;
}

// Now we can call...
jay.init('Jay', 2010, 'Computer Science');

// Let's add some other methods to the 'StudentProto' class object

StudentProto.introduce = function () {
	console.log(`My name is ${this.fullName} and I study ${this.course}`);
}

// Now we can call...
jay.introduce(); // this will work
jay.calcAge(); // this will also work, because it inherits 'calcAge()' from
// parent class 'PersonProto'
```

In this case, we don't use or worry about `constructor` objects anymore, prototype properties (like `.prototype`) , or the `new` keyword. That's the major difference between this method and constructor functions or ES6 classes.

### Encapsulation: Private Class Fields and Methods
Four types of encapsulation of private class fields and methods in JavaScript are:
1. Public fields
2. Private fields
3. Public methods
4. Private methods
There are also *STATIC* versions of these four fields but we'll not talk about them yet.

Using an `Account` class to show examples:

```js
class Account {
	// Fields must end with ';'
	locale = navigator.language; // Public field
	bank = 'Bankist';
	#movements = []; // Private field
	#pin;

	constructor(owner, currency, pin) {
		this.owner = owner;
		this.currency = currency;
		this.#pin = pin;

		console.log(`Thanks for opening account, ${owner}`);
	}

	// Public interface
	getMovements() { // Public methods
		return this.#movements;
	}

	
	deposit(val) {
		this.#movements.push(val);
	}

	withdraw(val) {
		this.deposit(-val);
	}

	#approveLoan(val) { // Private method
		return true;
	}

	requestLoan(val) {
		if (this.#approveLoan(val)) {
			this.deposit(val);
			console.log(`Loan approved`);
		}
	}

	static test() { // Public static method
		console.log('I\'m, a public static method.');
	}

	static #privateTest() {
		console.log('I\'m, a private static method.');
	}
}

const acc1 = new Account('Jonas', 'EUR', 1111);
acc1.deposit(300); // this works
acc1.withdraw(100); // this works as well
acc1.movements = []; // this will NOT affect the private 'movements' field in
// 'Account' class

acc1.test(); // this will NOT work, because 'test' function is a static method
// meaning, it can only be called on the class (not the instance object)
Account.test(); // this will work: Returns: "I'm a public static method."
Account.#privateTest(); // this will NOT work: Error: 'cannot access private
// field'
```

#### **Enable Method Chaining**
To enable method chaining in your ES6 classes, as is possible in JavaScript, simply return the class object via the `this` keyword. Therefore, `return this;`.

```js
// To enable method chaining, as exemplified like so:
acc1
	.deposit(300)
	.withdraw(100)
	.withdraw(50)
	.requestLoan(25000)
	.withdraw(4000)
	.getMovements()

// In the class 'Account' return the class using 'this' for every function
// that manipulates or sets a value on the class object. Like so:

class Account {
	// Fields must end with ';'
	locale = navigator.language; // Public field
	bank = 'Bankist';
	#movements = []; // Private field
	#pin;

	constructor(owner, currency, pin) {
		this.owner = owner;
		this.currency = currency;
		this.#pin = pin;

		console.log(`Thanks for opening account, ${owner}`);
	}

	// Public interface
	getMovements() { // Public methods
		return this.#movements;
	}

	deposit(val) {
		this.#movements.push(val);
		return this;
	}

	withdraw(val) {
		this.deposit(-val);
		return this;
	}

	#approveLoan(val) { // Private method
		return true;
	}

	requestLoan(val) {
		if (this.#approveLoan(val)) {
			this.deposit(val);
			console.log(`Loan approved`);
		}
		return this;
	}

	static test() { // Public static method
		console.log('I\'m, a public static method.');
	}

	static #privateTest() {
		console.log('I\'m, a private static method.');
	}
}
```

In other words, we're returning the object itself on each of the methods which we want to be chainable.

### ES6 Classes Summary

![es6 classes summary](../assets/Pasted%20image%2020250505131209.png)

## Asynchronous JavaScript
Asynchronous JavaScript allows code to run non-blocking operations, enabling tasks like fetching data or timers to execute without halting the main program flow. It leverages the event loop, call stack, and callback queue to handle tasks in the background and process results when ready.

**Key Concepts**:

- **Event Loop**: Continuously checks the call stack and callback queue, moving completed asynchronous tasks to the stack for execution.
- **Callbacks**: Functions passed as arguments to run after an async operation completes. Example: `setTimeout(() => console.log("Delayed"), 1000)`.
- **Promises**: Objects representing eventual completion (or failure) of an async operation, with `.then()` for success and `.catch()` for errors. Example: `fetch(url).then(response => response.json())`.
- **Async/Await**: Syntactic sugar for Promises, allowing async code to look synchronous. Example: `const data = await fetch(url).then(res => res.json())`.

Example:
```js
async function getData() {
	try {
		const response = await fetch("https://api.example.com/data");
		const data = await response.json();
		console.log(data);
	} catch (error) {
		console.error("Error": error);
	}
}

getData();
console.log("Runs immediately"); // Output: "Runs immediately" logs first,
// then 'data' when the fetch completes.
```

Asynchronous JavaScript ensures efficient handling of I/O-bound tasks, keeping applications responsive.

### AJAX Calls
What are AJAX Calls?
*A*synchronous *J*avaScript *A*nd *X*ML: Allows us to communicate with remote servers in an *asynchronous way*. With AJAX calls, we can *request data* from web servers dynamically.

![ajax calls](../assets/Pasted%20image%2020250505134813.png)

### APIs
- Application Programming Interface: Piece of software that can be used by another piece of software, in order to allow applications to talk to each other
- There are many types of APIs in web development:
	- DOM API
	- Geolocation API
	- Own Class API
	- "Online" API or "Web" API or just API
- "Online" API: Application running on a server, that receives requests for data, and sends data back as response
- We can build our own APIs (requires back-end development, e.g. with Node.JS) or use **3rd-party** APIs

Here are some of the things you can do with APIs:

![api uses](../assets/Pasted%20image%2020250505135655.png)

XML (Extensible Markup Language) format for data transfer used to be popular back in the day but today JSON (JavaScript Object Notation) is most popular.



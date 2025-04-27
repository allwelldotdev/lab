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






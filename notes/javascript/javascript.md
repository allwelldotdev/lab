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
Reusable blocks of code that perform a task or return a value. Defined using `function` keyword, arrow syntax (`=>`), or function expressions. Can accept parameters and have a return statement. Example:
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
	arrow: () => conso
}
```



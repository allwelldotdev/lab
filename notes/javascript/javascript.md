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



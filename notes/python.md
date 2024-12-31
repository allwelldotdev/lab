Learning from Dr Fred Baptiste's Python Fundamentals course on [Udemy](https://www.udemy.com/course/python3-fundamentals/).

- Created by **Guido van Rossum** in 1989.
- Was named after the British comedy group **Monty Python**.
- Still actively developed today
	- Python 2.0 released in 2000 → last release was 2.7 (end of April 2020).
- The reference implementation of python is **CPython** otherwise known as **Canonical Python**. This is the standard, official python.

## Virtual Environments
Virtual Environments `venv` in Python are used to make a copy of Python installation to enable you switch between Python versions and Python dependency versions or differences for your Python project.

Within Python virtual environments (or venv) are scripts to "activate" / "deactivate" the environment.

### Creating virtual environments
```bash
<python version/path> -m venv <your_env_name>
```

For example, if using python3.9 and you'd like to name your virtual environment 'py39_course':
```bash
python3.9 -m venv py39_course
```

> Where do you store the `venv` file? I usually store mine in the same directory as the project I'm working on to keep things simple and less complicated.

> If using zsh on wsl:ubuntu and you encounter a problem of not seeing the indication of the activated virtual environment via the text in parenthesis next to your user prompt in your terminal (as shown in the image below). Do the following:

![venv indication in python](assets/Pasted%20image%2020241224235005.png)

1. Open `~/.zshrc` file and check that you have included `virtualenv` in your plugins array, like so:
   ```bash
   plugins=(git virtualenv python)
   ```
2. If plugin included and venv indication is not working, disable prompt suppression:
   ```bash
   # export VIRTUAL_ENV_DISABLE_PROMPT=1
   ```
3. Check common configuration files for the env variable `VIRTUAL_ENV_DISABLE_PROMPT`:
   ```bash
   grep -r "VIRTUAL_ENV_DISABLE_PROMPT" ~/.zshrc ~/.zshenv ~/.oh-my-zsh/*
     ```
4. If after commenting out the env variable `export` command, it's still set. Unset the environment variable:
   ```bash
   unset VIRTUAL_ENV_DISABLE_PROMPT
     ```
5. Reload `~/.zshrc`:
   ```bash
   source ~/.zshrc
	 ```

> Switch to another venv when a venv is activated by running:

```bash
source <path_to_venv>/bin/activate
```
## Python package installer `pip`
`pip` lets you install python packages to your `venv`.
- activate the virtual environment first (sets your PATH)
- `pip install package_name`
	- install versions
	- `pip install package_name==1.3.2`
	- `pip install package_name<=1.2`
	- `pip install package_name>2.0`
- use `requirements.txt` to keep track of required packages and versions.
	- `pip install -r requirements.txt` to install packages from `requirements.txt`

## Running python
### Python REPL (read-eval-print-loop).
The Python REPL is the interactive mode of reading, compiling, running your code. This is also known as **interactive mode**.
![python repl](assets/Pasted%20image%2020241225112258.png)
### Python Script mode.
This is different from Python REPL mode. Here, in Script mode, you write all your code first then call Python and it runs your code and outputs it to the system.
![python script mode](assets/Pasted%20image%2020241225112435.png)
### Jupyter Notebooks
Jupyter Notebooks is software that has been developed on top of Python that wraps the REPL into a nicer interface. This is not the only one software available that does this. There are others are well.  
Jupyter Notebooks is a REPL, but browser-based.

![jupyter notebook](assets/Pasted%20image%2020241225115903.png)

The files you create and save into your repo locally from Jupyter Notebooks are called '**a notebook**' and they usually use the `.ipynb` extension.

> Jupyter Notebooks is also known as **interactive python**.

To start up Jupyter Notebooks in terminal, use command:
```bash
jupyter notebook <path/to/open/in/notebook>
```
#### Jupyter Notebook commands
- `CTRL + Enter` runs code in selected cell
- `ESC` exists edit mode to view mode
- `Enter` to edit a cell
- In view mode, `dd` deletes the selected cell
- `y` transforms a cell to code
- `m` transforms a cell to markdown
- `r` transforms a cell to raw.
## Python basics
### Integers
- The `int` type
	- used to represent literal numbers: 0, 1, 100, -100, etc.
	- integers can be of any magnitude (as long as you have memory!)
	- integer numbers can be created from a literal in the python code
		- 100
		- -100
		- 10_500_000 (note how you can use underscores for readability)
	- or, as the result of some calculation (expression)
		- 1 + 1
#### Integer representations
- computers only know two numbers
	- 0 and 1 → binary system, aka base 2
- any number in a computer is represented using powers of 2
### Floats
- the `float` type
- used to represent real numbers (floating point): 3.14, -1.3
- can use python literals to define a `float`
	- 3.14
	- -1.3
	- 1_234.567_876
- the decimal point differentiates a `float` from an `int` when using literals
	- 1 → `int`
	- 1.0 → `float`
#### Float binary representations
 - `floats` work in the same way as [Integer representations](#Integer%20representations)
 - `floats` are represented by using powers of 2 and fractions of powers of 2

$$\begin{align}
\frac{1}{2^2}+\frac{0}{2^2}+\frac{1}{2^3}&=\frac{1}{2}+\frac{0}{4}+\frac{1}{8} \\
&= 0.5 + 0 + 0.125 = 0.625
\end{align}$$

- certain numbers do not have a finite decimal representation $(\frac{1}{3})$
- same happens with binary representations!

$$0.1=\frac{0}{2} + \underbrace{\underbrace{\frac{0}{4} + \frac{0}{8} + \frac{1}{16} + \frac{1}{32}}_{0.09375} + \frac{0}{64} + \frac{0}{128} + \frac{0}{256} + \frac{0}{512}}_{.099609375} + \dots$$^latex-underbrace

#### Floats are not always exact
- bottom line: not all exact decimal numbers have an exact float representation
	- not a limitation of Python
	- all languages (incl. Excel) that use these binary fractions have that issue
	- ⚠️ be careful when comparing floats to one another
- there is a data type that can handle exact representations of decimal fractions
	- `Decimal` (we will look at this type later)
	- ⚠️ calculations using `Decimal` are much slower than `float`

### Objects
Entities created by Python, which have **state** *(data)* and **methods** *(functionality)*. This is otherwise known as **encapsulation**.
- `int` is an object
	- when we do `1+1`, python calls the `__add__()` method of the `int` object.
	- `10 + 100 == 110` is `(10).__add__(100) == 110`
- `float` numbers are objects too
	- a popular *functionality* for the `float` object is `as_integer_ratio()`
	- `as_integer_ratio()` gives you the fraction value of `floats` in ratio
	- `(0.125).as_integer_ratio() == 1, 8` represented in fraction as $\frac{1}{8}$

>Therefore, everything in Python is an object, which has **state** *(data)* and **methods** *(functionality)*.

#### Dot Notation
The dot notation is used to access the (**state** and **functionality**) attributes of objects. For example: `car.brand` or `car.accelerate(10, "mph")`.

##### Immutability & Mutability
- an object is **mutable** if its internal state **can** be changed
	- on or more attributes can be changed
- an object is **immutable** if its internal state **cannot** be changed
	- the state of the object is "set in stone"
- in Python many data types are immutable, ex:
	- integers
	- floats
	- booleans
	- strings
	- ...
- while some are mutable, ex:
	- lists
	- dictionaries
	- sets
	- ...

### Variables
- Variables are a store of value.
- `=` is used to assign values, it's not the equal sign in Python.
#### How Variable Assignment Works
$$\underbrace{apy}_{LHS} = \underbrace{0.25}_{RHS}$$
- python evaluates the RHS first
	- then it "assigns" that result to the symbol in the "LHS" (the LHS becomes a named reference to whatever results from RHS)

#### Variable Naming
- case sensitive: `apr` is a different symbol than `APR`
- *must* follow certain rules
- *should* follow certain conventions
##### Must-Follow Rules
- *start* with underscore `_` or letter `a-z A-Z` (unicode characters are actually okay but stick to `a-z A-Z`
- *followed* by a number of underscores or letters, or digits `0-9`
- for example: `var my_var index1 index_1 _var __var __add__`
	- variables like these `__add__` are referred to as dunder methods in Python. They are special characters that have special meanings for Python's internal operations. Therefore, even though they're legal variables, stay away from using them in your code as they could clash with Python internals. [Learn more](https://www.perplexity.ai/search/what-is-the-variable-naming-co-QQcwnxNxTUOFG2IoPJpPRA).
- cannot be reserved words: `True False if def and or`
##### Should-Follow Conventions
- **PEP 8** style guide → typical conventions followed by most Python devs. [Learn more](https://www.python.org/dev/peps/pep-0008/).
- terminology:
	- **camel case** → separate words are distinguished by uppercase letters: `accountBalance bankAccount`
	- **snake case** → separate words are distinguished by underscores: `account_balance bank_account`
- for standard variables: 
	- snake case
	- all lowercase letters
		- `account_balance` ✅
		- `account_Balance` ❌
- good idea to follow standard conventions
	- but sometimes you may want to break those conventions
	- that's okay - just have a good reason and be consistent.
> From the PEP 8 Style Guide:
> *A foolish consistency is the hobgoblin of little minds.
> (Emerson)*

### Arithmetic Operators
An operator is a programming language symbol that performs some operation on one or more values.
Certain types of operators include:
- arithmetic operators
- comparison (or relational) operators
- logical operators
The values an operator acts on are called **operands**
- An operator that works on a single operand is called a **unary operator**
- An operator that works on two operands is called a **binary operator**
- An operator that works on three operands is called a **ternary operator**
Unary Operators
- Unary Minus `-10`
- Unary Plus `+10`
Binary Operators
- Addition `10 + 20`
- Subtraction `20 - 10`
- Multiplication `10 * 2`
- Division `10 / 2`
- Power (exponentiation) `2 ** 4`
Use parenthesis `(` and `)` to group expressions.

#### Operand Types
- Arithmetic operators can act on any numerical type: `int` & `float`
- What the operator does is determined by the type of the operands

#### The Power Operator
- The power operator works just like its mathematical counterpart
- Python supports floats for either operand of the `**` operator
- Python also supports negative bases with real exponents 
	- complex numbers
	- it's actually a numerical type in Python (`complex`)

### Operator Precedence
Operators have precedence
- an operator with higher precedence will *bind* more tightly
- fancy way of saying *it will be evaluated first*
Advice:
- relying on operator precedence is tricky
- very easy to introduce bugs
- use parenthesis
	- it's just a few keystrokes more and will save a lot of pain later!
	- use `(2 * 10) + 5`
> Always specify parenthesis in your expressions, both to be safe, and to reduce confusion for someone else, who may not be as familiar with operator precedence and may be reading your code.
### Integer Division and Modulus
- Python integer division: `//`
	- `131 // 3 == 43`
- Remainder: use python mod operator `%` ^6dabf9
	- `131 % 3 == 2`

#### The `//` Operator
- `a // b` calculates the "integer portion" of `a / b`
	- easy to understand when `a` and `b` are positive
- Reality: `a // b` is the floor of `a / b`
	- `floor(x)` is the *largest* integer number `<= x`
	![floor in the integer division operator](assets/Pasted%20image%2020241229155420.png)
	- `12 / 5 == 2.4` and `12 // 5 == 2`
	- While `-12 / 5 == -2.4` then `-12 // 5 == -3`

#### The `mod` Operator
- As in [the Integer Division](#The%20`//`%20Operator) operator, negative numbers here complicate things a bit!
- But `%` is defined for negative integers and even floats as well.
- Going back to [the earlier example](#^6dabf9):
$$\frac{131}{3} = floor\left(\dfrac{131}{3}\right) + \frac{131mod3}{3}$$
- In other words: `a / b = a // b + (a % b)/b`
- Rewriting through algebraic manipulation:
	- 1st iteration: `a % b = b (a / b - a // b)`
	- Final iteration: `a % b = a - b (a // b)` *this value here is what modulus us defined as* ^787ec5
- Therefore, `a % b` is defined as the value that satisfies [the above equation](#^787ec5).
	- and that's how `a % b` is well defined for negative values.
	- and even for `floats`!
	- this explains the "weird" (aka non-intuitive) behavior for negative numbers
		- `12 % 5 = 2` *this is intuitive*
		- *these are non-intuitive* (because they involve negative numbers)
			- `12 % -5 = -3`
			- `-12 % 5 = 3`
			- `-12 % -5 = -2`
	- as well as how it works for real numbers `12.5 % 3 = 0.5`
		- Let's calculate it:
			- `12.5 // 3 = 4.0`
			- `a % b = a - b (a // b)`
			- `12.5 & 3 = 12.5 - 3 (4.0) = 12.5 - 12.0 = 0.5`
	- moral: be careful using "intuition" for `%` and `//` and negative values.

### Comparison Operators
- also known as *relational* operators
- compares two things and yields a Boolean `bool` result
- `==` equality comparison
- `!=` for "not equal"
- `<, <=, >, >=` assumes the operands are comparable
	- `10.5 < 100` makes sense
	- `hello > 100` doesn't really make sense.
- `==` between operands that are not comparable returns `False`
- `<, <=,...` etc. between non-comparable operands usually generates an `Exception`
	- `TypeError` (we'll come back to exceptions later.)
- `int` and `float` types are comparable to each other
	- `10 <= 10.9` → `True`
- `float` is a different story!
	- `0.1 + 0.1 + 0.1 == 0.3` → `False`
	- Why? Because of [finite representation](#Floats%20are%20not%20always%20exact).

> In general, never use `==` to compare floats.

#### Identity vs Value Equality of Objects
- To see if two objects are the *same object* → `is`
- To see if two (compatible) objects are *equal in value* (in some sense) → `==`
- The `is` operator is purely concerned with the memory address (*identity*) of objects
	- `is` is called the *identity comparison* operator
- The `==` operator, is, like `+`, actually implemented by the type itself.
	- recall: `a + b` actually executes `a.__add__(b)`
	- `==` works the same way, using the `__eq__` method
	- `a == b` → `a.__eq__(b)`
- So we can define what `==` means for custom types, by implementing `__eq__`

#### `in` and `not in` Comparison Operators
- these are known as membership operators: `in` and `not in`
- works with collection types
	- determines membership in some collection
		- `s = {1, 2, 3.14, True, 5.1}` (like a mathematical set)
		- `1 in s` → `True`
		- `10 in s` → `False`
		- `10 not in s` → `True`

 > `id()` is used to display the memory address or ID of an object in memory.

### Boolean Operators
- in Boolean algebra we only have two values: `True` and `False`
- and three basic operators: `and`, `or`, `not`
- `not` is a unary operator
- `and`, `or` are binary operators

#### The `not` Operator
- `not` simply reverses the Boolean value
![truth table](assets/Pasted%20image%2020241230155004.png)

#### The `and` Operator
- `a and b` is `True` if and only if both `a` and `b` are `True`
	- `False` otherwise.
- if `a` is `False`, then `a and b` is always `False`, no matter what `b` is.
![truth table for `and` operator](assets/Pasted%20image%2020241230160410.png)

#### The `or` Operator
- `a and b` is `False` if and only if both `a` and `b` are `False`
	- `True` otherwise
![truth table for `or` operator](assets/Pasted%20image%2020241230160548.png)
- If `a` is `True`, then `a or b` is always `True`, no matter what `b` is.

> **Short-Circuited Evaluation** occurs in Python when a the left operand expression in an `and` Boolean operator results to a `False` value, causing Python to skip running the rest of the program and return the result.
> 
> For example:
```python
sin(a) > 0 and cos(a) < 0
# if the left operand returns `False`, Python would not calculate the right operand to get an answer → 'Short-Circuited Evaluation'
```

#### Example of Short-Circuiting Usefulness
- suppose we have some trading algorithm that can calculate some buy signal (True/False)
	- the catch is that the calculation is complex and resource-intensive
	- in addition, we only want to place an order if the exchange is open
- we could write some code to do this:
```python
if calc_signal(symbol) and exchange_open(symbol):
	buy(symbol)
```
- the problem: when exchange is closed we needlessly calculate the signal
- but because of short-circuiting we can write:
```python
if exchange_open(symbol) and calc_signal(symbol):
	buy(symbol)
```
- this way if exchange is closed, we don't even calculate the signal.

## Conditional Execution
### `if`...`else`...
#### The `if` statement
```python
if <expression evaluates to True: # note the colon ':'
	code line 1 # notice how this code block is indented
	code line 2 # this tells Python that all these lines should
	...         # be executed if the condition is True
```
- you "exit" a code block by unindenting your code
- Python uses code indentation to group together chunks of code
	- called *code blocks*
- if you are familiar with other languages such as Java, JavaScript or C/C++, this is equivalent to using braces `{ }`.
#### The `else` Clause
- Python's `if` statement supports an `else` clause (*it is optional*)
```python
if <expression is True>:
	[Code Block 1]
else:
	[Code Block 2]
```
#### Nested `if` Statements
- sometimes we need to nest conditional logic, either in the `if` block or in the `else` block.
```python
if price < 1000:
	if price < 500:
		volume = 50
	else:
		volume = 10
	make_purchase(volume)
else:
	print('Too pricey!')
```
- can nest to any number of levels 
- too much nesting can make code very hard to read!
- keep nesting to a minimum.
### `elif`...
We've seen multi-level `if` statements before and how hard they are to read for humans (*especially because of their much use of nesting*). `elif` helps us solve that.
#### The `elif` Clause
Instead of an `if` nested structure, Python provides an `elif` clause
- equivalent to a nested `else-if`
- does not require this double indentation
- easier to read!
```python
if grade >= 90:
	grade_letter = 'A'
elif grade >= 80:
	grade_letter = 'B'
else:
	grade_letter = 'F'
```

> As you know with Java, C++, C#, or JavaScript, the `elif` statement resembles a `switch` statement. Python does not have a `switch` statement - `elif` is as close as you're going to get.

## Ternary Conditional Operator
[[|Remember where we learned about operators]], there, we learned that operators that work on three operands is a Ternary Operator. 

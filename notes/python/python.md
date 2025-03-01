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

![venv indication in python](../assets/Pasted%20image%2020241224235005.png)

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

![python repl](../assets/Pasted%20image%2020241225112258.png)
### Python Script mode.
This is different from Python REPL mode. Here, in Script mode, you write all your code first then call Python and it runs your code and outputs it to the system.

![python script mode](../assets/Pasted%20image%2020241225112435.png)
### Jupyter Notebooks
Jupyter Notebooks is software that has been developed on top of Python that wraps the REPL into a nicer interface. This is not the only one software available that does this. There are others are well.  
Jupyter Notebooks is a REPL, but browser-based.

![jupyter notebook](../assets/Pasted%20image%2020241225115903.png)

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
- An operator that works on three operands is called a **ternary operator** ^a59e93

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
	![floor in the integer division operator](../assets/Pasted%20image%2020241229155420.png)
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
![truth table](../assets/Pasted%20image%2020241230155004.png)

#### The `and` Operator
- `a and b` is `True` if and only if both `a` and `b` are `True`
	- `False` otherwise.
- if `a` is `False`, then `a and b` is always `False`, no matter what `b` is.
![truth table for `and` operator](../assets/Pasted%20image%2020241230160410.png)

#### The `or` Operator
- `a and b` is `False` if and only if both `a` and `b` are `False`
	- `True` otherwise
![truth table for `or` operator](../assets/Pasted%20image%2020241230160548.png)
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

### Ternary Conditional Operator
[Remember where we learned about operators](#^a59e93), there, we learned that operators that work on three operands is a Ternary Operator. 
The conditional ternary operator changes this:
```python
if <conditional exp>:
	var = value1
else:
	var = value2
```
to this:
```python
value1 if <conditional exp> else value2
```
- this is a *single* ternary operator
- if condition is `True`, it returns `value1`
- if condition is `False`, it returns `value2`
- instead of values, the return can also be an expression (the result of the expression is then used). Like so:
```python
<exp1> if <condition> else <exp2>

# something like this below ...
var = (a - b) if a > b else (b - a)
```

> Just like we saw with Boolean operators, the ternary operator also uses short-circuit evaluation. See how below:
```python
<exp1> if <condition> else <exp2>
```
- first evaluates `<condition>`
- if it is `True`, evaluates and returns `<exp1>`
	- but does not evaluate `<exp2>`
- if it is `False`, evaluates and returns `<exp2>`
	- but does not evaluate `<exp1>`

> **On code readability:** When you start writing a chunk of code, your first focus should be on correctness (no bugs) and readability. Maybe the code could be written more concisely, but sacrificing readability, or maybe it could be written more efficiently, but again at the cost of readability. It could... But don't fall into that trap - write your code for clarity and correctness **first**. Then later, once its working and you determine that that piece of code is a bottleneck to your overall program, then, and only then, go back and optimize it.
> 
> And readability is not for the benefit of the Python compiler - it's for us, humans, who have to read the code!

> The ternary operator is also very useful to deal with `null` values.

> The ternary operator has a lower precedence, so we need to use `( )` parenthesis to make our code behave the way we want. See what I mean in the code below:

```python
current_value = -999
running_total = 15_000
running_cost = 125

running_total = running_total + (0 if current_value == -999 else current_value)
# notice the use of parenthesis `( )` to encapsulate the ternary operator, telling Python to return the ternary operator first before adding it to the `running_total`

print(running_total)
```

## Sequence Types
#### What are Sequences?
- sequences are ordered collections of objects
	- there is a *first* element
	- there is a *second* element
	- and the *next* one.
	- sometimes called the *sequential order*
- we can *index* those elements using integers
	- like counting them one by one
- but in Python (and most programming languages)
	- **numbering starts at** `0`
- like anything in Python, **sequences are objects**
	- they just happen to be *container type objects* that *contain* other objects.
#### Indexing Sequences
- *first* element refers to the element at index `0`
- *second* element refers to the element at index `1`
- *last* element refers to the element at index `n-1` (assuming `n` elements in sequence)
#### Homogenous vs Heterogeneous Sequences
- certain sequence types *can only* contain objects that are *all the same type*
	- *homogeneous* sequence types
- other types of sequences *may* contain objects that are of different type
For instance:
- lists → mutable heterogeneous sequence type 
- tuples → immutable heterogeneous sequence type 
- strings → immutable homogeneous sequence type 

### Lists
- it is a *container* type
	- it contains elements
- it is a *sequence* type
	- elements are *ordered* sequentially
	- elements are *indexed*
- lists can be *heterogeneous*
- lists are *mutable*
	- can add, replace or remove elements
- lists have *unbounded* growth
	- can add as many elements as we want
	- but they are still *finite*
- lists are *objects*
	- they have state
		- the elements contained in the list
	- they have functionality
		- add element, remove element, etc.

#### Sequence Length
```python
l = [10, 20, 30, 40, 50]
```
- visual inspection
	- length of `l` is `5`
- but we can use `code` to calculate this for us
- the `len` function
	- `len(l)` → `5`
	- `len([True, False])` → `2`

#### Empty Lists
- sometimes we want to start with an empty list and have code that adds to the list as our program runs
- to create an empty list we can just use a *literal*
	- `l = []`
	- then `len(l)` → `0`
	- `l[0]`

### Tuple
#### The `tuple` Type
- very similar to the `list` type
	- it is a *container* type
	- it is a *sequence* type
	- tuples can be *heterogenous*
	- BUT... they are an *immutable* container type
	- unlike lists, once a tuple has been created
		- cannot add or remove elements
		- cannot replace elements

#### `tuple` Literals
- Python tuples can be created using *literals*
	- `(10, 20, 30, 40)`
	- note the enclosing *round brackets* `( )`
	- this indicates the collection is a tuple
- just like lists, they can contain *any* object, including another tuple. See below:
```python
(10, 20, (3, 4))
(10, 20, (True, False), [100, 200])
```
- often we don't even need the `( )`
- Python interprets a *comma* separated list of elements as a tuple
	- so we can write `(10, 20, 30)`
	- or just `10, 20, 30`
- both these code snippets result in `t` being a tuple:
```python
t = (10, 20, 30)
t = 10, 20, 30
```
- just like lists, tuples can contain any object
	- including other tuples or lists

![tuples](../assets/Pasted%20image%2020241231211917.png)

- we can omit the parenthesis on the outer tuple
```python
1, [True, False], (3, 4)
```
- but not `(3, 4)`
```python
1, [True, False], 3, 4 # not the same
```

#### *tuples* are Immutable
- unlike lists, we *cannot replace* an element of a tuple
- the *container* is immutable
	- does not mean *elements* in the container are immutable.

#### Creating Empty *tuples*
- not very useful, so not used very often
- use empty parenthesis
```python
t = ()
```
- that tuple is immutable, so it will remain empty for its lifetime

> Tuples are handy. They are often used to return multiple values from a function.

>An, interesting thing to note about the `tuple()` function is that we can pass it any sequence, and it will take each element of that sequence and create a tuple containing the same elements. We can also use the `list()` function the same way.
>
>We could be faced with a tuple that we want to mutate. We can't mutate the tuple - but we can easily transform it into a list, and then mutate the list. And if we really want to, we could create a tuple from a list and re-assign it to the symbol `t`.

>When we use these `list()` and `tuple()` functions it is very important to understand that although a new tuple or list object is created, the references inside those sequences are shared. We'll come back to this topic when we look at copying sequences.

### Strings
#### The `str` Type
- this is a *container* type
- it is a *sequence* type
- string are *homogeneous* → they can only contain characters (Unicode)
- they are *immutable*

#### `str` Literals
- Python strings can be created using *literals*
	- `'this is a string'`
	- note the enclosing quotes `'...'`
- can also use double quotes
	- `"this is a string"`
	- note the enclosing double quotes `"..."`
- these quotes/double quotes are called the string *delimiters*
- an empty string literal can be `' '` or `" "`

#### Indexing, Length
- works the same way as any sequence type
	- use an *index* number to access elements of the string
	- use the `len()` function to find the length of the string
	- `s = 'Python'`
	- `len(s)` → `6`
	- `s[0]` → `'P'` (a string containing a single character)
	- `s[1]` → `'y'`
	- `s[6]` → `IndexError`

> Just like `list()` and `tuple()` functions, the `str()` function can take any sequence type and make a string out of it.
> 
> In much the same way, a string can be converted to a list or tuple using the `list()` and `tuple()` functions. This can be handy if we want to create a list of items (see below):
```python
l = list('abcdef')
```

> String also support being multiplied by an integer (see below):
> 
> There is one very important caveat to this: the repeated element is the same element repeated `n` times.  **To resolve this**, we can use libraries such as `numpy` to easily create initialized lists and matrices without these pitfalls.
```python
s = '=' * 10
```

### Slicing
- *slicing* is a way to extract ranges of elements from a sequence
```python
[start:stop]
```
- *start* position (by index number)
- *stop* position (by index number)
- start index is *inclusive* of the element
- stop index is *exclusive* of the element
- slices are the *same type* of the sequences being sliced
- `str` is a sequence type (slicing for strings works the same way)

#### Including Last Element in Slice
- how do we specify including the last element?
- it's ok to specify indexes outside the sequence bounds!
	- Python will automatically figure it out
	- `s[6:12]` → `'Newton'`
	- `s[6:1000]` → `'Newton'`
- we can also leave the end index *blank*
	- Python will interpret as "*up to and including the last element*"
	- `s[6:]`

#### Including First Element in Slice
- just specify `0` as the start
- can also leave the start index *blank*
	- `s[:5]` → `'Isaac'`
- this is actually valid
	- `s[:]` → `'Isaac Newton'`
	- this made a *shallow copy* of the sequence
	- we'll come back to that soon.

#### Slicing in Steps
- a step is a way to specify an interval when slicing a sequence
	- `s[start:stop:step]`
	- `[2:10:2]`
		- start at (and include) index `2`
		- end at (but exclude) index `10`
		- move in steps of `2`

#### Negative Steps
- possible to use *negative* step values
	- starts at index `start` (inclusive)
	- stops at index `end` (exclusive)
	- moves *backwards* → so `start` should be greater than `end`

![negative steps - slicing](../assets/Pasted%20image%2020250102212933.png)

To recap:
![slicing summary](../assets/Pasted%20image%2020250102213236.png)

### Manipulating Sequences
- mutable sequences can be modified
	- *replace* elements
	- *delete* elements
	- *add* elements
		- often *appended* (to the end)
		- can also specify where in the sequence to *insert*

#### Replacing Single Elements
Replace an element at index `i` by assigning a new element to that index
```python
l = [10, 20, 30]
l[1] = 'hello'
# l → [10, 'hello', 30]
```

#### Replacing an Entire Slice
- can also replace an entire slice
	- just assign a new collection to the slice
	- slice will be replaced with elements in RHS
	- `my_list = [1, 2, 3, 4, 5]`
	- ![replacing an entire slice in python](../assets/Pasted%20image%2020250103152242.png)
- Python uses the *elements* of the sequence in RHS when assigning to a *slice* (but not when assigning using a single index)

> You can also replace an entire slice in reverse order but it acts a bit weird when you do it. Therefore, minimize this application as much as possible. [Learn more](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150726?start=354#notes).

#### Deleting Elements
- can delete an element by *index*
	- ![delete element by index](../assets/Pasted%20image%2020250103152640.png)
- can delete an element by *slice*
	- ![delete element by slice](../assets/Pasted%20image%2020250103152709.png)

#### Appending Elements
- we can *append* one element
	- `my_list = [1, 2, 3]`
	- `my_list.append(4)`
	- `my_list` → `[1, 2, 3, 4]`
- to append multiple elements, we *extend* the sequence
	- `my_list = [1, 2, 3]`
	- ![append elements using extend](../assets/Pasted%20image%2020250103153028.png)

#### Inserting an Element
- instead of appending, we could *insert* at some index
	- use sparingly - this is much slower than appending or extending
- ![inserting an element into a sequence](../assets/Pasted%20image%2020250103160322.png)
- ![inserting an element into a sequence - 2](../assets/Pasted%20image%2020250103160412.png)
- element is inserted so its position is the *index* - remaining elements are shifted right

> When *inserting* or *appending* items it takes it as is not as a sequence.
> For example:
> using *append* `[1, 2, 3, 4].append('python')` → `[1, 2, 3, 4, 'python']`
> using *insert* `[1, 2, 3, 4].insert(0, 'abc')` → `['abc', 1, 2, 3, 4]` 

> To test how long a program runs in Python use:
^9d76ab
```python
from timeit import timeit # import the timeit() function
timeit('<add_code_to_run_here>', globals=globals(), number=100_000) # number attr is the number of times the code to run and test for on avg.
```

### Copying Sequences
#### Shallow vs Deep Copies
- two types of copies
	- *shallow* copies
		- new sequence is created (not same sequence object as original)
		- elements in new sequence *reference the same elements* as original
	- *deep* copies
		- new sequence is created (not same sequence object as original)
		- each element in ew sequence is a *deep copy* of the original
			- totally new and independent objects

#### Shallow Copy
- ![shallow copy](../assets/Pasted%20image%2020250103163026.png)
- `origial` and `shallow_copy` are *not* the same containers
- but the elements are referencing the *same* objects
- add/remove/replace element in one does not affect the other
	- ![shallow copy 2](../assets/Pasted%20image%2020250103163219.png)
- but mutating an element will affect both (since it is a shared reference)
	- ![shallow copy 3](../assets/Pasted%20image%2020250103163329.png)

#### Creating Shallow Copies
- use slicing to slice the entire sequence `my_list[:]`
- use the copy method `my_list.copy()`

#### Creating Deep Copies
- uses `deepcopy` function in the `copy` module
```python
from copy import deepcopy
my_list = [['a', 'b'], 2, 3]
my_copy = deepcopy(my_list)
```

> Again, just to recap, shallow copies apply to mutable sequence types only (for instance, you can shallow copy a tuple but it'll be useless because you cannot modify a tuple), but deep copies work for either.

### Unpacking Sequences
- consider the sequence
	- `data = (1, 2, 3)` → this is a `tuple` with three elements
- we want to assign those values `1`, `2` and `3` to some symbols `a`, `b` and `c` respectively
- could do it this way:
```python
a = data[0]
b = data[1]
c = data[2]
```
- but Python has a better way of doing this! It's called *Unpacking*
	- `a, b, c = (1, 2, 3)`
	- Since tuples don't actually need the parenthesis in this case, we can write:
	- `a, b, c = 1, 2, 3`

- this works with any sequence in general
	- `a, b = [10, 20]`
	- `a, b, c = 'XYZ'`
- beware!
	- the number of elements in sequence on RHS must match number of symbols on LHS
	- ![unpacking sequences](../assets/Pasted%20image%2020250103174402.png)

#### Swapping Two Variable Values
- this is a common problem
	- given two variables `a` and `b`, swap the value of `a` and `b`
	- Initial State:
		- `a = 10`
		- `b = 20`
	- End State:
		- `a = 20`
		- `b = 10`
- typical solutions uses a temporary variable
```python
temp = a
a = b
b = temp
# 3 lines of code and an unnecessary variable
```
- can use unpacking to our advantage here
- remember: in an assignment, the RHS expression is evaluated completely first then the assignment takes place
	- `a, b = b, a`
	- RHS is evaluated first `b, a` is the tuple `20, 10`
	- then the assignment is made `a, b = 20, 10`
	- values of `a` and `b` have been swapped!

> You can also evaluate expressions while unpacking sequences, like so:
```python
s = 'abcdef'
a, b, c = (1 + 1, s[::-1], 3.14)
# a = 2
# b = 'fedcba'
# c = 3.14
```

## Strings
- strings are sequence types 
	- but they are more *specialized* than generic sequences
		- they are *homogeneous*
		- every element is a *single character*

### Unicode
#### In the beginning...
...there was ASCII (American Standard Code for Information Interchange)
- addressed the problem of a *standard* for assigning
	- numeric codes
		- to characters
		- printable and non-printable
	- and encoding the value into binary (using sequences of 7 bits)
			- given a data stream filled with `0`'s and `1`'s
			- carve up in 7 bits and decode character
- fonts handle *displaying* the character
	- a bunch of pixels
- supported character set was limited
	- 128 characters 
		- 95 printable characters (a-z, A-Z, 0-9, * / etc.) 
		- 33 non-printable characters (control code, esc, newline, tab, etc.)
	- ![unicode chart](../assets/Pasted%20image%2020250104141322.png)
- attempts were made to extend the ASCII set
	- still far too limited
	- standard was poorly followed
- Unicode was developed 
	- focused on assigning a code to a character (*code point*)
	- does *not* specify how to encode the code points into a binary format
		- other standards for doing that appeared
			- UTF-8 (*very popular, default in Python*)
			- UTF-16
			- UTF-32 (*UTF* → *Unicode Transformation Format*)
	- `> 100_000` code points defined so far.

#### Code Points
- backward compatible with ASCII
	- ASCII character code for 'A' → 65 (decimal), 41 (hexadecimal)
	- Unicode code point for 'A' → 65 (decimal), 41 (hexadecimal)
		- decimal → base 10 (0-9)
		- hexadecimal → base 16 (0-9, A-F)

#### What are hexadecimals?
> To understand hexadecimal numbers and how to calculate them as well as binary and decimal, [click here](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150878?start=329#notes).

[See](https://www.compart.com/en/unicode/U+0041) the Unicode Character A for example.
![unicode character A](../assets/Pasted%20image%2020250104143511.png)
- `ord()` function
	- returns code point for single character (in decimal)
	- `ord('A')` → 65
- `hex()`
	- converts decimal to hex *string*
	- `hex(65)` → `'0x41'` (`0x` prefix indicates the number after that is in hex)

#### Other ways to specify the character in a string
- use *escape* codes
	- by name `\N{}`
	- by hex code
		- `\u03B1` or `\u03b1` for four hex digits (0-F)
			- `\u` is limited to 4 digits
		- `\U000003B1` for eight hex digits (0-F)
			- `\U` is followed by 8 digits

### Common String Methods
- Python has a ton of string methods. Check them out [here](https://docs.python.org/3/library/stdtypes.html#string-methods).
- categories to look at:
	- case conversions
	- stripping start and end characters
	- concatenating strings
	- splitting and joining strings
	- finding substrings
- methods, called using dot notation `my_string.method()`
- remember that strings are *immutable*
	- operations never modify a string
	- just return a *new string*

#### Case Mappings
```python
lower() # 'Hello World'.lower() → 'hello world'
upper() # 'python'.upper() → 'PYTHON'
title() # 'one two three'.title() → 'One Two Three'
```
- returns a *new string*
	- primarily for visual display
		- BEWARE: may not work for caseless comparisons

#### Case Folding
```python
casefold() # used for caseless comparisons
s1 = 'hello'
s2 = 'HeLlo'
s1.casefold() == s2.casefold() # True
```

#### Stripping
- sometimes we want to remove leading and trailing characters
	- trailing commas
	- whitespace around a string
```python
.lstrip() # strips all whitespace on left of string
.rstrip() # strips all whitespace on right of string
.strip() # strips all whitespace on both ends of string
```
- can *specify* what characters to strip
```python
.strip(' ') # strip space characters from both ends
.lstrip('abc') # strip the characters 'a', 'b', 'c' from left end
```
- returns *new string*

#### Concatenation
- combining two or more strings to form a single string is called *concatenation*
```python
'Hello' + ' ' + 'World!' # 'Hello World!'
```
- again, this creates a *new* string

#### Splitting Strings
- useful for parsing data from a text file
	- `data = '100, 200, 300, 400'`
		- a string containing comma delimited values
- can easily split this on the comma
	- `data.split(',')`
- returns a `list` of strings
	- `['100', ' 200', ' 300', ' 400']`
		- note the spaces
		- we can *strip* them later

#### Joining Strings
- this is the opposite of splitting strings
```python
# suppose we want to join the strings below with ', ' characters between each:
# 'a' 'b' 'c' 'd'
# we could write:
'a' + ', ' + 'b' + ', ' + 'c' + ', ' + 'd'
```
- tedious to type out
- hardcoded
	- what if we have a *sequence* of strings we want to concatenate
	- this approach is not general enough
```python
data = ('ab', 'cd', 'ef') # data is a *sequence* of strings
'--'.join(data) # joins the strings in data with `--` in between
				# 'ab--cd--ef'
','.join(['10', '20', '30']) # '10,20,30'
```
- remember that a string is a *sequence* of single characters
```python
'='.join('python') # 'p=y=t=h=o=n'
```

#### Finding Substrings
- you often want to know if a sequence of characters is contained inside another
- use the `in` operator
```python
'x' in 'xyz' # True
'a' in 'xyz' # False
'pyt' in 'python' # True
'pyt' in 'Python' # False (python is case-sensitive)
```
- tests containment
	- but gives no indication of where the substring is
- slight variation
	- does string start (or end) with the specified characters
	- still a containment test
	- `.startswith('...')`.
	- `.endswith('...')`.
```python
'python'.startswith('py') # True
'python'.startswith('hon') # False
'python'.endswith('py') # False
'python'.endswith('hon') # True
```

#### Finding the Index of a Substring
- used when we need to know the *index* of the start position of a substring
	- `data = 'This is a grammatically correct sentence.'`
	- at what index does the string 'correct' occur?
		- `data.index('correct')` → `24`
	- what if substring is *not found*?
		- Python raises a `ValueError`
		- potentially useful once we learn how to handle exceptions
	- what if we don't want an exception?
		- `find` → returns `-1` if substring is not found
```python
data = 'This is a grammatically correct sentence.'
data.find('correct') # 24
data.find('DOW') # -1
```
- once we know how to handle exceptions, this method is a bit redundant
	- instead, use `index` with exception handling

> **Important Note:**
> - only interested in *whether or not* a substring is contained in another string
> 	- use `in`
> - only use `index` or `find` when you *need* to know the *index*
> 	- `in` is much faster!
> - the `in` operator works for all sequence types, but `find` only works for `str`

> A little bit more work on `index` string method:
```python
message = 'To every action there is an equal and opposite reaction.'
# if I want to find the index of action in the string
message.index('action') # 9
# if I want to find the index of the other representation of 'action' in the string - re(action)
message.index('action', 9 + len('action')) # 58
```

### String Interpolation
- often we want to *build* strings that contain values from some variable
- can use concatenation
	- `+` works with two strings
	- cannot mix string and numeric for example
		- `'test' + 100 # TypeError`
		- `'test' + str(100) # 'test100'`

> `open` is a keyword in Python. Therefore, never use `open` as a variable name in Python. Instead use `open_`

- multiple variants
	- here are two most common techniques
		- the `format` method
		- using `f` Strings

#### The `format` Method
- use `{}` as placeholders in our string
- pass variables to `format` method in the same order we want them in the string
- number of `{}` in string and arguments in `format` should match
	- `format` can have more arguments, they'll just be ignored
	- `IndexError` exception if not enough arguments
- note how we didn't have to convert the values to strings (Python did that for us)

#### f Strings
- new to Python 3.6
- prefix the string with `f`
- use `{expr}` directly inside the string
- Python evaluates `expr` and interpolates the result directly inside the string
```python
f'1 + 1 = {1 + 1}' # '1 + 1 = 2'
value - 3.14
f'pi is approximately {value}' # 'pi is approximately 3.14'
```

## Iteration
- fundamental aspect of writing code is *repetition*
	- want to repeat the same process (code) multiple times
- how many times?
	- *known in advance*
		- load files with 10,000 rows
		- process each row
			- repeat the same process 10,000 times
		- *deterministic*
	- *not known in advance*
		- get commodity tick data
			- analyze data until ask price falls below some level
				- then do something else and stop processing
		- process may repeat 10 times, or 100 times, we don't know in advance
		- *non-deterministic*
- this repetition is called *iteration*

#### Deterministic Iteration
- we iterate over the elements of some container
	- e.g. sequences
	- more generally over objects that are *iterable*
	- not all iterables are sequences 
		- a bag of marbles is iterable, but it's not a sequence!
	- `for` loop

#### Non-Deterministic Iteration
- we iterate while some condition is `True`
- `while` loop

### `range`
#### The `range` Object
- `range` object is an *iterable* object
	- it serves up integers one by one as they are requested
	- but the full list of integers does not exist all at once in memory
	- memory efficient
	- it has a *finite* number of integers
- we can *iterate* over that range object
	- since it exists and has a finite number of integers
		- *deterministic* iteration
- we can use the `range()` function to create `range` *objects*
#### The `range()` Function
- three flavors depending on how many arguments are specified
	- `range(end)` *one argument*
		- generates integers from `0` (*inclusive*) to `end` (*exclusive*)
	- `range(start, end)` *two arguments*
		- generates integers from `start` (*inclusive*) to `end` (*exclusive*)
	- `range(start, end, step)` *three arguments*
		- generates integers from `start` (*inclusive*) to `end` (*exclusive*)
			- in steps of `step`
- should remind you of [slicing](#Slicing)
#### Viewing Contents of `range` Object
```python
r = range(5)
print(r) # 'range(5)'
		 # not what we wanted
```
- we can *convert* `range` object to a `list` or `tuple`
```python
print(tuple(r)) # (0, 1, 2, 3, 4)
print(list(r)) # [0, 1, 2, 3, 4]
```

> **To recap:**
> - `range` objects is *iterable*
> - we can use a `for` loop to iterate over this iterable

### `for` Loops
- `for` loops are used to *iterate* over elements of any *iterable*
- the loop mechanism retrieves elements from the iterable *one at a time*
- the *body* of the `for` loop is *executed* for each element retrieved
- the loop terminates when all elements have been iterated

This is an example of a `for` loop:
```python
for x in ['a', 'b']:
	y = x + x
	print(y)
print('done')
```

#### Iterating over `range` Objects
- `range` objects are iterable
```python
for i in range(4): # range(4) → 0, 1, 2, 3
	sq = i * i
	print(sq) # output: 0
			  #         1
			  #         4
			  #         9
```

#### Loop Bodies (Blocks)
- blocks can contain any valid Python code
	- `if...else`
	- another loop (nested loop)
```python
for i in range(1, 4):
	for j in range (1, i+1):
		print(i, j, i*j)
	print('')
# output: 1 1 1 ← i = 1; j in range(1, 1+1)

#         2 1 2 ← i = 2; j in range(1, 2+1)
#         2 2 4

#         3 1 3 ← i = 3; j in range(1, 3+1)
#         3 2 6
#         3 3 9
```

> Using a `for` loop to both iterate over and remove/insert elements of a sequence at the same time is, in general, a bad idea. [See this](#^5ba42a) to learn more.

#### The `enumerate` Function
`enumerate` is a function that:
- takes an iterable argument
- returns a new iterable whose elements are a `tuple` consisting of:
	- the *index* number of the original element
	- the *original element* itself
```python
data = [10, 20, 30, -10, 40, -5]
for t in enumerate(data):
	print(t) # output: (0, 10)
			 #         (1, 20)
			 #         (2, 30)
			 #         (3, -10)
			 #         (4, 40)
			 #         (5, -5)
```
- at each iteration `t` is a tuple `(index, element)`
- it can be unpacked!

> We can unpack values into variables in the `for` clause itself. Like so:
```python
data = [10, 20, 30, -10, 40, -5]
for index, element in enumerate(data):
	if element < 0:
		data[index] = 0
# output: data → [10, 20, 30, 0, 40, 0]
```

### `while` Loops
- different than `for`
- here we want to repeat some code as long as some condition is `True`
	- non-deterministic
		- we don't necessarily know when condition becomes `True`
		- maybe never!
		- infinite loop

This is an example of a `while` loop:
```python
while expr:
	<code block>
```
- `expr` is evaluated at the *start* of each iteration
- if it is `True`, execute `<code block>`
- if it is `False`, terminate loop immediately
- may never execute (if `expr` is `False` on first iteration)
- may never terminate (if `expr` never becomes `False`)

Example:
```python
value = 10

while value < 15:
	print(value)
	value = value + 1 # increments `value` by 1
# output: 10
#         11
#         12
#         13
#         14
```

> `while` loops are useful for modifying iterables as we are iterating over them.
> 
> For example, suppose we have a list of data that we have to process and remove from the list once we have processed it, repeating this until the list is empty.
^5ba42a

```python
data = [100, 200, 300, 400, 500]

while len(data) > 0:
    last_element = data.pop()  # retrieves and removes last list element
    print(f'processing element: {last_element}')
# output: processing element: 500
#         processing element: 400
#         processing element: 300
#         processing element: 200
#         processing element: 100
```

> On the other hand, trying to the do the same using a `for` loop is not recommended!
> 
> Using a `for` loop to both iterate over and remove/insert elements of a sequence at the same time is, in general, a bad idea.

### `continue`, `break` and `else`
- sometimes we want to skip an iteration, but without terminating the loop
	- `continue`
	- immediately jumps to the next iteration
```python
my_list = [1, 2, 3, 100, 4, 5]

for i in my_list:
	if i > 50:
		continue
	print(i)
print('done')
# output: 1
#         2
#         3
#         4
#         5
#         'done'
```
- when `i` is 100
- `continue` is executed
- loop jumps to next iteration
---
- `continue` is not used very often
	- can sometimes make code difficult to read and understand
```python
for i in my_list:
	if i > 50:
		continue
	print(i)
print('done')

# equivalently...

for i in my_list:
	if i <= 50:
		print(i)
print('done')
```
- less code, easier to read / understand.

#### Early Termination
- loops can be exited early (before all elements have been iterated)
	- `break`
```python
my_list = [1, 2, 3, 100, 4, 5]

for i in my_list:
	if i > 50:
		break
	print(i)
print('done')
# output: 1
#         2
#         3
#         'done'
```
- when `i` is 100
- `break` is executed
- loop is terminated immediately
---
- loop terminating early using `break`
	- sometimes called *abnormal* or *early* termination
- sometimes you may want to execute some code if loop terminated normally
	- and a different code otherwise (early/abnormal termination)

> **For example:**
> Let's say we're scanning through an iterable, looking for an element equal to: `'Python'`
> 
> If we find the value, we want to terminate our scan immediately and print `'found'`, otherwise we want to print `'not found'`
^0d0684

```python
found = False

for el in my_list:
	if el == 'Python'
		found = True
		print('found')
		break
if not found:
	print('not found')
```

#### The `else` Clause
- Python is really confusing here...
- `for` loops can have an `else` clause
	- but it has nothing to do with the `else` clause of an `if` statement
- the `else` clause of a `for` loop executes *if and only if* no `break` was encountered
	- *in your mind: read it as, "else if no break"*
```python
for i in range(5):
	<code block 1>
else: # if no break
	<code block 2>
```
- `<code block 2>` executes if loop terminated normally

Therefore, back to our [example](#^0d0684):
```python
found = False

for el in my_list:
	if el == 'Python'
		found = True
		print('found')
		break
if not found:
	print('not found')

# equivalently...

for el in my_list:
	if el == 'Python':
		print('found')
		break
else: # if no break
	print('not found')
```

## Dictionaries
Dictionaries are one of the most important data structures in Python
- we don't always see them
	- but they're lurking in the shadows! 😎
- we saw that variables are symbols pointing to objects
	- some string (variable name) is *associated* with some object
- objects are also dictionaries
	- properties are symbols *associated* to some value (object)
	- methods are names *associated* to some function
		- `s.upper()`
		- `l.append()`
- associating two things together is extremely useful
	- a phone book
		- associates a number to a name
	- DNS
		- associates a URL with a numeric IP address
	- book index
		- associates a chunk of text with a page number
- associative arrays
	- sometimes called a *map*
	- *abstract* concept
	- can be implemented in *different* ways
	- Dictionaries (or hash maps) are one *concrete* implementation

### Associative Arrays and Dictionaries
> [Learn](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150934?start=121#notes) one of the old ways of associating keys to values in Python, and code in general.
> [Learn](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150934?start=229#notes) another approach that uses storing data in separate lists (lists of tuples).
- both approaches have major drawbacks
	- must scan an array until we find the correct element
	- the longer the array, the longer time this will take (worst case is last element)

#### Hash Maps (aka Dictionaries)
- better implementation is the *hash map* (or *dictionary*)
	- similar to the last approach we saw
	- but a special mechanism is used to quickly find a key 
		- lookup *speed is not affected by size* of dictionary

**IMPORTANT THINGS TO NOTE**
- keys must be *hashable* (hence the terms 'hash map')
	- `strings` are hashable
	- `numerics` are hashable
	- `tuples` may be hashable (if all the elements are themselves hashable)
	- `lists` are *not* hashable (in general, *mutable* objects are *not* hashable)

> To find out if an element is hashable use the `hash()` function in Python. Use it by passing the element as an argument in the function. It should return a string of numbers.

#### Python Dictionaries
- a dictionary is a data structure that associates a `value` to a `key`
	- both `value` and `key` are Python objects
	- `key` must be *hashable* type (e.g. `str`, `int`, `bool`, `float`, ...) and unique
	- `value` can be *any type*
	- type is `dict`
	- it is a collection of `key: value` pairs
	- it is *iterable*
		- but it is not a sequence type
		- `values` are looked up by `key`, not by `index`
		- technically, there is no ordering in a dictionary (we'll come back to this point)
	- it's a *mutable* collection

#### Looking up values in a Dictionary
- use `[]` just like for sequence types
	- but instead of an index values we specify the *key*
```python
d = {
	 'a': 97,
	 'b': 98,
	 'A': 65,
	 'B': 66,
	 'z': 122,
	 'Z': 90
}

d['a'] # 97
d['Z'] # 90
```

#### Replacing the Value of an existing Key
```python
d = {
	 'symbol': 'AAPL',
	 'date': '2020-03-10',
	 'close': 285
}

# to change the values associated with 'close':
d['close'] = 285.34

# `d` (dictionary) now looks like this:
"""
d = {
	'symbol': 'AAPL',
	'date': '2020-30-10',
	'close': 285.34
}
"""
```

#### Adding a New `key:value` Pair
- simply assign a value to a new key
	- if key exists, it will be *updated* as we just saw
	- if key does not exist, a new entry is *inserted* with key and value
		- (and this explains why keys in a dictionary are necessarily unique!)

#### Deleting a `key:value` Pair
- we can remove `key:value` pairs from a dict
	- use the `del` keyword
```python
# to delete a key:value pair in an object or dictionary in Python:
del d['open']
```

#### Common Exceptions
- certain operations on dictionaries can lead to `KeyError` exceptions
	- trying to read a non-existent key
	- trying to delete a non-existent key
- trying to use a *non-hashable* object as a `key` leads to a `TypeError` exception
```python
d[[10, 20]] = 100
# this leads to a `TypeError` exception
```
- `TypeError: unhashable type: 'list'`
	- `[10, 20]` is a `list`, and lists are not hashable
	- *cannot* be used as a key

### Iterating Dictionaries
#### Dictionaries are Iterable
- we can use the `for` loop to iterate over Dictionaries
	- `keys`
	- `values`
	- `key:value` pairs
- default iteration is over the dictionary `keys`
```python
data = {'a': 1, 'b': 2, 'c': 3}

for k in data:
	print(k) # output: a
	#                  b
	#                  c
```

#### Iterating over `values`
- dictionaries have a method called `values()`
	- `values()` returns an *iterable* containing just the values of the dictionary
```python
data = {'a': 1, 'b': 2, 'c': 3}

for v in data.values():
	print(v) # output: 1
	#                  2
	#                  3
```

#### Iterating over `key:value` Pairs
- dictionaries have a method called `items()`
	- `items()` returns an *iterable* containing the `keys` and `values` in a *tuple*
```python
data = {'a': 1, 'b': 2, 'c': 3}

for t in data.items():
	print(t) # output: ('a', 1)
	#                  ('b', 2)
	#                  ('c', 3)

# using unpacking
for k, v in data.items():
	print(f'{k} = {v}') # output: a = 1
	#                             b = 2
	#                             c = 3
```

#### The `keys()` Method
Technically, there is also a `keys()` method
- behaves like `values()` or `items()`
- but it is iterable over the `keys` of the dictionary
```python
data = {'a': 1, 'b': 2, 'c': 3}

for k in data.keys():
	print(k) # output: a
	#                  b
	#                  c
```
- but default iterations is over `keys` anyway
	- so `keys()` is not particularly useful for iteration

#### Insertion Order
- we saw in sequence types that elements have positional order
	- not every iterable has positional order
		- we can pull marbles out of the bag, but there is no particular order
	- for a long time Python dictionaries were the same
		- a "bag" of `key:value` pairs that could be looked up by key
		- iteration order was not guaranteed to be anything specific
- this changed in **Python 3.6**
	- the *iteration order* reflects the *insertion order*.

> What do we mean by 'insertion order'?
> Insertion Order is the order in which the `key:value` pairs are listed out. Adding a new element, makes the new element the last element added in order. But we still cannot retrieve elements by index.
> 
> **NOTE:** Updating the value of an existing key does not change it's insertion order. However removing the key and reinserting it again does change the order.

### Working with Dictionaries
#### Membership Testing
How do we test if a `key` exists in a `dict`? Using `in`
```python
d = {'a': 1, 'b': 2}
'a' in d # True
'x' in d # False
```
- `not in` can be used to test if a `key` is not present

#### Useful Methods and Functions
- `d.clear()`
	- removes all elements from `d`
- `d.copy()`
	- creates a *shallow copy* of `d`
		- same as sequences
		- use `copy.deepcopy()` to create a *deep copy*
- `len(d)`
	- returns number of elements in `d`

#### Other Methods to Create Dictionaries
- `d = {'a': 1, 'b': 2}`
- `d = dict(a = 1, b = 2)`
	- symbols must be valid variable names and will be used, in string form, as the keys
- can create a dictionary with several keys all initialized to the same value
```python
d = dict.fromkeys(['cnt_1', 'cnt_2', 'cnt_3'], 0)
d # {'cnt_1': 0, 'cnt_2': 0, 'cnt_3': 0}
```
- first argument of `fromkeys()` should be an *iterable* (list, tuple, string, etc)
```python
d = dict.fromkeys('abc', 100)
d # {'a': 100, 'b': 100, 'c': 100}
```

#### Creating Empty Dictionaries
- often we create dictionaries that start empty
	- and get mutated (modified) as our code runs
- can use a literal `d = {}`
- can use the `dict()` function

#### The `get()` Method
- trying to retrieve a non-existent key results in a `KeyError` exception
	- sometimes we want to have a "default" value if a key does not exist
		- could use `if` statements and test if key exists using `in`
		- could try to retrieve the key and *handle* the exception
		- or, use the `get()` method
- `get()` can take two arguments
	- the `key` for which we want the corresponding `value`
	- the default `value` we want to use if the `key` does not exist
- `get()` can also take a single argument, the `key`
	- default value is `None` (special object to handle "nothing")
- data in Python is often handled using dictionaries
	- when we work with data we often have *missing values*
	- sometimes, not only is the value missing, but the *key* as well
- using `get()` allows us to simplify our code to assign a default for missing keys
```python
if 'ssn' in person_dict:
	social = person_dict['ssn']
else:
	social = ''

# using the get() method for dictionaries, we could also write:
social = person_dict.get('ssn', '')
# this will return '' and empty string if 'ssn' key is not available in 'person_dict' dictionary
```

#### Merging one Dictionary into Another
- the `update()` method
	- takes a single argument: another dictionary
```python
d1.update(d2) # the key:value pairs of 'd2' will be merged into 'd1'
```
- keys in `d2` *not in* `d1` will be *added* to `d1` (with the value)
- keys in `d2` that *are present* in `d1` will *overwrite* the value in `d1` with that of `d2`
- **Important:** `d1` is *mutated*

> Dictionaries are used extensively in Python.

## Sets
### What are Python Sets?
- just like a mathematical set
	- a *collection* of elements
	- *no ordering* to the elements
	- each element is *unique*
- it is an *iterable*
	- but *no guarantee* on what the *iteration order* will be
- think of it like a bag of marbles (a *collection* of marbles)
	- to iterate you reach in the bag and grab a marble (any marble)
	- continue doing so until the bag is empty
		- *no order* is guaranteed!
- just like mathematical sets, Python supports *operations*
	- union
	- intersection
	- difference
	- membership (is some object an element of a set or not)
	- containment (subset, strict subset, superset, strict superset)

### Python Sets
- type is `set`
- sets are *iterable*
- iteration *order* is *not guaranteed* (at least not yet)
- set *elements* must be *hashable*
- sets are *mutable*
	- **sets are not hashable**
		- a set cannot be an element of another set, or a key in a dictionary
		- if you really want nested sets, use `frozenset` (you can check this out in Python docs)
			- *immutable* equivalent of sets - those are **hashable** (if all elements are themselves hashable)

#### Defining Sets
- literal form
	- note the `{}` - just like for dictionaries
	- but no `key:value` pairs, just the `keys`
```python
{1, 'a', True}
```
- can also use the `set()` function
```python
set([1, 'a', True])
```
- *empty* set
	- cannot use `{}`
	- that would be a empty *dictionary*
	- `set()`
---
- use a `for` loop for iteration
- use `in` for membership testing
- `len(s)` returns the number of elements in the set
- `s.clear()` removes all the elements of the set
- `s.copy()` creates a *shallow copy*

### Common Set Operations
#### Disjointedness
- two sets are *disjoint* if they have no elements in common
- `s1.isdisjoint(s2)`
	- `True` if no common elements exist
	- `False` if one or more common elements exist
#### Adding or Removing Elements
```python
s = {10, 'b', True}

s.add(4) # {10, 'b', True, 4}
s.add('b') # {10, 'b', True, 4} no duplicates in a set

s.remove('b') # {10, True, 4}
s.remove(100) # **KeyError**

s.discard(4) # {10, True}
s.discard(100) # no exception {10, True}
```
#### Subsets and Supersets
```python
s1 < s2 # True if s1 is a strict subset of s2
s1 <= s2 # True if s1 is a subset of s2
s1 > s2 # True if s1 is a strict superset of s2
s1 >= s2 # True if s1 is a superset of s2
# Therefore...
{1, 2} < {1, 2, 3} # True - Strict Subset
{1, 2} < {1, 2} # False - Not Strict Subset

{1, 2} <= {1, 2, 3} # True - Subset
{1, 2} <= {1, 2} # True - Subset

{1, 2, 3} > {1, 2} # True - Strict Superset
{1, 2} > {1, 2} # False - Not Strict Superset

{1, 2, 3} >= {1, 2} # True - Superset
{1, 2} >= {1, 2} # True - Superset
```
 [See this response](https://www.perplexity.ai/search/i-may-have-missed-the-teaching-TzpsGBtPRrS8M7Q3hWBO3A) by PerplexityAI to refresh your memory on Mathematical Sets and the meaning of subsets & supersets.
#### Unions and Intersections
```python
s1 | s2 # returns the union of s1 & s2
s1 & s2 # returns the intersection of s1 & s2
# For example...
s1 = {1, 2, 3}
s2 = {3, 4, 5}
s1 | s2 # {1, 2, 3, 4, 5}
s1 & s2 # {3} : (again, set elements are unique)
```
#### Set Difference
the *difference* `s1 - s2` of two sets is all the elements of one set minus the elements of the other set.
![set difference](../assets/Pasted%20image%2020250110130939.png)

> **Caution**: set difference is *not commutative* `s1 - s2 != s2 - s1`
```python
s1 = {1, 2, 3}
s2 = {3, 4, 5}
s1 - s2 # {1, 2}
s2 - s1 # {4, 5}
```
---
- always keep sets in mind when coding
	- *membership* testing in sets is much faster than lists or tuples
	- easy to *eliminate duplicate values* from a collection
	- easy to find *common values* between two collections
	- easy to find values in one collection but *not in another*

> To write the entire alphabet string `'abcdefghijklmnopqrstuvwxyz'` using a Python string property, do the following:
```python
import string
string.ascii_lowercase # abcdefghijklmnopqrstuvwxyz
string.ascii_uppercase # ABCDEFGHIJKLMNOPQRSTUVWXYZ
string.ascii_letters # abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```

## Comprehensions
- comprehensions are an easy way to create new iterables from other iterables
	- like using a loop, but easier and more concise syntax
	- works well for simple cases
		- can quickly become unreadable!
		- readability matters!!
Example:
- given a list of 2D vectors
	- `[(0, 0), (1, 1), (1, 2), (3, 5)]`
- create a new list containing the magnitude of each vector
	- ![magnitude of each vector in comprehensions](../assets/Pasted%20image%2020250113112235.png)

### List Comprehensions
- a *comprehension* is a way to use one iterable to *create* another
	- more *concise* than using regular loops
		- use for *simple* computations
- different types of comprehensions
	- lists
	- dictionaries
	- sets
	- generators
- a list comprehension is used to generate a `list` object

The `list` *comprehension* syntax looks like this:
![list comprehension](../assets/Pasted%20image%2020250113112950.png)
```python
# In general, `list` comprehensions look like this:
[expression for item in iterable]
```
> Comprehensions are actually functions. We'll look at this later when we examine Function and Closures. Comprehensions build up and returns the calculated iterable.

Example:
From the list `numbers`, create a new list of even numbers.
- the standard approach:
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8]
evens = []
for number in numbers:
	if number % 2 == 0:
		evens.append(number)
```
- using `list` comprehensions:
	- comprehension syntax supports an `if` clause:
```python
evens = [number ** 2 for number in numbers if number % 2 == 0]
```
- in general:
```python
[expression1 for item in items if expression2]
# `if` acts like a filter
```

> `perf_counter` is something we can use to measure relative elapsed times. [Similar to](#^9d76ab) `timeit`. It is a measure of elapsed time from some point of origin - this origin point is undefined, but we can get the `perf_counter` value at different points in our code, and calculate the difference between the different measurements. As shown in the example below:

Comprehensions are much more efficient to use than `for` loops. See the example test below for comparison:
- `for` loop speed
```python
from time import perf_counter
start = perf_counter()
for i in range(100_000):
    magnitudes = []
    for vector in vectors:
        magnitude = sqrt(vector[0] ** 2 + vector[1] ** 2)
        magnitudes.append(magnitude)
end = perf_counter()

elapsed_time = end - start
print(elapsed_time) # 0.359776093
```
- comprehension speed
```python
from time import perf_counter
start = perf_counter()
for i in range(100_000):
    magnitudes = [sqrt(vector[0] + vector[1]) for vector in vectors]
end = perf_counter()

elapsed_time = end - start
print(elapsed_time) # 0.07221294499999997
```
- as you can see the comprehension approach is quite faster.

> In conclusion, `list` comprehensions are mechanisms we use to easily create a new list based on another iterable. But comprehension syntax can quickly devolve into hard to understand messes - keep it simple!

> While using list comprehensions, to iterate two levels deep into an iterable like the below: (do this!)
```python
l = [[1, 2, 3], [3, 4, 5], [5, 6, 7]]
new_l = [num for row in l for num in row]
print(new_l) # [1, 2, 3, 3, 4, 5, 5, 6, 7]

# what happened was, the first `for` loop (for row in l) in the comprehension
# should iterate over the outer loop, while the second `for` loop (for num in row) 
# should iterate over the inner loop

# In Python list comprehensions, the order of the `for` clauses should mirror how you would write nested `for` loops
```

### Dictionary and Set Comprehensions
- similar to list comprehensions
	- use `{}` instead of `[]`
		- remember literals for dictionaries and sets use `{}`
			- dictionary elements are *pairs* → `key:value`
			- set elements are *single* values
![dictionary comprehensions](../assets/Pasted%20image%2020250113120849.png)

#### Set Comprehensions
- similar to a dictionary comprehension
	- but elements are not `key:value` pairs
		- just the `key` portion
![set comprehensions](../assets/Pasted%20image%2020250113121631.png)

> The idea of creating a dictionary that contains all the unique keys from a collection and a count for the corresponding value is one way of expressing what's called a mathematical multiset. See how this is calculated below:
```python
data = ['a', 'a', 'a', 'b', 'b', 'c', 'c', 'c', 'd']
# given `data` we want to do the below:
# freq = {
	# 'a' = 3,
	# 'b' = 2,
	# 'c' = 3,
	# 'd' = 1,
# }

# we can achieve this one of two ways:

# 1. using regular `for` loop
freq = {}

for element in set(data):
	count = len([char for char in data if char == element])
	freq[element] = count

print(freq) # freq = {
				# 'a' = 3,
				# 'b' = 2,
				# 'c' = 3,
				# 'd' = 1,
			# }

#2. using set comprehensions
freq = {
		element: len([char for char in data if char == element])
		for element in set(data)
}
print(freq) # freq = {
				# 'a' = 3,
				# 'b' = 2,
				# 'c' = 3,
				# 'd' = 1,
			# }
```
> The function we just executed in the previous example above is performed by a module in built-in Python library known as `collections`. We can call the function like so: `from collections import Counter`. [Learn more](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150982?start=874#notes)

## Exceptions
#### What are exceptions?
- exceptions are special events that happen when something out of the ordinary happens while our code is running
- an exception is generally unexpected behavior 
	- but not always
	- it may be something we expect to happen from time to time
		- we can deal with it and continue running our code
- so an exception is not necessarily an error
- but unhandled exception will cause our program to terminate
#### Terminology
- *exception*: a special type of *object* in Python
- *raising*: starting an exception *event flow*
- *exception handling*: *interacting* with an exception flow in some manner
- *unhandled exception*: an exception flow that is *not handled* by our code
#### Exception Hierarchy
- python exceptions form a hierarchy (this will be covered precisely when looking at Object Oriented Programming - OOP). See → https://docs.python.org/3/library/exceptions.html#exception-hierarchy
- basically means that exceptions can be classes sub-divided into sub-exceptions that are more specific
- for example a broad exception might be `LookupError`
	- more specifically it could be an `IndexError` or a `KeyError`
	- both of these are categorized more broadly as a `LookupError`
		- can choose to handle `IndexError` specifically
		- or `LookupError` more broadly
- this means that if an exception object is an `IndexError` exception
	- it is also a `LookupError` exception
	- and it is also an `Exception` exception
		- most exceptions we work with are classified as `Exception` types
#### Python Built-in Exceptions
- python has many built-in exception types, which you can read here: http://docs.python.org/3/library/exceptions.html
- common exceptions include:
	- `SyntaxError`
	- `ZeroDivisionError`
	- `IndexError`
	- `KeyError`
	- `ValueError`
	- `TypeError`
	- `FileNotFoundError`
	- and many more...
#### EAFP vs LBYL
- when we think something unexpected may go wrong in our code
	- figure out if something it's going to go wrong before we do it
		- LBYL → Look Before You Leap
	- just do it, and handle the exception if it occurs
		- EAFP → Easier to Ask Forgiveness than Permission
- generally, in Python
	- follow EAFP
	- exception handling
#### Why EAFP?
Something that is exceptional should be *infrequent*
- if we are dividing two integers in a loop that repeats 1,000 times
	- out of every 1,000 times we run, we expect division by zero to occur 5 times
		- LBYL → test that divisor is non-zero 1,000 times
		- EAFP → just do it, and handle the division by zero error 5 times
			- often more efficient
- also trying to fully determine if something is *going* to go wrong is a *lot harder* to write than just handling things when they *do* go wrong.
#### Exception Handling Flow
- an exception occurs
	- an exception object is created
	- an exception flow is started
	- we do nothing about it
		- program terminates
	- we interrupt the exception flow
		- try to handle the exception in some sense, if possible
		- then
			- resume running program uninterrupted
			- or, let the exception resume
			- or, start a new exception flow
### Raising Exceptions
- often we want to start an exception flow ourselves
	- called *raising* an exception
- an exception object is associated with an exception flow
	- we create a new exception object
	- we raise the exception object
- doing this is most useful when we create functions
	- we'll see this later
	- for now, we'll just learn how to raise an exception

Example:
- create an exception object using one of Python's built-in exceptions
	- `ex = ValueError()`
	- usually we include a custom exception message
	- `ex = ValueError('Name must be at least 5 characters long.')`
- we raise the exception, starting an exception flow
	- `raise ex`
- often we do both in one step
	- `raise ValueError('Name must be at least 5 characters long.')`
- raising an exception ourselves results in the same exception flow that Python does when it raises some exception
	- we can choose to handle the exception
	- if we don't handle the exception, program terminates

> 3 functions in Python that display information on the exception object is `repr()`, `print()`, and `str()`.
> - `repr()` displays developer information of the object parsed. This is what Jupyter uses to display contents of variables and objects in the notebook. This is mostly used by developers.
> - `str()` displays the string message of the object. This can be used to share error messages to users.
> - `print()` does the same thing `str()` does.

> `input()` function takes in some input and returns it to a variable. Like so:
```python
name = input('Enter name (5 chars min): ')
# notice the space after the colon. It's used to space out the input box and the string repr of `input`
```

> `issubclass()` is a function that returns a Boolean value if the first argument is a subclass in the [exception hierarchy](#Exception%20Hierarchy) to the second argument.

> `type()` is a function that returns the kind of object the variable or object passed into it is.
### Handling Exceptions
#### General Suggestions for Exception Handling
- in general we do not want to just handle any exception anywhere in our code
	- too much work
	- cannot anticipate every point of failure
	- it's OK for program to terminate - we can figure out what went wrong and attempt to fix it later - possibly handling that case specifically
	- if we don't know exactly why or where the problem occurs in our code, there's not much we can do to recover from the exception
- we handle exceptions that are raised by *small chunks* of code
- we try to handle very *specific* exceptions, not broad ones 
	- usually handle exceptions that we can do something about
#### `try...except...`
- wrap the code we want to implement an exception handler for inside a `try` block
- we handle possible exception(s), using `except` blocks (one or more)
![try-except-block](../assets/Pasted%20image%2020250114172023.png)
#### Handling and re-raising an exception
- sometimes we want to handle an exception, but then re-raise the same exception or a different exception
	- often because there's nothing we can do
	- sometimes to create a more explicit exception
- to raise an exception in an `except` block:
	- `raise` → re-raises the same exception that caused the except block to be entered
	- `raise SomeException('...')`
##### Application
- one very common case for re-raising is for error logging
	- we can view the logs after our program has terminated abnormally
```python
try:
	...
except Exception as ex:
	log(ex)
	raise
```
- we intercept a broad range of exceptions by handling `Exception`
- we *log* the exception somewhere (console, file, database, etc)
- we *re-raise* the exception and let something else either handle it or terminate the program
---
- what do I mean "something else will handle it?"
	- we'll see this more when we cover functions
	- `try...except...` can be *nested*
		- usually indirectly
		- but directly too
```python
try:
	try:
		raise ValueError('something happened')
	except ValueError as ex:
		log(ex)
		raise
except Exception as ex:
	print(f'ignoring: {ex}')
```
- here our `ValueError` gets handled twice.
#### Handline Multiple Exception Types
- not limited to a single `except` block
```python
try:
	...
except IndexError as ex:
	...
except ValueError as ex:
	...
except Exception as ex:
	...
```
- Python will match the exception to the *first* type that matches in sequence of except blocks
- so write except blocks *from most specific to least specific* exception types
#### The `finally` Clause
- sometimes we want some code to run after a `try...except...` whether an exception occurred or not, and whether it was handled or not
	- use the `finally` clause
```python
try:
	...
except ValueError as ex:
	...
except IndexError as ex:
	...
finally:
	# always runs no matter what, before exception flow resumes
```
##### Application
- useful when we want a piece of code to always run
	- whether an exception has occurred or not
	- whether the exception was handled or not
		- whether exception was re-raised or a new one was raised
```python
try:
	open_database_connection()
	start_transaction()
	write_data()
	commit_transaction()
except WriteException as ex:
	rollback_transaction()
	raise
finally:
	close_database_connection()
```

> Say you have a list of values that you'd like to find the average for.
> 1. If there's a non-numeric value in the list you'd like to skip that value and go on to calculate average for the numeric values in the list.
>2. If there's no value in the list, you'd like to return `0` as average.
>
>This is a good example of nested `try...except...` blocks. See below:
```python
data = [10, 20, 30, None, 'abc']

sum_data = 0
count_data = 0

try:
	for element in data:
		try:
			sum_data += element
			count_data += 1
		except TypeError:
			# skip iteration element - so do nothing 
			pass	
	
	average = sum_data / count_data
except ZeroDivisionError:
	average = 0

print(f'average = {average}') # average = 20.0
# this would also output `0` if `data` was an empty list
```

> Usually, when you raise another error inside an `except...` block, Python shows you a traceback display where you can trace the parent exception where the newly raised exception was raised from.

> For the `finally` block to run, you don't need to have a preceding `except` block, as long as you don't anticipate raising any exceptions whether knowingly or unknowingly through the code within the `try` block.

## Iterables and Iterators
- an iterable is something that can be iterated over (kind of a circular definition)
	- i.e. we take one element, then the next, then the next, until we've covered all elements
	- no specific iteration order is mandated
- obviously a *sequence* type is iterable (positional ordering)
- we saw *dictionaries* can be iterated over (insert order)
- but we also saw *sets*: iterable, but no guaranteed order of any kind
- general idea behind *iteration* is then:
	- start somewhere in the collection (at the beginning if that means something)
	- keep requesting the next element
	- until there's nothing left (exhausted)
---
- so we have two concepts here:
	- a collection of objects that can be iterated over
		- an *iterable*
	- something that is able to give us the next element when we request it
		- an *iterator*
### Iterables and Iterators
- an iterable is something that can be iterated over
	- but we still need something that can
		- give us the next item
		- keep track of what it's given us so far (so it doesn't give us the same element twice)
		- informs us when there's nothing left for it to give us
		- that's called an *iterator*
			- used by Python to iterate over an *iterable*
---
- an iterable is just a *collection of objects*
	- it doesn't know anything about *how* to iterate
	- however it knows how to create and give us an *iterator* when we need it
- iterables implement a special method `__iter__()` that returns a new iterator
	- can also be called using the `iter()` function
- the iterator has a special method called `__next__()` that can be called to get the next element
	- can also use the `next()` function
	- it keeps track of what is has already handed out (so iterator are kind of one-time use!)
	- it raises a `StopIteration` exception when `next()` is called if there's nothing left
#### The Internal Mechanics of a `for` Loop
When we write a `for` loop that iterates over an iterable, what Python is actually doing is this:
```python
l = [1, 2, 3, 4, 5]

iterator = iter(l)
try:
	while True:
		# return next(iterator) - here we'll just print it
		print(next(iterator))
except StopIteration:
	# expected when we reach the end
	# so silence this exception
	pass
```
- the key thing here is that we can see the iterator has some state
	- it has a `__next__()` method
	- but there's no going back, no starting from the beginning again
	- to do that we have to request for a *new* iterator
- and that's what a `for` loop does - it requests a new iterator from the iterable before it starts looping
- objects such as lists, tuples, sets, dictionaries, string, range objects are *iterables*
- but some objects in Python are *iterators* - not iterables
	- iterators actually implement an `__iter__()` method
		- but they just return themselves (with their current state), not a new iterator
	- they allow us to iterate over them
	- but *only once*

> To delete a variable from memory, use the `del` keyword. Like so:
```python
l = list(range(100_000_000))
del l # this will delete `l` variable and release the memory space occupied by it (this also known as 'garbage collection')
```

> `range()` is a *lazy iterable*, it's not an iterator. It's a lazy iterable because it doesn't create the objects yet, until the next object is requested of it.
> 
> In difference, `enumerate()` returns an *iterator* not an iterable. Therefore, once it's been iterated over, it can't be used again unless a *new* iterator is created.

^78ba5e

> Objects that support `iter()` and `next()` are iterators. Objects that only support `iter()` are iterables.

> **Why learn about Iterables and Iterators?**
> Often in Python, objects we get (from calling some functions), are not iterables, but just iterators. In other words, they are not re-usable - in the sense that they become exhausted, and we cannot re-use them to iterate from the beginning. Why do these types of objects even exist? Basically for performance reasons.

> As we'll see later when we study **generators**, it is possible to have an iterator that allows us to iterate over a "virtual" collection - in the sense that the next element is calculated and returned, but no memory is wasted in holding all the elements, and the up-front computational cost of calculating all the elements is avoided - they are generated one at a time, and doled out one at a time.
> 
> This technique of calculating and doling out elements one at a time is called **lazy iteration**. Iterators are generally lazy, and [some iterables can be lazy too](#^78ba5e).

### Generators
- we've seen list, dictionary and set comprehensions
- but no tuple comprehensions...
	- works because list is mutable
	- tuples are not mutable
		- no tuple comprehension
but we *can* write this as a (valid) expression:
```python
(i ** 2 for i in range(5))
```
- this creates a *generator* object
	- generators are *iterators*
		- which means they calculate the `next()` method/function 
	- they calculate and hand out elements *one at a time* as requested
		- unlike `[i ** 2 for i in range(5)]`
			- calculates all the elements and *creates* the list *immediately*
	- generators use *lazy iteration*
		- a *lazy property* is one that is not calculated until it is requested
#### Why use Generators?
- memory efficiency
	- e.g. take all the rows from a file, and write them out, transformed to some other file
		- read the entire file in memory, iterate through that and save rows 
			- entire file in memory!
			- you may not have enough memory!
		- read lines one at a time from file
			- read a row, process it, save it, discard it, request next row, ...
			- only one line in memory at any point
- performance (possibly)
	- if you only need to read the first few elements of the iterable
	- why go through the computations to calculate all of them?
		- plus unnecessary memory usage on top of that
#### What's the Downside of Generators?
- generators are *lazy iterators*
	- *one-time use*
- not good if you need to iterate through the same iterable many times
	- or even just a few times if the calculations are computationally expensive or take a long time (maybe IO bound)
#### Creating Generators
- use *generator comprehensions*
- use the *yield* keyword in functions instead of return
	- this is a more advanced concept than python fundamentals

> In summary, generators are great! But beware, they are not re-usable, and if you're going to need to iterate through them multiple times, you may be better off making the performance/memory trade-off.

## Functions
#### Why?
- easy *code re-use*
	- much easier to code the `sqrt()` function once
		- and then call it multiple times
- breaking up complex code into easier to understand chunks
	- problem *decomposition*
---
- when we create a `function`, we may want values to be passed into it when it's called
	- *arguments* or *parameters*
		- technically not the same thing but everyone uses them interchangeably
- when we define a function, we may define symbols for the values that will be passed into the function
	- these symbols are called *parameters*
- when we call a function, we specify values for these parameters
	- these values are called *arguments*
- so a *parameter* is when we *define* the function
- an *argument* is when we *call* the function
#### Functions are Python Objects
- just like everything in Python, functions are objects
	- they have state
		- name (maybe!)
		- code
		- parameters
		- they are *callable*
			- and **always** `return` something when called
- they can be assigned to a symbol
- can be passed as a parameter to another function
- can be returned from a function call
#### Callables
- an object is *callable* if it can be *called* - using `()`
- functions are run by calling them
	- `print('hello')`
	- `math.sqrt(4)`
- but other types of objects are also *callable*
	- not necessarily a `function` object
```python
my_list.copy() # calling the *method* on the `my_list` object
range(100) # creating a *new* `range` object
```
- the more general term is *callable*

### Custom Functions
- functions can be defined using the `def` keyword
```python
def function_name():
	# indented block
	...
	return <value>
```
![how functions are defined](../assets/Pasted%20image%2020250116114922.png)
- function body contains valid Python code
- this creates a function object
- the function object is associated with the symbol `function_name`
	- (in the same way `a = 10` associates the integer object `10` with the symbol `a`)

> If you don't explicitly specify a `return` value in your function, Python will automatically return `None`

- functions are more useful when we pass values to them
	- `len(my_iter)`
		- here, we pass an argument `my_iter` to the `len()` function
	- every time we call the `len` function we can pass a different value
		- the function body (implementation) of the `len` function start running
			- it's aware of the value that was passed to it
- same with custom functions
	- need to specify the *parameters*, by name, that will be used when we *call* it
```python
def add(a, b):
	return a + b

add(2, 3) # returns 5
add(10, 1) # returns 11
```
- when we call `add(2, 3)`, how does Python assign `2` to the symbol `a`, and `3` to `b`?
	- it does this by *position*
![positional arguments in python function](../assets/Pasted%20image%2020250116120859.png)
- *positional arguments*

> Although the parameters are defined as positional parameters, Python supports **calling** the function with **named** arguments as follows:
```python
add(a=2, b=3) # returns  5
```
> The advantage of this is: we don't have to rely on getting the positions of the arguments correct, *as long as we are naming them*, Python will assign them to their respective parameter in the function.
> 
> So, although the `add(a, b)` function uses **positional** parameters, we can choose to pass them **positionally** or **named** (also referred to as **keyword** arguments).

> It's possible to call a function that defines positional parameters using a **mix** of positional and named arguments. However, we have to be careful, once we start using named arguments in a call, *all subsequent arguments must be named too*.

> In Python, there is a naming convention to use `_` or `__` by themselves when dealing with variables whose values are not used elsewhere. For instance in a list comprehension or loop. Like so:
```python
# instead of doing :
[[default_value for i in range(n)] for j in range(m)]
# do :
[[default_value for _ in range(n)] for __ in range(m)]
```

#### Namespaces
- when a function is called *it knows nothing about how it was called before*
- every time a function is called 
	- an empty dictionary is created
	- populated with any arguments passed in
		- key = param name, value = argument
		- nothing else
	- then the function code runs
	- this dictionary is called the (local) *namespace*
		- after the function returns the dictionary is wiped out from memory
		- consecutive calls to the same function are independent of each other
[Learn more about local namespaces in functions](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35151032?start=642#notes).

### Star Arguments
- [saw](assets/Pasted%20image%2020250116120859.png) how to specify positional parameters in a function
```python
def average(a, b, c, d):
	return (a + b + c + d) / 4
```
- but what if we wanted to specify an *arbitrary* number of parameters?
	- we'd like to call our function with different number of arguments
```python
average(1)
average(1, 2, 3)
average(1, 2, 3, 4)
```
- could write a function to use an iterable as a single argument
```python
def average(iterable):
	return sum(iterable) / len(iterable)
```
- but this makes calling the syntax a little weird
```python
average([1, 2, 3])
average([1])
```
- would be nicer if we had a mechanism to accept a *variable* number of args 
---
- Python supports a special parameter type for this
	- uses a `*` prefix on a parameter name
```python
def average(*values):
	# return average
```
- this means we can call `average` with any number of arguments 
```python
average(1)
average(1, 2, 3, 4, 5)
```
- how do we access these values *inside* the function?
	- use the parameter name
		- `values` in this case
		- it'll be a `tuple` containing all the argument values
---
- often you'll see code that uses `*args`
	- the `*` is the important part
	- there is nothing special about the name `args`
	- we can use any valid name
		- ultimately, use a meaningful name
		- `args` is often too generic (but this is largely what python developers use out in the wild when specifying *an arbitrary number of positional arguments*)

> You cannot specify *specific positional parameters* **after** a *starred parameter has been defined*. Meaning you cannot do this:
```python
# this is wrong :
def my_func(a, b, *args, c):
	# function body
	...
# this is wrong because `*args` will scoop up all the positinal arguments after `a` & `b`. `c` will not get any positional arguments

# the only way this works if for `c` to be passed in as a *keyword-only argument*, like so :
my_func(1, 2, 3, 4, 5, c=6)
```

### Default Values
- possible to specify *optional* parameters
	- means function can be called *without* passing in the argument
	- but we still have that parameter
		- it needs a value
		- we can specify a *default* value to use if the argument is not supplied
For example:
```python
def func(a=1): # `a` parameter has a default value of 1
	print(a)
func() # returns 1 'default value'
func(10) # returns 10 'argument value'
```
![default values](../assets/Pasted%20image%2020250116132425.png)
- once you specify a positional argument with a default value
	- all positional parameters after that *must* specify a default value too
		- with the exception of a *starred* parameter
	- why is that?
```python
def func(a=1, b):
	pass

func(10)
```
- is `10` supposed to go into `a`?
	- in which case we're short one argument
- or use default for `a` and assign `10` to `b`?
- don't know! (this is problematic for Python to parse)
---
- so once we have default arguments we need to specify default for all parameters after it
```python
def func(a, b, c=1, d=2):
	...
```
- but this is still ok:
```python
def func(a, b=1, *args)
```

### Keyword-Only Arguments
- we saw how positional parameters can be passed
	- positionally
	- as a *named* argument
		- also called a *keyword* argument
```python
def func(a, b, c):
	...

func(1, 2, 3)
# or ...
func(c=3, b=2, a=1)
```
 - passing argument as a keyword argument is *optional*
---
- can also make passing an argument by name *mandatory*
	- these are called *keyword-only* arguments
- keyword-only parameters must come *after* all positional parameters
	- `def func(a, b, c)`
	- we want `c` to *always* be passed as a named argument
	- somehow we have to tell Python that after `a` and `b` there are no more positional arguments
	- one way is to use a `*` parameter
		- `def func(a, b, *args, c)`
		- since `*args` will scoop up every remaining positional argument
			- `c` *must* be a keyword-only argument
```python
# calling the function above: `def func(a, b, *args, c)`
func(10, 20, 30, c=100) # returns a=10, b=20, args=(30,), c=100
func(10, 20, c=100) # returns a=10, b=20, args=(,), c=100
```
---
- but this allows someone to pass in as many positional arguments as they want
	- what if we don't want that?
- we still have to tell Python that there are no more positional arguments
- we use a `*` *without* a parameter name
```python
def func(a, b, *, c):
	...
```
- `a` and `b` are positional parameters
- there are no more positional parameters after that
- so `c` is a keyword-only argument
```python
# calling the function above: `def func(a, b, *, c)`
func(10, 20, c=100) # ✅
func(10, 20, 30, c=100) # ❌
```
- using this technique `c` *must* be passed as a named argument
- `a` and `b` *can* be passed as positional arguments
	- or as *named* arguments
```python
# calling the function above: `def func(a, b, *, c)`
func(b=2, a=1, c=3) # ✅
func(a, c=3, b=2) # ✅
```
#### Default Values
- can also assign *default* values to keyword-only arguments
```python
def func(a, b, *, c=100):
	...
```
- `c` is optional, and will default to `100`
- if `c` is passed, it *must* still be passed as a named argument
```python
# calling the function above: `def func(a, b, *, c=100)`
func(10, 20) # c=100
func(10, 20, c=30) # c=30
```
- can mix default values for both positional and keyword-only arguments
#### Arbitrary Number of Keyword-only Parameters
- saw `*` for arbitrary number of positional arguments
- use `**` for arbitrary number of keyword-only arguments
```python
def func(a, b, *args, c, d, **kwargs):
	...
```
- `a` and `b` are positional
- `c` and `d` are keyword-only
- extra positional arguments are scooped up into `args`
- extra named arguments are scooped up into `kwargs`
---
how are keyword-only (or named) arguments they scooped up?
- `**` keyword-only arguments are scooped up into a *dictionary*
	- *key* is the argument *name*
	- *value* is the argument *value*
```python
def func(a, *, d, **others): # `others` is a dict
	...
```
![keyword-only arguments](../assets/Pasted%20image%2020250116194652.png)

> In the same way we use `*` as a parameter to express that all the parameters *after it* must be keyword-only arguments, we use `/` as a parameter to express that all the parameters *before it* must be positional-only argument (in other words, they cannot be named arguments). Like so:
```python
def func(a, b, *, c): # `x` indicates `c` and any other prevailing parameter after `*` must be passed as a keyword-only argument
	...
# in the same way...
def func(a, b, /, *, c) # `/` indicates `a` and `b` and any other parameter before `/` must be passed as a positional-only argument (they cannot be passed as named arguments)
```

> When formatting code using print format like so `print(f'...')`. If you want to return a JSON format that contains curly braces as a `str` literal `'{...}'` then you double up the interpolation curly braces to tell the `print(f'...')` function that you're escaping the interpolation. Like so:
```python
print(f'{{"longitude": {longitude}, "latitude": {latitude}}}') # notice the double-up `f-string` curly braces

# this would return valid JSON syntax, like so :
'''
{
	"longtitude": 10,
	"latitude": 20
}
'''
```

### Lambda Functions
- *lambda functions* are just functions
	- they are not defined using a `def` and a block of code
	- it's an *expression* that returns a function object
		- Python does not create a name or symbol for the function
		- just returns the function object
		- we can assign it to a variable or pass it as an argument
		- also called *anonymous functions
		- they are very *simple* functions (no code block)
```python
lambda a, b: a + b # this is a lambda function (an anonymous function)
```
![lambda function](../assets/Pasted%20image%2020250116204612.png)
- this expression returns a `function` object
- we need to assign it to a symbol if we want to use it
```python
f = lambda a, b: a + b
f(10, 20) # 30
```

> Positional, keyword-only arguments, and complex parameters work in Lambda Functions the same way they do in normal (or defined) functions. 

- can *always* use a function defined using `def` instead of these lambdas
- generally used to write shorter code in some *simple* cases
	- we'll see an example of this in the next sections
	- but you *don't have* to use them
- however, they do get used often, so you should be aware of them

> The `replace()` function or rather method for `str` can iterate through a string of characters and replace the first argument character specified in the function parameter with the second. Like so: 
```python
s = "word1, word2. word3 word4 word4"

s = s.replace(',', ' ') # replace('replace_first_arg', 'with_second_arg')
s # 'word1  word2. word3 word4 word4'
```

## Some Additional Functions
### `round`
- `round()` is a built-in function that can be used to round floats
	- uses *banker's rounding*
		- also called *round half to even*
		- rounds away from zero
			- `1.8` → `2`
			- `-1.8` → `-2`
		- ties round to closest even digit
			- `1.5` → `2`
			- `2.5` → `2`
		- good choice to eliminate various *biases*
- can also use `round()` to round to the closest multiple of $\frac{1}{10}$
	- `round(value, exponent)`
	- `exponent` is used to specify what power of $\frac{1}{10}$ to round to
- let's look at it mathematically first (i.e. without worrying about float representations)
	- `round(x, 1)` → rounds to the nearest `0.1` $10^{-1}$
	- `round(x, 2)` → rounds to the nearest `0.01` $10^{-2}$
	- `round(x, -1)` → rounds to the nearest `10` $10^{1}$
	- `round(x, -2)` → rounds to the nearest `100` $10^{2}$
- not just rounding things *after* the decimal point but also *before* the decimal point
![rounding to closest multiple of...](../assets/Pasted%20image%2020250119154947.png)
#### Rounding Ties in Floats
- technically rounds to closest number that ends with an even digit
```python
round(0.125, 2) # 0.12
```
- so why this?
```python
round(0.325, 2) # 0.33, why not 0.32?
```
- remember floats do not have (in general) an exact representation!
```python
0.325 # 0.325000000000000011102230246252
```
- so this is **not** a tie!

> By default, `round()` will round to the closest integer (using banker's rounding).

> Remember, `format(0.325, '.20f')` shows you the exact representation of the float `0.325` in Python.

### `sorted`, `min`, `max`
#### Sorting Numbers
- numbers have a *natural* sort order
- they can be sorted *ascending* or *descending* by that *sort order*
- `sorted` is the built-in function that can be used to sort a collection of numbers
	- single positional argument: an *iterable* containing the numbers
	- by default, it sorts in *ascending* order
	- keyword-only argument to `reverse` the sort order
		- default is `False` → sorts *ascending*
		- specify `reverse=True` → sorts *descending*
	- always returns a new `list`
		- original iterable is *not mutated*
For example: ^a8d2f3
```python
t = (1, 10, 2, 9, 3, 8)

sorted(t) # [1,2, 3, 8, 9, 10]
sorted(t, reverse=True) # [10, 9, 8, 3, 2, 1]
```
#### Sorting Strings
- numbers have a natural sort order
- strings also have a natural sort order in Python
	- *lexicographic* order
		- *dictionary* order, *alphabetical* order
- **BEWARE** the characters `a` and `A` are not the same
- Python assigns a numerical character code (the unicode character code) to every character in a string
```python
ord('A') # 65
ord('Z') # 90
ord('a') # 97
ord('z') # 122

# therefore, 'A' < 'Z' < 'a' < 'z'
```
- so, Python will use "alphabetical" sorting, but upper case letters will be sorted before their equivalent lower case versions
- natural sort order of string is case sensitive
```python
sorted(['Boy', 'baby']) # ['Boy', 'baby'] - ascending sort
```
#### Sorting Other Types
- we can "visually" sort other types of objects 
	- list of *Persons*
		- we can sort this list
			- *by* name
			- *by* age
			- *by* profession
- we always sort *by* some property of the objects we're sorting
	- we'll come back to this in a later section
#### `min` and `max`
- closely related to sorting
	- to find the *minimum* of a collection
		- *sort* the collection (*by* something) in *ascending* order
		- pick the *first* element
	- to find the *maximum* of a collection
		- *sort* the collection (*by* something) in *descending* order
		- pick the *first* element
	- (or you could sort in the other direction in both cases and pick the last element)
```python
min([1, 10, 2, 9, 8]) # 1
max([1, 10, 2, 9, 8]) # 10
min([]) # `ValueError` exception
```
- can specify a `default` value to return if the iterable is empty
	- keyword-only argument
```python
min([], default=0) # 0
```
- can also use an arbitrary number of positional arguments instead
```python
min(1, 10, 2, 9, 3, 8) # 1
max(1, 10, 2, 9, 3, 8) # 10
```
- we'll come back to `min` and `max` when we look at sorting again later...
### `zip`
- the `zip()` function is a very useful and often used function
	- consider these lists that contain related information
```python
l1 = ['a', 'b', 'c', 'd', 'e', 'f']
l2 = [97, 98, 99, 100, 101, 102]
```
- we want to create a list of tuples that contain the corresponding elements from `l1` and `l2` 
- we could do this:
```python
combo = [(l1[i], l2[i]) for i in range(len(l1))]
combo # [
#			('a', 97)
#			('b', 98)
#			('c', 99)
#			('d', 100)
#			('e', 101)
#			('f', 102)
#       ]
```
- but, we may have an issue if two lists are not of the same length
	- have to stop at the shortest of the two lengths
```python
l1 = ['a', 'b', 'c', 'd', 'e']
l2 = [97, 98, 99]

combo = [(li[i], l2[i]) for i in range(min(len(l1), len(l2)))]
combo # [
	#       ('a', 97)
	#       ('b', 98)
	#       ('c', 99)
#       ]
```
- that's what the `zip()` function does!
```python
l1 = ['a', 'b', 'c', 'd', 'e']
l2 = [97, 98, 99]

combo = zip(l1, l2)
```
- **BEWARE** `zip()` returns an *iterator*
	- remember those?
	- can only iterate through them once
```python
list(combo) # [
		#       ('a', 97)
		#       ('b', 98)
		#       ('c', 99)
#             ]
list(combo) # []
```
---
- if you want to iterate multiple times over the same zipped collection
	- store it in a list
```python
combo = list(zip(li, l2))
```
- often don't need to do that (why?):
	- `zip()` does not actually create anything other than an iterator
	- no physical space has been used for the tuples
	- iterating over `zip()` result, just iterates over the iterables simultaneously
- costs almost nothing to call `zip(l1, l2)` multiple times
---
- zip is extensible
	- not limited to two iterables
	- any number of iterables (as positional args)
```python
l1 = [1, 2, 3]
l2 = [1, 2, 3, 4, 5]
l3 = [1, 2, 3, 4, 5, 6, 7]

zip(l1, l2, l3) # (1, 1, 1)
				# (2, 2, 2)
				# (3, 3, 3)
```
- always returns an *iterator* that produces *tuples*

## Higher Order Functions
- in Python any object can:
	- be *passed to* a function as an argument (or a callable in general)
	- be *returned from* a function (or callable in general)
- functions are *objects*
- functions can be *passed to* and or *returned from* functions
- these are called *higher order functions*
	- it's a math concept too - often referred to as operators or functionals
		- (functions that do not allow passing a function to or returning a function are called first order functions)
- amongst other things:
	- a function definition can itself contain another function definition 
		- and can return it
	- this means we can call a function that builds another function and runs it, or even returns it
	- what becomes interesting is that variables in the outer function become available to the inner function

### Passing and Returning Functions
- function arguments can be functions
	- the object is passed, not called
		- so don't use `()` to pass a function, that would pass the result of the function!
![passing and returning functions](../assets/Pasted%20image%2020250122134328.png)
#### Nested Functions
- function bodies can contain any valid Python code
	- including defining functions
![nested functions](../assets/Pasted%20image%2020250122134459.png)
#### Returning Functions
- a function can also return a function
![returning functions](../assets/Pasted%20image%2020250122134639.png)
- silly example
- that said, often we return a nested function
```python
def generate_func(name):
	def add(a, b):
		return a + b
	def mult(a, b):
		return a * b
	if name == 'sum':
		return add
	else:
		return mult

f = generate_func('sum')
f(2, 3) # 5
```
- still a silly example but we'll see more real examples soon.

### `map`
- the `map()` function calls a specified function for every element of some iterable
- very similar to doing something like this:
```python
def my_map(func, iterable):
	result = [func(element) for element in iterable]
	return result
```
- here we are creating a `list` that contains the function `func` applied to every element of `iterable`
	- but it creates a `list`
	- can take a lot of space if iterable is large
	- especially wasteful if we don't iterate over all the values

- `map()` returns an iterator
```python
iterator = map(func, iterable)
```
- as we iterate over that iterator:
	- Python moves to the next item in `iterable`
	- calls `func(element)`
	- returns the result 
		- less wasted space
		- saves computations if we don't iterate over the whole list
- equivalently we could also just use a *generator expression* 
```python
(func(el) for el in iterable)
```

> It's important to understand that iterators/generators are far more efficient than building a list with comprehension.

### Closures
- formerly, we saw that function definitions can be *nested* within another function
```python
def outer():
	def inner():
		...
```
- and we saw that we can return the inner function from the outer function
```python
def outer():
	def inner():
		...
	return inner
```
- but we can create variables from the outer function also, or pass arguments when we call it
```python
def outer(a, b):
	c = 100
	def inner():
		...
	return inner
```
- `inner` can "see" those variables 
	- it even retains these values when it is returned
	- the inner function can "capture" those variables
		- this is called *closure*
```python
def outer(a):
	def inner():
		return a * 10
	return inner

f = outer(2)

f() # returns 20
```
- `f` is now the inner function that *closes* over `a` *with a value of* `2`
	- `a` is called a *free variable* of the closure `f`
- we can call `f`
	- `f()` → `20`
---
- but there are some *rules*!
	- you can always "*read*" a variable from the outer scope
![closure](../assets/Pasted%20image%2020250122150001.png)
- outer
	- `{'c': 100, 'inner': <function>}`
- inner
	- *closure* `inner`, with `c=100`
---
- but things change if we *set* that symbol to a value in the *inner* scope
![closure - inner and outer scope](../assets/Pasted%20image%2020250122150253.png)
- outer
	- `{'c': 100, 'inner': <function>}`
- inner
	- `{'c': 20}`
---
Example:
```python
def power(n):
	def inner(x):
		return x ** n
	return inner
squares = power(2)
squares(3) # 9
```
- call `power(2)`
	- `power` runs with `n = 2`
	- `inner` is a function that "captures" `n = 2` → a *closure*
	- the closure is returned
- `squares` is the closure: function `inner` with `n=2` that takes one argument (`x`)
- we can re-use `power` multiple times:
```python
def power(n):
	def inner(x):
		return x ** n
	return inner

squares = power(2) # [inner with n = 2]
cubes = power(2) # [inner with n = 3]

squares(3) # 9
cubes(3) # 27
```

## Sorting and Filtering
### Filtering
- *filtering* is the selection of a subset of items based on whether some condition is true or not
- given a list of numbers from 1 to 100, filter this list to contain even numbers only
- can think of it this way:
![filtering](../assets/Pasted%20image%2020250124184223.png)
- apply a function (`is_even`) to every item in the list
	- only keep items for which function returns `True`
#### Predicate Function
- a *predicate function* is simply a function or one or more arguments that returns `True` or `False`
- for filtering in general:
	- given an *iterable* and *predicate function*
	- only keep the items for which the predicate function evaluates to `True`
```python
# for example: imagine we have this list below
l = [1, 2, -5, 6, -1, 0] # iterable

# predicate function
def is_positive(x):
	return x > 0

# filter
l = [1, 2, -5, 6, -1, 0]
pred = is_positive
# output : [1, 2, 6]
```
---
- python has a `filter` function that works exactly that way 
```python
filter(pred, iterable)
```
See examples:
```python
data = [1, 2, 3, -1, -2, 0]

def is_positive(x):
	return x > 0

filter(is_positive, data) # [1, 2, 3]

def is_even(x):
	return x % 2 == 0

filter(is_even, data) # [2, -2, 0]
```
- `filter(is_positive, data)`
	- lazy *iterator*
	- can only iterate through this once
---
- can also use a predicate function created via a lambda
```python
is_positive = lambda x: x > 0
filter(is_positive, data)
```
- and often, directly inline with the call to `filter`:
```python
filter(lambda x: x > 0, data)
```

> Functions that return `True` or `False` based on one or more arguments, are called **predicate** functions.

### Sorting
- we've looked at the `sorted` function [before](#^a8d2f3)
	- given an *iterable*
		- return a `list`, that has been *sorted*
			- but *by what*?
```python
sorted([10, 9, 3, 1, 2, 8]) # [1, 2, 3, 8, 9, 10]
```
- sorting numbers is very intuitive
	- numbers have a natural sort order, and we can sort the elements based on their values
- sorting strings was a bit more complicated
	- assign an integer value to each character, and use that to sort strings
---
- we can sort the *same* data in *different* ways
```python
data = [3, 1, -6, -2, -4, -5]
```
- sort based on value
```python
# [-6, -4, -2, 1, 3, 5]
```
- sort based on absolute value
```python
# [1, -2, 3, -4, 5, -6]
```
- sort based on the second digit of the square root of the absolute value
	- maybe something more practical
		- sort a collection of objects (symbol, open, high, low, close)
			- by symbol
			- by open
			- by high - low
---
- how do we sort by a different criteria
- how do we sort arbitrary objects that may not have a natural sort order
- approach is similar to how filter worked 
	- iterable
	- to each element in iterable, assign a value that is used to sort
- just like `filter` used a predicate function to calculate `True`/`False` for each element
---
- `sorted` can take a `key` function as a *named* argument
	- `key` function returns a value for each element
	- those values have a natural sort order
	- usually numbers, but does not have to be
```python
data = [3, 1, -6, -2, -4, 5]

def sort_key(x):
	return abs(x)

sorted(data, key=sort_key)
sorted(data, key=lambda x: abs(x))
sorted(data, key=abs)
```
---
- the main point is that `key` is just a *function* that returns a value for each element of the iterable
- `sorted()` then uses that value to sort the items in the iterable
```python
data = {'a': 300, 'b': 100, 'c': 200}
```
- sort the keys of the dictionary based on the corresponding value
	- *key_func(dict_key)* → *corresponding value*
```python
sorted(data.keys(), key=lambda k: data[k]) # ['b', 'c', 'a']
sorted(data.keys(), key=lambda k: data[k], reverse=True) # ['a', 'c', 'b']
```

> We define a **key** function that, for each element of an iterable, calculates some value. The sort can be be made based on that **key** function's return value for each element.

> The data below was sorted by absolute value.
> Something to notice in the result is that both `-6` and `6` have the same key value (`6`) - so which one comes first in the sorted elements? That will depend on the relative positioning of those original elements in the iterable, and since `-6` occurred before `6` in the original `data`, we end up with the relative positioning in the sorted list.
> Not all sorts behave this way - those that do, and Python's `sorted` sort algorithm does, are called **stable sorts**.
```python
data = [-10, -6, 0, 3, 6]

def key_func(x):
    return abs(x)

sorted(data) # [-10, -6, 0, 3, 6]

sorted(data, key=key_func) # [0, 3, -6, 6, -10]
```

### `min` and `max`
- previously we saw that to get the minimum of an iterable
	- sort iterable from low to high
	- take first element
- similarly with maximums
- but we just saw that sorting always uses an associated key
	- so when we talk of `min` and `max`
	- we really have the same thing - a *sort key* is used
```python
min(iterable, key=<func>)
max(iterable, key=<func>)

data = [-1, 2, -3, 4, -5]

min(data) # -5
max(data) # 4
```
- but this assumes a natural sort order
	- i.e. key func is an identity function - returns the iterable value as is
- let's say we want the sort to be based on the absolute value 
```python
min(data, key=abs) # -1
max(data, key=abs) # -5
```

## Decorators
- decorators are a form of metaprogramming
	- **metaprogramming** is a fancy way of saying we can modify how some piece of code runs by using another piece of code
- they allows us to wrap some functionality around an already defined function
	- without having to modify the code of the original function
- leverages:
	- closures
	- functions are first class citizens (aka. higher order functions) 
	- reassign any object to an existing symbol
#### Why are decorators useful?
- let's use an example to understand this
- suppose we have a program with some functions called over and over again
	- `fun1`, `fun2`, `fun3`, etc
- every time one of those functions is called, we want to produce a *log*
	- maybe just print to the console that the function was called
- we could certainly put the "logging" functionality into each function (as seen below)
![why are decorators useful](../assets/Pasted%20image%2020250128113734.png)
- repeating the same code multiple times
- what if we want to include date/time the call was made
	- go back and edit logging code inside each function
	- 3 weeks later: add some timing to it too
		- go back and edit logging code inside each function
- does not work this way, WHY?
	- *too much typing!*
	- *error-prone*
	- *easy to be inconsistent*
---
- instead we write the logging code *once*
	- and "apply" it to each function we want to log
- basically we want to build a second function that will:
	- run some code
	- execute the original function with the arguments that were passed in
	- run some code
	- return the result of the call
- when we call `fun1`, `fun2`, etc
![why are decorators useful 2?](../assets/Pasted%20image%2020250128114451.png)
### Decorators
- recall nested functions
```python
def outer():
	def inner():
		...
	return inner
```
- calling `outer()` --> returns the function `inner`
```python
f = outer()
f() # this called `inner` returned by `outer` 
```
---
- using closures, we can do this:
```python
def outer(fn):
	def inner():
		print(f'Calling {fn}...')
		result = fn()
		return result
	return inner

def hello():
	return 'Hello!'

f = outer(hello) # `inner` function is created
				 # it's a closure with `fn` pointing to `hello`

f() # calls `inner`, with `fn` pointing to `hello`
	# this calls `hello()`
	# and returns the result of the call - 'Hello!'
```
- so `outer` can *create* and *return* a function that will execute whatever function is passed as an argument to `outer`
```python
f = outer(fun1) # f() --> will call `fun1` (and maybe do some extra things)
f = outer(fun2) # f() --> will call `fun2` (and maybe do some extra things)
f = outer(fun3) # f() --> will call `fun3` (and maybe do some extra things)
```
- but we also want to be able to pass *arguments* to `fun1`, `fun2`, `fun3`
	- so pass them to `inner` and use those args to call `fn`
```python
def outer(fn):
	def inner(*args, **kwargs):
		print(f'Calling {fn}...')
		result = fn(*args, **kwargs)
		return result
	return inner
```
- can think of this as a wrapper for `fn`
	- we call `outer(fn)` to create a *new function* that wraps `fn`
		- we can *call* that *new function* with *arguments*
			- it does it's *own thing* (like `print` for example)
			- but it also *executes* `fn`, with whatever arguments we pass in
				- and *returns* the result of the call
```python
def log(fn):
	def inner(*args, **kwargs):
		print(f'Calling {fn}...')
		result = fn(*args, **kwargs)
		return result
	return inner
```
- we have a very simple logger here `log(fn)`
- suppose we have some functions
```python
def add(x, y):
	return x + y

def greet(name):
	return f'Hello {name}!'
```
- we can create some functions that will perform the same task and also run the logging code
```python
add_logged = log(add)
greet_logged = log(greet)
```
---
- so now, instead of calling `add`
	- call `add_logged`
		- if we need to change logging format
		- do it in just one place!
	- but we must change all calls to `add` to now be `add_logged`
		- yikes!
- remember that Python is a dynamic language
	- we can re-assign any object to any symbol
- instead of: `add_logged = log(add)`
	- how about: `add = log(add)`
	- the symbol `add` now points to the *new* function (not the original `add`)
		- which will call the *original* function object
---
- that's the basic decorator pattern
```python
def wrapper(func):
	def inner(*args, **kwargs):
		# some code here ...
		result = func(*args, **kwargs)
		# some code here ...
		return result
	return inner

def func(a, b):
	...

func = wrapper(func)
```
- `wrapper` is called a *decorator*
---
- this is so common
```python
def func(a, b):
	...

func = wrapper(func)
```
- there is a shorthand notation!
```python
@wrapper
def func(a, b):
	...
```
- exactly same as above
```python
def func(a, b):
	...

func = wrapper(func)
```

> [Learn more](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35151144?start=650#notes) about Python's logging system by importing the logging package, like so:
> `import logging`
> 
> To configure the logger/logging system per application, do this:
```python
import logging

logging.basicConfig(
					format='%(asctime)s %(levelname)s: %(message)s',
					level=logging.DEBUG
)

# after configuring the logging system for your application...

logger = logging.getLogger('Custom Log') # this attaches a logging object to the `logger` symbol

logger.debug('debug message') # this calls the logging object on the 'debug' log level with the message "debug message"

# to learn more about the logger & logging system in Python standard library, check out the Python docs
```

> Decorators are very handy for adding pre and post function-call code that is reusable across multiple functions in a way that is completely transparent to the caller of the function.

### LRU Cache
- this is a really interesting application of decorators
- it solves the following problem
	- you have some function that gets called often
		- the *same* set of *arguments* are used *often*
		- the function is *deterministic*
			- calls with the same arguments should produce the same result
		- re-calculating the function is fairly *costly*
- we could use a *caching* mechanism
	- *first time* a set of arguments is encountered, calculate result
		- store result in a *cache*
	- *subsequent* calls with *same* arguments, recovers result from *cache*
---
- basic idea is this:
```python
cache = {}
def func(a, b, c):
	key = (a, b, c)
	if key in cache:
		return cache[key]
	# calculations here
	cache[key] = result # add result to cache
	return result
```
- first time we call `func` with `func(1, 2, 3)`
	- result is calculated
	- result is inserted into `cache` dictionary using the key `(1, 2, 3)`
- next time `func(1, 2, 3)` is called, result is returned directly from *cache* dictionary 
- we'll see in code, how we can try doing this ourselves using decorators
---
- Python has such a decorator - the `lru_cache` decorator
	- LRU - Least Recently Used
- caches should not grow indefinitely
	- so keep the `n` most recent
- works well when most recent calls are good predictors of upcoming calls
- can specify the cache size we want
	- `maxsize` positional argument
		- `None` means unbounded (this produces an unbounded cache which using more and more memory, the more the cache needs for storage)
		- otherwise specify an `int` to set cache `maxsize`
---
- this is the way it works:
```python
from functools import lru_cache

@lru_cache(maxsize=20)
def func(a, b, c):
	...
```
- uses a decorator
	- this decorator can also take arguments
- there is a restriction
	- the arguments passed to the function must be *hashable* values
		- that's because they are used as a key in the cache dictionary

> The term "**memorization**", which is not a spelling typo but a real term in computing is a specialized computing technique that involves storing the results of expensive function calls and reusing them when the same inputs occur again.
> 
> This is what is achieved in Python through the use of the **LRU Cache**.

> When a function calls itself, it's known as a **recursive function**. An example of a recursive function is the Fibonacci algorithm. Recursive functions are used to implement certain algorithms like Fibonacci, factorials, etc. Below is the Fibonacci algorithm:
```python
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)
```

> The LRU cache + Fibonacci algorithm performance and memory relationship is the perfect example of how, as often is the case, we need to balance performance with memory usage.

## Text Files
- opening text files
- read data from them
- write data to them
- remembering to close the files when we're done!
	- a mechanism to make sure we never forget
		- **context managers**
#### What are Contexts and Context Managers?
- this doesn't cover how to create one from scratch (although Python lets us do that)
	- we'll use one already created
- a *context* is an area of code that is *entered* and *exited*
- it is *entered* by "calling" a *context manager* using a `with` statement 
- it is *exited* when the `with` code block is exited
- the context manager is responsible for...
	- running some code on entry
	- running some on exit
![context manager](../assets/Pasted%20image%2020250129195529.png)
### Reading Text Files
#### Opening Files
- to read or write to a text file, we first need to *open* the file
```python
open(file_path) # where `file_path` is path to file you want to open
				# can be absolute or relative to where the Python app is running
```
- need to tell Python *how* we want to interact with the file, the `mode` of operation
```python
open(file_path, mode) # `mode` has three values:
					  # `r` : read-only (default)
					  # `w` : write-only, create new file, or overwrite if it exists
					  # `a` : write-only, create new file, or append if it exists
					  # `r+` : both read and write
```
---
- what is returned by `open()`?
	- an object that has many methods and properties
		- `readlines()` --> to read the file line by line where each line is a `list` item
		- `closed` --> is file closed?
		- `close()` --> this allows us to close the file after we're done with it
		- `name` --> the name of the file
		- `readable()` --> is the file readable?
		- `writeable()` --> is the file writeable?
		- `mode` --> what is the file 'open' mode? `r`, `w`, or `a` ?
	- but it's also an *iterator*
		- provides iteration over the individual lines in the text file
		- `next`
			- `for` loop, etc.
		- technically, we can reset the "play head" (but that would be learned in an advanced course)
			- just think of it as an iterator
#### Closing Files
- always close a file after you're done with it
	- releases the resource (not unlimited number of open files)
	- writes are often buffered until the file is closed
```python
f = open('file_path', 'w')
# write to file ...
f.close()
```
---
- but what happens if an exception occurs while the file is open?
- use a `try...finally...` to always close the file, no matter what
```python
f = open('file_path', 'w')
try:
	# write to file ...
finally:
	f.close()
```
- that's one approach
#### `open()` as a Context Manager
- `open()` is also a context manager
```python
with open('file_path', 'w') as f:
	# write to file ...
```
- as soon as the context exists, file is closed
	- even if an unhandled exception occurs in context block

### Writing Text Files
- same principle as reading text files
	- *open* file (in write mode)
		- *write* to file
	- *close* file
		- especially *important* for writes, since changes may be lost otherwise
- best to use a *context manager*
#### Write modes
- `w`
```python
open('<file_path>', 'w')
```
- *creates* file if it does *not exist*
- *overwrites* (clears out) file if it *already exists*
---
- `a`
```python
open('<file_path>', 'a')
```
- *creates* file if it does *not exist*
- *appends* to end of file if it *already exists*
#### Writing Text to File
- `f.write(<some string>)`
	- writes specified to string
		- it does not add a `\n` character automatically
		- have to do that ourselves if we need it
- `f.writelines(<iterable of strings>)`
	- writes each string as iterable to file
		- it does not add a `\n` character automatically after each string

> When we use the `write()` method on the object returned from the `open()` function, the return value is the number of characters written to the file.
```python
f = open('test.csv', 'w')
f.write('abc') # 3 : `3` is the return value of how many characters 'abc' were written into the file
```

> To create a newline, when using `write()` or `writelines()`, we have to specifically write a `\n` character.

> When trying to append some data to an already written file, if the file does not have a trailing newline `\n` character on the last line, the new appended data will be joined to the last written data to the file. 
> This is one reason why we usually include the `\n` character, even for the last line in our text file.

## Modules & Imports
What happens when amount of code becomes very *large*? (e.g. lots of functions)
- sometimes our code needs to grow beyond a single file or a Jupyter notebook
- need to *break up* code into *multiple* files
	- each file can group similar or related functionality together
	- code in one file (like a function) should be available to the other files
- in Python these code files are called *modules*
	- modules can be *nested* within other modules
		- modules that contain other modules are called *packages*
- creating packages is beyond the scope of this course
- but we should know what they are and how to use existing ones
#### Built-Ins
- Python has many *built-in* object types and functions
	- `bool`, `int`, `float`, `str`, `list`, `tuple`, `dict`
	- `print()`, `filter()`, `sorted()`, `zip()`, `len()`, etc
- these are baked right into Python
	- they're always available
		- we don't have to do anything special to use them

> Check out Python built-in library here: https://docs.python.org/3/library/functions.html

#### Standard Library
- Python has a lot of libraries (modules and packages) that come *standard* with base Python installation
- we have to specifically tell Python we want to use them
	- we "load" them using an `import` statement
	- why not just load (import) everything always?
		- there is a ton of libraries
		- do you really want to load up thousands of libraries into memory for things you don't even need?
		- other reasons too, which we'll see as we work with packages during the remainder of this course
---
- Python provides a huge selection of libraries (modules and packages) that cover things like:
	- numerical and math
		- math functions
		- stats functions
		- random functions
		- Decimal objects (alternative to floats)
	- date and time
	- CSV files
	- cryptography
	- networking, internet and many, many more...

> Check out Python standard library here: https://docs.python.org/3/library/index.html

#### 3rd Party Libraries
- sometimes the standard library is insufficient or too cumbersome
	- standard library has to be as generic as possible
	- it may provide the basic building blocks to do something
	- but you may need to write a lot of functions/code to tie them together
- many developers create libraries that leverage the standard library (or other 3rd party libraries), but provide a higher level, easier to use interface to more specialized functionality
- sometimes 3rd party libraries have a very narrow and specific focus
	- performance or advanced functionality might be one reason
		- `NumPy`
		- `SciPy`
		- `QuantLib`
		- `Pandas`
		- `Matplotlib`
		- `scikit-learn`
---
- we can *install* those libraries, and `import` them like any package
- where do you find a list of available 3rd party libraries
	- most exhaustive source is *PyPI* (*Py*thon *P*ackage *I*ndex) --> https://pypi.org
	- they can be installed using `pip install` like we saw in the beginning
	- more than 220,000 libraries
---
- how to find the "good" ones?
	- read blogs, posts, books, web sites and see what other people are using
	- does it have a good documentation?
	- is it still actively developed?
	- is it widely used?
- but maybe your niche is extremely specific and very niche
	- maybe something not so great is there that will work as a starting point
But don't just look for a 3rd party library for everything you write!
- if 3rd party library is full of bugs and unsupported, life will be painful!
- write your own code - often it's far simpler!
---
- here, we only cover a few specialized 3rd party libraries
- Official [Python docs](https://docs.python.org/3/index.html) and [library docs](https://docs.python.org/3/library/index.html) are your best source of information
	- blog posts and similar online resources can be very helpful (unless they're just plain wrong!)
	- [stackoverflow](https://stackoverflow.com)h is a fantastic resource for getting questions answered
		- so is AI
			- [Claude](https://claude.ai)
			- [Perplexity](https://perplexity.ai)
	- but at some point you'll need to look into the official docs

### Basic Imports
- Python has quite a few built-in functions and data types (classes) --> https://docs.python.org/3/library/functions.html
	- built-ins are always available
		- they are essentially "pre-loaded"
		- but there's a lot more in Python's "standard library"
			- way too much to load everything all the time
			- even more so with 3rd party libraries
			- so we need to load those as needed
	- all this functionality is split up into *separate* modules or packages
		- think of a module as a code file
		- we then load just the modules we need
#### Loading a Module
- modules are just objects
	- we need to "create" the object
	- we need to assign a symbol (variable) to that object
	- we can then use the variable to reference the module object
		- which has properties, functions, other objects
the `import` statement is used to both:
- load the module (create the module object)
- assign a symbol to that object

Example:
```python
import math
```
- `math` is a module in the standard library for math related functionality
- the `math` module has been loaded (from file) 
- and variable (symbol) `math` is a reference to that module object
- `math` contains many functions, such as `sqrt`
- like with any object, we use *dot notation* to reach inside the object
![modules and imports - dot notation](../assets/Pasted%20image%2020250131153929.png)

> In general it is customary not to rename (alias) a module, unless there are reasons to do so - like trying to import two modules from different libraries that might be named the same.
> 
> As always there are exceptions to that rule, and you'll often see people import some of the standard libraries such as `numpy` using `np` as an alias, or `pandas` using `pd` as an alias.
> 
> These are pretty much widely known and accepted conventions, so that's OK. But otherwise, using a custom alias might make your code harder to read, so use wisely.
> 
> For example using `m` as an alias to `math` like I did earlier is probably a good example of what **not** to do!

> One thing you will notice when you compare the Python standard library vs 3rd party libraries is that Python tends to keep modules/packages very flat - not a whole lot of nested modules.

#### Aliasing
```python
import some_module
```
- loads `some_module`
- creates a variable of the *same name* that references that module
what if we wanted to make that module symbol something else ?
```python
import some_module as sm
```
- load `some_module`
- creates a variable `sm` that references the `some_module` object

### Import Variants
- so far we've seen two variants of the `import` statement
	- `import some_module`
	- `import some_module as alias`
- if we want to use something inside the module we have to use the dot notation 
	- `fractions.Fraction(1, 2)`
	- `fractions.Fraction(1, 4)`
- what if we just want to use `Fraction` inside `fractions`
	- can we avoid using `fractions.Fraction` all the time?
	- yes!
---
- we can import symbols from a module directly into the corresponding symbols in our code
```python
from fractions import Fraction
```
- the `fractions` module is *loaded*
	- but the symbol `fractions` is NOT added to our local variables
	- instead the `Fraction` symbol is added to our local variables
		- references the `Fraction` property inside the `fractions` module
```python
f1 = Fraction(1, 2)
```
---
- can do the same with any module
```python
from math import sqrt
sqrt(2)
```
- what if we want more than one attribute from the module?
```python
from math import sqrt, pi, factorial
```
- `sqrt`, `pi`, `factorial` are now available as symbols in our local scope
```python
sqrt(2)
2 * pi
```

> When we write:
```python
from math import sqrt
```
> the **entire** `math` module is **still** loaded into memory - it's just what variables are put into our namespace that change.

## Dates and Times
#### Fundamental Concepts
- time zones and UTC
- epoch times
- times without dates
- dates without times
- dates with times
- ISO 8601 Standard
#### Coordinated Universal Time
- *UTC*
	- sometimes still referred to as *GMT* (Greenwich Mean Time)
	- world standard
	- no adjustment for daylight saving time
	- easiest is to *always* use UTC *internally* in our programs
		- *convert* incoming times to UTC
		- work exclusively in UTC *internally*
		- *display* to user using their preferred *time zone*

![how datetime data is parsed is usually parsed in our program](../assets/Pasted%20image%2020250202144449.png)
#### Challenges with External Sources of Time Data
- Python has special data types, for `time`, `date`, and `datetime`
- *external* sources of time data usually given as *strings*
	- it is a *visual* (string) *representation* of a date/time
	- but what *format*?
		- `3/1/2020 2:35:01 pm` --> is this *March 1*, or *January 3*?
		- `March 1, 2020 14:35:01`
		- `03-01-2020 02:35:01 PM`
	- what *time zones*?
		- may or may not be specified, using different "standards"
		- how do we convert these to *UTC* based date/times in our apps?
		- what formatting should we use?
#### ISO Format
- *ISO 8601* defines *standards* for string representations of dates and times
![iso format standard](../assets/Pasted%20image%2020250202145413.png)
---
why are time zones important?
*May 1, 2020, 10:23:35am* in *Eastern Time*
- daylight savings time in effect (*EDT*) --> `2020-05-01T10:23:35-04:00`

*December 1, 2020, 10:23:35am* in *Eastern Time*
- daylight savings time NOT in effect (*EST*) --> `2020-12-01T10:23:35-05:00`
---
- keeping track of all this in calculations is difficult!
- convert to UTC first
	- `2020-05-01T10:23:35-04:00` --> `2020-05-01T14:23:35Z`
	- `2020-12-01T10:23:35-05:00` --> `2020-12-01T15:23:35Z`
- then convert to whatever time zone for display purposes to user (if necessary)
---
- Python has a lot of functionality for calculations with dates and times
	- to minimize introducing bugs, always use UTC based times
	- but converting these "input" times to UTC is difficult!
		- can be done using Python and the standard library
		- much easier to leverage 3rd party libraries for this:
			- `dateutil` - easier to deal with parsing datetime strings
			- `pytz` - easier to deal with parsing time zones
#### Epoch Time
- we saw that dealing with date/times involves time zones (whether UTC or something else)
- introduced by Unix as a way to define a datetime without using timezones
	- start with a base datetime
		- the *epoch*
	- given a datetime, calculate it as the *difference* in seconds from the *epoch*
	- also called Unix or POSIX time
		- epoch is system dependent
		- Usually: `January 1, 1970 00:00:00 UTC`
			- `2020-05-01T10:23:35-04:00` --> `1588343015.0` (the datetime presented: the difference between the date to default epoch time (January 1, 1970) is the value in seconds - `1588343015.0`)
			- but if ingesting datetime information that use epoch times, you need to know the epoch!
#### The `time` Module
- used for *time* manipulations
	- mostly uses epoch times
	- we won't use this much
- but it also includes some useful functions
	- `sleep`
	- `perf_counter`
#### The `datetime` Module
- used for `date`(only), `time`(only), `datetime`(date with time) objects
- can handle time zones
- provides *formatting* and *parsing* capabilities
- defines a `timedelta` data type (class)
	- used to represent time *difference* between two date/time objects

### The `time` Module
#### `perf_counter`
- `perf_counter` is used to measure *elapsed* time in (float) seconds
	- from some *undefined* start (0) (usually when program starts running)
	- always look at *difference* between calls to `perf_counter`
	- uses a clock with highest available precision
```python
from time import pert_counter
t1 = perf_counter()
t2 = perf_counter()
elapsed = t2 - t1
```
#### `sleep`
- `sleep(n)` is used to *pause* execution for (float) `n` seconds
	- why would you want to slow your program down ??
		- give time for something else to finish 
			- usually some external resource 
			- maybe a network connection is temporarily down
				- retry connecting a few times but wait in-between retries
#### Getting the *epoch*
- Unix systems use *January 1, 1970, 00:00:00 (UTC)*
```python
time.gmtime(n)
```
- returns a time object (`struct_time`)
- based on `n` seconds elapsed from *epoch*
- has the following properties:
	- `tm_year`
	- `tm_mon`
	- `tm_mday`
	- `tm_hour`
	- `tm_min`
	- `tm_sec`
- ignores fractional seconds (if float)
---
- to find the `epoch` on your system
```python
time.gmtime(0) # returns :
# struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, ...)
```
#### Getting the Current *epoch* Time
```python
time.time() # returns the current time (in seconds) since the epoch
```
- get UTC `time_struct` from that...
```python
time.gmtime(time.time())
```
#### Converting from `time_struct` to epoch Time
- `gmtime()` converts an *epoch time* `n` to a `time_struct`
- can also convert a `time_struct` back to an *epoch time*
```python
calendar.timegm(time_struct)
```
- `timegm` is the inverse of `gmtime`
- it is located in the `calendar` module
```python
from calendar import timegm
n = 1_000_000_000
t = gmtime(n) # struct_time(tm_year=2001, tm_mon=9, ...)
timegm(t) # 1_000_000_000
```
#### Formatting *epoch* time to human readable string
- if we show someone an epoch time (a float), that does not mean much to them
	- as humans we are used to certain formats for the date and time
	- use the `strftime(format, time_struct)` function (*str*ing *f*ormat *time*)
	- *format* is a *string* that contains special formatting directives
- for example, suppose we have an epoch time: `1587253022` (which is actually `2020-04-18 23:37:02`) 
	- we can format this time into *April 18, 2020* as follows:
```python
t_struct = gmtime(1587253022)
strftime("%B, %d, %Y", gmtime(t_struct))
```
---
> here are a few more format directives: https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes

See some below:

`%Y` --> four digit year
`%y` --> two digit year

`%m` --> month number
`%B` --> month full name
`%b` --> month abbreviated name

`%d` --> day of the month name

---
`%H` --> hour in 24-hour clock
`%I` --> hour in 12-hour clock
`%p` --> AM or PM

`%M` --> minute number

`%S` --> second number

`%z` --> time zone offset `+/-HH:MM`
`%Z` --> time zone name

---
`%w` --> weekday number (Sunday=0)
`%A` --> weekday full name
`%a` --> weekday abbreviated name

---
for example:
- epoch time `t = 1587253022` (2020-04-18T23:37:02)
```python
from time import strftime, gmtime
t_struct = gmtime(t)

strftime("%Y-%m-%dT%H:%M:%Sz", t_struct) # '2020-04-18T23:37:02z'
strftime("Today is %A, %B, %d, %Y", t_struct) # 'Today is Saturday, April 18, 2020'
strftime('Time: %I:%M %p %Z', t_struct) # 'Time: 11:37 PM UTC'
```
#### Parsing Date/Time Strings
- this is the *reverse* of the formatting we just saw
	- given a string such as `"04/18/2020 11:37:02 PM"`
		- "convert" it to an epoch time 
- we'll assume the time was given in UTC (since no indication was given)
- also assume format is Month/Day/Year (not Day/Month/Year)
	- in this case we can safely assume this, since there is no month 18
	- not always that lucky!
- we need to tell Python *what to expect* in the string, using same *directives* as before
	- `time.strptime(date_string, format)` - (*str*ing *p*arse *time*)
```python
from time import strptime

s = "04/18/2020 11:37:02 PM"

strptime(s, "%m/%d/%Y %I:%M:%S %p") # returns :
# time.struct_time(
					# tm_year=2020,
					# tm_mon=04,
					# tm_mday=18,
					# tm_hour=23,
					# tm_min=37,
					# tm_sec=2,
					# tm_wday=5,
					# tm_yday=109,
					# tm_isdst=-1,
#)
```
---
- for every date/time formatting variant, we have to specify the `format` to parse it
	- `4/18/20 23:45:34` --> "%m/%d/%y %H:%M:%S"
	- `18/04/20 11:45:34 PM` --> "%d/%m/%y %I:%M:%S %p"
	- `20/4/18 11:45:34 PM` --> "%y/%m/%d %I:%M:%S %p"
- this can get difficult
- especially if our various data sources use a *mixture* of formats
- this is where 3rd party libraries, such as `dateutil` can help
	- we'll come back to this later...

> `gmtime` ignores the non-integer portion (fractional seconds) of the argument.
> 
> We can access individual fields of this `time_struct` structure, using either positional indexes, or the property names (this structure is something called a [[advanced_python-functional.md#named tuples|named tuple]] - a tuple of values, but where each element of the tuple can be accessed by name also).
```python
from time import gmtime, time
current = gmtime(time())
current[0], current.tm_year # 2025, 2025
```

> A list of all the supported formatting directives for dates and times here: https://docs.python.org/3/library/time.html#time.strftime
> 
> Some additional directives you can use, here: https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior

### The `datetime` Module
- the `time` module is a low-level library
	- good for working with *epoch times*
		- but a bit cumbersome
		- not a ton of functionality
- instead use the `datetime` module
	- isolates us from epoch times (used internally)
	- provides handy data types (classes)
		- `date`
		- `time`
		- `datetime`
		- `timedelta`
		- `timezone`
#### `datetime.date`
- `date` is a data type (class) for working with pure dates (no times)
```python
from datetime import date
date(year, month, day)
```
(or import `datetime` module, and use *fully qualified* names)
```python
import datetime
datetime.date(year, month, day)
```
- properties:
	- `year`
	- `month`
	- `day`
---
- `date.today()`
	- returns local date as a `date` object
- `<date_obj>.isoformat()`
	- returns an ISO 8601 string for the `date` object
- `date.fromisoformat("iso formatted date string")`
	- parses and creates a `date` object from an ISO formatted date string
#### `datetime.time`
- `time` is a data type (class) used to work with pure times (no date)
	- it can be time zone *naive* or *aware*
- `time(hour, minute, second, microsecond, tzinfo`
	- properties:
		- `hour, minute, second, microsecond`
		- `tzinfo` --> `None` for naive times
	- `time.fromisoformat(s)`
	- `<time_obj>.isoformat()`
#### `datetime.datetime`
- class that supports *both* date and time
```python
from datetime import datetime
datetime(year, month, day, hour, minute, second, microsecond, tzinfo)
```
- properties for `year`, `month`, ...
- `datetime.dateime.fromisoformat(s)`
- `<datetime_obj>.isoformat()` --> converting "to iso format"
- `datetime.datetime.utcnow()`
	- returns *naive* local date/time in UTC

>  As we work with dates and times in our program, everything is in UTC, we do not have to worry about time zones and daylight savings, and we only convert to some other time zone when we want to display a date/time to our users in their local time zone - and usually that's up to the UI to do this.
>  
>  The same idea goes for storing data (in a database, file, etc.) - we always store these dates in UTC (whether aware or naive will depend on your storage solution and particular circumstances).

> To get date/time/datetime from the epoch time, we use: `datetime.date.fromtimestamp(time.time())`

> We can also convert an ISO formatted day without resorting to parsing directives.
```python
datetime.date.fromisoformat('2020-12-31') # datetime.date(2020, 12, 31)
```
> We can do the reverse of that and produce an ISO formatted date string.
```python
dt = datetime.date(2020, 12, 1)
dt.isoformat() # '2020-12-01'
```

### Date Arithmetic
- date arithmetic mostly involves working with
	- `date`, `time`, `datetime`
- time *durations* (e.g. 1 day and 2 hours and 30 minutes and 15 seconds)
- `datetime` module has a special class for durations
	- `timedelta`
	- subtracting one date/time from another results in a `timedelta`
	- can add or subtract a `timedelta` from a date/time
#### `datetime.timedelta`
```python
from datetime import timedelta
timedelta(
			days,
			seconds, microseconds, milliseconds,
			minutes,
			hours,
			weeks
)
```
- arguments are optional and default to `0`
- argument values are additive
	- `timedelta(days=1, hours=1)`
		- duration of 1 day and 1 hour
		- 25 hours
---
- notice there is no *month* argument
	- what does it mean to add a month to a date ???
		- 31 days, 30 days, 29 days, 28 days ???
```
1/15/2020 + 1 month --> 2/15/2020 ?
1/31/2020 + 1 month --> 2/31/2020 ??
```
---
- most arguments in `timedelta()` are for *convenience*
	- internally `timedelta`objects store the values in days, seconds, and microseconds
- properties:
	- `.days`
	- `.seconds`
	- `.microseconds`
- `<timedelta_obj>.total_seconds()`
	- returns the total number of seconds (fractional duration) in duration

### Naive and Aware Times
- *aware* time
	- time that has a time zone attached to it
- *naive* time
	- no time zone info
- to simplify our coding life, we made two decisions:
	- all times we create or work with will be:
		- naive
		- in UTC
- the idea is that any datetime we ingest, immediately gets transformed into a native UTC datetime 
- convert to aware non-UTC for display or output purposes only
#### `timezone` Definition
```python
datetime.timezone
```
- class to *define* a time zone
	- name (optional)
	- UTC offset
		- defined as a `timedelta` object
- how is that offset defined exactly?
- time zone offset defines the number of hours or minutes that should be *added to or subtracted from* the corresponding UTC time
- if a time zone is 4 hours behind, then the offset is `-4 hours`
```python
tz_EDT = timezone(timedelta(hours=-4), 'EDT')
```
- pre-defined UTC time zone: `timezone.utc`
#### Aware `datetime`
how do we create aware datetimes? Using either of these two ways
```python
from datetime import datetime, timezone, timedelta

d1 = datetime.fromisoformat('2020-05-15T13:30:00-05:00')
```
```python
tz_EDT = timezone(timedelta(hours=-4), 'EDT')

d2 = datetime(year=2020, month=5, day=13, hour=13, minute=30, second=0, tzinfo=tz_EDT)
```
#### Converting from One Time Zone to Another
- if we have an aware `datetime`, we can easily *change* it to another *time zone*
	- use the `.astimezone(target_tz)` method of the `datetime` object
```python
d1 = datetime.fromisoformat('2020-05-15T13:30:00-04:00')

tz_CDT = timezone(timedelta(hours=-5), 'CDT')

d1.astimezone(tz_CDT) # datetime(2020, 05, 15, 12, 30, 00, tzinfo=tz_CDT)
d1.astimezone(timezone.utc) # datetime(2020, 05, 15, 17, 30, 00, tzinfo=timezone.utc))
```
- notice that the `datetime` objects remain *aware*
#### Adding or Removing Time Zone
- careful! Do not remove the time zone from a non-UTC timestamp!
	- unless you know what you're doing and this is intentional
- ok to remove from a UTC aware timestamp - since we assume everything is UTC
- to make a UTC aware timestamp naive, just replace the `tzinfo` value with `None`
- to add a time zone to a naive timestamp, replace `tzinfo` with appropriate `timezone`
- use the `.replace()` method on `datetime`objects
#### The `replace()` Method
- if `dt` is some `datetime` object
	- create a new `datetime` object with the exact same values:
		- `dt_copy = dt.replace()`
	- or replace one or more values while we do the copy
		- `dt_copy = dt.replace(year=2021, hour=0)`
	- in particular, we can do that with the `tzinfo` value
		- `dt.replace(tzinfo=None)`
		- `dt.replace(tzinfo=timezone.utc)`
#### Daylight Savings Time
- many places change their clock twice a year - daylight savings time
	- *not everyone* does
		- Most parts of Arizona do not, but some do!
	- *not* everyone does it at the *same time* 
	- *not* everyone changes by the *same amount* 
	- when and how much has *changed over the years* for the same places
- so, how do we convert a UTC datetime into some specific time zone?
	- it must take all these things into account
		- difficult!
	- Olson Database (or IANA time zone database)
		- https://en.wikipedia.org/wiki/Tz_database
	- the [`pytz`](#The%20`pytz`%20Library) 3rd party library (we'll come back to this later)

### Custom Representations
- recall the `time` module
	- `strftime()`
		- format a time struct using formatting directives
			- `strftime('Time: %I:%M %p %Z', t_struct)`
	- `strptime()`
		- parse a datetime into a struct using formatting directives
			- `strptime(s, "%m/%d/%Y %I:%M:%S %p")`
---
- `strftime` is available for:
	- `datetime.time`
	- `datetime.date`
	- `datetime.datetime`
- `strptime` is available for:
	- `datetime.datetime`
- uses the *same* special formatting directives

> The `strptime()` method in `datetime.datetime` object comes handy when we need to parse an ISO format that is not formatted in Python way. This is an example:
> 
> When we parse the `datetime` ISO format:
```python
datetime.fromisoformat('2020-05-15T22:30:00-05:00') # returns ...
# datetime.datetime(2020, 5, 15, 22, 30, tzinfo=datetime.timezone(datetime.timedelta(days=-1, seconds=68400)))

# that worked just fine, but recall that the `:` in the time zone offset is actually **optional** in the ISO 8601 specification
```
> That worked, so no problems. But when we parse a datetime in another valid ISO format that is not formatted for Python such as:
```
'2020-05-15T22:30:00-0500'

as well as this:

'2020-05-15T22:30:00-05'
```
> If we try to parse those examples, we'll get an `Invalid isoformat string` `ValueError` message. This is why the `strptime()` method in `datetime.datetime` is useful. That said, there are 3rd party libraries such as [`dateutil`](#The%20`dateutil`%20Library) that help us handle this much better.

## CSV Module
- [earlier](#Reading%20Text%20Files) we saw how to read and write text files
	- we even parsed some data from a simple CSV file
- but CSV formats *vary*, so more complicated than that simple example
	- often called CSV *dialects*
- would require a lot of manual work on our part to deal with all these variants
- `csv` module provides functionality to read and write a wide variety of CSV formats
	- including tab delimited, pipe (`|`) delimited
	- can deal with different line separators
		- Unix and Windows line separators are different
			- `\n` in Unix
			- `\r\n` in Windows
### Reading CSV Files
#### What is CSV Data?
- CSV also known as **Comma Separated Values**, is a format for tabular data
	- rows and columns
- basic idea:
	- each row in a file is a row of data
		- rows in files are separated by a *newline* (OS Specific)
		- each field in the row is separated by a *separator* aka *delimiter*
- but that brings up a few things...
	- what field separator to use?
		- comma?
		- yes, but not necessarily
	- how to deal with a field containing a comma (or whatever separator)?
---
```csv
FULL_NAME,DOB,SSN
Smith, John,3/1/1985,123-456-789
```
- `Smith, John` is actually a single field but the `,` inside is going to cause problems
- use some *delimiter*
	- maybe double quotes
	- but doesn't have to be!
```csv
"Smith, John","3/1/1985","123-456-789"
```
- but we don't *need* the delimiters around the DOB or SSN
	- `"Smith, John",3/1/1985,123-456-789`
---
- what if field contains the field delimiter character?
```csv
"Doyle, Conan","First Holmes book was the "Scarlet Letter""
```
- double up the quotes
```csv
"Doyle, Conan","First Holmes book was the ""Scarlet Letter"""
```
- or use a prefix character to "escape" the next character
	- e.g. `\` like Python (`\n`, `\t`, etc)
		- but doesn't have to be!
```csv
"Doyle, Conan","First Holmes book was the \"Scarlet Letter\""
```
- as you can see there can be different ways of approaching this
#### CSV is not a standard format
- unfortunately CSV is not exactly a standard
	- a variety of flavors exist
		- *dialects*
		- most common one is Excel
			- `delimiter` (field separator) --> `,`
			- `quotechar` (field delimiter) --> `"`
			- *doubles* `quotechar` if found inside a field
			- only uses `quotechar` if delimiter is found inside a field
- but there are other valid CSV formats too
	- `field1|field2|field3`
		- *pipe* (`|`) delimited
	- `field1  field2  field3` (spaced using tab character)
		- *tab* delimited
#### Parsing CSV Data
- *default* parser dialect is `excel`
	- but we can specify custom settings for `delimiter`, `quotechar`, etc
```python
csv.reader(f, delimiter=',', quotechar='"')
# `f` : an open file to read from
# `delimiter` : optional, defaults to `,`
# `quotechar` : optional, defaults to `"`
```
- returns an *iterator* of parsed rows over the file
```python
with open('some_file') as f:
	reader = csv.reader(f) # default uses , and "
	for row in reader:
		# row is a list containing parsed fields
```

> Check out the CSV module in Python Standard Library: https://docs.python.org/3/library/csv.html

> The basic premise of CSV format is:
> 1. each row is a record
> 2. first record may contain the field names
> 3. each row (ending with a `\n` or `\r\n`) represents a single record
> 4. fields can be separated by an arbitrary character or set of characters (but it is consistent for the entire file)
> 5. text fields may themselves be delimited by some arbitrary delimiter (usually single or double quotes)

> The primary function to read a CSV file, is the `reader` function. It returns an iterator that can be used to iterate over the rows (records) one by one. In general, we need to give this `reader`:
> - an open file to use
> - what `delimiter` is used for field separators
> - what character (`quotechar`) is used for delimiting text fields when necessary
```python
import csv

# open file using a context manager
with open('actors.csv') as f:
	reader = csv.reader(f, delimiter=',', quotechar='"')
	for row in reader:
		print(row)
```
> There are a lot more parameters that `reader` supports, such as:
> 
> - `skipinitialspace` (in case a space is after the delimiter - if `True` (the default), it just ignores it)
> - `doublequote` and `escapechar` - can control how `quotechar` characters inside a field should themselves be quoted or escaped.
> 	- we'll come back to this in the context of writing CSV files
> 
> Basically, different variants of CSV files will require possibly different settings.

### Dialects
- previously, we saw that we can define all kinds of settings to specify the CSV format
- works, but if we need to repeat the same settings often
	- tedious typing the same code over and over again
	- error prone - might forget or mis-type one of the settings
- instead we can bundle up all the settings into a custom *dialect*
	- basically just a way to package the settings once in our program
	- and re-use elsewhere in the same program multiple times
#### Listing Available Dialects
- `csv` module comes with some pre-defined dialects
	- `excel`
	- `excel-tab` (for tab delimited values)
- we can add our own to that list
	- register a dialect with
		- a *name* for the dialect
		- *values* for `delimiter`, `quotechar`, etc.
```python
csv.register_dialect("<name>", delimiter='...', quotechar='...', ...)
```

> The dialect gets registered while your program is running. When you start your program and rerun your program, your dialect doesn't exist anymore. You have to recreate and register your dialect. 

#### Using a Defined Dialect
- we can specify a dialect instead of individual values for `csv.reader`
```python
csv.reader(f, dialect='excel')
```
- `excel` is the *default* for dialect
	- same as `csv.reader(f)`
- or we can specify our custom dialect we registered
```python
csv.reader(f, dialect='my-custom-dialect')
```

> We can list available `csv` dialects by running
```python
csv.list_dialects()
```

> In Python, as is with most programs including Linux, `\` in a string literal "escapes" the next character - i.e. gives it a special meaning:
```python
print("1\t2\t3\t4")
# 1    2    3    4   
```
> As you can see `\t` is interpreted as a tab character. So, we cannot just use `\` by itself in a string, to actually define a `\` character in a string literal we can use `\\`:
```python
print('\\') # \
```

> This is the `csv` dialect to parse 'pdv' (pipe delimited value) files:
^503740
```python
csv.register_dialect(
	'pdv',
	delimiter='|',
	quotechar="'",
	skipinitialspace=True,
	escapechar='\\'
)

# check if the newly-created dialect is in the dialect list
csv.list_dialects() # ['excel', 'excel-tab', 'unix', 'pdv']

# now when using csv.reader you can specify this dialect like so:
with open('actors.pdv') as f:
	reader = csv.reader(f, dialect='pdv')
	for row in reader:
		print(row)
```

### Writing CSV Files
- reverse of reading and parsing a csv file 
- given some data, write it out to a csv file
	- an iterable of rows
		- each row is itself an iterable of fields (columns)
- just like reading a CSV file, we can specify formatting options
	- either by using individual values (`delimiter`, `quotechar`, etc.)
	- or using a *dialect* (built-in or custom)
- unless there are some reasons not to, just use the standard `excel` dialect
#### Writing a CSV File
- use `csv.writer`
	- then use the `writerow` method to write out each row in your data
```python
with open('<file_name', 'w') as f:
	writer = csv.writer(f, dialect='...')
	for row in data:
		writer.writerow(row)
```
- where `data` is an iterable containing iterables of fields
```python
data = [
	[row1_col1, row1_col2, ...],
	[row2_col1, row2_col2, ...],
	...
]
```
- if you want a header row, write that out too using `writerow` and an iterable of headers 

> The `csv.write` function, similarly to the `csv.reader` function expects the file we want to write out to as its first argument, then we can specify a dialect (default is `excel`), and/or specify specific CSV settings.

> We can specify a different dialect if we wanted to, like that `pdv` dialect we [defined earlier](#^503740)
```python
csv.register_dialect(
    'pdv', 
    delimiter='|', 
    quotechar= "'", 
    escapechar="\\",
    doublequote=False # this enforces the csv.writer to use the `escapechar` instead of the `double-doublequote` for `quotechar`
)
```

## Random Module
- `random` module
- random number generator
	- integer, floats
	- pseudo random
	- actually generated by an *algorithm*
		- gives the appearance of random number generation (uniform distribution)
		- Mersenne Twister algorithm
	- *PRNG* (*p*seudo *r*andom *n*umber *g*enerator)
	- deterministic generator
		- we know ahead of time what the sequence will be
		- not suitable for security / cryptography for example
		- but suitable for most other purposes
---
- doesn't deterministic algo defeat purpose of generating random numbers?
	- goal is to generate a sequence of numbers that
		- is uniformly distributed
		- appears random to user
- but if the sequence is the same every time?
	- that's a good thing when testing code
		- testing and debugging random things is difficult
	- we can make it so the generated sequence is *not* the same every time program runs
	- *seed* value
		- every different seed results in a different sequence
		- use a different seed every time program starts
		- Python does that for us (uses epoch time)
---
- beyond uniformly distributed PRN
	- random number generator using various distributions (normal, lognormal, triangular, beta, gamma, and more)
	- shuffle a sequence of elements
	- random sampling
		- *without* replacement
			- choose 5 cards from a deck of 52
		- *with* replacement
			- roll two die (each of which can be 1-6)
			- pick two elements with replacement from {1, 2, 3, 4, 5, 6}
#### Interval Notation
`[a, b]` --> `a <= x <= b`
`(a, b)` --> `a < x < b`
`(a, b]` --> `a < x <= b`
`[a, b)` --> `a <= x < b`
- `[` includes endpoint
- `(` excludes endpoint

### Random Numbers
#### Random *seed*
- *seed* is used as a "primer" for different random number sequences
	- Python automatically sets one based on system time
		- so every time our program restarts we get different sequences of random numbers
	- we can *override* the seed value
		- useful to guarantee repeatability of "random" sequence
			- testing, debugging
```python
random.seed() # uses system (epoch) time
random.seed(a) # uses value a (system time if `a` is `None`)
```
#### The base PRNG
- there is a *single* pseudo random number generator
	- - generates and returns the next PRN
	- `float` in `[0.0, 1.0)`
	- uniformly distributed
```python
random.random()
```
- call it repeatedly to get the next number, and the next...
- other random related functions
	- all use this one at their base 
	- random integer generator
	- random numbers that will display certain distributions (e.g. normal)
	- shuffling, sampling
- all use `random()`
	- all display *same repeatability* for *same seed*
#### Generating Random Integers
```python
randrange(stop) # generates random integers in `range(stop)`
randrange(start, stop, step) # generates random integers in `range(start, stop, step)`
```
```python
randint(a, b)
```
- generates `int` in `[a, b]`
- equivalent to `randrange(a, b+1)`
	- syntax convenience
---
- uniform distribution
- call repeatedly to produce a sequence of random integers
#### Generating Random Floats
- `random()`
	- random `float` in `[0.0, 1.0)`
	- uniform distribution
- `uniform(a, b)`
	- random `float` in `[a, b]`
	- uniform distribution
- `gauss(mu, sigma)`
	- random float
	- normal distribution
		- mean = `mu`, std deviation = `sigma`
and many more - see the online docs : https://docs.python.org/3/library/random.html

> To use the `random` module, you have to import it.
```python
import random
```

### Sampling and Shuffling
#### Shuffling
- *in-place* shuffle of items in a *mutable* sequence
```python
from random import shuffle

l = [1, 2, 3]
shuffle(l) # [3, 1, 2]
```
- `l` was *mutated*
#### Choosing a Single Random Element
- `choice(seq)`
	- chooses a single random element from `seq`
	- `seq` can be any sequence type (even immutable)
	- does not modify `seq` in any way
```python
from random import choice

l = [1, 2, 3, 4, 5]

choice(l) # 3
choice(l) # 5
choice(l) # 3
```
- `choice()` uses unform distribution
#### Choosing Multiple Random Elements at a Time 
- `choices(seq, k=...)`
- choose `k` random elements from some sequence `seq` (*uniform* distribution)
- *with replacements*
	- the same element may get picked more than once in each set of `k` elements
- returns result as a list of `k` elements
```python
from random import choices

l = 1, 2, 3, 4, 5, 6

choices(l, k=2) # [6, 5]
choices(l, k=2) # [1, 3]
choices(l, k=2) # [2, 2]
```
- `k` can be larger than sequence *length* (*guaranteed* to have repeated elements!)
#### Sampling a Population
- `sample(population, k)`
	- population can be a *sequence* or a `set`, and even a `range` object
	- choose `k` random elements from some `population` (*uniform* distribution)
	- *without replacements*
		- the same element *cannot* be picked twice in each set of `k` elements
		- as you can tell `sample()` is similar to `choices()` only difference is:
			- `choices()` returns *with replacements*
			- `sample()` returns *without replacements*
	- random sampling
		- `k` is the *sample size*
	- returns result as list of `k` elements
	- `k` cannot exceed `len(seq)`
		- `ValueError` otherwise
#### Weighted Choices
```python
l = [1, 2, 3, 4, 5, 6, 7, 8]

choices(l, k=3) # `k` is a kw-only arg 
```
- a list of `k` random elements from `l` 
- with replacement
- uniform distribution
	- for each pick of an element to include in the `k` choices
	- every element has the *same probability* of being picked
---
- but we can change those probabilities
	- by specifying a sequence of *weights* to assign to each element of the sequence
	- if specified, `len(weights)` *must equal* `len(sequence)` 
```python
l = [1, 2, 3, 4, 5, 6, 7, 8]
weights = [1, 1, 1, 1, 2, 1, 1, 1]

choices(l, weights=weights, k=3)
```
- at every pick of `k` elements
	- *`5`* has two times chances of being picked than all other elements
- weights can be floats too
- no longer a uniform distribution

> Sampling can be quite useful when you are dealing with a huge dataset, but want to perform some quick calculations based on a smaller random sample (or like estimating the population mean with a sample mean).

> Running the `random.sample()` function on a `set`, I received this error message from Python on Jupyter Notebooks: 
> `DeprecationWarning: Sampling from a set deprecated since Python 3.9 and will be removed in a subsequent version.`

## Math and Statistics Modules
- already seen `math` module
	- look at a few more functions in that module
- `statistics` module
	- variety of simple stats
		- means, variances
		- normal distributions
### `math` Module
- `factorial(n)` --> factorial function
- `perm(n, k)` --> permutations
- `comb(n, k)` --> combinations
- `gcd(a, b)` --> greatest common divisor of integers `a` and `b`
- `fsum(iterable)` --> floating point sum, more accurate than `sum()`
- `prod(iterable, *, start=1)` --> product of all elements in iterable
---
- `dist(p, q)` --> Euclidean distance between `p` and `q` (iterables)
- `hypot(*coords)` --> Euclidean norm of vector with specified coordinates
- `sqrt` --> square root
- `exp(x)` --> exponent (`e ** x`)
- `log(x)` --> natural log (base `e`)
- `log10(x)` --> log base `10`
- `e` --> Euler's constant
---
- `degrees(x)`, `radians(x)` --> degree/radian conversions
- `sin(x)`, `cos(x)`, `tan(x)` --> trig functions
- `asin(x)`, `acos(x)`, `atan(x)` --> arc functions
- `sinh(x)`, `cosh(x)`, `tanh(x)` --> hyperbolic functions
- `asinh(x)`, `acosh(x)`, `atanh(x)` --> ac hyperbolic functions
- `pi`
---
> For full list of functions in `math` module: https://docs.python.org/3/library/math.html

> For `complex` number math, see `cmath` module: https://docs.python.org/3/library/cmath.html

> Use like so:
```python
import math
from math import sum, fsum, cos, sin, pi
```

### `statistics` Module
#### Measures of Central Location
- `statistics` module
- `s` is a non-empty sequence or iterable
- `mean(s)` --> arithmetic average of an iterable
- `fmean(s)` --> converts everything to `float`, then calculates mean (faster than `mean`)
- `median(s)` --> median (may not be an element of the iterable)
	- To ensure median is an element of the iterable use:
		- `median_low(s)`
		- `median_high(s)`
- `mode(s)` --> applies to numeric or nominal data
#### Measures of Spread
- `pstdev(s)` --> population standard deviation
- `pvariance(s)` --> population variance
- `stdev(s)` --> sample standard deviation
- `variance(s)` --> sample variance
- `quantiles(s, *, n=4, method='exclusive')`
	- `n=4` for *quartiles*, `n=100` for *percentiles*
	- `method='exclusive'/'inclusive'`
		- indicates if `s` is a sample that does / does not include most extreme population values
#### Normal Distribution
- `NormalDist` data type (class)
		- used to *create* and *manipulate* normal distributions of a random variable
```python
d = NormalDist(mu=0.0, sigma=1.0)
d.mean, d.median, d.mode, d.stdev, d.variance
```
- `d.pdf(x)` --> probability density function
- `d.cdf(x)` --> cumulative distribution function
- `d.inv_cdf(p)` --> inverse CDF (aka quantile function)
- `d.quantiles(n=4)` --> returns a list of `n-1` cut points for the quantiles
---
- `d.overlap(other_normal_dist)` --> calculates area overlap of two distributions
- `d.samples(n)` --> returns list of `n` random samples
- supports *arithmetic operations*
	- `+` or `-` with *constants* --> translate distribution
	- `*` or `/` with *constants* --> scales distribution
```python
d = NormalDist(0, 1)
d * 5 + 20 # NormalDist(20, 5)
```
---
- can also combine two normal distributions (`+`)
```python
d1 = NormalDist(1, 3)
d2 = NormalDist(2, 5)
d1 + d2 # NormalDist(3, 5)
```
- mean = sum of two means
	- `1 + 2` --> `3`
- variance = sum of two variances
	- `9 + 16 = 25` --> std dev = `5`

> For more functionality of the `statistics` module, check out the Python docs: https://docs.python.org/3/library/statistics.html#module-statistics

> Use like so:
```python
import statistics as stats # if you don't want to keep spelling `statistics`
```

## Decimal Module
- we have seen that floats do not have exact representations
	- most of the time that's not an issue
		- often deal transforming data
			- slight loss of precision, rounding errors, matter
			- but level of float precision is sufficient
- you may have cases where the loss of precision is unacceptable
	- you may have to store decimals *exactly*
	- addition, subtraction, multiplication have to be *exact*
		- division is going to suffer from rounding errors
			- `1/3 = 0.333...`
				- cannot store infinite decimal numbers
				- but what precision?
---
- `Decimal` objects can store decimal numbers exactly
	- but at what *cost*?
		- literals have to use strings to represent numbers - unwieldy
		- cannot use most `math` functions (they convert to floats)
			- many *specialized* math functions are defined in `Decimal`
		- arithmetic operations are *slower* than floats
		- they use *more memory* than floats

> Learn more about `Decimal` module by checking out Python docs: https://docs.python.org/3/library/decimal.html

- implements IBM General Decimal Arithmetic Specification standard https://speleotrove.com/decimal/decarith.html

### Decimal Objects
#### The Decimal Data Type
- `decimal` module
- `Decimal` data type (class)
- `Decimal(3)`
	- take the *integer* `3` and convert it to a `Decimal` object
- `Decimal(0.1)`
	- take the *float* `0.1` and convert it to a `Decimal` object
- do you see the problem here?
	- `0.1` is a `float`
		- it is *already inexact*, before we even pass it to `Decimal`
	- `Decimal('0.1')`
		- take the *string* `0.1` and convert it to a `Decimal` object
		- `0.1` will be stored exactly as `0.1` in the `Decimal` type
```python
0.1 + 0.1 + 0.1 == 0.3 # False
Decimal('0.1') + Decimal('0.1') + Decimal('0.1') == Decimal('0.3') # True
Decimal(1) / Decimal(3) # Decimal('0.3333333333333333333333333333')
```
- what precision?
	- default is `28` *significant digits*
	- we can override this value
#### Significant Digits
- number of digits needed to represent the decimal number
	- *leading* zeros are *ignored*
		- `001.2345` --> `5` significant digits
	- *trailing* zeros are *not*!
		- `1.2000` --> `5` significant digits
- important to understand how this affects *arithmetic operations*
```python
Decimal('0.15') * Decimal(2) # Decimal('0.30') not 0.3
Decimal('0.100') * Decimal('0.200') # Decimal('0.020000')
```
#### Rounding
- can use the `round()` function
	- it will use a special rounding method defined by Decimal objects
```python
round(Decimal('1.2335'), 3) # Decimal('1.234')
round(Decimal('1.2345'), 3) # Decimal('1.234')
```
- Banker's rounding (round to closest, ties to closest even)
	- default
	- we can specify other types of rounding
#### Arithmetic Contexts
- when we perform arithmetic operations on Decimal numbers
	- precision can affect results
	- rounding methodology can affect results

Example: suppose we are using a precision of `5`, and Banker's rounding
```python
d1 = Decimal('1.2335')
d2 = Decimal('122')
# 1.2335 * 122 = 150.3650
```
- `d1 * d2` --> `Decimal('150.36')`
	- only `5` significant numbers - so *had* to round to two decimals
	- used Banker's rounding --> `150.36`
---
- view your current context settings
	- `decimal.getcontext()`
		- prec = `28`
		- rounding = `ROUND_HALF_EVEN` (Bankers rounding)
- later we'll see how to modify the arithmetic context
**IMPORTANT**
- precision of a defined Decimal number is *independent* of context precision
	- `Decimal(1.23456789')` will be stored *exactly*, even if context precision is `5`
	- *calculations*, however, will use the *context precision*
#### Mathematical Functions
- standard arithmetic operators and functions:
```python
+, -, *, /, //, %, **, round, abs, min, max, sum
```
- careful with `math` module
	- you can use...
	- `Decimals` get converted to `float`s first
- `Decimal` objects implement some math functions:
```python
d = Decimal('...')
d.exp()
d.sqrt()
d.ln()
d.log10()
```

> Learn more about `Decimal` arithmetic and mathematical functions here: https://docs.python.org/3/library/decimal.html#module-decimal

> Using `decimal.getcontext()` gets the global arithmetic context for the Decimal object. Which contains the precision value `prec=28` and rounding type `rounding=ROUND_HALF_EVEN`, as well as other properties.
```python
decimal.getcontext()
# Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999, Emax=999999, capitals=1, clamp=0, flags=[], traps=[InvalidOperation, DivisionByZero, Overflow])
```

> There is a performance impact when using a `Decimal` objects than `float`s. Also, there is a storage impact - more memory is needed to store a `Decimal` object than a `float`. We can use `sys.getsizeof()` to see this. Like so:
```python
from sys import getsizeof

getsizeof(0.1) # 24 bits
getsizeof(Decimal('0.1')) # 104 bits - more than 4 times a float
```

### Arithmetic Contexts
- arithmetic contexts used in decimal calculations to define many things
	- precision of intermediate calculations
	- rounding algorithm
- `decimal.getcontext()`
	- returns the current context information
	- `prec` --> precision (defaults to `28`)
	- `rounding` --> the rounding algorithm (default `ROUND_HALF_EVEN)` and more...
- we can change those definitions
	- *globally*
	- temporarily just for a section of code (using a context *manager*)
#### Rounding Methods
https://docs.python/org/3/library/decimal.html#rounding-modes
- default is `ROUND_HALF_EVEN`
	- rounds to nearest, with ties to nearest even integer
		- `0.135` --> `0.14`
		- `0.145` --> `0.14`
- but can define other rounding methods `ROUND_HALF_UP`
	- rounds to nearest with ties away from zero
		- `0.135` --> `0.14`
		- `0.145` --> `0.15`
#### Global Context Changes
- can modify `prec` and `rounding` in the *global* context
	- context settings persist for the remainder of the program
```python
ctx = decimal.getcontext()
ctx.prec = 5
ctx.rounding = decimal.ROUND_HALF_UP
```
#### Temporarily Changing Context Settings
- sometimes we want to temporarily change the context
	- perform some operations using that context
	- revert the context to its previous state
- could change the *global* context
```python
ctx = getcontext()
current_prec = ctx.prec

ctx.prec = new_prec

# perform operations
ctx.prec = current_prec
```
- cumbersome
- may even forget to switch back
#### Using a Context Manager
- much easier (and safer) to use a *context manager* 

![using a context manager for decimals](../assets/Pasted%20image%2020250212203830.png)

```python
# create context and enter context manager
with decimal.localcontext() as ctx:
	ctx.rounding = decimal.ROUND_HALF_UP # modify the local context
	print(round(Decimal('1.12345'), 4)) # Decimal('1.1235')
# after exiting context manager, global context is automatically restored
```

## Custom Classes
- everything in Python is an object
	- has a *type* (aka *class*)
	- has *state*
	- has *functionality*
- for example, `[1, 2, 3, 4, 5]` is an object
	- its *type* is `list` --> we say it's an *instance* of a `list`
	- its *state* are the *elements* in the list
	- `functionality` such as `.append`
```python
l1 = [1, 2, 3]
l2 = ['a', 'b', 'c']
```
- two different objects
- both instances of the `list` type
- but different *state*
```python
l2.append('d') # affects `l2`, not `l1`
```
#### Methods and Bindings
- why does `l2.append('d')` not affect `l1`?
	- `append` is a function that works on a *specific instance* of the class
		- `append` is called a *method* of the `list` class
	- when we call the `append` method: `l2.append('d')`
		- the *method* is *bound* to the object `l2`
			- basically it will operate on `l2`
- in general `append` will operate on whatever `list` object is specified before the dot
	- `l1.append(10)`
	- `l2.append('c')`
#### Custom Classes
- we can define our own custom types (classes)
	- instances of those classes will have
		- a *type* (the custom type we created)
		- some *state* (we can store values specific to the *instance*)
		- *functionality* (methods that are *functions bound to the instance*)
#### Initializers (aka. Constructors)
- when we create a *new instance* of a class
	- often want to create some initial state
		- usually by passing arguments to the "creation" phase
	- this is called the *initialization* phase
- creation process is started by *calling* the class (type)
```python
a = tuple([1, 2, 3])
```
- we are *calling* the `tuple` class (using `()`)
	- passing it an argument: `[1, 2, 3]`
	- call *returns* a *new* `tuple` instance, *initialized* with the elements `1, 2, 3`
---
- every object creation follows this basic principle
```python
reader = csv.reader(f, dialect=custom_dialect)
```
- *create* an instance of the `csv.reader` class by calling it
- pass some arguments used for *initialization* (file and dialect)
- call returns an initialized new *instance* of `reader`
```python
d = Decimal('1.2345')
```
- *create* an instance of the `Decimal` class by *calling* it
- pass some arguments used for *initialization* (number string)
- call returns an initialized new *instance* of `Decimal`
#### Classes as Blueprints
- classes are often referred to as *blueprints* for creating objects
	- a single class can be used to create many instances of that class
		- each instance will have its *own state*
		- the functions defined in the class become *methods bound* to the *instance* because these functions are *bound* to the instance
			- *they can access the state of the instance*
- suppose we have a `Person` class defined
	- we wrote our class so the initializer requires first and last names
```python
john = Person('John', 'Cleese')
eric = Person('Eric', 'Idle')
```
- we implemented a `greet()` method to say hello
```python
john.greet() # 'John says hi!'
eric.greet() # 'Eric says hi!'
```
#### Creating Custom Classes in Python
- use the `class` keyword
```python
class Person:
	'''A simple Person class'''
```
- code above is as simple as a class can be
	- but Python "injects" a lot of functionality into that class for us
		- it is *callable*
			- `p = Person()`
			- this created a new *instance* of `Person`
		- `Person` and `p` have some state Python defined for us
			- `Person.__doc__` --> `'A simple Person class'`
			- `Person.__name__` --> `'Person'`
			- `type(p)` --> `Person`
			- and more
### Defining Classes
- classes are like templates for creating objects
	- objects has state and functionality
	- we can define what the state and functionality is using a class
		- every instance of that class will have that functionality
		- but every instance has its own state
---
- class --> `Circle`
	- state: `radius`
	- functionality: `area()`, `perimeter()`
```python
circle_1 = Circle(radius=1)
circle_2 = Circle(radius=2)
```
- two different circles
	- each one has its *own* values for radius
- but formula to calculate `area` and `perimeter` can be *common*
	- it just needs access to the instance value for `radius`
---
- to define a class we use the `class` keyword
```python
class Circle:
	# definition of class is indented 
```
- one (optional) part of the definition of a class is a *docstring*
	- basically documentation of the class
```python
class Circle:
	"""This class can be used to represent a circle and calculate
	area and perimeter
	"""
```
- this is a valid Python class
---
```python
class Circle:
	"""docs for class"""
```
- class does not do much
	- but it still has quite a bit of functionality built in for us in Python
```python
Circle.__name__ # 'Cicle'
Circle.__doc__ # 'docs for class'
Circle.__class__ # Circle
Circle.__class__ is Circle # True
```
- Python also makes the class *callable*
---
- `Circle` can be *called* to create *new instances* of that class
```python
c1 = Circle()
c2 = Circle()
```
- two different *instances* of `Circle`
	- `c1 is c2` --> `False`
- the *type* of `c1` and `c2` is Circle
	- `type(c1) is Circle` --> `True`
- `c1` is an instance of `Circle`
	- `isinstance(c1, Circle)` --> `True`
---
- we can *set* attributes directly on the instances
```python
c1 = Circle()
c1.radius = 10

c2 = Circle()
c2.radius = 20
```
- we can *retrieve* the attribute from each instance
	- `print(c1.radius)` --> `10`
	- `print(c2.radius)` --> `20`
---
- we create *instances* of a class by *calling* the class
- we can *set/get attributes* directly on the instances using *dot notation*
	- to *create* and *initialize* a Circle instance
  ```python
	c1 = Circle()
	c1.radius = 10
	```
	- these attributes exist in the instance *namespace* --> normally a *dictionary*
  ```python
	c1.__dict__ # {'radius': 10}
	```
	- *sometimes* the state is not in that dictionary (this is covered in a more advanced course)
- but initializing the object state this way is cumbersome
	- we'll see a better way soon!

### Initializing Classes
- we've seen how to define custom classes
	- we *call* the custom class to create new instances of that class
	- but can we provide *initial* values when the class is created?
- we've seen this before!
	```python
	d = Decimal('10.1')
	```
	- creates new `Decimal` instance
	- initialized to `10.1`
	- the initial value was passed in the *same call* used to *create* the instance
---
- could mimic this initialization somewhat
```python
class Circle:
	"""Circle class"""

def create_circle(radius):
	c = Circle() # create the Circle instance (instantiation)
	c.radius = radius # set the instance radius (initialization)
	return c # return the initialized instance

c1 = create_circle(10)

type(c1) # Circle
c1.__dict__ # {'radius': 10}
```
#### Recall Methods
```python
# two different instances of a list
l1 = list('abc')
l2 = list('def')

# same `append` function
l1.append('d')
l2.append('g')
# but operates on two different instances of a list
l1 # ['a', 'b', 'c', 'd']
l2 # ['d', 'e', 'f', 'g']
```
- `l1.append('d')`
	- `append` is *bound* to `l1`
- `l2.append('g')`
	- `append` is *bound* to `l2`
- `obj.func()`
	- `func` is *bound* to `obj`, and is called a *method*
#### The `__init__` Method
- the `__init__` function is a special function that is called by Python when we create a new instance of a class
```python
class Circle:
	def __init__(self):
		print('__init__ called...')
```
- class creation: `Circle()` does *two* things
	- create a *new instance* of the class, let's give it some name, `new_obj`
	- calls the `__init__`function, passing `new_obj` as the *first argument*
		- in that sense, `__init__` is a *method bound* to `new_obj`
---
- `__init__` is a function defined inside the class
	- but a function nonetheless
- we can define additional parameters!
	- recall what we did here
  ```python
	def create_circle(radius):
		c = Circle()
		c.radius = 10
		return c
	```
	- *same thing!*
  ```python
	class Circle():
		def __init__(self, radius):
			self.radius = radius
	```
- note that the name `self` is not a special name - it is just convention
	- could name it anything else
- specify this additional parameter when we create the instance
```python
c = Circle(10)
c.__dict___ # {'radius': 10}
```

> The instance creation that Python does for us when we write `Person()`, is actually broken down into a few steps:
> 
> 1. Python creates a new instance of `Person` - let's call that object `temp`
> 2. Python calls `temp.__init__()` - i.e. `__init__` is a method bound to the newly created object
> 
> What's important to realize is that by the time `temp.__init__` is called, the new object (`temp`) already exists, and the `__init__` function is therefore a method bound to the new object.
> 
> So, if `__init__` is a method that is bound to some instance when it is called, how does it know what that instance is?
> 
> Python actually **injects** the instance object as the **first** argument of any method - here I called it `self`. This is a convention you should stick to, but we can name it whatever we want.

### Instance Methods
so far we know we can...
- create instances from classes by calling them
	- use `__init__` method to initialize instances
	- add value attributes using dot notation
- but how do we add functionality?
	```python
	c = Circle(10)
	c.area() # math.pi * r ** 2
	```
	- `area` needs to be a *function* in the class
		- *bound* to the *instance* when called with a dot notation
---
- exactly the same as the `__init__` function
	- define a *function* in the class
	- *first argument* will be the *instance*
```python
class Person:
	def __init__(self, name):
		self.name = name
	
	def say_hello(self):
		return f'Hello, {self.name}'

p = Person('Alex')
p.say_hello() # 'Hello, Alex'
```
---
- just like `__init__` method we can pass additional parameters to methods
```python
class Person:
	def __init__(self, name):
		self.name = name

	def eat(self, food):
		return f'{self.name} is eating {food.lower()}.'

p = Person('Alex')
p.eat('Broccoli') # 'Alex is eating broccoli.'
```

> Now we know how to create and initialize classes, as well as implement methods that can be used to calculate things, or even modify the internal state of the object.
> 
> This allows us to "package" up related functionality neatly in custom classes. (This is called _encapsulation_)

### Special Methods
- already seen `__init__`
	- provides *special behavior* to our custom classes
- there are many such methods that provide special behaviour
	- they *start* and *end* with double underscores
	- often referred to as *dunder* methods (so don't use this convention for your own method names!)
#### Object String Representations
```python
l = [1, 2, 3]
print(l) # '[1, 2, 3]'

class Circle:
	def __init__(self, r):
		self.radius = r

c = Circle(10)
print(c) # '<__main__.Circle object at 0x7fc2703b4b20>'
```
- Python's default string representation of our custom objects
---
- can *override* this default behaviour
	- via special dunder methods
		- `__str__`
		- `__repr__`
- `str(c)` --> will call `c.__str__()`
- `repr(c)` --> will call `c.__repr__()`
- why two methods?
	- `__str__` is used for string representation for users
	- `__repr__` is used for string representation for developers (more details usually)
- `print(c)` uses `__str__` if present
	- otherwise `__repr__`
	- otherwise default (class name & object id)
#### Object Equality
```python
l1 = [1, 2, 3]
l2 = [1, 2, 3]

# not the same objects
l1 is l2 # False

# but they are equal
l1 == l2 # True

class Person:
	def __init__(self, name):
		self.name = name

p1 = Person('Alex')
p2 = Person('Alex')

# not the same objects
p1 is p2 # False
p1 == p2 # False - this is incorrect, here we can use the `__eq__` dunder method
```
- we can override equality definition for our custom objects
	- `__eq__` method
```python
class Person:
	def __init__(self, name):
		self.name = name
	
	def __eq__(self, other):
		return self.name == other.name

p1 = Person('Alex)
p2 = Person('Alex)

p1 == p2 # True - same as p1.__eq__(p2)
```
- in general `a == b` --> `a.__eq__(b)`

> There are a lot of these "special" methods and attributes in Python - they are usually called **dunder** methods (they start and end with a double underscore).
> 
> For that reason, you should never create custom methods with that naming convention - Python kind of reserves that for itself - you have a lot of choices for your method and attribute names, just stay away from dunder names.

### Properties
- we've see how to define custom classes and how to
	- define instance methods
	- get/set attributes directly on the instance
  ```python
	# these are sometimes called 'bare' or 'regular' attributes
	c.radius = 10
	self.radius = 10
	```

```python
class Person:
	def __init__(self, name):
		self.name = name

	def say_hello(self):
		return f'Hello, my name is {self.name}'

alex = Person('Alex')
alex.say_hello() # 'Hello, my name is Alex'
alex.name = 'Eric'
alex.say_hello() # 'Hello, my name is Eric'
```
---
- we've been accessing these attribute values *directly*
	- we have no control over what the assigned values are
	- we have no control on formatting or modifying attribute when it is read
- sometimes, we do!
- we *can* control things in the `__init__` when the instance is *created*
```python
class Sale:
	def __init__(self, quantity):
		if not isinstance(quantity, int):
			raise ValueError('Must be an int')
		self.quantity = quantity
```
---
- *cannot* control how it is set subsequently
```python
class Sale:
	def __init__(self, quantity):
		if not isinstance(quantity, int):
			raise ValueError('Must be an int')
		self.quantity = quantity

s = Sale(10)
s.quantity = "zero" # this works
```
#### Properties
- a property is like an attribute, but 
	- the value is set via a method (setter)
	- the value is retrieved via a method (getter)
- if `name` is a *property* in the `Person` class, and `p` is an instance
- `p.name = 'Alex'`
	- calls the *setter* method for `name`, passing 'Alex'
- `print(p.name)`
	- calls the `getter` method for `name`, returning a value
#### Read-Only Properties
- can create read-only properties
	- define a getter method
	- but don't define a setter
(write-only properties are possible, but not common, and a little harder to achieve)
#### Creating a Read-Only Property
- define a *method*, with the *name* of the property
- *decorate* the method with `@
- `property` is a built-in class for creating *managed* attributes
```python
class Math:
	@property
	def pi(self): # this is a getter method
		return 3.14

m = Math()
m.pi # calls the method `pi()`, bound to `m` (e.g. `m.pi()`)
```

```python
class Person:
	def __init__(self, name):
		self._name = name # notice the underscore
						  # - convention
						  # - signifies `_name` is a private attribute to the class
						  # - people using this class should not modify it directly

	@property
	def name(self):
		return self._name
```
#### Read/Write Property
- first define a getter
- then define the setter
```python
class Person:
	def __init__(self, name):
		self._name = name

	# all the property names 'must' be the same
	@property
	def name(self):
		return self._name

	@name.setter
	def name(self, value):
		self._name = value
```
#### Calculated Properties
- properties are very general
	- they are just methods
	- they do not have to be be used just to return an attribute
	- they can just calculate and return some value
```python
class Person:
	def __init__(self, dob):
		self.dob = dob

	@property
	def age(self):
		age = <calc current age>
		return age
```

## 3rd Party Libraries
- in this and the next sections we cover some popular 3rd party libraries
- there are thousands of 3rd party libraries
	- we cover only a tiny subset
- those libraries can have a ton of functionality
	- we only scratch the surface here
- but, provided are all the tools and knowledge to research further
	- read the docs
	- read blog posts and see what other libraries are popular for your needs
---
3rd party libraries that are covered:
- `pytz` --> dealing with time zones and DST
- `dateutil` --> provides an "intelligent" datetime string parser 
- `requests` --> used to query web servers and web APIs (over http(s))
- `numpy` --> highly efficient implementations for array processing and math computations
- `pandas` --> used for data manipulation and analysis
- `matplotlib` --> used for creating plots and charts
---
- these are 3rd party libraries
	- they need to be installed
	  ```python
		pip install
		```
	- to `pip install` you need to know the package name
		- library docs have that information
---
- create a virtual environment
```python
# for Linux
python3 -m venv env_name
# for Windows
py -m venv env_name
```
- activate virtual environment
```python
# for Linux
source env_name/bin/activate
# for Windows
.\env_name\Scripts\activate
```
- install 3rd party library into the virtual environment 
```python
pip install pytz
```

### The `pytz` Library
- used for dealing with time zones
	- implements the Olson (or IANA) database
		- supports *DST* (daylight savings times)
		- uniform naming convention
		  ```
			US/Eastern
			America/New_York
			Europe/Paris
			```
			- *Area*/*Location*
			- goes back to *1970* (Unix Epoch) 
---
- https://pythonhosted.org/pytz/
- `pip install pytz`
- `import pytz`
- `pytz.all_timezones` --> returns a list of all named time zones
- internally uses Python's `tzinfo`
	- but with some extras used for DST
	- a `pytz` timezone can be used instead of a `tzinfo` object
#### Looking up a Time Zone
- can retrieve a time zone from its name
	- `pytz.timezone('US/Eastern')`
	- `pytz.timezone('UTC')`
		- `pytz.UTC`
- can use these time zones instead of Python's `tzinfo`
```python
datetime(
	2020, 5, 15, 10, 0, 0,
	tzinfo=pytz.timezone('US/Eastern')
)
```
#### Making a naive `datetime` aware
- use `pytz` time zone's `localize` method
	```python
	tz_ny = pytz.timezone('America/New_York')
	tz_ny.localize(naive_dt)
	```
	- `pytz` will figure out if it needs to use DST or not!
	- this just *attaches* the time zone information to the naive datetime
		- *it does not convert the datetime to the new timezone*
		- i.e. it just assumes the datetime was given in the timezone that is being attached.
#### Converting Aware `datetime` to Other Time Zones
- once we have an *aware* datetime we can convert it to another timezone
	- use the `astimezone` method of the `datetime` object
		- but because we are using `pytz` timezone objects, conversions work fine, including DST calculations
- if we start with a naive UTC time, we can directly transform it to a specific timezone
```python
<py_tz_timezone>.fromutc(<naive datetime>)
```

> In the special case where we are **not** converting a UTC aware datetime to another timezone, we can use a slightly more efficient method in `pytz`, the `fromutc` available in `pytz` time zone objects:
```python
import pytz
from datetime import datetime

tz_chicago = pytz.timezone('US/Chicago')
tz_chicago.fromutc(datetime.utcnow()) # returns the current datetime converted from a naive utc datetime to an aware chicago datetime

# the benefit of this is that we don't have to localize the naive utc time first
# before converting its tzinfo
```

### The `dateutil` Library
- https://dateutil.readthedocs.io/en/stable/
- `pip install python-dateutil`
- `parser`
	- ability to automatically parse dates and times from string in various formats
	- this is what we'll look at in this course
but it has a lot more...
- computing dates based on advanced recurrence formulas
	- generate sequence of dates weekly on Tuesday and Thursday for 5 weeks
	- generate sequence of dates every weekday for 3 months
		- very similar to what you might see when you set recurring calendar meetings
#### Basic Parsing Functionality
```python
from dateutil import parser

parser.parse('2020-01-01T10:30:00')
parser.parse('2020-01-01T10:30:00 am')
parser.parse('12/31/2020')
parser.parse('31/12/2020')
```
#### Ambiguous Month/Day
```
4/3/2020 or 2020/4/3
```
- is this *Month/Day* or *Day/Month*?
- parser *default* assumes *Month/Day*
	- i.e. month is specified first
- can override this using `dayfirst` keywork argument
	```python
	parser.parse('2020/4/3') # April 3, 2020
	parser.parse('2020/4/3', dayfirst=True) # March 4, 2020
	```
	- raises `ParserError` exception if date is invalid or unrecognizable
#### Fuzzy Parsing
- parser can eve attempt parsing strings that contain extra information
	- *March the 4th, 2020*
	- default parsing will not work
- use `fuzzy_with_tokens=True` argument when calling `parse`
	- returns a tuple *(parsed datetime, ignored text elements)*
	- raises `ParserError` exception if date is invalid or unrecognizable
- it's quite good, but cannot handle just anything
	- *May the fourth, 2020* is not recognized

### JSON Data
- *J*ava*S*cript *O*bject *N*otation
- it is a *simple* way of representing objects using just strings
	- very easy to transmit strings
	- over a network, as a text file, etc.
- JSON is a lightweight *standard* that we can use
	- *encode* an object into a string --> *serialization*
	- *decode* a JSON string into an object --> *deserialization*
- most often used when transmitting data over the web (e.g. REST APIs)
---
- JSON is very simple
	- easy for humans to read and write JSON
	- easy for computers to parse and generate
	- it is a pure text format
	- language independent (e.g. Python, C++, C#, JavaScript, Java, etc)
---
what does JSON consist of? JSON consists of:
- objects --> *unordered* `key:value` pairs delimited by `{ }` (*dictionary*)
- array --> *ordered* list elements separated by `,` and delimited by `[ ]` (*list*)
- values
	- numbers (integer or with decimal point)
	- strings, delimited by double quotes `"..."`
	- boolean `true` or `false` (note the *lowercase*!) 
	- `null` (*None*)
	- objects --> so objects can contain other objects, arrays
	- array --> arrays can contain other arrays, objects
- basically JSON looks like a Python dictionary!
Example:
```python
''' # it's a string
{ # root is an object
	"firstName": "Eric", # key:value pairs, key must be a string
	"lastName": "Smith", # strings must be double-quote delimited
	"address": { # value is another object
		"country": "USA",
		"state": "New York",
	},
	"age": 28,
	"favouriteNumbers": [42, 3.14], # value is a list
	"likesSushi": false,
	"driversLicense": null
}
'''
```
---
- white spaces (spaces, tabs, newlines) do not matter
```python
'''
{
	"firstName": "Eric",
	"lastName": "Smith"
}
'''
# is the same as...
'''{"firstName": "Eric", "lastName": "Smith"}'''
```
- but which one is more human-readable?
- note the stylistic difference: `camelCase` vs `snake_case`
	- of course, they're just strings, so you can use whatever you want
#### Deserializing JSON (decoding)
- Python standard library `json` module
	- `json.loads(json_string)`
		- *parses* a JSON string and returns a `dict` object
since JSON is a standard, Python `loads` can handle any standard JSON object
#### Serializing JSON (encoding)
- `json.dumps(dict)` --> returns a JSON string
- have to be more careful here
- basic JSON data types are very simple: `int`, `float`, `str`, `bool`, `None`
	- Python has a far richer set of data types
		- `datetime`, `Decimal`, custom classes, etc.
		- those are not serialized by default, and if we try, we'll get an exception
		- there is a way to specify custom encoders
			- beyond the scope of this course (we'll look at this when we study JSON in-detail as well as Pydantic)
			- they rely on something called *Inheritance*

> When we use `json.dumps()` there's an `indent` parameter that lets us add whitespace to the display of the serialized JSON data. Like so:
```python
print(json.dumps(<dict>, indent=2)) # indents the JSON data for easier human comprehension
```

> There is a way to specify a simple custom encoder, using the named argument `default` in the `dumps` function. This argument can be used to specify a function that will get called when the default encoder cannot serialize the object. That function should either return the encoded value, or raise a `TypeError`. Like so:
```python
d = {
	"name": "Isaac Newton",
	"dob": datetime(1643, 1, 4)
}

def my_encoder(obj):
    print(f'my_encoder({obj}) called...')
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError  # only handles datetimes

json.dumps(d, default=my_encoder) # returns...
# my_encoder(1643-01-04 00:00:00) called...
# '{"name": "Isaac Newton", "dob": "1643-01-04T00:00:00"}'
```

### REST APIs
#### What is an API?
API --> Application Programming Interface

![an application programming interface](../assets/Pasted%20image%2020250219135421.png)

- enables your application to *interact* with another application
- a Python class exposes an API
	- methods, properties

![python code and class interface](../assets/Pasted%20image%2020250219135459.png)

- these days many applications are "in the cloud"
	- CRM
	- Payroll
	- trading platforms
	- Automated AI
- they expose an API available via the web using http(s)
	- web sites
		- request data using a URL
		- this is called a *GET* request (fetches data)

![websites performing get requests to a server](../assets/Pasted%20image%2020250219135818.png)

#### How a browser retrieves a web page 

![browser retrieves webpage](../assets/Pasted%20image%2020250219140026.png)

- also supports *query arguments*
	- basically like named arguments in Python functions
	  ```
		GET https://mysite.com/curentTemp?city=Chicago&units=metric
		```
- web server at `mysite.com` waits to receive these requests
- browser sends request to web server
- server sends back data (often HTML, but does not have to be!)
- browser displays returned data
#### Sending Data
- can also send data to a web server
	- e.g. user registration data
- different methods or verbs --> e.g. *POST*
- specific "PATH" on web server we need to send the data to (specific *URL*)
	- data is attached when request is sent by browser
- web server receives this data and does something with it
	- and usually return a response of some kind
#### In general...
- web servers listen for incoming requests
- request contains
	- *method* `GET`, `POST`, ...
	- *URL* --> specifies exactly what we are trying to "access"
	- *query arguments* (maybe)
	- "attached" *data* (maybe)
- the set of what URLs, query arguments, methods and data a web server understands
	- is essentially an API
- data is not necessarily HTML - can be *JSON*, *XML*, ...
#### REST APIs
- REST APIs are special types of APIs
	- REST has to do with how they are implemented and their behaviour
	- as users of the API we don't actually care if it's REST or something else!
- one of the fundamental characteristics of a REST API is that calls are independent of each other (**stateless**)
	- call to API does not rely on remembering how you interacted with it in the past
	- not quite the same as web sites
		- log in
		- now you can access pages on the site
			- web server remembers who you are 
				- **stateful**
#### Authentication / Authorization
- REST APIs are generally secured
	- you need to be *authenticated* --> web server needs to know *who you are*
		- usually a *secret token* you pass in the request
			- in something called *headers*
				- just an extra "bucket" of key-value data that can be sent/received along with request
	- you also need to be *authorized* to perform the request
		- you may be authorized to read some data
		- but you may not be authorized to create/delete that data
*Authentication* --> establishes who you are to the system you are interacting with
*Authorization* --> governs what you can and cannot do in the system
#### API Data Formats
- most modern APIs use *JSON* for sending/receiving data 
	- sometimes uses *XML*, or even proprietary formats

![api data formats](../assets/Pasted%20image%2020250219142921.png)

#### Resources
- REST APIs allow us to interact with entities, called *resources*
	- *bank account*
		- create new account
		- list accounts for specific customer
		- get balance
		- deposit, withdraw
		- delete the account
	- *customer*
		- create new customer
		- get customer info
		- update customer info
		- delete customer

![resources for REST APIs](../assets/Pasted%20image%2020250219143338.png)
#### API Methods
- since humans design / write these APIs, things are not always consistent!
- *GET*
	- retrieves resource(s)
	- often used with query args
- *POST*
	- used to create a resource
	- issuing the same POST request twice can end up creating two resources
- *PUT*, *PATCH*
	- usually used for updating an existing resource
- *DELETE*
	- delete a resource
#### Status Codes
- making an HTTP request (GET, POST, etc) always returns a *status code*
	- plus whatever else the API specifies
- *2xx* --> success
	- `200` --> OK (request was successful)
	- `201` --> Created (resource created successfully)
	- `202` --> Accepted (request accepted, but not finished processing (async))
- *4xx* --> you did something wrong
	- `400` --> Bad Request (server did not understand the request)
	- `401` --> Unauthorized (technically this means "*not authenticated*")
	- `403` --> Forbidden (this means not authorized)
	- `404` --> Not Found (server cannot find specific resource)
- *5xx* --> Server had an issue
	- usually not your fault!
- many more here: https://en.wikipedia.org/wiki/List_of_HTTP_status_codes

> `429` status code is an error code that means "Too Many Requests" - in other words, the user has sent too many requests in a given amount of time. Intended for use with rate-limiting schemes.

#### Finnhub Stock API
- https://finnhub.io
- provides free and paid tiers
	- REST API
	- uses JSON
	- mostly GET requests
- sign up for the free tier and try it out
### The `requests` Library
- python has a module in the standard library for making http requests
	- slightly low level interface (think time vs datetime)
	- 3rd party library: *Requests: HTTP for Humans*
		- pretty much standard
		- even Python's own docs suggest it!
- check out the docs: https://requests.readthedocs.io/en/master/
```
pip install requests
```
#### Making Requests
- all standard methods/verbs are implements as functions
```python
requests.get(...)
requests.post(...)
requests.put(...)
# etc.
```
- common arguments
	- `url` --> the URL request will be sent to
	- `params` --> dictionary of query parameters (key = value)
	- `json` --> JSON sent in request (usually for POST, PUT, etc.)
	- `headers` --> dictionary of headers (key = value)
	- and many more...
#### Receiving Responses
- result of making a request (get, post, etc.) is a `Response` object
- it has the following properties (amongst others):
	- `status_code` --> e.g. `200`, `403`
	- `reason` --> e.g. `OK`, `Forbidden`
	- `text` --> content of the response
	- `json` --> returned *deserialized* JSON (if any) --> so a `dict`
	- `headers` --> dictionary of headers received from server
	- `cookies` --> cookies received from server
---
*Example:* Google search results (HTML response)
- search:
	- search terms: python http requests
	- number of results: 5
	- https://www.google.com/search?q=python+http+requests&num=5
- using `requests` library to retrieve the HTML search results
```python
response = requests.get(
	 url='https://www.google.com/search',
	 params={'q': 'python http requests', 'num': 5}
)

response.status_code # 200
response.reason # OK
response.text # HTML page broswer would display
```
- calling an undefined URL
```python
response = requests.get(
	 url='https://www.google.com/search2',
	 params={'q': 'python http requests', 'num': 5}
)

response.status_code # 404
response.reason # Not Found
```

## NumPy
- *NumPy* is a widely used library, mainly used for working with arrays
	- very fast
	- memory efficient
	- very flexible
```python
pip install numpy
```

> Learn more at https://numpy.org/

#### What are Arrays?
- basically lists
	- a Python `list` is a type of array
	- elements are indexed --> `arr[0]`, `arr[1]`, `...`
	- array can be sliced --> `arr[start:stop:step]`
	- variable size --> can add/remove elements from array
	- heterogenous --> elements can have different data types
- a NumPy array (`ndarray`)
	- fixed size
	- homogeneous
#### Python `list` vs. NumPy `ndarray`
- these are some of the similarities and differences
- `ndarray`
	- fixed size
	- can be reshaped
	- homogeneous
	- elements have specialized, restricted data types
- `list`
	- variable size
	- heterogeneous
	- elements are Python objects

![python list vs numpy ndarray](../assets/Pasted%20image%2020250220214349.png)

#### NumPy Efficiency
- more *space efficient* than Python
- array manipulation and calculations are *much faster*
	- *vectorization*
- but at a cost
	- fixed size
		- once created, *cannot add/remove* elements
		- elements *can* be *replaced*
	- homogeneous
		-  all elements must be the *same type*
		- even in multi dimensional arrays (arrays of arrays)
	- data types
		- it uses data types from underlying *C* language
		- memory efficiency and vectorization
#### Integer Sizes
- integers are stored as sequences of *bits* (`0`s and `1`s)
- number of bits determines how large the integer can be
- 4 bits - largest number --> $1111_{2}$ = $2^{0}$ + $2^{1}$ + $2^{2}$ + $2^{3}$ = $15$
	- range is: `[0, 15]` (16 numbers) 
	- but may want *negative* numbers
	- in that case, one bit is reserved to keep track of the sign
		- 3 bits --> $(111)_{3}$ = $2^{0}$ + $2^{1}$ + $2^{2}$ = $7$
		  ```
			-7, -6, ..., -1, -0, 0, 1, 2, ..., 6, 7
			```
		- `0` does not need `-0` ( a negative sign) --> `[-8, 7]`
---
```
# signed integers
8 bits [-128, 127]
16 bits [-32_768, 32_767]
32 bits [-2_147_483_648, 2_147_483_647]
64 bits [-9_223_372__036_854_775_808, 9_223_372__036_854_775_807]

# unsigned integers
8 bits [0, 255]
16 bits [0, 65_535]
32 bits [0, 4_294_967_295]
64 bits [0, 18_446_744_073_709_551_615]
```
#### Floats
- Python uses 64 bits to store floats
	- certain precision and size of exponent
- C also has 32-bit floats
	- less precision, smaller exponent
	- but more efficient storage
#### NumPy Types
- in NumPy you choose your data type
	- if you pick an unsigned integer you can only store numbers in `[0, 255]`
		- signed integers --> `int8, int16, int32, int64`
		- unsigned integers --> `uint8, uint16, uint32, uint64`
		- floats --> `float32, float64` (`float64` is compatible with Python `float`)
		- complex --> `complex64, complex128` (`complex128` is compatible with Python `complex`)

> Learn more here: https://numpy.org/doc/stable/user/basics.types.html

#### Vectorization
- suppose we want to multiply every element of one array by the corresponding element in another array
```python
a = [1, 2, 3, 4]
b = [10, 20, 30, 40]
# where result is [10, 40, 90, 160]

# we can use a loop
result = []
for i in range(4):
	result.append(a[i] * b[i])
# or a list comprehension
[x * y for x, y in zip(a, b)]
```
- at every loop, Python must:
	- lookup the operand objects
	- determine the types
	- try to perform the operation (if `a * b` does not work, it tries `b * a`)
- `C` does not have to do all that work --> *significantly faster*
---
- NumPy implements things in such a way that
	- given `a` and `b` are NumPy arrays (`ndarray`)
	- given a supported function or operator
		- `a + b` --> `add(a, b)`
		- `a * b` --> `multiply(a, b)`
		- `a / b` --> `divide(a, b)`
		- `sin(a) / sin(b)` --> `divide(sin(a), sin(b))`
	- NumPy pushes the loop and calculations down into `C`
	- this is called *vectorization*
		- these functions are called *universal functions (ufunc)*
#### Why are Arrays Important?
- most data we deal with is represented as arrays
	- often multi-dimensional arrays
- an image is a 2-dimensional array of colored pixels
	- each pixel is an array, e.g. `[red, green, blue, alpha]`
- a video is an array of images (a bit oversimplified)
- audio is encoded into arrays
- stock quotes, tick data are arrays of data
- an Excel spreadsheet is a (2-dimensional) array
#### NumPy is a Huge Library 
- lots of universal functions
	- financial, math, stats, linear algebra, sorting, sampling, Fourier transforms (discrete) and more...
	- we'll just look at a few of these
- here, we take an introductory look at array creation and manipulation (indexing, slicing, fancy indexing, masking, reshaping)

> Learn more here: https://numpy.org/doc/stable/

- it also is the foundation of the *Pandas* library (dealing with data sets)
### Creating Arrays from Lists
- first, we import NumPy
```python
import numpy
# typically everyone aliases it for less typing
import numpy as np
```
- the array type is `np.ndarray` (n-dimensional array)
---
how do we create arrays? we can convert a python list into a numpy array like so:
```python
a = np.array([1, 2, 3])
type(a) # ndarray
```
- but what type was used for the elements themselves?
	- remember that in Python we use the C types, not the Python types
		- also array is homogeneous, i.e. every element has same data type
  ```python
	# to check the C data type of a numpy we use:
	a.dtype # int64
	```
	- NumPy analyzes the data and picks something appropriate
	- in this case a 64-bit integer
	- for floats it defaults to 64-bit floats
#### Specifying the Element Data Type 
- we can override that default and select a specific type
```python
a = np.array([1, 2, 3], dtype=np.int8)
a.dtype # int8
```
- **CAREFUL !!!**
	- do not use a type that is too restrictive
	- weird things happen when integer in list is too large for specified `dtype`
	- floats in a list will be truncated if `dtype` is set to an integer
- why not just always use `int64`?
	- memory efficiency for extremely large datasets
#### Multi-Dimensional Python Lists
- in this course we'll stick to two-dimensional arrays

![multi-dimensional arrays](../assets/Pasted%20image%2020250220225342.png)

- only using 2 dimensions is not particularly restrictive

![two-dimensional arrays not restrictive](../assets/Pasted%20image%2020250220225415.png)

#### Converting Multi-Dimensional Lists to Arrays 
- works exactly the same way as with 1-D arrays
	- but again, remember that *all* elements in the array must be of the *same type*
```python
l = [
	[1, 0, 0],
	[0, 1, 0],
	[0, 0, 1]
]

m = np.array(l)
m.dtype # int64

m = np.array(l, dtype=np.uint8)
m.dtype # int8
```
#### Array Shape 
- *shape* of an array is *number of elements* in each *dimension*
```python
[
	[1, 2, 3], 
	[4, 5, 6]
]
# 2 dimensions
# first dimension has 2 elements
# second dimention has 3 elements
# (2, 3)

[1, 2, 3]
# 1 dimension
# first dimension has 3 elements
# (3, )
```
- use the `shape` attribute of the `ndarray` objects
### Creating Arrays from Scratch
- seen how to create arrays from lists
	- handy to convert lists of data loaded from a CSV file for example
		- or retrieved via a web API
- sometimes we just need to generate specialized arrays
	- could do it from a Python list
	- but NumPy has several convenient functions
#### Array of Zeros

> For context, `size` is a method on an `ndarray` object that means the number of elements within the array. For ex:
```python
m = np.array([1, 2, 3, 4, 5])
m.size # 5
```

```python
np.zeros(size_or_shape, dtype)
```
- `size_or_shape`
	- `size`
		- single number --> 1-D array of that length
	- `shape`
		- tuple --> shape `(rows, colunms)`
- `dtype`
	- optionally specify data type
	- defaults to `float64`
---
```python
np.zeros # arrays filled with zeros
np.ones # arrays filled with ones
np.full # arrays filled with some specified constant value
np.eye # generates identity matrices
np.arange # generates 1-D array based on a range (start:stop:step)
np.linspace # generates evenly spaced numbers between start/stop
np.random.random # arrays filled with random floats [0, 1)
np.random.randint # arrays filled with random integers [low, high)
```

> `np.arange(2, 11, 2)` does the same as this `list(range(2, 11, 2))` - creating an n-dimensional array of numbers.

> We can also transform existing arrays into other arrays by applying functions on them.

### Reshaping Arrays
#### What is Reshaping?
```python
[1, 2, 3, 4, 5, 6] # shape --> (6,)
```
- using the same elements we can rearrange them

![reshaping arrays](../assets/Pasted%20image%2020250221203014.png)

#### Reshaping Shares Elements
reshaping shares elements in the array
- this is *very important* (and we'll see later this applies to slicing also)

![reshaping shares elements in the array](../assets/Pasted%20image%2020250221203149.png)

#### Making a Copy
- `arr.copy()`
	- this will make a *copy* of `arr`
	- can use to break the tie between an array and the reshaped array

> In Python arrays (n-dimensional arrays), we can reshape the arrays but the slots (the elements) are still linked to each other.

> To ensure that reshaped array does not share slots (or elements) with the original array or other arrays reshaped from the original array, we use the `copy()` method. Like so: 
```python
import numpy as np

arr = nd.arange(1, 12) # array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
m1 = arr.reshape(3, 4).copy() # this reshapes `arr` but also copies it dissociating it from the slots (or elements) in `arr`
# the above is the same as this...
m1 = arr.copy().reshape(3, 4)
```

### Stacking Arrays
- concept is very straightforward
	- we can stack arrays on top of each other (`vstack`)
	- or we can stack them side by side (`hstack`)

![stacking arrays](../assets/Pasted%20image%2020250221204619.png)

#### Stacking
- `a1`, `a2`, `a3` are arrays
	- stack vertically
		- `np.vstack((a1, a2, a3))`
	- stack horizontally
		- `np.hstack((a1, a2, a3))` (remember, the argument is a tuple)

#### Shapes must be Compatible
- if stacking vertically, same number of columns for each array is required 
- if stacking horizontally, same number of columns for each array is required

![shapes must be compatible](../assets/Pasted%20image%2020250221205006.png)

#### What happens to `dtype`?
- can stack arrays with different `dtype`
	- NumPy will determine a suitable common data type
		- we cannot control that
	- stacking `uint8`, `uint16` and `int64`
		- NumPy picks a `float64` for the stacked array

> In a future version of NumPy (1.20), it will be possible to specify the data type when using the `concatenate` function - which is a more generic form of `vstack` and `hstack`. *Not sure if this is available already, might have to check.* *Checked: It's available - the current version of NumPy now is 2.2.*\*
#### Casting an Array to another Data Type
- we can however control the stacked data type by first *converting* the arrays we are stacking to a *common type*
	- use the `astype` method on an array
	  ```python
		arr1.astype(np.int64)
		```
	- so we could use this to stack multiple arrays
	  ```python
		np.vstack(
			[
				arr1.astype(np.int64),
				arr2.astype(np.int64)
			]
		)
		```
#### Stacked Arrays are Independent of Original Arrays
- we saw that a reshaped array is "linked" to the original array
- this is *not* the case for stacked arrays
	- modifying an element in the stack does *not* modify original array
	- modifying element in original array does *not* modify the stack 

> We can easily convert an array from one type to another using the `astype()` method. Like so:
```python
import numpy as np

a1 = np.array([1, 2], dtype=uint8) # array([1, 2], dtype=uint8)
a1.astype(np.float32) # array([1., 2.], dtype=float32)
```

> **IMPORTANT**: Unlike reshaped arrays, stacked arrays do not "share" their elements with the original arrays.

> In NumPy arrays, selecting/getting/setting an array element is done like so:
```python
# let's say we want to select/get or set an element in `a1`
a1 = np.array([
	    [1, 2, 3, 4, 5],
	    [6, 7, 8, 9, 10]
	])

a1[0, 2] # 3
a1[1, 2] # 8
a1[-1, -1] # 10
a1[-1, 3] = 90 # changes `9` in the array to `90`
```
### Indexing
#### Python Sequence Types
- recall Python sequence types such as lists and tuples
- elements are positionally indexed `0, 1, 2, ...`
- get element at index `i` `lst[i]`
- replace element at index `i` `lst[i] = x`
- indexing 2-D lists (a list of lists) works the same
```python
arr = [[1, 2], [3, 4]]
arr[0][1] # 2
arr[0][0] = 100 # arr: [[100, 2], [3, 4]]
```
#### Indexing NumPy Arrays
- very similar to Python sequence types
- with NumPy arrays instead of `[i][j]`, we can use `[(i, j)]`
	- `(i, j)` is a tuple so we can omit the `( )`. Like so: `[i, j]`
```python
# therefore, the both are similar (specific to indexing NumPy arrays)
arr[0][0], arr[0, 0]
arr[1][2], arr[1, 2]
```
- for 1-D array
```python
arr = np.arange(1, 7)
# both index syntax below are similar
arr[1] # 2
arr[(1,)] # 2
```
#### Mutating Elements
- works the same as Python lists
```python
arr = np.arange(1, 7)
arr[2] = 30 # arr: ndarray([1, 2, 30, 4, 5, 6])
arr = np.arange(1, 7).reshape((2, 3)) # returns...
# [
#   [1, 2, 3],
#   [4, 5, 6]
# ]
arr[1, 2] = 60 # returns...
# [
#   [1, 2, 3],
#   [4, 5, 60]
# ]
```

> Remember, when assigning or mutating values of an array, ensure the data type of the new value corresponds with the data type `dtype` of the array.

### Slicing
#### Slicing Python Sequences
```python
l = [1, 2, 3, 4, 5]
l[0:3] # [1, 2, 3]
```
- slicing returns a new, independent, list
```python
slice_ = l[0:3]
slice_[1] = 20 # [1, 20, 3]
l = [1, 2, 3, 4, 5]
```
#### Slicing Python 2-D Sequences
```python
m = [
	[1, 2, 3],
	[4, 5, 6],
	[7, 8, 9],
]
```
- want to slice in two axes
	```python
	m[0:2] # returns...
	# [
	#   [1, 2, 3],
	#   [4, 5, 6]
	#]
	```
	- cannot just use a slice to isolate
	  ```python
		[
			[2, 3],
			[5, 6],
		]
		```
#### Python Sequence Slice Assignments
- we can mutate a Python list by using the assignment operator with a slice definition
```python
l = [1, 2, 3, 4, 5]
l[0:3] = [10, 20, 30] # [10, 20, 30, 4, 5]
```
- since Python lists are not fixed size, we can also replace the slice with more or less elements
```python
l = [1, 2, 3, 4, 5]
l[0:2] = [10, 20, 30, 40]
l # [10, 20, 30, 40, 3, 4, 5]
```
#### Slicing 1-D NumPy Arrays
- very similar to slicing lists
```python
arr = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8])
arr[0:3] # nd.array([0, 1, 2])
```
- step, negative indexing, etc are all supported, just like `list` slicing
```python
arr[2:6:2] # [2, 4]
arr[1::2] # [1, 3, 5, 7]
arr[::-1] # [8, 7, 6, 5, 4, 3, 2, 1, 0]
```
#### Slicing 2-D NumPy Arrays
- NumPy provides support for slicing along *multiple axes*

![slicing 2-D NumPy arrays](../assets/Pasted%20image%2020250223225820.png)

- can also write `arr[0:2, 1:3]` as `arr[:2, 1:]`
---
- can get even fancier when using steps

![](../assets/Pasted%20image%2020250223230005.png)

- can think of this as the *intersection* of
	- rows `0, 2, 4` --> `[::2]`
	- columns `1, 3` --> `[1::2]`
	- can also be rewritten as:
		- `arr[::2, 1::2]`
#### Slicing Assignment in NumPy Arrays
- works very similarly to assigning to `list` slices
	- cannot replace with an array that is not the *same shape*
		- also means we cannot *change size* of the original array
			- makes sense since NumPy arrays are fixed size
		- be careful with data types!
```python
a = np.array([1, 2, 3, 4, 5])
a[0:3] = np.array([10, 20, 30])
a # nd.array([10, 20, 30, 4, 5])
```
- can also replace with a `list` or `tuple` - NumPy will handle it
---
- can also assign a single value (not an array) to a slice
- NumPy basically fills the slice with the same value repeated as many times as necessary (this is called broadcasting)
```python
arr = np.array([1, 2, 3, 4, 5, 6, 7])
arr[::3] # nd.array([1, 4, 7])
arr[::3] = 0 # nd.array([0, 2, 3, 0, 5, 6, 0])
```
#### Slices are "linked" to Original Array
- similar to `reshape` we saw earlier
- a slice is "linked" to the array it was sliced from

	![slices linked to original array](../assets/Pasted%20image%2020250223231130.png)

	- replacing an element in `s` will be "seen" by `arr`
		- and vice versa
	- to avoid this, make a copy of the slice
	  ```python
		s = arr[0:3].copy()
		```

> We can also combine indexing along one axis with slicing in another. Like so:
```python
arr = np.arange(1, 26).reshape(5, 5)
arr[2, 1::2] # nd.array([12, 14])
```

> An interesting question is: how do I pick a slice of the first, second, and fourth rows of `arr`? You can't use a slice with a step for selecting (irregular) rows - a slice cannot slice the rows `0, 1, 3`. So what's the solution? We'll look at that in **Fancy Indexing**.

> Another thing to note is that although an `ndarray` slice is "linked" to the original array, if we replace it with another array, that replaced slice is not linked to that second array. Let's see an example:
```python
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([10, 20])

arr1[0:2] = arr2
arr1 # array([10, 20,  3,  4,  5]) - same as nd.array()

arr2[0] = 100
arr2 # array([100,  20])

arr1 # array([10, 20,  3,  4,  5]) - as you can see, no change!
```

> If we try assign an array that is not of the same shape, we'll get an exception, even if the total number of elements matches the total number of elements in the slice. Ex:
```python
arr = np.arange(9).reshape(3, 3)

arr[:2, 1:] = [[10, 20], [40, 50]] # this works!
arr[:2, 1:] = [10, 20, 40, 50] # this does not work!

# of course you can reshape array before assignment, like so:
arr[:2, 1:] = np.array([10, 20, 40, 50]).reshape(2, 2) # this works!
```

> Again, since NumPy arrays have a fixed homogenous `dtype`, we have to be careful when we mix types. Ex: suppose we have an array of `uint8` (unsigned 8-bit integers), so range is `[0, 255]`:
```python
arr = np.array([10, 20, 30, 40, 50], dtype=np.uint8)

arr[:2] = [-100, 300] # as you can imagine, weird things happen...
arr # array([156,  44,  30,  40,  50], dtype=uint8)
# the integers wrap around; we've discussed this before; see previous notes for understanding
```

### Fancy Indexing
- saw how to use single index values to specify an array item
	- 1-D --> `arr[3]`
	- 2-D --> `arr[2, 5]`
- saw how to use slicing
	- 1-D --> `arr[1:3:2]`
	- 2-D --> `arr[1:3:2, :5]`
- single items at a time
- items that can be defined using slicing
- sometimes not enough - what if we want items (or rows) `1`, `2`, and `4`?
#### One way of solving that problem...
```python
arr = np.array([1, 2, 3, 4, 5, 6])
```
- want an array consisting of elements at indices `0`, `1`, `3` and `5`
```python
sub = np.array([arr[0], arr[1], arr[3], arr[5]])
```
- it works, but a bit clumsy
- but what we really have is an *array of indices*
```python
np.array([0, 1, 3, 5])
```
- and NumPy supports specifying elements using an *array of indices* instead of just a single index - that is what is called *Fancy Indexing*
#### Fancy Indexing
- use an *array of indices* (an *index array*)
```python
arr = np.array([1, 2, 3, 4, 5, 6])
index_array = np.array([0, 1, 3, 5])
sub = arr[index_array] # array([1, 2, 4, 6])
```
- can also just define the index array inline
```python
sub = arr[np.array([0, 1, 3, 5])] # array([1, 2, 4, 6])
```
#### Array Index Shape
- shape of *array index* determines the shape of selection
```python
arr = np.array([1, 2, 3, 4, 5, 6])
arr[np.array([0, 1, 3, 4])] # array([1, 2, 4, 5]) - note here:
# the array index or index_array shape is (4,)
# hence the result is 1-D

# let's see another example where the array index shape is 2-D
arr[np.array([[0, 1], [3, 4]])] # [[1, 2], [4, 5]]
# here the array index shape is (2, 2)
```
#### Fancy Indexing in Multiple Dimensions
- fancy indexing can be applied to multiple axes
```python
row, column
[index_array, index], [index, index_array]
[index_array, slice_], [slice_, index_array]
[index_array, index_array] # this one is most complicated
```
Let's see each one and how they work...
##### `index_array` and `index`

![index array and index](../assets/Pasted%20image%2020250224105409.png)

- notice how the resulting array is 1-D
##### `index_array` and `slice`

![index array and slice](../assets/Pasted%20image%2020250224105533.png)

##### `index_array` and `index_array`
- not commonly used - can be confusing for someone reading your code
- keep index arrays same shape

**1-D and 1-D**
```python
arr[np.array([0, 2]), np.array([1, 3])]
```
- think of this as zipping the indices from the two axes
	- `(0, 1), (2, 3)` --> `[2, 14]`

![index_array and index_array](../assets/Pasted%20image%2020250224110104.png)

**2-D and 2-D**
- again think of this as zipping up indices from both axes
- but now our "index array" is really 2-D as well
```python
arr[np.array([[0, 1], [3, 4]]), np.array([[0, 2], [1, 3]])]
```
- zipping them up the arrays for each axes, we get:
	- `[(0, 0), (1, 2)], [(3, 1), (4, 3)]`
		- result is --> `[[1, 8], [17, 24]`

![2d and 2d index_array and index_array](../assets/Pasted%20image%2020250224110705.png)

> For more insight, [see this](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35151994?start=802#notes).

> You can retype `arr[np.array([0, 1, 3])]` as `arr[[0, 1, 3]]`, but because of code readability when arrays get complicated, use the former.

> Fancy Indexing is not linked with the original array (different from slicing an array). Ex:
```python
arr = np.arange(10, 100, 10)
sub = arr[[0, 2, 3]] # array([10, 30, 40])
sub[0] = 100
sub # array([100, 30, 40])
arr # array([10, 20, 30, 40, 50, 60, 70, 80, 90])
# notice `arr` remains unchanged
```

> There are ways you can *vectorize* your own functions in NumPy, but that's beyond the scope of this course.
> 
> **What is Vectorization in NumPy?** Since NumPy computes code in low-level C, not Python, vectorization is when mathematical or data transformation functions that execute on NumPy arrays (n-dimensional arrays) also compute code in low-level C and output the result back in Python.
> 
> Here is another definition of vectorization:
> **Vectorization** in NumPy is the process of applying operations to entire arrays at once, rather than using loops to process individual elements. It leverages optimized, low-level C code to perform these operations efficiently, making computations faster and code more concise.

### Masking
#### Boolean Masking
- use an expression that evaluates to a Boolean for each element of an array
	- make an array of those `True`/`False` values
	- use that array to filter elements in another array

![boolean masking](../assets/Pasted%20image%2020250224142326.png)

#### Comparison Functions
- functions which can be applied to each element of an array
	- returns an array containing the result of each element
	  ```python
		np.less(arr, value)
		```
	- looks at every element of `arr` and evaluates `element < value`
```python
arr = np.array([1, 2, 3, 4, 5])
np.less(arr, 4) # array([True, True, True, False, False])
```
#### NumPy Logic Functions
- other functions exist:
```python
greater, less_equal, equal, not_equal # and many more...
```

> Learn more about NumPy logic functions here: https://numpy.org/doc/stable/reference/routines.logic.html

- but we can just use comparison operator symbols
	- `< <= > >= == !=`
	- using these will use the NumPy corresponding functions

> NumPy functions allow you to have greater control over how that function is going to get evaluated (being universal functions - in other words, functions that are vectorized).

#### Applying the Mask
- this array of `True`/`False` values is called a *mask*
- we can apply this mask to an array (use same shaped arrays)
	```python
	arr = np.array([1, 2, 3, 4])
	mask = np.array([True, True, False, True])
	```
	- or just use
	  ```python
		mask = arr != 3
		```

	```python
	arr[mask] # array([1, 2, 4])
	```

- can do all this is a single statement
```python
arr[arr != 3]
```
#### Masking 2-D Arrays
- masks will return a 1-D array, even if array being masked is 2-D
	- basically applies mask element by element
```python
arr = np.array([[1, 2], [3, 4]])
mask = arr != 3
mask # array([[True, True], [False, True]])
arr[mask] # array([1, 2, 4])
```
#### Combining Logical Operators
- Python uses `and or not`
- for NumPy we have to use
	- `&` --> `and`
	- `|` --> `or`
	- `~` --> `not` (also called, complement)
- because of operator precedence, use `( )` to group logic expressions
```python
arr = np.arange(-10, 10)
arr[(arr > 0) & (arr % 2 == 0)] # array([2, 4, 6, 8])
```

> `and`, `or`, `not` keywords in Python do something different for NumPy arrays. Instead, for and, or, not operations in NumPy, we use `&`, `|`, and `~` for `and`, `or`, and `not` comparisons operators in NumPy.

### Universal Functions (ufunc)
- earlier we saw that *universal* functions are *vectorized* functions
	- they apply a function to each element of an array
	- the loop and function evaluation are done in C, not Python
	- very fast
		- we'll see how much faster during code
	- NumPy has a large number of universal functions
		- math operations (arithmetic, logs, exponential, sqrt, abs, ...)
		- trig and hyperbolic
		- comparison functions (equal, less than, greater than, min/max, ...)

> You can learn more here: https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs

#### Universal Functions and Operators
- `add`, `subtract`, `multiply`, `divide`, `floor_divide`, `mod`, `power`, ...
- can be called as functions, with at least one argument being an array
	```python
	np.add(arr_1, arr_2)
	np.add(arr_1, scalar)
	```
	- or just use the `+` operator
		- Python will use `np.add()`
	- similarly with `-`, `*`, `/`, `//`, `%`, `**`
#### Array and Array

![array and array](../assets/Pasted%20image%2020250224145321.png)

- keep array shapes the same
	- technically possible to use different shapes
		- broadcasting

> You can learn more here: https://numpy.org/doc/stable/user/basics.broadcasting.html
#### Array and Scalar
- simplest form of broadcasting

![array and scalar](../assets/Pasted%20image%2020250224145447.png)

#### Mismatched Shapes
- sometimes possible
	- not going to focus on this here

![mismatched shapes](../assets/Pasted%20image%2020250224145546.png)

> There's a ton more functionality to NumPy that an introductory course cannot cover - but you should have some basic ideas of how NumPy works, and be able to read the NumPy documentation to look for functionality that you may need for your specific problems.
> 
> One of the reaons why we study NumPy, is that another library, Pandas, is built on top of NumPy. That library is one that again has a ton of functionality, but focused on data sets, which offer more functionality than just plain multi dimensional arrays. We'll look at the Pandas library a littler later.

> See this [Grok chat](https://grok.com/share/bGVnYWN5_6427fbbf-6238-4bc8-bb96-5b511398702c) to learn about ufunc methods such as `np.add.reduce(arr, axis=(row=0, col=0))`.

### Additional Math and Stats Functions
- NumPy has a host of array manipulation and computational functions
	- trig/hyperbolic, logs/exponents
	- linear algebra (matric/vector products, eigenfunctions/values, inverses, etc.)
	- stats (averages, variances, correlations, histograms)
	- discrete Fourier transforms

> Find out more here: https://numpy.org/doc/stable/reference/routines.html

- simple financial functions
	- mainly related to interest calculations
	- slated to be removed by NumPy
		- don't use them
		- https://numpy.org/neps/nep-0032-remove-financial-functions.html
#### Other More Specialized Libraries
- many more specialized libraries
	- usually built on top of NumPy and Pandas
- *SciPy* --> interpolations, optimization, integration, linear algebra, stats ...
- *statsmodels* --> regression, imputation, models, time series, ...
- *pyfolio* --> portfolio performance and risk analysis
- *QuantLib* --> quantitative financial library
- *Quandl* --> useful for getting financial datasets directly into Python (not all datasets are free)
#### Axes
- recall discussion on axes

![axes](../assets/Pasted%20image%2020250224180644.png)

- also called *axes*
		- rows --> axis 0
		- columns --> axis 1
- many of the universal functions in NumPy can operate
	- on the array as a whole
	- along an axis

#### Max
- 1-D is intuitive
```python
np.amax(np.array([1, 2, 3])) # 3
```

![max function in numpy](../assets/Pasted%20image%2020250224184422.png)

- simply runs through all elements of array
---
- we can specify an axis
```python
np.amax(arr, axis=0) # performs the operation across each row (i.e. for each column)
```

![axis direction for max math function in NumPy](../assets/Pasted%20image%2020250224184620.png)

---
```python
np.amax(arr, axis=1) # performs the operation across each column (i.e. for each row)
```

![max performed for each column](../assets/Pasted%20image%2020250224184819.png)

#### Other Functions
- some functions only perform element by element
	- `sin`, `sinh`, `arcsin`, `arcsinh`, `log`, `exp`, `around`, ...
- some functions like `amax`, that operate on groups of data, support axes
	- `amax`, `amin`, `mean`, `median`, `std`, `sum`, `cumsum`, `product`, ...

> Learn more here: https://numpy.org/doc/stable/reference/routines.math.html

#### Histogram
- `np.histogram`
	- creates binned frequency distribution

![histogram in NumPy](../assets/Pasted%20image%2020250224185259.png)

- define bin bounds using left edge, and rightmost edge (which is inclusive)
	- `bins = [0, 3, 8, 10`
- `np.histogram(a, bins_arr)` --> tuple: (array *frequencies*, *bins* array)
- `np.histogram(a, int)`
	- calculates evenly spaced bins in min/max range
	- tuple: (array *frequencies*, *bins* array)
- returns
	- a tuple of `(result, bin_arr)`
- other variants
## Pandas
- *Pandas* is built on top of *NumPy*
	- data manipulation and analysis, focused on tabular and time series data
		- arrays with rows and columns
		- but uses *labels* to identify rows and columns
			- in addition to positional indices
		- columns in the same array can have *different* data types
---
- `Series`
	- 1-dimensional
- `DataFrame`
	- 2-dimensional
	- a collection of `Series` objects
- `Index`
	- used to *index* `Series` and `DataFrame` objects
	- one of the key differences between Pandas and Numpy
		- NumPy array elements are indexed (implicitly) by position
		- in Pandas we can assign our own explicit labels

> Learn more about Pandas here: https://pandas.pydata.org/ and https://pandas.pydata.org/docs/user_guide/index.html
### Indexes
#### What is an Index?
- arrays / lists

![positional index in pandas](../assets/Pasted%20image%2020250226125210.png)

- dictionaries

![dictionaries in pandas](../assets/Pasted%20image%2020250226125303.png)

- an index is a way to "look up" one or more values in an array or dictionary

#### Sequence Types
- sequence types such as Python lists, tuple and NumPy arrays
	- have a natural *positional* order to their elements
	- this forms an *implicit index* on the sequence
	  ![implicit index on sequences](../assets/Pasted%20image%2020250226125519.png)
	```python
	# uses the positional indices
	l[0]
	l[1:4]
	```
	- with Pandas we can define an *explicit* index (in addition to the implicit index)
		![explicit indexing](../assets/Pasted%20image%2020250226125721.png)
		- we'll see how this works later
#### Pandas Indexes
- `pd.Index`
	- most generic type of `Index`
	- they contain elements
	- they are based on *NumPy arrays*
	- they themselves have an *implicit positional index*
```python
idx = pd.Index([10, 20, 30, 40]) # Python list, tuple, NumPy array, ...
idx[0] # 10
# these ones below return an index object
idx[1:4] # Index([20, 30])
idx[[0, 2]] # Index([10, 30])
idx[idx % 4 == 0] # Index([20, 40])
```
#### Specialized Indexes
- Int64 indexes `Int64Index()` --> for indexes that contain integer indices
- Float64 indexes `Float64Index()` --> for indexes that contain float indices
- Range indexes `RangeIndex()` --> for integer sequence defined via a range
	- similar to difference between Python `list` and `range`
	  ```python
		[0, 1, 2, 3, 4, 5] # here, sequence is materialized
		range(6) # here, sequence is not materialized

		# a range index in Pandas is created in one of two ways:
		pd.Index(range(1, 10, 2)) # via a Python range object
		pd.RangeIndex(1, 10, 2) # or directly in Pandas
		```
		- elements are produced as requested when iterating
	- Range indexes can be more efficient (storage and computation)
#### Indexes have Set-Like Properties
- can find the union and intersection of indexes
	- `&` --> intersection
	- `|` --> union
	- `in` --> element of
- Pandas will use broadest data type needed for union/intersection
- `RangeIndex` indexes will try to return a `RangeIndex` as a result of union / intersection
	- not always possible
#### String Integer and Float Indexes
- strings will result in an `Index` object, with an `object` data type (a catchall type)
```python
pd.Index(['a', 'b', 'c'])
```
 - integers will result in an `Int64Index` object
```python
pd.Index([1, 2, 3])
```
 - floats will result in a `Float64Index` object
```python
pd.Index([0.1, 0.2, 0.3])
```
#### Range Indexes
- can create using the Python `range` object
```python
pd.Index(range(1, 10, 2))
```
- can use Pandas `RangeIndex` class directly
```python
pd.RangeIndex(start, stop, step)
```

> Not all unions and intersections of ranges can be expressed as a new range, so sometimes we end up with a regular index, not a range index:
```python
pd.RangeIndex([1, 10, 2]) | pd.RangeIndex([1, 10, 3]) # returns...
# Int64Index([1, 3, 4, 5, 7, 9], dtype='int64')
```

---
- index values do not have to be unique
```python
pd.Index([1, 1, 2, 2]) # perfectly legal
```
- but if we associate an index with a sequence, how does a non-unique index work?

![index values do not have to be unique](../assets/Pasted%20image%2020250226131608.png)

### Series
#### Python Sequences, NumPy Arrays
- associative arrays

![python sequences](../assets/Pasted%20image%2020250226134645.png)

- there is an association between the index and the values --> *associative arrays*
	- in Python lists, tuples, NumPy arrays, this positional index is *implicit*
	- index provides a *unique mapping* between indices and values
#### Python Dictionaries
- another type of associative array
	- mapping between *keys* and *values*
	- keys are *not* positional based
		- do not even have to be numbers
	- but it's still an associative array
	```python
	d = {'a': 1, 'b': 2, 'c': 3}
	# see the index
	# 'a' --> 1
	# 'b' --> 2
	# 'c' --> 3
	```
	- *unique* index
	- no *implicit* positional index
#### Pandas Series
- another type of associative array
	- has some dictionary-like properties
	- has some sequence-like properties
- it's a sequence type - so elements have a definite position in collection 
	- positional index
- can also define an explicit index
	- a second index

![pandas series example](../assets/Pasted%20image%2020250226135326.png)

---
- can reference items by positional indices `[0] [1] ...`
- or by using the explicit index `['a'] ['b'] ...`
	- can even use slicing and fancy indexing
		- even with an explicit index that is not numerical `['a': 'c']`
---
- indexing works as expected
- unlike Python `dict` however, the index of a `Series` can contain repeated elements
- slicing has a twist
	- positional index `[0:5]` --> *excludes* endpoint
	- explicit index `['a':'c']` --> *includes* endpoint

![pandas series and slicing](../assets/Pasted%20image%2020250226135810.png)

#### A point of confusion ...
```python
# 0    1    2    3 - implicit index
[100, 200, 300, 400]
# 2    3    4    5 - explicit index
```
- `[2]` --> is this using implicit index?
- `[2:3]` --> or explicit index?
if both implicit and explicit index are integers:
- `[2]` --> uses *explicit* index
- `[2:3]` --> uses *implicit* index

#### `loc` and `iloc` Attributes
- allows us to specifically indicate use of implicit or explicit index

![loc and iloc attributes](../assets/Pasted%20image%2020250226140310.png)

- `s.iloc[2]` --> uses implicit index
- `s.loc[2]` --> uses explicit index
	- note the *square* brackets `[ ]`, not parenthesis `( )`
#### Deleting Items
- indexes are immutable
	- deleting an item would require deleting the corresponding index value
	- instead use `.drop()` method
	- returns a *new* series with new explicit index
#### Creating Series Objects
```python
from pandas import Series
```

![creating series objects](../assets/Pasted%20image%2020250226140804.png)

- from a dictionary
```python
Series({'a': 1, 'b': 2})
```
- from a list, specifying explicit index using another list
```python
Series([1, 2], index=['a', 'b'])
```
#### Series Attributes and Methods
- `.index` --> returns the explicit Index object
- `.values` --> returns a NumPy array of the values
- `.items` --> zip of explicit index values and array values
- `.iloc` --> used for indexing using implicit index
- `.loc` --> used for indexing using explicit index
- `.drop` --> used to remove an element by explicit index

> We can also use Boolean Masking (which remember will use the values, not the index when calculating conditional logic expressions), so no confusion regarding which index it uses - it does not use any.
```python
areas[areas != 'Glasgow']
```

> We have seen how to select and mutate values in a series. How do we delete an item? It's not as straightforward as you might think, since we also have an (immutable) explicit index associated with the series. Instead, we can use the `drop()` method, specifying the indices we want to drop from the series, which will return a **new** series, but without affecting the original:
```python
s = pd.Series([10, 20, 30], index=list('abc'), name='test')
new = s.drop(['a', 'c']) # returns...
# b 20
# Name: test, dtype: int64
# remember, despite the `drop()` function applied, `s` is untouched
```

> Can we drop by position? Not directly, no. But we can recover the explicit index value for a specific location. Remember how we studied Indexes in a previous set of lectures? The Index is just another series, with implicit positional indexing.
```python
s.index # Index(['a', 'b', 'c'], dtype='object')

# we can get the explicit index value for a specific (or set of specific) positional indices:

s.index([0, 2]) # Index(['a', 'c'], dtype='object')

# we can now use this in our drop() call:

s.drop(s.index([0, 2])) # returns...
# b    20
# Name: test, dtype: int64

# again though, the original series `s` is not affected:

s # returns...
# a    10
# b    20
# c    30
# Name: test, dtype: int64
```

### DataFrames
- `Series` --> analogous to 1-D NumPy array with an explicit index
- `DataFrame` --> analogous to 2-D NumPy array with an explicit index
	- for the rows
	- and for the columns
another way to look at it...
- a `DataFrame` is a collection of `Series` objects
	- a *common* explicit index for the rows --> series are *aligned*
	- the columns (Series) form a Series too
		- explicit index
		- column names possibly

![dataframes](../assets/Pasted%20image%2020250226145301.png)

- you can think of it as a Series of Series
	- or a dictionary of dictionaries

![dictionary of dictionaries](../assets/Pasted%20image%2020250226145406.png)

#### Constructing a `DataFrame`
```python
pd.DataFrame(...)
```
- from a list of Series objects
- from a list of lists
- from a list of dictionaries
- from a dictionary of Series objects
- from a dictionary of dictionaries
	- in some cases row and column explicit indexes are created as expected
	- in some cases we may have to define these indexes manually
#### Some `DataFrame` Properties and Methods
- `.info()` --> prints some useful info about the data frame
- `.transpose()` --> transposes the data frame, maintaining indexes
- `.raname()` --> allows us to rename the index labels (rows and/or columns)
- `.set_index()` --> use an existing column in the data frame as a row index
- `.index` --> the Index object used to index the rows
- `.columns` --> the Index object used to index the columns
- `.drop()` --> used to drop rows/columns from the data frame

> We can rename the row indices (the labels) of a data frame by using the `rename()` method, where we specify the old label and the new label using a dictionary:
```python
import pandas as pd

new_york = pd.DataFrame([countries, populations, gdp, areas])

new_york.rename(
	# renaming row index
	index={0: 'county', 1: 'population', 2: 'gdp', 3: 'area'}
	
	# renaming column index
	columns={0: 'county', 1: 'population', 2: 'gdp', 3: 'area'}
)
```

> We can set the index (row index) of a data frame using the `set_index()` method. This allows us to pick an existing column to become the row index:
```python
new_york.set_index('burroughs') # makes 'burroughs' column to become row index
```

> Data frames have many properties and methods, some of which we'll study later in this section, but you can read up more on them in the Pandas documentation: 
> https://pandas.pydata.org/pandas-docs/stable/reference/frame.html

> To drop row or columns in data frames, we use `drop()` method, but this time, we specify `index` to drop row indices and `columns` to drop column indices (same idea goes for the `rename()` method):
```python
# dropping row indices
new_df = new_df.drop(index=['Brooklyn', 'Queens'])

# dropping column indices
new_df = new_york.drop(columns='county')
```

### Selecting Data
#### DataFrames
- analogous to a `Series` of `Series`

![data frames axis 0 and 1](../assets/Pasted%20image%2020250226204417.png)

- or a dictionary of lists / dictionaries
	```python
	{
		"col0": {
			"row0": value,
			"row1": value
		},
		"col1": {
			"row0": value,
			"row1": value
		}
	}
	```
	- a *sequence* of *aligned columns*
---
- consider it as a `Series` of `Series`

	![selecting data](../assets/Pasted%20image%2020250226205040.png)

	- all share a common row index `['r1', 'r2', 'r3']` 
		- `c1` is a series of values
		- `c2` is a series of values
		- `c3` is a series of values
	- `df` is like a Series `[c1, c2, c3]` with index `['c1', 'c2', 'c3']`
		- or like a like a dictionary `{'c1': c1, 'c2': c2, 'c3': c3}`
- `df['c1']`
	- this selects the item with label `'c1'`
		- the *column* (series) `c1`
- note that `[ ]` cannot be used with positional indices with `DataFrame` objects
#### `loc` and `iloc`
- just like with `Series`, but with 2 axes
	- `loc` uses the *explicit* index
	- `iloc` uses the *implicit* (*positional*) index
- but think of `DataFrame` like a NumPy *array* with *two axes*

![data frames use axis 0 and 1 same as numpy arrays](../assets/Pasted%20image%2020250226205753.png)

- slicing and fancy indexing works the same way as with `Series`, but using 2 axes
#### Replacing Values
- can replace using assignment (`=`) operator
- replace *single* selected cell
	- with a *scalar* value
- replace *multiple* cells selected using slicing/fancy indexing
	- with a 2-D NumPy array / list of lists of *same shape*
	- with a *scalar* value that will be *broadcast*
	- with a 1-D NumPy array that will be *broadcast*
	- can replace with a `Series` or `DataFrame` but indexes can cause issues!

> You can replace values in the data frame using an assignment operation, just like with `Series` and NumPy arrays. We can also replace with another Pandas `DataFrame` or `Series`, but when we do we have to be careful because of the explicit indexes! Consider this:
```python
ser = pd.Series([-10, -20], index=['n1', 'n2'])

# let's replace a slice of same shape in `df`
df.loc[0:2, 0:2] = ser
df # returns... (notice the `NaN` values inplace of the assignment values)
# |c1|c2|c3|
# |---|---|---|
# |r1|NaN|NaN|2.0|
# |r2|NaN|NaN|5.0|
# |r3|6.0|7.0|8|

# the reason for this is that the index on the series `ser` did not match any index in `df`
# if we truly want to replace the values without worrying about the index on `ser`,
# we can do it this way:
df.loc[0:2, 0:2] = ser.values
df # returns...
# |c1|c2|c3|
# |---|---|---|
# |r1|-10.0|-20.0|2.0|
# |r2|-10.0|-20.0|5.0|
# |r3|6.0|7.0|8.0|
```

> We can also use boolean masking to select elements but we'll cover that later. Pandas data selection can get more complicated. If you're interested in reading up more on it, you can look at the Pandas docs: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html

### Missing Data
#### In Python
- `None` object
	- can be used to indicate undefined or missing in a sequence `[1, 2, None, 4]`
- IEEE standard for floats also has the concept of an *undefined* float
	- *NaN* (not a number)
		- `float('nan')`
		- `math.nan`
		- `np.nan`
#### Equality of NaN
- two *NaN* values always compare `False`
	- cannot compare two undefined (unknown) values...
	  ```python
		a = math.nan
		b = math.nan
		a == b # False
		a is b # False
		```
	- so how do we test if a number is NaN?
	  ```python
		math.isnan()
		math.isnan(np.nan) # True
		```
	- NumPy *universal* function `np.isnan()`
#### Pandas Series
- if the series is a series of floats
	- `nan` --> `nan`
	- `None` --> `nan`
```python
pd.Series([1, 2, None, np.nan]) # [1.0, 2.0, NaN, Nan], dtype=float64
# notice the series was made into a float
```
- if the series is a series of `object` (for example for series of strings)
```python
pd.Series(['a', 'b', None, np.nan]) # ['a', 'b', None, NaN], dtype=object
```
#### Testing for Missing Data
- could be `None` --> could be `Nan`
- `pd.isnull()`
	- handles *both*
	- universal function (operates on `Series` or `DataFrames`)
		- returns element by element comparison
			- `True` if value is `None` or `NaN`
- `pd.notnull()`
	- similar to `isnull()`, but opposite result
#### Replacing `Series` Missing Data
- use loops to iterate and replace missing values
- specialized Pandas functions
	- `s.fillna(value)` --> replaces any null with specified `value`
	- `s.fillna(method=...)`
		- `method=ffill`
			- forward fill
				![forward fill](../assets/Pasted%20image%2020250227155223.png)
				- `null, 1, null, 2, null, null`
				- `[null, 1, 1, 2, 2, 2]`
		- `method=bfill`
			- backward fill
#### Replacing `DataFrame` Missing Data
- works same as `Series` replacement
	- but the `axis` is important for back/forward fills

![replacing dataframes with missing data](../assets/Pasted%20image%2020250227155359.png)

#### Interpolating Missing Data
- more advanced techniques
	- linear interpolation
	- splines ...
- beyond the scope of this course
- but we'll look at simple linear interpolation in code 

> To learn more, see here: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.interpolate.html

#### Dropping Data
- already saw this for `Series` objects 
- `DataFrame` is 2-D

![dropping data](../assets/Pasted%20image%2020250227155821.png)

- do we deleting rows with missing values?
- or do we delete columns with missing values?
	- need to specify an *axis*
	  ```python
		df.dropna(axis=0) # default of `axis` is 0 (if not specified)
		df.dropna(axis=1)
		```
		- axis defaults to `0` if not specified
### Loading Data
- Pandas has built-in functions for loading many types of data
- in this lecture we'll look at
	- CSV files 
	- Excel files
- many other data sources are supported (SQL, JSON, SAS, SPSS, etc)

> You can learn more here: https://pandas.pydata.org/pandas-docs/stable/reference/io.html

#### Loading CSV File
- `pd.read_csv(<file_name>)`
- has many optional arguments
	- `sep` and `delimiter` (just like Python's `csv.reader`)
	- `header` row number to use as column labels, otherwise infers them
	- `usecols` a list of positional indexes indicating which columns to keep
	- `names` renames the columns
	- `index_col` specifies (by name or index) which columns to use as the row index

> You can learn more here: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html

#### Loading an Excel File
- Pandas relies on external 3rd party libraries to read Excel files
	- many exist, such as `xlrd`, `openpyxl`
	- need to `pip install` the library in your virtual env
- `pd.read_excel('file_name')`
	- `sheet_name` the sheet name or index (zero based) to load
	- `header`
	- `usecols`
	- `names`
	- `index_col` and more...

> Spreadsheets often have multiple tabs, so one of the arguments to the `read_excel` function is which tab to use, either by index (starting at `0`, and the default setting), or by name (such as `Sheet1`, etc.).
> 
> In order for Pandas to read Excel spreadsheets, an additional library needs to be installed that will handle reading Excel files - one like the `xlrd` library is a possible choice (there are others. You can learn more here: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html

> There is a whole plethora of different readers available (pickle, csv, etc.) to load data from a variety of sources, which you can see here: [https://pandas.pydata.org/pandas-docs/stable/reference/io.html](https://pandas.pydata.org/pandas-docs/stable/reference/io.html)

> Python's `csv` module loads a csv file into a list of lists, Pandas loads a csv file into a `DataFrame`.

### Basic Data Analysis
- basic facts about a loaded data set
	- `.info()` --> column names, types, not-null counts
	- `.describe()` --> mean, min, max, quartiles, std dev
		- by default only includes numerical columns
		- `include='all'`
			- categorical columns
				- *#* unique values
				- most frequent value + frequency
	- output is "print" output
---
- equivalent methods to obtain the same data
	- `.nunique()` --> *#* of unique values
	- `.unique()` --> array of unique values
	- `.value_counts()` --> Series of values and their frequency (returns a `Series` containing the frequency of each distinct row in the data frame.)
	- `.count()`
	- `.mean()`
	- `.std()`
	- `.quantile()`

> `DataFrame.head()` displays the first 5 rows of a data frame if `n` value is not passed in.

> When there are just too many columns (based on the setting `pd.options.display.max_info_columns`), you will not see all the columns listed as we have seen before (using the `.info()` method). By default max columns is set to `100`. See:
```python
import pandas as pd
pd.options.display.max_info_columns # 100

# two ways we can change this Pandas setting is:
# 1. using the `set_option` method
pd.set_option('display.max_columns', None)
# 2. reassigning the attribute directly:
pd.options.display.max_info_columns = None
```

> If we don't want to change the `pd.options.display.max_info_columns` value, we can pass `verbose=True` argument into the `.info()` DF method.
> 
> Using this method, we may still be missing the null counts for this display. We can specify they be included by using the `show_counts=True` argument (`null_counts=True` was the old argument setting, with new Pandas update the new and accurate argument to use is `show_counts=True`). 

> When we use `DataFrame.describe()`, Pandas does not run analysis for non-numerical data within our dataset. If however, you are interested in categorical data, and understanding the number of unique values in the column, you can tell Pandas to include all columns using the `include='all'` argument. This will show `unique`: number of unique elements in the column, `top`: unique element in the column with the most count frequency, `count`: most count frequency of unique element.

### Sorting and Filtering
#### Filtering
- boolean masking
	- works similarly to NumPy and Series masking
	- create a boolean masking array
	- apply mask to data frame
	- use explicit or implicit index
	  ```python
		mask = df['col'] >= 0
		mask = df.iloc[:, 2] >= 0
		df[mask]
		```
#### Sorting
- sort rows based on the row index labels
```python
df.sort_index()
```
- sort rows based on values in a column
```python
df.sort_values('col label')
```
- similarly to Python's `sorted()` function, these support a `key` argument
#### Reviewing `sorted(key=...)`
```python
l = ['Z', 'a', 'b']
sorted(l, key=lambda x: x.casefold())
```
- `key` is a function that transforms each element of `l`, *one by one*
	```python
	l = ['Z', 'a', 'b']
	sort_keys = ['z', 'a', 'b']
	```
	- sorting is then based on `sort_keys`
		- *sort by an associated series of keys*
#### The `key` Argument for DataFrames
the `key` argument for data frames is the same thing
- *sort by an associated series of keys*
- instead of using a function that generates the keys one by one
	- use a *vectorized function* that generates the sequence of sort keys all at once
		- key function *receives* a `Series` as its argument
		- should *return* a `Series` object with same shape
		  ```python
			s = Series([1, -1, 2, -2])
			key = np.abs(s) # Series([1, 1, 2, 2])
			```
#### Sorting by Index

![sorting by index](../assets/Pasted%20image%2020250301095417.png)

```python
df.sort_index(key=sort_func)
# or
df.sort_index(key=lambda ind: ind.str.casefold()) # notice here that
# `.casefold()` method is abstracted/obfuscated behind the `str` namespace.  
# this is a similarity drawn across object manipulation methods in Pandas
```
#### Sorting by Values
- same as sorting by index
	- uses some specified column instead of index

![sorting by values](../assets/Pasted%20image%2020250301095818.png)

```python
df.sort_values('c1')
```
- sorts based on values in `c1`

![sorting by values - index is preserved](../assets/Pasted%20image%2020250301095914.png)

#### Sorting by Values with a `key`
- `key` function receives the sort by column (`Series`) as its argument

![sorting by values with a key](../assets/Pasted%20image%2020250301100115.png)

```python
df.sort_values('c1', key=lambda col: np.abs(col))
```
- `key` function receives column `c1` as its argument
	- returns a new `Series` --> `1, 40, 7`

![sorting by values with a key - result](../assets/Pasted%20image%2020250301100315.png)

#### Sorting on Multiple Columns
- can specify a multi-level sort based on multiple columns

![sorting on multiple columns](../assets/Pasted%20image%2020250301100434.png)

- `df.sort_values('c1')`
	- stable sort based on `c1` column

![stable sort based on c1 column](../assets/Pasted%20image%2020250301100544.png)

- `df.sort_values(['c1', 'c2'])`
	- sorts on `c1`, then `c2`

![multi-level sort based on multiple columns](../assets/Pasted%20image%2020250301100755.png)

### Manipulating Data















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

![tuples](assets/Pasted%20image%2020241231211917.png)

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

![negative steps - slicing](assets/Pasted%20image%2020250102212933.png)

To recap:
![slicing summary](assets/Pasted%20image%2020250102213236.png)

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
	- ![replacing an entire slice in python](assets/Pasted%20image%2020250103152242.png)
- Python uses the *elements* of the sequence in RHS when assigning to a *slice* (but not when assigning using a single index)

> You can also replace an entire slice in reverse order but it acts a bit weird when you do it. Therefore, minimize this application as much as possible. [Learn more](https://www.udemy.com/course/python3-fundamentals/learn/lecture/35150726?start=354#notes).

#### Deleting Elements
- can delete an element by *index*
	- ![delete element by index](assets/Pasted%20image%2020250103152640.png)
- can delete an element by *slice*
	- ![delete element by slice](assets/Pasted%20image%2020250103152709.png)

#### Appending Elements
- we can *append* one element
	- `my_list = [1, 2, 3]`
	- `my_list.append(4)`
	- `my_list` → `[1, 2, 3, 4]`
- to append multiple elements, we *extend* the sequence
	- `my_list = [1, 2, 3]`
	- ![append elements using extend](assets/Pasted%20image%2020250103153028.png)

#### Inserting an Element
- instead of appending, we could *insert* at some index
	- use sparingly - this is much slower than appending or extending
- ![inserting an element into a sequence](assets/Pasted%20image%2020250103160322.png)
- ![inserting an element into a sequence - 2](assets/Pasted%20image%2020250103160412.png)
- element is inserted so its position is the *index* - remaining elements are shifted right

> When *inserting* or *appending* items it takes it as is not as a sequence.
> For example:
> using *append* `[1, 2, 3, 4].append('python')` → `[1, 2, 3, 4, 'python']`
> using *insert* `[1, 2, 3, 4].insert(0, 'abc')` → `['abc', 1, 2, 3, 4]` 

> To test how long a program runs in Python use:
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
- ![shallow copy](assets/Pasted%20image%2020250103163026.png)
- `origial` and `shallow_copy` are *not* the same containers
- but the elements are referencing the *same* objects
- add/remove/replace element in one does not affect the other
	- ![shallow copy 2](assets/Pasted%20image%2020250103163219.png)
- but mutating an element will affect both (since it is a shared reference)
	- ![shallow copy 3](assets/Pasted%20image%2020250103163329.png)

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
	- ![unpacking sequences](assets/Pasted%20image%2020250103174402.png)

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







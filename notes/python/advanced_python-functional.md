## Variables and Memory
### Python Optimizations: Interning


### Python Optimizations: String Interning


### Python Optimizations: Peephole


## Function Parameters
### Extended Unpacking


### Parameter Defaults


## First-Class Functions
### Function Introspection
#### Functions are First-Class Objects
- they have attributes
	- `__doc__`
	- `__annotations__`
- we can attach our own attributes (as seen below)
```python
def my_func(a, b):
	return a + b

my_func.category = 'math'
my_func.sub_category = 'arithmetic'

print(my_func.category) # 'math'
print(my_func.sub_category) # 'arithmetic'
```
#### The `dir()` Function
`dir()` is a built-in function that, given an object as an argument, will return a **list of valid attributes** for that object
![the dir() function](../assets/Pasted%20image%2020250206135121.png)
#### Function Attributes: `__name__`, `__defaults__`, `__kwdefaults__`
- `__name__` --> name of function
- `__defaults__` --> tuple containing positional parameter defaults
- `__kwdefaults__` --> dictionary containing keyword-only parameter defaults
```python
def my_func(a, b=2, c=3, *, kw1, kw2=2):
	pass

my_func.__name__ # my_func
my_func.__defaults__ # (2, 3)
my_func.__kwdefaults__ # {'kw2': 2}
```
#### Function Attribute: `__code__`
```python
def my_func(a, b=1, *args, **kwargs):
	i = 10
	b = min(i, b)
	return a * b

my_func.__code__ # <code object my_func at 0x00020EFF...>
```
- this `__code__` object itself has various properties, which include:
	- `co_varnames`
		- parameter and local variables
		- `my_func.__code__.co_varnames` --> `('a', 'b', 'args', 'kwargs', 'i')`
		- parameter names *first*, followed by local variable names 
	- `co_argcount`
		- number of parameters
		- `my_func.__code__.co_argcount` --> `2`
		- *does not count* `*args` *and* `**kwargs`*!*

> You can list the properties and attributes of the `__code__` object by using the `dir()` function. Like so:
```python
dir(my_func.__code__)
```

#### The `inspect` Module
```python
import inspect
from inspect import ismethod, isfunction, isroutine

ismethod(obj), isfunction(obj), isroutine(obj)
```
- 

### Callables


### Reducing Functions


### Partial Functions


### The `operator` Module


## Scopes, Closures and Decorators
### Global and Local Scopes


### Nonlocal Scopes


### Decorator Application (Logger, Stacked Decorators)


### Decorator Application (Memoization)


### Decorator Factories


### Decorator Application (Class)


### Decorator Application (Decorating Classes)


### Decorator Application (Dispatching)


## Tuples as Data Structures and Named Tuples
### Tuples as a Data Structure


### Named Tuples



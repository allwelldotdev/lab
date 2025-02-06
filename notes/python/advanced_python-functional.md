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
from inspect import ismethod, isfunction, isroutine # and many more...
# again, the can list the available properties, attributes, and methods
# in `inspect` using `dir(inspect)`

ismethod(obj), isfunction(obj), isroutine(obj)
```
what's the difference between a *function* and a *method*?
- classes and object have *attributes* - an object that is bound (to the class or the object)
- an attribute that is *callable*, is called a *method*
```python
def my_func():
	pass

def MyClass:
	def func(self):
		pass

my_obj = MyClass()
```
- `func` is bound to `my_obj`, an instance of `MyClass`
	- `isfunction(my_func)` --> True
	- `ismethod(my_func)` --> False
	- `isfunction(my_obj.func)` --> False
	- `ismethod(my_obj.func)` --> True
	- `isroutine(my_func)` --> True
	- `isroutine(my_obj.func)` --> True
		- both `isroutines()` work out to `True` because `isroutine()` checks whether the object passed in is a *function* or *method*
#### Code Introspection
- we can recover the source code of our functions/method
```python
inspect.getsource(my_func) # a string containing our entire `def` statement,
# including annotations, docstrings, etc.
```
- we can find out in which module our function was created
```python
inspect.getmodule(my_func) # <module '__main__'>
inspect.getmodule(print) # <module 'builtins' (built-in)>
inspect.getmodule(math.sin) # <module 'math' (built-in)>
```
#### Function Comments
```python
# setting up variable
i = 10

# TODO: Implement function
# some additional notes
def my_func(a, b=1):
	#commend inside my_func
	pass

inspect.getcomments(my_func) # '# TODO: Implement function\nsome additional notes'
```
- many IDE's support the **TODO** commend to flag functions and other callables
- note that this is not the same as docstrings
#### Callable Signatures
```python
inspect.signature(my_func) # Signature instance
```
- contains an attribute called `parameters`
	- essentially a dictionary of parameter names (keys), and metadata about the parameters (values)
		- `keys` --> parameter name
		- `values` --> objects with attributes such as `name`, `default`, `annotation`, `kind`
- `kind` refers to the type of argument
	- `POSITIONAL_OR_KEYWORD`
	- `VAR_POSITIONAL`
	- `KEYWORD_ONLY`
	- `VAR_KEYWORD`
	- `POSITIONAL_ONLY`
---
```python
def my_func(a: 'a string',
			b: int = 1,
			*args: 'additional positional args',
			kw1: 'first keyword-only arg',
			kw2: 'second keyword-only arg' = 10,
			**kwargs: 'additional keyword-only args') -> str:
	"""does something
	or other"""
	pass

for param in inspect.signature(my_func).parameters.values():
	print('Name:', param.name)
	print('Default:', param.default)
	print('Annotation:', param.annotation)
	print('Kind:', param.kind)
```

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



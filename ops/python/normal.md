## \* & \*\*
The `*args` and `**kwargs` are common idioms to allow an arbitrary number of arguments to functions
- The `*args` will give you all positional arguments as a tuple. `args` can be any iterable object
- The `**kwargs` will give you all keyword arguments as a dictionary
Both idioms can be mixed with normal arguments to allow a set of fixed and some variable arguments:

```python
def foo(kind, *args, bar=None, **kwargs):
    print(kind, args, bar, kwargs)

foo(123, 'a', 'b', apple='red')
# 123 ('a', 'b') None {'apple': 'red'}
```

It is also possible to use this the other way around:

```python
def foo(a, b, c):
    print(a, b, c)

obj = {'b':10, 'c':'lee'}

foo(100, **obj)
# 100 10 lee
```

Another usage of the `*l` idiom is to **unpack argument lists** when calling a function.

```python
def foo(bar, lee):
    print(bar, lee)

baz = [1, 2]

foo(*baz)
# 1 2
```

In Python 3 it is possible to use `*l` on the left side of an assignment
```python
a, *b = [1,2,3,4]
# a will be 1
# b will be [2,3,4]
```
Note: The keys in `kwargs` have to be named exactly like the parameters of  the function `foo`
- In a **function call** the '\*' **unpacks** data structure of tuple or list into positional or keyword arguments to be received by function definition.
- In a **function call** the '\*\*' **unpacks** data structure of dictionary into positional or keyword arguments to be received by function definition.
- In a **function definition** the '\*' **packs** positional arguments into a tuple.
- In a **function definition** the '\*\*' **packs** keyword arguments into a dictionary.

## try-except

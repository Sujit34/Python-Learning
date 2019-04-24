#### Simple Decorators

Now that you’ve seen that functions are just like any other object in Python, you’re ready to move on and see the magical beast that is the Python decorator. Let’s start with an example:

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_whee():
    print("Whee!")

say_whee = my_decorator(say_whee)
say_whee()

Output:
Something is happening before the function is called.
Whee!
Something is happening after the function is called.
```

##### Syntactic Sugar!
Python allows you to use decorators in a simpler way with the @ symbol, sometimes called the “pie” syntax. The following example does the exact same thing as the first decorator example:

```python

def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_whee():
    print("Whee!")
```

So, @my_decorator is just an easier way of saying say_whee = my_decorator(say_whee). It’s how you apply a decorator to a function.

#### Nesting Decorators
You can apply several decorators to a function by stacking them on top of each other:

```python
from decorators import debug, do_twice

@debug
@do_twice
def greet(name):
    print(f"Hello {name}")
greet("Eva")
```
Think about this as the decorators being executed in the order they are listed. In other words, @debug calls @do_twice, which calls greet(), or debug(do_twice(greet())).

Observe the difference if we change the order of @debug and @do_twice:

```python
from decorators import debug, do_twice

@do_twice
@debug
def greet(name):
    print(f"Hello {name}")
greet("Eva")
```
In this case, @do_twice will be applied to @debug as well.

#### Decorators With Arguments

Sometimes, it’s useful to pass arguments to your decorators. For instance, @do_twice could be extended to a @repeat(num_times) decorator. The number of times to execute the decorated function could then be given as an argument.

This would allow you to do something like this:

```python

@repeat(num_times=4)
def greet(name):
    print(f"Hello {name}")
greet("World")
```

#### Classes as Decorators

The typical way to maintain state is by using classes. In this section, you’ll see how to rewrite the @count_calls example from the previous section using a class as a decorator.

Recall that the decorator syntax @my_decorator is just an easier way of saying func = my_decorator(func). Therefore, if my_decorator is a class, it needs to take func as an argument in its .__init__() method. Furthermore, the class needs to be callable so that it can stand in for the decorated function.

For a class to be callable, you implement the special .__call__() method:

```python
class Counter:
    def __init__(self, start=0):
        self.count = start

    def __call__(self):
        self.count += 1
        print(f"Current count is {self.count}")
counter = Counter()
counter()
```
The .__call__() method is executed each time you try to call an instance of the class.

Therefore, a typical implementation of a decorator class needs to implement .__init__() and .__call__()

```python
import functools

class CountCalls:
    def __init__(self, func):
        functools.update_wrapper(self, func)
        self.func = func
        self.num_calls = 0

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print(f"Call {self.num_calls} of {self.func.__name__!r}")
        return self.func(*args, **kwargs)

@CountCalls
def say_whee():
    print("Whee!")

say_whee()

output:
Call 1 of 'say_whee' 
Whee!
```

The .__init__() method must store a reference to the function and can do any other necessary initialization. The .__call__() method will be called instead of the decorated function. It does essentially the same thing as the wrapper() function in our earlier examples. Note that you need to use the functools.update_wrapper() function instead of @functools.wraps.

This @CountCalls decorator works the same as the one in the previous section.

 > source: www.realpython.com

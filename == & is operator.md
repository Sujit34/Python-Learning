## The == and is Operators

Python has the two comparison operators == and is. At first sight they seem to be the same, but actually they are not. == compares two variables based on their actual value. In contrast, the is operator compares two variables based on the object id and returns True if the two variables refer to the same object.

The next example demonstrates that for three variables with integer values. The two variables a and b have the same value, and Python refers to the same object in order to minimize memory usage.


>>> a = 1
>>> b = 1
>>> c = 2
>>> a is b
True  
>>> a is c
False  
>>> id(a)
10771520  
>>> id(b)
10771520  

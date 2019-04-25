
## Enumerate() in Python

A lot of times when dealing with iterators, we also get a need to keep a count of iterations. Python eases the programmers’ task by providing a built-in function enumerate() for this task.
Enumerate() method adds a counter to an iterable and returns it in a form of enumerate object. This enumerate object can then be used directly in for loops or be converted into a list of tuples using list() method.


```python

Syntax:

enumerate(iterable, start=0)

Parameters:
Iterable: any object that supports iteration
Start: the index value from which the counter is 
              to be started, by default it is 0 
```

```python

# Python program to illustrate 
# enumerate function in loops 
l1 = ["eat","sleep","repeat"] 
  
# printing the tuples in object directly 
for ele in enumerate(l1): 
    print ele 
print 
# changing index and printing separately 
for count,ele in enumerate(l1,100): 
    print count,ele 

Output:
(0, 'eat')
(1, 'sleep')
(2, 'repeat')

100 eat
101 sleep
102 repeat
```

 > Source: Geeksforgeeks

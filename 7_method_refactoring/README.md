# Sample Refactoring

In this section we will look at how to edit the following code to convert it to
a class method, make a similar method, and fix several style problems.

```python
def fact(n):
   myResult = 1
   theValues = list(range(n))
   for i in range(len(theValues)):
      nextVal = theValues[i]
      myResult = myResult * nextVal
   return myResult
```

We want to make the following changes:
 - promote the method to a class method which takes the parameter n, call it `NaturalFunctions`
 - rename `myResult` to `result`, remove the non-pythonic for loop and unnecessary
   variables
 - change the function name to `factorial`
 - add a similar method `cumulative_sum` which returns the sum
 - make the indentation a multiple of 4, currently is 3

At the end it should look like this
```python
class NaturalFunctions:
    def __init__(self, n):
        self.n = n

    def factorial(self):
        result = 1
        for i in range(self.n):
            result = result * i
        return result

    def cumulative_sum(self):
        result = 0
        for i in range(self.n):
            result = result + i
        return result
```
and took me about 2 minutes

On your marks, get set, go!
```python
def fact(n):
   myResult = 1
   theValues = list(range(n))
   for i in range(len(theValues)):
      nextVal = theValues[i]
      myResult = myResult * nextVal
   return myResult
```

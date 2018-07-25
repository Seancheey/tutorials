
Numpy array functions
===
Here I will discuss a bunch of simple functions in numpy about array. By default, I assume that numpy is imported as `import numpy as np`.

1: Creating array/matrix
---
**Normal Way**: directly import from standard python array
```python
a = np.array([1,2,3])
```
**Numpy functions**: zeros, ones, full, random.random,eye
```python
np.zeros((3,2)) # 3 rows 2 columns of 0's
np.ones((4,3)) # 4 rows 3 columns of 1's
np.full((3,2),7) # 3 rows 2 columns of 7's
np.random.random((2,3)) # 2 rows 3 columns of random number between 0 and 1
np.eye(4) # create a 4x4 identity matrix "I"
```
2: Getting shapes of numpy array/matrix
---
You can get the shape of an array by using `my_array.shape`. It returns the dimensions of the array/matrix.
```python
print(np.array([1,2,3]).shape)
print(np.array([[1,2],[3,4],[5,6]]).shape)
```
When you execute it, python prints this:
```
(3,)
(3,2)
```
The first array is regarded as one-dimension by numpy. But what if we want it to be a two-dimension matrix with one row or we want it to be reshaped vertically? 

3: Reshaping array/matrix
---
reshape function returns a matrix with specified rows and columns. Either row or column argument can be set to -1 to indicate numpy to auto-calculate its size.
```python
a = np.array([1,2,3,4,5,6])
a = a.reshape(3,-1) # reshape to matrix with 3 rows
# returns:
# array([[1, 2],
#        [3, 4],
#        [5, 6]])
a = a.reshape(-1,3) # reshape t matrix with 3 columns
# returns
# array([[1, 2, 3],
#        [4, 5, 6]])
```
So to answer the question in the 2nd section, we can simply use `a = a.reshape(-1,1)` to make an array single-line vertical matrix.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1NTI0NTY0N119
-->
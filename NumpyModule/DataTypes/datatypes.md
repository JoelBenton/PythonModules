 # Numpy Vs Python Data types
NumPy data types are more specific than python data types in that numpy data
types included both the type of data such as integer or string and the amount
of memory available in bits. For example the `np.int64` data type holds 64 bits
and `np.int32` holds 32 bits. Numpy data types can be optimised for memory by reducing
the datatype's a bit size when our data doesn't require a large bit size.

## Bits and Bytes
Bit is short for binary digit. A bit can hold only values of zero or one. It 
is the smallest unit of memory data available on a computer. A byte is a sequence
of eight bits. NumPy's 32-bit integer can store two to the 32nd power numbers since this
is the number of possible combinations of zeros and ones available in 32 bits.
This means that `np.int32` can hold over 4 billion integers, from around negative 2.1 billion
to around positive 2.1 billion. Numbers outside these bounds require a larger bitsize,
such as `np.int64`

## The .dtype attribute
We can find the data type of elements in an array using the `.dtype` array attribute.
float64 is the default for an array made of python floats
```python
print(np.array([1.32, 5.78.175.55]).dtype)
```
output:
```terminaloutput
dtype('float64')
```

## Default Data Types
Numpy chooses data type based on the data in the array at creation. Here, NumPy
detects integers in a Python list. The default bit size is 64

```python
int_array = np.array([[1,2,3], [4,5,6]])
print(int_array.dtype)
```
output:
```terminaloutput
dtype('int64')
```
Strings work a little differently. Numpy selects a string data type with capacity
large enough for the longest string. Here, the U12 indicates the data type is a Unicode
string with a maximum length of 12.
```python
print(np.array(['introduction', 'to','NumPy']).dtype)
```
output:
```terminaloutput
dtype('<U12')
```

## The dtype as an argument
Rather than changing an array's data type after creation, it is possible to declare a data
type when you create the array using the optional dtype keywork argument. The dtype keywork argument
exists in many NumPy functions, including `np.zeros`, `np.random.random` and `np.arange`.
```python
float32_array = np.array([1.32, 5.78, 175.55], dtype=np.float32)
print(float32_array.dtype)
```
output:
```terminaloutput
dtype('float32')
```

## Type Conversion
Type conversion occurs when we explicitly tell NumPy to convert the data type of elements within an array.
This is done with the `.astype` method. For example, to convert a Boolean array to zero and one values, 
we could change the data type of the array to an integer type. Notice that the `np.bool_` data type has no
bit size because booleans do not vary in size.

```python
import numpy as np

boolean_array = np.array([[True, False], [False, False]], dtype=np.bool_)
boolean_array.astype(np.int32)
```
boolean_array now equals
```
array([[1,0], [0,0]], dtype=int32)
```

## Type Coercion
What happens if we try to make an array out of a Python list with several data types? All the data changes
to one data type: in this case, a string! Since NumPy did this without us telling it to, this is called type
coercion. NumPy did this because while numbers are easily cast into strings, strings are not easily cast into
numbers while still preserving the original data.

## Type coercion hierarchy.
We just read that adding a single string to an array means that NumPy will cast all elements into strings. 
Similarly, adding a single float to an array of integers will change all integers into floats, and adding 
a single integer to an array of Booleans will change all Booleans into integers. As we know, using one data 
type is one reason that NumPy has a lower memory consumption, but pay attention to the data type of the elements 
in your array as they can change without notice.
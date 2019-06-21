
## Python Built-in Functions and Models
[reference](https://docs.python.org/3.6/library/functions.html)
|	|	| Built-in Functions |	 |	|
| --- | --- | --- | --- | --- |
abs() | dict() | help() | min() | setattr()
all() | dir() | hex() | next() | slice()
any() | divmod() | id() | object() | sorted()
ascii() | enumerate() | input() | oct() | staticmethod()
bin() | eval() | int() | open() | str()
bool() | exec() | isinstance() | ord() | sum()
bytearray() | filter() | issubclass() | pow() | super()
bytes() | float() | iter() | print() | tuple()
callable() | format() | len() | property() | type()
chr() | frozenset() | list() | range() | vars()
classmethod() | getattr() | locals() | repr() | zip()
compile() | globals() | map() | reversed() | __import__()
complex() | hasattr() | max() | round()	 
delattr() | hash() | memoryview() | set()	 

1. **list，tuple和range的方法**

| Operations| Result |
| ---- | ---- |
| x in s	| True if an item of s is equal to x, else False
| x not in s	| False if an item of s is equal to x, else True
| s + t	| the concatenation of s and t
| s * n or n * s	| equivalent to adding s to itself n times
| s[i]	| ith item of s, origin 0
| s[i:j]	| slice of s from i to j
| s[i:j:k]	| slice of s from i to j with step k
| len(s)	| length of s	 
| min(s)	| smallest item of s	 
| max(s)	| largest item of s	 
| s.index(x[, i[, j]]) | index of the first occurrence of x in s (at or after index i and before index j)
| s.count(x)	| total number of occurrences of x in s

| Operation	| Result | 
| ---- | ---- |
s[i] = x	| item i of s is replaced by x	 
s[i:j] = t	| slice of s from i to j is replaced by the contents of the iterable t	 
del s[i:j]	| same as s[i:j] = []	 
s[i:j:k] = t	| the elements of s[i:j:k] are replaced by those of t
del s[i:j:k]	| removes the elements of s[i:j:k] from the list	 
s.append(x)	| appends x to the end of the sequence (same as s[len(s):len(s)] = [x])	 
s.clear()	| removes all items from s (same as del s[:])
s.copy()	| creates a shallow copy of s (same as s[:])
s.extend(t) or s += t	| extends s with the contents of t (for the most part the same as s[len(s):len(s)] = t)	 
s *= n	| updates s with its contents repeated n times
s.insert(i, x)	| inserts x into s at the index given by i (same as s[i:i] = [x])	 
s.pop([i])	| retrieves the item at i and also removes it from s
s.remove(x)	| remove the first item from s where s[i] == x
s.reverse()	| reverses the items of s in place
**multidimensioanl list**
``` python 
>>> lists = [[] for i in range(3)]
>>> lists[0].append(3)
>>> lists[1].append(5)
>>> lists[2].append(7)
>>> lists
[[3], [5], [7]]
```
**注：==如果相对多维list的每个子list能够单独操作，定义如上,==**__==另一个定义多维list的方法[[]]*3,对单个子list使用append、extend等操作都会作用到全部子list上，赋值除外==__
``` python
>>> lists = [[]] * 3
>>> lists
[[], [], []]
>>> lists[0].append(3)
>>> lists
[[3], [3], [3]]
```
**从list中选择满足条件的元素**
``` python
a = [1, 2, 3, 4, 5]
b = [p for p in a if p > 2]
print(a)
print(b)
```

2. **range(start, stop[, step])**

3. **Python中的可迭代对象**
包括：list，tuple， dict, string, array  
ang()和all()都要作用到可迭代对象，all(1 == 2)会报错“ TypeError: 'bool' object is not iterable”


表格中的 s 是可变序列类型的实例，t 是任意可迭代对象，而 x 是符合对 s 所规定类型与值限制的任何对象 (例如，bytearray 仅接受满足 0 <= x <= 255 值限制的整数)。

| 运算	| 结果 |
| ---- | ---- |
s[i] = x | 将 s 的第 i 项替换为 x	 
s[i:j] = t | 将 s 从 i 到 j 的切片替换为可迭代对象 t 的内容	 
del s[i:j] | 等同于 s[i:j] = []	 
s[i:j:k] = t | 将 s[i:j:k] 的元素替换为 t 的元素
del s[i:j:k] | 从列表中移除 s[i:j:k] 的元素	 
s.append(x)	| 将 x 添加到序列的末尾 (等同于 s[len(s):len(s)] = [x])	 
s.clear() | 从 s 中移除所有项 (等同于 del s[:])
s.copy() | 创建 s 的浅拷贝 (等同于 s[:])
s.extend(t) 或 s += t | 用 t 的内容扩展 s (基本上等同于 s[len(s):len(s)] = t)	 
s *= n | 使用 s 的内容重复 n 次来对其进行更新
s.insert(i, x) | 在由 i 给出的索引位置将 x 插入 s (等同于 s[i:i] = [x])	 
s.pop([i]) | 提取在 i 位置上的项，并将其从 s 中移除
s.remove(x)	 | 删除 s 中第一个 s[i] 等于 x 的项目。
s.reverse()	| 就地将列表中的元素逆序。
4. **super()**
广度优先调用父类中函数。
5. **__str__**
在类中定义函数__str__，当遇到print()类的实例时，会将__str__中定义好的字符串打印出来。
6. **copy**
python中，对对象的幅值实际上是对对象的引用，例如创建一个对象a后，把它赋值给另一变量b，python不是把a的值拷贝到另一b，而是拷贝了a的引用给b。如果a的值发生改变，则b的值也跟着a变。  
如果只赋值不复制对象引用，通过copy来实现：
1） copy.copy(x): 返回x的浅层复制。
2） copy.deepcopy(x[, memo])：返回x的深层复制
浅层复制和深层复制之间的区别仅在复合对象（包含其他对象的对象，如嵌套的list，类的实例）：
一个浅层复制会构造一个新的复合对象，然后将原对象中的引用插入其中。  
一个深层复制会构造一个新的复合对象，然后递归的将原对象所找到的对象的副本插入。  

==注：一个标量赋值给另一个标量，只产生副本，不产生引用==

eg. 
```python 
a =[1, 2, [3, 4]]
b = copy.copy(a)     # [3, 4]是以引用的方式存在于b
print(b)  # >>> [1, 2, [3, 4]]
c = copy.deepcopy(a) # [3, 4]是以值的方式存在于c
print(c)  # >>> [1, 2, [3, 4]]
a[2].append(5)
print(b)  # >>> [1, 2, [3, 4, 5]]
print(c)  # >>> [1, 2, [3, 4]]

```
#
## import math
0. 三角函数


## import time
0. **print program run time**
``` python
start = time.clock()

for i in range(1000):
    pass

end = time.clock()
print(end - start)
```


## numpy的数学函数由c实现，速度比Python math中的函数快10倍##
## import numpy as np,from numpy import linalg as LA
### numpy functions reference
[Table of Rough MATLAB-NumPy Equivalents](https://docs.scipy.org/doc/numpy/user/numpy-for-matlab-users.html#table-of-rough-matlab-numpy-equivalents)  

matlab | Numpy | Note
--- | --- | ---
ndims(a) | ndim(a) 或 a.ndim | 获取数组的维数。
numel(a) | size(a) 或 a.size | 获取数组的元素个数。
size(a)  | shape(a) 或 a.shape | 求矩阵的大小。
a([1:end 1], :) | a[np.r_:len(a),0] | 将数组a的第一行的副本添加到数组尾行下边
norm(v)	| sqrt(v @ v) or np.linalg.norm(v)	| L2 norm of vector v

[num reference routines](https://docs.scipy.org/doc/numpy/reference/routines.html#routines)
[some useful Numpy functions](https://docs.scipy.org/doc/numpy/user/quickstart.html)

**[math functons in numpy](https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/)**
func | note
---- | ----
np.sqrt() | 
np.arctan2() | 

### 创建数组
1. **list < = > array**
``` python
array1 = np.array(list1)
list1 = array1.tolist()

ls = [[[0, 2, 4, 6],[1, 3, 5, 7]]]
print(ls[0])
print(np.array(ls[0]).reshape(2, -1).T)
```
2. **创建全部相同元素数组**
``` python 
np.zeros((3, 4))
np.ones((3, 4))
```
3. **索引数组**
当创建的素组被用来当做其他数组的索引，必须确保为整数，比如[0], 不能是[0.]
```python  
np.arange(10)
np.int_([1, 2, 3])
```
4. **创建元数组**
``` python
a = np.arange(4).reshape(-1, 2)
b = np.arange(4, 10).reshape(-1, 2)
c = np.array([a, b])    # array 
print(c)
print(c.size)
print(c[1])
# c = np.array([a, b]) ## matrix
# print(c)
# print(c.shape)
# print(c[0, 1])
```
```
>>> [array([[0, 1],
       [2, 3]])
 array([[4, 5],
       [6, 7],
       [8, 9]])]
>>> 2
>>> [[4 5]
    [6 7]
    [8 9]]
```
### [数组索引与切片](https://www.numpy.org.cn/user_guide/numpy_basics/indexing.html)
数组索引是指使用方括号[]来索引数组值。
1. 一维数组
``` python
x = np.arange(10)
x[2]       # >>> 2
x[-2]      # >>> 8
x[2:5]     # >>> array([2, 3, 4])
x[:-7]     # >>> array([0, 1, 2])
x[1:7:2]   # >>> array([1, 3, 5])
```
2. 多维数组
``` python
x = np.arange(10).reshape(2, 5) # (2, 5) array
x[1, 3]      # >>> 8
x[1, -1]     # >>> 9
# 索引数量小于维度的多维数组，会得到一个子维数组
x[0]         # >>> array([0, 1, 2, 3, 4])， 
##[]中的a:b:c表示，索引从a（包含a）开始到b（不包含b）每间隔c个元素取值
x[2:5:2, ::3] # >>> array([2, 3, 4]) 
```
==注：**x[0, 2] = x[0][2]**，第一种情况返回的指向对应的数组内存值，相比，第二种情况效率更低，因为一个新的临时数组在第一个索引后创建了，这个临时数组随后才被2这个数字索引。==
简而言之，使用双“[]”引用多维数组元素，会在返回的第1个“[]”索引指向的数组上使用第2个“[]”索引，而返回一个元素。
``` python
x = np.arange(10).reshape(2, 5) # (2, 5) array
x[0] == x[:][0] === x[0][:] # 这三种方式得到结果相同，都是一个一维数组，array([0, 1, 2, 3, 4])

```

3. 索引数组
==一个数组采用索引数组进行取值，返回的是原始数组的副本。==
一维数组使用一维索引数组得到一个一维数组，，一维数组使用一个二维索引数组得到一个二维数组，得到的数组与索引数组相同纬度。
``` python
x = np.arange(10,1,-1)
x        # >>> array([10,  9,  8,  7,  6,  5,  4,  3,  2])
x[np.array([3, 3, 1, 8])]     # >>> array([7, 7, 9, 2])
x[np.array([3, 3, -3, 8])]    # >>> array([7, 7, 4, 2])
x[np.array([[1, 1], [2, 3]])] # x为一维数组，而索引数组是二维素组，返回一个维数与索引数组相同的数组，
```
二维数组
``` python
y = np.arange(35).reshape(5,7)
y[np.array([0, 2, 4]), np.array([0, 1, 2])]  # >>> array([0, 15, 30])
# 第一个索引数组表示行索引，第二个索引数组表示列索引，两个数组索引的元素数量相同。
y[np.array([0, 2, 4]), 1]   # >>> array([1, 15 ,29])
# 索引数组与标量组合
y[np.array([0, 2, 4])]  # 只有一个一维索引数组时，返回索引数组对应的行的所有元素
# >>> array([[ 0,  1,  2,  3,  4,  5,  6],
#            [14, 15, 16, 17, 18, 19, 20],
#            [28, 29, 30, 31, 32, 33, 34]])

```
4. 布尔值或掩码索引数组
布尔数组形状与被索引数组的初始维形状相同，二维被索引数组的返回结果为一维数组。当布尔数组的维数小于被索引数组时，
``` python
b = y > 20  # 布尔数组与y的形状相同
y[b]        # >>> array([21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34])
b[:, 5]     # >>> array([False, False, False, True, True], dtype=bool)
y[b[:, 5]]  # 这里第4行和第5行是从索引数组中选择出来的，并组合起来构成一个二维数组.
# >>> array([[21, 22, 23, 24, 25, 26, 27],
#            [28, 29, 30, 31, 32, 33, 34]])
```
三维被索引数组
``` python 
x = np.arange(30).reshape(2,3,5)
# >>> array([[[ 0,  1,  2,  3,  4],
#             [ 5,  6,  7,  8,  9],
#             [10, 11, 12, 13, 14]],
#            [[15, 16, 17, 18, 19],
#             [20, 21, 22, 23, 24],
#             [25, 26, 27, 28, 29]]])
b = np.array([[True, True, False], [False, True, True]])
x[b]
# >>> array([[ 0,  1,  2,  3,  4],
#            [ 5,  6,  7,  8,  9],
#            [20, 21, 22, 23, 24],
#            [25, 26, 27, 28, 29]])
```

### 数组运算操作
2. **vector cross-product**
``` python
x = [1, 2, 3]
y = [4, 5, 6]
np.cross(x, y)
```
result: array([-3,  6, -3])
``` python
x = [1,2]
y = [4,5]
np.cross(x, y)
```
result: -3
3. **np.reshape**
``` python
a = np.array([[1 2 3 4 5], [6 7 8 9 0]]).reshape(-1,2)
[[1 2]
 [3 4]
 [5 6]
 [7 8]
 [9 0]]
``` 
4. **LA.norm**
5. __np.dot and *__
dot()函数是矩阵乘，而*则表示逐个元素相乘
6. **np.zeros((5))与np.zeros((1,5))** 之间的差别np.zeros((5))显示为[0, 0, 0, 0, 0], np.zeros((1,5))显示为[[0, 0, 0, 0, 0]]。它们之间的差别等同与[ ]与[[ ]]之间的差别，前者是一个数组，后者是一个矩阵。
7. **np.random.random()**

8. **np.sign**

9. **np.argsort()** 
从小到大排序并返回索引
10. **两个数组叠加**
> 垂直叠加
``` python
np.concatenate([a, b], axis=0)
np.vstack([a, b])
np.r_[a, b]
```
>水平叠加
``` python
np.concatenate([a, b], axis=1)
np.hstack([a, b])
np.c_[a, b]
```

14. **dtype定义结构数组**

15. **数组shape数学是一个元组（tuple）**
一维数组的shape形如(n,)，二维数组的shape形如(n, m)
16. **判断数据是否为空**
np.size() == 0
在编写程序时，发现使用[]和None都出现过问题。
``` python
a = []
b = np.array([1.0])
c = None
print(np.size(a)) #>>> 0
print(np.size(b)) #>>> 1
print(np.size(c)) #>>> 1
```
17. **array_like(n, )与array_like(n, m)操作区别**
``` python
a1 = np.arange(3) 
b1 = np.arange(3, 6)
print(np.r_[a1, b1])
print(np.c_[a1, b1])
print(np.hstack([a1, b1]))
print(np.vstack([a1, b1]))
>>>[0 1 2 3 4 5]
>>>  [[0 3]
      [1 4]
      [2 5]]
>>>[0 1 2 3 4 5]
>>>  [[0 1 2]
      [3 4 5]]
a2 = np.arange(3).reshape(3, -1)
b2 = np.arange(3, 6).reshape(3, -1)
print(np.r_[a1, b1])
print(np.c_[a1, b1])
print(np.hstack([a1, b1]))
print(np.vstack([a1, b1]))
>>>  [[0]
      [1]
      [2]
      [3]
      [4]
      [5]]
>>>  [[0 3]
      [1 4]
      [2 5]]
>>>  [[0 3]
      [1 4]
      [2 5]]
>>>  [[0]
      [1]
      [2]
      [3]
      [4]
      [5]]
```



## import scipy

## python异常
1. try...except...else...finaly
2. raise


# 数据可视化
## import matplotlib
## import pandas as pd
0. **读入csv数据**
```python
import pandas as pd
data = pd.read_csv("text\data.csv")
print(data.head(5)) # print first 5 rows
print(data.columns) # print column index
print(data.shape)   # print data shape
print(data.iloc[1:2])          # print row 1
print(data.iloc[1:2]['date'])  # print row 1 column 'date'
print(data.iloc[1:2].date)     # print row 1 column 'date'
```


## terminal
1. cls：清空命令行里的信息
2. 注释 CTRL + K CTRL + C; 取消注释 CTRL + K CTRL + U


# 第六章：深入pandas：数据处理


# 6.2. 连接

另一类数据组合称为连接。NumPy提供了一个concatenate()函数，用于使用数组执行此类操作。

```python
>>> array1 = np.arange(9).reshape((3,3))
>>> array1
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
>>> array2 = np.arange(9).reshape((3,3))+6
>>> array2
array([[ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
>>> np.concatenate([array1,array2],axis=1)
array([[ 0,  1,  2,  6,  7,  8],
       [ 3,  4,  5,  9, 10, 11],
       [ 6,  7,  8, 12, 13, 14]])
>>> np.concatenate([array1,array2],axis=0)
array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
```

通过使用pandas及其数据结构(如series和dataframe)，使用标记轴可以进一步概括数组的连接。concat()函数由pandas提供，用于这种操作。

```python
>>> ser1 = pd.Series(np.random.rand(4), index=[1,2,3,4])
>>> ser1
1    0.636584
2    0.345030
3    0.157537
4    0.070351
dtype: float64
>>> ser2 = pd.Series(np.random.rand(4), index=[5,6,7,8])
>>> ser2
5    0.411319
6    0.359946
7    0.987651
8    0.329173
dtype: float64
>>> pd.concat([ser1,ser2])
1    0.636584
2    0.345030
3    0.157537
4    0.070351
5    0.411319
6    0.359946
7    0.987651
8    0.329173
dtype: float64
```
默认情况下，concat()函数在axis = 0上工作，返回的对象是一个series。如果您设置axis = 1，那么结果将是一个dataframe。

```python
>>> pd.concat([ser1,ser2],axis=1)
          0         1
1  0.636584       NaN
2  0.345030       NaN
3  0.157537       NaN
4  0.070351       NaN
5       NaN  0.411319
6       NaN  0.359946
7       NaN  0.987651
8       NaN  0.329173
```

这种操作的问题是连接部分在结果中无法识别。例如，您希望在连接轴上创建层次索引。为此，您必须使用keys选项。

```python
>>> pd.concat([ser1,ser2], keys=[1,2])
1  1    0.636584
   2    0.345030
   3    0.157537
   4    0.070351
2  5    0.411319
   6    0.359946
   7    0.987651
   8    0.329173
dtype: float64
```

对于axis = 1的series之间的组合，keys将成为dataframe的列标题。

```python
>>> pd.concat([ser1,ser2], axis=1, keys=[1,2])
          1         2
1  0.636584       NaN
2  0.345030       NaN
3  0.157537       NaN
4  0.070351       NaN
5       NaN  0.411319
6       NaN  0.359946
7       NaN  0.987651
8       NaN  0.329173
```
到目前为止，您已经看到了将连接应用于series，但是同样的逻辑也可以应用于dataframe。

```python
>>> frame1 = pd.DataFrame(np.random.rand(9).reshape(3,3), index=[1,2,3],
columns=['A','B','C'])
>>> frame2 = pd.DataFrame(np.random.rand(9).reshape(3,3), index=[4,5,6],
columns=['A','B','C'])
>>> pd.concat([frame1, frame2])
          A         B         C
1  0.400663  0.937932  0.938035
2  0.202442  0.001500  0.231215
3  0.940898  0.045196  0.723390
4  0.568636  0.477043  0.913326
5  0.598378  0.315435  0.311443
6  0.619859  0.198060  0.647902
>>> pd.concat([frame1, frame2], axis=1)
          A         B         C         A         B         C
1  0.400663  0.937932  0.938035       NaN       NaN       NaN
2  0.202442  0.001500  0.231215       NaN       NaN       NaN
3  0.940898  0.045196  0.723390       NaN       NaN       NaN
4       NaN       NaN       NaN  0.568636  0.477043  0.913326
5       NaN       NaN       NaN  0.598378  0.315435  0.311443
6       NaN       NaN       NaN  0.619859  0.198060  0.647902

```

## 组合

还有另一种情况，即数据的组合无法通过合并或连接获得。以希望两个数据集具有全部或至少部分重叠的索引为例。

一个适用于series的函数是combine_first()，它同时实现了数据对齐。

```python
>>> ser1 = pd.Series(np.random.rand(5),index=[1,2,3,4,5])
>>> ser1
1    0.942631
2    0.033523
3    0.886323
4    0.809757
5    0.800295
dtype: float64
>>> ser2 = pd.Series(np.random.rand(4),index=[2,4,5,6])
>>> ser2
2    0.739982
4    0.225647
5    0.709576
6    0.214882
dtype: float64
>>> ser1.combine_first(ser2)
1    0.942631
2    0.033523
3    0.886323
4    0.809757
5    0.800295
6    0.214882
dtype: float64
>>> ser2.combine_first(ser1)
1    0.942631
2    0.739982
3    0.886323
4    0.225647
5    0.709576
6    0.214882
dtype: float64
```
相反，如果希望部分重叠，则只能指定要重叠的series中的部分。

```python
>>> ser1[:3].combine_first(ser2[:3])
1    0.942631
2    0.033523
3    0.886323
4    0.225647
5    0.709576
dtype: float64
```

## 绕轴旋转

除了组织数据以统一从不同来源收集的值之外，另一个相当常见的操作是旋转。事实上，按行或按列排列值并不总是适合您的目标。有时，您希望按行上的列值重新排列数据，反之亦然。

### 带层级索引的翻转

您已经看到dataframe可以支持分层索引。可以利用这个特性在dataframe中重新排列数据。在旋转上下文中，有两个基本操作:

* 堆叠-旋转或翻转, 将列转换为行的数据结构
* 反堆栈-将行转换为列

```python
>>> frame1 = pd.DataFrame(np.arange(9).reshape(3,3),
...                    index=['white','black','red'],
...                    columns=['ball','pen','pencil'])
>>> frame1
       ball  pen  pencil
white     0    1      2
black     3    4      5
red       6    7       8
```
使用dataframe上的stack()函数，得到以行为单位的列的旋转，从而生成一个系列:

```python
>>> ser5 = frame1.stack()
white  ball      0
       pen       1
       pencil    2
black  ball      3
       pen       4
       pencil    5
red    ball     6
       pen       7
       pencil    8
dtype: int32
```

从这个分层索引的系列中，使用unstack()函数将dataframe重新组装到一个翻转的表中。

```python
>>> ser5.unstack()
       ball  pen  pencil
white     0    1      2
black     3    4      5
red       6    7       8
```

还可以在另一个级别上执行反堆栈，指定级别的数量或其名称作为函数的参数。

```python
>>> ser5.unstack(0)
        white  black  red
ball        0      3    6
pen         1      4    7
pencil      2      5    8

```

### 从`长`格式翻转为`宽`格式

存储数据集的最常见方式是通过数据的准时注册生成，这些数据将填充文本文件(例如CSV)或数据库表。特别是当使用仪器读数，计算结果随时间迭代，或一系列值的简单手工输入时，这种情况会发生。这些文件的一个类似示例是日志文件，它通过在其中累积数据逐行填充。

这种类型数据集的特殊特性是在不同的列上有条目，通常在后续的行中重复。总是保持表格格式的数据，当你在这种情况下，你可以把他们作为长或堆叠格式。

要更清楚地了解这一点，请考虑下面的dataframe。

```python
>>> longframe = pd.DataFrame({ 'color':['white','white','white',
...                                  'red','red','red',
...                                  'black','black','black'],
...                         'item':['ball','pen','mug',
...                                 'ball','pen','mug',
...                                 'ball','pen','mug'],
...                         'value': np.random.rand(9)})
>>> longframe
   color  item     value
0  white  ball  0.091438
1  white   pen  0.495049
2  white   mug  0.956225
3    red  ball  0.394441
4    red   pen  0.501164
5    red   mug  0.561832
6  black  ball  0.879022
7  black   pen  0.610975
8  black   mug  0.093324
```

这种数据记录方式有一些缺点。例如，一个是某些字段的多样性和重复。考虑到列是键，这种格式的数据将很难读取，特别是在充分理解键值与其他列之间的关系时。

除了长格式之外，还有另一种方法可以将数据排列在一个称为宽的表中。这种模式更容易阅读，允许与其他表轻松连接，并且占用的空间更少。所以一般来说，它是一种更有效的存储数据的方式，尽管不太实用，尤其是在数据填充过程中。

作为标准，选择一列或一组列作为主键; 那么，其中包含的值一定是唯一的。

在这方面，pandas您提供了一个函数，允许您将dataframe从长类型转换为宽类型。这个函数是pivot()，它接受列(或列)作为参数，列将充当key的角色。

从上一个示例开始，您选择创建宽格式的dataframe，方法是选择color列作为键，而item作为第二个键，其值将形成dataframe的新列。

```python
>>> wideframe = longframe.pivot('color','item')
>>> wideframe
          value
item       ball       mug       pen
color
black  0.879022  0.093324  0.610975
red    0.394441  0.561832  0.501164
white  0.091438  0.956225  0.495049
```

正如您现在看到的，在这种格式中，dataframe要紧凑得多，其中包含的数据可读性也好得多。


## 删除

数据准备的最后一个阶段是删除列和行。你已经在第四章看到过这部分。但是，为了完整起见，这里重复说明。通过示例定义一个dataframe。

```python
>>> frame1 = pd.DataFrame(np.arange(9).reshape(3,3),
...                    index=['white','black','red'],
...                    columns=['ball','pen','pencil'])
>>> frame1
       ball  pen  pencil
white     0    1      2
black     3    4      5
red       6    7       8
```

为了删除列，只需使用指定列名的del命令应用于dataframe。

```python
>>> del frame1['ball']
>>> frame1
       pen  pencil
white    1    
2
black    4    
5
red      7       8
```

相反，要删除不需要的行，必须使用drop()函数，并将对应索引的标签作为参数。

```python
>>> frame1.drop('white')
       pen  pencil
black    4    
5
red      7       8
```

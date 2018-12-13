
# 第六章：深入pandas：数据处理


# 6.1. 数据准备


在您开始操作数据之前，有必要准备数据并以数据结构的形式组装它们，以便以后可以使用pandas库提供的工具对它们进行操作。这里列出了数据准备的不同步骤。

* 加载
* 组装
    ; - Merging
    - 合并
    ; - Concatenating
    - 连接
    ; - Combining
    - 组合
* 整形(旋转)
* 删除

上一章讲的是加载。在加载阶段，还有一部分准备工作涉及到将许多不同格式转换为数据结构(如dataframe)。但是，即使您拥有了数据(可能来自不同的源和格式)并将其统一为dataframe之后，还需要执行进一步的准备操作。在本章中，特别是在本节中，您将看到如何执行将数据转换为统一数据结构所需的所有操作。

pandas对象中包含的数据可以以不同的方式组合:

* 合并-- pandas.merge()函数根据一个或多个键连接dataframe中的行。对于那些熟悉SQL语言的人来说，这种模式非常熟悉，因为它还实现连接操作。
* 连接--pandas.concat()函数的作用是将对象连接在一个轴上。
* 组合-- pandas.DataFrame.combine_first()函数是一个方法，它允许您连接重叠数据，以便通过从另一个结构获取数据来填充数据结构中的缺失值。

此外，准备过程也包括旋转操作，其中包括行和列之间的交换。


## 合并

对于熟悉SQL的人来说，合并操作对应于JOIN操作，它由使用一个或多个键的行连接的数据组合而成。

实际上，任何使用关系数据库的人通常使用连接查询和SQL从不同的表中获取数据，这些表使用它们之间共享的一些引用值(键)。在这些键的基础上，可以通过组合其他表以表格的形式获得新的数据。这个与库pandas的操作称为merge, merge()是执行这种操作的函数。

首先，导入pandas并定义两个dataframe，作为本节的示例。

```python
>>> import numpy as np
>>> import pandas as pd
>>> frame1 = pd.DataFrame( {'id':['ball','pencil','pen','mug','ashtray'],
...                      'price': [12.33,11.44,33.21,13.23,33.62]})
>>> frame1
        id  price
0     ball  12.33
1   pencil  11.44
2      pen  33.21
3      mug  13.23
4  ashtray  33.62
>>> frame2 = pd.DataFrame( {'id':['pencil','pencil','ball','pen'],
...                      'color': ['white','red','red','black']})
>>> frame2
   color      id
0  white  pencil
1    red  pencil
2    red    ball
3  black     pen
```

通过对两个dataframe对象应用merge()函数来执行合并。
```python
>>> pd.merge(frame1,frame2)
       id  price  color
0    ball  12.33    red
1  pencil  11.44  white
2  pencil  11.44    red
3     pen  33.21  black
```
从结果中可以看到，返回的dataframe由具有相同ID的所有行组成。除了公共列之外，还添加了来自第一个和第二个dataframe的列。

在本例中，使用merge()函数时没有显式地指定任何列。事实上，在大多数情况下，需要决定以哪个列作为合并的基础。

为此，添加on选项，指定列名作为合并键。
```python
>>> frame1 = pd.DataFrame( {'id':['ball','pencil','pen','mug','ashtray'],
...                      'color': ['white','red','red','black','green'],
...                      'brand': ['OMG','ABC','ABC','POD','POD']})
>>> frame1
  brand  color       id
0   OMG  white     ball
1   ABC    red   pencil
2   ABC    red      pen
3   POD  black      mug
4   POD  green  ashtray
>>> frame2 = pd.DataFrame( {'id':['pencil','pencil','ball','pen'],
...                      'brand': ['OMG','POD','ABC','POD']})
>>> frame2
  brand      id
0   OMG  pencil
1   POD  pencil
2   ABC    ball
3   POD    pen
```

在本例中，有两个dataframes，其列具有相同的名称。如果执行合并操作，将不会得到任何结果。

```python
>>> pd.merge(frame1,frame2)
Empty DataFrame
Columns: [brand, color, id]
Index: []
```

有必要显式地定义pandas必须遵循的合并条件，在on选项中指定关键列的名称。
```python
>>> pd.merge(frame1,frame2,on='id')
  brand_x  color      id brand_y
0     OMG  white    ball     ABC
1     ABC    red  pencil     OMG
2     ABC    red  pencil     POD
3     ABC    red     pen     POD

>>> pd.merge(frame1,frame2,on='brand')
  brand  color     id_x    id_y
0   OMG  white     ball  pencil
1   ABC    red   pencil    ball
2   ABC    red      pen    ball
3   POD  black      mug  pencil
4   POD  black      mug     pen
5   POD  green  ashtray  pencil
6   POD  green  ashtray     pen
```

正如预期的那样，结果根据合并的条件有很大的不同。但是，通常会出现相反的问题，即有两个dataframe，其中的键列没有相同的名称。要纠正这种情况，您必须使用left_on和right_on选项，它们为第一个和第二个dataframe指定键列。看一个例子。

```python
>>> frame2.columns = ['brand','sid']
>>> frame2
  brand     sid
0   OMG  pencil
1   POD  pencil
2   ABC    ball
3   POD    pen

>>> pd.merge(frame1, frame2, left_on='id', right_on='sid')
  brand_x  color      id brand_y     sid
0     OMG  white    ball     ABC    ball
1     ABC    red  pencil     OMG  pencil
2     ABC    red  pencil     POD  pencil
3     ABC    red     pen     POD     pen
```

默认情况下，merge()函数执行内部连接;结果中的键是交集的结果。

其他可能的选项是左连接、右连接和外连接。外连接产生所有键的联合，将左连接和右连接的效果组合在一起。要选择连接的类型，必须使用how选项。

```python
>>> frame2.columns = ['brand','id']
>>> pd.merge(frame1,frame2,on='id')
  brand_x  color      id brand_y
0     OMG  white    ball     ABC
1     ABC    red  pencil     OMG
2     ABC    red  pencil     POD
3     ABC    red     pen     POD
>>> pd.merge(frame1,frame2,on='id',how='outer')
  brand_x  color       id brand_y
0     OMG  white     ball     ABC
1     ABC    red   pencil     OMG
2     ABC    red   pencil     POD
3     ABC    red      pen     POD
4     POD  black      mug     NaN
5     POD  green  ashtray     NaN
>>> pd.merge(frame1,frame2,on='id',how='left')
  brand_x  color       id brand_y
0     OMG  white     ball     ABC
1     ABC    red   pencil     OMG
2     ABC    red   pencil     POD
3     ABC    red      pen     POD
4     POD  black      mug     NaN
5     POD  green  ashtray     NaN
>>> pd.merge(frame1,frame2,on='id',how='right')
  brand_x  color      id brand_y
0     OMG  white    ball     ABC
1     ABC    red  pencil     OMG
2     ABC    red  pencil     POD
3     ABC    red     pen     POD
```
要合并多个键，只需向on选项添加一个列表。
```python
>>> pd.merge(frame1,frame2,on=['id','brand'],how='outer')
  brand  color       id
0   OMG  white     ball
1   ABC    red   pencil
2   ABC    red      pen
3   POD  black      mug
4   POD  green  ashtray
5   OMG    NaN   pencil
6   POD    NaN   pencil
7   ABC    NaN    ball
8   POD    NaN      pen
```

### 索引合并

在某些情况下，索引可以用作合并键，而不是将dataframe的列看作键。然后，为了决定要考虑哪些索引，您将left_index或right_index选项设置为True以激活它们，也可以同时激活它们。

```python
>>> pd.merge(frame1, frame2, right_index=True, left_index=True)
  brand_x  color    id_x brand_y    id_y
0     OMG  white    ball     OMG  pencil
1     ABC    red  pencil     POD  pencil
2     ABC    red     pen     ABC    ball
3     POD  black     mug     POD     pen
```

但是dataframe对象有一个join()方法，当您希望通过索引进行合并时，这个函数更方便。它还可以用来组合许多具有相同或相同索引但没有列重叠的dataframe对象。

In fact, if you launch
事实上，如果你执行
```python
>>> frame1.join(frame2)
```
将得到一个错误代码，因为frame1中的一些列与frame2的名称相同。然后在启动join()函数之前重命名frame2中的列。

```python
>>> frame2.columns = ['brand2','id2']
>>> frame1.join(frame2)
  brand  color       id brand2     id2
0   OMG  white     ball    OMG  pencil
1   ABC    red   pencil    POD  pencil
2   ABC    red      pen    ABC    ball
3   POD  black      mug    POD     pen
4   POD  green  ashtray    NaN     NaN
```

在这里，您已经执行了合并，但是基于索引的值而不是列。这一次，索引4只出现在frame1中，但是与frame2的列相对应的值将NaN报告为值。



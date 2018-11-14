

[*第四章：pandas库简介*](./README.md)

# 4.12. 索引与层级结构

分级索引是panda的一个非常重要的特性，因为它允许您在单个轴上拥有多个级别的索引。它为您提供了一种处理多维数据的方法，同时继续在二维结构中工作。

让我们从一个简单的示例开始，创建一个包含两个索引数组的series，即创建一个具有两个级别的结构。

```python
>>> mser = pd.Series(np.random.rand(8),
...        index=[['white','white','white','blue','blue','red','red',
           'red'],
...               ['up','down','right','up','down','up','down','left']])
>>> mser
white  up       0.461689
       down     0.643121
       right    0.956163
blue   up       0.728021
       down     0.813079
red    up    
0.536433
       down     0.606161
       left     0.996686
dtype: float64
>>> mser.index
Pd.MultiIndex(levels=[['blue', 'red', 'white'], ['down',
'left', 'right', 'up']],
...        labels=[[2, 2, 2, 0, 0, 1, 1, 1],
           [3, 0, 2, 3, 0, 3, 0, 1]])
```

通过层次索引的规范，选择值的子集在一定程度上简化了。

实际上，您可以为第一个索引的给定值选择值，您可以采用经典的方法:

```python
>>> mser['white']
up       0.461689
down     0.643121
right    0.956163
dtype: float64
```

或者您可以按照以下方式为第二个索引的给定值选择值:

```python
>>> mser[:,'up']
white    0.461689
blue     0.728021
red      0.536433
dtype: float64
```

直观地说，如果您想要选择一个特定的值，您需要指定两个索引。

```python
>>> mser['white','up']
0.46168915430531676
```

分层索引在重塑数据和基于组的操作(如数据透视表)方面发挥着关键作用。例如，数据可以通过一个名为unstack()的特殊函数重新排列并在dataframe中使用。该函数将具有层次索引的series转换为简单的dataframe，其中第二组索引转换为一组新的列。

```python
>>> mser.unstack()
           down      left     right       up
blue   0.813079       NaN       NaN  0.728021
red    0.606161  0.996686       NaN  0.536433
white  0.643121       NaN  0.956163  0.461689
```

如果您想执行反向操作(将dataframe转换为series)，则使用stack()函数。

```python
>>> frame
        ball  pen  pencil  paper
red        0    1    
2     3
blue       4    5       6     7
yellow     8    9      10     11
white     12   13      14     15
>>> frame.stack()
red     ball      0
        pen        1
        pencil     2
        paper      3
blue    ball    
4
        pen        5
        pencil     6
        paper      7
yellow  ball       8
        pen        9
        pencil    10
        paper     11
white   ball      12
        pen       13
        pencil    14
        paper     15
dtype: int64
```

使用dataframe，可以为行和列定义层次索引。在声明dataframe时，必须为索引和列选项定义数组数组。

```python
>>> mframe = pd.DataFrame(np.random.randn(16).reshape(4,4),
...      index=[['white','white','red','red'], ['up','down','up','down']],
...      columns=[['pen','pen','paper','paper'],[1,2,1,2]])

>>> mframe
                 pen               paper
                   1         2         1         2
white up   -1.964055  1.312100 -0.914750 -0.941930
      down -1.886825  1.700858 -1.060846 -0.197669
red   up   -1.561761  1.225509 -0.244772  0.345843
      down  2.668155  0.528971 -1.633708  0.921735
      
```


## 重新排序和排序级别

有时候，您可能需要在轴上重新排列级别的顺序，或者对特定级别的值进行排序。

swaplevel()函数将分配给要交换的两个级别的名称作为参数接受，并返回一个新对象，在它们之间交换这两个级别，同时不修改数据。

```python
>>> mframe.columns.names = ['objects','id']
>>> mframe.index.names = ['colors','status']
>>> mframe
objects             pen              paper
id                    1         2         1         2
colors status
white  up     -1.964055  1.312100 -0.914750 -0.941930
       down   -1.886825  1.700858 -1.060846 -0.197669
red    up     -1.561761  1.225509 -0.244772  0.345843
       down    2.668155  0.528971 -1.633708  0.921735
>>> mframe.swaplevel('colors','status')
objects             pen              paper
id                    1         2         1         2
status colors
up     white  -1.964055  1.312100 -0.914750 -0.941930
down   white  -1.886825  1.700858 -1.060846 -0.197669
up     red    -1.561761  1.225509 -0.244772  0.345843
down   red     2.668155  0.528971 -1.633708  0.921735

```

相反，sort_index()函数通过将数据指定为参数对数据进行排序，只考虑某个级别的数据

```python
>>> mframe.sort_index(level='colors')
objects             pen              paper
id                    1         2         1         2
colors status
red    down    2.668155  0.528971 -1.633708  0.921735
       up     -1.561761  1.225509 -0.244772  0.345843
white  down   -1.886825  1.700858 -1.060846 -0.197669
       up     -1.964055  1.312100 -0.914750 -0.941930
```

## 按级别汇总统计

许多在dataframe或series上执行的描述性统计信息和汇总统计信息都有一个level选项，您可以使用该选项确定应该在哪个级别确定描述性统计信息和汇总统计信息。

例如，如果在行级别创建统计信息，则只需使用级别名称指定Level选项即可。

```python
>>> mframe.sum(level='colors')
objects       pen               paper
id              1         2         1         2
colors
red      1.106394  1.754480 -1.878480  1.267578
white   -3.850881  3.012959 -1.975596 -1.139599
```

如果要为列的给定级别(例如id)创建统计信息，则必须通过将axis选项设置为1将第二个轴指定为参数。

```python
>>> mframe.sum(level='id', axis=1)
id                    1         2
colors status
white  up     -2.878806  0.370170
       down   -2.947672  1.503189
red    up     -1.806532  1.571352
       down    1.034447  1.450706

```

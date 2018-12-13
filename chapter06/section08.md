
# 第六章：深入pandas：数据处理


# 6.8. 组迭代

GroupBy对象支持迭代操作，以生成包含组名称和数据部分的两个元组序列。

```python
>>> for name, group in frame.groupby('color'):
...     print(name)
...     print(group)
...
green
   color  object  price1  price2
2  green  pencil    1.30    1.60
4  green     pen    2.75    3.15
red
  color   object  price1  price2
1   red   pencil    4.20    4.12
3   red  ashtray    0.56    0.75
white
   color object  price1  price2
0  white    pen    5.56    4.75
```

在您刚才看到的示例中，仅应用print变量作为示例。实际上，您可以将变量的打印操作替换为应用于它的函数。


## 链式转换

从这些示例中，您可以看到，对于每个分组，在进行某些函数计算或其他一般操作时，不管其获得方式及选择标准，结果都将是一个series结构(如果我们选择了单个列数据)或一个Dataaframe，然后保留索引系统和列的名称。

```python
>>> result1 = frame['price1'].groupby(frame['color']).mean()
>>> type(result1)
<class 'pandas.core.series.Series'>
>>> result2 = frame.groupby(frame['color']).mean()
>>> type(result2)
<class 'pandas.core.frame.DataFrame'>
```

因此，可以在此过程的各个阶段的任意点选择一列。以下是在三个不同的过程阶段中选择单个列的三种情况。这个例子说明了pandas提供的这种分组系统的很大的灵活性。

```python
>>> frame['price1'].groupby(frame['color']).mean()
color
green    2.025
red      2.380
white    5.560
Name: price1, dtype: float64
>>> frame.groupby(frame['color'])['price1'].mean()
color
green    2.025
red      2.380
white    5.560
Name: price1, dtype: float64
>>> (frame.groupby(frame['color']).mean())['price1']
color
green    2.025
red      2.380
white    5.560
Name: price1, dtype: float64
```

此外，经过聚合操作后，某些列的名称可能不是很有意义。实际上，向描述业务组合类型的列名添加前缀通常很有用。添加前缀(而不是完全替换名称)对于跟踪源数据非常有用，这些源数据从这些源数据派生出聚合值。如果您应用一个转换链的过程(一个序列或dataframe是相互生成的)，在这个过程中保持对源数据的一些引用是很重要的，那么这一点很重要。

```python
>>> means = frame.groupby('color').mean().add_prefix('mean_')
>>> means
       mean_price1  mean_price2
color
green        2.025        2.375
red          2.380        2.435
white        5.560        4.750
```

## 组函数

尽管许多方法还没有实现专门用于GroupBy，但它们在series中可以正确地使用数据结构。在前一节中，您看到了通过GroupBy对象获得series是多么容易，方法是指定列的名称，然后应用该方法进行计算。例如，可以使用quantiles()函数计算分位数。

```python
>>> group = frame.groupby('color')
>>> group['price1'].quantile(0.6)
color
green    2.170
red      2.744
white    5.560
Name: price1, dtype: float64
```

还可以定义它们自己的聚合函数。分别定义函数，然后作为参数传递给mark()函数。例如，您可以计算每个组的值的范围。

```python
>>> def range(series):
...      return series.max() - series.min()
...
>>> group['price1'].agg(range)
color
green    1.45
red      3.64
white    0.00
Name: price1, dtype: float64
```

agg()函数允许在整个dataframe上使用聚合函数。

```python
>>> group.agg(range)
       price1  price2
color
green    1.45    1.55
red      3.64    3.37
white    0.00    0.00
```

您还可以同时使用更多的聚合函数，mark()函数传递一个包含要执行的操作列表的数组，该列表将成为新的列。

```python
>>> group['price1'].agg(['mean','std',range])
        mean       std  range
color
green  2.025  1.025305   1.45
red    2.380  2.573869   3.64
white  5.560       NaN   0.00

```


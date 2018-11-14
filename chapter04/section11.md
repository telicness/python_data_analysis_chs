

[*第四章：Pandas库简介*](./README.md)

# 4.11. "非数值"数据

在前面的部分中，您看到了很容易形成丢失的数据。可以通过NaN(而不是数字)值在数据结构中识别它们。因此，数据结构中没有定义的值在数据分析中非常常见。

然而，Pandas的设计是为了更好地应对这种情况。实际上，在本节中，您将学习如何处理这些值，从而避免许多问题。例如，在Pandas库中，计算描述性统计数据隐式地排除了NaN值。


## 分配NaN值

如果需要向数据结构中的元素具体分配NaN值，则可以使用NumPy库的np.NaN(或np.nan)值。

```python
>>> ser = pd.Series([0,1,2,np.NaN,9],
...                   index=['red','blue','yellow','white','green'])
>>> ser
red      0.0
blue     1.0
yellow   2.0
white    NaN
green    9.0
dtype: float64
>>> ser['white'] = None
>>> ser
red      0.0
blue     1.0
yellow   2.0
white    NaN
green    9.0
dtype: float64
```

## 过滤出NaN值

在数据分析过程中，有多种方法可以消除NaN值。手工消除它们，逐个元素地消除它们，可能会非常乏味和危险，而且您永远无法确定是否消除了所有NaN值。这就是dropna()函数的用途。

```python
>>> ser.dropna()
red     0.0
blue    1.0
yellow  2.0
green   9.0
dtype: float64
```

还可以通过在选择条件中放置notnull()直接执行过滤函数。

```python
>>> ser[ser.notnull()]
red     0.0
blue    1.0
yellow  2.0
green   9.0
dtype: float64
```

如果您正在处理dataframe，它会变得更加复杂。如果您在这种类型的对象上使用dropna()函数，并且在列或行上只有一个NaN值，那么它就会消除这个值。

```python
>>> frame3 = pd.DataFrame([[6,np.nan,6],[np.nan,np.nan,np.nan],[2,np.nan,5]],
...                        index = ['blue','green','red'],
...                        columns = ['ball','mug','pen'])
>>> frame3
       ball  mug  pen
blue    6.0  NaN  6.0
green   NaN  NaN  NaN
red     2.0  NaN  5.0
>>> frame3.dropna()
Empty DataFrame
Columns: [ball, mug, pen]
Index: []
```

因此，为了避免整个行和列完全消失，您应该指定how选项，为其分配all值。这告诉dropna()函数只删除所有元素为NaN的行或列。

```python
>>> frame3.dropna(how='all')
      ball  mug  pen
blue   6.0  NaN  6.0
red    2.0  NaN  5.0
```

## 填充NaN矿点

与其在数据结构中过滤NaN值，还可能将其与数据分析上下文中可能相关的值一起丢弃，您还可以用其他数字替换它们。在大多数情况下，addna()函数是一个很好的选择。此方法使用一个参数，即替换任何NaN的值。在所有情况下都是一样的。

```python
>>> frame3.fillna(0)
       ball  mug  pen
blue    6.0  0.0  6.0
green   0.0  0.0  0.0
red     2.0  0.0  5.0
```

或者您可以根据列的不同用不同的值替换NaN，逐个指定索引和相关值。

```python
>>> frame3.fillna({'ball':1,'mug':0,'pen':99})
       ball  mug   pen
blue    6.0  0.0   6.0
green   1.0  0.0  99.0
red     2.0  0.0   5.0
```


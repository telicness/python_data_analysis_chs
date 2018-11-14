

[*第四章:pandas库——导论*](./README.md)

# 4.8. 函数应用与映射

本节介绍pandas库的函数。

## 按元素划分的函数

pandas库建立在NumPy的基础上，然后通过将其调整为series和dataframe等新数据结构来扩展其许多特性。其中有通用函数ufunc。这类函数由数据结构中的元素操作。

```python

>>> frame = pd.DataFrame(np.arange(16).reshape((4,4)),
...                   index=['red','blue','yellow','white'],
...                   columns=['ball','pen','pencil','paper'])
>>> frame
        ball  pen  pencil  paper
red        0    1    
2     3
blue       4    5       6     7
yellow     8    9      10     11
white     12   13      14     15
```

例如，可以使用NumPy的np.sqrt()计算dataframe中的每个值的平方根。

```python

>>> np.sqrt(frame)
            ball       pen    pencil     paper
red     0.000000  1.000000  1.414214  1.732051
blue    2.000000  2.236068  2.449490  2.645751
yellow  2.828427  3.000000  3.162278  3.316625
white   3.464102  3.605551  3.741657  3.872983
```

## 按行或列划分的函数

函数的应用不仅限于ufunc函数，还包括用户定义的函数。重要的一点是它们在一维数组上操作，结果是给出一个数字。例如，您可以定义一个lambda函数，该函数计算数组中元素所涵盖的范围。

```python

>>> f = lambda x: x.max() - x.min()
```

也可以这样定义函数:

```python
>>> def f(x):
...    return x.max() - x.min()
...
```
使用apply()函数，您可以应用刚刚在dataframe上定义的函数。

```python

>>> frame.apply(f)
ball      12
pen       12
pencil    12
paper     12
dtype: int64
```

这次的结果是列的一个值，但是如果您喜欢按行而不是按列应用函数，那么您必须将axis选项设置为1。

```python
>>> frame.apply(f, axis=1)
red       3
blue      3
yellow    3
white     3
dtype: int64
```

方法apply()返回一个标量值并不是必须的。它还可以返回一个series。一个有用的例子是将应用程序同时扩展到许多功能。在本例中，我们将为应用的每个特性有两个或更多的值。这可以通过以下方式定义一个函数来实现:

```python
>>> def f(x):
...     return pd.Series([x.min(), x.max()], index=['min','max'])
...
```

然后，像以前一样应用这个函数。但在本例中，作为返回的对象，您将得到一个dataframe，而不是一个series，其中的行数与函数返回的值一样多。

```python
>>> frame.apply(f)
     ball  pen  pencil  paper
min     0    1      2      3
max    12   13     14     15
```


## 统计函数

大多数数组的统计函数对于dataframe仍然有效，因此不再需要使用apply()函数。例如，sum()和mean()等函数可以分别计算dataframe中包含的元素的和和平均值。

```python

>>> frame.sum()
ball      24
pen       28
pencil    32
paper     36
dtype: int64
>>> frame.mean()
ball      6.0
pen       7.0
pencil    8.0
paper     9.0
dtype: float64
```

还有一个名为describe()的函数，它允许您立即获得汇总统计信息。

```python

>>> frame.describe()
            ball        pen     pencil      paper
count   4.000000   4.000000   4.000000   4.000000
mean    6.000000   7.000000   8.000000   9.000000
std     5.163978   5.163978   5.163978   5.163978
min     0.000000   1.000000   2.000000   3.000000
25%     3.000000   4.000000   5.000000   6.000000
50%     6.000000   7.000000   8.000000   9.000000
75%     9.000000  10.000000  11.000000  12.000000
max    12.000000  13.000000  14.000000  15.000000
```




[*第四章：pandas库简介*](./README.md)

# 4.7. 数据结构之间的操作

现在，您已经熟悉了数据结构，例如series和dataframe，并且已经了解了如何在它们上执行各种基本操作，现在该进行涉及两个或多个结构的操作了。

例如，在上一节中，您看到了算术运算符如何应用于这两个对象之间。现在，在本节中，您将更深入地讨论可以在两个数据结构之间执行的操作。

## 灵活的运算方法

您已经看到了如何直接在pandas数据结构上使用数学运算符。同样的操作也可以使用适当的方法执行，称为灵活的算术方法。

* add()
* sub()
* div()
* mul()

为了调用这些函数，需要使用一种不同于处理数学运算符的规范。例如，您不需要在两个dataframes之间(例如frame1 + frame2)编写和，而是必须使用以下格式:

```python

>>> frame1.add(frame2)
         ball  mug  paper   pen  pencil
blue      6.0  NaN    NaN   6.0     NaN
green     NaN  NaN    NaN   NaN     NaN
red       NaN  NaN    NaN   NaN    NaN
white    20.0  NaN    NaN  20.0    NaN
yellow   19.0  NaN    NaN  19.0    NaN

```

如您所见，结果与使用加法运算符+得到的结果相同。您还可以注意到，如果索引和列名在不同系列之间存在很大差异，那么您将发现自己的dataframe中充满了NaN值。您将在本章后面看到如何处理这种数据。



## DataFrame和Series之间的操作

回到算术运算符，pandas允许您在不同结构之间进行事务处理。例如，在dataframe和series之间。例如，您可以用以下方式定义这两个结构。

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
>>> ser = pd.Series(np.arange(4), index=['ball','pen','pencil','paper'])
>>> ser
ball      0
pen       1
pencil    2
paper     3
dtype: int64
```

已经专门创建了两个新定义的数据结构，以便系列的索引与dataframe的列的名称匹配。这样，您就可以应用一个直接操作。

```python

>>> frame - ser
        ball  pen  pencil  paper
red        0    0    
0     0
blue       4    4       4     4
yellow     8    8      8      8
white     12   12      12     12
```

如您所见，序列的元素从列上相同索引对应的dataframe的值中减去。对列的所有值减去该值，而不管它们的索引是什么。

如果两个数据结构中的一个中没有索引，那么结果将是一个新列，其中只有该索引的所有元素都是NaN。

```python

>>> ser['mug'] = 9
>>> ser
ball      0
pen       1
pencil    2
paper     3
mug       9
dtype: int64

>>> frame - ser
        ball  mug  paper  pen  pencil
red        0  NaN      0    0       0
blue       4  NaN      4    4       4
yellow     8  NaN      8    8       8
white     12  NaN     12   12      12
```



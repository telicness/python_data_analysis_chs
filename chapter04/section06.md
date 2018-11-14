

[*第四章：pandas库简介*](./README.md)

# 4.6. 索引的其他功能

与Python常用的数据结构相比，您可以看到panda不仅利用了NumPy数组提供的高性能质量，还选择在其中集成索引。

这个选择被证明是成功的。事实上，尽管已经存在的动态结构提供了巨大的灵活性，但是使用结构的内部引用，例如标签提供的引用，允许必须执行操作的开发人员以更简单、更直接的方式执行它们。

本节详细分析了利用这一机制的一些基本特性。

* 重建索引
* 下降
* 对齐

## 重建索引

以前曾说过，一旦在数据结构中声明了Index对象，则无法更改它。这是正确的，但是通过执行重新索引，您也可以克服这个问题。

实际上，可以从现有的数据结构中获得新的数据结构，可以在其中重新定义索引规则。

```python

>>> ser = pd.Series([2,5,7,4], index=['one','two','three','four'])
>>> ser
one      2
two      5
three    7
four     4
dtype: int64
```

为了重新索引series，panda为您提供了reindex()函数。这个函数创建一个新的series对象，该对象的值按照新的标签序列重新排列前一个series的值。

在索引检索期间，可以更改索引序列的顺序、删除其中一些索引或添加新索引。在新标签的情况下，pandas添加NaN作为相应的值。

```python

>>> ser.reindex(['three','four','five','one'])
three     7.0
four      4.0
five      NaN
one       2.0
dtype: float64
```

从返回的值可以看出，标签的顺序已经完全重新排列。与标签2对应的值已经被删除，这个series中出现了一个名为five的新标签。

但是，要度量索引检索过程，定义标签列表可能会很困难，尤其是在使用大的数据爆炸时。所以你可以使用一些方法让你可以自动填充或插值。
 
为了更好地理解这种自动索引模式的功能，请定义以下series。

```python
>>> ser3 = pd.Series([1,5,6,3],index=[0,3,5,6])
>>> ser3
0    1
3    5
5    6
6    3
dtype: int64
```

正如您在本例中看到的，索引列不是完美的数字序列;实际上有一些缺失的值(1、2和4)。一个常见的需要是执行插值以获得完整的数字序列。要实现这一点，您将使用方法索引，方法选项设置为ffill。此外，还需要为索引设置一series值。在这种情况下，要指定0到5之间的一组值，可以使用range(6)作为参数。

```python

>>> ser3.reindex(range(6),method='ffill')
0    1
1    1
2    1
3    5
4    5
5    6
dtype: int64

```

从结果中可以看到，添加了原始series中没有的索引。通过插值，那些在原始序列中索引最低的被赋值。实际上，索引1和2的值是1，它属于索引0。

如果希望在插值过程中分配这个索引值，则必须使用bfill方法。

```python

>>> ser3.reindex(range(6),method='bfill')
0    1
1    5
2    5
3    5
4    6
5    6
dtype: int64
```

在本例中，分配给索引1和2的值是值5，它属于索引3。

将用级数进行索引检索的概念扩展到dataframe，您不仅可以对索引(行)进行重排，还可以对列进行重排，或者两者都进行重排。如前所述，添加一个新的列或索引是可能的，但是由于原始数据结构中缺少值，panda为它们添加了NaN值。

```python

>>> frame.reindex(range(5), method='ffill',columns=['colors','price','new',
'object'])
   colors price  new     object
0    blue   1.2  blue    ball
1   green   1.0  green   pen
2  yellow   3.3  yellow  pencil
3     red   0.9  red     paper
4   white   1.7  white   mug

```

## 下降

另一个连接到索引对象的操作是drop。由于用于指示索引和列名的标签，删除行或列变得很简单。

在本例中，panda还为这个操作提供了一个特定的函数，称为drop()。此方法将返回一个新对象，而不包含要删除的项。

例如，以我们希望从series中删除单个项目为例。为此，定义一个由四个元素组成的具有四个不同标签的通用series。

```python

>>> ser = pd.Series(np.arange(4.), index=['red','blue','yellow','white'])
>>> ser
red       0.0
blue      1.0
yellow    2.0
white     3.0
dtype: float64
```

现在，例如，您希望删除与标签黄色对应的项。简单地指定标签作为函数drop()的参数来删除它。

```python

>>> ser.drop('yellow')
red      0.0
blue     1.0
white    3.0
dtype: float64
```

要删除更多项，只需传递一个具有相应标签的数组。

```python
>>> ser.drop(['blue','white'])
red       0.0
yellow    2.0
dtype: float64
```

对于dataframe，可以通过引用两个轴的标签来删除值。通过示例声明以下框架。

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


要删除行，只需传递行的索引。

```python

>>> frame.drop(['blue','yellow'])
       ball  pen  pencil  paper
red       0    1       2     3
white    12   13     14     15
```

要删除列，总是需要指定列的索引，但是必须指定要从中删除元素的轴，这可以使用axis选项来完成。因此，要引用列名，您应该指定axis = 1。

```python
>>> frame.drop(['pen','pencil'],axis=1)
        ball  paper
red        0      3
blue       4      7
yellow     8     11
white     12     15
```

## 算术运算和数据对齐

涉及数据结构中的索引的最强大特性可能是pandas可以对齐来自两个不同数据结构的索引。当你对它们进行算术运算时尤其如此。实际上，在这些操作中，两个结构之间的索引不仅可以以不同的顺序出现，而且只能出现在两个结构中的一个中。

从下面的示例中可以看到，事实证明，在这些操作期间，pandas在调整索引方面非常强大。例如，您可以开始考虑两个定义它们的series，分别是两个标签数组，它们彼此不完全匹配。

```python
>>> s1 = pd.Series([3,2,5,1],['white','yellow','green','blue'])
>>> s2 = pd.Series([1,4,7,2,1],['white','yellow','black','blue','brown'])
```

在各种算术运算中，考虑简单求和。从刚刚声明的两个series中可以看到，两个series中都有一些标签，而其他的标签只出现在两个中的一个。当标签出现在两个操作符中时，它们的值将被添加，而在相反的情况下，它们也将在结果(新series)中显示，但值NaN。

```python

>>> s1 + s2
black    NaN
blue     3.0
brown    NaN
green    NaN
white    4.0
yellow   6.0
dtype: float64
```

对于dataframe，尽管它可能看起来更复杂，但是对齐遵循相同的原则，但是对行和列都执行对齐。

```python

>>> frame1 = pd.DataFrame(np.arange(16).reshape((4,4)),
...                   index=['red','blue','yellow','white'],
...                   columns=['ball','pen','pencil','paper'])
>>> frame2 = pd.DataFrame(np.arange(12).reshape((4,3)),
...                   index=['blue','green','white','yellow'],
...                   columns=['mug','pen','ball'])
>>> frame1
        ball  pen  pencil  paper
red        0    1    
2     3
blue       4    5       6     7
yellow     8    9      10     11
white     12   13      14     15

>>> frame2
        mug  pen  ball
blue      0    1     2
green     3    4     5
white     6    7     8
yellow    9   10    11

>>> frame1 + frame2
        ball  mug  paper  pen  pencil
blue     6.0  NaN    NaN  6.0     NaN
green    NaN  NaN    NaN  NaN     NaN
red      NaN  NaN    NaN  NaN     NaN
white    20.0 NaN    NaN  20.0    NaN
yellow   19.0 NaN    NaN  19.0    NaN
```


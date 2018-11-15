

[*第四章：pangas库简介*](./README.md)

# 4.9. 排序和排名

使用索引的另一个基本操作是排序。对数据进行排序通常是必要的，能够轻松地进行排序非常重要。pandas提供了sort_ index()函数，该函数返回一个新的对象，该对象与开始时相同，但元素是按顺序排列的。

让我们先看看如何对series中的项目进行排序。这个操作非常简单，因为要排序的索引列表只有一个。

```python
>>> ser = pd.Series([5,0,3,8,4],
...     index=['red','blue','yellow','white','green'])
>>> ser
red       5
blue      0
yellow    3
white     8
green     4
dtype: int64
>>> ser.sort_index()
blue      0
green     4
red       5
white     8
yellow    3
dtype: int64
```

正如您所看到的，这些项是根据其标签(从A到Z)按照字母的升序排列的，这是默认的行为，但是您可以通过将升序选项设置为False来设置相反的顺序。

```python
>>> ser.sort_index(ascending=False)
yellow    3
white     8
red       5
green     4
blue      0
dtype: int64
```

使用dataframe，排序可以在每个轴上独立执行。因此，如果希望在索引后面按行排序，只需继续使用sort_index()函数，不带参数，就像前面看到的那样，或者如果希望按列排序，则需要将axis选项设置为1。

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
>>> frame.sort_index()
        ball  pen  pencil  paper
blue       4    5       6     7
red        0    1    
2     3
white     12   13      14     15
yellow     8    9      10     11
>>> frame.sort_index(axis=1)
        ball  paper  pen  pencil
red        0      3    1       2
blue       4      7    5       6
yellow     8     11    9      10
white     12     15   13      14
```

到目前为止，您已经学习了如何根据索引对值进行排序。但通常情况下，您可能需要对数据结构中包含的值进行排序。在本例中，您必须根据需要对级数值或dataframe值进行排序来进行区分。

如果您想对级数进行排序，则需要使用sort_values()函数。

```python
>>> ser.sort_values()
blue      0
yellow    3
green     4
red       5
white     8
dtype: int64
```

如果需要在dataframe中对值进行排序，请使用前面看到的sort_values()函数，但是要使用by选项。然后必须指定要排序的列的名称。

```python
>>> frame.sort_values(by='pen')
        ball  pen  pencil  paper
red        0    1    
2     3
blue       4    5       6     7
yellow     8    9      10     11
white     12   13      14     15
```

如果排序标准将基于两个或多个列，则可以将包含列名称的数组分配给by选项。

```python
>>> frame.sort_values(by=['pen','pencil'])
        ball  pen  pencil  paper
red        0    1    
2     3
blue       4    5       6     7
yellow     8    9      10     11
white     12   13      14     15
```


排名是与排序密切相关的操作。它主要包括为级数的每个元素分配一个秩(即从0开始，然后逐渐增加的值)。排名将从最低值开始分配到最高值。

```python
>>> ser.rank()
red       4.0
blue      1.0
yellow    2.0
white     5.0
green     3.0
dtype: float64
```

排名也可以按照数据已经在数据结构中的顺序分配(不需要排序操作)。在这种情况下，只需添加带有第一个赋值的方法选项。

```python
>>> ser.rank(method='first')
red       4.0
blue      1.0
yellow    2.0
white     5.0
green     3.0
dtype: float64
```

默认情况下，甚至排名也遵循升序排序。若要反转此标准，请将升序选项设置为False。

```python
>>> ser.rank(ascending=False)
red       2.0
blue      5.0
yellow    4.0
white     1.0
green     3.0
dtype: float64
```



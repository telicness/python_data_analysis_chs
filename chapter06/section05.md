
# 第六章：深入pandas：数据处理


# 6.5. 排列

使用numpy.random.permutation()函数很容易实现series或dataframe的行的置换(随机重排序)操作。

对于此示例，创建一个包含按升序排列的整数的dataframe。

```python
>>> nframe = pd.DataFrame(np.arange(25).reshape(5,5))
>>> nframe
    0   1   2   3   4
0   0   1   2   3   4
1   5   6   7   8   9
2  10  11  12  13  14
3  15  16  17  18  19
4  20  21  22  23  24
```

现在，使用permutation()函数创建一个由5个整数组成的数组，这些整数从0到4以随机的顺序排列。这将是设置dataframe的一行值的新顺序。

```python
>>> new_order = np.random.permutation(5)
>>> new_order
array([2, 3, 0, 1, 4])
```

现在，使用take()函数将其应用到所有行的dataframe上。

```python
>>> nframe.take(new_order)
    0   1   2   3   4
2  10  11  12  13  14
3  15  16  17  18  19
0   0   1   2   3   4
1   5   6   7   8   9
4  20  21  22  23  24
```

如你所见，行顺序改变了; 现在，索引的顺序与new_order数组中的顺序相同。

您甚至可以将整个dataframe的一部分提交给一个排列。它生成的数组序列限制在一定的范围内，例如，在我们的例子中是2到4。

```python
>>> new_order = [3,4,2]
>>> nframe.take(new_order)
    0   1   2   3   4
3  15  16  17  18  19
4  20  21  22  23  24
2  10  11  12  13  14
```

## 随机抽样

您刚刚了解了如何通过排列来提取数据dataframe的一部分。有时候，当您有一个巨大的dataframe时，您可能需要随机取样，而最快的方法是使用np.random.randint()函数。

```python
>>> sample = np.random.randint(0, len(nframe), size=3)
>>> sample
array([1, 4, 4])
>>> nframe.take(sample)
    0   1   2   3   4
1   5   6   7   8   9
4  20  21  22  23  24
4  20  21  22  23  24
```

从这个随机抽样中，你可以更容易地得到相同的样本。



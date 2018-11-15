
[*第五章：pandas：读写数据*](./README.md)


# 5.8. HDF5格式

到目前为止，您已经了解了如何以文本格式读写数据。当数据分析涉及大量数据时，最好以二进制格式使用它们。Python中有几种处理二进制数据的工具。在这方面取得成功的一个库是HDF5库。

术语HDF表示分层数据格式，实际上，该库涉及读取和写入包含节点结构的HDF 5文件，以及存储多个数据集的能力。

然而，这个完全用C语言开发的库也与Python、MATLAB和Java等其他类型的语言有接口。它非常高效，尤其是在使用这种格式来保存大量数据时。与其他二进制格式相比，HDF5支持实时压缩，从而利用数据结构中的重复模式压缩文件大小。

目前，Python中HDF5接口库有PyTables和h5py。这两种形式在多个方面不同，因此，它们的选择在很大程度上取决于使用它的人的需要。

h5py提供了与高级api HDF5的直接接口，而PyTables抽象了许多HDF5的细节，以提供更灵活的数据容器、索引表、查询功能和计算上的其他媒体。

pandas有一个类似于类的dict，叫做hdfStore，用Pytable来存储pandas对象。因此，在使用HDF 5格式之前，必须导入HDFStore类：

```python
>>> from pandas.io.pytables import HDFStore
```

现在，你已经准备好将一个dataframe中的数据保存到一个.h5格式的文件中了，首先，创建一个dataframe。

```python
>>> frame = pd.DataFrame(np.arange(16).reshape(4,4),
...                      index=['white','black','red','blue'],
...                      columns=['up','down','right','left'])
```

创建一个名为amydata.h5的文件，然后写入dataframe中的数据。

```python
>>> store = HDFStore('mydata.h5')
>>> store['obj1'] = frame
```

在此，请猜猜看，如何在同一个HDF5文件中存储多个数据结构,为每一数据结构指定一个标签。
```python
>>> frame
       up  down  right  left
white   0   0.5      1   1.5
black   2   2.5      3   3.5
red     4   4.5      5   5.5
blue    6   6.5     7   7.5
>>> store['obj2'] = frame
```

这样，你便可以在单个文件中存储多个数据结构，用存储变量表示。

```python
>>> store
<class 'pandas.io.pytables.HDFStore'>
File path: mydata.h5
/obj1            frame        (shape->[4,4])
```

甚至反向过程也很简单，如果一个HDF5文件中包含有多种数据结构，内部对象可以用下列方式调用。

```python
>>> store['obj2']
       up  down  right  left
white   0   0.5      1   1.5
black   2   2.5      3   3.5
red     4   4.5      5   5.5
blue    6   6.5     7   7.5
```


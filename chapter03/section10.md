
[*第三章:NumPy库*](./README.md)


# 3.10. 结构化数组

到目前为止，在前面部分的各种示例中，您已经看到了一维数组和二维数组。NumPy允许您创建更复杂的数组，不仅在大小上，而且在结构上，称为结构化数组。这种类型的数组包含结构或记录，而不是单个项。

例如，您可以创建一个简单的结构数组作为项。由于有了dtype选项，您可以指定逗号分隔的说明符列表，以指示构成结构的元素以及数据类型和顺序。

```python
bytes                     b1
int                       i1, i2, i4, i8
unsigned ints             u1, u2, u4, u8
floats                    f2, f4, f8
complex                   c8, c16
fixed length strings      a<n>
```

例如，如果您想指定一个由整数、长度为6的字符串和一个布尔值组成的结构，您可以使用相应的说明符以正确的顺序在dtype选项中指定三种类型的数据。

> 注意，在不同的操作系统和Python发行版中，dtype和其他格式属性的结果可能有所不同。

```python
>>> structured = np.array([(1, 'First', 0.5, 1+2j),(2, 'Second', 1.3,
2-2j), (3, 'Third', 0.8, 1+3j)],dtype=('i2, a6, f4, c8'))
>>> structured
array([(1, b'First', 0.5, 1+2.j),
       (2, b'Second', 1.3, 2.-2.j),
       (3, b'Third', 0.8, 1.+3.j)],
      dtype=[('f0', '<i2'), ('f1', 'S6'), ('f2', '<f4'), ('f3', '<c8')])
```

还可以使用数据类型显式地指定int8、uint8、float16、complex64等等。

```python
>>> structured = np.array([(1, 'First', 0.5, 1+2j),(2, 'Second', 1.3,2-2j),
(3, 'Third', 0.8, 1+3j)],dtype=('
int16, a6, float32, complex64'))
>>> structured
array([(1, b'First', 0.5, 1.+2.j),
       (2, b'Second', 1.3, 2.-2.j),
       (3, b'Third', 0.8, 1.+3.j)],
      dtype=[('f0', '<i2'), ('f1', 'S6'), ('f2', '<f4'), ('f3', '<c8')])
```

两种情况都有相同的结果。在数组中，您将看到一个dtype序列，其中包含结构的每个项的名称和相应的数据类型。

写入适当的引用索引后，将获得相应的行，其中包含结构。

```python
>>> structured[1]
(2, 'bSecond', 1.3, 2.-2.j)
```

自动分配给struct的每个项的名称可以看作是数组的列的名称。使用它们作为结构索引，可以引用同一类型或同一列的所有元素。
```python
>>> structured['f1']
array([b'First', b'Second', b'Third'],
      dtype='|S6')
```
正如您刚才看到的，名称是用f(表示字段)和表示序列中位置的递进整数自动分配的。实际上，用更有意义的单词来指定名称更有用。这是可能的，你可以在数组声明时做到:
```python

>>> structured = np.array([
        (1,'First',0.5,1+2j),(2,'Second',1.3,2-2j), (3,'Third',0.8,1+3j)
    ], dtype=[
        ('id','i2'),('position','a6'),('value','f4'),('complex','c8')
    ])
    
>>> structured
array([(1, b'First', 0.5, 1.+2.j),
       (2, b'Second', 1.3, 2.-2.j),
       (3, b'Third', 0.8, 1.+3.j)],
       dtype=[('id', '<i2'), ('position', 'S6'), ('value', '<f4'),
        ('complex', '<c8')])
```

或者，您可以稍后重新定义分配给结构化数组的dtype属性的名称元组。

```python
>>> structured.dtype.names = ('id','order','value','complex')
```

现在您可以对各种字段类型使用有意义的名称:

```python
>>> structured['order']
array([b'First', b'Second', b'Third'],
      dtype='|S6')
      
```


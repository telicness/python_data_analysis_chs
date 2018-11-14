
[*第三章: NumPy库*](./README.md)


# 3.3. Ndarray: Numpy库的心脏

NumPy库基于一个主要对象:ndarray(代表n维数组)。这个对象是一个多维的齐次数组，它有预定数量的项:齐次的，因为它中几乎所有的项都具有相同的类型和大小。实际上，数据类型是由另一个称为dtype的NumPy对象指定的;每个ndarray只与一种类型的dtype相关联。

数组中的维数和项数由其形状定义，形状是一个n个正整数的元组，它指定每个维的大小。维度定义为轴，轴的数量定义为秩。

而且，NumPy数组的另一个特性是它们的大小是固定的，也就是说，一旦在创建时定义了它们的大小，它就保持不变。这种行为与Python列表不同，Python列表的大小可以变大或变小。

定义新ndarray最简单的方法是使用array()函数，传递一个包含要包含在其中的元素的Python列表作为参数。

```python
>>> a = np.array([1, 2, 3])
>>> a
array([1, 2, 3])
```

通过将新变量传递给type()函数，您可以轻易地检查新创建的对象是否是ndarray。
```python
>>> type(a)
<type 'numpy.ndarray'>
```
为了知道与新创建的ndarray相关联的dtype，必须使用dtype属性。

> 注意，dtype、shape和其他属性的结果在不同的操作系统和python发行版中可能会有所不同。

```python
>>> a.dtype 
dtype('int64')
```

刚刚创建的数组有一个轴，它的秩是1，而它的形状应该是(3,1)。要从对应的数组中获得这些值，只需使用ndim属性来获取轴，使用size属性来知道数组长度，使用shape属性来获取形状就足够了。

```python
>>> a.ndim 1
>>> a.size 3
>>> a.shape
 (3,)
```

你刚才看到的是一维数组的最简单情况。但是数组的使用可以很容易地扩展到多个维度。例如，如果定义一个二维数组2x2:

```python
>>> b = np.array([[1.3, 2.4],[0.3, 4.1]])
>>> b.dtype dtype('float64')
>>> b.ndim 2
>>> b.size 4
>>> b.shape
 (2, 2)
```

这个数组的秩是2，因为它有两个轴，每个轴的长度都是2。

另一个重要属性是itemsize，它可以用于ndarray对象。它定义数组中每个项的字节大小，数据是包含数组实际元素的缓冲区。第二个属性仍然没有被普遍使用，因为要访问数组中的数据，您将使用索引机制，您将在下一节中看到这种机制。

```python

>>> b.itemsize 
8
>>> b.data
<read-write buffer for 0x0000000002D34DF0, size 32, offset 
0 at 0x0000000002D5FEA0>
```


## 创建一个数组

要创建一个新数组，可以遵循不同的途径。最常见的是在前一节中通过列表或列表序列看到的作为array()函数参数。
```python
>>> c = np.array([[1, 2, 3],[4, 5, 6]])
>>> c
array([[1, 2, 3],
      [4, 5, 6]])

```

除了列表之外，array()函数还可以接受元组和元组序列。
```python
>>> d = np.array(((1, 2, 3),(4, 5, 6)))
>>> d
array([[1, 2, 3],
      [4, 5, 6]])

```
它还可以接受元组序列和相互连接的列表。
```python
>>> e = np.array([(1, 2, 3), [4, 5, 6], (7, 8, 9)])
>>> e
array([[1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]])
```


## 数据类型

到目前为止，您只看到了简单的整数和浮点数值，但是NumPy数组被设计成包含各种各样的数据类型(见表3-1)。例如，您可以使用数据类型字符串:

```python
>>> g = np.array([['a', 'b'],['c', 'd']])
>>> g
array([['a', 'b'],
        ['c', 'd']], dtype='|<U1')
        
>>> g.dtype 
    dtype('<U1')
    
>>> g.dtype.name 
'str32'
```


Data Type       | Description
数据类型        |描述
|-------------- | ----------------------------------------------------------------------|
bool_	        | boolean (true or false) stored as a byte
bool_           | 布尔值(真或假)存储为字节
int_	        | Default integer type (same as C long; normally either int64 or int32)
int_            | 默认整数类型(与C长相同;通常是int64或int32)
intc	        | identical to C int (normally int32 or int64)
intc            | 与C 语言INT完全相同(通常为int 32或int 64)
intp	        | integer used for indexing (same as C size_t; normally either int32 or int64) 
intp            | 用于索引的整数(与C语言 size_t相同; 通常是int32或int64)
int8	        | byte (–128 to 127)
int8            | 字节(-128到127)
int16	        | integer (–32768 to 32767)
int16           | 整数(-32768到32767)
int32	        | integer (–2147483648 to 2147483647)
int32           | 整数(-2147483648到2147483647)
int64	        | integer (–9223372036854775808 to 9223372036854775807)
int64           |整数(-9223372036854775808至9223372036854775807)
uint8	        | unsigned integer (0 to 255)
uint8           |无符号整数(0到255)
uint16	        | unsigned integer (0 to 65535)
uint16          |无符号整数(0到65535)
uint32	    | unsigned integer (0 to 4294967295)
uint32           |无符号整数(0到4294967295)
uint64	    | unsigned integer (0 to 18446744073709551615)
uint64          |无符号整数(0到18446744073709551615)
float_	    | Shorthand for float64
float_          |是float64的缩写
float16	    | half precision float: sign bit, 5-bit exponent, 10-bit mantissa 
float16         | 半精度浮点数：符号位，5位指数，10位尾数。
float32	    | Single precision float: sign bit, 8-bit exponent, 23-bit mantissa 
浮点数32        | 单精度浮点数：符号位，8位指数，23位尾数。
float64	    | Double precision float: sign bit, 11-bit exponent, 52-bit mantissa 
float64         | 双精度浮点:符号位，11位指数，52位尾数
complex_	| Shorthand for complex128
complex_        | complex128的简写
complex64	| Complex number, represented by two 32-bit floats (real and imaginary components)
complex64       | 复数，由两个32位浮点数(实和虚分量)表示
complex128	| Complex number, represented by two 64-bit floats (real and imaginary components)
complex128      | 复数，由两个64位浮点数(真实和虚构的组件)表示

>> 表3-1。NumPy支持的数据类型
- - - - - - - - - - - - - - - - - - - - -

## dtype选项

array()函数不接受单个参数。每个ndarray对象都与dtype对象相关联，dtype对象唯一地定义了将占用数组中每个项的数据类型。默认情况下，array()函数可以根据列表或元组序列中包含的值关联最合适的类型。实际上，您可以使用dtype选项作为函数的参数显式地定义dtype。

例如，如果要定义具有复杂值的数组，可以使用
dtype选项如下:

```python
>>> f = np.array([[1, 2, 3],[4, 5, 6]], dtype=complex)
>>> f
array([[1.+0.j, 2.+0.j, 3.+0.j],
      [4.+0.j, 5.+0.j, 6.+0.j]])

```


## 数组的内部创建

NumPy库提供了一组函数，它们生成初始内容的ndarrays，根据函数的不同使用不同的值创建。贯穿整章，贯穿整本书，你会发现这些特性是非常有用的。实际上，它们只允许一行代码生成大量数据。
例如，zeros()函数创建了一个完整的0数组，其维度由shape参数定义。例如，要创建一个二维数组3x3，您可以使用:
```python
>>> np.zeros((3, 3))

array([[ 0., 0., 0.],
        [ 0., 0., 0.],
        [ 0., 0., 0.]])
    
```

虽然ones()函数以非常类似的方式创建了一个填充了1的数组。
```python
>>> np.ones((3, 3))

array([[ 1., 1., 1.],
        [ 1., 1., 1.],
        [ 1., 1., 1.]])
```

默认情况下，这两个函数使用float64数据类型创建数组。一个特别有用的特性是arange()。这个函数生成带有数字序列的NumPy数组，这些数字序列根据传递的规则响应特定的规则

参数。例如，如果您想要生成一个0到10之间的值序列，您将只被传递给函数一个参数，这个参数就是您想要结束序列的值。

```python
>>> np.arange(0, 10)
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

```

如果您不想从0开始，而是想从另一个值开始，您只需指定两个参数:第一个是初始值，第二个是最终值。
```python
>>> np.arange(4, 10)
array([4, 5, 6, 7, 8, 9])
```

还可以生成具有精确间隔的值序列。如果指定了arange()函数的第三个参数，这将表示值序列中的一个值与下一个值之间的差距。
```python

>>> np.arange(0, 12, 3)
array([0, 3, 6, 9])
```

此外，第三个参数也可以是浮点数。
```python
>>> np.arange(0, 6, 0.6)
array([ 0. , 0.6, 1.2, 1.8, 2.4, 3. , 3.6, 4.2, 4.8, 5.4])
```

到目前为止，您只创建了一维数组。要生成二维数组，您仍然可以继续使用arange()函数，但要与取景()函数结合使用。这个函数按照形状参数指定的方式将一个线性数组分成不同的部分。

```python
>>> np.arange(0, 12).reshape(3, 4)
array([[ 0, 1, 2, 3],
    [ 4, 5, 6, 7],
    [ 8, 9, 10, 11]])
```

另一个与arange()非常相似的函数是linspace()。这个函数仍然将序列的初始值和结束值作为它的前两个参数，但是第三个参数没有指定一个元素和下一个元素之间的距离，而是定义了我们希望将间隔分割到其中的元素的数量。

```python
>>> np.linspace(0,10,5)
array([	0. ,	2.5,	5. ,	7.5,	10. ])
```

最后，获取已经包含值的数组的另一种方法是用随机值填充数组。这可以使用numpy的random()函数实现。随机的模块。这个函数将根据参数中指定的元素生成一个数组。

```python
>>> np.random.random(3)
array([ 0.78610272,	0.90630642,	0.80007102])
```

所获得的数字将随每次运行而变化。要创建多维数组，只需将数组的大小作为参数传递即可。

```python
>>> np.random.random((3,3))
array([[ 0.07878569, 0.7176506 , 0.05662501],
        [ 0.82919021, 0.80349121, 0.30254079],
        [ 0.93347404, 0.65868278, 0.37379618]])

```



[*第五章：pandas：读写数据*](./README.md)


# 5.9. Pickle-Python对象序列化

pickle模块实现了一种强大的算法，用于序列化和反序列化Python中实现的数据结构。pickle是将对象的层次结构转换为字节流的过程。

这允许一个对象被传输和存储，然后由接收者自己重新构建，保留所有原始特性。

在Python中,使用pickle模块执行pickling操作,但有一个模块叫cPickle,这是一个结果优化pickle模块(用C编写)。这个模块可以实际上在很多情况下甚至比pickle模块快1000倍。但是，无论使用哪个模块，这两个模块的接口几乎是相同的。

在明确提到在这种格式上运行的pandas的I/O函数之前，让我们更详细地了解一下cPickle模块并了解如何使用它。


## 用cPickle序列化Python对象

pickle(或cPickle)模块使用的数据格式是特定于Python的。默认情况下，使用ASCII表示来表示它，以便从人的角度可读。然后，通过使用文本编辑器打开文件，您可能能够理解其内容。要使用这个模块，您必须首先导入它:

```python
>>> import pickle
```

然后创建一个足够复杂的对象，使其具有内部数据结构，例如dict对象。

```python
>>> data = { 'color': ['white','red'], 'value': [5, 7]}
```

现在，您将通过cPickle模块的dumps()函数执行数据对象的序列化。

```python
>>> pickled_data = pickle.dumps(data)
```

现在，要查看它如何序列化dict对象，您需要查看pickled_data变量的内容。

```python
>>> print(pickled_data)
(dp1
S'color'
p2
(lp3
S'white'
p4
aS'red'
p5
asS'value'
p6
(lp7
I5
aI7
as.
```

一旦序列化了数据，就可以很容易地将其写入文件或通过套接字、管道等发送。

在传输之后，可以使用cPickle模块的loads()函数重新构造序列化对象(反序列化)。

```python
>>> nframe = pickle.loads(pickled_data)
>>> nframe
{'color': ['white', 'red'], 'value': [5, 7]}
```

## 使用pandas执行pickling操作

当涉及到用pandas库进行pickling(和unpickling)时，一切都变得容易多了。不需要在Python会话中导入cPickle模块，整个操作是隐式执行的。

另外，pandas使用的序列化格式并不完全是ASCII格式。

```python
>>> frame = pd.DataFrame(np.arange(16).reshape(4,4), index =
['up','down','left','right'])
>>> frame.to_pickle('frame.pkl')
```

在您的工作目录中有一个名为frame.pkl的新文件，它包含了有关frame的所有信息。

要打开PKL文件并读取内容，只需使用以下命令：

```python
>>> pd.read_pickle('frame.pkl')
        0   1   2   3
up      0   1   2   3
down    4   5   6   7
left    8   9  10  11
right  12  13  14  15
```

正如您所看到的，对于pandas用户来说，pickling和unpickling的操作都是完全隐藏的，对于那些必须专门处理数据分析的用户来说，这使得这项工作尽可能简单易懂。

> 注意，当您使用这种格式时，请确保您打开的文件是安全的。实际上，pickle格式的设计目的不是为了防止错误的和恶意构建的数据。



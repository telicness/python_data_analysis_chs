
# 第六章：深入pandas：数据处理


# 6.6. 字符串操作

Python是一种流行的语言，因为它在处理字符串和文本时使用方便。大多数操作都可以通过使用Python提供的内置函数轻松完成。对于更复杂的匹配和操作情况，有必要使用正则表达式。


## 用于字符串操作的内置方法

在许多情况下，您有组合字符串，希望在其中分离各个部分，然后将它们分配给正确的变量。split()函数允许您分隔文本的各个部分，以分隔符(例如逗号)作为参考点。

```python
>>> text = '16 Bolton Avenue , Boston'
>>> text.split(',')
['16 Bolton Avenue ', 'Boston']
```

正如您在第一个元素中看到的，您有一个字符串，末尾有一个空格字符。为了克服这个常见的问题，您必须使用split()函数和strip()函数，后者可以删除空格(包括换行符)。

```python
>>> tokens = [s.strip() for s in text.split(',')]
>>> tokens
['16 Bolton Avenue', 'Boston']
```

结果是一个字符串数组。如果元素的数量很少并且总是相同的，一个非常有趣的分配方法可能是这样的:

```python
>>> address, city = [s.strip() for s in text.split(',')]
>>> address
'16 Bolton Avenue'
>>> city
'Boston'
```

到目前为止，您已经了解了如何将文本拆分为多个部分，但通常还需要使用相反的方法，即在它们之间连接各种字符串，以形成更扩展的文本。最直观和简单的方法是用+运算符连接文本的各个部分。

```python
>>> address + ',' + city
'16 Bolton Avenue, Boston'
```

当您只有两个或三个字符串要连接时，这可能很有用。如果有许多部分需要连接，在本例中更实用的方法是使用分配给分隔符的join()函数，您希望使用它连接各种字符串。

```python
>>> strings = ['A+','A','A-','B','BB','BBB','C+']
>>> ';'.join(strings)
'A+;A;A-;B;BB;BBB;C+'
```

可以对字符串执行的另一类操作是搜索其中的文本片段，即子字符串。Python提供了表示检测子字符串的最佳方法的in关键字。

```python
>>> 'Boston' in text
True
```

有两个函数可以达到这个目的:index()和find()。

```python
>>> text.index('Boston')
19
>>> text.find('Boston')
19
```

在这两种情况下，它都会返回文本中包含子字符串的对应字符的位序。但是，当没有找到子字符串时，可以看出这两个函数行为上的差异:

```python
>>> text.index('New York')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: substring not found
>>> text.find('New York')
-1
```

实际上，index()函数返回一条错误消息，如果没有找到子字符串，find()返回-1。在相同的区域中，您可以知道一个字符或字符组合(子字符串)在文本中发生了多少次。count()函数为您提供了这个数字。

```python
>>> text.count('e')
2
>>> text.count('Avenue')
1
```

可以对字符串执行的另一个操作是替换或消除子字符串(或单个字符)。在这两种情况下，都将使用replace()函数，如果提示用空白字符替换子字符串，则操作将等同于从文本中删除子字符串。

```python
>>> text.replace('Avenue','Street')
'16 Bolton Street , Boston'

>>> text.replace('1',")
'16 Bolton Avenue, Boston'
```

## 正则表达式

正则表达式提供了一种非常灵活的方式来搜索和匹配文本中的字符串模式。单个表达式(通常称为regex)是根据正则表达式语言形成的字符串。有一个名为re的内置Python模块，它负责regex的操作。

首先，当您想要使用正则表达式时，您需要导入re模块:

```python
>>> import re
```

re模块提供了一组功能，可分为三类:

* 模式匹配
* 替换
* 切分

现在从几个例子开始。例如，表示一个或多个空格字符序列的正则表达式是\s+。正如您在前一节中看到的，要通过分隔符将文本拆分为多个部分，需要使用split()。即使对于执行相同操作的re模块，也有一个split()函数，只有它可以接受regex模式作为分离标准，这使得它更加灵活。

```python
>>> text = "This is      an\t odd  \n text!"
>>> re.split('\s+', text)
['This', 'is', 'an', 'odd', 'text!']
```

让我们更深入地分析re模块的机制。在调用re.split()函数时，首先编译正则表达式，然后在文本参数上调用split()函数。您可以使用re.compile()函数编译regex函数，从而获得一个可重用的对象regex，从而获得CPU周期。

对于在集合或字符串数组中迭代搜索子字符串的操作，尤其管用。

```python
>>> regex = re.compile('\s+')
```

因此，如果使用compile()函数创建一个regex对象，您可以用以下方式直接对其应用split()。

```python
>>> regex.split(text)
['This', 'is', 'an', 'odd', 'text!']
```

要将regex模式与文本中的任何其他业务子字符串匹配，可以使用findall()函数。它返回文本中满足regex要求的所有子字符串的列表。

例如，如果要在字符串中查找以`A`大写开头的所有单词，或例如，以`a`开头的所有单词，无论大写还是小写，都需要输入以下内容：

```python
>>> text = 'This is my address: 16 Bolton Avenue, Boston'
>>> re.findall('A\w+',text)
['Avenue']
>>> re.findall('[A,a]\w+',text)
['address', 'Avenue']
```

还有两个与findall()函数- match()和search()相关的函数。findall()返回列表中的所有匹配项，而search()函数只返回第一个匹配项。此外，该函数返回的对象是一个特殊对象:

```python
>>> re.search('[A,a]\w+',text)
<_sre.SRE_Match object; span=(11, 18), match='address'>
```

该对象不包含响应regex模式的子字符串的值，而是返回字符串中的起始位置和结束位置。

```python
>>> search = re.search('[A,a]\w+',text)
>>> search.start()
11
>>> search.end()
18
>>> text[search.start():search.end()]
'address'
```

match()函数只在字符串的开头执行匹配;如果没有匹配到第一个字符，那么在字符串中就没有进一步的研究了。如果您没有找到任何匹配，那么它将不会返回任何对象。

```python
>>> re.match('[A,a]\w+',text)
>>>
```

如果match()有响应，它将返回一个与search()函数相同的对象。

```python
>>> re.match('T\w+',text)
<_sre.SRE_Match object; span=(0, 4), match='This'>
>>> match = re.match('T\w+',text)
>>> text[match.start():match.end()]
'This'
```




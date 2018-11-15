
[*第五章：pandas：读写数据*](./README.md)


# 5.5. 从XML文件中读取数据

在I/O API函数列表中，没有关于XML(可扩展标记语言)格式的特定工具。事实上，虽然没有列出这种格式，但是这种格式非常重要，因为许多结构化数据都是XML格式的。这也不成问题，因为Python有许多其他库(除了pandas)可以管理XML格式数据的读写。

其中一个库是lxml库，它在解析非常大的文件时表现出色。在本节中，您将了解如何使用此模块解析XML文件，以及如何将其与panda集成以最终获得包含所请求数据的dataframe。关于这个库的更多信息，我强烈建议访问lxml的官方网站http://lxml.de/index.html。

以清单5-11所示的XML文件为例。把它写下来并保存在名字簿中。直接在工作目录中使用xml。

```text
<?xml version="1.0"?>
<Catalog>
   <Book id="ISBN9872122367564">
      <Author>Ross, Mark</Author>
      <Title>XML Cookbook</Title>
      <Genre>Computer</Genre>
      <Price>23.56</Price>
      <PublishDate>2014-22-01</PublishDate>
   </Book>
   <Book id="ISBN9872122367564">
      <Author>Bracket, Barbara</Author>
      <Title>XML for Dummies</Title>
      <Genre>Computer</Genre>
      <Price>35.95</Price>
      <PublishDate>2014-12-16</PublishDate>
   </Book>
</Catalog>
```
>> 清单5-11。books.xml

在本例中，您将使用XML文件中描述的数据结构将其直接转换为dataframe。首先要做的是使用lxml库的子模块objectify，以以下方式导入它。

```python
>>> from lxml import objectify
Now you can do the parser of the XML file with just the parse() function.
>>> xml = objectify.parse('books.xml')
>>> xml
<lxml.etree._ElementTree object at 0x0000000009734E08>
```

得到了一个对象树，它是lxml模块的内部数据结构。更详细地看一下这种类型的对象。要在这个树结构中导航，以便按元素选择元素，您必须首先定义根。您可以使用getroot()函数来做到这一点。

```python
>>> root = xml.getroot()
```

既然已经定义了结构的根，您就可以访问树的各个节点，每个节点对应于原始XML文件中包含的标记。这些项的名称将与对应的标记相同。因此，要选择它们，只需编写带有点的各种独立标记，以某种方式反映树中的节点层次结构。

```python
>>> root.Book.Author
'Ross, Mark'
>>> root.Book.PublishDate
'2014-22-01'
```

通过这种方式，您可以单独访问节点，但是您可以使用getchildren()同时访问其子元素。使用这个函数，您将获得引用元素的所有子节点。

```python
>>> root.getchildren()
[<Element Book at 0x9c66688>, <Element Book at 0x9c66e08>]
```
使用tag属性，您将得到与子节点对应的标记的名称。

```python
>>> [child.tag for child in root.Book.getchildren()]
['Author', 'Title', 'Genre', 'Price', 'PublishDate']
```

在使用text属性时，可以获得相应标记之间包含的文本内容。

```python
>>> [child.text for child in root.Book.getchildren()]
['Ross, Mark', 'XML Cookbook', 'Computer', '23.56', '2014-22-01']
```

但是，不管是否能够通过lxml.etree树结构，你需要的是把它转换成dataframe。定义以下函数，该函数的任务是分析eTree的内容，以逐行填充dataframe。

```python
>>> def etree2df(root):
...    column_names = []
...    for i in range(0,len(root.getchildren()[0].getchildren())):
...       column_names.append(root.getchildren()[0].getchildren()[i].tag)
...    xml:frame = pd.DataFrame(columns=column_names)
...    for j in range(0, len(root.getchildren())):
...       obj = root.getchildren()[j].getchildren()
...       texts = []
...       for k in range(0, len(column_names)):
...          texts.append(obj[k].text)
...       row = dict(zip(column_names, texts))
...       row_s = pd.Series(row)
...       row_s.name = j
...       xml:frame = xml:frame.append(row_s)
...    return xml:frame
...
>>> etree2df(root)
             Author            Title     Genre  Price PublishDate
0        Ross, Mark     XML Cookbook  Computer  23.56  2014-22-01
1  Bracket, Barbara  XML for Dummies  Computer  35.95  2014-12-16
```


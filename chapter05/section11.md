
[*第五章：pandas：读写数据*](./README.md)


# 5.11. 用NoSQL数据库读写数据：MongoDB

在所有的NoSQL数据库(BerkeleyDB, Tokyo Cabinet, MongoDB)中，MongoDB变得最普遍。考虑到它在许多系统中的广泛使用，在数据分析过程中可以考虑使用MongoDb存取pandas库生成的数据。

首先，如果您的PC上安装了MongoDB，您可以启动服务指向给定的目录。

```commandline
mongod --dbpath C:\MongoDB_data
```

现在服务正在监听端口27017，您可以使用MongoDB的官方驱动程序：pymongo连接到这个数据库。

```python
>>> import pymongo
>>> client = MongoClient('localhost',27017)
```

MongoDB的一个实例可以同时支持多个数据库。现在需要指向一个特定的数据库。

```python
>>> db = client.mydatabase
>>> db
Database(MongoClient('localhost', 27017), u'mycollection')
```

为了引用这个对象，您还可以使用
```python
>>> client['mydatabase']
Database(MongoClient('localhost', 27017), u'mydatabase')
```

既然已经定义了数据库，就必须定义集合。这个集合是存储在MongoDB中的一组文档，可以认为它相当于SQL数据库中的表。

```python
>>> collection = db.mycollection

>>> db['mycollection']
Collection(Database(MongoClient('localhost', 27017), u'mydatabase'), u'mycollection')

>>> collection
Collection(Database(MongoClient('localhost', 27017), u'mydatabase'), u'mycollection')
```

现在是加载集合中的数据的时候了。创建一个DataFrame。
```python
>>> frame = pd.DataFrame( np.arange(20).reshape(4,5),
...                       columns=['white','red','blue','black','green'])
>>> frame
   white  red  blue  black  green
0      0    1     2      3      4
1      5    6     7      8      9
2     10   11    12     13     14
3     15   16    17     18     19
```

在添加到集合之前，必须将其转换为JSON格式。转换过程并不像你想象的那样直接;这是因为您需要在DB上设置要记录的数据，以便尽可能公平、尽可能简单地重新提取DataFrame。

```python
>>> import json
>>> record = json.loads(frame.T.to_json()).values()
>>> record
[{u'blue': 7, u'green': 9, u'white': 5, u'black': 8, u'red': 6},
{u'blue': 2, u'green': 4, u'white':
0, u'black': 3, u'red': 1}, {u'blue': 17, u'green': 19, u'white': 15,
u'black': 18, u'red': 16}, {u
'blue': 12, u'green': 14, u'white': 10, u'black': 13, u'red': 11}]
```

现在您终于可以在集合中插入文档了，您可以使用insert()函数来实现这一点。

```python
>>> collection.mydocument.insert(record)
[ObjectId('54fc3afb9bfbee47f4260357'), ObjectId('54fc3afb9bfbee47f4260358'),
ObjectId('54fc3afb9bfbee47f4260359'), ­ObjectId('54fc3afb9bfbee47f426035a')]
```

可以看到，记录的每一行都有一个对象。现在数据已经加载到MongoDB数据库中的文档中，您可以执行反向过程，即，读取文档中的数据，然后将其转换为dataframe。

```python
>>> cursor = collection['mydocument'].find()
>>> dataframe = (list(cursor))
>>> del dataframe['_id']
>>> dataframe
   black  blue  green  red  white
0      8     7      9    6      5
1      3     2      4    1      0
2     18    17     19   16     15
3     13    12     14   11     10
```

您已经删除了包含ID号的列，以供MongoDB内部引用。



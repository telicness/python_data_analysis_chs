
[*第五章：pandas：读写数据*](./README.md)


# 5.10. 与数据库交互

在许多应用程序中，数据很少来自文本文件，因为这肯定不是存储数据的最有效方式。

这些数据通常存储在基于SQL的关系数据库中，也存储在最近非常流行的许多替代NoSQL数据库中。

在dataframe中从SQL加载数据非常简单，pandas提供了一些函数来简化这个过程。

pandas.io.sql模块提供独立于DB的统一接口，称为sqlalchemy。这个接口简化了连接模式，因为无论数据库是什么，命令都是一样的。要建立连接，可以使用create_engine()函数。有了这个特性，您可以配置使用驱动程序所需的所有属性，如用户、密码、端口和数据库实例。

以下是各类资料库的例子:

```python
>>> from sqlalchemy import create_engine
```

对于PostgreSQL：
```python
>>> engine = create_engine('postgresql://scott:tiger@localhost:5432/mydatabase')
```

对于MySQL
```python
>>> engine = create_engine('mysql+mysqldb://scott:tiger@localhost/foo')
```

对于Oracle
```python
>>> engine = create_engine('oracle://scott:tiger@127.0.0.1:1521/sidname')
```
用于MSSQL
```python
>>> engine = create_engine('mssql+pyodbc://mydsn')
```

对于SQLite
```python
>>> engine = create_engine('sqlite:///foo.db')
```


## 使用SQLite3加载和写入数据

作为第一个示例，您将使用Python内置驱动的sqlite3数据库。SQLite3是一种以非常简单和轻量级的方式实现DBMS SQL的工具，因此可以将其集成到任何使用Python语言实现的应用程序中。实际上，这个实用的软件允许您在单个文件中创建嵌入式数据库。

这使它成为任何想拥有数据库功能而不需要安装真正数据库的人的完美工具。SQLite3对于任何想在进入真正的数据库之前进行实践的人来说都是正确的选择，对于任何需要使用数据库的功能来收集数据的人来说也是正确的选择，但是要在单个程序中进行，而不必与数据库进行交互。

创建一个dataframe，用于在SQLite 3数据库上创建一个新表。

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

现在是时候实现到SQLite3数据库的连接了。
```python
>>> engine = create_engine('sqlite:///foo.db')
```

在数据库中的表中转换dataframe。
```python
>>> frame.to_sql('colors',engine)
```

相反，要读取数据库，必须使用read_sql()函数，其中包含表和引擎的名称。
```python
>>> pd.read_sql('colors',engine)
   index  white  red  blue  black  green
0      0      0    1     2      3      4
1      1      5    6     7      8      9
2      2     10   11    12     13     14
3      3     15   16    17     18     19
```

如您所见，即使在这种情况下，由于panda库中提供的I/O api，数据库上的写入操作也变得非常简单。

现在您将看到相同的操作，但不使用I/O API。这对于了解pandas是如何被证明是读取和写入数据到数据库的有效工具很有用。

首先，必须建立到DB的连接，并通过定义正确的数据类型来创建一个表，以便容纳要加载的数据。

```python
>>> import sqlite3
>>> query = """
... CREATE TABLE test
... (a VARCHAR(20), b VARCHAR(20),
...  c REAL,        d INTEGER
... );"""

>>> con = sqlite3.connect(':memory:')
>>> con.execute(query)
<sqlite3.Cursor object at 0x0000000009E7D730>
>>> con.commit()

```
现在可以使用SQL INSERT语句输入数据。

```python
>>> data = [('white','up',1,3),
...         ('black','down',2,8),
...         ('green','up',4,4),
...         ('red','down',5,5)]
>>> stmt = "INSERT INTO test VALUES(?,?,?,?)"
>>> con.executemany(stmt, data)
<sqlite3.Cursor object at 0x0000000009E7D8F0>
>>> con.commit()
```

现在您已经了解了如何在表上加载数据，现在应该了解如何查询数据库以获得刚刚记录的数据。这可以使用SQL SELECT语句。

```python
>>> cursor = con.execute('select * from test')
>>> cursor
<sqlite3.Cursor object at 0x0000000009E7D730>
>>> rows = cursor.fetchall()
>>> rows
[(u'white', u'up', 1.0, 3), (u'black', u'down', 2.0, 8), (u'green', u'up',
4.0, 4), (u'red', 5.0, 5)]
```

可以将元组列表传递给dataframe的构造函数，如果需要列的名称，可以在游标的description属性中找到它们。

```python
>>> cursor.description
(('a', None, None, None, None, None, None), ('b', None, None, None, None, None, None), 
('c', None, None, None, None, None), ('d', None, None, None, None, None, None))

>>> pd.DataFrame(rows, columns=zip(*cursor.description)[0])
       a     b  c  d
0  white    up  1  3
1  black  down  2  8
2  green    up  4  4
3    red  down  5  5
```

正如你所看到的，这种方法相当费劲。


## 使用PostgreSQL加载和写入数据

从0.14起pandas开始支持PostgreSQL数据库。因此，请再次检查您的PC上的版本是否与此版本或更高版本相对应。

```python
>>> pd.__version__
>>> '0.22.0'
```

要运行这个示例，您必须在系统上安装了PostgreSQL数据库。在我的例子中，我创建了一个名为postgres的数据库，postgres是用户，密码是密码。将这些值替换为与系统对应的值。首先要做的是安装psycopg2库，它的设计目的是管理和处理与数据库的连接。

对于Anaconda：
```commandline
conda install psycopg2
```

如果你使用PyPi:
```commandline
pip install psycopg2
```

现在可以与数据库建立连接：
``` python
>>> import psycopg2
>>> engine = create_engine('postgresql://postgres:password@localhost:5432/ postgres')
```

创建一个dataframe对象:

```python
>>> frame = pd.DataFrame(np.random.random((4,4)),
              index=['exp1','exp2','exp3','exp4'],
              columns=['feb','mar','apr','may']);
```

现在我们来看看如何轻松地将这些数据传输到表中。使用to_sql()，您将在一个名为dataframe的表中记录数据。

```python
>>> frame.to_sql('dataframe',engine)
```
pgAdmin III是一个用于管理PostgreSQL数据库的图形化应用程序。它是一个非常有用的工具，在Linux和Windows上都有。使用这个应用程序，很容易看到刚刚创建的表dataframe(参见图5-5)。


![Figure 5-5](images/figure-5-5.png)
>> 图5-5.pgAdmin III应用程序是PostgreSQL的完美图形DB管理器。


如果您熟悉SQL语言，那么查看新创建的表及其内容的更经典的方法是使用psql会话。

```python
>>> psql -U postgres
```

在我的例子中，我连接到postgres用户;你的情况可能不同。连接到数据库后，对新创建的表执行SQL查询。

```commandline
postgres=# SELECT * FROM DATAFRAME;
index|       feb       |       mar       |       apr       |       may
-----+-----------------+-----------------+-----------------+-----------------
exp1 |0.757871296789076|0.422582915331819|0.979085739226726|0.332288515791064
exp2 |0.124353978978927|0.273461421503087|0.049433776453223|0.0271413946693556
exp3 |0.538089036334938|0.097041417119426|0.905979807772598|0.123448718583967
exp4 |0.736585422687497|0.982331931474687|0.958014824504186|0.448063967996436
(4 righe)
```

即使是dataframe中的表的转换也是一个简单的操作。甚至这里还有一个read_sql_table()函数，它直接读取数据库并返回一个dataframe。

```python
>>> pd.read_sql_table('dataframe',engine)
  index       feb       mar       apr       may
0  exp1  0.757871  0.422583  0.979086  0.332289
1  exp2  0.124354  0.273461  0.049434  0.027141
2  exp3  0.538089  0.097041  0.905980  0.123449
3  exp4  0.736585  0.982332  0.958015  0.448064
```

但是，当您想在数据库中读取数据时，将整个表和单个表转换为dataframe并不是最有用的操作。实际上，那些使用关系数据库的人更喜欢使用SQL语言来选择插入SQL查询以何种形式导出数据。

SQL查询的文本可以集成到read_sql_query()函数中。

```python
>>> pd.read_sql_query('SELECT index,apr,may FROM DATAFRAME WHERE apr > 0.5',engine)

  index       apr       may
0  exp1  0.979086  0.332289
1  exp3  0.905980  0.123449
2  exp4  0.958015  0.448064
```


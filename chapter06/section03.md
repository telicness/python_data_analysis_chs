
# 第六章：深入pandas：数据处理


# 6.3. 数据转换

到目前为止，您已经看到了如何为分析准备数据。这个过程实际上代表了dataframe中包含的数据的重新组装，可能还会添加其他dataframe并删除不需要的部件。

现在我们开始数据操作的第二阶段:数据转换。在数据结构中安排数据的形式和它们的处理之后，转换它们的值是很重要的。事实上，在本节中，您将看到一些常见问题，以及使用pandas库的功能克服它们所需的步骤。

其中一些操作涉及重复或无效值，可能会被删除或替换。其他操作通过修改索引来关联。其他步骤包括处理和处理数据和字符串的数值。

## 删除重复

由于各种原因，dataframe中可能存在重复的行。在非常大的dataframes中，对这些行的检测可能非常成问题。在这种情况下，pandas供了一系列工具来分析大型数据结构中的重复数据。

首先，用一些重复的行创建一个简单的dataframe。

```python
>>> dframe = pd.DataFrame({ 'color': ['white','white','red','red','white'],
...                      'value': [2,1,3,3,2]})
>>> dframe
   color  value
0  white      2
1  white      1
2    red     3
3    red     3
4  white      2
```

应用于dataframe的duplicate()函数可以检测看起来是重复的行。它返回一系列布尔值，其中每个元素对应于一行，如果行被复制(即如果前面的元素中没有重复项，则为False。

```python
>>> dframe.duplicated()
0    False
1    False
2    False
3     True
4     True
dtype: bool
```

将布尔序列作为返回值在很多情况下都很有用，特别是对于过滤。实际上，如果您想知道哪些是重复的行，只需输入以下命令:

```python
>>> dframe[dframe.duplicated()]
   color  value
3    red     3
4  white      2
```

通常，所有重复的行都要从dataframe中删除;为此，pandas供了drop_duplicate()函数，该函数返回没有重复行的dataframes。

```python
>>> dframe[dframe.duplicated()]
   color  value
3    red     3
4  white      2
```

## 映射

pandas库提供了一组函数，您将在本节中看到，它们利用映射来执行一些操作。映射只不过是在两个不同值之间创建一个匹配列表，并能够将值绑定到特定的标签或字符串。

要定义映射，没有比dict对象更好的对象了。

```python
map = {
   'label1' : 'value1,
   'label2' : 'value2,
   ...
}
```

您将在本节中看到的函数执行特定的操作，但是它们都接受一个dict对象。

* replace()-- 替换值
* map()-- 创建一个新列
* rename() -- 替换索引值

### 通过映射替换值

通常在您组装的数据结构中，有一些值不满足您的需求。例如，文本可以是外语，也可以是另一个值的同义词，或者不以期望的形状表示。在这种情况下，各种值的替换操作通常是必要的过程。

例如，定义一个dataframe，它包含各种对象和颜色，包括两种非英语颜色。通常在组装操作过程中，可能会以一种不受欢迎的形式维护数据。

```python
>>> frame = pd.DataFrame({ 'item':['ball','mug','pen','pencil','ashtray'],
...                     'color':['white','rosso','verde','black','yellow'],
                        'price':[5.56,4.20,1.30,0.56,2.75]})
>>> frame
    color     item     price
0   white    ball      5.56
1   rosso      mug      4.20
2   verde      pen      1.30
3   black   pencil      0.56
4  yellow  ashtray      2.75
```

要用新值替换不正确的值，必须定义对应关系的映射，其中包含新值作为键。

```python
>>> newcolors = {
...    'rosso': 'red',
...    'verde': 'green'
... }
```

现在唯一可以做的就是使用replace()函数，将映射作为参数。

```python
>>> frame.replace(newcolors)
     color     item  price
0    white     ball   5.56
1      red      mug   4.20
2    green     pen   1.30
3    black   pencil   0.56
4   yellow  ashtray   2.75
```

从结果中可以看到，这两种颜色已经被dataframe中的正确值替换了。例如，一个常见的情况是用另一个值替换NaN值，例如0。您可以使用replace()，它执行得非常好。

```python
>>> ser = pd.Series([1,3,np.nan,4,6,np.nan,3])
>>> ser
0   1.0
1   3.0
2   NaN
3   4.0
4   6.0
5   NaN
6   3.0
dtype: float64
>>> ser.replace(np.nan,0)
0    1.0
1    3.0
2    0.0
3    4.0
4    6.0
5    0.0
6    3.0
dtype: float64
```

### 通过映射添加值

在前面的示例中，您看到了如何通过映射对应关系来替换值。在本例中，您将继续使用另一个示例中的值映射。在本例中，您正在利用映射来根据另一个列中包含的值在列中添加值。映射总是单独定义的。

```python
>>> frame = pd.DataFrame({ 'item':['ball','mug','pen','pencil','ashtray'],
...                     'color':['white','red','green','black','yellow']})
>>> frame
    color     item
0   white    ball
1     red      mug
2   green      pen
3   black   pencil
4  yellow  ashtray

```

假设您希望添加一列来指示dataframe中显示的商品的价格。在您这样做之前，假定您有一个可用的价目表，其中描述了每种商品的价格。然后定义一个dict对象，该对象包含每种商品类型的价格列表。

```python
>>> prices = {
...    'ball' : 5.56,
...    'mug' : 4.20,
...    'bottle' : 1.30,
...    'scissors' : 3.41,
...    'pen' : 1.30,
...    'pencil' : 0.56,
...    'ashtray' : 2.75
... }
```

应用于dataframe的系列或列的map()函数接受一个包含有映射的dict的函数或对象。因此，在您的示例中，您可以在列项上应用价格的映射，确保向price dataframe添加一个列。

```python
>>> frame['price'] = frame['item'].map(prices)
>>> frame
    color     item  price
0   white    ball   5.56
1     red      mug   4.20
2   green      pen   1.30
3   black   pencil   0.56
4  yellow  ashtray   2.75
```

### 重命名轴的索引

与您在系列和dataframe中看到的值非常类似，甚至可以使用映射以非常相似的方式转换axis标签。因此，为了替换标签索引，pandas供了rename()函数，它将映射作为一个参数，也就是一个dict对象。

```python
>>> frame
    color     item  price
0   white    ball   5.56
1     red      mug   4.20
2   green      pen   1.30
3   black   pencil   0.56
4  yellow  ashtray   2.75
>>> reindex = {
...   0: 'first',
...   1: 'second',
...   2: 'third',
...   3: 'fourth',
...   4: 'fifth'}
>>> frame.rename(reindex)
         color     item  price
first    white     ball   5.56
second     red      mug   4.20
third    green     pen   1.30
fourth   black   pencil   0.56
fifth   yellow  ashtray   2.75
```

如您所见，默认情况下，索引被重命名。如果要重命名列，必须使用columns选项。这一次，您将各种映射显式地分配给两个索引和列选项。

```python
>>> recolumn = {
...    'item':'object',
...    'price': 'value'}
>>> frame.rename(index=reindex, columns=recolumn)
         color   object  value
first    white     ball   5.56
second     red      mug   4.20
third    green     pen   1.30
fourth   black   pencil   0.56
fifth   yellow  ashtray   2.75
```

同样，对于需要替换单个值的最简单情况，您可以避免编写和分配多个变量。

```python
>>> frame.rename(index={1:'first'}, columns={'item':'object'})
        color   object  price
0       white     ball   5.56
first     red      mug   4.20
2       green      pen   1.30
3       black   pencil   0.56
4      yellow  ashtray   2.75
```

到目前为止，您已经看到rename()函数返回一个带有更改的dataframe，保持原来的dataframe不变。如果希望更改对调用函数的对象生效，则将inplace选项设置为True。

```python
>>> frame.rename(columns={'item':'object'}, inplace=True)
>>> frame
    color   object  price
0   white    ball   5.56
1     red      mug   4.20
2   green      pen   1.30
3   black   pencil   0.56
4  yellow  ashtray   2.75

```

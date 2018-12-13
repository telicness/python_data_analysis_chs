
# 第六章：深入pandas：数据处理


# 6.9. 高级数据聚合

在本节中，我们将介绍transform()和apply()函数，它们允许您执行许多类型的组操作，有些非常复杂。

现在假设我们想在同一个dataframe中合并以下内容:原始的dataframe(包含数据的dataframe)和通过计算组聚合得到的dataframe，例如sum求和函数。

```python
>>> frame = pd.DataFrame({ 'color':['white','red','green','red','green'],
...                     'price1':[5.56,4.20,1.30,0.56,2.75],
...                     'price2':[4.75,4.12,1.60,0.75,3.15]})

>>> frame
   color  price1  price2
0  white    5.56    4.75
1    red    4.20    4.12
2  green    1.30    1.60
3    red    0.56    0.75
4  green    2.75    3.15

>>> sums = frame.groupby('color').sum().add_prefix('tot_')
>>> sums
       tot_price1  tot_price2
color
green        4.05        4.75
red          4.76        4.87
white        5.56        4.75

>>> merge(frame,sums,left_on='color',right_index=True)
   color  price1  price2  tot_price1  tot_price2
0  white    5.56    4.75        5.56        4.75
1    red    4.20    4.12        4.76        4.87
3    red    0.56    0.75        4.76        4.87
2  green    1.30    1.60        4.05        4.75
4  green    2.75    3.15        4.05        4.75
```

由于merge()，可以在dataframe的每一行中添加聚合的结果。但是还有另一种方法来做这种操作。这是通过使用transform()实现的。这个函数执行聚合，就像您以前看到的那样，但是同时显示了根据dataframe的每一行的键值计算的值。

```python
>>> frame.groupby('color').transform(np.sum).add_prefix('tot_')
   tot_price1  tot_price2
0        5.56        4.75
1        4.76        4.87
2        4.05        4.75
3        4.76        4.87
4        4.05        4.75
```

如您所见，transform()方法是一个更专门化的函数，具有非常特定的需求:作为参数传递的函数必须生成要广播的单个标量值(聚合)。

覆盖更一般的GroupBy的方法适用于apply()。该方法完全适用于分割-应用-组合方案。实际上，这个函数将对象划分为几个部分，以便操作，调用每个部分上的函数通道，然后尝试将各个部分链接在一起。

```python
>>> frame = pd.DataFrame( { 'color':['white','black','white','white','black','black'],
...                      'status':['up','up','down','down','down','up'],
...                      'value1':[12.33,14.55,22.34,27.84,23.40,18.33],
...                      'value2':[11.23,31.80,29.99,31.18,18.25,22.44]})

>>> frame
   color status  value1  value2
0  white     up   12.33   11.23
1  black     up   14.55   31.80
2  white   down   22.34   29.99
3  white   down   27.84   31.18
4  black   down   23.40   18.25

>>> frame.groupby(['color','status']).apply( lambda x: x.max())
              color status  value1  value2
color status
black down    black   down   23.40   18.25
      up      black     up   18.33   31.80
white down    white   down   27.84   31.18
      up      white     up   12.33   11.23
5  black     up   18.33   22.44

>>> frame.rename(index=reindex, columns=recolumn)
         color   object  value
first    white     ball   5.56
second     red      mug   4.20
third    green     pen   1.30
fourth   black   pencil   0.56
fifth   yellow  ashtray   2.75

>>> temp = pd.date_range('1/1/2015', periods=10, freq= 'H')
>>> temp
DatetimeIndex(['2015-01-01 00:00:00', '2015-01-01 01:00:00',
               '2015-01-01 02:00:00', '2015-01-01 03:00:00',
               '2015-01-01 04:00:00', '2015-01-01 05:00:00',
               '2015-01-01 06:00:00', '2015-01-01 07:00:00',
               '2015-01-01 08:00:00', '2015-01-01 09:00:00'],
              dtype='datetime64[ns]', freq='H')
              
Length: 10, Freq: H, Timezone: None

>>> timeseries = pd.Series(np.random.rand(10), index=temp)
>>> timeseries
2015-01-01 00:00:00    0.368960
2015-01-01 01:00:00    0.486875
2015-01-01 02:00:00    0.074269
2015-01-01 03:00:00    0.694613
2015-01-01 04:00:00    0.936190
2015-01-01 05:00:00    0.903345
2015-01-01 06:00:00    0.790933
2015-01-01 07:00:00    0.128697
2015-01-01 08:00:00    0.515943
2015-01-01 09:00:00    0.227647
Freq: H, dtype: float64

>>> timetable = pd.DataFrame( {'date': temp, 'value1' : np.random.rand(10),
...                                       'value2' : np.random.rand(10)})
>>> timetable
                 date    value1    value2
0 2015-01-01 00:00:00  0.545737  0.772712
1 2015-01-01 01:00:00  0.236035  0.082847
2 2015-01-01 02:00:00  0.248293  0.938431
3 2015-01-01 03:00:00  0.888109  0.605302
4 2015-01-01 04:00:00  0.632222  0.080418
5 2015-01-01 05:00:00  0.249867  0.235366
6 2015-01-01 06:00:00  0.993940  0.125965
7 2015-01-01 07:00:00  0.154491  0.641867
8 2015-01-01 08:00:00  0.856238  0.521911
9 2015-01-01 09:00:00  0.307773  0.332822
```

然后添加到列前面的dataframe，该列表示一组文本值，您将使用这些值作为键值。

```python
>>> timetable['cat'] = ['up','down','left','left','up','up','down','right', 'right','up']   

>>> timetable
                 date    value1    value2    cat
0 2015-01-01 00:00:00  0.545737  0.772712     up
1 2015-01-01 01:00:00  0.236035  0.082847   down
2 2015-01-01 02:00:00  0.248293  0.938431   left
3 2015-01-01 03:00:00  0.888109  0.605302   left
4 2015-01-01 04:00:00  0.632222  0.080418     up
5 2015-01-01 05:00:00  0.249867  0.235366     up
6 2015-01-01 06:00:00  0.993940  0.125965   down
7 2015-01-01 07:00:00  0.154491  0.641867  right
8 2015-01-01 08:00:00  0.856238  0.521911  right
9 2015-01-01 09:00:00  0.307773  0.332822     up           
```

然而，这里显示的示例具有重复的键值。


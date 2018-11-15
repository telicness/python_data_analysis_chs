
[*第五章：pandas：读写数据*](./README.md)


# 5.1. I/O API工具

panda是一个专门用于数据分析的库，所以您希望它主要关注计算和数据处理。从外部文件中写入和读取数据的过程可以看作是数据处理的一部分。实际上，您将看到，即使在这个阶段，您也可以执行一些操作，以便为操作准备传入的数据。

因此，这个步骤对于数据分析非常重要，因此，这个目的的特定工具必须出现在pandas库中——一组称为I/O API的函数。这些功能分为两大类:读者和作者。

读取工具| 写入工具
|------- |:---------|
read_csv   | to_csv
read_excel | to_excel
read_hdf |  to_hdf
read_sql |  to_sql
read_json |  to_json
read_html |  to_html
read_stata |  to_stata
read_clipboard|   to_clipboard
read_pickle |  to_pickle
read_msgpack |  to_msgpack (experimental)
read_gbq to_gbq |  (experimental)
-----------------------------



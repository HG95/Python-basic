# Python pandas读写csv文件

## pandas.read_csv函数

```python
pandas.read_csv(filepath_or_buffer,
 				sep=', ', 
 				usecols=None, 
 				engine=None, 
                header='infer',
                skiprows=None,
                nrows=None, 
                skipfooter=0
               )


```

- `filepath_or_buffer`：可以是一个URL或者本地文件。有效的URL包括http，ftp，s3和文件。也可以是本地文件：table.csv（在本机的绝对地址）。
- `sep`：分隔符。如果sep为None，那么C引擎不会自动检测到分隔符，但是Python解释器可以使用，这意味着后者将被使用，并通过Python的内置嗅探工具csv.Sniffer自动检测分隔符。 另外，长度超过1个字符且与’\ s +‘不同的分隔符将被解释为正则表达式，并且还将强制使用Python解释器。 请注意，正则表达式分隔符很容易忽略带引号的数据。 正则表达式示例：’\ r \ t’
- `usecols`：返回列的一个子集。 如果是数组的，所有元素必须是位置索引的（即文档列中的整数索引），或者是与由用户提供的名称或从文档标题行推断的列名相对应的字符串。 例如，有效的类似数组的usecols参数应该是[0,1,2]或[‘foo’，‘bar’，‘baz’]。如果可调用，则可调用函数将根据列名进行评估，返回可调用函数评估为True的名称。 一个有效的可调用参数的例子是[‘AAA’，‘BBB’，‘DDD’]中的lambda x：x.upper（）。 使用此参数可以缩短解析时间并降低内存使用量。
- `header`：指定第几行来作为列名，默认是数据最开始的那一行。如果文件中没有列名，则默认为0，否则设置为None。如果明确设定header=0就会替换掉原来存在列名。header参数可以是一个list例如：[0,1,3]，这个list表示将文件中的这些行作为列标题（意味着每一列有多个标题），介于中间的行将被忽略掉（例如本例中的2；本例中的数据1,2,4行将被作为多级标题出现，第3行数据将被丢弃，dataframe的数据从第5行开始。）。 注意：如果skip_blank_lines=True 那么header参数忽略注释行和空行，所以header=0表示第一行数据而不是文件的第一行。
- `skiprows`：list-like，int或callable，optional 要在文件开头跳过
  （0索引）或要跳过的行数（int）的行号。 如果是可调用的，则将根据行索引计算可调用函数，如果应该跳过该行则返回True，否则返回False。
  有效可调参数的一个例子是[0,2]中的lambda x：x。
- `nrows`：int，可选 要读取的文件行数。用于读取大文件。
- `engine`：使用的解释器。{‘c’, ‘python’}二选一。
- `skipfooter`：文件底部要跳过的行数（不支持引擎=‘c’）



# DataFrame.to_csv

**to_csv()是DataFrame类的方法，read_csv()是pandas的方法**

```
dt.to_csv() #默认dt是DataFrame的一个实例，参数解释如下
```

```python
DataFrame.to_csv(path_or_buf=None, sep=',', 
                 na_rep='', float_format=None, 
                 columns=None, header=True, 
                 index=True, index_label=None, 
                 mode='w', encoding=None, 
                 compression='infer', quoting=None, 
                 quotechar='"', line_terminator=None, 
                 chunksize=None, date_format=None, 
                 doublequote=True, escapechar=None, 
                 decimal='.', errors='strict')
```

- **path_or_buf=None：** string or file handle, default None
  File path or object, if None is provided the result is returned as a string.
  字符串或文件句柄，默认无文件
  路径或对象，如果没有提供，结果将返回为字符串。

- **sep :** character, default ‘,’
  Field delimiter for the output file.
  默认字符 ‘ ，’
  输出文件的字段分隔符。

- **na_rep :** string, default ‘’
  Missing data representation
  字符串，默认为 ‘’
  浮点数格式字符串

- **float_format :** string, default None
  Format string for floating point numbers
  字符串，默认为 None
  浮点数格式字符串

- **columns :** sequence, optional Columns to write
  顺序，可选列写入

- **header :** boolean or list of string, default True
  Write out the column names. If a list of strings is given it is assumed to be aliases for the column names
  字符串或布尔列表，默认为true
  写出列名。如果给定字符串列表，则假定为列名的别名。

- **index :** boolean, default True
  Write row names (index)
  布尔值，默认为Ture
  写入行名称（索引）

- **index_label :** string or sequence, or False, default None
  Column label for index column(s) if desired. If None is given, and header and index are True, then the index names are used. A sequence should be given if the DataFrame uses MultiIndex. If False do not print fields for index names. Use index_label=False for easier importing in R
  字符串或序列，或False,默认为None
  如果需要，可以使用索引列的列标签。如果没有给出，且标题和索引为True，则使用索引名称。如果数据文件使用多索引，则应该使用这个序列。如果值为False，不打印索引字段。在R中使用index_label=False 更容易导入索引.

- **mode :** str
  模式：值为‘str’，字符串
  Python写模式，默认“w”

- **encoding :** string, optional
  编码：字符串，可选
  表示在输出文件中使用的编码的字符串，Python 2上默认为“ASCII”和Python 3上默认为“UTF-8”。

- **compression :** string, optional
  字符串，可选项
  表示在输出文件中使用的压缩的字符串，允许值为“gzip”、“bz2”、“xz”，仅在第一个参数是文件名时使用。

- **line_terminator :** string, default ‘\n’
  字符串，默认为 ‘\n’
  在输出文件中使用的换行字符或字符序列

- **quoting :** optional constant from csv module
  CSV模块的可选常量
  默认值为to_csv.QUOTE_MINIMAL。如果设置了浮点格式，那么浮点将转换为字符串，因此csv.QUOTE_NONNUMERIC会将它们视为非数值的。

- **quotechar :** string (length 1), default ‘”’
  字符串（长度1），默认“”
  用于引用字段的字符

- **doublequote :** boolean, default True
  布尔，默认为Ture
  控制一个字段内的quotechar

- **escapechar :** string (length 1), default None
  字符串（长度为1），默认为None
  在适当的时候用来转义sep和quotechar的字符

- **chunksize :** int or None
  int或None
  一次写入行

- **tupleize_cols :** boolean, default False
  布尔值 ，默认为False
  从版本0.21.0中删除：此参数将被删除，并且总是将多索引的每行写入CSV文件中的单独行
  （如果值为false）将多索引列作为元组列表（如果TRUE）或以新的、扩展的格式写入，其中每个多索引列是CSV中的一行。

- **date_format :** string, default None
  字符串，默认为None
  字符串对象转换为日期时间对象

- **decimal:** string, default ‘.’
  字符串，默认’。’
  字符识别为小数点分隔符。例如。欧洲数据使用 ​​’，’



参考

- <a href="https://blog.csdn.net/u010801439/article/details/80033341" target="_blank">pandas系列 read_csv 与 to_csv 方法各参数详解（全，中文版）</a> 
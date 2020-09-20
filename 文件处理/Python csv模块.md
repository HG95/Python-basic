# Python csv模块

csv模块包含在Python标准库中，可用于分析CSV文件中的数据行，让我们能够快速提取感兴趣的值。

csv模块 中的方法 只能够读取 / 写入到一个`sheet`中

## `csv.reader()`

```python
csv.reader(csvfile, dialect='excel', **fmtparams)
```

参数：

- `csvfile`，必须是支持迭代(Iterator)的对象，可以是文件(file)对象或者列表(list)对象，如果是文件对象，打开时需要加"b"标志参数。
- `dialect`，编码风格，默认为excel的风格，也就是用逗号（,）分隔，dialect方式也支持自定义，通过调用register_dialect方法来注册，。
- `fmtparam`，格式化参数，用来覆盖之前dialect对象指定的编码风格。


```python

import csv
with open('test.csv','rb') as myFile:
    lines=csv.reader(myFile)
    for line in lines:
        print line

```

`open()`返回了一个文件对象myFile，`reader(myFile)`只传入了第一个参数，另外两个参数采用缺省值，即以excel风格读入。reader()返回一个 reader 对象 lines, ines 是一个list，当调用它的方法lines.next()时，会返回一个string。

## `csv.writer()`

```
csv.writer(csvfile, dialect='excel', **fmtparams)
```

```

with open('t.csv','wb') as myFile:    
    myWriter=csv.writer(myFile)
    myWriter.writerow([7,'g'])
    myWriter.writerow([8,'h'])
    myList=[[1,2,3],[4,5,6]]
    myWriter.writerows(myList)


```

`csv.writer(myFile)`返回writer对象myWriter。

**`writerow()方法`是一行一行写入，`writerows`方法是一次写入多行。**

补充：除了writerow、writerows，writer对象还提供了其他一些方法：writeheader、dialect

## 追加

除了直接写入，还能实现追加：还是刚才那个例子，我现在将一行新的数据添加到旧的数据后面，最后写入CSV

```python
import csv

# 新增的数据行，以列表的形式表示
add_info = ["Guo", 150]

# 以添加的形式写入csv，跟处理txt文件一样，设定关键字"a"，表追加
csvFile = open("instance.csv", "a")

# 新建对象writer
writer = csv.writer(csvFile)

# 写入，参数还是列表形式
writer.writerow(add_info)

csvFile.close()

```









## 案例：从 CSV 文件中删除表头

```python
#! python3
# removeCsvHeader.py - Removes the header from all CSV files in the current
# working directory.
import csv, os

os.makedirs('headerRemoved', exist_ok=True)
# Loop through every file in the current working directory.
for csvFilename in os.listdir('.'):
	if not csvFilename.endswith('.csv'):
		continue # skip non-csv files
	print('Removing header from ' + csvFilename + '...')

	# Read the CSV file in (skipping first row).
	csvRows = []
	csvFileObj = open(csvFilename)
	readerObj = csv.reader(csvFileObj)
	for row in readerObj:
		if readerObj.line_num == 1:   #利用 line_num 属性确定要跳过哪一行
			continue # skip first row
		csvRows.append(row)
	csvFileObj.close()

	# Write out the CSV file.
	csvFileObj = open(os.path.join('headerRemoved', csvFilename), 'w',	newline='')
	csvWriter = csv.writer(csvFileObj)
	for row in csvRows:
		csvWriter.writerow(row)
	csvFileObj.close()

```

## `csv.DictReader`

```python
class csv.DictReader(csvfile, 
                     fieldnames=None, 
                     restkey=None, 
                     restval=None,
                     dialect='excel', 
                     *args, **kwds
                    )

```

创建一个对象，其操作类似于普通读取器，但将读取的信息映射到一个 dict 中，其中的键由可选的 fieldnames 参数给出。fieldnames 参数是一个 sequence，其元素按顺序与输入数据的字段相关联。这些元素成为结果字典的键。如果省略 fieldnames 参数，则 csvfile 的第一行中的值将用作字段名称，如果读取的行具有比字段名序列更多的字段，则剩余数据将作为键值为 restkey 的序列添加。如果读取的行具有比字段名序列少的字段，则剩余的键使用可选的 restval 参数的值。任何其他可选或关键字参数都传递给底层的 reader 实例。

DictReader，和 reader 函数类似，接收一个可迭代的对象，能返回一个生成器，但是返回的每一个单元格都放在一个字典的值内，而这个字典的键则是这个单元格的标题（即列头）


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618134224386.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_4,color_FFFFFF,t_70)

```python
with open('names.csv') as csvfile:
    reader = csv.DictReader(csvfile)
    for row in reader:
        print(row['first_name'], row['last_name'])
#输出
Baked Beans
Lovely Spam
Wonderful Spam

```

```python
import csv

#读
with open("test.csv", "r", encoding = "utf-8") as f:
    reader = csv.DictReader(f)
    column = [row for row in reader]

print(column)

输出：
[{'No.': '1', 'Age': '18', 'Score': '99', 'Name': 'mayi'},
 {'No.': '2', 'Age': '21', 'Score': '89', 'Name': 'jack'},
 {'No.': '3', 'Age': '25', 'Score': '95', 'Name': 'tom'},
 {'No.': '4', 'Age': '19', 'Score': '80', 'Name': 'rain'}]


```

如果我们想用 DictReader 读取 csv 的某一列，就可以用列的标题查询：

```python
import csv

#读取Name列的内容
with open("test.csv", "r", encoding = "utf-8") as f:
    reader = csv.DictReader(f)
    column = [row['Name'] for row in reader]
print(column)

输出：
['mayi', 'jack', 'tom', 'rain']

```

## `csv.DictWriter`

```python
class csv.DictWriter(csvfile, 
                     fieldnames, 
                     restval='', 
                     extrasaction='raise', 
                     dialect='excel', 
                     *args, **kwds
                    )

```

创建一个操作类似于常规writer的对象，但将字典映射到输出行。fieldnames参数是一个sequence，用于标识传递给 writerow() 方法的字典中的值被写入csvfile。如果字典在fieldnames中缺少键，则可选的restval参数指定要写入的值。如果传递给writerow()方法的字典包含fieldnames中未找到的键，则可选的extrasaction参数指示要执行的操作。如果设置为’raise’，则会引发ValueError。如果设置为’ignore’，则会忽略字典中的额外值。任何其他可选或关键字参数都传递给底层的writer实例。

请注意，与 DictReader 类不同，DictWriter 的 fieldnames 参数不是可选的。由于Python的 dict 对象没有排序，因此没有足够的信息来推断将该行写入到csvfile的顺序。


```python
import csv

with open('names.csv', 'w') as csvfile:
    fieldnames = ['first_name', 'last_name']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

    writer.writeheader()
    writer.writerow({'first_name': 'Baked', 'last_name': 'Beans'})
    writer.writerow({'first_name': 'Lovely', 'last_name': 'Spam'})
    writer.writerow({'first_name': 'Wonderful', 'last_name': 'Spam'})

```

![img](https://img-blog.csdnimg.cn/20190618115408120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_4,color_FFFFFF,t_70)

`delimiter`**和** `lineterminator` **关键字参数**

假定你希望用制表符代替逗号来分隔单元格，并希望有两倍行距。

```
>>> import csv
>>> csvFile = open('example.tsv', 'w', newline='')
>>> csvWriter = csv.writer(csvFile, delimiter='\t', lineterminator='\n\n')
>>> csvWriter.writerow(['apples', 'oranges', 'grapes'])
24
>>> csvWriter.writerow(['eggs', 'bacon', 'ham'])
17
>>> csvWriter.writerow(['spam', 'spam', 'spam', 'spam', 'spam', 'spam'])
32
>>> csvFile.close(

```

默认情况下， CSV 文件的分隔符是逗号。行终止字符是出现在行末的字符。默认情况下，行终止字符是换行符。你可以利用 `csv.writer()`的 `delimiter` 和 `lineterminator`关键字参数，将这些字符改成不同的值。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618141639837.png)
# Python ZIP文件读取，解压

zipfile 模块是一个底层模块，是Python标准库的一部分。 zipfile 具有可以轻松打开和提取ZIP文件的函数。 要读取ZIP文件的内容，首先要做的是创建一个 ZipFile 对象。ZipFile 对象类似于使用 open() 创建的文件对象。ZipFile 也是一个上下文管理器，因此支持with语句：


```python
import zipfile

with zipfile.ZipFile('data.zip', 'r') as zipobj:
    pass

```

这里创建一个`ZipFile 对象`，传入ZIP文件的名称并以读取模式下打开。 打开ZIP文件后，可以通过 `zipfile 模块`提供的函数访问有关存档文件的信息。 上面示例中的 data.zip 存档是从名为 data 的目录创建的，该目录包含总共5个文件和1个子目录：

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504093012.png"/>

要获取存档文件中的文件列表，请在 ZipFile 对象上调用 `namelist()`：

```python
import zipfile

with zipfile.ZipFile('data.zip', 'r') as zipobj:
    zipobj.namelist()

```

生成一个文件列表:

```
[‘file1.py’, ‘file2.py’, ‘file3.py’, ‘sub_dir/’, ‘sub_dir/bar.py’, ‘sub_dir/foo.py’]
```

`.namelist()`返回存档文件中文件和目录的名称列表。要检索有关存档文件中文件的信息，使用 `.getinfo()`：

```python

import zipfile

with zipfile.ZipFile('data.zip', 'r') as zipobj:
    bar_info = zipobj.getinfo('sub_dir/bar.py')
    print(bar_info.file_size)

```

## 提取ZIP文件

zipfile 模块允许你通过`.extract()`和` .extractall()`从ZIP文件中提取一个或多个文件。
默认情况下，这些方法将文件提取到当前目录。 它们都采用可选的路径参数，允许指定要将文件提取到的其他指定目录。 如果该目录不存在，则会自动创建该目录。 要从压缩文件中提取文件，请执行以下操作：

```python
>>> import zipfile
>>> import os

>>> os.listdir('.')
['data.zip']

>>> data_zip = zipfile.ZipFile('data.zip', 'r')

>>> # 提取单个文件到当前目录
>>> data_zip.extract('file1.py')
'/home/test/dir1/zip_extract/file1.py'

>>> os.listdir('.')
['file1.py', 'data.zip']

>>> # 提所有文件到指定目录
>>> data_zip.extractall(path='extract_dir/')

>>> os.listdir('.')
['file1.py', 'extract_dir', 'data.zip']

>>> os.listdir('extract_dir')
['file1.py', 'file3.py', 'file2.py', 'sub_dir']

>>> data_zip.close()

```

第三行代码是对 `os.listdir() `的调用，它显示当前目录只有一个文件 data.zip 。

接下来，以读取模式下打开 data.zip 并调用` .extract() `从中提取 file1.py 。 .extract() 返回提取文件的完整文件路径。 由于没有指定路径，`.extract()` 会将 file1.py 提取到当前目录。

下一行打印一个目录列表，显示当前目录现在包括除原始存档文件之外的存档文件。 之后显示了如何将整个存档提取到指定目录中。`.extractall() `创建 extract_dir 并将 data.zip 的内容提取到其中。 最后一行关闭ZIP存档文件。


## 从加密的文档提取数据

zipfile 支持提取受密码保护的ZIP。 要提取受密码保护的ZIP文件，请将密码作为参数传递给` .extract()`或`.extractall() `方法：

```python
>>> import zipfile

>>> with zipfile.ZipFile('secret.zip', 'r') as pwd_zip:
...     # 从加密的文档提取数据
...     pwd_zip.extractall(path='extract_dir', pwd='Quish3@o')

```

将以读取模式打开 secret.zip 存档。 密码提供给 .extractall() ，并且压缩文件内容被提取到 extract_dir 。 由于with语句，在完成提取后，存档文件会自动关闭。
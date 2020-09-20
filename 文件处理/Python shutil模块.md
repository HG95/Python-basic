# Python shutil模块

  ` shutil`（或称为 shell 工具）模块中包含一些函数，让你在 Python 程序中复制、移动、改名和删除文件。要使用 shutil 的函数，首先需要 import shutil。

## 复制文件和文件夹

`shutil 模块`提供了一些函数，用于复制文件和整个文件夹。

### `shutil.copy()`

调用 shutil.copy(source, destination)，**将路径 source 处的文件复制到路径 destination处的文件夹**（source 和 destination 都是字符串）。如果 destination 是一个文件名，它将作为被复制文件的新名字。该函数返回一个字符串，表示被复制文件的路径。

```python
>>> import shutil, os
>>> os.chdir('C:\\')
>>> shutil.copy('C:\\spam.txt', 'C:\\delicious')
'C:\\delicious\\spam.txt'
>>> shutil.copy('eggs.txt', 'C:\\delicious\\eggs2.txt')
'C:\\delicious\\eggs2.txt'

```

第一个 `shutil.copy()`调用将文件 C:\spam.txt 复制到文件夹 C:\delicious。返回值是刚刚被复制的文件的路径。

请注意，因为指定了一个文件夹作为目的地，原来的文件名 spam.txt 就被用作新复制的文件名。第二个 shutil.copy()调用也将文件C:\eggs.txt 复制到文件夹 C:\delicious，但为新文件提供了一个名字 eggs2.txt.

### `shutil.copytree()`

shutil.copy()将复制一个文件， `shutil.copytree()`将复制整个文件夹，以及它包含的文件夹和文件。调用 shutil.copytree(source, destination)，将路径 source 处的文件夹，包括它的所有文件和子文件夹，复制到路径 destination 处的文件夹。 source 和destination 参数都是字符串。该函数返回一个字符串，是新复制的文件夹的路径。


```python
>>> import shutil, os
>>> os.chdir('C:\\')
>>> shutil.copytree('C:\\bacon', 'C:\\bacon_backup')
'C:\\bacon_backup'

```

`shutil.copytree()` 调用创建了一个新文件夹， 名为 bacon_backup，其中的内容与原来的 bacon 文件夹一样。现在你已经备份了非常非常宝贵的“bacon”

## 文件和文件夹的移动与改名

### `shutil.move()`

调用 `shutil.move(source, destination)`， 将路径 source 处的文件夹移动到路径destination，并返回新位置的绝对路径的字符串。
如果 destination 指向一个文件夹， source 文件将移动到 destination 中， 并保持原来的文件名。

```python
>>> import shutil
>>> shutil.move('C:\\bacon.txt', 'C:\\eggs')
'C:\\eggs\\bacon.txt'

```

假定在 C:\目录中已存在一个名为 eggs 的文件夹， 这个 shutil.move()调用就是说，“将 C:\bacon.txt 移动到文件夹 C:\eggs 中。
如果在 C:\eggs 中原来已经存在一个文件 bacon.txt，它就会被覆写。因为用这种方式很容易不小心覆写文件， 所以在使用 move()时应该注意。

destination 路径也可以指定一个文件名。在下面的例子中， source 文件被移动并改名。

```python
>>> shutil.move('C:\\bacon.txt', 'C:\\eggs\\new_bacon.txt')
'C:\\eggs\\new_bacon.txt'

```

这一行是说，“将 C:\bacon.txt 移动到文件夹 C:\eggs，完成之后，将 bacon.txt文件改名为 new_bacon.txt。”

## 永久删除文件和文件夹

`shutil.rmtree(path)`

利用 os 模块中的函数，可以删除一个文件或一个空文件夹。但利用 `shutil 模块`，可以删除一个文件夹及其所有的内容。

- 用 `os.unlink(path)`将删除 path 处的文件。
- 调用 `os.rmdir(path)`将删除 path 处的文件夹。该文件夹必须为空，其中没有任何文件和文件夹。
- 调用`shutil.rmtree(path)`将删除 path 处的文件夹，它包含的所有文件和文件夹都会被删除。
  

```python
import os
for filename in os.listdir():
	if filename.endswith('.rxt'):
		os.unlink(filename)

```


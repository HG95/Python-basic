# Python 文件过滤

`endswith()` 和 `startswith()`字符串方法

`fnmatch.fnmatch()`

`glob.glob()`

`pathlib.Path.glob()`

## 使用字符串方法

Python有几个内置 修改和操作字符串 的方法。当在匹配文件名时，其中的两个方法 `.startswith()`和 `.endswith()`非常有用。要做到这点，首先要获取一个目录列表，然后遍历。

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504083824.png"/>

```python
import os

for f_name in os.listdir('some_directory'):
    if f_name.endswith('.txt'):
        print(f_name)
```

## `fnmatch.fnmatch()`

使用 fnmatch进行简单文件名模式匹配
字符串方法匹配的能力是有限的。fnmatch 有对于模式匹配有更先进的函数和方法。我们将考虑使用 `fnmatch.fnmatch()`，这是一个支持使用`*`和 `?`等通配符的函数。例如，使用 `fnmatch` 查找目录中所有 .txt 文件，你可以这样做:

```python
import os
import fnmatch

for f_name in os.listdir('some_directory'):
    if fnmatch.fnmatch(f_name, '*.txt'):
        print(f_name   
```

```python
import os
import fnmatch

for f_name in os.listdir('some_directory'):
    if fnmatch.fnmatch(f_name, 'data_*_backup.txt'):
        print(f_name)

```


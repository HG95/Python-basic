# Python pathlib模块

`pathlib`是 Python 内置库，Python 文档给它的定义是 Object-oriented filesystem paths（面向对象的文件系统路径）。pathlib 提供表示文件系统路径的类，其语义适用于不同的操作系统。路径类在纯路径之间划分，纯路径提供纯粹的计算操作而没有I / O，以及具体路径，它继承纯路径但也提供I / O操作。

## 基本用法

```python
Path.iterdir()　　# 遍历目录的子目录或者文件
				 # 相当于os.listdir

Path.is_dir()　　	# 判断是否是目录（文件夹）

Path.glob()　　	# 过滤目录(返回生成器)

Path.resolve()　　# 返回绝对路径

Path.exists()　　	# 判断路径是否存在

Path.open()　　	# 打开文件(支持with)

Path.unlink()　　	# 删除文件或目录(目录非空触发异常)

```

`Path.glob()`

```python
>>> sorted(Path('.').glob('*.py'))
[PosixPath('pathlib.py'), PosixPath('setup.py'), PosixPath('test_pathlib.py')]
>>> sorted(Path('.').glob('*/*.py'))
[PosixPath('docs/conf.py')]
>>> sorted(Path('.').glob('**/*.py'))
[PosixPath('build/lib/pathlib.py'),
 PosixPath('docs/conf.py'),
 PosixPath('pathlib.py'),
 PosixPath('setup.py'),
 PosixPath('test_pathlib.py')]
# The "**" pattern means "this directory and all subdirectories, recursively"

```

`Path.iterdir()`

使用`.iterdir`方法获取当前文件下的所以文件.

```python
import pathlib
from collections import Counter
now_path = pathlib.Path.cwd()
gen = (i.suffix for i in now_path.iterdir())
print(Counter(gen))

# Counter({'.py': 16, '': 11, '.txt': 1, '.png': 1, '.csv': 1})

```



## 基本属性

```python
Path.parts　　# 分割路径 类似os.path.split(), 不过返回元组

Path.drive　　# 返回驱动器名称

Path.root　　# 返回路径的根目录

Path.anchor　　# 自动判断返回drive或root

Path.parents　　# 返回所有上级目录的列表

```

## 改变路径

```python
Path.with_name()　　# 更改路径名称, 更改最后一级路径名

Path.with_suffix()　　# 更改路径后缀

```

`PurePath.with_name(name)`

返回一个新的路径并修改 name。如果原本路径没有 name，ValueError 被抛出:

```python
>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')
>>> p.with_name('setup.py')
PureWindowsPath('c:/Downloads/setup.py')
>>> p = PureWindowsPath('c:/')
>>> p.with_name('setup.py')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/antoine/cpython/default/Lib/pathlib.py", line 751, in with_name
    raise ValueError("%r has an empty name" % (self,))
ValueError: PureWindowsPath('c:/') has an empty name

```



## 拼接路径

```python
Path.joinpath()　　# 拼接路径

Path.relative_to()　　# 计算相对路径

```

## 测试路径

```python
Path.match()　　# 测试路径是否符合pattern

Path.is_dir()　　# 是否是文件

Path.is_absolute()　　# 是否是绝对路径

Path.is_reserved()　　# 是否是预留路径

Path.exists()　　# 判断路径是否真实存在

```

`PurePath.is_absolute()`

返回此路径是否为绝对路径。如果路径同时拥有驱动器符与根路径（如果风格允许）则将被认作绝对路径。

```python
>>> PurePosixPath('/a/b').is_absolute()
True
>>> PurePosixPath('a/b').is_absolute()
False

>>> PureWindowsPath('c:/a/b').is_absolute()
True
>>> PureWindowsPath('/a/b').is_absolute()
False
>>> PureWindowsPath('c:').is_absolute()
False
>>> PureWindowsPath('//some/share').is_absolute()
True

```

`PurePath.match(pattern)`

将此路径与提供的通配符风格的模式匹配。如果匹配成功则返回 True，否则返回 False。
如果 pattern 是相对的，则路径可以是相对路径或绝对路径，**并且匹配是从右侧完成的：**

```python
>>> PurePath('a/b.py').match('*.py')
True
>>> PurePath('/a/b/c.py').match('b/*.py')
True
>>> PurePath('/a/b/c.py').match('a/*.py')
False

```



## 其他方法

```python
Path.cwd()　　# 返回当前目录的路径对象

Path.home()　　# 返回当前用户的home路径对象

Path.stat()　　# 返回路径信息, 同os.stat()

Path.chmod()　　# 更改路径权限, 类似os.chmod()

Path.expanduser()　　# 展开~返回完整路径对象

Path.mkdir()　　# 创建目录

Path.rename()　　# 重命名路径

Path.rglob()　　# 递归遍历所有子目录的文件

```

## 列出指定类型的文件

```python
p = pathlib.Path('./')
# print([str(x) for x in p.iterdir() ])

print([i for i in p.glob('*.py')])
#[WindowsPath('01.py'), WindowsPath('2018年Tibet温度统计.py'),
# WindowsPath('lob.py')]

```

## 列出所有子目录

```python
import  pathlib

p = pathlib.Path('./')
print([x for x in p.iterdir() if x.is_dir()])

#当前目录所有文件
p = pathlib.Path('./')
print([x for x in p.iterdir() ])
#[WindowsPath('.idea'), WindowsPath('01.py'), WindowsPath('数据下载.xlsx'), WindowsPath('数据下载3.xlsx')]

#转化为字符串
p = pathlib.Path('./')
print([str(x) for x in p.iterdir() ])
#['.idea', '01.py',  '数据下载.xlsx', '数据下载3.xlsx']


```

## 路径拼接 可以使用`/`符号来拼接路径

```python

p = pathlib.Path('./')
# print([str(x) for x in p.iterdir() ])
q = pathlib.Path(r'F:\cookies\python')

print([str(q/i )  for i in p.glob('*.py')])
print([str(q.joinpath(i))  for i in p.glob('*.py')])
'''
['F:\\cookies\\python\\01.py', 'F:\\cookies\\python\\2018年Tibet温度统计.py', 'F:\\cookies\\python\\lob.py']
['F:\\cookies\\python\\01.py', 'F:\\cookies\\python\\2018年Tibet温度统计.py', 'F:\\cookies\\python\\lob.py']
'''

p = pathlib.Path(r'F:\cookies\python')
q = p / 'learnPython'
print(q)

#F:\cookies\python\learnPython

```

```python
data_folder = Path("source_data/text_files/")

file_to_open = data_folder / "raw_data.txt"
```







打开文件

```python

>>> q = q / "hello_world.py"
>>> with q.open() as f:
>>> print(f.readline())
#!/usr/bin/env python

```

## PurePath

PurePath 是一个纯路径对象，纯路径对象提供了实际上不访问文件系统的路径处理操作。有三种方法可以访问这些类，我们也称之为flavor。

一个通用的类，代表当前系统的路径风格（实例化为 `PurePosixPath` 或者 `PureWindowsPath`）:
产生Pure paths的三种方式

```
class pathlib.PurePath(*pathsegments)
```

```python
m=pathlib.PurePath('foo', 'some/path', 'bar')
print(m)        #foo\some\path\bar

n=pathlib.PureWindowsPath('foo', 'some/path', 'bar')
print(n)        #foo\some\path\bar


pa=pathlib.PurePath(pathlib.Path('foo'), pathlib.Path('bar'))
print(pa)       #foo\bar

pa=pathlib.PureWindowsPath('foo','bar')
print(pa)       #foo\bar

```

如果参数为空，则默认指定当前文件夹

```python
print(pathlib.PurePath())
#.

```

当同时指定多个绝对路径,则使用最后一个

```python

>>> PureWindowsPath('c:/Windows', 'd:bar')
PureWindowsPath('d:bar')

```

```python
class pathlib.PurePosixPath(*pathsegments)
一个 PurePath 的子类，路径风格不同于 Windows 文件系统:

```

```python
class pathlib.PureWindowsPath(*pathsegments)
PurePath 的一个子类，路径风格为 Windows 文件系统路径:

```

### 常用属性和方法

```python
返回驱动路径名称
>>> PureWindowsPath('c:/Program Files/').drive
'c:'

不太明白
print(pathlib.PureWindowsPath(r'c:/Program Files/').root)   #\

```

### `name` 获取文件名

````python
p = Path(r'd:\test\tt.txt.bk')
p.name                          # 获取文件名

PureWindowsPath(r'd:\test\tt.txt.bk').name
````

### 获取文件的信息(`size`,` createtime`...)

```python
p = Path(r'd:\test\tt.txt')
p.stat()                        # 获取详细信息
# os.stat_result(st_mode=33206, st_ino=562949953579011, st_dev=3870140380, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1525254557, st_mtime=1525254557, st_ctime=1525254557)
p.stat().st_size                # 文件大小
# 0
p.stat().st_ctime               # 创建时间
# 1525254557.2090347
# 其他的信息也可以通过相同方式获取
p.stat().st_mtime               # 修改时间
```





### `parents`属性

获取不同等级的根目录

```python
>>> p = PureWindowsPath('c:/foo/bar/setup.py')
>>> p.parents[0]
PureWindowsPath('c:/foo/bar')

>>> p.parents[1]
PureWindowsPath('c:/foo')

>>> p.parents[2]
PureWindowsPath('c:/')

```

此路径的逻辑父路径: `PurePath.parent`

```python
>>> p = PurePosixPath('/a/b/c/d')
>>> p.parent
PurePosixPath('/a/b/c')

```

访问个别部分:`parts`
一个元组，可以访问路径的多个组件:

```python
>>> p = PurePath('/usr/bin/python3')
>>> p.parts
('/', 'usr', 'bin', 'python3')

>>> p = PureWindowsPath('c:/Program Files/PSF')
>>> p.parts
('c:\\', 'Program Files', 'PSF')

```

### `PurePath.name`属性

可以获取文件的名字，包含拓展名。

```python
>>> PureWindowsPath('//some/share/setup.py').name
'setup.py'
>>> PureWindowsPath('//some/share').name
''

```

### PurePath.suffix

最后一个组件的文件扩展名，如果存在:：`PurePath.suffix`、
路径的文件扩展名列表:`PurePath.suffixes`属性

```python
>> PurePosixPath('my/library/setup.py').suffix
'.py'
>>> PurePosixPath('my/library.tar.gz').suffix
'.gz'
>>> PurePosixPath('my/library').suffix
''

>>> PurePosixPath('my/library.tar.gar').suffixes
['.tar', '.gar']
>>> PurePosixPath('my/library.tar.gz').suffixes
['.tar', '.gz']
>>> PurePosixPath('my/library').suffixes
[]

```

### PurePath.stem

最后一个路径组件，除去后缀:

```python
>>> PurePosixPath('my/library.tar.gz').stem
'library.tar'
>>> PurePosixPath('my/library.tar').stem
'library'
>>> PurePosixPath('my/library').stem
'library'

```

### PurePath.with_suffix(suffix)

返回一个新的路径并修改 suffix。如果原本的路径没有后缀，新的 suffix 则被追加以代替。如果 suffix 是空字符串，则原本的后缀被移除:

```python
>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')
>>> p.with_suffix('.bz2')
PureWindowsPath('c:/Downloads/pathlib.tar.bz2')
>>> p = PureWindowsPath('README')
>>> p.with_suffix('.txt')
PureWindowsPath('README.txt')
```





参考

1. <a href="https://docs.python.org/zh-cn/3/library/pathlib.html" blank="">`pathlib`--- 面向对象的文件系统路径</a> 
2. <a href="https://www.jianshu.com/p/a820038e65c3" blank="">超好用python库(Pathlib)</a> 
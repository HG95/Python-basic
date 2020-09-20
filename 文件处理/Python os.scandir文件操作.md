# Python os.scandir文件操作

`scandir`方法返回了一个DirEntry迭代器对象，它非常轻巧方便，并且能告诉你迭代文件的路径。之前案例中，我们检查了entry是一个文件或者是一个文件夹，与此同时，我们添加它的路径到列表中。

在 Python 3.5版本中，新添加了 os.scandir()方法，它是一个目录迭代方法。os.scandir() 的运行效率要比 os.walk 高。在 PEP 471 中，Python 官方也推荐我们使用 os.scandir() 来遍历目录。

具有以下属性和方法：

- `name·: 条目的文件名，相对于 scandir path 参数( 对应于 os.listdir的返回值)
- `path·: 输入路径 NAME ( 不一定是绝对路径) --与 os.path.join(scandir_path, entry.name)
- `is_dir`(*, follow_symlinks=True): 类似于 pathlib.Path.is_dir()，但返回值在 DirEntry 对象上是缓存；大多数情况下不需要系统调用；如果 follow_symlinks 是 false，则不要跟随符号链接。
  *
- `is_file(*, follow_symlinks=True)`: 类似于 pathlib.Path.is_file()，但返回值在 DirEntry 对象上是缓存；大多数情况下不需要系统调用；如果 follow_symlinks 是 false，则不要跟随符号链接。
- `is_symlink()`: 类似 pathlib.Path.is_symlink()，但返回值缓存在 DirEntry 对象上；大多数情况下不需要系统调用
- `stat(*, follow_symlinks=True)`: 类似 os.stat()，但返回值缓存在 DirEntry 对象上；不需要对 Windows (。除了符号符号外) 进行系统调用；如果 follow_symlinks 是 false，则不跟随符号链接( 像 os.lstat() )。
- `inode()`: 返回项的节点数；返回值在 DirEntry 对象上缓存


<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504162949.png"/>

```
for i in os.scandir(r'.\test'):
    print('Filename:',i.name,'Filepath:',i.path)
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504163027.png"/>





````python
import os
import shutil
 
data_path='D:/oanda/'   
target_path='D:/data/'
with os.scandir(data_path) as it:
    for entry in it:
        if not entry.name.startswith('.') and entry.is_file():
            file_name=entry.name
            pair='__'.join(file_name.split('__')[:2])
            shutil.move(data_path+file_name,target_path+pair+'/'+file_name) 
            print(file_name)

````


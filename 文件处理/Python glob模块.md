# Python glob模块

功能描述：`glob模块`可以使用Unix shell风格的通配符匹配符合特定格式的文件和文件夹，跟windows的文件搜索功能差不多。glob模块并非调用一个子shell实现搜索功能，而是在内部调用了`os.listdir(`)和`fnmatch.fnmatch()`。

glob模块共包含以下3个函数：

- `glob(pathname, recursive=False)`
  第一个参数 `pathname`为需要匹配的字符串。（该参数应尽量加上r前缀，以免发生不必要的错误）
  第二个参数代表递归调用，与特殊通配符“”一同使用，默认为False。
  **返回值为列表**
  该函数返回一个符合条件的路径的字符串列表，如果使用的是Windows系统，路径上的“\”符号会自动加上转义符号变为“\”（方便使用）。
  在3.5版本之后，glob函数支持一个特殊的通配符“”，该通配符可以匹配指定路径里所有文件和目录，包括子目录里的所有文件和目录。相当于递归地调用了这个函数。使用这个通配符必须加上 recursive=True 参数。
  （在有复杂目录结构的情况下使用该通配符可能会导致性能下降，拖累整个程序的运行，需谨慎使用！）
- `iglob(pathname, recursive=False)`
  参数与glob()一致。
  返回一个迭代器，该迭代器不会同时保存所有匹配到的路径，遍历该迭代器的结果与使用相同参数调用glob()的返回结果一致。
- `escape(pathname)`
  这个函数是在3.4版本之后才有的，功能是忽略所有通配符。（可以用于测试某文件是否存在）
  （3.5.1版本该函数不能正常运行，升级到3.5.2之后恢复正常）


**glob模块支持的通配符：**

![img](https://img-blog.csdnimg.cn/20190914233245617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)

```python
import glob
 
listglob = []
listglob = glob.glob(r"/home/xxx/picture/*.png")
listglob.sort()
print listglob
 
print '--------------------'
listglob = glob.glob(r"/home/xxx/picture/0?.png")
listglob.sort()
print listglob
 
print '--------------------'
listglob = glob.glob(r"/home/xxx/picture/0[0,1,2].png")
listglob.sort()
print listglob
 
print '--------------------'
listglob = glob.glob(r"/home/xxx/picture/0[0-3].png")
listglob.sort()
print listglob
 
print '--------------------'
listglob = glob.iglob(r"/home/xxx/picture/0[a-z].png")
print listglob
for item in listglob:
    print item

```

![img](https://img-blog.csdnimg.cn/20190914233731388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)

```python
import  glob,os
path="F:\\CodeFile\\my_directory"
l=[]
l=glob.glob(os.path.join(path,"*.txt"))
for i in l:
    print(i)


输出：
F:\CodeFile\my_directory\file3 - 副本 (2).txt
F:\CodeFile\my_directory\file3 - 副本 (3).txt
F:\CodeFile\my_directory\file3 - 副本 (4).txt
F:\CodeFile\my_directory\file3 - 副本.txt
F:\CodeFile\my_directory\file3.txt

```


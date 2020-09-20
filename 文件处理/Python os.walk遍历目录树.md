# Python os.walk遍历目录树

假定你希望对某个文件夹中的所有文件改名， 包括该文件夹中所有子文件夹中的所有文件。也就是说， 你希望遍历目录树， 处理遇到的每个文件。写程序完成这件事，可能需要一些技巧。 好在， Python 提供了一个函数， 替你处理这个过程。

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503232031.png"/>

```python
import os 
for dirpath, dirname, files in os.walk('.'):
	print(f'Found directory: {dirpath}') 
    for file_name in files: 
    	print(file_name)

```

`os.walk()`函数被传入一个字符串值，即一个文件夹的路径。

你可以在一个 for循环语句中使用 `os.walk()`函数，遍历目录树， 就像使用 range()函数遍历一个范围的数字一样。不像 range()，**`os.walk` 的返回值是一个生成器(generator),也就是说我们需要不断的遍历它，来获得所有的内容**, `os.walk()` 在循环的每次迭代中，返回 3 个值：

- 当前文件夹名称的字符串。
- 当前文件夹中子文件夹的字符串的列表。
- 当前文件夹中文件的字符串的列表。
  所谓当前文件夹，是指 for 循环当前迭代的文件夹。程序的当前工作目录，不会因为 `os.walk()`而改变。



如果我们有如下的文件结构:

```
      a ->   b   ->   1.txt,  2.txt
             c   ->   3.txt
             d   ->   
           4.txt
           5.txt
```

```python
for (root, dirs, files) in os.walk('a'):
    #第一次运行时，当前遍历目录为 a
    所以 root == 'a'
         dirs == [ 'b', 'c', 'd']
         files == [ '4.txt', '5.txt']
    
    。。。

    # 接着遍历 dirs 中的每一个目录
    b:  root  = 'a\\b'
        dirs  = []
        files = [ '1.txt', '2.txt']
    
    # dirs为空，返回
    # 遍历c
    c:  root  = 'a\\c'
        dirs  = []
        files = [ '3.txt' ]
    
    PS : 如果想获取文件的全路径，只需要 
    for f in files:
        path = os.path.join(root,f)
    
    # 遍历d
    d:  root  = 'a\\b'
        dirs  = []
        files = []

    遍历完毕，退出循环

```



<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504084851.png"/>



```python
import os 
for dirpath, dirname, files in os.walk('.'):
	print(f'Found directory: {dirpath}') 
    for file_name in files: 
    	print(file_name)

```







```
在每次迭代中，会打印出它找到的子目录和文件的名称：
Found directory: .
test1.txt
test2.txt
Found directory: ./folder_1
file1.py
file3.py
file2.py
Found directory: ./folder_2
file4.py
file5.py
file6.py

```


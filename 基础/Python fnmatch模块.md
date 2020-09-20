# Python fnmatch模块

`fnmatch`主要是用来判断一个文件名是否匹配”Unix shell-style wildcards”这种模式,就是平常用的那种ls *.log这样，看是否匹配。
用法很简单，常用也就这两个函数`fnmatch`/`filter`:
`fnmatch. fnmatch(filename, pattern)`
测试 filename 字符串是否匹配 pattern 字符串，返回 True 或 False。

| 通配符 |            含义             |
| :----: | :-------------------------: |
|   *    |     匹配任何数量的字符      |
|   ？   |        匹配单个字符         |
| [seq]  |      匹配 seq 中的字符      |
| [!seq] | 匹配除 seq 以外的任何的字符 |

fnmatch 这个库相对比较简单，只有4个函数，分别是`fnmatch`、fnmatchcase、`filter`和 translate，其中最常用的是fnmatch。主要功能如下：

- fnmatch：判断文件名是否符合特定的模式。
- fnmatchcase：判断文件名是否符合特定的模式，区分大小写。
- filter：返回输入列表中，符合特定模式的文件名列表。
- translate：将通配符模式转换成正则表达式。
- fnmatch和fnmatchcase用法相同，判断名称是否符合表达式，返回True or False
  

```python
>>> os.listdir(os.curdir)
['A1.jpg', 'a1.txt', 'a2.txt', 'aA.txt', 'b3.jpg', 'b2.jpg', 'b1.jpg']
 
>>> [ name for name in os.listdir(os.curdir) if fnmatch.fnmatch(name,'*.jpg') ]
['A1.jpg', 'b3.jpg', 'b2.jpg', 'b1.jpg']
 
>>> [ name for name in os.listdir(os.curdir) if fnmatch.fnmatch(name,"[ab]*") ]
['a1.txt', 'a2.txt', 'aA.txt', 'b3.jpg', 'b2.jpg', 'b1.jpg']
 
>>> [ name for name in os.listdir(os.curdir) if fnmatch.fnmatch(name,"[!a]*") ]
['A1.jpg', 'b3.jpg', 'b2.jpg', 'b1.jpg']
 
>>> [ name for name in os.listdir(os.curdir) if fnmatch.fnmatch(name,"b?.jpg") ]
['b3.jpg', 'b2.jpg', 'b1.jpg']
 
>>> [ name for name in os.listdir(os.curdir) if fnmatch.fnmatchcase(name,"A?.jpg") ]
['A1.jpg']



```

`filter`和`fnmatch`类似，只不过`filter`接受的第一个参数是一个文件名列表，返回符合表达式的列表(即：筛选)

```python

>>> name = os.listdir(os.curdir)
>>> name
['A1.jpg', 'a1.txt', 'a2.txt', 'aA.txt', 'b3.jpg', 'b2.jpg', 'b1.jpg']
>>> fnmatch.filter(name,'*.txt')
['a1.txt', 'a2.txt', 'aA.txt']

```


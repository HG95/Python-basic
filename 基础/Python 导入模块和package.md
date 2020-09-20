# Python 导入模块和package

**模块**：用来从逻辑上组织python代码(变量，函数，类)，本质上就是以.py结尾的python文件
**package**:本质上就是一个目录，但是必须带一个init.py的文件，它是用来从逻辑上组织模块的

import的本质是什么？
**导入模块的本质**：就是把 python 文件拿来解释一遍
**导入包的本质**：就是执行该 package 下的 `init.py` 文件、

**基础知识**
当你import的时候，python只会在sys.path这个变量（一个list，你可以print出来看）里面的路径中找可能匹配的package和module。

而一个package跟一个普通文件夹的区别在于，package的文件夹中多了一个__init__.py文件。换句话说，如果你在某个文件夹中添加了一个__init__.py文件，则python就认为这个文件夹是一个package。

init.py文件可以是空的（也推荐者这么做），它只是告诉python当前文件夹是一个package。当然，也可以在里面添加一些代码，这些代码会在import这个包的时候运行。

所以，请确保你要import的文件所在的文件夹有__init__.py文件（除非它在sys.path中某个文件夹下）。


如果我现在有一个这样的目录：

![img](https://img-blog.csdnimg.cn/20190626082220697.png)

在bin文件目录下的hello.py内容是:

```python
def hello():
    print('i am in bin dir')
```

在Day5文件目录下的hello.py内容是:

```python
def hello():
    print('i am in Day5 dir')

```

在pythonFile文件目录下的hello.py内容是:

```python
def hello():
    print('i am in pythonFile dir')

```

如果我现在在bin目录下的 test.py 下写入

```python
import hello
hello.hello()

```

会出现的结果是什么呢？

```
i am in bin dir

```

是的确实是这样的。但是为什么呢？
这个需要提及到环境变量的问题，我们可以看看当前文件所处的环境变量到底里面有什么？
使用下面的语句：

```
import sys, os
print('--------')
for path in sys.path:
    print(path)
print('--------')

```

其中sys.path是所有环境变量的所构成的列表，

```python
--------
D:\0 sty file\myCode\pythonFile\Day5\bin
D:\0 sty file\myCode\pythonFile
C:\ProgramData\Anaconda3\python36.zip
C:\ProgramData\Anaconda3\DLLs
C:\ProgramData\Anaconda3\lib
C:\ProgramData\Anaconda3
C:\ProgramData\Anaconda3\lib\site-packages
C:\ProgramData\Anaconda3\lib\site-packages\Babel-2.5.0-py3.6.egg
C:\ProgramData\Anaconda3\lib\site-packages\win32
C:\ProgramData\Anaconda3\lib\site-packages\win32\lib
C:\ProgramData\Anaconda3\lib\site-packages\Pythonwin
--------

```

导入模块和package时候，我们的程序会从这个sys.path中从前到后寻找这些目录下有没有我们要找的模块，可以看见D:\0 sty file\myCode\pythonFile\Day5\bin是在第一个的所有我们直接导入hello模块。python找到的D:\0 sty file\myCode\pythonFile\Day5\bin里面的hello.py找到之后，就不找了，然后打印出来了。

**导入的问题来了**

现在问题来了，我现在就想要导入`D:\0 sty file\myCode\pythonFile\Day5`下面的hello，那我应该怎么办？

我们知道在 python 中我们可以找到当前文件所在目录的父目录，然后将他加入到运行环境时候的环境变量中去，注意是运行环境中的，因为在当前python文件运行结束后，这个环境变量就释放了。


```
import sys, os    # 导入系统模块
print(os.path.abspath(__file__))  #打印当前文件所处的绝对路径
print(os.path.dirname(os.path.abspath(__file__))) #打印当前文件所处的上级目录
# 打印当前文件所处的上级目录的父目录
print( os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

# Outputs:
# D:\0 sty file\myCode\pythonFile\Day5\bin\test.py
# D:\0 sty file\myCode\pythonFile\Day5\bin
# D:\0 sty file\myCode\pythonFile\Day5 

```

然后我们就可以在写下如下的代码:

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.append(BASE_DIR)  # 将BASE_DIR加入到系统环境变量

```

我们将当前文件所在目录的父目录加入到环境变量中去，然后我们在打印下sys.path得到：

```
--------
D:\0 sty file\myCode\pythonFile\Day5\bin
D:\0 sty file\myCode\pythonFile
C:\ProgramData\Anaconda3\python36.zip
C:\ProgramData\Anaconda3\DLLs
C:\ProgramData\Anaconda3\lib
C:\ProgramData\Anaconda3
C:\ProgramData\Anaconda3\lib\site-packages
C:\ProgramData\Anaconda3\lib\site-packages\Babel-2.5.0-py3.6.egg
C:\ProgramData\Anaconda3\lib\site-packages\win32
C:\ProgramData\Anaconda3\lib\site-packages\win32\lib
C:\ProgramData\Anaconda3\lib\site-packages\Pythonwin
D:\0 sty file\myCode\pythonFile\Day5
--------

```


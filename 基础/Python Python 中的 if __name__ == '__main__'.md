# Python Python 中的 if name == 'main'

## 程序入口

对于很多编程语言来说，程序都必须要有一个入口，比如 C，C++，以及完全面向对象的编程语言 Java，C# 等。如果你接触过这些语言，对于程序入口这个概念应该很好理解，C 和 C++ 都需要有一个 main 函数来作为程序的入口，也就是程序的运行会从 main 函数开始。同样，Java 和 C# 必须要有一个包含 Main 方法的主类来作为程序入口。

而 Python 则有不同，它属于脚本语言，不像编译型语言那样先将程序编译成二进制再运行，而是动态的逐行解释运行。也就是从脚本第一行开始运行，没有统一的入口。

一个 Python 源码文件除了可以被直接运行外，还可以作为模块（也就是库）被导入。不管是导入还是直接运行，最顶层的代码都会被运行（Python 用缩进来区分代码层次）。而实际上在导入的时候，有一部分代码我们是不希望被运行的。

举一个例子来说明一下，假设我们有一个const.py文件，内容如下：

```python
PI = 3.14

def main():
    print ("PI:", PI)

mian()


```

我们在这个文件里边定义了一些常量，然后又写了一个 main 函数来输出定义的常量，最后运行 main 函数就相当于对定义做一遍人工检查，看看值设置的都对不对。然后我们直接执行该文件`(python const.py)`,输出：

```
PI: 3.14

```

现在，我们有一个`area.py 文件`，用于计算圆的面积，该文件里边需要用到 const.py 文件中的 PI 变量，那么我们从 const.py 中把 PI 变量导入到 area.py 中：

```python
from const import PI

def calc_round_area(radius):
    return PI * (radius ** 2)

def main():
    print ("round area: ", calc_round_area(2))

main()
#输出：
#PI: 3.14
#round area:  12.56

```

可以看到，**const 中的 main 函数也被运行了，实际上我们是不希望它被运行，提供 main 也只是为了对常量定义进行下测试**。这时，`if __name__ == '__main__'`就派上了用场。把 const.py 改一下：

```python
PI = 3.14

def main():
    print ("PI:", PI)

if __name__ == "__main__":
    main()

```

然后再运行 area.py，输出如下：

```
round area:  12.56

```

再运行下 const.py，输出如下：

```python
PI: 3.14
```

`if __name__ == '__main__'`就相当于是 Python 模拟的程序入口。Python 本身并没有规定这么写，这只是一种编码习惯。由于模块之间相互引用，不同模块可能都有这样的定义，而入口程序只能有一个。到底哪个入口程序被选中，这取决于 `__name__`的值。

# `__name__`

`__name__`是内置变量，用于表示当前模块的名字，同时还能反映一个包的结构。来举个例子，假设有如下一个包：

```
a
├── b
│   ├── c.py
│   └── __init__.py
└── __init__.py
```

目录中所有` .py` 文件的内容都为：

```python
print __name__
```

我们执行`python -c "import a.b.c"，`输出结果：

```python
a
a.b
a.b.c

```

# `__file__` 、`__doc__`的用法

`__file__`：当前文件路径

`__doc__` ： 当前文件描述

```python
#index.py
'''
Create on XXXX
@author:HHHH
'''

print(__file__)
print(__doc__)
#打印
F:/CodeFile/Python1/file2/index.py

Create on XXXX
@author:HHHH

```


# Python with...as的用法

`with…as`，就是个 python 控制流语句，像 `if` ，`while`一样。
`with…as` 语句是简化版的`try except finally`语句。

## `try…except`语句

用于处理程序执行过程中的异常情况，比如语法错误、从未定义变量上取值等等，也就是一些 python 程序本身引发的异常、报错。比如你在 python下面输入 1 / 0：

```python
>>> 1/0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero

```

说白了就是为了防止一些报错影响你的程序继续运行，就用try语句把它们抓出来(捕获)。

### **`try…except`的标准格式：**

```python
try:  
    ## normal block  
except A:  
    ## exc A block  
except:  
    ## exc other block  
else:  
    ## noError block  

```

程序执行流程:

> –>执行normal block
> –>发现有A错误，执行 exc A block(即处理异常)
> –>结束
> 如果没有A错误呢？
> –>执行normal block
> –>发现B错误，开始寻找匹配B的异常处理方法，发现A，跳过，发现except others(即except:)，执行exc other block
> –>结束
> 如果没有错误呢？
> –>执行normal block
> –>全程没有错误，跳入else 执行noError block
> –>结束
>

Tips: 我们发现，一旦跳入了某条except语句，就会执行相应的异常处理方法(block)，执行完毕就会结束。不会再返回try的normal block继续执行了。

```python
try:
    a = 1 / 2 #a normal number/variable
    print(a)
    b = 1 / 0 # an abnormal number/variable
    print(b)
    c = 2 / 1 # a normal number/variable
    print(c)
except:
    print("Error")

```

输出：

```
0.5
Error

```

结果是，先打出了一个0，又打出了一个Error。就是把ZeroDivisionError错误捕获了。

先执行try后面这一堆语句，由上至下：
step1: a 正常，打印a. 于是打印出0.5 (python3.x以后都输出浮点数)
step2: b, 不正常了，0 不能做除数，所以这是一个错误。直接跳到except报错去。于是打印了Error。
step3: 其实没有step3，因为程序结束了。c是在错误发生之后的b语句后才出现，根本轮不到执行它。也就看不到打印出的c了

但这还不是` try/except`的所有用法

except 后面还能跟表达式的! 所谓的表达式，就是错误的定义。也就是说，我们可以捕捉一些我们想要捕捉的异常。而不是什么异常都报出来。


**异常分为两类：**

- python标准异常
- 自定义异常

[看看except都能捕捉到哪些python标准异常](https://www.runoob.com/python/python-exceptions.html) 

```python
try:
    a = 1 / 2
    print(a)
    print(m)  # 此处抛出python标准异常
    b = 1 / 0 # 此后的语句不执行
    print(b)
    c = 2 / 1
    print(c)
except NameError:
    print("Ops!!")
except ZeroDivisionError:
    print("Wrong math!!")
except:
    print("Error")

```

## `try…finallly`语句

用于无论执行过程中有没有异常，都要执行清场工作。

好的 现在我们看看他俩合在一起怎么用!!

```python
try:  
    execution block  ##正常执行模块  
except A:  
    exc A block ##发生A错误时执行  
except B:  
    exc B block ##发生B错误时执行  
except:  
    other block ##发生除了A,B错误以外的其他错误时执行  
else:  
    if no exception, jump to here ##没有错误时执行  
finally:  
    final block  ##总是执行  

```

tips: 注意顺序不能乱，否则会有语法错误。如果用else就必须有except，否则会有语法错误。

```python
try:
    a = 1 / 2
    print(a)
    print(m) # 抛出NameError异常
    b = 1 / 0
    print(b)  
    c = 2 / 1
    print(c)
except NameError:
    print("Ops!!")  # 捕获到异常
except ZeroDivisionError:
    print("Wrong math!!")
except:
    print("Error")
else:
    print("No error! yeah!")
finally:      # 是否异常都执行该代码块
    print("Successfully!")

```

## `with…as`语句

with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭／线程中锁的自动获取和释放等。



with as 语句的结构如下：

```python
with expression [as variable]:  
    with-block  

```

### with 工作原理

（１）紧跟with后面的语句被求值后，返回对象的“–enter–()”方法被调用，这个方法的返回值将被赋值给as后面的变量；
（２）当with后面的代码块全部被执行完之后，将调用前面返回对象的“–exit–()”方法。
with工作原理代码示例：

```python
class Sample:
    def __enter__(self):
        print "in __enter__"
        return "Foo"
    def __exit__(self, exc_type, exc_val, exc_tb):
        print "in __exit__"
def get_sample():
    return Sample()
with get_sample() as sample:
    print "Sample: ", sample
```

代码的运行结果如下：

```python
in __enter__
Sample:  Foo
in __exit__
```

可以看到，整个运行过程如下：
（１）**enter**()方法被执行；
（２）**enter**()方法的返回值，在这个例子中是”Foo”，赋值给变量sample；
（３）执行代码块，打印sample变量的值为”Foo”；
（４）**exit**()方法被调用；

【注：】**exit**()方法中有３个参数， exc_type, exc_val, exc_tb，这些参数在异常处理中相当有用。
exc_type：　错误的类型
exc_val：　错误类型对应的值
exc_tb：　代码中错误发生的位置

示例代码：

```
class Sample():
    def __enter__(self):
        print('in enter')
        return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        print "type: ", exc_type
        print "val: ", exc_val
        print "tb: ", exc_tb
    def do_something(self):
        bar = 1 / 0
        return bar + 10
with Sample() as sample:
    sample.do_something()
```

程序输出结果：

```
in enter
Traceback (most recent call last):
type:  <type 'exceptions.ZeroDivisionError'>
val:  integer division or modulo by zero
  File "/home/user/cltdevelop/Code/TF_Practice_2017_06_06/with_test.py", line 36, in <module>
tb:  <traceback object at 0x7f9e13fc6050>
    sample.do_something()
  File "/home/user/cltdevelop/Code/TF_Practice_2017_06_06/with_test.py", line 32, in do_something
    bar = 1 / 0
ZeroDivisionError: integer division or modulo by zero

Process finished with exit code 1
```





实际上，在with后面的代码块抛出异常时，**exit**()方法被执行。开发库时，清理资源，关闭文件等操作，都可以放在**exit**()方法中。
总之，with-as表达式极大的简化了每次写finally的工作，这对代码的优雅性是有极大帮助的。
如果有多项，可以这样写：

```python
With open('1.txt') as f1, open('2.txt') as  f2:
    do something
```


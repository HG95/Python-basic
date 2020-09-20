# Python chr()和ord()函数

Python中经常会获得一些字符串，但是我们在对其进行计算的时候需要先将其转化为整型数。

## ord（）

ord（）函数就是用来返回单个字符的ascii值（0-255）或者unicode数值（）。

```python

>>> ord('d')
100
>>> ord('5')

53
```

## chr（）

chr（）函数是输入一个整数【0，255】返回其对应的ascii符号.

```python
>>> chr(100)
'd'
>>> chr(53)
'5'

```

```python
def formatStrToInt(target):
    for i in range(len(target)):
        temp=ord(target[i])
        print temp,
    return
formatStrToInt("abcdefghijk")
 
结果如下：
==============================================
97 98 99 100 101 102 103 104 105 106 107

```


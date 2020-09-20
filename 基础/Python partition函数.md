# Python partition函数

## 描述

Python `partition() `方法用来根据指定的分隔符将字符串进行分割。

如果字符串包含指定的分隔符，则返回一个3元的元组，第一个为分隔符前面的子字符串，第二个为分隔符本身，第三个为分隔符后面的子字符串。

## 语法

partition() 方法语法：

```
S.partition(sep)
```

### **参数**

- sep : 指定的分隔符。

### 返回值

返回一个3元的元组，第一个为分隔符前面的子字符串，第二个为分隔符本身，第三个为分隔符后面的子字符串。

## 实例

```python
S = "http://www.w3cschool.cc/"
 
print (S.partition("://"))
# ('http', '://', 'www.w3cschool.cc/')

print (S.partition("/"))
# ('http:', '/', '/www.w3cschool.cc/')
```


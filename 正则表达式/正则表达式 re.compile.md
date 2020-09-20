# python 正则表达式 re.compile()

﻿## 正则表达式re.compile()
对于一些经常要用到的正则表达式，可以使用 `compile` 进行编译，后期再使用的时候可以直接拿过来用，执行效率会更快。而且 `compile`还可以指定`flag=re.VERBOSE`，在写正则表达式的时候可以做好注释。
`compile()`的定义：

```python
compile(pattern, flags=0) 
Compile a regular expression pattern, returning a pattern object.
```
```python
text = "the number is 20.50"
r = re.compile(r"""
                \d+ # 小数点前面的数字
                \.? # 小数点
                \d* # 小数点后面的数字
                """,re.VERBOSE)
ret = re.search(r,text)
print(ret.group())
```

从`compile()`函数的定义中，可以看出返回的是一个匹配对象，它单独使用就没有任何意义，需要和`findall()`, `search()`,` match(）`搭配使用。

**`compile()`与`findall()`一起使用，返回一个列表。**

```python
import re


def main():
    content = 'Hello, I am Jerry, from Chongqing, a montain city, nice to meet you……'
    regex = re.compile('\w*o\w*')
    x = regex.findall(content)
    print(x)


if __name__ == '__main__':
    main()
# ['Hello', 'from', 'Chongqing', 'montain', 'to', 'you']
```
`compile()`与`match()`一起使用，可返回一个`class`、`str`、`tuple`。但是一定需要注意 `match()`，从位置 0 开始匹配，匹配不到会返回 None，返回 None 的时候就没有span/group属性了，并且与group使用，返回一个单词‘Hello’后匹配就会结束。

```python
import re


def main():
    content = 'Hello, I am Jerry, from Chongqing, a montain city, nice to meet you……'
    regex = re.compile('\w*o\w*')
    y = regex.match(content)
    print(y)
    print(type(y))
    print(y.group())
    print(y.span())


if __name__ == '__main__':
    main()
'''
<re.Match object; span=(0, 5), match='Hello'>
<class 're.Match'>
Hello
(0, 5)
'''
```
`compile()`与`search()`搭配使用, 返回的类型与`match()`差不多， 但是不同的是`search()`, 可以不从位置 0 开始匹配。但是匹配一个单词之后，匹配和 `match()`一样，匹配就会结束。

```python
import re


def main():
    content = 'Hello, I am Jerry, from Chongqing, a montain city, nice to meet you……'
    regex = re.compile('\w*o\w*')
    z = regex.search(content)
    print(z)
    print(type(z))
    print(z.group())
    print(z.span())


if __name__ == '__main__':
    main()
'''
<re.Match object; span=(0, 5), match='Hello'>
<class 're.Match'>
Hello
(0, 5)
'''
```
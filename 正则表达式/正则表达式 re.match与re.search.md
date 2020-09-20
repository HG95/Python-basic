# python 正则表达式 match()与search()

﻿## `re.match()`

```python
import re

def main():
    username = 'jerry_friend'
    m = re.match(r'\w{8,20}', username)
    print(m)
    print(m.span())  # span()，如果匹配值成功，则返回匹配成功的下标
    print(m.group())  # group（）， 匹配成功，返回匹配成功后的部分

if __name__ == '__main__':
    main()

'''
<re.Match object; span=(0, 12), match='jerry_friend'>
(0, 12)
jerry_friend
'''
```

**注意**：`re.match()`函数只检测 RE 是不是在 string 的开始位置匹配，**也就是说 match() 只有在0位置匹配成功的话才有返回， 如果不是开始位置匹配成功的话，match()就返回none**, 不能和span()、group()搭配使用，否则会报错。以下的写法是错误的。

```python
import re


def main():
    username =  '#jerry_friend'
    m = re.match(r'\w{8,20}', username)
    print(m)
    print(m.span())  # span()，如果匹配值成功，则返回匹配成功的下标
    print(m.group())  # group（）， 匹配成功，返回匹配成功后的部分


if __name__ == '__main__':
    main()
    
'''
None
AttributeError: 'NoneType' object has no attribute 'span'
'''
```
下面的程序也会报错，那是应为能匹配的只有‘jerry’，总共匹配了5次，然而正则表达式中要求匹配 8 到 20 次，所以匹配的结果返回的是None。

```python
import re


def main():
    username = 'jerry#friend'
    m = re.match(r'\w{8,20}', username)
    print(m)
    print(m.span())
    print(m.group())


if __name__ == '__main__':
    main()
'''
None
    print(m.span())
AttributeError: 'NoneType' object has no attribute 'span'
'''
```

## `re.search()`
注意：`search()`会扫描整个 string 查找匹配；`search()`可以不从 0 位置开始匹配，这就是和`match()`的区别。以上的‘username = #jerry_friend’，如果选择使用search()，那么是不会返回None的。

```python
if __name__ == '__main__':
    main()
'''
<re.Match object; span=(1, 13), match='jerry_friend'>
(1, 13)
jerry_friend
'''
```
**注意，如果 string中 存在多个 pattern 子串，只返回第一个。**


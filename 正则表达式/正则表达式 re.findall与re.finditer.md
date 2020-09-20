# python 正则表达式 re.findall()与re.finditer()

﻿## 正则表达式`re.findall()`与`re.finditer()`的区别
`re.findall()`如果可以匹配返回的是一个列表，`re.finditer()`返回的是一个迭代器，需要对其进行遍历，才能获取数据。

```python
def findall(pattern, string, flags=0):
    """Return a list of all non-overlapping matches in the string.

    If one or more capturing groups are present in the pattern, return
    a list of groups; this will be a list of tuples if the pattern
    has more than one group.

    Empty matches are included in the result."""
    return _compile(pattern, flags).findall(string)
```

```python
import re


def main():
    content = '八神是我的好朋友，他的手机电话是18381665314， 他的QQ是1911966573， 他女朋友的电话是18381665315, QQ:1911966574 ！'
    regex = re.compile(r'\d{11}')
    tels = regex.findall(content)
    print(tels)


if __name__ == '__main__':
    main()
# ['18381665314', '18381665315']
```
用以下的`finditer()`代码，输出的效果是等价的.

```python
import re


def main():
    content = '八神是我的好朋友，他的手机电话是18381665314， 他的QQ是1911966573， 他女朋友的电话是18381665315, QQ:1911966574 ！'
    regex = re.compile(r'\d{11}')
    tels_obj = regex.finditer(content)
    print(tels_obj)     #<callable_iterator object at 0x0000016C716066C8>
    tels_list = []
    for tel in tels_obj:
        tels_list.append(tel.group())
    print(tels_list)


if __name__ == '__main__':
    main()
# ['18381665314', '18381665315']
```
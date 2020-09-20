# python 正则表达式 re.sub()

## re.sub()替换功能
`re.sub()` 用于替换字符串中的匹配项。

`re.sub(pattern, repl, string[, count])`

使用 `repl` 替换 `string` 中每一个匹配的子串后返回替换后的字符串。

当`repl` 是一个字符串时，可以使用 `\id`或`\g`、`\g`引用分组，但不能使用编号 0。
当`repl`是一个方法时，这个方法应当只接受一个参数（Match对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。
`count`用于指定最多替换次数，不指定时全部替换。

`re.sub()`是个正则表达式方面的函数，用来实现通过正则表达式，实现比普通字符串的 replace 更加强大的替换功能。简单的替换功能可以使用`replace()`实现。

```python
def main():
    text = '123, word!'
    text1 = text.replace('123', 'Hello')
    print(text1)


if __name__ == '__main__':
    main()
# Hello, wold!
```
如果通过`re.sub()`函数则可以匹配任意的数字，并将其替换：

```python
import re


def main():
    content = 'abc124hello46goodbye67shit'
    list1 = re.findall(r'\d+', content)
    print(list1)
    mylist = list(map(int, list1))
    print(mylist)
    print(sum(mylist))
    print(re.sub(r'\d+[hg]', 'foo1', content))
    print()
    print(re.sub(r'\d+', '456654', content))


if __name__ == '__main__':
    main()
# ['124', '46', '67']
# [124, 46, 67]
# 237
# abcfoo1ellofoo1oodbye67shit

# abc456654hello456654goodbye456654shit
```
## split()分割方法
使用正则表达式来分割字符串。

```python
text = "hello world ni hao"
ret = re.split('\W',text)
print(ret)
>> ["hello","world","ni","hao"]
```

```python
>>> import re
>>> formula = 'YOU == ME**2'
>>> re.split('[A-Z]+', formula)
['', ' == ', '**2']
```
这里，`[A-Z]+`中的加号`+`表示，至少1次。`[A-Z]+`则表示，至少出现 1 个大写字母。

`re.split('[A-Z]+', formula)`的含义是，将 formula 字符串分解。分解的规则是，将 formula 字符串中的1个及以上字母去掉，返回剩余字符的1个列表。
# python 正则表达式 flags 参数

## flags参数

```python
re.I
    IGNORECASE
    忽略字母大小写

re.L
    LOCALE
    影响 “w, “W, “b, 和 “B，这取决于当前的本地化设置。

re.M
    MULTILINE
    使用本标志后，‘^’和‘$’匹配行首和行尾时，会增加换行符之前和之后的位置。

re.S
    DOTALL
    使 “.” 特殊字符完全匹配任何字符，包括换行；没有这个标志， “.” 匹配除了换行符外的任何字符。

re.X
    VERBOSE
    当该标志被指定时，在 RE 字符串中的空白符被忽略，除非该空白符在字符类中或在反斜杠之后。
    它也可以允许你将注释写入 RE，这些注释会被引擎忽略；
    注释用 “#”号 来标识，不过该符号不能在字符串或反斜杠之后。

```
## 忽略大小写

```python
import re
text = '我爱Python我爱python'
pat1 = 'p'
# search
r1 = re.findall(pattern=pat1, string=text, flags=re.I)
print(r1)
#[‘P’, ‘p’]
```
## 多行模式

```python
import re
text = '我爱数学\n我爱Python\n我爱python'
pat1 = '^我'
# search
r1 = re.findall(pattern=pat1, string=text)
r2 = re.findall(pattern=pat1, string=text, flags=re.M)
print(r1)
print(r2)
#[‘我’]
[‘我’, ‘我’, ‘我’]
```
## 匹配任何字符

```python
import re
text = '''
我爱Python
我爱pandas
'''
pat1 = '.我'
# search
r1 = re.findall(pattern=pat1, string=text, flags=re.S)
print(r1)
r2 = re.findall(pattern=pat1, string=text)
print(r2)
#[’\n我’, ‘\n我’]
#[]
```
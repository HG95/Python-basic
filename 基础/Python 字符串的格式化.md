# Python 字符串的格式化

## `format()`

`str.format()` 函数，相比于 % 操作符，format函数使用{}和:代替了%，威力更加强大，在映射关系方面，format函数支持位置映射、关键字映射、对象属性映射、下标映射等多种方式，不仅参数可以不按顺序，也可以不用参数或者一个参数使用多次，下面通过几个例子来说明。

```python
'{1} {0}'.format('abc', 123)  # 可以不按顺序进行位置映射，输出'123 abc'

'{} {}'.format('abc', 123)  # 可以不指定参数名称，输出'abc 123'

'{1} {0} {1}'.format('abc', 123)  # 参数可以使用多次，输出'123 abc 123'

'{name} {age}'.format(name='tom', age=27)  # 可以按关键字映射，输出'tom 27'

'{person.name} {person.age}'.format(person=person)  # 可以按对象属性映射，输出'tom 27'

'{0[1]} {0[0]}'.format(lst)  # 通过下标映射

```

可以看到，format函数比%操作符使用起来更加方便，不需要记住太多各种占位符代表的意义，代码可读性也更高。在复杂格式控制方面，format函数也提供了更加强大的控制方式：

<a href="https://blog.csdn.net/HHG20171226/article/details/101436292" blank="">参考</a> 

## `f-string`

Python在3.6版本中也为我们带来了类似的功能：Formatted String Literals（字面量格式化字符串），简称f-string。

f-string就是以f’‘开头的字符串，类似u’‘和b’’，字符串内容和format方法中的格式一样，但是可以直接将变量带入到字符串中，可读性进一步增加，例如：

```python
amount = 1234
f'请转账给我{amount:,.2f}元'  # '请转账给我1,234.00元'

```

同时，f-string的性能是比%和format都有提升的，我们做一个简单的测试，分别使用%操作符、format和f-string将下面语句执行10000次：

```python
'My name is %s and i'm %s years old.' % (name, age)
'My name is {} and i'm {} years old.'.format(name, age)
f'My name is {name} and i'm {age} years old.'

```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503210952.png"/>

### 简单使用

f-string用大括号 `{}` 表示被替换字段，其中直接填入替换内容：

```python
>>> name = 'Eric'
>>> f'Hello, my name is {name}'
'Hello, my name is Eric'

>>> number = 7
>>> f'My lucky number is {number}'
'My lucky number is 7'

>>> price = 19.99
>>> f'The price of this book is {price}'
'The price of this book is 19.99'

```

### 表达式求值与函数调用

f-string的大括号 `{}` 可以填入表达式或调用函数，Python会求出其结果并填入返回的字符串内：

```python
>>> f'A total number of {24 * 8 + 4}'
'A total number of 196'

>>> f'Complex number {(2 + 2j) / (2 - 3j)}'
'Complex number (-0.15384615384615388+0.7692307692307692j)'

>>> name = 'ERIC'
>>> f'My name is {name.lower()}'
'My name is eric'

>>> import math
>>> f'The answer is {math.log(math.pi)}'
'The answer is 1.1447298858494002'

```

### 引号、大括号与反斜杠

f-string大括号内所用的引号不能和大括号外的引号定界符冲突，可根据情况灵活切换 `'` 和 `"`：

```python
>>> f'I am {"Eric"}'
'I am Eric'
>>> f'I am {'Eric'}'
  File "<stdin>", line 1
    f'I am {'Eric'}'
                ^
SyntaxError: invalid syntax

```

若 `'` 和 `"` 不足以满足要求，还可以使用 `'''` 和 `"""`：

```python
>>> f"He said {"I'm Eric"}"
  File "<stdin>", line 1
    f"He said {"I'm Eric"}"
                ^
SyntaxError: invalid syntax

>>> f'He said {"I'm Eric"}'
  File "<stdin>", line 1
    f'He said {"I'm Eric"}'
                  ^
SyntaxError: invalid syntax

>>> f"""He said {"I'm Eric"}"""
"He said I'm Eric"
>>> f'''He said {"I'm Eric"}'''
"He said I'm Eric"

```

大括号外的引号还可以使用 `\` 转义，但大括号内不能使用 `\` 转义：

```python
>>> f'''He\'ll say {"I'm Eric"}'''
"He'll say I'm Eric"
>>> f'''He'll say {"I\'m Eric"}'''
  File "<stdin>", line 1
SyntaxError: f-string expression part cannot include a backslash

```

f-string大括号外如果需要显示大括号，则应输入连续两个大括号 `{{` 和 `}}`：

```python
>>> f'5 {"{stars}"}'
'5 {stars}'
>>> f'{{5}} {"stars"}'
'{5} stars'

```

### 自定义格式：对齐、宽度、符号、补零、精度、进制等

f-string采用 `{content:format}` 设置字符串格式，其中 `content` 是替换并填入字符串的内容，可以是变量、表达式或函数等，`format` 是格式描述符。采用默认格式时不必指定 `{:format}`，如上面例子所示只写 `{content}` 即可。

#### **对齐**相关格式描述符

| 格式描述符 |          含义与作用          |
| :--------: | :--------------------------: |
|    `<`     | 左对齐（字符串默认对齐方式） |
|    `>`     |  右对齐（数值默认对齐方式）  |
|    `^`     |             居中             |

#### **数字符号**相关格式描述符

| 格式描述符 |                   含义与作用                    |
| :--------: | :---------------------------------------------: |
|    `+`     |    负数前加负号（`-`），正数前加正号（`+`）     |
|    `-`     | 负数前加负号（`-`），正数前不加任何符号（默认） |
|  （空格）  |      负数前加负号（`-`），正数前加一个空格      |

注：仅适用于数值类型。

#### **数字显示方式**相关格式描述符

| 格式描述符 |    含义与作用    |
| :--------: | :--------------: |
|    `#`     | 切换数字显示方式 |

注1：仅适用于数值类型。
注2：`#` 对不同数值类型的作用效果不同，详见下表：

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503212025.png"/>

#### **宽度与精度**相关格式描述符

|    格式描述符     |                        含义与作用                         |
| :---------------: | :-------------------------------------------------------: |
|      `width`      |                   整数 `width` 指定宽度                   |
|     `0width`      | 整数 `width` 指定宽度，开头的 `0` 指定高位用 `0` 补足宽度 |
| `width.precision` |   整数 `width` 指定宽度，整数 `precision` 指定显示精度    |

注1：0width 不可用于复数类型和非数值类型，width.precision 不可用于整数类型。
注2：width.precision 用于不同格式类型的浮点数、复数时的含义也不同：用于 f、F、e、E 和 % 时 precision 指定的是小数点后的位数，用于 g 和 G 时 precision 指定的是有效数字位数（小数点前位数+小数点后位数）。
注3：width.precision 除浮点数、复数外还可用于字符串，此时 precision 含义是只使用字符串中前 precision 位字符。

```python
>>> a = 123.456
>>> f'a is {a:8.2f}'
'a is   123.46'
>>> f'a is {a:08.2f}'
'a is 00123.46'
>>> f'a is {a:8.2e}'
'a is 1.23e+02'
>>> f'a is {a:8.2%}'
'a is 12345.60%'
>>> f'a is {a:8.2g}'
'a is  1.2e+02'

>>> s = 'hello'
>>> f's is {s:8s}'
's is hello   '
>>> f's is {s:8.3s}'
's is hel     '

```

#### **千位分隔符**相关格式描述符

| 格式描述符 |      含义与作用       |
| :--------: | :-------------------: |
|    `,`     | 使用`,`作为千位分隔符 |
|    `_`     | 使用`_`作为千位分隔符 |

注1：若不指定 , 或 _，则f-string不使用任何千位分隔符，此为默认设置。
注2：, 仅适用于浮点数、复数与十进制整数：对于浮点数和复数，, 只分隔小数点前的数位。
注3：_ 适用于浮点数、复数与二、八、十、十六进制整数：对于浮点数和复数，_ 只分隔小数点前的数位；对于二、八、十六进制整数，固定从低位到高位每隔四位插入一个 _（十进制整数是每隔三位插入一个 _）。

```python
>>> a = 1234567890.098765
>>> f'a is {a:f}'
'a is 1234567890.098765'
>>> f'a is {a:,f}'
'a is 1,234,567,890.098765'
>>> f'a is {a:_f}'
'a is 1_234_567_890.098765'

>>> b = 1234567890
>>> f'b is {b:_b}'
'b is 100_1001_1001_0110_0000_0010_1101_0010'
>>> f'b is {b:_o}'
'b is 111_4540_1322'
>>> f'b is {b:_d}'
'b is 1_234_567_890'
>>> f'b is {b:_x}'
'b is 4996_02d2'

```

........

参考

<a href="https://blog.csdn.net/sunxb10/article/details/81036693" blank="">Python格式化字符串f-string</a> 


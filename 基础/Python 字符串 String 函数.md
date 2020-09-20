# Python 字符串 String 函数

## `find() 函数`

```python
str.find(str, beg=0, end=len(string))
	参数
		str -- 指定检索的字符串
		beg -- 开始索引，默认为0。
		end -- 结束索引，默认为字符串的长度。
	返回值
		如果包含子字符串返回开始的索引值，否则返回-1。

```

`find()` 方法检测字符串中是否包含`子字符串 str` ，如果指定 beg（开始） 和 end（结束） 范围，则检查是否包含在指定范围内，如果包含子字符串返回开始的索引值，否则返回-1。

```python
str1 = "this is string example....wow!!!";
str2 = "exam";
 
print(str1.find(str2))
print(str1.find(str2, 10))
print(str1.find(str2, 40))

```

## `count() 函数`

```python
count(str, start=0, end=len(string))

功能
	返回该字符串中出现某字符串序列（或字符）的次数
用法
	str.count(sub, start=0, end=len(string))
参数
	sub: 被查找的字符串序列
	start: 开始查找的索引位置，默认为字符串开始
	end: 结束查找的索引位置，默认为字符串结束
返回值
	被查找的序列在字符串的查找位置中出现的次数

```

```python
str = "hello world! hello world!"

sub = "o"
print("str.count(sub): ", str.count(sub))
#4
sub = "hello"
print("str.count(sub, 5) ", str.count(sub, 5))
#1

```

## `index() 函数`

```
index(str, start=0, end=len(string)) 

功能
	功能上与 find() 相同，只是在未找到子字符串是抛出异常
用法
	str.index(str, start=0, end=len(string))
参数
	同 find()
返回值
	如果查找到，返回该子字符串的索引；未查找到，抛出异常

```

```python
str = "hello world!"

str1 = "wo"

print(str.index(str1))      #6
print(str.index(str1, 8))
# Traceback (most recent call last):
#   File "teststrmethods.py", line 6, in <module>
#     print str.index(str1, 8)
# ValueError: substring not found

```

## `join() 函数`

```python
join(seq) 函数

功能
	用该字符串连接某字符序列(seq)
用法
	str.join(sequence)
参数
	sequence: 被连接的字符序列
返回值
	返回连接之后的字符串

str = "-"
sequence = ("hello", "world", "everyone", "!")

print(str.join(sequence))
#hello-world-everyone-!



```

## `ljust函数`

```python
ljust(width, fillchar=’ ‘)函数

功能
	在字符串的右边填充字符使得字符串达到指定长度
用法
	str.ljust(width, fillchar=' ')
参数
	width: 填充后的目标长度
	fillchar: 用于填充的字符，默认为空格
返回值
	返回填充后的字符串

print(str.ljust(15))
print(str.ljust(15,'!'))
# Hello world    
# Hello world!!!!



```

## `center() 函数`

```
center(width, fillchar) 函数

将字符串居中，居中后的长度为 width
功能
	将字符串居中，居中后的长度为 width
用法
	str.center(width[, fillchar])
参数
	width: 表示字符串总长度
	fillchar: 使字符串居中所填充的字符，默认为空格
返回值
	返回填充字符后的字符串

str = "hello world!"

print("str.center(20): ", str.center(20))
print("str.center(20,'-'): ", str.center(20,'-'))
# str.center(20):      hello world!
# str.center(20,'-'):  ----hello world!----


```





## `isalnum() 函数`

```python
功能
	判断该字符串是否只是字母数字组合
用法
	str.isalnum()
参数
	无
返回值
	如果该字符串是字母数字组合，返回 True,否则返回 False

str = "helloworld"
print(str.isalnum())    #True

str = "hello world"
print(str.isalnum())    #False

str = "hello123"
print(str.isalnum())    #True

str = "hello123!"
print(str.isalnum())    #False


```

## `isalpha() 函数`

```python
功能
	判断该字符串是否是字母组合
用法
	str.isalpha()
参数
	无
返回值
	如果该字符串是字母组合，返回 True,否则返回 False
	

str = "helloworld"
print(str.isalpha())    #True

str = "hello world"
print(str.isalpha())    #False

str = "hello123"
print(str.isalpha())    #False

str = "hello123!"
print(str.isalpha())    #False


```

## `isdigit() 函数`

```python
功能
	判断该字符串是否只包含数字
用法
	str.isdigit()
参数
	无
返回值
	如果该字符串只包含数字，则返回 True,否则返回 False

str = "hello123"
print(str.isdigit())    #False

str = "123456"
print(str.isdigit())    #True


```

## `replace() 函数`

```python
功能
	将字符串中的子字符串用某字符串来代替
用法
	str.replace(old, new[, max])
参数
	old: 被替换的子字符串
	new: 替换后的字符串
	max: 需要替换的个数
返回值
	返回替换后的字符串

str = "this is a string, this is a string"

print(str.replace("is", "was"))
print(str.replace("is", "was", 2))
# thwas was a string, thwas was a string
# thwas was a string, this is a string


```

## `split() 函数`

```python
功能
	分割字符串
用法
	str.split(str=" ", num=string.cout(str))
参数
	str: 分隔符，默认是空格
	num: 分割的次数，默认为按照分隔符分割整个字符串
返回值
	返回分割后的 list


str = "word1 word2 word3 word4"

print(str.split())
print(str.split('r'))
print(str.split(' ', 2))
# ['word1', 'word2', 'word3', 'word4']
# ['wo', 'd1 wo', 'd2 wo', 'd3 wo', 'd4']
# ['word1', 'word2', 'word3 word4']

```

## `splitlines() 函数`

```python
功能
	将字符串按行分割
用法
	str.splitlines(num=string.count('\n'))
参数
	num: 该数值如果不为0，表示分割后的字符串中保留\n
返回值
	返回分割后的 list

str = "line1\nline2\nline3\nline4"

print(str.splitlines())
print(str.splitlines(0))
print(str.splitlines(3))
# ['line1', 'line2', 'line3', 'line4']
# ['line1', 'line2', 'line3', 'line4']
# ['line1\n', 'line2\n', 'line3\n', 'line4']

```



## `lstrip() 函数`

## `strip() 函数`

## `startswith() 函数`

```python
功能
	判断字符串是否是以某子字符串开头
用法
	str.stratswith(str, start=0, end=len(str))
参数
	str: 被检查的子字符串
	start: 检查的字符串的起始 index，默认为 str 的开始位置
	end: 检查的字符串的结束 index，默认为 str 的终止位置
返回值
	如果字符串是否是以某子字符串开头，返回True；否则返回False

str = "hello world!"

print(str.startswith('hel'))        #True
print(str.startswith('hel',2,8))    #False

```

## `endswith() 函数`

```python
endswith(suffix, start=0, end=len(string)) 

判断字符串是否是以某字符串结尾的

用法
	str.endswith(suffix, start=0, end=len(string))
参数
	suffix: 被查找的字符串
	start: 字符串查找的起始位置，默认为字符串起始位置
	end: 字符串查找的结束位置，默认为字符串结束位置
返回值
	如果字符串是以 suffix 结尾的返回 True, 否则返回 False

str = "hello world!"

suffix = "world!"
print(str.endswith(suffix))     #True

suffix = "llo"
print(str.endswith(suffix,0,4)) #False
print(str.endswith(suffix,0,5)) #True


```



## `istitle() 函数`

```python
功能
	检查该字符串中的`单词`是否首字母都大写
用法
	str.istitle()
参数
	无
返回值
	如果该字符串中的单词首字母都大写了，返回True,否则返回False

str = "Hello world!"
print(str.istitle())    #False

str = "Hello Wolrd!"
print(str.istitle())    #True

```



## `isupper() 函数`

```
功能
	判断该字符串中的字母是否都是大写
用法
	str.isupper()
参数
	无
返回值
	如果该字符串中的字母都是大写，返回True,否则返回False
str = "Hello world!"
print(str.isupper())    #False

str = "HELLO WORLD!"
print(str.isupper())    #True


```



## `islower() 函数`

```python
功能
	判断该字符串中是否只是小写字母
用法
	str.islower()
参数
	无
返回值
	如果该字符串中只是小写字母，返回True,否则返回False

str = "hello wolrd!"
print(str.islower())    #True

str = "Hello Wolrd!"
print(str.islower())    #False


```

## `captalize() 函数`

```python
功能
	将一个字符串的第一个字母大写
用法
	str.captalize()
参数
	无
返回值
	string
str = "hello world!"

print("str.capitalize(): ", str.capitalize())
#str.capitalize():  Hello world!


```

## `swapcase() 函数`

```python
功能
	将字符串中的大小写字母转换
用法
	str.swapcase()
参数
	无
返回值
	返回转换后的字符串

str = "Hello World!"

print(str.swapcase())   #hELLO wORLD!


```

## `upper() 函数`

```python
功能
	将字符串中的字母都转换成大写
用法
	str.upper()
参数
	无
返回值
	返回转换后的字符串## 标题

str = "Hello World!"

print(str.upper())  #HELLO WORLD!

```

## `lower() 函数`

```python
功能
	将字符串中的字母转换为小写
用法
	str.lower()
参数
	无
返回值
	字符串

#示例代码
str = "HELLO WORLD!"
print (str.lower())
#hello world!

```


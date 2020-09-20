# Python JSON模块

程序都要求用户输入某种信息，如让用户存储游戏首选项或提供要可视化的数据。不管专注的是什么，程序都把用户提供的信息存储在列表和字典等数据结构中。用户关闭程序时，你几乎总是要保存他们提供的信息；一种简单的方式是使用模块 json 来存储数据。
模块 json 让你能够将简单的Python数据结构转储到文件中，并在程序再次运行时加载该文件中的数据。还可以使用 json 在Python程序之间分享数据。 更重要的是， JSON 数据格式并非Python专用的，这让你能够将以 JSON 格式存储的数据与使用其他编程语言的人分享。

> JSON（ JavaScript Object Notation）格式最初是为JavaScript开发的，但随后成了一种常见 格式，被包括Python在内的众多语言采用。

Json 模块提供了四个功能：`dumps`、`dump`、`loads`、`load`

`json.dump()` 和 `json.load()` 来编码和解码JSON数据。

编写一个存储一组数字的简短程序，再编写一个将这些数字读取到内存中的程序。第一个程序将使用`json.dump()`来存储这组数字，而第二个程序将使用`json.load()`。
`函数json.dump()`接受两个实参：要存储的数据以及可用于存储数据的文件对象。

```python
import json
numbers = [2, 3, 5, 7, 11, 13]
filename = 'numbers.json'
with open(filename, 'w') as f_obj:
	json.dump(numbers, f_obj)

```

## dumps

实现python类型转化为json字符串，返回一个str对象。把一个Python对象编码转换成Json字符串，从python原始类型向json类型转化对照表如下：

![image-20200813221505904](C:\Users\Hu\AppData\Roaming\Typora\typora-user-images\image-20200813221505904.png)

```python
import json

test_dict = {'bigberg': [7600, {1: [['iPhone', 6300], ['Bike', 800], ['shirt', 300]]}]}
print(test_dict)
print(type(test_dict))
#dumps 将数据转换成字符串
json_str = json.dumps(test_dict)
print(json_str)
print(type(json_str))
```

```
{'bigberg': [7600, {1: [['iPhone', 6300], ['Bike', 800], ['shirt', 300]]}]}
<class 'dict'>
{"bigberg": [7600, {"1": [["iPhone", 6300], ["Bike", 800], ["shirt", 300]]}]}
<class 'str'>
```

## loads

把 json 格式字符串解码转换成Python对象,  从json到Python的类型转化对照如下：

![image-20200813221602675](C:\Users\Hu\AppData\Roaming\Typora\typora-user-images\image-20200813221602675.png)

```python
new_dict = json.loads(json_str)
print(new_dict)
print(type(new_dict))
```

```
{'bigberg': [7600, {'1': [['iPhone', 6300], ['Bike', 800], ['shirt', 300]]}]}
<class 'dict'>
```

## dump

将Python内置类型序列化为 json 对象后写入文件

```python
with open("record.json","w") as f:
    json.dump(new_dict,f)
    print("加载入文件完成...")
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504115110.png"/>



## load

读取文件中 json 形式的字符串元素转化成 python 类型

```python
with open("record.json",'r') as load_f:
    load_dict = json.load(load_f)
    print(load_dict)
load_dict['smallberg'] = [8200,{1:[['Python',81],['shirt',300]]}]
print(load_dict)

with open("record.json","w") as dump_f:
    json.dump(load_dict,dump_f)
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200504115302.png"/>

## 重构

常会遇到这样的情况：代码能够正确地运行，但可做进一步的改进——将代码划分为一系列完成具体工作的函数。这样的过程被称为`重构`。重构让代码更清晰、更易于理解、更容易扩展。

```python
#remember_me.py
import json
# 如果以前存储了用户名，就加载它
# 否则，就提示用户输入用户名并存储它
filename = 'username.json'
try:
	with open(filename) as f_obj:
	username = json.load(f_obj)
except FileNotFoundError:
	username = input("What is your name? ")
	with open(filename, 'w') as f_obj:
		json.dump(username, f_obj)
		print("We'll remember you when you come back, " + username + "!")
else:
	print("Welcome back, " + username + "!")

```

函数greet_user()所做的不仅仅是问候用户，还在存储了用户名时获取它，而在没有存储用户名时提示用户输入一个。
下面来重构greet_user()，让它不执行这么多任务。为此，首先将获取存储的用户名的代码移到另一个函数中：

```python
import json

def get_stored_username():
	"""如果存储了用户名，就获取它"""
	filename = 'username.json'
	try:
		with open(filename) as f_obj:
			username = json.load(f_obj)
	except FileNotFoundError:
		return None
	else:
		return username


def greet_user():
	"""问候用户，并指出其名字"""
	username = get_stored_username()
	if username:
		print("Welcome back, " + username + "!")
	else:
		username = input("What is your name? ")
		filename = 'username.json'
		with open(filename, 'w') as f_obj:
			json.dump(username, f_obj)
			print("We'll remember you when you come back, " + username + "!")

greet_user()

```

还需将greet_user()中的另一个代码块提取出来：将没有存储用户名时提示用户输入的代码放在一个独立的函数中：

```python
import json

def get_stored_username():
	"""如果存储了用户名，就获取它"""
	filename = 'username.json'
	try:
		with open(filename) as f_obj:
			username = json.load(f_obj)
	except FileNotFoundError:
		return None
	else:
		return username

def get_new_username():
	"""提示用户输入用户名"""
	username = input("What is your name? ")
	filename = 'username.json'
	with open(filename, 'w') as f_obj:
		json.dump(username, f_obj)
	return username

def greet_user():
	"""问候用户，并指出其名字"""
	username = get_stored_username()
	if username:
		print("Welcome back, " + username + "!")
	else:
		username = get_new_username()
		print("We'll remember you when you come back, " + username + "!")

greet_user()

```









- 
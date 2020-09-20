# Python enumerate()函数

`enumerate()` 是python的内置函数
`enumerate` 在字典上是枚举、列举的意思
`enumerate(sequence, start=0)` ，返回一个枚举对象。sequence 必须是序列或迭代器 iterator，或者支持迭代的对象。
`enumerate()` 返回对象的每个元素都是一个元组，每个元组包括两个值，一个是计数，一个是 sequence 的值，计数是从start开始的，start默认为0。

```python
a=["q","w","e","r"]
c=enumerate(a)
for i in c:
    print(i)

输出：
(0, 'q')
(1, 'w')
(2, 'e')
(3, 'r')

```

对于一个可迭代的（iterable）/可遍历的对象（如列表、字符串），enumerate将其组成一个索引序列，利用它可以同时获得索引和值
enumerate多用于在for循环中得到计数

# `enumerate()`使用

- 如果对一个列表，既要遍历索引又要遍历元素时

```python
list1 = ["这", "是", "一个", "测试"]
for index, item in enumerate(list1):
    print( index, item)
>>>
0 这
1 是
2 一个
3 测试

```

- **enumerate还可以接收第二个参数，用于指定索引起始值，如：**

```python
list1 = ["这", "是", "一个", "测试"]
for index, item in enumerate(list1, 1):
    print index, item
>>>
1 这
2 是
3 一个
4 测试

```

统计文件的行数

```python
count = 0
for index, line in enumerate(open(filepath,'r'))： 
    count += 1

```


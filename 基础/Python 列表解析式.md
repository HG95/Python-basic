# Python 列表解析式

## 什么是列表解析式？

列表解析式是将一个列表（实际上适用于任何可迭代对象（iterable））转换成另一个列表的工具。在转换过程中，可以指定元素必须符合一定的条件，才能添加至新的列表中，这样每个元素都可以按需要进行转换。
如果你熟悉函数式编程（functional programming），你可以把列表解析式看作为结合了`filter`函数与`map`函数功能的语法糖：

```python
>>> doubled_odds = map(lambda n: n * 2, filter(lambda n: n % 2 == 1, numbers))

>>> doubled_odds = [n * 2 for n in numbers if n % 2 == 1]

```

## 从循环到解析式

每个列表解析式都可以重写为for循环，但不是每个for循环都能重写为列表解析式。掌握列表解析式使用时机的关键，在于不断练习识别那些看上去像列表解析式的问题。

```python
new_things = []
for ITEM in old_things:    
    if condition_based_on(ITEM):
        new_things.append("something with " + ITEM)


# 列表解析式格式
new_things = ["something with " + ITEM for ITEM in old_things 
              if condition_based_on(ITEM)]

```

## 嵌套循环

那么嵌套循环（nested loop）又该怎样改写为列表解析式呢？
下面是一个拉平（flatten）矩阵（以列表为元素的列表）的for循环：

```python
flattened = []
for row in matrix:
    for n in row:
        flattened.append(n)

```

下面这个列表解析式实现了相同的功能：

```python
flattened = [n for row in matrix for n in row]

```

如果要在列表解析式中处理嵌套循环，请记住for循环子句的顺序与我们原来for循环的顺序是一致的。

同样地原则也适用集合解析式（set comprehension）和字典解析式（dictionary comprehension）。

下面的代码将原有字典的键和值互换，从而创建了一个新的字典：

```python
flipped = {}
for key, value in original.items():
    flipped[value] = key

```

同样的代码可以改写为字典解析式：

```python
flipped = {value: key for key, value in original.items()}

```

## **可读性**

你有没有发现上面的列表解析式读起来很困难？我经常发现，如果较长的列表解析式写成一行代码，那么阅读起来就非常困难。

不过，还好Python支持在括号和花括号之间断行。

列表解析式 List comprehension
断行前：

```python
doubled_odds = [n * 2 for n in numbers if n % 2 == 1]

```

断行后：

```python
doubled_odds = [
    n * 2
    for n in numbers
    if n % 2 == 1]

]

```


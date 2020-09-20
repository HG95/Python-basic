# Python set集合的并集union, 交集intersection, 差集difference

python的集合set和其他语言类似，是一个无序不重复元素集, 可用于消除重复元素。

- 支持union(联合), intersection(交), difference(差)和sysmmetric difference(对称差集)等数学运算。
- 不支持 indexing, slicing, 或其它类序列（sequence-like）的操作。因为，sets作为一个无序的集合，sets不记录元素位置或者插入点。
  

## 并集

```python
>>> a=[1,3,5]
>>> b=[1,2,3]
>>> set(a) | set(b)
set([1, 2, 3, 5])

# 或者
>>> set(a).union(b)
set([1, 2, 3, 5])
```

## 交集

```python
>>> a=[1,3,5]
>>> b=[1,2,3]
>>> set(a) & set(b)
set([1, 3])
>>>

# 或者
>>> set(a).intersection(b)
set([1, 3])
>>>
```

## 差集

```python
>>> a=[1,3,5]
>>> b=[1,2,3]
>>> set(a) - set(b)
set([5])

# 或者
>>> set(a).difference(b)
set([5])
>>>
```

## 对称差集

返回两个集合中不重复的元素

```python
>>> a=[1,3,5]
>>> b=[1,2,3]
>>> set(a) ^ set(b)
set([2, 5])

# 或者
>>> set(a).symmetric_difference(b)
set([2, 5])
>>>
```


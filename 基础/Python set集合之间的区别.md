# Python set(集合) & 与 and 、 | 与 or之间的区别

"集合的交" --- (a & b)

"集合的并" --- (a | b)

"集合的差" --- a & (~b)

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200506103959.png"/>




```python
a = set([1, 2, 3, 4, 5])
b = set([4, 5, 6, 7, 8])
#求两个集合的交集
print(a & b)
# {4, 5}

print(a and b)
# {4, 5, 6, 7, 8}


#求两个集合的并集
print(a | b)
#{1, 2, 3, 4, 5, 6, 7, 8}

print(a or b)
# {1, 2, 3, 4, 5}
```

主要的原因是 & ！= and , | != or

python 中 & 、| 代表的是位运算符， and 、or代表的是逻辑运算符



① 当 a and b的结果为True 的时候，返回的并不是True，而是 运算结果的最后一位变量的值。这里是 返回b的值

（b and a 为真 ，返回的是 a 的值）,

当a and b结果为False 的时候，返回的是第一个False 的值，如 a 和 b都为False 那么返回 a的 值

，a 为 真， b 为假，那么返回的是 b的值



②当 a or b 为真的时候，返回的是第一个真的变量的值，如，当a 和 b都为真，那么返回的是 a

若 a or b 假的时候，返回的是最后一个判断条件的值，这里返回的是 b 的值



所以上面的代码 a and b返回的是 b的值  {4, 5, 6, 7, 8}

a or b 返回的则是 a 的值 {1, 2, 3, 4, 5, 6, 7, 8}

------

题目：

给定两个数组，编写一个函数来计算它们的**交集**。

示例 1:

输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2]
示例 2:

输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [9,4]
说明:

输出结果中的每个元素一定是唯一的。
我们可以不考虑输出结果的顺序。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1=set(nums1)
        nums2=set(nums2)
        return list(nums1&nums2)
```




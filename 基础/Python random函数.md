# Python random函数

该模块实现了各种分布的伪随机数生成器。



对于整数，从范围中有统一的选择。 对于序列，存在随机元素的统一选择、用于生成列表的随机排列的函数、以及用于随机抽样而无需替换的函数。

## `.random()`

`random.random()`
返回 [0.0, 1.0) 范围内的下一个随机浮点数。

## `.uniform()`

返回一个随机浮点数 N ，当 a <= b 时 a <= N <= b ，当 b < a 时 b <= N <= a 

```
m1=random.uniform(2.5, 10.0)                   # Random float:  2.5 <= x < 10.0
print(m1)

```



## `.randrange()`

```python
#random.randrange(stop)
m=random.randrange(10)   # Integer from 0 to 9 inclusive
print(m)

#random.randrange(start, stop[, step])
m=random.randrange(0, 101, 3)                 # Even integer from 0 to 100 inclusive
print(m)


```

 补充：`range() `函数，其功能是创建一个整数列表

语法格式：range(start, stop, step)，参数使用参考如下：

- `start`: 计数从 start 开始。默认是从 0 开始。例如range（4）等价于range（0， 4）;结果：（0,1,2,3）
- `stop` : 计数到 stop 结束，但不包括 stop。例如：range（0， 5） 是[0, 1, 2, 3, 4]没有5
- `step` ：步长，默认为1。例如：range（0， 5） 等价于 range(0, 5, 1)


```python
# random.randint(a, b)
m=random.randint(1, 10)
print(m)

```



## `.choice()`

Choose a random element from a non-empty sequence.

```python
#random.choice(seq)
m0=['hu','gu','gg']
m1=random.choice(m0)
# 'hu'
```

从非空序列 seq 返回一个随机元素。 如果 seq 为空，则引发 IndexError。

```python
andom.choices(population, weights=None, *, cum_weights=None, k=1)
```

从*population*中选择替换，返回大小为 k 的元素列表。 如果 population 为空，则引发 IndexError。

```python
#random.choices(population, weights=None, *, cum_weights=None, k=1)
# Six roulette wheel spins (weighted sampling with replacement)
n=random.choices(['red', 'black', 'green'], [18, 18, 2], k=6)
print(n)

输出：['black', 'red', 'black', 'red', 'red', 'red']

```

如果指定了 weight 序列，则根据相对权重进行选择。 或者，如果给出 cum_weights 序列，则根据累积权重（可能使用 itertools.accumulate() 计算）进行选择。 例如，相对权重[10, 5, 30, 5]相当于累积权重[10, 15, 45, 50]。 在内部，相对权重在进行选择之前会转换为累积权重，因此提供累积权重可以节省工作量。

如果既未指定 weight 也未指定 cum_weights ，则以相等的概率进行选择。 如果提供了权重序列，则它必须与 population 序列的长度相同。 一个 TypeError 指定了 weights 和cum_weights。

weights 或 cum_weights 可以使用任何与 random() 返回的 float 值互操作的数值类型（包括整数，浮点数和分数但不包括十进制小数）。

对于给定的种子，具有相等加权的 choices() 函数通常产生与重复调用 choice() 不同的序列。 choices() 使用的算法使用浮点运算来实现内部一致性和速度。 choice() 使用的算法默认为重复选择的整数运算，以避免因舍入误差引起的小偏差。


## `.shuffle()`

将序列 x 随机打乱位置。

```python
deck = 'ace two three four'.split()
random.shuffle(deck)                        # Shuffle a list
print(deck)

```

可选参数 random 是一个0参数函数，在 [0.0, 1.0) 中返回随机浮点数；默认情况下，这是函数 random() 。

要改变一个不可变的序列并返回一个新的打乱列表，请使用sample(x, k=len(x))。

请注意，即使对于小的 len(x)，x 的排列总数也可以快速增长，大于大多数随机数生成器的周期。 这意味着长序列的大多数排列永远不会产生。 例如，长度为2080的序列是可以在 Mersenne Twister 随机数生成器的周期内拟合的最大序列。

## `.sample()`

`andom.sample(population, k)`
返回从总体序列或集合中选择的唯一元素的 k 长度列表。== 用于无重复的随机抽样==。

```
>>> sample([10, 20, 30, 40, 50], k=4)    # Four samples without replacement
[40, 10, 50, 30]

```

返回包含来自总体的元素的新列表，同时保持原始总体不变。 结果列表按选择顺序排列，因此所有子切片也将是有效的随机样本。 这允许抽奖获奖者（样本）被划分为大奖和第二名获胜者（子切片）。

总体成员不必是 hashable 或 unique 。 如果总体包含重复，则每次出现都是样本中可能的选择。

要从一系列整数中选择样本，请使用 range() 对象作为参数。 对于从大量人群中采样，这种方法特别快速且节省空间：sample(range(10000000), k=60) 。

如果样本大小大于总体大小，则引发 ValueError 。


## `验证码案例`

```python
import  random

def v_code():
    ret=''
    for i in range(5):
        num=random.randint(0,9)
        alf=chr(random.randint(65,122))
        s=str(random.choice([num,alf]))
        ret+=s
    return  ret

print(v_code())

```


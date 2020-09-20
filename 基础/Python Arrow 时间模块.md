# Python Arrow 时间模块

Arrow: Better dates & times for Python

`Arrow`是一个用于时间处理的python库。它能够一键转化`dates`、`times`、`timestamps`等多种时间格式，而不需要大量import各种时间模块和格式转化函数。十分便捷和人性化，能够极大程度简化你的代码。

使用Arrow仅需两步，第一步接受各种类型时间类型（`datetime`,`date`,`timestamps`）转化为Arrow类型，第二步转化成自己需要的格式或进行操作。



```python
import arrow

#获取当前时间的Arrow格式
i=arrow.now()
i
<Arrow [2019-11-21T17:04:57.018170+08:00]>

str(i)
'2019-11-21T17:04:57.018170+08:00'

转化时间戳
i.timestamp
1574327097

年 月 日 时 分 秒
i.format('YYYY-MM-DD HH:mm:ss')
'2019-11-21 17:04:57'

年 月 日
i.format('YYYY-MM-DD')
'2019-11-21'

时 分 秒
i.format('HH:mm:ss')
'17:04:57'

星期
i.format('MMM DD dddd ')
'Nov 21 Thursday '

66天后日期
i.shift(days=66).format('YYYY-MM-DD')
'2020-01-26'


修改日期,梦回2008
i.replace(year=2008, month=8, day=8).format('YYYY-MM-DD')
'2008-08-08'



```

## 获取当前时间

```python
arrow.now()
#<Arrow [2019-11-21T17:23:21.463980+08:00]>

arrow.now('US/Pacific')
# <Arrow [2019-11-21T01:23:50.697479-08:00]>

arrow.utcnow()
# <Arrow [2019-11-21T09:24:57.171157+00:00]>

```

## `get` 格式转化

```python
from datetime import datetime
arrow.get(datetime(2019, 11, 21))
#<Arrow [2019-11-21T00:00:00+00:00]>

arrow.get(datetime(2019, 11, 21),'US/Pacific')

```

解析字符串格式：

```python
>>> arrow.get('2013-05-05 12:30:45', 'YYYY-MM-DD HH:mm:ss')
#<Arrow [2013-05-05T12:30:45+00:00]>

```

当天数为一位数字时 ，前面不可以＋0

```python
>>> arrow.get(2013, 5, 5)
<Arrow [2013-05-05T00:00:00+00:00]>

>>> arrow.Arrow(2013, 5, 5)
<Arrow [2013-05-05T00:00:00+00:00]>

arrow.get(2019,11,21).format("YYYY-MM-DD")
#'2019-11-21'

arrow.get("20191121").format("YYYY-MM-DD")
#'2019-11-21'


```

## `Replace` & `Shift`

Get a new Arrow object, with altered attributes, just as you would with a datetime:

**时间替换  a.replace(\**kwargs)**

​    返回一个被替换后的arrow对象，原对象不变

```python
In [14]: arw = arrow.utcnow()

In [15]: arw
Out[15]: <Arrow [2019-11-21T11:07:16.675758+00:00]>

In [16]: arw.replace(hour=4, minute=40)
Out[16]: <Arrow [2019-11-21T04:40:16.675758+00:00]>

```

Or, get one with attributes shifted forward or backward:

` shift` 方法获取某个时间之前或之后的时间,关键字参数为years,months,weeks,days,hours，seconds，microseconds

```python
In [17]: arw.shift(weeks=+3) #三周后
Out[17]: <Arrow [2019-12-12T11:07:16.675758+00:00]>

In [18]: arw.shift(days=-14)
Out[18]: <Arrow [2019-11-07T11:07:16.675758+00:00]>

In [19]: arw.shift(days =14)
Out[19]: <Arrow [2019-12-05T11:07:16.675758+00:00]>

In [20]: arw.shift(years=1)
Out[20]: <Arrow [2020-11-21T11:07:16.675758+00:00]>

```

## `Format`

```python
In [21]: arrow.utcnow().format('YYYY-MM-DD HH:mm:ss ZZ')
Out[21]: '2019-11-21 11:11:14 +00:00'

```

## `Ranges` & `Spans`

Get the time span of any unit:

```python
In [22]: arrow.utcnow().span('hour')
Out[22]:
(<Arrow [2019-11-21T11:00:00+00:00]>,
 <Arrow [2019-11-21T11:59:59.999999+00:00]>)

```

Or just get the floor and ceiling:

```python
In [23]: arrow.utcnow().floor('hour')
Out[23]: <Arrow [2019-11-21T11:00:00+00:00]>

In [24]: arrow.utcnow().ceil('hour')
Out[24]: <Arrow [2019-11-21T11:59:59.999999+00:00]>

```

You can also get a range of time spans:

`arrow.Arrow.span_rang()`

返回一个元组

```python
start = datetime(2013, 5, 5,)
end = datetime(2013,6, 5,)
for r in arrow.Arrow.span_range('day', start, end):
    print(r)
    print(type(r))
    

# (<Arrow [2013-05-05T00:00:00+00:00]>, <Arrow [2013-05-05T23:59:59.999999+00:00]>)
# <class 'tuple'>

```

Or just iterate over a range of time:

```python
start = datetime(2013, 5, 5, 12, 30)
end = datetime(2013, 5, 5, 17, 15)
for r in arrow.Arrow.range('hour', start, end):
    print( repr(r))

<Arrow [2013-05-05T12:30:00+00:00]>
<Arrow [2013-05-05T13:30:00+00:00]>
<Arrow [2013-05-05T14:30:00+00:00]>
<Arrow [2013-05-05T15:30:00+00:00]>
<Arrow [2013-05-05T16:30:00+00:00]>


'或者'
start =arrow.get (2013, 5, 5, 12, 30)
end = arrow.get(2013, 5, 5, 17, 15)
for r in arrow.Arrow.range('hour', start, end):
    print( repr(r))
<Arrow [2013-05-05T12:30:00+00:00]>
<Arrow [2013-05-05T13:30:00+00:00]>
<Arrow [2013-05-05T14:30:00+00:00]>
<Arrow [2013-05-05T15:30:00+00:00]>
<Arrow [2013-05-05T16:30:00+00:00]>




'对获取范围内的日期进行格式化'
'range' 的到的范围，为闭区间
start =arrow.get (2013, 5, 5, 12, 30)
end = arrow.get(2013, 5, 10, 17, 15)
for r in arrow.Arrow.range('day', start, end):
    print(r.format("YYYY-MM-DD"))

2013-05-05
2013-05-06
2013-05-07
2013-05-08
2013-05-09
2013-05-10


'或者'
start =arrow.get (2013, 5, 5, 12, 30)
end = arrow.get(2013, 5, 5, 17, 15)
for r in arrow.Arrow.range('hour', start, end):
    print( repr(r))


'对获取范围内的日期进行格式化'
'range'的到的范围，为闭区间
start =arrow.get (2013, 5, 5, 12, 30)
end = arrow.get(2013, 5, 10, 17, 15)
for r in arrow.Arrow.range('day', start, end):
    print(r.format("YYYY-MM-DD"))

2013-05-05
2013-05-06
2013-05-07
2013-05-08
2013-05-09
2013-05-10

```

## **人性化输出  a.humanize()**

```python
>>> present = arrow.utcnow()
>>> past = present.shift(hours=-1)
>>> past.humanize()       #相对于当前时间
'an hour age'
>>> future = present.shift(hours=2)
>>> future.humanize(present)    #相对于参数时间
'in 2 hours'
>>> past.humanize(present, locale='zh')   #locale参数可以指定地区语言
'1天前'
```





参考：

<a>https://arrow.readthedocs.io/en/latest/</a> 


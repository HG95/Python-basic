# Python 时间模块

Python时间和日期操作需要用到datetime和time标准库模块。

## 一、time模块

### time模块表示时间的方式

①时间戳
②格式化的时间字符串
③以数组的形式表示，即(struct_time),共有九个元素

- year (four digits, e.g. 1998)
- month (1-12)
- day (1-31)
- hours (0-23)
- minutes (0-59)
- seconds (0-59)
- weekday (0-6, Monday is 0)
- Julian day (day in the year, 1-366)
- DST (Daylight Savings Time) flag (-1, 0 or 1) 是否是夏令时

### 常用函数

##### `asctime([tuple])`-> string

将一个struct_time(默认为当时时间)，转换成字符串。

```python
>>> time.asctime()
'Sun Jul 31 11:10:39 2016'
>>> import time
>>> thisTime = "2016-07-31 12:12:12"
>>> timeTuple = time.strptime(thisTime, "%Y-%m-%d %H:%M:%S")
>>> time.asctime(timeTuple)
'Sun Jul 31 12:12:12 2016'

```

##### `ctime(seconds)`-> string

将一个时间戳(默认为当前时间)转换成一个时间字符串。

```python
>>> time.ctime()
'Sun Jul 31 11:11:33 2016'
>>> time.ctime(1469938332)
'Sun Jul 31 12:12:12 2016'

```

##### `strftime(format[, tuple])`-> string

将指定的struct_time(默认为当前时间)，根据指定的格式化字符串输出。

```python
>>> time.strftime("%Y-%m-%d %H:%M:%S")
'2016-07-31 11:20:41'
>>> import time
>>> thisTime = "2016-07-31 12:12:12"
>>> timeTuple = time.strptime(thisTime, "%Y-%m-%d %H:%M:%S")
>>> time.strftime("%Y-%m-%d %H:%M:%S",timeTuple)
'2016-07-31 12:12:12'

```

##### `strptime(string, format)`-> struct_time

将时间字符串根据指定的格式化符转换成数组形式的时间。

```python
>>> import time
>>> thisTime = "2016-07-31 12:12:12"
>>> timeTuple = time.strptime(thisTime, "%Y-%m-%d %H:%M:%S")
>>> print timeTuple
time.struct_time(tm_year=2016, tm_mon=7, tm_mday=31, tm_hour=12, tm_min=12, tm_sec=12, tm_wday=6, tm_yday=213, tm_isdst=-1)

```

##### `time()` -> floating point number

返回当前时间的时间戳。

```python
>>> time.time()
1469935566.776

```

## 二、datetime模块

### datetime中的常量

- `datetime.MINYEAR`，表示datetime所能表示的最小年份，MINYEAR = 1。
- `datetime.MAXYEAR`，表示datetime所能表示的最大年份，MAXYEAR = 9999

### datetime中的类

- datetime.date：表示日期的类。常用的属性有year, month, day；
- datetime.time：表示时间的类。常用的属性有hour, minute, second, microsecond；
- datetime.datetime：表示日期时间。
- datetime.timedelta：表示时间间隔，即两个时间点之间的长度。
- datetime.tzinfo：与时区有关的相关信息

#### date类

date类表示一个由年、月、日组成的日期。

##### 类方法与类属性

date类定义了一些常用的类方法与类属性，方便我们操作：

- date.max、date.min：date对象所能表示的最大、最小日期；
- date.resolution：date对象表示日期的最小单位。这里是天。
- `date.today()`：返回一个表示当前本地日期的date对象；

```python
from datetime import  date

print(date.today())         #datetime.date(2019, 11, 12)
print(type(date.today()))   #<class 'datetime.date'>

#可以转化为字符串格式的,进行分割等字符串的相关操作
print(str(date.today()))    #'2019-11-12'

```

##### 实例方法和属性

- date.year、date.month、date.day：年、月、日；
- date.replace(year, month,day)：生成一个新的日期对象，用参数指定的年，月，日代替原有对象中的属性。（原有对象仍保持不变）
- date.timetuple()：返回日期对应的time.struct_time对象；
- date.toordinal()：返回日期对应的Gregorian Calendar日期；
- date.weekday()：返回weekday，如果是星期一，返回0；如果是星期2，返回1，以此类推；
- data.isoweekday()：返回weekday，如果是星期一，返回1；如果是星期2，返回2，以此类推；
- date.isocalendar()：返回格式如(year，month，day)的元组；
- date.isoformat()：返回格式如’YYYY-MM-DD’的字符串；
- date.strftime(fmt)：自定义格式化字符串。

##### date支持的其他操作

- date2 = date1 + timedelta #日期加上一个间隔，返回一个新的日期对象）
- date2 = date1 - timedelta #日期隔去间隔，返回一个新的日期对象
- timedelta = date1 - date2 #两个日期相减，返回一个时间间隔对象
- date1 < date2 #两个日期进行比较

```python
date1 , date2  要是这样的时间格式
datetime.date(year, month, day)

```

## 三、常见的日期时间操作

更改日期格式
方法:先转换为时间元组,然后转换为其他格式。

```python
>>> import time
>>> thisTime = "2016-07-31 12:12:12"
>>>timeTuple = time.strptime(thisTime, "%Y-%m-%d %H:%M:%S")
>>>otherTime = time.strftime("%Y/%m/%d %H:%M:%S", timeTuple)
>>> print otherTime
2016/07/31 12:12:12

```

将当前时间并转换为指定日期格式

```python
>>> import time
>>> nowStamp = int(time.time())  
>>> timeTuple = time.localtime(nowStamp)
>>> otherTime = time.strftime("%Y-%m-%d %H:%M:%S", timeTuple)
>>> print otherTime
2016-07-31 10:19:47



>>> import datetime
>>> now = datetime.datetime.now()
>>> otherTime = now.strftime("%Y-%m-%d %H:%M:%S")
>>> print otherTime
2016-07-31 10:21:45


```

参考

<a>https://blog.csdn.net/HHG20171226/article/details/102808269</a> 
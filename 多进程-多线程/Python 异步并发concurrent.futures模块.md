# Python 异步并发concurrent.futures模块

﻿python异步并发模块concurrent.futures。它非常简单易用，主要用来实现多线程和多进程的异步并发。

##  `Executor`对象

```python
class concurrent.futures.Executor
```
Executor是一个抽象类，它提供了异步执行调用的方法。它不能直接使用，但可以通过它的两个子类`ThreadPoolExecutor`或者`ProcessPoolExecutor`进行调用。

```python
Executor.submit(fn, *args, **kwargs)

fn：需要异步执行的函数

*args, **kwargs：fn参数
```
示例：
```python
from concurrent import futures


def test(num):
    import time
    return time.ctime(), num


with futures.ThreadPoolExecutor(max_workers=1) as executor:
    future = executor.submit(test, 1)
    print(future.result())      
    
    #('Tue Oct 29 21:47:45 2019', 1)
```

```python
Executor.map(func, *iterables, timeout=None)

相当于map(func, *iterables)，但是func是异步执行。timeout的值可以是int或float，如果操作超时，会返回raisesTimeoutError；如果不指定timeout参数，则不设置超时间。

func：需要异步执行的函数

*iterables：可迭代对象，如列表等。每一次func执行，都会从iterables中取参数。

timeout：设置每次异步操作的超时时间
```

```python
data = [1, 2, 3]
with futures.ThreadPoolExecutor(max_workers=1) as executor:
    for future in executor.map(test, data):
        print(future)
'''
('Tue Oct 29 21:52:01 2019', 1)
('Tue Oct 29 21:52:01 2019', 2)
('Tue Oct 29 21:52:01 2019', 3)
'''
```
关闭线程
```python
Executor.shutdown(wait=True)
释放系统资源,在Executor.submit()或 Executor.map()等异步操作后调用。
使用with语句可以避免显式调用此方法。
```
##  `ThreadPoolExecutor`对象

```python
ThreadPoolExecutor类是Executor子类，使用线程池执行异步调用.

class concurrent.futures.ThreadPoolExecutor(max_workers)

使用max_workers数目的线程池执行异步调用
```

```python
import pandas as pd
from  datetime import date
from time import time

from  concurrent.futures import ThreadPoolExecutor,ProcessPoolExecutor
def parse (date_str, point):
    print(date_str,'------->',point)

#创建线程对象，和线程个数

threadPool = ThreadPoolExecutor(30)

date_list = pd.date_range("19000101", str(date.today()))
# 时间点
time_point = [2, 8, 14, 20]

for each_date in date_list:
    date_str = str(each_date).split(" ")[0]
    for each_point in time_point:
        # 多线程
        threadPool.submit(
            parse,
            date_str=date_str,
            point=each_point
        )

# 关闭线程池
threadPool.shutdown()
```
## 线程池的基本使用

```python
# coding: utf-8
from concurrent.futures import ThreadPoolExecutor
import time


def spider(page):
    time.sleep(page)
    print(f"crawl task{page} finished")
    return page

with ThreadPoolExecutor(max_workers=5) as t:  # 创建一个最大容纳数量为5的线程池
    task1 = t.submit(spider, 1)
    task2 = t.submit(spider, 2)  # 通过submit提交执行的函数到线程池中
    task3 = t.submit(spider, 3)

    print(f"task1: {task1.done()}")  # 通过done来判断线程是否完成
    print(f"task2: {task2.done()}")
    print(f"task3: {task3.done()}")

    time.sleep(2.5)
    print(f"task1: {task1.done()}")
    print(f"task2: {task2.done()}")
    print(f"task3: {task3.done()}")
    print(task1.result())  # 通过result来获取返回值
```
执行结果如下:

```python
task1: False
task2: False
task3: False
crawl task1 finished
crawl task2 finished
task1: True
task2: True
task3: False
1
crawl task3 finished

```
1. 使用` with` 语句 ，通过 `ThreadPoolExecutor `构造实例，同时传入 `max_workers` 参数来设置线程池中最多能同时运行的线程数目。
2. 使用` submit 函数`来提交线程需要执行的任务到线程池中，并返回该任务的句柄（类似于文件、画图），注意 submit() 不是阻塞的，而是立即返回。
3. 通过使用 `done() 方法`判断该任务是否结束。上面的例子可以看出，提交任务后立即判断任务状态，显示四个任务都未完成。在延时2.5后，task1 和 task2 执行完毕，task3 仍在执行中。
4. 使用` result() 方法`可以获取任务的返回值。

### **主要方法**

#### `wait`

```python
 wait(fs, timeout=None, return_when=ALL_COMPLETED)

```
`wait `接受三个参数： `fs`: 表示需要执行的序列 timeout: 等待的最大时间，如果超过这个时间即使线程未执行完成也将返回 ,`return_when`：表示wait返回结果的条件，默认为` ALL_COMPLETED` 全部执行完成再返回.


```python
from concurrent.futures import ThreadPoolExecutor, wait, FIRST_COMPLETED, ALL_COMPLETED
import time

def spider(page):
    time.sleep(page)
    print(f"crawl task{page} finished")
    return page

with ThreadPoolExecutor(max_workers=5) as t: 
    all_task = [t.submit(spider, page) for page in range(1, 5)]
    wait(all_task, return_when=FIRST_COMPLETED)
    print('finished')
    print(wait(all_task, timeout=2.5))

# 运行结果
crawl task1 finished
finished
crawl task2 finished
crawl task3 finished
DoneAndNotDoneFutures(done={<Future at 0x28c8710 state=finished returned int>, <Future at 0x2c2bfd0 state=finished returned int>, <Future at 0x2c1b7f0 state=finished returned int>}, not_done={<Future at 0x2c3a240 state=running>})
crawl task4 finished

```

#### `as_completed`

上面虽然提供了判断任务是否结束的方法，但是不能在主线程中一直判断啊。最好的方法是当某个任务结束了，就给主线程返回结果，而不是一直判断每个任务是否结束。
`ThreadPoolExecutor` 中 的 `as_completed()` 就是这样一个方法，当子线程中的任务执行完后，直接用 `result() `获取返回结果

```python
# coding: utf-8
from concurrent.futures import ThreadPoolExecutor, as_completed
import time


def spider(page):
    time.sleep(page)
    print(f"crawl task{page} finished")
    return page

def main():
    with ThreadPoolExecutor(max_workers=5) as t:
        obj_list = []
        for page in range(1, 5):
            obj = t.submit(spider, page)
            obj_list.append(obj)

        for future in as_completed(obj_list):
            data = future.result()
            print(f"main: {data}")

# 执行结果
crawl task1 finished
main: 1
crawl task2 finished
main: 2
crawl task3 finished
main: 3
crawl task4 finished
main: 4

```
`as_completed()` 方法是一个生成器，在没有任务完成的时候，会一直阻塞，除非设置了 timeout。
当有某个任务完成的时候，会 yield 这个任务，就能执行 for 循环下面的语句，然后继续阻塞住，循环到所有的任务结束。同时，先完成的任务会先返回给主线程。
#### `map`

```python
map(fn, *iterables, timeout=None)

```
`fn`： 第一个参数 fn 是需要线程执行的函数；
`iterables`：第二个参数接受一个可迭代对象；
`timeout`： 第三个参数 timeout 跟 wait() 的 timeout 一样，但由于 map 是返回线程执行的结果，如果 timeout小于线程执行时间会抛异常 TimeoutError。

```python
import time
from concurrent.futures import ThreadPoolExecutor

def spider(page):
    time.sleep(page)
    return page

start = time.time()
executor = ThreadPoolExecutor(max_workers=4)

i = 1
for result in executor.map(spider, [2, 3, 1, 4]):
    print("task{}:{}".format(i, result))
    i += 1

#  运行结果
task1:2
task2:3
task3:1
task4:4

```
使用 `map 方法`，无需提前使用 `submit 方法`，map 方法与 python 高阶函数 map 的含义相同，都是将序列中的每个元素都执行同一个函数。

上面的代码对列表中的每个元素都执行 spider() 函数，并分配各线程池。

```python
# coding: utf-8
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed
import time
import json
from requests import adapters

from proxy import get_proxies

headers = {
    "Host": "splcgk.court.gov.cn",
    "Origin": "https://splcgk.court.gov.cn",
    "User-Agent": "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36",
    "Referer": "https://splcgk.court.gov.cn/gzfwww/ktgg",
}
url = "https://splcgk.court.gov.cn/gzfwww/ktgglist?pageNo=1"

def spider(page):
    data = {
        "bt": "",
        "fydw": "",
        "pageNum": page,
    }
    for _ in range(5):
        try:
            response = requests.post(url, headers=headers, data=data, proxies=get_proxies())
            json_data = response.json()
        except (json.JSONDecodeError, adapters.SSLError):
            continue
        else:
            break
    else:
        return {}

    return json_data

def main():
    with ThreadPoolExecutor(max_workers=8) as t:
        obj_list = []
        begin = time.time()
        for page in range(1, 15):
            obj = t.submit(spider, page)
            obj_list.append(obj)

        for future in as_completed(obj_list):
            data = future.result()
            print(data)
            print('*' * 50)
        times = time.time() - begin
        print(times)

if __name__ == "__main__":
    main()

```

### ` ProcessPoolExecutor`对象

```python
ThreadPoolExecutor类是Executor子类，使用进程池执行异步调用.

class concurrent.futures.ProcessPoolExecutor(max_workers=None)

使用max_workers数目的进程池执行异步调用，如果max_workers为None
则使用机器的处理器数目（如4核机器max_worker配置为None时，则使用4个进程进行异步并发）。
```

```python
from concurrent import futures


def test(num):
    import time
    return time.ctime(), num


def muti_exec(m, n):
    # m 并发次数
    # n 运行次数

    with futures.ProcessPoolExecutor(max_workers=m) as executor:  # 多进程
        # with futures.ThreadPoolExecutor(max_workers=m) as executor: #多线程
        executor_dict = dict((executor.submit(test, times), times) for times in range(m * n))

    for future in futures.as_completed(executor_dict):
        times = executor_dict[future]
        if future.exception() is not None:
            print('%r generated an exception: %s' % (times, future.exception()))
        else:
            print('RunTimes:%d,Res:%s' % (times, future.result()))


if __name__ == '__main__':
    muti_exec(5, 1)

'''
RunTimes:4,Res:('Tue Oct 29 21:56:12 2019', 4)
RunTimes:2,Res:('Tue Oct 29 21:56:12 2019', 2)
RunTimes:0,Res:('Tue Oct 29 21:56:12 2019', 0)
RunTimes:1,Res:('Tue Oct 29 21:56:12 2019', 1)
RunTimes:3,Res:('Tue Oct 29 21:56:12 2019', 3)
'''
```

线程池：

```python
from concurrent.futures import ProcessPoolExecutor,ThreadPoolExecutor
import threading
import os,time,random
def task(n):
    print('%s:%s is running' %(threading.currentThread().getName(),os.getpid()))
    time.sleep(2)
    return n**2

if __name__ == '__main__':
    p=ThreadPoolExecutor()   #不填则默认为cpu的个数*5
    l=[]
    start=time.time()
    for i in range(10):
        obj=p.submit(task,i)
        l.append(obj)
    p.shutdown()
    print('='*30)
    print([obj.result() for obj in l])
    print(time.time()-start)

#上面方法也可写成下面的方法
    # start = time.time()
    # with ThreadPoolExecutor() as p:   #类似打开文件,可省去.shutdown()
    #     future_tasks = [p.submit(task, i) for i in range(10)]
    # print('=' * 30)
    # print([obj.result() for obj in future_tasks])
    # print(time.time() - start)
'''
 ThreadPoolExecutor-0_0:1204 is running
ThreadPoolExecutor-0_1:1204 is running
ThreadPoolExecutor-0_2:1204 is running
ThreadPoolExecutor-0_3:1204 is running
ThreadPoolExecutor-0_4:1204 is running
ThreadPoolExecutor-0_5:1204 is running
ThreadPoolExecutor-0_6:1204 is running
ThreadPoolExecutor-0_7:1204 is running
ThreadPoolExecutor-0_8:1204 is running
ThreadPoolExecutor-0_9:1204 is running
==============================
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
2.0051467418670654
'''
```
进程池：

```python
from concurrent.futures import ProcessPoolExecutor
import os,time,random
def task(n):
    print('%s is running' %os.getpid())
    time.sleep(2)
    return n**2


if __name__ == '__main__':
    p=ProcessPoolExecutor()  #不填则默认为cpu的个数
    l=[]
    start=time.time()
    for i in range(10):
        obj=p.submit(task,i)   #submit()方法返回的是一个future实例，要得到结果需要用obj.result()
        l.append(obj)

    p.shutdown()  #类似用from multiprocessing import Pool实现进程池中的close及join一起的作用
    print('='*30)
    # print([obj for obj in l])
    print([obj.result() for obj in l])
    print(time.time()-start)

    #上面方法也可写成下面的方法
    # start = time.time()
    # with ProcessPoolExecutor() as p:   #类似打开文件,可省去.shutdown()
    #     future_tasks = [p.submit(task, i) for i in range(10)]
    # print('=' * 30)
    # print([obj.result() for obj in future_tasks])
    # print(time.time() - start)
'''
32088 is running
30512 is running
23388 is running
25216 is running
32088 is running
30512 is running
23388 is running
25216 is running
32088 is running
30512 is running
==============================
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
6.5124876499176025
'''
```
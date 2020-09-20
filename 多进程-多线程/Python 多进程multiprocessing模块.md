# Python 多进程multiprocessing模块

`multiprocessing模块介绍`
python中的多线程无法利用多核优势，如果想要充分地使用多核CPU的资源（os.cpu_count()查看），在python中大部分情况需要使用多进程。Python提供了`multiprocessing`。
    multiprocessing模块用来开启                                                                                           子进程，并在子进程中执行我们定制的任务（比如函数），该模块与多线程模块threading的编程接口类似。

　 multiprocessing模块的功能众多：支持子进程、通信和共享数据、执行不同形式的同步，提供了Process、Queue、Pipe、Lock等组件。
>    与线程不同，进程没有任何共享状态，进程修改的数据，改动仅限于该进程内。
## 一、`Process类的介绍`
 创建进程的类：

```python
Process(group=None, 
		target=None, 
		name=None, 
		args=(), 
		kwargs={}
	)
```

**参数介绍：**
- `group`参数未使用，值始终为None
- `targe`t表示调用对象，即子进程要执行的任务
- `args`表示调用对象的位置参数元组，args=(1,2,'egon',)
- `kwargs`表示调用对象的字典,kwargs={'name':'egon','age':18}
- name为子进程的名称
**强调：**
1. 需要使用关键字的方式来指定参数
2. args指定的为传给target函数的位置参数，是一个元组形式，必须有逗号

 **方法介绍：**
- `star()` 方法启动进程，
- `join() `方法实现进程间的同步，等待所有进程退出。
- `close()` 用来阻止多余的进程涌入进程池 Pool 造成进程阻塞。
- `p.is_alive()` 如果p在运行，返回True
- `p.join([timeout])` 等待进程p运行结束。timeout是可选的超时期限。如果timeout为None，则认为要无限期等待
- ` p.run()` 进程启动时运行的方法。默认情况下，会调用传递给Process构造函数中的target；定义进程的另一种方法是继承Process并重写run()方法
- `p.start()` 运行进程p，并调用p.run()
- `p.terminate()` 强制杀死进程。如果调用此方法，进程p将被立即终止，同时不会进行任何清理动作。如果进程p创建了自己的子进程，这些进程将会变成 僵尸进程,此方法要小心使用
- `p.authkey` 进程的身份验证键
- `p.daemon` 守护进程标志，布尔变量。指进程是否为后台进程。如果该进程为后台进程(daemon = True)，当创建它的Python进程终止时，后台进程将自动终止; 其中p.daemon的值要在使用p.start()启动进程之前设置,并且禁止后台进程创建子进程
  - `p.daemon = True` 主进程终止,子进程终止,
  - `p.daemon = False` 主进程终止,子进程不会终止	
- `p.exitcode `进程的整数退出码。如果进程仍在运行，值为None。如果值为-N，表示进程由信号N终止
- ` p.name `进程名
- `p.pid` 进程号

<font color=#D2691E size=4>需要注意的是`start()`，`join()`，`is_alive()`， `terminate()`和`exitcode`方法只能由创建进程对象的过程调用。

###  `Process类的使用`

>注意：在windows中Process()必须放到# if __name__ == '__main__':下

```python
class multiprocessing.pool.Pool([processes
								[, initializer
								[, initargs
								[, maxtasksperchild
								[, context]]]]]
						)

```
- `processes`： 是要使用的工作进程数。如果进程是None，那么使用返回的数字os.cpu_count()。也就是说根据本地的cpu个数决定，processes小于等于本地的cpu个数；
- `initializer`： 如果initializer是None，那么每一个工作进程在开始的时候会调用initializer(*initargs)。
- `maxtasksperchild`：工作进程退出之前可以完成的任务数，完成后用一个新的工作进程来替代原进程，来让闲置的资源被释放。
- `maxtasksperchild`默认是None，意味着只要Pool存在工作进程就会一直存活。
- `context`: 用在制定工作进程启动时的上下文，一般使用 multiprocessing.Pool() 或者一个context对象的Pool()方法来创建一个池，两种方法都适当的设置了context。

**创建并开启子进程的两种方式**
开进程的方法一:
```python
import multiprocessing
import os

def run_proc(name):
    print('Child process {0} {1} Running '.format(name, os.getpid()))

if __name__ == '__main__':
    print('Parent process {0} is Running'.format(os.getpid()))
    for i in range(5):
        p = multiprocessing.Process(target=run_proc, args=(str(i),))
        print('process start')
        p.start()
    p.join()
    print('Process close')

```

结果：

```python
Parent process 132412 is Running
process start
process start
process start
process start
process start
Child process 3 18612 Running 
Child process 1 8840 Running 
Child process 2 142376 Running 
Child process 0 39368 Running 
Child process 4 142564 Running 
Process close
```

开进程的方法二:
```python
import time
import random
from multiprocessing import Process


class Piao(Process):
    def __init__(self,name):
        super().__init__()
        self.name=name
    def run(self):
        print('%s piaoing' %self.name)

        time.sleep(random.randrange(1,5))
        print('%s piao end' %self.name)

p1=Piao('egon')
p2=Piao('alex')
p3=Piao('wupeiqi')
p4=Piao('yuanhao')

p1.start() #start会自动调用run
p2.start()
p3.start()
p4.start()
print('主线程')
```

**Process对象的join方法**
```python
from multiprocessing import Process
import time
import random
def piao(name):
    print('%s is piaoing' %name)
    time.sleep(random.randint(1,3))
    print('%s is piao end' %name)

p1=Process(target=piao,args=('egon',))
p2=Process(target=piao,args=('alex',))
p3=Process(target=piao,args=('yuanhao',))
p4=Process(target=piao,args=('wupeiqi',))

p1.start()
p2.start()
p3.start()
p4.start()

#有的同学会有疑问:既然join是等待进程结束,那么我像下面这样写,进程不就又变成串行的了吗?
#当然不是了,必须明确：p.join()是让谁等？
#很明显p.join()是让主线程等待p的结束，卡住的是主线程而绝非进程p，

#详细解析如下：
#进程只要start就会在开始运行了,所以p1-p4.start()时,系统中已经有四个并发的进程了
#而我们p1.join()是在等p1结束,没错p1只要不结束主线程就会一直卡在原地,这也是问题的关键
#join是让主线程等,而p1-p4仍然是并发执行的,p1.join的时候,其余p2,p3,p4仍然在运行,等#p1.join结束,可能p2,p3,p4早已经结束了,这样p2.join,p3.join.p4.join直接通过检测，无需等待
# 所以4个join花费的总时间仍然是耗费时间最长的那个进程运行的时间
p1.join()
p2.join()
p3.join()
p4.join()

print('主线程')
#上述启动进程与join进程可以简写为
# p_l=[p1,p2,p3,p4]
# 
# for p in p_l:
#     p.start()
# 
# for p in p_l:
#     p.join()
```
## 二、`Pool`，进程池
Pool 可以提供指定数量的进程供用户使用，默认是 CPU 核数。当有新的请求提交到 Poll 的时候，如果池子没有满，会创建一个进程来执行，否则就会让该请求等待。
- `apply`(func[, args[, kwds]])
- `apply_async`(func[, args[, kwds[, callback]]])
- `map`(func, iterable[, chunksize])
- `map_async`(func, iterable[, chunksize[, callback]])
- `imap`(func, iterable[, chunksize])
- `imap_unordered`(func, iterable[, chunksize])


- `Pool `对象调用` join` 方法会等待所有的子进程执行完毕
- 调用` join `方法之前，必须调用 `close`
- 调用 `close `之后就不能继续添加新的 Process 


实例方法
### 1、`apply`
```python
apply（func [，args [，kwds ] ] ）
使用参数args和关键字参数kwds调用func。它会阻塞，直到结果准备就绪。鉴于此块，更适合并行执行工作。此外，func 仅在池中的一个工作程序中执行。
```

```python
apply(func[, args[, kwds]])
```
该方法只能允许一个进程进入池子，在一个进程结束之后，另外一个进程才可以进入池子。

```python
from multiprocessing import Pool
import time
def test(p):
       print(p)
       # time.sleep(3)
if __name__=="__main__":
    pool = Pool(processes=10)
    for i  in range(500):
        '''
        ('\n'
         '	（1）遍历500个可迭代对象，往进程池放一个子进程\n'
         '	（2）执行这个子进程，等子进程执行完毕，再往进程池放一个子进程，再执行。（同时只执行一个子进程）\n'
         '	 for循环执行完毕，再执行print函数。\n'
         '	')
        '''
        pool.apply(test, args=(i,))   #维持执行的进程总数为10，当一个进程执行完后启动一个新进程.
    print('test')
    pool.close()
    pool.join()
```
   for循环内执行的步骤顺序，往进程池中添加一个子进程，执行子进程，等待执行完毕再添加一个子进程……等500个子进程都执行完了，再执行print。（从结果来看，并没有多进程并发）
```python
import multiprocessing
import os
import time

def run_task(name):
    # print('Task {0} pid {1} is running, parent id is {2}'.format(name, os.getpid(), os.getppid()))
    # time.sleep(1)
    print('Task {0} end.'.format(name))

if __name__ == '__main__':
    print('current process {0}'.format(os.getpid()))
    p = multiprocessing.Pool(processes=5)
    for i in range(100000):
        p.apply(run_task, args=(i,))

    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All processes done!')
```
### 2、`apply_async`

```python
apply_async(func [，args [，kwds [，callback [，error_callback ] ] ] ] )
```



`apply_async 方法`用来同步执行进程，允许多个进程同时进入池子。

 异步进程池（非阻塞）,返回结果对象的方法的变体。如果指定了回调，则它应该是可调用的，它接受单个参数。当结果变为就绪时，将对其应用回调，即除非调用失败，在这种情况下将应用error_callback。如果指定了error_callback，那么它应该是一个可调用的，它接受一个参数。如果目标函数失败，则使用异常实例调用error_callback。回调应立即完成，否则处理结果的线程将被阻止。

```python
from multiprocessing import Pool
import time
def test(p):
       print(p)
       time.sleep(3)
if __name__=="__main__":
    pool = Pool(processes=2)
    for i  in range(500):
        '''
         （1）循环遍历，将500个子进程添加到进程池（相对父进程会阻塞）\n'
         （2）每次执行2个子进程，等一个子进程执行完后，立马启动新的子进程。（相对父进程不阻塞）\n'
        '''
        pool.apply_async(test, args=(i,))   #维持执行的进程总数为10，当一个进程执行完后启动一个新进程.
    print('test')
    pool.close()
    pool.join()
```

```python
import multiprocessing
import os
import time

def run_task(name):
    # print('Task {0} pid {1} is running, parent id is {2}'.format(name, os.getpid(), os.getppid()))
    # time.sleep(1)
    print('Task {0} end.'.format(name))


if __name__ == '__main__':
    print('current process {0}'.format(os.getpid()))
    p = multiprocessing.Pool(processes=2)

    for i in range(10):
        p.apply_async(run_task, args=(i,))

    print('Waiting for all subprocesses done...')
    p.close()	# 不能继续往p里面添加新的进程
    p.join()	# jion 之后，主线(主函数)程等待 p 结束之后，主线程才能结束
    print('All processes done!')
```
调用`join`之前，先调用`close`或者`terminate`方法，否则会出错。执行完`close`后不会有新的进程加入到pool,`join`函数等待所有子进程结束。
### 3、`map`

```python
map(func,iterable [,chunksize ])
```



`map()`内置函数的并行等价物（尽管它只支持一个可迭代的参数）。它会阻塞，直到结果准备就绪。此方法将`iterable`内的每一个对象作为单独的任务提交给进程池。可以通过将chunksize设置为正整数来指定这些块的（近似）大小。

```python
from multiprocessing import Pool
import time

def test(i):
    print(i)
    time.sleep(1)
    
if  __name__ == "__main__":
    lists = [x for x in  range(100)]
    pool = Pool(processes=2)       #定义最大的进程数
    pool.map(test, lists)          #lists必须是一个可迭代变量。
    pool.close()
    pool.join()
```
### 4、`map_async`

```python
map_async(func,iterable [,chunksize [,callback [,error_callback]]])
```



map()返回结果对象的方法的变体。需要传入可迭代对象iterable

```python
import time
from multiprocessing import Pool


def test(p):
    print(p)
    time.sleep(3)


if __name__ == "__main__":
    pool = Pool(processes=2)
    # for i  in range(500):
    #     '''
    #      （1）循环遍历，将500个子进程添加到进程池（相对父进程会阻塞）\n'
    #      （2）每次执行2个子进程，等一个子进程执行完后，立马启动新的子进程。（相对父进程不阻塞）\n'
    #     '''
    #     pool.apply_async(test, args=(i,))   #维持执行的进程总数为10，当一个进程执行完后启动一个新进程.
    pool.map_async(test, range(500))
    print('test')
    pool.close()
    pool.join()
```

## 三、`Queue` 用于进程通信，资源共享

允许多个进程之间

```python
class multiprocessing.Queue（[ maxsize ] ）
```

`Queue` 用来在多个进程间通信。`Queue `有两个方法，`get `和` put`。

- `put` 方法
放数据，`Queue.put( )`默认有`block=True`和`timeout`两个参数。当`block=True`时，写入是阻塞式的，阻塞时间由timeout确定。当队列q被（其他线程）写满后，这段代码就会阻塞，直至其他线程取走数据。`Queue.put（）方法`加上 block=False 的参数，即可解决这个隐蔽的问题。但要注意，非阻塞方式写队列，当队列满时会抛出 exception Queue.Full 的异常


- `get `方法
`get` 方法用来从队列中读取并删除一个元素。有两个参数可选，`blocked` 和 `timeout`
- blocked = False （默认），timeout 正值
>等待时间内，没有取到任何元素，会抛出 Queue.Empty 异常。
>Queue 有一个值可用，立刻返回改值；Queue 没有任何元素，

```python
import os
import random
import time
# 进程间的通信
from multiprocessing import Process, Queue


# 写数据进程执行的代码:
def proc_write(q, urls):
    print('Process(%s) is writing...' % os.getpid())
    for url in urls:
        q.put(url)
        print('Put %s to queue...' % url)
        time.sleep(random.random())


# 读数据进程执行的代码:
def proc_read(q):
    print('Process(%s) is reading...' % os.getpid())
    while True:
        url = q.get(True)
        print('Get %s from queue.' % url)


if __name__ == '__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()

    proc_writer1 = Process(target=proc_write, args=(q, ['url_1', 'url_2', 'url_3']))
    proc_writer2 = Process(target=proc_write, args=(q, ['url_4', 'url_5', 'url_6']))
    proc_reader = Process(target=proc_read, args=(q,))

    # 启动子进程proc_writer，写入:
    proc_writer1.start()
    proc_writer2.start()

    # 启动子进程proc_reader，读取:
    proc_reader.start()

    # 等待proc_writer结束:
    proc_writer1.join()
    proc_writer2.join()

    # proc_reader进程里是死循环，无法等待其结束，只能强行终止:
    proc_reader.terminate()
```
## 四、`Pipe` 进程间通信

(用于管道通信，用于两个进程之间的连接）

常用来在两个进程间通信，两个进程分别位于管道的两端。

如果是全双工的(构造函数参数为True)，则双端口都可接收发送，否则前面的端口用于接收，后面的端口用于发送。
```python
multiprocessing.Pipe([duplex])
```
示例 1 
```python
from multiprocessing import Process, Pipe

def send(pipe):
    pipe.send(['spam'] + [42, 'egg'])   # send 传输一个列表
    pipe.close()

if __name__ == '__main__':
    (con1, con2) = Pipe()                            # 创建两个 Pipe 实例

	 # 函数的参数，args 一定是实例化之后的 Pip 变量，不能直接写 args=(Pip(),)
    sender = Process(target=send, args=(con1, ))     
   
    sender.start()                                   # Process 类启动进程
    print("con2 got: %s" % con2.recv())              # 管道的另一端 con2 从send收到消息
    con2.close()                                     # 关闭管道
    
    # 输出：con2 got: ['spam', 42, 'egg']
```
con1管道的一端，负责存储,也可以理解为发送信息
con2管道的另一端，负责读取,也可以理解为接受信息

示例 2
管道是可以同时发送和接受消息的：
```python
from multiprocessing import Process, Pipe

def talk(pipe):
    pipe.send(dict(name='Bob', spam=42))            # 传输一个字典
    reply = pipe.recv()                             # 接收传输的数据
    print('talker got:', reply)

if __name__ == '__main__':
    (parentEnd, childEnd) = Pipe()                  # 创建两个 Pipe() 实例，也可以改成 conf1， conf2

    child = Process(target=talk, args=(childEnd,))  # 创建一个 Process 进程，名称为 child
    child.start()                                   # 启动进程

    print('parent got:', parentEnd.recv())          # parentEnd 是一个 Pip() 管道，可以接收 child Process 进程传输的数据

    parentEnd.send({x * 2 for x in 'spam'})         # parentEnd 是一个 Pip() 管道，可以使用 send 方法来传输数据
                                                    # 传输的数据被 talk 函数内的 pip 管道接收，并赋值给 reply
    child.join()
    print('parent exit')
```
## 五、`Lock`、`Rlock`进程同步
`Value`，`Array`（用于进程通信，资源共享）（还不太明白）

<font color=#D2691E size=4>不使用锁进行同步

```python
import multiprocessing
import time


def job(v, num):
    for _ in range(5):
        time.sleep(0.1)  # 暂停0.1秒，让输出效果更明显
        v.value += num   # v.value获取共享变量值
        print(v.value, end=",")


def multicore():
    v = multiprocessing.Value('i', 0)  # 定义共享变量
    p1 = multiprocessing.Process(target=job, args=(v, 1))
    p2 = multiprocessing.Process(target=job, args=(v, 3))  # 设定不同的number看如何抢夺内存
    p1.start()
    p2.start()
    p1.join()
    p2.join()


if __name__ == '__main__':
    multicore()

'''
# 进程1和进程2在相互抢着使用共享内存v
1,5,9,13,17,4,8,12,16,20,
'''

```
<font color=#D2691E size=4>使用锁进行同步

```python
import multiprocessing
import time
# lock = multiprocessing.Lock()
lock = multiprocessing.RLock()
def job(v, num,lock):
    lock.acquire()
    for _ in range(5):
        time.sleep(0.1)  # 暂停0.1秒，让输出效果更明显
        v.value += num  # v.value获取共享变量值
        print(v.value, end=",")
    lock.release()

def multicore():
    v = multiprocessing.Value('i', 0)  # 定义共享变量
    p1 = multiprocessing.Process(target=job, args=(v, 1, lock))
    p2 = multiprocessing.Process(target=job, args=(v, 3, lock))  # 设定不同的number看如何抢夺内存
    p1.start()
    p2.start()
    p1.join()
    p2.join()

'''
# 显然，进程锁保证了进程p1的完整运行，然后才进行了进程p2的运行
1,2,3,4,5,8,11,14,17,20,
'''
```

参考：

<https://blog.csdn.net/CityzenOldwang/article/details/78584175>
<https://blog.csdn.net/brucewong0516/article/details/85776194>
# python Python操作MongoDB

﻿Python 3下MongoDB的存储操作。
在开始之前，请确保已经安装好了MongoDB并启动了其服务，并且安装好了Python的PyMongo库。

## 2. 连接MongoDB
连接MongoDB时，我们需要使用PyMongo库里面`MongoClient`。一般来说，传入MongoDB的`IP`及端口即可，其中第一个参数为地址host，第二个参数为端口port（如果不给它传递参数，默认是27017）：

```python
import pymongo
client = pymongo.MongoClient(host='localhost', port=27017)
```
这样就可以创建MongoDB的连接对象了。
## 3. 指定数据库
MongoDB中可以建立多个数据库，接下来我们需要指定操作哪个数据库。这里我们以test数据库为例来说明，下一步需要在程序中指定要使用的数据库：

```python
db = client.test
```
这里调用client的test属性即可返回test数据库。当然，我们也可以这样指定：

```python
db = client['test']
```
## 4. 指定集合
MongoDB的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表。

下一步需要指定要操作的集合，这里指定一个集合名称为students。与指定数据库类似，指定集合也有两种方式：

```python
collection = db.students
```

```python
collection = db['students']
```
## 5. 插入数据
接下来，便可以插入数据了。对于students这个集合，新建一条学生数据，这条数据以字典形式表示：

```python
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}
```
这里指定了学生的学号、姓名、年龄和性别。接下来，直接调用collection的`insert()`方法即可插入数据，代码如下：

```python
result = collection.insert(student)
print(result)
```

在MongoDB中，每条数据其实都有一个`_id`属性来唯一标识。如果没有显式指明该属性，MongoDB会自动产生一个`ObjectId`类型的`_id`属性。`insert()`方法会在执行后返回`_id`值。

也可以同时插入多条数据，只需要以列表形式传递即可，示例如下：

```python
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}

result = collection.insert([student1, student2])
print(result)
```
返回结果是对应的`_id`的集合：

在PyMongo 3.x版本中，官方已经不推荐使用`insert()`方法了。当然，继续使用也没有什么问题。官方推荐使用`insert_one()`和`insert_many()`方法来分别插入单条记录和多条记录，示例如下：

```python
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

result = collection.insert_one(student)
print(result)
print(result.inserted_id)
```
调用其`inserted_id`属性获取`_id`。
对于`insert_many()`方法，我们可以将数据以列表形式传递，示例如下：

```python
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}

result = collection.insert_many([student1, student2])
print(result)
print(result.inserted_ids)
```
调用`inserted_ids`属性可以获取插入数据的`_id`列表。
<br>


## [python使用pymongo访问MongoDB的基本操作，以及CSV文件导出](https://blog.csdn.net/zwq912318834/article/details/77689568)
<br>


```python
# coding:utf-8
import pymongo


class MongoDB:
    def __init__(self,db,collections):
        """
        初始化数据库
        :param db:数据库名称 
        :param collections: 数据库的集合的名称
        """
        self.client = pymongo.MongoClient('localhost', 27017)    #获取的连接
        self.db = self.client[db]        #创建数据库db
        self.post = self.db[collections]    #创建或者选择要操作的集合


    def update(self, data,upsert):
        """
        更新数据库中的数据，如果upsert为Ture，那么当没有找到指定的数据时就直接插入，反之不执行插入
        :param data: 要插入的数据
        :param upsert: 判断是插入还是不插入
        :return: 
        """
        self.post.update({"ip": data}, {'$set': {'ip': data}} , upsert)
    def find(self,select):
        """
        根据传入的参数查找指定的值，注意这里的select是字典
        :param select: 指定的查找条件，这里的是字典类型的，比如{"name":"chenjiabing","age":22}
        :return: 返回的是查询的结果，同样是字典类型的
        """
        return self.post.find(select)

    def insert(self,data):
        """
        向数据库中插入指定的数据
        :param data: 要插入的数据，这里的是字典的类型比如：{"name":"chenjiabing","age":22}
        :return: 插入成功返回True,反之返回false
        """
        try:
            self.post.insert(data)
            return True
        except:
            return False

    def remove(self,select):
        """
        删除指定条件的记录
        :param select: 指定的条件，这里是字典类型的，比如{"age":22} 表示删除age=22的所有数据
        :return: 如果删除成功返回True，else返回False
        """
        try:
            self.post.remove(select)
            return True
        except:
            return False
```

```python
# coding:utf-8
import requests
from bs4 import BeautifulSoup
import time
from Queue import Queue
import threading
from Mongo import MongoDB   #导入文件


class XICI:
    def __init__(self, page):
        """
        self.header:请求头
        self.q:存储ip的队列
        slef.urls:页面的url
        :param page:传入的参数，表示获取多少页的ip
        """
        self.header = {"User-Agent": 'Mozilla/5.0 (Windows NT 6.3; WOW64; rv:43.0) Gecko/20100101 Firefox/43.0'}
        self.q = Queue()
        self.urls = []
        for i in range(1, page + 1):
            self.urls.append("http://www.xicidaili.com/nn/" + str(i))
        self.mongo = MongoDB('python','ip')  # 创建MogoDB对象

    def get_ips(self, url):
        """
        根据一页的请求爬取一个页面的ip
        :param url:传入的参数，表示每一页的链接
        :return: None
        """
        try:
            res = requests.get(url, headers=self.header)
            if res.status_code == 200:
                soup = BeautifulSoup(res.text, 'lxml')
                ips = soup.find_all('tr')
                for i in range(1, len(ips)):
                    ip = ips[i]
                    tds = ip.find_all("td")
                    ip_temp = "http://" + tds[1].contents[0] + ":" + tds[2].contents[0]
                    print ip_temp
                    self.q.put(ip_temp)  # ip进入队列

        except:
            print "-------------------------------------------请求出现异常------------------------------------------------"

    def insert(self, url):
        """
        验证出过来的ip，如果成功就直接存入数据库
        :param url: 验证ip地址的url
        :return: 无返回值
        """
        while not self.q.empty():
            ip = self.q.get()
            proxy = {"http": ip}
            print proxy
            try:
                res = requests.get(url, headers=self.header, proxies=proxy, timeout=5)
                if res.status_code == 200:
                    self.mongo.update(ip,True)  # 如果成功验证直接进入数据库
                    print "**************************成功存入数据库********************************************"
                else:
                    print "这个ip地址不能用"

            except:
                print "--------------------------请求失败---------------------------------------------"

    def main(self):
        for url in self.urls:
            self.get_ips(url)
        threads = []
        for i in range(5):
            t=threading.Thread(target=self.insert,args=["http://blog.csdn.net/qq_34162294/article/details/72353389"])
            threads.append(t)
        for t in threads:
            t.start()

if __name__ == '__main__':
    p = XICI(3)
    p.main()
```


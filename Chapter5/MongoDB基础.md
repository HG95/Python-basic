# python MongoDB基础

﻿MongoDB 的数据以类似于 JSON 格式的二进制文档存储：

```python
{
    name: "Angeladady",
    age: 18,
    hobbies: ["Steam", "Guitar"]
}
```
在项目目录中，使用 mongod 命令来启动 mongoDB 进程：
创建完目录之后，直接运行`mongod`命令即可启动MongoDb服务器。`mongod`命令默认使用`/data/db`为 MongoDb 数据库的数据文件目录。如果需要改变数据文件存储目录，需要指定`--dbpath`参数，例如：

```python
mongod --dbpath /Users/yurongchan/mongodb_data_file
```
类似的启动配置参数还有：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110712222844.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
连接MongoDb服务器:
服务器启动之后，我们启动一个终端连接到 MongoDb 服务器，这样我们就可以运行进行数据库CURD操作。连接MongoDb服务器命令的语法如下：

```python
mongo server_ip:port/dbname -u user -p password
```
这里直接连接本地服务器，因此直接运行`mongo`命令即可。

## MongoDb创建数据库
MongoDb 创建和切换数据库的语法格式为：

```python
use database_name
```
如果数据库不存在，则创建数据库，否则切换到指定数据库。

```python
> use chenyurong
switched to db chenyurong
> db
chenyurong
> show dbs
admin  0.000GB
local  0.000GB
```
上面创建了名为chenyurong的数据库，但是我们使用`show dbs`命令时并没有看到该数据库存在，这是因为该数据库中还没有数据。要显示它，我们需要向 chenyurong 数据库插入一些数据。
## MongoDb创建表
MongoDb 中并没有直接创建表的命令，表的数据结构在你往表插入数据时确定。因此在 MongoDb 中，你创建完数据库之后就可以直接往表中插入数据，表名在插入数据时指定。

## 常用shell操作
- `db` 显示当前所在数据库

- `show dbs` 列出可用数据库

- `show tables` or  `show collections` 列出数据库中可用集合

- `use <database>` 用于切换数据库

## [MongoDb插入数据](https://docs.mongodb.com/manual/reference/insert-methods/)
MongoDB 使用 `insert()` 或` save() `方法向集合中插入文档
- `db.collection.inserOne()` 插入单个文档
- `db.collection.inserMany()` 插入多个文档
- `db.collection.insert()` 插入单条或多条文档
```python
db.collection.insert()


db.collection.insert(
   <document or array of documents>,
   {
     writeConcern: <document>,
     ordered: <boolean>
   }
)
```

```python
_id Field

If the document does not specify an _id field, then
 MongoDB will add the _id field and assign a unique
  ObjectId for the document before inserting. Most 
  drivers create an ObjectId and insert the _id field, 
  but the mongod will create and populate the _id if 
  the driver or application does not.
```
## [MongoDb查询数据](https://docs.mongodb.com/manual/reference/method/db.collection.find/)

```python
db.collection.find()

db.collection.find(query, projection)



返回查询值的列表
```
- `query`（可选）：使用查询操作符指定查询条件。该参数是一个JSON对象，key 一般为查询的列名，value 为查询匹配的值。
- `projection`（可选）：使用投影操作符指定返回的键。如果省略该参数，那么查询时返回文档中所有键值。该参数是一个JSON对象，key 为需要显示的列名，value 为 1（显示） 或 0（不显示）。` _id`:默认就是1，没指定返回该字段时，默认会返回，除非设置为0是，就不会返回该字段。


第一个参数为查询条件：

```python
> db.drivers.find() #查找所有文档
{ "_id" : ObjectId("598964bd56b8c69ae1e5f36a"), "name" : "Chen1fa", "age" : 18 }
{ "_id" : ObjectId("598964d456b8c69ae1e5f36b"), "name" : "Xiaose", "age" : 35 }
{ "_id" : 91, "name" : "Sun1feng", "age" : 34 }

> db.drivers.find({name: "Xiaose"}) #查找 name 为 Xiaose 的文档
{ "_id" : ObjectId("598964d456b8c69ae1e5f36b"), "name" : "Xiaose", "age" : 35 }

> db.drivers.find({age:{$gt:20}}) #查找 age 大于 20 的文档
{ "_id" : ObjectId("598964d456b8c69ae1e5f36b"), "name" : "Xiaose", "age" : 35 }
{ "_id" : 91, "name" : "Sun1feng", "age" : 34 }
```
上述代码中的`$gt`对应于大于号>的转义。

第二个参数可以传入投影  ,文档映射数据：

```python
> db.drivers.find({age:{$gt:20}},{name:1})
{ "_id" : ObjectId("598964d456b8c69ae1e5f36b"), "name" : "Xiaose" }
{ "_id" : 91, "name" : "Sun1feng" }

如果不 想显示'_id'
 db.drivers.find({age:{$gt:20}},{name:1,_id:0})
```
投影文档中字段为 1 或真值表示包含，0 或假值表示排除，可以设置多个字段为 1 或 0，但不能混合使用。

除此之外，还可以通过 `count`、`skip`、`limit` 等指针（Cursor）方法，改变文档查询的执行方式：

```python
db.drivers.find().count() #统计查询文档数目
3
> db.drivers.find().skip(1).limit(10).sort({age:1})
{ "_id" : 91, "name" : "Sun1feng", "age" : 34 }
{ "_id" : ObjectId("598964d456b8c69ae1e5f36b"), "name" : "Xiaose", "age" : 35 }
```
上述查找命令跳过 1 个文档，限制输出 10 个，以 name 子段正序排序（大于 0 为正序，小于 0 位反序）输出结果。最后，可以使用 `Cursor` 方法中的` pretty `方法，提升查询文档的易读性，特别是在查看嵌套的文档和配置文件的时候：

```python
MongoDB Enterprise > db.test.find().pretty()
{
        "_id" : ObjectId("5dc2791643fecd3d34021152"),
        "id" : "20170101",
        "name" : "Jordan",
        "age" : 20,
        "gender" : "male"
}
{
        "_id" : ObjectId("5dc27af23b8c62878c1061e4"),
        "id" : "20170101",
        "name" : "Jordan",
        "age" : 20,
        "gender" : "male"
}
{
        "_id" : ObjectId("5dc27af23b8c62878c1061e5"),
        "id" : "20170202",
        "name" : "Mike",
        "age" : 21,
        "gender" : "male"
}
```
查询第一条数据
```python
db.collection.findOne()
```
## [范围操作符](https://blog.csdn.net/congcong68/article/details/46841075)
范围操作符指的是：`大于`、`大于等于`、`等于`、`不等于`、`小于`、`小于等于`操作符，在 MongoDb 中它们的表示以及使用如下面表格所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110713390830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
例如我要查询用户表中所有年龄大于等于25岁的用户，那么查询语句为：

```python
db.user.find({"age": {$gte:25}},{"_id":0}).pretty()
```
<font color=#D2691E size=4> AND操作符 </font>

MongoDB 的 `find()` 方法可以传入多个键（key），每个键（key）以逗号隔开。每个键（key）之间是与的逻辑关系。

```python
 { $and: [ { <expression1> }, { <expression2> } , ... , ]}
```

例如我要查询用户表（user）中地址为ShenZhen且年龄大于等于25岁的用户，那么查询语句为：

```python
db.user.find({"addr": "ShenZhen","age": {$gte:25}},{"_id":0}).pretty()
```
<font color=#D2691E size=4>OR操作符</font>

MongoDB 中关键字`$or`表示或逻辑关系，其语法格式如下：

```python
语法：  

       { $nor: [ { <expression1> }, { <expression2> }, ... ] }
```

```python
db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```
例如我要查询用户表（user）中地址为ShenZhen或者年龄大于等于30岁的用户，那么查询语句为：

```python
db.user.find({$or:[{"addr":"ShenZhen"},{"age":{$gte:30}}]}).pretty()
```
AND操作符和OR操作符可以混合使用，例如要实现以下SQL查询：

```python
select * from user
where name = "ChenYuRong" or (age <= 25 and addr == "JieYang")
```
那么该 MongoDb 查询语句应该这样写：

```python
db.user.find({$or:[{"name":"ChenYuRong"}, {"age": {$lte:25}, "addr": "JieYang"}]}).pretty()
```


<font color=#D2691E size=4>`$in`（包含）、`$nin`（不包含）条件查询

<font color=#D2691E size=3>1）`$in`（包含）条件查询

```python
 语法：            
 { field: { $in: [<value1>, < value2>, ...] } }


 >db.orders.find({"onumber":{$in:["001","002"]}})
```
查询onumber in("001","002") 条件的文档，就是onumber等于001或者等于002 这个跟`$or`有点像，不过`$or`做为条件查询时，可以指定不同的字段: ` db.orders.find({$or:[{"onumber":"002"},{"cname":"zcy1"}]})`
，而`$in`只针对一个字段。

<font color=#D2691E size=3> 2）`$nin`（不包含）条件查询

```python
语法：             
{ field: { $nin: [<value1>, < value2>, ...] } }
```
<font color=#D2691E size=4> `$not`(不等于) 条件查询

```python
语法：
{ field: { $not: { < expression1> } } }
```
` $not`操作符不能独立使用，必须跟其他操作条件一起使用（除`$regex`）

```python
>db.orders.find({"onumber":{$not:{$gt:"002"}}})
```
 查找onumber不等于大于002的文档数据

<font color=#D2691E size=4> `$exists`用来判断一个field是否存在

```python
语法：
{ field: { $ exists:  < boolean>  } }
```

```python
  >db.orders.find({"age":{$exists:true}})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107174253558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
 没有age这个元素，什么都没返回
 插入有age元素，在执行一下
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107174326625.png)

## [对数组根据条件查询](https://blog.csdn.net/congcong68/article/details/46919227)

```python
 $all、$size、$slice、$elemMatch
```

```python
s=[
    {
        'onumber':'008',
        'date':'2015-07-08',
        'cname':'zcy8',
        'books':['java','c','mongo']
    },
    {
        'onumber': '009',
        'date': '2015-07-09',
        'cname': 'zcy8',
        'books': ['java', 'c']
    }
]
```

（1）`$all`查找数组中包含指定的值的文档

```python
语法：
{ field:{ $all: [ <value> , <value1> ... ]}
```

```python
 db.orders.find({"books":{$all:["java","mongo"]}})
 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107200319219.png)
查找books包含java、mongo的文档数据

（2）`$size` 查找数组大小等于指定值的文档

```python
语法：
{field: {$size: number } }
```

```python
>db.orders.find({"books":{$size:2}})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107201120267.png)
（3）`$slice`查询数组中指定返回元素的个数

```python
 语法：
 >db.collect.find({},{field:{$slice: number }})
number 说明：
为正数表示返回前面指定的值的个数：例如1 返回数组第一个
为负数表示返回倒数指定的值的个数：例如-1返回数组倒数第一个

```

```python
 >db.orders.find({"onumber":{$in:["008","009"]}},{books:{$slice:1}})
```
  `$slice`可以查询数组中第几个到第几个


```python
语法：
>db.collect.find({},{field:{$slice:[ number1, number2] }})
跳过数组的number1个位置然后返回number2个数
number1说明：
为正数表示跳到指定值的数组个数：例如2 跳到数组第3个
为负数表示跳到指定值的数组倒数个数：例如-2跳到到数组倒数第3个
```

```python
>db.orders.find({"onumber":{$in:["008","009"]}},{books:{$slice:[1,1]}})
```
## [对数组内嵌文档查询](https://blog.csdn.net/congcong68/article/details/46919227)

```python
db. orders.insert([
{
        "onumber" : "001", 
        "date" : "2015-07-02", 
        "cname" : "zcy1", 
         "items" :[ {
                   "ino" : "001",
                  "quantity" :2, 
                  "price" : 4.0
                 },{
                   "ino" : "002",
                  "quantity" : 4, 
                  "price" : 6.0
                }
                ]
},{
         "onumber" : "002", 
        "date" : "2015-07-02", 
        "cname" : "zcy2", 
         "items" :[ {
                  "ino" : "001",
                  "quantity" :2, 
                  "price" : 4.0
                   },{
                  "ino" : "002",
                  "quantity" :6, 
                  "price" : 6.0
                 }
               ]
}

```
（1）`$elemMatch` 文档包含有一个元素是数组，那么$elemMatch可以匹配内数组内的元素并返回文档数据

```python
 语法：
>{field:{$elemMatch:{ field1:value1, field2:value2,………}}}
```

```python
  >db.orders.find({"items":{$elemMatch:{"quantity":2}}})
```
  返回quantity为2的文档

```python
也可以这样查询db.orders.find({"items.quantity":2})
```
（2） `$elemMatch`可以带多个查询条件

```python
 >db.orders.find({"items":{$elemMatch:{"quantity":4,"ino":"002"}}})
```
 我们查询数组中的quantity等于4并且ino等于002，但是我们就想返回数组中的quantity等于4并且ino等于002的这个文档，并不想把ino等于001等这些无关的文档返回。

 （3）`$elemMatch` 同样可以用在find方法的第二个参数来限制返回数组内的元素，只返回我们需要的文档


```python
db.orders.find({"onumber":"001"},
{"items":{$elemMatch:{"quantity":4,"ino":"002"}},"cname":1,"date":1,"onumber":1})
```
我们只返回quantity等于4并且ino等于002的文档，无关的文档没有返回，方便我们处理数据，这样也可以节省传输数据量，减少了内存消耗，提高了性能，在数据大时，性能很明显的。
## [游标的操作](https://blog.csdn.net/congcong68/article/details/46933609)
我们还可以对文档的游标，可以随意修改返回结果的限制、跳跃、和排序顺序的功能。
1. `limit`
`  limit方法`是限制游标返回结果的数量，如下面例子：

	```python
	>db.items.find().limit(5)
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107203905397.png)
	只返回结果5条文档

2.  `sort()`排序
	在 MongoDB 中使用使用 `sort() 方法`对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列。
	sort()方法基本语法如下所示：

	```python
	db.collection.find().sort({KEY:1})
	```
	其中`KEY`表示要进行排序的字段。
	例如我们将所有年龄小于30岁的用户查询出来并将其按照年龄升序排列：
	
	```python
	db.user.find({"age":{$lt:30}}).sort({age:1}).pretty()
	```
		 我们可以指定多个字段排序，例如我们先按quantity字段降序，然后在按info升序,如下面的例子：
	
	
	```python
	 >db.items.find({"ino":{$lt:5}}).sort({"quantity":-1,"info":1})
	```
	我们查询ino小于5，并对这结果进行排序，按quantity字段降序
	我们还可以跟limit()方法进行组合查询并对结果进行排序
	
	```python
	 >db.items.find().limit(3).sort({"quantity":1})
	```
3. `skip`
   skip方法可以跳过指定值的条数，返回剩下的条数的结果，可以跟limit()方法进行组合可以`实现分页的效果`。

	```python
	db.items.find().skip(10).limit(10)
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107204313767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
	  跳过第10条，从第11条开始返回，只返回10条文档.
	  skip方法是跳过条数，而且是一条一条的跳过，如果集合比较大时（如书页数很多）skip会越来越慢, 需要更多的处理器(CPU)，这会影响性能。
	  我们可以通过一个键值比较有顺序的来进行分页，这样就避免使用skip方法。

	```python
	>db.items.find({"ino":{$lt:20,$gt:9}}).sort({"info":1})
	```

## 更新（Update)
MongoDB 提供 updata 方法更新文档：
- `db.collection.updateOne()` 更新最多一个符合条件的文档

- `db.collection.updateMany()` 更新所有符合条件的文档

- `db.collection.replaceOne() `替代最多一个符合条件的文档

- `db.collection.update()` 默认更新一个文档，可配置 multi 参数，跟新多个文档

update() 方法用于更新已存在的文档。语法格式如下：

```python
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
- `query`：为查询条件
- `update`：为修改的文档。
- upsert（可选）：如果不存在update的记录，是否将其作为记录插入。true为插入，默认是false，不插入。
- `multi`（可选）：是否更新多条记录。MongoDb 默认是false，只更新找到的第一条记录。如果这个参数为true,就把按条件查出来多条记录全部更新。
- `writeConcern`（可选）：表示抛出异常的级别

下面的命令将 name 字段为 Chen1fa 的文档，更新 age 字段为 30：

```python
> db.drivers.update({name:"Chen1fa"},{name:"Chen1fa", age:30})
```
要注意的是，如果更新文档只传入 age 字段，那么文档会被更新为{age: 30}，而不是{name:"Chen1fa", age:30}。要避免文档被覆盖，需要用到` $set `指令，`$set `仅替换或添加指定字段：

```python
> db.drivers.update({name:"Chen1fa"},{$set:{age:30}})
```
如果要在查询的文档不存在的时候插入文档，要把 `upsert 参数`设置真值：

```python
> db.drivers.update({name:"Alen"},{age:24},{upsert: true})
```
update 方法默认情况只更新一个文档，如果要更新符合条件的所有文档，要把 `multi `设为真值，并使用` $set` 指令：

```python
> db.drivers.update({age:{$gt:25}},{$set:{license:'A'}},{multi: true})
> db.drivers.update({age:{$lt:25}},{$set:{license:'C'}},{multi: true})
```
如果不想传入`multi`参数，可以用
[db.collection.updateOne()](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/)
[db.collection.updateMany()](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/)
## 对单个字段进行修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107160310359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
<font color=#6495ED size=4>2) 采用`$set`来根据查询条件修改文档，用来指定一个键的值，如果不存在则创建它。

```python
> db.orders.update(                          
   {"onumber" : "001"},
   { $set: { "cname " : "zcy"} },
   false,
   true
)
```
`multi `设置为`true`，全部更新

<font color=#6495ED size=4>3） `$mul ` 将该字段的值乘以指定的值

```python
语法：

{ $mul: { field: <number> } }

>db. orders.update(                          
	{"ino" : "001"},
	{ $mul: {"quantity" :3} }
)

修改之前：
>db. orders.insert({
                  "ino" : "001",
                  "quantity": 2, 
                  "price" : 4.0
}

```
修改之后：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107161122945.png)
<font color=#6495ED size=4> 4）`$setOnInsert `    操作时,操作给相应的字段赋值

```python
语法：

db.collection.update(
  <query>,
   {$setOnInsert: { <field1>: <value1>, ... } },
   {upsert: true }
)
```
## 对数组进行修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107161418773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
（1）根据查询条件修改文档里内嵌文档（第二层级的），例如我们想修改items 字段ino为001下的price的4修改8,语法items.$. price ，更新数组中第一个匹配的子文档，我们内嵌文档的ino是唯一的，满足我们的需求

```python
>db. orders.update(                          
	{"onumber" : "001","items.ino":"001"},
	{ $set: {"items.$.price" : 8.0} }
)
```
修改前的数据：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107164752619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
修改后的数据：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107164833603.png)
（2）根据查询条件修改文档里内嵌文档在内嵌文档（第三层级的），例如我们想修改items 字段ino等于001下的products并且pno等于001的pName值为ps,语法`items.0. products.$. pName`,0代表items第一个数组（也就是数组的下标），`$` 更新数组中第一个匹配的子文档。

```python
>db. orders.update(                          
	{"onumber" : "001","items.ino":"001","items.products.pno":"001"},
	{ $set: {"items.0.products.$.pName": "ps"} }
)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107165047378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191107165120493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hIRzIwMTcxMjI2,size_16,color_FFFFFF,t_70)
<font color=#6495ED size=4>（3）`$pop`删除数组的第一个或最后一个项

```python
 语法：
{ $pop: { <field>: <-1 | 1>,... } }

1最后一项

-1是第一项
```

```python
>db. orders.update(                          
	{"onumber" : "001"},
	{ $pop: {"items" : -1} }
)
```

<font color=#6495ED size=4>（4）`$push`将值添加到数组中，如果有的数组存在则向数组末尾添加该值，如果数组不存在则创建该数组并保存该值

```python
 语法：
 { $push: { <field1>: <value1>,... } }

>db. orders.update(                          
{"onumber" : "001"},
{ $push: {"items" : {
					  "ino" : "002",
	                  "quantity" :2, 
	                  "price" : 6.0, 
	                  "products" : [
										{
										   "pno":"003",
										   "pName":"p3"
										},
										{
										   "pno":"004",
										  "pName":"p4"
										}
									]
						}
				} }
)
```


## MongoDb删除数据
MongoDB 提供了 delete 方法删除文档：
- `db.collection.deleteOne() `删除最多一个符合条件的文档

- `db.collection.deleteMany() `删除所有符合条件的文档

- `db.collection.remove()` 删除一个或多个文档

## [MongoDB 聚合Group](https://blog.csdn.net/congcong68/article/details/45012717)





## 关闭实例
关闭 mongoDB 服务：
```python
> use admin
> db.shutdownServer()
```
使用 exit 或 Ctrl + C 断开连接:

```python
> exit
```

参考：
[MongoDb 快速入门教程](https://www.cnblogs.com/chanshuyi/p/quick_start_of_mongodb.html)
[简明 MongoDB 入门教程](https://segmentfault.com/a/1190000010556670)
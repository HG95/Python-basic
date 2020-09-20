# Python JsonPath模块

## jsonpath介绍

用来解析多层嵌套的json数据;JsonPath 是一种信息抽取类库，是从JSON文档中抽取指定信息的工具，提供多种语言实现版本，包括：Javascript, Python， PHP 和 Java。

jsonpath表达式总是和JSON结构结合在一起的 就如同 XML文档和XPath结合在一起一样。“根元素对象”在JsonPath中被称为`$` 无论它是一个对象或数组。





## 使用方法

```python
import jsonpath
response = json.loads(reqs)
jpid = jsonpath.jsonpath(response, '$..key_name')
```

其中：“$”表示最外层的{}，“..”表示模糊匹配,当传入不存在的key_name时,程序会返回false

JsonPath表达式可以使用 **点号**
`$.store.book[0].title`
或使用 **中括号**
`$['store']['book'][0]['title']`

### 操作符

**`n/a`表`not applicable`不适用**

| JsonPath           | XPath | 描述                                                         |
| :----------------- | :---- | :----------------------------------------------------------- |
| `$`                | `/`   | 根节点                                                       |
| `@`                | `.`   | 过滤器断言（filter predicate）处理的 **当前节点对象**，类似于this |
| `*`                | `*`   | 通配符，匹配所有的元素                                       |
| `..`               | `//`  | 递归搜索,不管位置，选择所有符合条件的条件                    |
| `.`or`[]`          | `/`   | 子节点                                                       |
| `[]`               | `[]`  | 迭代器标示（可以在里边做简单的迭代操作，如数组下标，根据内容选值等） |
| `[,]`              | `l`   | 连接操作符在XPath结果合并其它结点集合。JsonPath允许name或者数组索引 |
| `?()`              | `[]`  | 应用过滤表示式,可进行过滤操作                                |
| `[start:end:step]` | `n/a` | 数组分割操作，XPath不支持                                    |
| `()`               | `n/a` | 脚本表达式，使用在脚本引擎下面,XPath不支持                   |
| `n/a`              | `@`   | 属性访问字符,JsonPath不支持                                  |
| `n/a`              | `..`  | 父元素,JsonPath不支持                                        |
| `n/a`              | `()`  | Xpath分组,JsonPath不支持                                     |

**注意**：

- [ ]在xpath表达式总是从前面的路径来操作数组，索引是从1开始。
- 使用JOSNPath的[]操作符操作一个对象或者数组，索引是从0开始。

### 函数

函数可以在路径尾部调用——函数的输入是路径表达式的输出。函数的输出取决于函数本身

| 函数       | 描述                     | 输出    |
| :--------- | :----------------------- | :------ |
| `min()`    | 获取**数字数组**的最小值 | Double  |
| `max()`    | 获取**数字数组**的最大值 | Double  |
| `avg()`    | 获取**数字数组**的平均值 | Double  |
| `stddev()` | 获取**数字数组**的标准差 | Double  |
| `length()` | 获取**数组**的长度       | Integer |

### 过滤运算符

过滤器是用于筛选数组的逻辑表达式。`[?(@.age > 18)]`是一个典型的过滤器，其中`@`代表当前被操作的节点。更多复杂的过滤语句可以通过`&&`和`||`创建出来。字符必须被但双引号闭合关闭 `([?(@.color == 'blue')]` 或`[?(@.color == "blue")])`。

| Operator | Description                                             |
| :------: | :------------------------------------------------------ |
|    ==    | 等于（注意`1`不等于`'1'`）                              |
|    !=    | 不等于                                                  |
|    <     | 小于                                                    |
|    <=    | 小于等于                                                |
|    >     | 大于                                                    |
|    >=    | 大于等于                                                |
|    =~    | 匹配正则表达式`[?(@.name =~ /foo.*?/i)]`                |
|    in    | 左值存在于右边`[?(@.size in ['S', 'M'])]`               |
|   nin    | 左值不存在于右边                                        |
| subsetof | 左值是右边的子集`[?(@.sizes subsetof ['S', 'M', 'L'])]` |
|   size   | 左值（数组或字符串）的长度与右值相同                    |
|  empty   | 左值(数组或字符串)为空                                  |

### 范例

```json
{
    "store": {
        "book": [
            {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
            {
                "category": "fiction",
                "author": "J. R. R. Tolkien",
                "title": "The Lord of the Rings",
                "isbn": "0-395-19395-8",
                "price": 22.99
            }
        ],
        "bicycle": {
            "color": "red",
            "price": 19.95
        }
    },
    "expensive": 10
}
```

| JsonPath表达式                          | 结果                                                 |
| :-------------------------------------- | :--------------------------------------------------- |
| `$.store.book[*].author`                | 所有`book`的`author`                                 |
| `$..author`                             | 所有`author`(递归搜索)                               |
| `$.store.*`                             | `store`下的所有子节点包括(`book`和`bicycle`)         |
| `$.store..price`                        | `store`下的所有`price`(递归搜索)                     |
| `$..book[2]`                            | 第三个`book`                                         |
| `$..book[-2]`                           | 倒数的第二个`book`                                   |
| `$..book[0,1]`                          | 第一、二个`book`                                     |
| `$..book[:2]`                           | 索引0到2(**不含2**)的所有`book`                      |
| `$..book[1:2]`                          | 索引1到2(**不含2**)的所有`book`                      |
| `$..book[-2:]`                          | 索引-2到0(**不含0**)的所有`book`                     |
| `$..book[2:]`                           | 索引2到末尾的所有`book`                              |
| `$..book[?(@.isbn)]`                    | 带有`isbn`的所有`book`                               |
| `$.store.book[?(@.price < 10)]`         | `price`少于10的所有`book`                            |
| `$..book[?(@.price <= $['expensive'])]` | 价格少于`expensive`的所有`book`                      |
| `$..book[?(@.author =~ /.*REES/i)]`     | `book`中`author`以`REES`结尾的所有值（不区分大小写） |
| `$..*`                                  | 逐层列出json中的所有值，层级由外到内                 |
| `$..book.length()`                      | `book`数组的长度                                     |

```json
{
	"store": {
		"book": [{
				"category": "reference",
				"author": "Nigel Rees",
				"title": "Sayings of the Century",
				"price": 8.95
			}, {
				"category": "fiction",
				"author": "Evelyn Waugh",
				"title": "Sword of Honour",
				"price": 12.99
			}, {
				"category": "fiction",
				"author": "Herman Melville",
				"title": "Moby Dick",
				"isbn": "0-553-21311-3",
				"price": 8.99
			}, {
				"category": "fiction",
				"author": "J. R. R. Tolkien",
				"title": "The Lord of the Rings",
				"isbn": "0-395-19395-8",
				"price": 22.99
			}
		],
		"bicycle": {
			"color": "red",
			"price": 19.95
		}
	}
}
```

| XPath                  | JsonPath                                   | Result                                   |
| ---------------------- | ------------------------------------------ | ---------------------------------------- |
| `/store/book/author`   | `$.store.book[*].author`                   | 所有book的author节点                     |
| `//author`             | `$..author`                                | 所有author节点                           |
| `/store/*`             | `$.store.*`                                | store下的所有节点，book数组和bicycle节点 |
| `/store//price`        | `$.store..price`                           | store下的所有price节点                   |
| `//book[3]`            | `$..book[2]`                               | 匹配第3个book节点                        |
| `//book[last()]`       | `$..book[(@.length-1)]`，或 `$..book[-1:]` | 匹配倒数第1个book节点                    |
| `//book[position()<3]` | `$..book[0,1]`，或 `$..book[:2]`           | 匹配前两个book节点                       |
| `//book[isbn]`         | `$..book[?(@.isbn)]`                       | 过滤含isbn字段的节点                     |
| `//book[price<10]`     | `$..book[?(@.price<10)]`                   | 过滤`price<10`的节点                     |
| `//*`                  | `$..*`                                     | 递归匹配所有子节点                       |









参考

- 官方文档：[http://goessner.net/articles/JsonPath](https://link.jianshu.com/?t=http%3A%2F%2Fgoessner.net%2Farticles%2FJsonPath) 
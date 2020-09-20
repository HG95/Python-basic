# Python list中append和extend函数区别

`append` 和 `extend` 都是 python 内置函数，都有扩展列表的元素功能，但两者的扩展方式是不同的。

通过使用 `?list.append` 命令查看append函数帮助文档

```python
?list.append
Docstring: L.append(object) -> None -- append object to end
Type:      method_descriptor
```

通过 `?list.extend` 命令查看extend函数帮助文档

```python
?list.extend
Docstring: L.extend(iterable) -> None -- extend list by appending elements from the iterable
Type:      method_descriptor
```

可以看出两者区别：**append函数直接将object整体当作一个元素追加到列表中，而extend函数则是将可迭代对象中的元素逐个追加到列表中**。

```python
In [1]: lt1=['A','B','C']
   ...: lt2=['D','E','F']
   ...: lt1.append(lt2)#将lt2整体当作一个元素追加到到lt1中
   ...: print(lt1)
   ...: lt3=['A','B','C']
   ...: lt2=['D','E','F']
   ...: lt3.extend(lt2)#将lt2中每个元素逐个追加到t3中
   ...: print(lt3)
   ...:
['A', 'B', 'C', ['D', 'E', 'F']]
['A', 'B', 'C', 'D', 'E', 'F']
```


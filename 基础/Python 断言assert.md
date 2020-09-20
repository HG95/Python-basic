# Python 断言assert

- 使用assert断言是个好习惯
- 在没完善一个程序之前，我们不知道程序在哪里会出错，与其让它在运行最崩溃，不如在出现错误条件时就崩溃，这时候就需要assert断言的帮助。
- assert异常参数：在断言表达式后添加字符串信息，用来解释断言并更好的知道是哪里出了问题
- assert用法
  assert 表达式 [, 异常提示字符串]
- 示例如下：
  assert type(users) == list #断言users对象的类型为list
  assert len(users) >5 #断言users对象的元素个数大于5
- assert异常时，会报AssertionError错误，其后会带上断言时的异常参数，这样方便排查出问题的地方。

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503103901.png"/>


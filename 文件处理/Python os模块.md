# Python os模块

os 模块是与操作系统交互的一个接口

- `os.getcwd()`获取当前工作目录，即当前python脚本工作的目录路径

```python
print(os.getcwd())
输出：F:\CodeFile\16\21

```

- `os.chdir("dirname")` 改变当前脚本工作目录；相当于shell下cd
  <img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503224252.png"/>

```python
print(os.getcwd())
os.chdir('211')
print(os.getcwd())
输出：
F:\CodeFile\16\21
F:\CodeFile\16\21\211

```

- `os.makedirs('dirname1/dirname2')` 可生成多层递归目录

```python
os.makedirs('dir1/dir11')

```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503224420.png"/>

- `os.mkdir('dirname')` 生成单级目录；相当于shell中mkdir dirname

```python
import  random,os
print(os.getcwd())
os.chdir('211')
print(os.getcwd())
os.mkdir("100")

```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503224635.png"/>

- `os.rmdir(‘dirname’)` 删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir - dirname

- `os.listdir('dirname')` 列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印

```python
import  os
print(os.getcwd())
print(os.listdir())
输出：['01.py', '02.py', '211']

```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503224744.png"/>

- `os.rename("oldname","newname")` 重命名文件/目录

- `os.removedirs('dirname1')` 若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推

- `os.remove()` 删除一个文件

- `os.stat('path/filename')` 获取文件/目录信息

- `os.path.abspath(path)` 返回path规范化的绝对路径

- `os.path.split(path)` 将path分割成目录和文件名二元组返回

- `os.path.dirname(path)  `  返回 path 的目录。其实就是 os.path.split(path) 的第一个元素

- `os.path.basename(path) `返回path最后的文件名，如果path以／或\结尾，那么就会返回空值。即os.path.split(path)的第二个元素

  ```python
  print(os.path.split(r'F:\CodeFile\16\21\02.py'))
  print(os.path.dirname(r'F:\CodeFile\16\21\02.py'))
  print(os.path.basename(r'F:\CodeFile\16\21\02.py'))
  输出：
  ('F:\\CodeFile\\16\\21', '02.py')
  F:\CodeFile\16\21
  02.py
  
  ```

- `os.path.splitext()` 分离扩展名：

```python
os.path.splitext('poem.txt')        --->('poem', '.txt')

os.path.splitext('/home/swaroop/byte/code/poem.txt')    
 --->('/home/swaroop/byte/code/poem', '.txt')

```

- `os.path.getsize（filename）` 获取文件大小

-  `os.path.exists(path)` 如果path存在，返回True；如果path不存在，返回False
- `os.path.isabs(path)` 如果path是绝对路径，返回True
- `os.path.isfile(path)` 如果path是一个存在的文件，返回True。否则返回False
- `os.path.isdir(path)` 如果path是一个存在的目录，则返回True。否则返回False

```python
>>> os.path.exists('C:\\Windows')
True
>>> os.path.exists('C:\\some_made_up_folder')
False
>>> os.path.isdir('C:\\Windows\\System32')
True
>>> os.path.isfile('C:\\Windows\\System32')
False

```



- `os.path.join(path1[, path2[, ...]]) `将多个路径组合后返回，第一个绝对路径之前的参数将被忽略
- `os.path.getatime(path)` 返回path所指向的文件或者目录的最后存取时间
- `os.path.getmtime(path) `返回path所指向的文件或者目录的最后修改时间





案例

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503225142.png"/>

```python
#01.py
import  os,sys

PATH=os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.append(PATH)

print(os.path.abspath(__file__))
#打印F:\CodeFile\Python1\22\conf\01.py

print(os.path.dirname(os.path.abspath(__file__)))
#打印F:\CodeFile\Python1\22\conf

print(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
#打印F:\CodeFile\Python1\22

from  core import lo  #导入模块

lo.pp()
#打印hhg

```

```python
import re
import os
import time
#str.split(string)分割字符串
#'连接符'.join(list) 将列表组成字符串
def change_name(path):
    global i
    if not os.path.isdir(path) and not os.path.isfile(path):
        return False
    if os.path.isfile(path):
        file_path = os.path.split(path) #分割出目录与文件
        lists = file_path[1].split('.') #分割出文件与文件扩展名
        file_ext = lists[-1] #取出后缀名(列表切片操作)
        img_ext = ['bmp','jpeg','gif','psd','png','jpg']
        if file_ext in img_ext:
            os.rename(path,file_path[0]+'/'+lists[0]+'_fc.'+file_ext)
            i+=1 #注意这里的i是一个陷阱
        #或者
        #img_ext = 'bmp|jpeg|gif|psd|png|jpg'
        #if file_ext in img_ext:
        #    print('ok---'+file_ext)
    elif os.path.isdir(path):
        for x in os.listdir(path):
            change_name(os.path.join(path,x)) #os.path.join()在路径处理上很有用
img_dir = 'D:\\xx\\xx\\images'
img_dir = img_dir.replace('\\','/')
start = time.time()
i = 0
change_name(img_dir)
c = time.time() - start
print('程序运行耗时:%0.2f'%(c))
print('总共处理了 %s 张图片'%(i))

```


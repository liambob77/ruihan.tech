# Python Notes

---

函数内代码执行效率更高

## Python 修改文件

```python
文件打开模式
# t: Text模式, 默认模式
# b: 二进制模式， 比如图片
打开一个文件，如果文件不存在则创建文件
# w：以TEXT写方式打开，
# a：以TEXT追加模式打开 (从 EOF 开始, 必要时创建新文件)
# r+：以TEXT读写模式打开，保留文件原内容，文件操作标记指向文件头
# w+：以读写模式打开，删除文件原内容，只能读后面写的
# a+：以读写模式打开，保留文件原内容，文件操作标记指向文件尾
创建一个文件，如果文件存在则报错
# x: 文件存在就报错

fp = open("test.txt", 'w')
fp.read([size]) # size为读取的长度，以byte为单位
fp.readline([size]) #读一行，如果定义了size，有可能返回的只是一行的一部分
fp.readlines([size])
fp.write(str) # 把str写到文件中，write()并不会在str后加上一个换行符
fp.writelines(seq)
fp.flush() # 把缓冲区的内容写入硬盘
fp.fileno() # 返回一个长整型的"文件标签"
fp.isatty() # 文件是否是一个终端设备文件(unix系统中的)
fp.tell() # 返回文件操作标记的当前位置，以文件的开头为原点
fp.next() # 返回下一行
fp.seek(offset[,whence]) # 将文件打操作标记移到offset的位置

文件目录操作，下面操作都需要引入os模块
import os
os.remove("file.txt") # 删除文件
os.path.exists("demofile.txt") # 检查文件是否处在
os.rmdir("myfolder") # 删除文件夹
```

## Python `format` 字符串格式化

```python
使用位置参数
li = ['hoho',18]
'my name is {} ,age {}'.format(*li)
'my name is hoho ,age 18'
'my name is {1} ,age {0} {1}'.format(10,'hoho')
'my name is hoho ,age 10 hoho'
使用关键字参数
hash = {'name':'hoho','age':18}
'my name is {name},age is {age}'.format(name='hoho',age=19)
'my name is hoho,age is 19'
'my name is {name},age is {age}'.format(**hash)
'my name is hoho,age is 18'
填充与格式化
'{0:>10}'.format(5)  # 右对齐
'        5'
'{0:<10}'.format(5)  # 左对齐
'5        '
'{0:^10}'.format(5)  # 居中对齐
'    5    '
'{0:*^10}'.format(5)  # 居中对齐，填充*
'****5****'
正负数显示
'{:-} and {:-}'.format(-3, 7) # 正数不显示加号
'-3 and 7'
'{:-} and {:-}'.format(-3, 7) # 正数显示加号
'-3 and +7'
'{: } and {: }'.format(-3, 7) # 正数前面加空格
'-3 and  7'
精度与进制
'{0:.2f}'.format(1/3) # 浮点数 精度格式化, 两位精度
'0.33'
x = float('inf') # 大写浮点数格式化inf 和 nan 显示成 INF 和 NAN
'{:F}'.format(x)
INF
'{0:b}'.format(10)    # 整数 二进制格式化
1010
'{0:o}'.format(10)     # 整数 八进制格式化
12
'{0:x}'.format(10)     # 整数 16进制格式化, 小写字母
a
'{0:X}'.format(10)     # 整数 16进制格式化, 大写字母
A
'{0:e}'.format(500)     # 科学记数 小写e
5.000000e+02
'{0:E}'.format(500)     # 科学记数 大写e
5.000000E+02
'{:,}'.format(13800000000)  # 千分位格式化，逗号
13,800,000,000
'{:_}'.format(13800000000)  # 千分位格式化，下划线
13_800_000_000
'{:%}'.format(0.25) # 百分比格式化
25.000000%
'{:.0%}'.format(0.25) # 百分比格式化，精度控制
25%
```

## Python 日期和时间

```Python
from datetime import datetime
datetime.now().strftime('%Y-%m-%d %H:%M:%S') # 输出常用时间格式
from time import time
time() # 时间戳
```

## Python magic method

`__eq__`

- Py3: used on all compare situayion
- Py2: used only on equal sinatial

## Python buildin Modules

---

### os

```python
In [1]: import os
In [2]: os.system("date")  # 不仅执行命令而且返回执行后的信息对象(常用于需要获取执行命令后的返回信息)
Tue Jan  5 16:58:02 CST 2021

In [3]: nowtime = os.popen('date') # 打开一个管道运行文件，返回一个类文件对象，可以从中读取返回值
In [7]: nowtime.read()
Out[7]: 'Tue Jan  5 16:58:57 CST 2021\n'
```

### subprocess

生成新的进程，连接它们的输入、输出、错误管道，并且获取它们的返回码。  
推荐用`subprocess.Popen`代替`os.popen`

`run` Python3.5添加的函数, 可以参数如下:

```python
run(args, *, stdin=None, input=None, stdout=None, stderr=None, shell=False, timeout=None, check=False, encoding=None, errors=None) 
```

对于高级子进程用法, 可以用`Popen`类, 可用参数如下:

```python
Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0)
```

如果 shell 设为 True,，则使用 shell 执行指定的指令

| 方法       | 说明 |
| ---------- | -------------------- |
| Popen.poll | 检查子进程是否已被终止 |
| Popen.wait | 等待子进程被终止 |
| Popen.communicate | 与进程交互 |
| Popen.send_signal | 将信号发送给子进程 |
| Popen.terminate | 停止子进程 |
| Popen.kill | 杀死子进程 |

Popen与进程交互例子

```python
import subprocess
p1 = subprocess.Popen('dir', shell=True, stdin=None, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
p2 = subprocess.Popen('sort /R', shell=True, stdin=p1.stdout)
print(p1.stdout.read())
p1.stdout.close()
out, err = p2.communicate() 
```

### Hashlib

``` python
hashlib.md5('abc'.encode('utf-8').hexdigest()
```

### Traceback

``` python
def test_traceback():
    import traceback
    # 返回调用堆栈（一个list），最多两层， 其中最后一个元素为当前行的信息，包括（文件名，行号，函数名，这一行的代码）
    info = traceback.extract_stack(limit=2)
    print(info)
    # 打印函数调用堆栈
    traceback.print_stack(limit=2)
    # 无异常时为None
    traceback.print_exc()
```

### Json beautify

- load 读取 file 转化, useful parameter: `object_hook`, `object_paris_hook`
- loads 读取 str 转化
- dump 转化py 对象成 file, useful parameter: `indent`
- dumps 转换py 对象成json str

``` python
    s_json = """[{"name": "John","address": {"street": "8 bit avenue",
                "zip": "94444", "city": "San Jose"}},
                {"name": "Chris","address": {"street": "4 bit avenue",
                "zip": "95555", "city": "Milpitas"}}]"""

    o_json = json.loads(s_json)
    f_json = json.dumps(o_json, indent = 3, sort_keys=False)
```

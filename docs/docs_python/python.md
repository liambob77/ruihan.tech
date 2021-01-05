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

## Python magic method

`__eq__`

- Py3: used on all compare situayion
- Py2: used only on equal sinatial

## Ipython magics

---

### 常用line magics

- %lsmagic
- %pdoc
- %pdef
- %psource
- %pfile
- %timeit
    -n 单词循环中执行的次数
    -r 执行循环的次数

### 常用cell magics

## Python buildin Modules

---

### logging

logging lib 包含 4 个主要对象

- logger:logger 是程序信息输出的接口。它分散在不同的代码中使得程序可以在运行的时候记录相应的信息，并根据设置的日志级别或 filter 来决定哪些信息需要输出并将这些信息分发到其关联的 handler。常用的方法有 Logger.setLevel()，Logger.addHandler() ，Logger.removeHandler() ，Logger.addFilter() ，Logger.debug(), Logger.info(), Logger.warning(), Logger.error()，getLogger() 等。logger 支持层次继承关系，子 logger 的名称通常是父 logger.name 的方式。如果不创建 logger 的实例，则使用默认的 root logger，通过 logging.getLogger() 或者 logging.getLogger("") 得到 root logger 实例。

- Handler:Handler 用来处理信息的输出，可以将信息输出到控制台，文件或者网络。可以通过 Logger.addHandler() 来给 logger 对象添加 handler，常用的 handler 有 StreamHandler 和 FileHandler 类。StreamHandler 发送错误信息到流，而 FileHandler 类用于向文件输出日志信息，这两个 handler 定义在 logging 的核心模块中。其他的 hander 定义在 logging.handles 模块中，如 HTTPHandler,SocketHandler。

- Formatter:Formatter 则决定了 log 信息的格式 , 格式使用类似于 %(< dictionary key >)s 的形式来定义，如'%(asctime)s - %(levelname)s - %(message)s'，支持的 key 可以在 python 自带的文档 LogRecord attributes 中查看。

- Filter:Filter 用来决定哪些信息需要输出。可以被 handler 和 logger 使用，支持层次关系，比如如果设置了 filter 为名称为 A.B 的 logger，则该 logger 和其子 logger 的信息会被输出，如 A.B,A.B.C.

``` python
import logging
logging_format = "[%(asctime)s] %(process)d-%(levelname)s "
logging_format += "%(module)s::%(funcName)s():1%(lineno)d: %(message)s"
logging_formater = logging.Formatter(logging_format)
filehandler = logging.FileHandler('test.log','a') 
filter=logging.Filter('b') 
filehandler.addFilter(filter)
logger = logging.getLogger("Operate")
logger.addHandler(filehandler)
logger.setLevel(logging.DEBUG)
logger.debug('it is a debug info')
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

## Python external Modules

---

### Pytest

#### pytest enable logging print

``` bash
pytest -s
pytest.main(args=['-s', os.path.abspath(__file__)])
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

### Sqlachemy

#### SQLALchemy 字段

| 类型名  | python类型 | 说明 |
| ------- | ---------  | ------ |
| Integer | int        | 普通整数，32位 |
| Float   | float      | 浮点数 |
| String  | str        | 变长字符串 |
| Text    | str        | 变长字符串，对较长字符串做了优化 |
| Boolean | bool       | 布尔值 |
| Pickle  | Type       | 任何python对象  自动使用Pickle序列化 |

#### SQLALchemy 字段选项

| 选项名 | 说明 |
| ---    |  --  |
| primarykey |  如果设为True，表示主键 |
| unique |  如果设为True，这列不重复 |
| index |   如果设为True，创建索引，提升查询效率 |
| nullable |    如果设为True，允许空值 |
| default | 为这列定义默认值 |

- check db connectable

``` python
from sqlalchemy import MetaData, Table, create_engine
DEFAULT_URL = "mysql://wl:wl@localhost:3306/wl"
db = create_engine(DEFAULT_URL)
db.connect()
```

### Selenium

- css selector don't support #0

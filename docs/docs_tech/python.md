# Python Notes

---

[python 官方文档](https://docs.python.org/zh-cn/3/)

函数内代码执行效率更高

## Modify files

``` python
# 直接打开一个文件，如果文件不存在则创建文件
# w：以写方式打开，
# a：以追加模式打开 (从 EOF 开始, 必要时创建新文件)
# r+：以读写模式打开
# w+：以读写模式打开 (参见 w )
# a+：以读写模式打开 (参见 a )
# rb：以二进制读模式打开
# wb：以二进制写模式打开 (参见 w )
# ab：以二进制追加模式打开 (参见 a )
# rb+：以二进制读写模式打开 (参见 r+ )
# wb+：以二进制读写模式打开 (参见 w+ )
# ab+：以二进制读写模式打开 (参见 a+ )
fp = open("test.txt",w)
# size为读取的长度，以byte为单位
fp.read([size])
# 读一行，如果定义了size，有可能返回的只是一行的一部分
fp.readline([size])
fp.readlines([size])
# 把str写到文件中，write()并不会在str后加上一个换行符
fp.write(str) 
fp.writelines(seq)
# 把缓冲区的内容写入硬盘
fp.flush() 
# 返回一个长整型的"文件标签"
fp.fileno() 
# 文件是否是一个终端设备文件(unix系统中的)
fp.isatty() 
# 返回文件操作标记的当前位置，以文件的开头为原点
fp.tell()
# 返回下一行
fp.next() 
# 将文件打操作标记移到offset的位置
fp.seek(offset[,whence])
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
logging_format += "%(module)s::%(funcName)s():1%(lineno)d: "
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

- check db connectable

``` python
from sqlalchemy import MetaData, Table, create_engine
DEFAULT_URL = "mysql://wl:wl@localhost:3306/wl"
db = create_engine(DEFAULT_URL)
db.connect()
```

### Selenium

- css selector don't support #0

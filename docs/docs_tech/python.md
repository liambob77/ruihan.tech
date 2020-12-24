# Python Notes

## modify files

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

## Python Modules

---

### Pytest

#### pytest enable logging print

``` bash
pytest -s
pytest.main(args=['-s', os.path.abspath(__file__)])
```

### json beautify

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

### sqlachemy

- check db connectable

``` python
from sqlalchemy import MetaData, Table, create_engine
DEFAULT_URL = "mysql://wl:wl@localhost:3306/wl"
db = create_engine(DEFAULT_URL)
db.connect()
```

### hashlib

``` python
hashlib.md5('abc'.encode('utf-8').hexdigest()
```

### traceback

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


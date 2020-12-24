# Ipython magics
---

## 常用line magics
- %lsmagic
- %pdoc
- %pdef
- %psource
- %pfile
- %timeit
    -n 单词循环中执行的次数
    -r 执行循环的次数

## 常用cell magics

-


## pytest enable logging print

```
pytest -s
pytest.main(args=['-s', os.path.abspath(__file__)])
```

## json beautify

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

## sqlachemy records

- check db connectable

``` python
from sqlalchemy import MetaData, Table, create_engine
DEFAULT_URL = "mysql://wl:wl@localhost:3306/wl"
db = create_engine(DEFAULT_URL)
db.connect()
```

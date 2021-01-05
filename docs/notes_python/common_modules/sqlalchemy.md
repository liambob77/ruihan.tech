# Sqlachemy

## SQLALchemy 字段

| 类型名  | python类型 | 说明 |
| ------- | ---------  | ------ |
| Integer | int        | 普通整数，32位 |
| Float   | float      | 浮点数 |
| String  | str        | 变长字符串 |
| Text    | str        | 变长字符串，对较长字符串做了优化 |
| Boolean | bool       | 布尔值 |
| Pickle  | Type       | 任何python对象  自动使用Pickle序列化 |

## SQLALchemy 字段选项

| 选项名 | 说明 |
| ---    |  --  |
| primarykey |  如果设为True，表示主键 |
| unique |  如果设为True，这列不重复 |
| index |   如果设为True，创建索引，提升查询效率 |
| nullable |    如果设为True，允许空值 |
| default | 为这列定义默认值 |

check db connectable

``` python
from sqlalchemy import MetaData, Table, create_engine
DEFAULT_URL = "mysql://wl:wl@localhost:3306/wl"
db = create_engine(DEFAULT_URL)
db.connect()
```

# logging

logging lib 包含 4 个主要对象

logger:logger 是程序信息输出的接口。它分散在不同的代码中使得程序可以在运行的时候记录相应的信息，并根据设置的日志级别或 filter 来决定哪些信息需要输出并将这些信息分发到其关联的 handler。常用的方法有 Logger.setLevel()，Logger.addHandler() ，Logger.removeHandler() ，Logger.addFilter() ，Logger.debug(), Logger.info(), Logger.warning(), Logger.error()，getLogger() 等。logger 支持层次继承关系，子 logger 的名称通常是父 logger.name 的方式。如果不创建 logger 的实例，则使用默认的 root logger，通过 logging.getLogger() 或者 logging.getLogger("") 得到 root logger 实例。  

Handler:Handler 用来处理信息的输出，可以将信息输出到控制台，文件或者网络。可以通过 Logger.addHandler() 来给 logger 对象添加 handler，常用的 handler 有 StreamHandler 和 FileHandler 类。StreamHandler 发送错误信息到流，而 FileHandler 类用于向文件输出日志信息，这两个 handler 定义在 logging 的核心模块中。其他的 hander 定义在 logging.handles 模块中，如 HTTPHandler,SocketHandler。  

Formatter:Formatter 则决定了 log 信息的格式 , 格式使用类似于 %(< dictionary key >)s 的形式来定义，如'%(asctime)s - %(levelname)s - %(message)s'，支持的 key 可以在 python 自带的文档 LogRecord attributes 中查看。  

Filter:Filter 用来决定哪些信息需要输出。可以被 handler 和 logger 使用，支持层次关系，比如如果设置了 filter 为名称为 A.B 的 logger，则该 logger 和其子 logger 的信息会被输出，如 A.B,A.B.C.  

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

**Python 3.8 and later**: A new option, force, has been made available to automatically remove the root handlers while calling basicConfig().
For example:

``` python
logging.basicConfig(filename='wocl.log', level=logging.DEBUG, force=True)`
```

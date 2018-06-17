---
title: python logging简单使用
date: 2018-05-20 14:31:39
tags: python
---

logging一共有三种4种配置形式
- 基础型 basicconfig
- 代码型
- 字典型
- 文件ini型

只使用了代码型
```python
def custom_formatTime(record, datefmt=None):
    return (datetime.datetime.utcnow() + datetime.timedelta(hours=8)).strftime(u"%Y-%m-%d %H:%M:%S")

# 通过下面的方式进行简单配置输出方式与日志级别
logger = logging.getLogger("binance_coin")
# 写入日志文件
handler_file = logging.handlers\
    .RotatingFileHandler("logs/debug.log", maxBytes=1024*1024, backupCount = 50,encoding = "UTF-8")#FileHandler("test.log")
handler_file.setLevel(logging.INFO)
handler_error_file = logging.handlers\
    .RotatingFileHandler("logs/error.log", maxBytes=1024*1024, backupCount = 10,encoding = "UTF-8")#FileHandler("test.log")
handler_error_file.setLevel(logging.ERROR)
# 打印日志文件到console
handler_console = logging.StreamHandler()
# 格式化器
formatter = logging.Formatter(
    "%(asctime)s %(name)-12s %(levelname)-8s %(message)s")
formatter.formatTime = custom_formatTime
# 添加格式化到控制器
handler_console.setFormatter(formatter)
handler_file.setFormatter(formatter)
handler_error_file.setFormatter(formatter)
# 添加处理器
logger.addHandler(handler_console)
logger.addHandler(handler_file)
logger.addHandler(handler_error_file)
# log级别
logger.setLevel(logging.INFO)
# 进行记录
logger.debug('often makes a very good meal of %s', 'visiting tourists')
```
#### 实现的需求
1. 日志分割，使用RotatingFileHandler处理器设定了maxBytes，在日志文件大小为1M的时候就会将日志文件重命名为**debug.log.1**,然后新建一个**debug.log**,第二次的时候将**debug.log.1**->**debug.log.2**, 每次都会这样依次更改名字,直到日志文件数量到达backupCount的限制,将最老的一个删除以维持最多50个的日志文件。logging也可以使用时间分割
2. 日志文件的编码, 需要在处理器里面指定`encoding = "UTF-8"`来避免出现日志编码错误
3. 自定义时间，重写了formatter.formatTime来设定打印出来的时间始终是utc+8
4. 分级别输出，info以上的日志都会用handler_file输出到**debug.log**，error以上的日志都会输出到**error.log**

参考链接 [日志（Logging） — The Hitchhiker's Guide to Python](http://pythonguidecn.readthedocs.io/zh/latest/writing/logging.html)
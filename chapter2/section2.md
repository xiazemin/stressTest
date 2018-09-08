# Siege做Web服务器压测

# 

用「Web压测」关键词检索，能找到好多进行压测的工具，比如ab、Http\_load、Webbench、Siege这些，不过今天并不是要对这些工具做对比，毕竟我们只是想得到一个结果。本文主要介绍Siege，因为Siege是上面四者中，在Mac上安装和使用最便利的，所以果断就是它了！

### 准备工作

在压测开始前，你需要确保你的`open files`足够大，否则会报`TOO MANY FILES OPEN`错误，可以通过`ulimit -a`查看，如下图:

使用`ulimit -n 10000`可以修改该值。不过这种修改并不是永久的，关闭终端会话，又会恢复回来。

### 安装

```
brew install siege
```

### 使用

```
siege -c 1000 -t 5s URL
siege -c 1000 -t 5s -f  URL_File_Name
```

上面是`siege`的两种使用方法，第一种是对指定站点进行压测，第二种是对文件中包含的若干URL进行批量测试。

* `-c` 并发数

* `-t` 压力测试时间，可以在时间后加单位，具体查帮助，上面表示的是压测时间持续5秒

* `-r` 重复次数，与`-t`表达方式不同，但含义相同，设一个即可

* `-f` 包含URL的文本名字

* `-b` BENCHMARK模式，请求之间无需延迟

### 输出结果

* Transactions 总测试数

* Availability 成功率

* Elapsed time 总用时

* Data transferred 总共传输数据

* Response time 响应耗时

* Transaction rate 每秒处理请求数

* Throughput 平均每秒传输数据量

* Concurrency 实际最高并发

* Successful transactions 成功处理次数

* Failed transactions 失败处理请求数

* Longest transaction 传输所花最长时间

* Shortest transaction 传输所花最短时间

最后说明下 Siege 能支持GET/POST两种请求，不过格式略有区别，并且上面罗列的只是Siege的部分参数，Siege还有很多其它参数，请一并参考手册。


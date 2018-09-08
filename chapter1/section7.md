# siege压力测试工具安装和介绍

输入参数说明:

| 输入名称 | 解释说明 |
| :--- | :--- |
| -V, –version | 打印版本信息 |
| -h, –help | 打印帮助信息 |
| -C, –config | 显示配置信息 |
| -v, –verbose | 打印冗余配置信息。 |
| -g, –get | 显示HTTP交易。 |
| -c, –concurrent=NUM | 设置并发用户数 |
| -u, –url=”URL” | 设置被测Web的URL |
| -i, –internet | 用户模拟、随机访问URL |
| -b, –benchmark . | 基准测试。 |
| -t, –time=NUM | 设置测试时间。 |
| -r, –reps=NUM | 设置测试次数 |
| -f, –file=FILE | 更改配置文件存档 |
| -R, –rc=FILE | 更改siegerc文件和环境变量 |
| -l, –log | 测试日志 |
| -m, –mark=”text” | 标记测试日志 |
| -d, –delay=NUM | 设置时间延迟 |
| -H, –header=”text” | 增加测试头文件 |
| -A, –user-agent=”text” | 设置代理测试请求 |

  
输出参数说明:

| 输出名称 | 解释说明 |
| :--- | :--- |
| Transactions: | 访问次数 |
| Availability: | 成功次数 |
| Elapsed time: | 测试用时 |
| Data transferred: | 测试传输数据量 |
| Response time: | 平均响应时间 |
| Transaction rate: | 每秒事务处理量 |
| Throughput: | 吞吐率 |
| Concurrency: | 并发用户数 |
| Successful transactions: | 成功传输次数 |
| Failed transactions: | 失败传输次数 |
| Longest transaction: | 最长响应时间 |
| Shortest transaction: | 最短响应时间 |

# 2.siege安装 {#2siege安装}

下载地址 :[http://download.joedog.org/siege/](http://download.joedog.org/siege/), 我用的版本 : siege-2.70.tar.gz

```
CaodeMacBook-Pro:local root# tar -xzvf siege-2.70.tar 
CaodeMacBook-Pro:local root# cd siege-2.70
CaodeMacBook-Pro:siege-2.70 root# ./configure
CaodeMacBook-Pro:siege-2.70 root# make 
CaodeMacBook-Pro:siege-2.70 root# make install

```

安装成功验证 :

```
CaodeMacBook-Pro:siege-2.70 root# siege -version
siege: invalid option -- e
siege: invalid option -- e
SIEGE 2.70
Usage: siege [options]
       siege [options] URL
       siege -g URL
Options:
  -V, --version           VERSION, prints the version number.
  -h, --help              HELP, prints this section.
  -C, --config            CONFIGURATION, show the current config.
  -v, --verbose           VERBOSE, prints notification to screen.
  -g, --get               GET, pull down HTTP headers and display the
                          transaction. Great for application debugging.
  -c, --concurrent=NUM    CONCURRENT users, default is 10
  -i, --internet          INTERNET user simulation, hits URLs randomly.
  -b, --benchmark         BENCHMARK: no delays between requests.
  -t, --time=NUMm         TIMED testing where "m" is modifier S, M, or H
                          ex: --time=1H, one hour test.
  -r, --reps=NUM          REPS, number of times to run the test.
  -f, --file=FILE         FILE, select a specific URLS FILE.
  -R, --rc=FILE           RC, specify an siegerc file
  -l, --log[=FILE]        LOG to FILE. If FILE is not specified, the
                          default is used: PREFIX/var/siege.log
  -m, --mark="text"       MARK, mark the log file with a string.
  -d, --delay=NUM         Time DELAY, random delay before each requst
                          between 1 and NUM. (NOT COUNTED IN STATS)
  -H, --header="text"     Add a header to request (can be many)
  -A, --user-agent="text" Sets User-Agent in request

Copyright (C) 2010 by Jeffrey Fulmer, et al.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE.

```

  


# 3.siege使用 {#3siege使用}

我这边是测试了一个server端的接口并发情况.

* 接口地址是:
  [http://118.212.149.xx:8080/xx/xx/xx](http://118.212.149.xx:8080/xx/xx/xx)
* 请求类型 : POST
* 请求参数 : {“accountId”:”123”,”platform”:”ios”}
* 请求次数 :10次
* 请求并发数量 : 200

请求 : \(请求参数说明请参照上文中表格\)

```
CaodeMacBook-Pro:siege-2.70 root# siege "http://118.212.149.xx:8080/xx/xx/xx POST {\"accountId\":\"123\",\"platform\":\"ios\"}" -r 10 -c 200

```

返回 : \(返回参数说明请参照上文中表格\)

```
done.
Transactions:               2000 hits
Availability:             100.00 %
Elapsed time:              15.27 secs
Data transferred:           0.07 MB
Response time:              0.47 secs
Transaction rate:         130.98 trans/sec
Throughput:             0.00 MB/sec
Concurrency:               61.45
Successful transactions:        2000
Failed transactions:               0
Longest transaction:            8.17
Shortest transaction:           0.06

```

  



# siege进行web压力测试

1、下载siege

     http://download.joedog.org/siege/siege-latest.tar.gz

2、解压

    tar -zxvf siege-latest.tar.gz 

3、配置、编译、安装

   ./configure;

   make && make install

4、参数详解

    -C,或-config 在屏幕上打印显示出当前的配置,配置是包括在他的配置文件$HOME/.siegerc中,可以编辑里面的参数,这样每次siege 都会按照它运行.  
    -v 运行时能看到详细的运行信息  
    -c n,或-concurrent=n 模拟有n个用户在同时访问,n不要设得太大,因为越大,siege 消耗本地机器的资源越多  
    -i,-internet 随机访问urls.txt中的url列表项,以此模拟真实的访问情况\(随机性\),当urls.txt存在是有效   
    -d n,-delay=n hit每个url之间的延迟,在0-n之间  
    -r n,-reps=n 重复运行测试n次,不能与 -t同时存在  
    -t n,-time=n 持续运行siege ‘n’秒\(如10S\),分钟\(10M\),小时\(10H\)  
    -l 运行结束,将统计数据保存到日志文件中siege.log,一般位于/usr/local/var/siege.log中,也可在.siegerc中自定义  
    -R SIEGERC,-rc=SIEGERC 指定用特定的siege配置文件来运行,默认的为$HOME/.siegerc  
    -f FILE, -file=FILE 指定用特定的urls文件运行siege ,默认为urls.txt,位于siege 安装目录下的etc/urls.txt  
    -u URL,-url=URL 测试指定的一个URL,对它进行"siege",此选项会忽略有关urls文件的设定  
    urls.txt文件：是很多行待测试URL的列表以换行符断开,格式为:  
    \[protocol://\]host.domain.com\[:port\]\[path/to/file\]

    用法示例

   siege -c 50 -r 100 -u http://192.168.91.100

 5、结果说明

\*\* SIEGE 2.72  
\*\* Preparing 300 concurrent users for battle.  
The server is now under siege.. done.  
Transactions: 30000 hits //完成30000次处理  
Availability: 100.00 % //100.00 % 成功率  
Elapsed time: 68.59 secs //总共使用时间  
Data transferred: 817.76 MB //总数据传输（不包含头数据）\*\*\*\*\*  
Response time: 0.04 secs //平均响应时间  
Transaction rate: 437.38 trans/sec //平均每秒完成 437.38 次处理\*\*\*\*\*\*  
Throughput: 11.92 MB/sec //平均每秒传送数据  
Concurrency: 17.53 //实际最高并发连接数  
Successful transactions: 30000 //成功处理次数  
Failed transactions: 0 //失败处理次数  
Longest transaction: 3.12 //满足一个请求所需最长时间\*\*\*\*\*  
Shortest transaction: 0.00 //满足一个请求所需最短时间 \*\*\*\*\*\*  
Data transferred部分包含每个请求收到的响应的总大小（MB）。

Transaction rate帮助我们了解当Web服务器在我们命令指定的负载下运行时可以满足的并发事务数（同时发生的请求）。


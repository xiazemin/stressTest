# http\_load压力测试

http\_load是基于linux平台的性能测试工具，它体积非常小，仅100KB。它以并行复用的方式运行，可以测试web服务器的吞吐量与负载。



 一、安装http\_load



A、进入/usr/local目录下创建man文件夹，并赋予权限；



\[root@localhost ~\]\#cd /usr/local



\[root@localhost local\]\#mkdir man



\[root@localhost local\]\#chmod 777 man



B、进man文件夹中，下载http\_load安装包；



\[root@localhost local\]\#cd man



\[root@localhost man\]\# wget  http://acme.com/software/http\_load/http\_load-12mar2006.tar.gz



C、解压、并安装http\_load-12mar2006.tar.gz包；



\[root@localhost man\]\# tar zxvf http\_load-12mar2006.tar.gz



\[root@localhost man\]\# cd http\_load-12mar2006



\[root@localhost http\_load-12mar2006\]\# make



\[root@localhost http\_load-12mar2006\]\# sudo make install



 



二、使用方法

1、每次使用前，需要先切换到http\_load目录下

cd http\_load-12mar2006

 



2、了解参数和文件

 



参数	全称	含义

-p	-parallel	并发的用户进程数。

-f	-fetches	总计的访问次数

-r	-rate	含义是每秒的访问频率

-s	-seconds	连续的访问时间

url	 	网站连接地址或url文件



其中，“url”是http\_load-12mar2006目录下其中一个文件，在使用前，先在http\_load-12mar2006新建一个空白的名为urls.txt的文件，使用vi命令新建。urls.txt文件，每个URL一行，且不能有空行，否则报错。



 



http\_load使用方式：



http\_load -parallel 100 -fetches 10000



\#100个并发执行10000次



http\_load -parallel 100 -seconds 3600



\#100个并发执行1小时



http\_load -rate 100 -fetches 10000



\#每秒100个请求频率，请求10000次



http\_load -rate 100 -seconds 3600



\#每秒100个请求频率执行1小时



 



3、开始测试



 







 



结果分析：

1．10 fetches, 10 max parallel, 20480bytes, in 0.052394 seconds

说明在上面的测试中运行了10个请求，最大的并发进程数是10，总计传输的数据是20480bytes，运行的时间是0.052394秒

2．2048 mean bytes/connection

说明每一连接平均传输的数据量2048/10\(fetches\)=204.8

3．190.862 fetches/sec, 390884 bytes/sec

说明每秒的响应请求为190.862，每秒传递的数据为390884 bytes/sec

4．msecs/connect: 1.4946 mean, 1.649 max, 1.353 min

说明每连接的平均响应时间是1.4946 毫秒，最大的响应时间1.649 毫秒，最小的响应时间1.353 毫秒

5．msecs/first-response: 26.9952 mean, 48.305 max,7.454 min

6、HTTP response codes: code 200 -- 10



每秒响应用户数和response time



每连接响应用户时间







 



结果分析：

1．49 fetches, 1 max parallel, 100352bytes, in 10 seconds

说明在上面的测试中运行了49个请求，最大的并发进程数是1，总计传输的数据是100352bytes，运行的时间是 10秒

2．2048 mean bytes/connection

说明每一连接平均传输的数据量100352/49\(fetches\)=2048

3．4.89999 fetches/sec, 10035.2 bytes/sec

说明每秒的响应请求为4.89999，每秒传递的数据为10035.2 bytes/sec

4．msecs/connect:0.284837 mean, 0.639 max, 0.163 min

说明每连接的平均响应时间是0.284837 毫秒，最大的响应时间0.639 毫秒，最小的响应时间0.163 毫秒

5．msecs/first-response: 4.91612 mean, 38.309 max, 3.393 min

6、HTTP response codes: code 200 -- 49





说明：



一般使用http\_load做压力测试时，主要会考虑这“fetches/sec、msecs/connect ”两个项的结果，即服务器每秒能够响应的查询次数来衡量性能指标。


# http\_load使用详解

1.什么是http\_load

http\_load是一款基于Linux平台的web服务器性能测试工具，用于测试web服务器的吞吐量与负载，web页面的性能。



2.http\_load的安装

1\)下载地址

wget http://www.acme.com/software/http\_load/http\_load-12mar2006.tar.gz

2\)安装

tar xzvfhttp\_load-12mar2006.tar.gz

make

make install



3.http\_load的使用

1\)创建文件

vi urls

写入要测的服务器域名或IP地址

比如urls里是http://www.baidu.com/ 亦或是192.168.0.1这一类的都可以测



2\)使用示例

./http\_load -rate 5 -seconds 10 urls



-parallel 简写-p ：含义是并发的用户进程数。

-fetches 简写-f ：含义是总计的访问次数

-rate 简写-p ：含义是每秒的访问频率

-seconds简写-s ：含义是总计的访问时间



执行结果：

说明执行了一个持续时间10秒的测试，每秒的频率为5。

49 fetches, 2 max parallel, 289884 bytes, in 10.0148 seconds

5916 mean bytes/connection

4.89274 fetches/sec, 28945.5 bytes/sec

msecs/connect: 28.8932 mean, 44.243 max, 24.488 min

msecs/first-response: 63.5362 mean, 81.624 max, 57.803 min

HTTP response codes:

code 200 -- 49



结果分析：

1．49 fetches, 2 max parallel, 289884 bytes, in 10.0148 seconds

说明在上面的测试中运行了49个请求，最大的并发进程数是2，总计传输的数据是289884bytes，运行的时间是10.0148秒

2．5916 mean bytes/connection

说明每一连接平均传输的数据量289884/49=5916

3．4.89274 fetches/sec, 28945.5 bytes/sec

说明每秒的响应请求为4.89274，每秒传递的数据为28945.5 bytes/sec

4．msecs/connect: 28.8932 mean, 44.243 max, 24.488 min

说明每连接的平均响应时间是28.8932 msecs，最大的响应时间44.243 msecs，最小的响应时间24.488 msecs

5．msecs/first-response: 63.5362 mean, 81.624 max, 57.803 min 

6、HTTP response codes: code 200 -- 49

说明打开响应页面的类型，如果403的类型过多，那可能要注意是否系统遇到了瓶颈。

特殊说明：这里，我们一般会关注到的指标是fetches/sec、msecs/connect

他们分别对应的常用性能指标参数

Qpt-每秒响应用户数和response time，每连接响应用户时间。

测试的结果主要也是看这两个值。当然仅有这两个指标并不能完成对性能的分析，我们还需要对服务器的cpu、men进行分析，才能得出结论



4.常见错误

1\)byte count wrong

http\_load在处理时会去关注每次访问同一个URL返回结果（即字节数）是否一致，若不一致就会抛出byte count wrong



2\)too many open files

系统限制的open files太小，ulimit -n 修改open files值即可



3\)无法发送大请求 （请求长度&gt;600个字符）

默认接受请求的buf大小 http\_load.c



4\)Cannot assign requested address

客户端频繁的连服务器，由于每次连接都在很短的时间内结束，导致很多的TIME\_WAIT，以至于用光了可用的端口号，所以新的连接没办法绑定端口，所以要改客户端机器的配置，

在sysctl.conf里加：

net.ipv4.tcp\_tw\_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；

net.ipv4.tcp\_timestamps=1 开启对于TCP时间戳的支持,若该项设置为0，则下面一项设置不起作用

net.ipv4.tcp\_tw\_recycle=1 表示开启TCP连接中TIME-WAIT sockets的快速回收


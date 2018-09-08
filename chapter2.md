# SIEGE安装及使用简介

Siege是一个多线程http负载测试和基准测试工具。它有3种操作模式：   
1\) Regression \(when invoked by bombardment\)Siege从配置文件中读取URLs，按递归方式，  
逐个发送请求   
2\) Internet simulation \(Siege从配置文件中读取URLs，随机选取URL发送请求\) 3\) Brute force \(在命令行上写上一个单独的URL，发送请求\)  

安装 

我这里使用的是最新版的。  
$ wget[http://www.joedog.org/pub/siege/siege-latest.tar.gz](http://www.joedog.org/pub/siege/siege-latest.tar.gz)

得到最新包siege-latest.tar.gz

解压之。

$ tar xvf siege-latest.tar.gz

得到的具体版本就是siege-3.0.6

$ cd siege-3.0.6/

编译的时候，我只制定了安装目录：/usr/local/siege/

$ ./configure --prefix=/usr/local/siege/

$ make   
$ make install

安装完成后，查看一下安装目录下具体都有哪些目录：

$ ll /usr/local/siege/

结果如下：

drwxr-xr-x 2 root root 4096 Jun 11 15:48 bin  
drwxr-xr-x 2 root root 4096 Jun 11 15:48 etc  
drwxr-xr-x 5 root root 4096 Jun 11 15:48 man

然后使用/usr/local/siege/bin/siege -help来查看是否真的安装成功了：

$ siege/bin/siege –help

如果看到如下信息，则说明安装成功了：

\*\* SIEGE 3.0.6  
\*\* Preparing 15 concurrent users for battle.  
The server is now under siege...  
done.  
siege aborted due to excessive socket failure; you  
can change the failure threshold in $HOME/.siegerc

Transactions:                      0 hits  
Availability:                   0.00 %  
Elapsed time:                  36.31 secs  
Data transferred:               0.00 MB  
Response time:                  0.00 secs  
Transaction rate:               0.00 trans/sec  
Throughput:                     0.00 MB/sec  
Concurrency:                    0.00  
Successful transactions:           0  
Failed transactions:            1038  
Longest transaction:            0.00  
Shortest transaction:           0.00



调用   
  
Siege以命令行方式使用，调用格式如下：  siege \[options\]   
 siege \[options\] \[url\]  siege -g \[url\]    
Siege的选项说明： -V  ,  --version   
打印siege的版本信息  



-h  ,  --help 打印帮助信息    
-C  ,  --config   
打印当前配置。这个选项读取 .siegerc 并打印。你可以通过编辑$HOME/.siegerc修改配置。如果没有这个文件，你可以运行siege.config（/usr/local/bin/siege.config ）来生成此文件。    
 -v  ,  --verbose    
打印详细信息。包含请求的协议、响应码、请求的URL    
 -g URL  ,  --get URL    
获得一个HTTP事务。导出headers和显示HTTP交易。对于debug有所帮助。    
-c NUM  ,  --concurrent=NUM  并发用户数（必需参数）。    
-i  ,  --internet    
此选项配合URLs的配置文件使用。每个虚拟用户每次请求的URL是随机从配置文件的获取。    
-t NUMm  ,  --time=NUMm    
设置测试运行的时间。单位S\M\H代表秒\分\时。单位大小写不敏感。数字和单位之间不要有空格。    
-f FILE  ,  --file=FILE    
被测试的URLs配置文件。默认$SIEGE\_HOME/etc/urls.txt    
 - l  ,  --log    
记录统计信息到$SIEGE\_HOME/var/siege.log    
- m MESSAGE  ,  --mark=MESSAGE   
此选项允许你使用分隔符标记日志文件。没必要与'-l'同时使用。    
-d NUM  ,  --delay=NUM    
Time DELAY, random delay before each requst between 1 and NUM. \(NOT COUNTED IN STATS\)    
-b  ,  --benchmark   
BENCHMARK,  runs  the  test  with  NO  DELAY  for throughput benchmarking. 负载测试时不推荐使用。    
-H HEADER ,  --header=HEADER   
HEADER, 该选项允许你添加额外的头信息。     
R SIEGERC ,  --rc=SIEGERC   
设置运行参数配置文件。 默认 $HOME/.siegerc    
-A "User Agent" , --user-agent="User Agent" AGENT, 定制客户端信息。



当一次测试中需要多个URL时，可以将URLs放到一个单独的文件中。默认$SIEGE\_HOME/etc/urls.txt 

例如：urls.txt   
\# 这里表述注释，一行一个URL 

[http://homer.whoohoo.com/index.html](http://homer.whoohoo.com/index.html)

[http://homer.whoohoo.com/howto.jsp](http://homer.whoohoo.com/howto.jsp)

[http://go.whoohoo.com/cgi-bin/q.cgi?scope=a](http://go.whoohoo.com/cgi-bin/q.cgi?scope=a)

[http://go.whoohoo.com/cgi-bin/q.cgi](http://go.whoohoo.com/cgi-bin/q.cgi) POST scope=a 

[http://homer.whoohoo.com/my.jsp](http://homer.whoohoo.com/my.jsp) POST a=1&b=2 

\# POST文件   
[www.haha.com/aha.jsp](http://www.haha.com/aha.jsp) POST &lt;/home/jeff/my.txt 

[www.haha.com/parser.jsp](http://www.haha.com/parser.jsp) POST &lt;./my.txt    
Siege一次性将文件读入内存，按照文件中顺序发送请求。\[-i\]选项可以随机发送URL请求。 

  
在文件中，我们可以设置和引用变量。先定义后引用原则。一个变量一行，类似于shell变量，引用时用$\(\)或者${}，如  HOST = homer.whoohoo.com  [http://${HOST}/index.html](http://%24%7Bhost%7D/index.html)  
如果变量不存在，则表示空字符串。

使用

　　siege的具体使用方法很简单，通常使用时用的比较多的就是并发用户数和运行时间

$ siege/bin/siege –c 10 –t 60s 

[http://host/xxx](http://host/xxx)

-c 并发用户数

-t  运行时间

 这里的URL有两种用法，当测试单个地址的时候，直接写就可以，如要测试

[http://www.cnblogs.com/lrxing/p/3626256.html](http://www.cnblogs.com/lrxing/p/3626256.html)

$ siege/bin/siege –c 10 –t 60s 

[http://www.cnblogs.com/lrxing/p/3626256.html](http://www.cnblogs.com/lrxing/p/3626256.html)


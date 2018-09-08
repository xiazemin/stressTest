# http\_load的安装及使用方法



http\_load

  


  


程序非常小，解压后也不到100K

  


  


http\_load以并行复用的方式运行，用以测试web服务器的吞吐量与负载。但是它不同于大多数压力测试工

  


  


具，它可以以一个单一的进程运行，一般不会把客户机搞死。还可以测试HTTPS类的网站请求。

  


  


下载地址：http://soft.vpser.net/test/http\_load/http\_load-12mar2006.tar.gz

  


安装很简单

  


\#tar zxvf http\_load-12mar2006.tar.gz

  


\#cd http\_load-12mar2006

  


\#make 

&

&

 make install

  


  


命令格式：http\_load -p 并发访问进程数 -s 访问时间 需要访问的URL文件

  


  


参数其实可以自由组合，参数之间的选择并没有什么限制。比如你写成http\_load -parallel 5 -seconds

  


  


300 urls.txt也是可以的。我们把参数给大家简单说明一下。

  


-parallel 简写-p ：含义是并发的用户进程数。

  


-fetches 简写-f ：含义是总计的访问次数

  


-rate    简写-p ：含义是每秒的访问频率

  


-seconds简写-s ：含义是总计的访问时间

  


  


准备URL文件：urllist.txt，文件格式是每行一个URL，URL最好超过50－100个测试效果比较好.文件格式

  


  


如下：

  


http://www.vpser.net/uncategorized/choose-vps.html

  


http://www.vpser.net/vps-cp/hypervm-tutorial.html

  


http://www.vpser.net/coupons/diavps-april-coupons.html

  


http://www.vpser.net/security/vps-backup-web-mysql.html

  


例如：

  


  


http\_load -p 30 -s 60 urllist.txt

  


参数了解了，我们来看运行一条命令来看看它的返回结果

  


命令：% ./http\_load -rate 5 -seconds 10 urls说明执行了一个持续时间10秒的测试，每秒的频率为5。

  


  


49 fetches, 2 max parallel, 289884 bytes, in 10.0148 seconds5916 mean bytes/connection4.89274

  


  


fetches/sec, 28945.5 bytes/secmsecs/connect: 28.8932 mean, 44.243 max, 24.488 minmsecs/first

  


  


-response: 63.5362 mean, 81.624 max, 57.803 minHTTP response codes: code 200 — 49

  


  


结果分析：

  


1．49 fetches, 2 max parallel, 289884 bytes, in 10.0148 seconds

  


说明在上面的测试中运行了49个请求，最大的并发进程数是2，总计传输的数据是289884bytes，运行的时间是10.0148秒

  


2．5916 mean bytes/connection说明每一连接平均传输的数据量289884/49=5916

  


3．4.89274 fetches/sec, 28945.5 bytes/sec

  


说明每秒的响应请求为4.89274，每秒传递的数据为28945.5 bytes/sec

  


4．msecs/connect: 28.8932 mean, 44.243 max, 24.488 min说明每连接的平均响应时间是28.8932 msecs

  


  


，最大的响应时间44.243 msecs，最小的响应时间24.488 msecs

  


5．msecs/first-response: 63.5362 mean, 81.624 max, 57.803 min

  


6、HTTP response codes: code 200 — 49     说明打开响应页面的类型，如果403的类型过多，那可能

  


  


要注意是否系统遇到了瓶颈。

  


特殊说明：

  


测试结果中主要的指标是 fetches/sec、msecs/connect 这个选项，即服务器每秒能够响应的查询次数，

  


  


用这个指标来衡量性能。似乎比 apache的ab准确率要高一些，也更有说服力一些。

  


Qpt-每秒响应用户数和response time，每连接响应用户时间。

  


测试的结果主要也是看这两个值。当然仅有这两个指标并不能完成对性能的分析，我们还需要对服务器的

  


  


cpu、men进行分析，才能得出结论


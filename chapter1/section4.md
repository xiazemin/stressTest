# web性能测试工具-http\_load

性能测试工具只是测试手段，不一段要掌握的多而全，关键是要顺手。当然，多了解一些测试工具可以信手掂来，快速进入测试。最重要的是了解工具的特性，适合的测试场景，能满足多高的并发需求，支持哪些协议，是否有足够好的扩展性。

我这里只是整理下很久以前的内容。简单说明下几款web性能测试工具。因为这些轻量级的测试工具通常不能模拟较复杂的场景，所以对几款工具本身未进行性能对比。

**http\_load：**

| 下载 | [http://www.acme.com/software/http\_load/](http://www.acme.com/software/http_load/) |
| :--- | :--- |
| 运行方式 | 运行命令行 1. http\_load -parallel 10 -fetches 1000 urls.txt 2. http\_load -rate 5 -seconds 300 urls.txt 可缩写为： 1. http\_load -p 10 -f 1000 urls.txt 2. http\_load -r 5 -s 300 urls.txt 参数介绍 -p并发访问进程数 -f总的访问次数 -r每秒的访问频率，限制大于1小于1000 -s总的访问时间 通常参数组合：-p –f；-r -s urls.txt是你要访问的网址名，参数可以是单个的网址也可以是包含网址的文件。 |
| 并发模式 | http\_load以并行复用的方式运行，它不同于大多数压力测试工具，它可以以一个单一的进程运行。这应该是http\_load不能支撑高并发的硬伤。 参考：[http://www.51testing.com/?uid-159438-action-viewspace-itemid-154740](http://www.51testing.com/?uid-159438-action-viewspace-itemid-154740) |
| 支持协议 | 支持http、https协议，请求方式默认为GET方式。不支持POST方式。 有同行对其源码进行修改可以支持POST方式， 参考：[http://www.51testing.com/?uid-159438-action-viewspace-itemid-153701](http://www.51testing.com/?uid-159438-action-viewspace-itemid-153701) |
| url参数 | 可以指定单个或者多个url，以文本文件的形式传参。 url个数限制：未明确限制，但是http\_load会将所有的url都读入内存，且对文件的大小有限制。 另外，url过多对性能影响较大，所以不推荐url个数过多。 参考：[http://www.51testing.com/?uid-159438-action-viewspace-itemid-151584](http://www.51testing.com/?uid-159438-action-viewspace-itemid-151584) [http://www.cnblogs.com/subsir/articles/2574905.html](http://www.cnblogs.com/subsir/articles/2574905.html) |
| 参数读取方式 | http\_load采取的是随机读取url。不支持顺序读。 参考：[http://www.51testing.com/?uid-159438-action-viewspace-itemid-151584](http://www.51testing.com/?uid-159438-action-viewspace-itemid-151584) |
| http参数 | 如果需要对http的header和body进行设置的话恐怕http\_load是不支持的，没看到有相关参数进行支持 |
| http响应解析 | 对http请求的响应会解析返回码，用户不能自定义解析响应，也不支持写文件存储 |
| 测试结果 | qatest@app-67:~/linsa/http\_load-12mar2006$ ./http\_load -r 1000 -s 5 url.txt 4331 fetches, 15 max parallel, 3.36822e+07 bytes, in 5.00107 seconds 7777 mean bytes/connection 866.015 fetches/sec, 6.735e+06 bytes/sec msecs/connect: 4.9993 mean, 3004.15 max, 2.074 min msecs/first-response: 2.34363 mean, 207.503 max, 2.119 min HTTP response codes: code 200 -- 4331fetches运行了4331个请求，最大的并发进程数是15，总计传输的数据是3.36822e+07 bytes，运行的时间是5.00107秒 关注点：总请求数、最大并发进程数 7777 mean bytes/connection：每个请求/连接传输的数据量 866.015 fetches/sec, 6.735e+06 bytes/sec fetches/sec每秒请求数/访问次数，bytes/sec每秒传输的数据量 关注点：每秒的响应请求数 msecs/connect: 4.9993 mean, 3004.15 max, 2.074 min 平均响应时间，最大响应时间，最小响应时间 HTTP response codes: code 200 -- 4331 请求响应码 服务器返回状态代码： 200 —表示请求成功。 3XX —-重定向类 403、404 —客户端错误类（服务器没有找到与请求URI相符的资源。） 500 —服务器错误类（内部服务器错误） 等等… 关注点：是否有403、404、500错误产生 |




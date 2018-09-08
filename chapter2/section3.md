# Siege 参数详解 & 示例

CentOS 上安装 Web

[**性能测试**](javascript:;)

工具

[**Siege**](javascript:;)

一个简单的示例：

| 　　示例 ==&gt; 并发请求指定URL  http://download.joedog.org/　　siege -c 5 -r 2 http://download.joedog.org/　　参数说明： -c 是并发量，并发数为5,  -r 是重复次数， 重复2次 |
| :--- |


　　某次运行的结果~

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011434421K6RO.jpg)](http://www.51testing.com/batch.download.php?aid=73575)

　　在本篇博文中，我们将对Siege使用方法和参数，结合示例进行说明~

**　Siege使用方法**

　　可以先通过 siege -h 或者 siege --help 命令，查看一下 siege 的帮助信息，如：

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011435081Im9J.jpg)](http://www.51testing.com/batch.download.php?aid=73576)

　　从上述帮助信息中，可以看出siege的使用方法有如下三种方法

| 　　Usage:　　       1.siege \[options\]　　       2.siege \[options\] URL　　       3.siege -g URL |
| :--- |


　　其中option的可选项有如下这些：

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011435411dJUA.jpg)](http://www.51testing.com/batch.download.php?aid=73577)

**　siege \[options\]**

　　该使用方法主要用于：

　　●查看版本信息

　　●查看帮助信息

　　●查看当前配置信息

　　使用siege -V 或者 siege --version 查看siege版本信息

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011436281O2cA.jpg)](http://www.51testing.com/batch.download.php?aid=73578)

　　使用siege -h 或者 siege --help 查看帮助信息

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011436441GUnk.jpg)](http://www.51testing.com/batch.download.php?aid=73579)

　　使用siege -C 或者 siege --config 查看当前Siege的配置信息

[![](http://www.51testing.com/attachments/2017/09/15201284_2017090114370811Wkc.jpg)](http://www.51testing.com/batch.download.php?aid=73580)

**　siege -g URL**

　　获取指定URL的Header信息，并显示HTTP处理信息~

　　siege -g http://www.baidu.com

[![](http://www.51testing.com/attachments/2017/09/15201284_201709011439411vGxQ.jpg)](http://www.51testing.com/batch.download.php?aid=73581)

　　siege \[options\] URL

　　这种使用方法是最主要的，接下来，结合示例对参数的使用进行说明~

**Siege 参数说明 **

**&**

** 示例**

**　Siege 常用参数**

[![](http://www.51testing.com/attachments/2017/09/15201284_20170901144018105M2.jpg)](http://www.51testing.com/batch.download.php?aid=73582)

　　一般测试基本上多个参数组合在一起来完成的


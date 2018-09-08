# 在mac os中用http\_load,valgrind和xdebug来分析php程序

http\_load,valgrind,xdebug这三个工具对php程序进行分析的方法。  
用xhprof也可以进行php程序的执行分析，但它会要求在php程序中嵌入一段代码，对于有洁癖的同学来说会觉得不爽，本文所述的方法可以不需要动源码来达到xhprof相同的效果。

先看一下标题所说的三个工具。

* 1.http\_load是基于linux平台的一种性能测工具。以并行复用的方式运行，用以测试web服务器的吞吐量与负载，测试web页面的性能。
* 2.valgrind是一款用于内存调试、内存泄漏检测以及性能分析的软件开发工具。
* 3.xdebug是一个开放源代码的PHP程序调试器\(即一个Debug工具\)，可以用来跟踪，调试和分析PHP程序的运行状况。

**调优的步骤一般为：**

**1.用http\_load找出需要调优的程序**

用http\_load我们可以分析出程序的大体性能，以我的mac book为例:

```

```

运行结果：

```

```

上面测试了一段从数据库中读取数据列表的php程序，可以看到在并发为5，执行1000次读取，性能可以达到928.657次/每秒。这个值越大越好。如果发现一个程序的数值仅为10几或个位数，那就有必要对该程序进行一些优化了。

http\_load的安装很简单：

下载地址：[http://acme.com/software/http\_load/http\_load-12mar2006.tar.gz](http://acme.com/software/http_load/http_load-12mar2006.tar.gz)

安装：

```

```



**2.以xdebug和valgrind对程序的执行流程和执行时间进行分析**

先看一个例子：

[![](http://blog.51cto.com/attachment/201211/144351484.png)](http://blog.51cto.com/attachment/201211/144351484.png)

从上面的图可以看到程序的执行流程，函数调用次数，执行时间占比。

要看到这张图，除了php开发所需的php,nginx或apache外，我们还需要安装几个工具。

**valgrind, QT, graphiviz, qcachegrind**

对于php开发环境的搭建可以参见：[http://ustb80.blog.51cto.com/6139482/1056493](http://ustb80.blog.51cto.com/6139482/1056493)



**valgrind的安装**

之前的一篇文章已经写过了，不再细说。详见[http://ustb80.blog.51cto.com/6139482/1054942](http://ustb80.blog.51cto.com/6139482/1054942)



**QT安装**

qcachegrind是依赖于QT和graphiviz的，需要先安装前两个

QT可以从[http://get.qt.nokia.com/qt/source/](http://get.qt.nokia.com/qt/source/)下载，挑最新的dmg下即可，我挑的是[http://get.qt.nokia.com/qt/source/qt-mac-opensource-4.8.1.dmg](http://get.qt.nokia.com/qt/source/qt-mac-opensource-4.8.1.dmg)

下载之后，双击解压，再双击里面的pkg安装即可，傻瓜式安装。



**graphiviz安装**

接着安装graphiviz，这个工具用于绘图，下载页为：[http://www.graphviz.org/Download\_macos.php](http://www.graphviz.org/Download_macos.php)

挑对应的版本下载，双击pkg安装即可



**qcachegrind安装**

qcachegrind是包含在kcachegrind中的，用svn取下源码来安装:

```

```

安装完后，用

```

```

可以打开qcachegrind程序，可以看到一个图形界面，左上角有一个打开按钮，通过它来选择cachegrind.out文件进行分析。



**下面是使用的步骤：**

首先需要配置一下php.ini中的xdebug，如果是按本博客中的php安装方法安装的php，其中已经安装了xdebug，仅需要修改下设置即可。

在php.ini中加上这三行，建议放到最后，以后好找。

```

```

当这个选项打开后，我们就可以通过url或是在post的时候触发xdebug了，例如：

```

```

注意问号后的参数，当我们的地址栏中带有这个参数并进行了访问后，在xdebug.profiler\_output\_dir所指定的目录就会生成一个cachegrind.out.XXXX文件。这个文件就可以用qcachegrind来分析了。

```

```

后面的数字是进程号。

用之前的qcachegrind程序来选择这里的cachegrind.out.63271文件，就可以看到我们程序的执行情况了，带图形的哟，非常直观。

接下来的事情就是根据图形上耗时最长的程序段进行合理的优化了。

  



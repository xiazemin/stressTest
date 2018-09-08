# http\_load的安装和使用

一、安装  
1.下载地址：[http://www.acme.com/software/http\_load/http\_load-09Mar2016.tar.gz](http://www.acme.com/software/http_load/http_load-09Mar2016.tar.gz)  
2.解压后进入目录，执行make & make install命令  
3.查看安装结果，输入http\_load不报错即成功  
![](https://img-blog.csdn.net/20170314164803682?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGpfZ2FtYmxlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

二、使用  
1.新建一个.txt文件，用来存储目标URL（每个URL占一行）  
2.输入命令 http\_load -parallel 5 -seconds 10 youname.txt  
![](https://img-blog.csdn.net/20170314165222107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGpfZ2FtYmxlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")  
参数解析：  
-parallel 简写-p ：含义是并发的用户进程数。  
-fetches 简写-f ：含义是总计的访问次数  
-rate 简写-p ：含义是每秒的访问频率  
-seconds简写-s ：含义是总计的访问时间


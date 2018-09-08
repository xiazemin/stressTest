# thrift协议的服务进压力测试

背景：Facebook 开发的远程服务调用框架 Apache Thrift，它采用接口描述语言定义并创建服务，支持可扩展的跨语言服务开发，所包含的代码生成引擎可以在多种语言中创建高效的、无缝的服务，其传输数据采用二进制格式，相对 XML 和 JSON 体积更小，对于高并发、大数据量和多语言的环境更有优势。



负责的搜索服务使用thrift，之前是对其http的上游服务进行压测，从而压到该thrift服务；后续想调研一种方式能够直接对thrift服务进行压测。



thrift框架的C++小程序见：http://www.cnblogs.com/zhaoxd07/p/5387215.html



调研方向：



一 Jmeter

具体逻辑与使用Jmeter进行java请求的压测方式相似：http://www.cnblogs.com/zhaoxd07/p/4895224.html



区别在于setUp中对于协议的选择，会对结果有较大影响



TProtocol protocol = new TCompactProtocol\(transport\);

client = new Service.Client\(protocol\);

原则为：与被测代码使用协议一致即可



二 bender

https://github.com/pinterest/bender



为Pinterest公司开发的，可以对http和thrift协议做压测，使用go语言实现的。我试了一下挺好用的，就是go语言用不太习惯



主要难点在于main.go文件的实现，与Jmeter需要注意点一致：协议的使用



具体操作步骤如下：



1  Go环境初始化

1\)  下载安装     

\# 下载安装go

cd /usr/local/  自己选择安装目录和工作目录

wget https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz

tar -xzvf go1.8.3.linux-amd64.tar.gz

2） 配置环境变量   

复制代码

\# 环境变量配置

vim /etc/profile 增加如下行：

\#go variable

export GOROOT=/usr/local/go

export PATH=$PATH:/usr/local/go/bin

export GOPATH=/data/bender/

export GOBIN=/data/bender/bin

完成后保存 并使其生效  source /etc/profile

复制代码

3） 确认变量配置是否生效

\#echo $GOROOT

/usr/local/go

2  下载安装需要的插件

cd $GOPATH

go get git.apache.org/thrift.git/lib/go/thrift

go get github.com/pinterest/bender

3 准备server和client

准备server和client



因为我这是个实际项目，server已经由开发同学实现并启动，所以此时只需要准备server即可。如下



Server

 



cd $GOPATH/workspace/myProject/                \# 进入我的工作目录

rz my\_interface.thrift                         \# 上传我的接口文件

thrift  -gen go my\_interface.thrift            \# 将thrift文件转成go语言

 



4 准备压测脚本bender

编写压测文件 在myProject目录下 新建 myBender文件夹，vim main.sh如下：



复制代码

package main



// import 若提示有错，就看下目录下文件是否可用

import \(    

    "github.com/pinterest/bender"

    bthrift "github.com/pinterest/bender/thrift"

    "git.apache.org/thrift.git/lib/go/thrift"

    ....

    "github.com/pinterest/bender/hist"

\)



// 准备压测数据,我用的服务的日志，解析后作为请求发送，相当于回放

func SyntheticRequests\(n int\) chan interface{} {

    f, err := os.Open\("./advanced\_search.info"\)

    paramList := make\(\[\]\*as.QueryParam, 0\)

    if err != nil {

        panic\(err\)

    }   

    defer f.Close\(\)

    

    rd := bufio.NewReader\(f\)

    for {

        line, err := rd.ReadString\('\n'\)

        

        if err != nil \|\| io.EOF == err {

            break

        } else {

            json\_transport := thrift.NewTMemoryBufferLen\(len\(\[\]rune\(line\)\)\)

            json\_transport.WriteString\(line\)

            json\_protocol := thrift.NewTJSONProtocol\(json\_transport\)

            param := as.NewQueryParam\(\)

            param.Read\(json\_protocol\)

            paramList = append\(paramList, param\)

        }

    }

    max := len\(paramList\)

    c := make\(chan interface{}, 100\)

    go func\(\) {

        for i := 0; i &lt; n; i++ {

            rand.Seed\(int64\(time.Now\(\).Nanosecond\(\)\)\)

            c &lt;- paramList\[rand.Intn\(max\)\]

        }

        close\(c\)

    }\(\)

    return c

}



//重要:需要跟服务端所用协议一致

func ASExecutor\(request interface{}, transport thrift.TTransport\) \(interface{}, error\) {

    pFac := thrift.NewTCompactProtocolFactory\(\)

    client := as.NewRecommendSrvClientFactory\(transport, pFac\)

    return client.GetCampaigns\(request.\(\*as.QueryParam\)\)   

}



// 主要是用bender的接口，完成压测

func main\(\) {

    intervals := bender.ExponentialIntervalGenerator\(150\)      // 150 -- qps

    requests := SyntheticRequests\(150\*60\*1\)                    //总请求数

    //buffer字节数, clientExec, 超时时间, hosts--写server所在的ip和端口,如非同一机器,保证端口可访问

    exec := bthrift.NewThriftRequestExec\(thrift.NewTBufferedTransportFactory\(125\), ASExecutor, 100 \* time.Second, "127.0.0.1:9099"\)

    recorder := make\(chan interface{}, 128\)

    bender.LoadTestThroughput\(intervals, requests, exec, recorder\)

    l := log.New\(os.Stdout, "", log.LstdFlags\)

    h := hist.NewHistogram\(60000, 1000000\)

    bender.Record\(recorder, bender.NewLoggingRecorder\(l\), bender.NewHistogramRecorder\(h\)\)

    fmt.Println\(h\)

}

复制代码

5 运行及结果

1  编译

cd $GOPATH

go install workspace/asthrift/asbender    // main.go在目录asbender下

编译后的可执行文件在目录bin下，貌似新文件不能直接覆盖旧文件，因此我会先手动删除旧文件



2 执行

如下结果是因为所请求的server未启动\(因该服务现阶段不属于我维护，该结果仅做参考示例\)



复制代码

\[root@ip bin\]\#./asbender

Percentiles:

 Min:     0

 Median:  1

 90th:    1

 99th:    1

 99.9th:  1

 99.99th: 4

 Max:     10

Stats:

 Average: 0.923222

 Total requests: 9000

 Elapsed Time \(sec\): 59.5483

 Average QPS: 151.14

 Errors: 9000

 Percent errors: 100.00

复制代码

6 问题与解决

 使用直接下载的bender运行时，会打印出请求的详细资料，导致压测的qps不能完成预期，可手动注释掉bender相关代码，并重新编译。



vim src/github.com/pinterest/bender/recorders.go，注释掉第44行。



 



复制代码

 41 // NewLoggingRecorder creates a new log.Logger-based recorder.

 42 func NewLoggingRecorder\(l \*log.Logger\) Recorder {

 43     return func\(msg interface{}\) {

 44 //      logMessage\(l, msg\)

 45     }

 46 }

复制代码

 



删除目录下的pkg/linux\_amd64/github.com/pinterest/bender.a文件，并重新编译：



go install github.com/pinterest/bender




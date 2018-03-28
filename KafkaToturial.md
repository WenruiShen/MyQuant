## Brief Tutorial for Kafka Modules



### 1.Zookeeper:
##### 1.1 Installation:
Zookeeper下载地址：https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/stable/

> tar -zxf zookeeper-3.4.10.tar.gz 
>
> cd zookeeper-3.4.10
>
> mkdir data

创建配置文件：
> vi conf/zoo.cfg

    tickTime=2000
    dataDir=../data
    clientPort=2181
    initLimit=5
    syncLimit=2

##### 2.2 Basic commends:

启动Zookeeper:
> bin/zkServer.sh start

执行此命令后，您将得到如下所示的响应：

    $ JMX enabled by default
    $ Using config: /Users/../zookeeper-3.4.6/bin/../conf/zoo.cfg
    $ Starting zookeeper ... STARTED

启动CLI
> bin/zkCli.sh

输入上面的命令后，您将被连接到zookeeper服务器，并将获得以下响应。

    Connecting to localhost:2181
    ................
    Welcome to ZooKeeper!
    ................
    WATCHER::
    WatchedEvent state:SyncConnected type: None path:null
    [zk: localhost:2181(CONNECTED) 0]

停止Zookeeper服务器:
> bin/zkServer.sh stop

#####---------------------------------------------------------------------------
### 2.Kafka:
##### 2.1 Installation:

kafka下载地址：https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.1/kafka_2.11-1.0.1.tgz

> tar -xvf kafka_2.11-1.0.1.tgz
>
> cd kafka_2.11-1.0.1

##### 2.2 Basic commends:
启动服务器： **( 请先启动 Zookeeper )**
> bin/kafka-server-start.sh config/server.properties &amp;

服务器启动后，您会在屏幕上看到以下响应:

    [2016-01-02 15:37:30,410] INFO KafkaConfig values:
    request.timeout.ms = 30000
    log.roll.hours = 168
    inter.broker.protocol.version = 0.9.0.X
    log.preallocate = false
    security.inter.broker.protocol = PLAINTEXT
    …………………………………………….

关闭服务器：
> bin/kafka-server-stop.sh config/server.properties

启动Kafka Broker后，在ZooKeeper终端上键入命令 jps ，您将看到以下响应 -

    821 QuorumPeerMain
    928 Kafka
    931 Jps

现在你可以看到两个守护进程运行在终端上，QuorumPeerMain是ZooKeeper守护进程，另一个是Kafka守护进程。


##### 2.3 References:
[1].https://www.w3cschool.cn/apache_kafka/apache_kafka_quick_guide.html

[2].https://blog.csdn.net/trigl/article/details/72581735





## Brief Tutorial for Kafka Modules



### 1.Zookeeper:
#### 1.1 Installation:
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

#### 1.2 Basic commends:

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
#### 2.1 Installation:

kafka下载地址：https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.1/kafka_2.11-1.0.1.tgz

> tar -xvf kafka_2.11-1.0.1.tgz
>
> cd kafka_2.11-1.0.1

#### 2.2 Basic commends:
##### 2.2.1 启动服务器： **( 请先启动 Zookeeper )**
> bin/kafka-server-start.sh config/server.properties &amp;

服务器启动后，您会在屏幕上看到以下响应:

    [2016-01-02 15:37:30,410] INFO KafkaConfig values:
    request.timeout.ms = 30000
    log.roll.hours = 168
    inter.broker.protocol.version = 0.9.0.X
    log.preallocate = false
    security.inter.broker.protocol = PLAINTEXT
    …………………………………………….

##### 2.2.2 关闭服务器：
> bin/kafka-server-stop.sh config/server.properties

启动Kafka Broker后，在ZooKeeper终端上键入命令 jps ，您将看到以下响应 -

    821 QuorumPeerMain
    928 Kafka
    931 Jps

现在你可以看到两个守护进程运行在终端上，QuorumPeerMain是ZooKeeper守护进程，另一个是Kafka守护进程。

#### 2.3: 单节点 - 单代理配置:

在此配置中，您有一个ZooKeeper和代理id实例。 以下是配置它的步骤:

##### 2.3.1 创建Kafka主题:

Kafka提供了一个名为 kafka-topics.sh 的命令行实用程序，用于在服务器上创建主题。 打开新终端并键入以下示例。


示例:

> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic Hello-Kafka

我们刚刚创建了一个名为 Hello-Kafka 的主题，其中包含一个分区和一个副本因子。 上面创建的输出将类似于以下输出 -

    输出: 创建主题 Hello-Kafka 

创建主题后，您可以在Kafka代理终端窗口中获取通知，并在config / server.properties文件中的“/ tmp / kafka-logs /"中指定的创建主题的日志。


##### 2.3.2 主题列表:
要获取Kafka服务器中的主题列表，可以使用以下命令:

> bin/kafka-topics.sh --list --zookeeper localhost:2181

    输出: Hello-Kafka

由于我们已经创建了一个主题，它将仅列出 Hello-Kafka。
假设，如果创建多个主题，您将在输出中获取主题名称。

##### 2.3.3 启动生产者以发送消息:

> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka

从上面的语法，生产者命令行客户端需要两个主要参数:

 - 代理列表 - 我们要发送邮件的代理列表。 在这种情况下，我们只有一个代理。Config / server.properties文件包含代理端口ID，因为我们知道我们的代理正在侦听端口9092，因此您可以直接指定它。
 
 - 主题名称 - 以下是主题名称的示例。

生产者将等待来自stdin的输入并发布到Kafka集群。 默认情况下，每个新行都作为新消息发布，然后在 config / producer.properties 文件中指定默认生产者属性。 现在，您可以在终端中键入几行消息，如下所示。

    输出:
    $ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka
    [2016-01-16 13:50:45,931] WARN property topic is not valid (kafka.utils.Verifia-bleProperties)
    Hello
    My first message
    My second message


##### 2.3.4 启动消费者以接收消息：
与生产者类似，在 config / consumer.proper-ties 文件中指定了缺省使用者属性。 打开一个新终端并键入以下消息消息语法。

> bin/kafka-console-consumer.sh --zookeeper localhost:2181 —topic Hello-Kafka --from-beginning

    输出:
    Hello
    My first message
    My second message

最后，您可以从制作商的终端输入消息，并看到他们出现在消费者的终端。 
到目前为止，您对具有单个代理的单节点群集有非常好的了解。 现在让我们继续讨论多个代理配置。

    broker, producer, consumer三者必须运行在不同的终端窗口中！

#### 2.4: 单节点多代理配置：
##### 2.4.1 





#####---------------------------------------------------------------------------
#### 2.5 References:
[1].https://www.w3cschool.cn/apache_kafka/apache_kafka_quick_guide.html

[2].https://blog.csdn.net/trigl/article/details/72581735

[3].https://kafka.apache.org/documentation/#quickstart





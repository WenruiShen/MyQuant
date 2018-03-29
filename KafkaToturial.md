## Brief Tutorial for Kafka Modules


[原文 -> Github](https://github.com/WenruiShen/MyQuant)

[转载请注明！]

Author: Shen Wenrui

Email:  Thomas.shen3904@qq.com
 

### 1.Zookeeper:
#### 1.1 Installation:
[Zookeeper下载地址:https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/stable/](https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/stable/)

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

#### 2.4: 单节点多代理配置(Setting up a multi-broker cluster)：
##### 2.4.1 创建多个Kafka Brokers:
So far we have been running against a single broker, but that's no fun. For Kafka, a single broker is just a cluster of 
size one, so nothing much changes other than starting a few more broker instances. But just to get feel for it, 
let's expand our cluster to three nodes (still all on our local machine).

First we make a config file for each of the brokers (on Windows use the copy command instead):

> cp config/server.properties config/server-1.properties
>
> cp config/server.properties config/server-2.properties

Now edit these new files and set the following properties:

    config/server-1.properties:
        broker.id=1
        listeners=PLAINTEXT://:9093
        log.dir=/tmp/kafka-logs-1
     
    config/server-2.properties:
        broker.id=2
        listeners=PLAINTEXT://:9094
        log.dir=/tmp/kafka-logs-2


The ``broker.id`` property is the unique and permanent name of each node in the cluster. We have to override the port and 
log directory only because we are running these all on the same machine and we want to keep the brokers from all trying 
to register on the same port or overwrite each other's data.

##### 2.4.2 启动多个多个Kafka Brokers:
We already have Zookeeper and our single node started, so we just need to start the two new nodes:

> bin/kafka-server-start.sh config/server-1.properties &amp;
>
> bin/kafka-server-start.sh config/server-2.properties &amp;

##### 2.4.3 创建主题：

Now create a new topic with a replication factor of three:

> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic my-replicated-topic


Describe 命令用于检查哪个代理正在侦听当前创建的主题，如下所示 -

> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic

    Topic:my-replicated-topic   PartitionCount:1    ReplicationFactor:3 Configs:
    Topic: my-replicated-topic  Partition: 0    Leader: 1   Replicas: 1,2,0 Isr: 1,2,0

从上面的输出，我们可以得出结论，第一行给出所有分区的摘要，显示主题名称，分区数量和我们已经选择的复制因子。 
在第二行中，每个节点将是分区的随机选择部分的领导者。

在我们的例子中，我们看到我们的第一个broker(with broker.id 0)是领导者。 
然后Replicas:0,2,1意味着所有代理复制主题最后 Isr 是 in-sync 副本的集合。 
那么，这是副本的子集，当前活着并被领导者赶上。


##### 2.4.4 启动生产者以发送消息
此过程保持与单代理设置中相同。

Let's publish a few messages to our new topic:

> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my-replicated-topic
    
    ...
    my test message 1
    my test message 2
    ^C


##### 2.4.5 启动消费者以接收消息:
此过程保持与单代理设置中所示的相同。

Now let's consume these messages:

> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic my-replicated-topic

    ...
    my test message 1
    my test message 2
    ^C

##### 2.4.6 Brokers的冗余和选举机制:
Now let's test out fault-tolerance. Broker 1 was acting as the leader so let's kill it:

> ps aux | grep server-1.properties

    7564 ttys002    0:15.91 /System/Library/Frameworks/JavaVM.framework/Versions/1.8/Home/bin/java...

> kill -9 7564

Leadership has switched to one of the slaves and node 1 is no longer in the in-sync replica set:

> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic

    Topic:my-replicated-topic   PartitionCount:1    ReplicationFactor:3 Configs:
    Topic: my-replicated-topic  Partition: 0    Leader: 2   Replicas: 1,2,0 Isr: 2,0

But the messages are still available for consumption even though the leader that took the writes originally is down:

> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic my-replicated-topic

    ...
    my test message 1
    my test message 2
    ^C


#### 2.5: 基本主题操作:
在本章中，我们将讨论各种基本主题操作。

##### 2.5.1 修改主题:

> 语法: bin/kafka-topics.sh —zookeeper localhost:2181 --alter --topic topic_name --partitions count

We have already created a topic “Hello-Kafka" with single partition count and one replica factor. 
Now using “alter" command we have changed the partition count.

> 示例: bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic my-replicated-topic --partitions 2

    输出
    WARNING: If partitions are increased for a topic that has a key, 
    the partition logic or ordering of the messages will be affected
    Adding partitions succeeded!


##### 2.5.2 删除主题:
要删除主题，可以使用以下语法。

> 语法: bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic topic_name

> 示例: bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic my-replicated-topic

    输出: Topic Hello-kafka marked for deletion

* 注意 - 如果 config/server.properties中 delete.topic.enable 未设置为true，则此操作不会产生任何影响！

[强制删除topic]https://github.com/darrenfu/bigdata/issues/6

#### 2.6: Use Kafka Connect to import/export data:
##### 2.6.1 从文件输入:

Here we'll run Kafka Connect with simple connectors that import data from a file to a Kafka topic and export data from a Kafka topic to a file.

First, we'll start by creating some seed data to test with:

> echo -e "foo\nbar" > test.txt

Next, we'll start two connectors running in standalone mode, which means they run in a single, local, dedicated process. 
We provide three configuration files as parameters. The first is always the configuration for the Kafka Connect process, 
containing common configuration such as the Kafka brokers to connect to and the serialization format for data. 
The remaining configuration files each specify a connector to create. These files include a unique connector name, 
the connector class to instantiate, and any other configuration required by the connector.

> bin/connect-standalone.sh config/connect-standalone.properties config/connect-file-source.properties config/connect-file-sink.properties

These sample configuration files, included with Kafka, use the default local cluster configuration you started earlier 
and create two connectors: the first is a source connector that reads lines from an input file and produces each to a 
Kafka topic and the second is a sink connector that reads messages from a Kafka topic and produces each as a line in an output file.

##### 2.6.2 从文件输出:

During startup you'll see a number of log messages, including some indicating that the connectors are being instantiated. 
Once the Kafka Connect process has started, the source connector should start reading lines from ``test.txt`` and producing 
them to the topic ``connect-test``, and the sink connector should start reading messages from the topic ``connect-test`` and write 
them to the file ``test.sink.txt``. We can verify the data has been delivered through the entire pipeline by examining the 
contents of the output file:

> more test.sink.txt

    foo
    bar

Note that the data is being stored in the Kafka topic ``connect-test``, so we can also run a console consumer to see the 
ata in the topic (or use custom consumer code to process it):

> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic connect-test --from-beginning

    {"schema":{"type":"string","optional":false},"payload":"foo"}
    {"schema":{"type":"string","optional":false},"payload":"bar"}
    ...

The connectors continue to process data, so we can add data to the file and see it move through the pipeline:

> echo Another line>> test.txt

You should see the line appear in the console consumer output and in the sink file.


#### 2.7: Use Kafka Streams to process data
Kafka Streams is a client library for building mission-critical real-time applications and microservices, 
where the input and/or output data is stored in Kafka clusters. Kafka Streams combines the simplicity of writing and 
deploying standard Java and Scala applications on the client side with the benefits of Kafka's server-side cluster 
technology to make these applications highly scalable, elastic, fault-tolerant, distributed, and much more. 
This [quickstart example](https://kafka.apache.org/11/documentation/streams/quickstart) 
will demonstrate how to run a streaming application coded in this library.

#####---------------------------------------------------------------------------
#### 2.8 References:
[1].https://www.w3cschool.cn/apache_kafka/apache_kafka_quick_guide.html

[2].https://blog.csdn.net/trigl/article/details/72581735

[3].https://kafka.apache.org/documentation/#quickstart





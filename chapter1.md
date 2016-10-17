## **3.1 Kafka的特性**

1. **高吞吐量、低延迟**：kafka每秒可以处理几十万条消息，它的延迟最低只有几毫秒
2. **可扩展性**：kafka集群支持热扩展
3. **持久性、可靠性**：消息被持久化到本地磁盘，并且支持数据备份防止数据丢失
4. **容错性**：允许集群中节点失败（若副本数量为n,则允许n-1个节点失败）
5. **高并发**：支持数千个客户端同时读写

## **3.2 Kafka一些重要设计思想**

下面介绍先大体介绍一下Kafka的主要设计思想，可以让相关人员在短时间内了解到kafka相关特性，如果想深入研究，后面会对其中每一个特性都做详细介绍。

1. **Consumergroup**：各个consumer可以组成一个组，每个消息只能被组中的一个consumer消费，如果一个消息可以被多个consumer消费的话，那么这些consumer必须在不同的组。
2. **消息状态**：在Kafka中，消息的状态被保存在consumer中，broker不会关心哪个消息被消费了被谁消费了，只记录一个offset值（指向partition中下一个要被消费的消息位置），这就意味着如果consumer处理不好的话，broker上的一个消息可能会被消费多次。
3. **消息持久化**：Kafka中会把消息持久化到本地文件系统中，并且保持极高的效率。
4. **消息有效期**：Kafka会长久保留其中的消息，以便consumer可以多次消费，当然其中很多细节是可配置的。
5. **批量发送**：Kafka支持以消息集合为单位进行批量发送，以提高push效率。
6. **push-and-pull** : Kafka中的Producer和consumer采用的是push-and-pull模式，即Producer只管向broker push消息，consumer只管从broker pull消息，两者对消息的生产和消费是异步的。
7. **Kafka集群中broker之间的关系**：不是主从关系，各个broker在集群中地位一样，我们可以随意的增加或删除任何一个broker节点。
8. **负载均衡方面**： Kafka提供了一个 metadata API来管理broker之间的负载（对Kafka0.8.x而言，对于0.7.x主要靠zookeeper来实现负载均衡）。
9. **同步异步**：Producer采用异步push方式，极大提高Kafka系统的吞吐率（可以通过参数控制是采用同步还是异步方式）。
10. **分区机制partition**：Kafka的broker端支持消息分区，Producer可以决定把消息发到哪个分区，在一个分区中消息的顺序就是Producer发送消息的顺序，一个主题中可以有多个分区，具体分区的数量是可配置的。分区的意义很重大，后面的内容会逐渐体现。
11. **离线数据装载**：Kafka由于对可拓展的数据持久化的支持，它也非常适合向Hadoop或者数据仓库中进行数据装载。
12. **插件支持**：现在不少活跃的社区已经开发出不少插件来拓展Kafka的功能，如用来配合Storm、Hadoop、flume相关的插件。

## **2.3 kafka 应用场景**

1. **日志收集**：一个公司可以用Kafka可以收集各种服务的log，通过kafka以统一接口服务的方式开放给各种consumer，例如hadoop、Hbase、Solr等。
2. **消息系统**：解耦和生产者和消费者、缓存消息等。
3. **用户活动跟踪**：Kafka经常被用来记录web用户或者app用户的各种活动，如浏览网页、搜索、点击等活动，这些活动信息被各个服务器发布到kafka的topic中，然后订阅者通过订阅这些topic来做实时的监控分析，或者装载到hadoop、数据仓库中做离线分析和挖掘。
4. **运营指标**：Kafka也经常用来记录运营监控数据。包括收集各种分布式应用的数据，生产各种操作的集中反馈，比如报警和报告。
5. **流式处理**：比如spark streaming和storm
6. **事件源**

### **2.4 Kafka架构组件**

Kafka中发布订阅的对象是topic。我们可以为每类数据创建一个topic，把向topic发布消息的客户端称作producer，从topic订阅消息的客户端称作consumer。Producers和consumers可以同时从多个topic读写数据。一个kafka集群由一个或多个broker服务器组成，它负责持久化和备份具体的kafka消息。

* topic：消息存放的目录即主题

* Producer：生产消息到topic的一方

* Consumer：订阅topic消费消息的一方

* Broker：Kafka的服务实例就是一个broker
* Controller：Kafka在所有broker中选出一个broker做为controller, 所有partition的leader选举都由controller决定。Controller会将Leader的改变直接通过RPC的方式（比ZooKeeper Queue的方式更高效）通知需为为此作为响应的Broker。同时controller也负责增删Topic以及Replica的重新分配。

  ![](/assets/WechatIMG8.jpeg)


### **2.5 Kafka Topic&Partition**

消息发送时都被发送到一个topic，其本质就是一个目录，而topic由是由一些Partition Logs\(分区日志\)组成,其组织结构如下图所示：

![](/assets/WechatIMG9.jpeg)

1. 每个Partition中的消息都是有序的，生产的消息被不断追加到Partition log上，其中的每一个消息都被赋予了一个唯一的offset值。
2. Kafka集群会保存所有的消息，不管消息有没有被消费；我们可以设定消息的过期时间，只有过期的数据才会被自动清除以释放磁盘空间。比如我们设置消息过期时间为2天，那么这2天内的所有消息都会被保存到集群中，数据只有超过了两天才会被清除。 
3. Kafka需要维持的元数据只有一个即消费消息在Partition中的offset值，Consumer每消费一个消息，offset就会加1。其实消息的状态完全是由Consumer控制的，Consumer可以跟踪和重设这个offset值，这样的话Consumer就可以读取任意位置的消息。
4. 把消息日志以Partition的形式存放有多重考虑，第一，方便在集群中扩展，每个Partition可以通过调整以适应它所在的机器，而一个 topic又可以有多个Partition组成，因此整个集群就可以适应任意大小的数据了；第二就是可以提高并发，因为可以以Partition为单位读写了。
5. Kafka这些特性可以允许consumer随时接入或退出，对其它consumer没有任何影响。比如，你可以用命令行工具消息费任意topic而不会改变这些消息被其它consumer所消费。


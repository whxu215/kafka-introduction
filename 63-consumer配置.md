## 6.3 Consumer配置

| 名称 | 描述 | 类型 | 默认值 |
| --- | --- | --- | --- |
| group.id | 消费进程所属的组的ID，如果多个消费者同属一个组那么这些消费都的group.id需配置成一样的 | string | "" |
| client.id |用来标识某个client端，该id会被发送到kafka server.|string|""|
| bootstrap.servers | kafka server list. \(host1:port1,host2:port2,...\) | list | || |key.deserializer | key反序列化类实现Deserializer interface | class | || value.deserializer | value反序列化类实现Deserializer interface | class | ||fetch.min.bytes|consumer一个fetch请求能取回的最小数据大小，如果不满足这个数值则会等待直到满足指定大小。|int|1||heartbeat.interval.ms|consumer跟kafka server之间的心跳时间间隔，如果心跳不正常则kafka server会认为该consumer离开了，之后会执行rebalancing。这个时间设置必须要小于session.timeout.ms，一般设置不要大于session.timeout.ms的1\/3|int|3000||session.timeout.ms|如果kafka server在这个时间内都没有收到consumer的心跳，则认为该consumer离开了，之后会执行rebalancing。|int|30000||max.partition.fetch.bytes|每次从单个分区中拉取的消息大小|int|1048576||auto.offset.reset|定义了Consumer在ZooKeeper中发现没有初始的offset时或者发现offset非法时定义Comsumer的行为。earliest: 自动把offset设为最小的offset; latest: 自动把offset设为最新的offset; none: throw exception to consumer; anything else: throw exception to consumer。|string|latest|| connections.max.idle.ms | connection空闲时间达到该配置时，该connection会被关闭 | long | 540000 ||enable.auto.commit|true: consumer 的offset会在被周期地commit。|boolean|true||auto.commit.interval.ms| 自动提交consumer offset的周期。在enable.auto.commit设置为true时起作用|long|5000|| receive.buffer.bytes | socket的接收缓存空间大小,当阅读数据时使用 | int | 65536 || request.timeout.ms | 客户端将等待请求的响应的最大时间,如果在这个时间内没有收到响应，客户端将重发请求;超过重试次数将抛异常 | int | 40000 || send.buffer.bytes | TCP send buffer \(SO\_SNDBUF\) | int | 131072 |



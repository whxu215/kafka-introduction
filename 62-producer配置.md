## 6.2 Producer配置

| 名称 | 描述 | 类型 | 默认值 |
| --- | --- | --- | --- |
| bootstrap.servers | kafka server list. (host1:port1,host2:port2,...) | list |  |
| key.serializer| key序列化类实现Serializer interface | class |  |
|value.serializer| value 序列化类实现Serializer interface| class||
|acks|指producer要求leader在收到几个确认通知后才认为当前消息接受成功。0：producer不会等待任何ack，直接把消息发送出去就不管了，retries配置也不会起作用，因为producer根本不知道消息发送成功还是失败；1：leader把消息写入本地的log后就会回复成功，不会等到followers回复；all：leader会等到所有isr(in-sync replicas)通知都到位后才会回复成功，这个能最高级别确保消息不会丢失|string|1
|buffer.memory|producer端用来存放尚未发送出去的Message的缓冲区大小。缓冲区满了之后可以选择阻塞发送或抛出异常，由block.on.buffer.full的配置来决定|long|33554432|
|compression.type|消息压缩方式none, gzip, snappy, or lz4|string|none|
|retries|如果设置一个大于0的数producer会在发送失败的时候重新发送消息|int|0|
|batch.size|produce尝试把发到同一相partition的消息进行批量发送，该配置就是批量消息数。这对client端与server的性能有很大的提高。|int|16384|
|client.id|用来标识某个client端，该id会被发送到kafka server.|string|""|
|connections.max.idle.ms|connection空闲时间达到该配置时，该connection会被关闭|long|540000|
|linger.ms|Producer默认会把两次发送时间间隔内收集到的所有Requests进行一次聚合然后再发送，以此提高吞吐量，而linger.ms则更进一步，这个参数为每次发送增加一些delay，以此来聚合更多的Message。|long|0|
|max.block.ms|控制block的时长,当buffer空间不够或者metadata丢失时产生block。KafkaProducer.send() 和KafkaProducer.partitionsFor()可能产生block.|long |60000|
|max.request.size|请求的最大字节数。这也是对最大记录尺寸的有效覆盖。注意：server具有自己对消息记录尺寸的覆盖，这些尺寸和这个设置不同。此项设置将会限制producer每次批量发送请求的数目，以防发出巨量的请求。|int|1048576|
|partitioner.class|分区类，实现Partitioner interface.|class|org.apache.kafka.clients.producer.internals.DefaultPartitioner|
|receive.buffer.bytes|socket的接收缓存空间大小,当阅读数据时使用|int|32768|
|request.timeout.ms|客户端将等待请求的响应的最大时间,如果在这个时间内没有收到响应，客户端将重发请求;超过重试次数将抛异常|int|30000|
|send.buffer.bytes|TCP send buffer (SO_SNDBUF)|int|131072|
|block.on.buffer.full|当缓存补耗尽时的处理行为。false:会阻塞max.block.ms时间，如果到了这个时间缓存还是没有释放出足够空间则抛出TimeoutException; true: 当缓存被耗尽时直接抛出BufferExhaustException|boolean|false|
|interceptor.classes|producer在把消息发送到kafka之前的拦截器需实现ProducerInterceptor接口，默认没有|list|null|
|max.in.flight.requests.per.connection|producer每个连接的最大
无ack请求个数。如果该配置大于1，且设置了重试，有可能会导致消息乱序。|int|5|
|metadata.fetch.timeout.ms|取元数据操作的超时时间|long|60000|
|metadata.max.age.ms|元数据过期时间，过了这个时间会再次取一下最新的元数据|long|300000|










































## 6.1 broker配置

| 名称 | 描述 | 类型 | 默认值 |
| --- | --- | --- | --- |
| broker.id | kafka服务ID. 在kafka集群中需唯一 | int | -1 |
| log.dirs | kafka 日志数据目录, 如果有多个以逗号间隔 | string | null |
| zookeeper.connect | zookeeper connect sring | string |  |
| listeners | kafka listener列表，多个以逗号间隔。如果IP指定为0.0.0.0则kafka 绑定到本机所有的IP上；如果未指定IP, kafka绑定到本机默认IP上。示例：PLAINTEXT:\/\/myhost:9092,TRACE:\/\/:9091 PLAINTEXT:\/\/0.0.0.0:9092, TRACE:\/\/localhost:9093 | string | null |
| advertised.listeners | listener 发布到zookeeper供client端使用。如果要发布的listener与上面listeners不一样才需要配置。一般用在该kafka提供公网IP出去给client端用的时候。如果不设置，则用listeners配置 | string | null |
| num.network.threads | 受理网络请求的线程数 | int | 3 |
| num.io.threads | I\/O操作的线程数, 用来完成网络请求 | int | 8 |
| socket.send.buffer.bytes | socket send buffer\(SO\_SNDBUF\) | int | 102400 |
| socket.receive.buffer.bytes | socket receive buffer\(SO\_RCVBUF\) | int | 102400 |
| socket.request.max.bytes | 一个socket请求最大数据字节大小,message.max.bytes\(单个消息最大字节数\)必须要小于这个值 | int | 104857600 |
| num.partitions | 默认每个topic的分区个数，若是在topic创建时候指定的话会被topic创建时的指定参数覆盖 | int | 1 |
| num.recovery.threads.per.data.dir | 启动时做log恢复、或在关闭时做flushing数据的操作线程数 | int | 1 |
| log.flush.interval.messages | log文件”sync”到磁盘之前累积的消息条数 | long | 9223372036854775807 |
| log.flush.interval.ms | log文件”sync”到磁盘的时间间隔。如果在这个时间到达时积累的消息条数未达到，同样也会做“sync”操作 | long | null |
| log.retention.hours | 数据存储的最长时间\(小时\), 过了这个时间的数据会被删除 | int | 168 |
| log.retention.minutes | 数据存储的最长时间\(分钟\), 过了这个时间的数据会被删除。优先级高于log.retention.hours | int | null |
| log.retention.ms | 数据文件存储的最长时间\(毫秒\), 过了这个时间的文件会被删除。优先级高于log.retention.minutes | int | null |
| log.retention.bytes | topic每个分区的文件大小，topic分区文件是由多个segment文件组成,所有segment文件大小总和要小于该配置， 和log.retention.hour任意一个达到要求，都会执行删除，会被topic创建时的指定参数覆盖 | long | -1 |
| log.segment.bytes | topic的分区是以一堆segment文件存储的，这个控制每个segment的大小，会被topic创建时的指定参数覆盖 | int | 1073741824 |
| log.retention.check.interval.ms | log清理周期，每隔这个时间会检查log是否触发了删除的规则，如果触发则会被删除 | long | 300000 |
| zookeeper.connect | zookeeper连接串。e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002" | string | "" |
| zookeeper.connection.timeout.ms | zookeeper连接超时时间, 如果不设置则用zookeeper.session.timeout.ms| string | "" |
|zookeeper.session.timeout.ms| zookeeper session超时时间，就是心跳间隔，如果在这个时间内没有心跳则认为zookeeper已经挂掉了|int|6000|









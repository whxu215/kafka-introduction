## 6.1 broker配置

| 名称 | 描述 | 类型 | 默认值 |
| --- | --- | --- | --- |
| broker.id | kafka服务ID. 在kafka集群中需唯一 | int | -1 |
| log.dirs | kafka 日志数据目录, 如果有多个以逗号间隔 | string | null |
| zookeeper.connect | zookeeper connect sring | string |  |
| listeners | kafka listener列表，多个以逗号间隔。如果IP指定为0.0.0.0则kafka 绑定到本机所有的IP上；如果未指定IP, kafka绑定到本机默认IP上。示例：PLAINTEXT:\/\/myhost:9092,TRACE:\/\/:9091 PLAINTEXT:\/\/0.0.0.0:9092, TRACE:\/\/localhost:9093 | string | null |
| advertised.listeners | listener 发布到zookeeper供client端使用。如果要发布的listener与上面listeners不一样才需要配置。一般用在该kafka提供公网IP出去给client端用的时候。如果不设置，则用listeners配置| string | null |
| num.network.threads | 受理网络请求的线程数 | int | 3|
| num.io.threads | I/O操作的线程数, 用来完成网络请求 | int | 8 |



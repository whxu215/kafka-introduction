## **7.1 Topic数据结构**

_**Zookeeper路径：**_ \/brokers\/topics\/\[topic\]

_**Schema:**_

```
{ "fields" :
    [ {"name": "version", "type": "int", "doc": "version id"},
    {"name": "partitions","type": 
        {"type": "map", "values": {"type": "array", "items":"int", "doc": "a list of
     replica ids"},
    "doc": "a map from partition id to replica list"},
    }
    ]
}
```

_**Example:**_

```
{
"version": 1,
"partitions": {"0": [0, 1, 3] } }
}
```

## **7.2 Partition state数据结构**

_**Zookeeper路径：**_ \/brokers\/topics\/\[topic\]\/partitions\/\[partitionId\]\/state

_**Schema:**_
```
{ "fields":
    [ {"name": "version", "type": "int", "doc": "version id"},
      {"name": "isr","type": {"type": "array", "items": "int", "doc": "an array of the 
        id of replicas in isr"}
      },
      {"name": "leader", "type": "int", "doc": "id of the leader replica"},
      {"name": "controller_epoch", "type": "int", "doc": "epoch of the controller 
        that last updated the leader and isr info"},
      {"name": "leader_epoch", "type": "int", "doc": "epoch of the leader"}
    ]
}
```
_**Example:**_
```
{
"version": 1,
"isr": [0,1],
"leader": 0,
"controller_epoch": 1,
"leader_epoch": 0
}
```

## **7.3 Broker注册信息数据结构**

_**Zookeeper路径：**_ /brokers/ids/[brokerId]

_**Schema:**_

```
{ "fields":
    [ {"name": "version", "type": "int", "doc": "version id"},
      {"name": "host", "type": "string", "doc": "ip address or host name of the broker"},
      {"name": "port", "type": "int", "doc": "port of the broker"},
      {"name": "jmx_port", "type": "int", "doc": "port for jmx"}
    ]
}
```
_**Example:**_

```
{
"version": 1,
"host": "192.168.1.148",
"port": 9092,
"jmx_port": 9999
}
```

## **7.4 Controller epoch数据结构**
_**Zookeeper路径：**_ /controller_epoch -> int (epoch)

***Controller epoch:*** 自增数字，在controller每次选举时该数会加1

## **7.5 Controller 注册信息数据结构**
_**Zookeeper路径：/controller -> int (broker id of the controller)

***Controller:*** Kafka在所有broker中选出一个broker做为controller, 所有partition的leader选举都由controller决定。





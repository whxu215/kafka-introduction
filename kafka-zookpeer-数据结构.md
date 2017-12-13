## 8**.1 Topic数据结构**

_**Zookeeper路径：**_ /brokers/topics/\[topic\]

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

## 8**.2 Partition state数据结构**

_**Zookeeper路径：**_ /brokers/topics/\[topic\]/partitions/\[partitionId\]/state

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

## 8**.3 Broker注册信息数据结构**

_**Zookeeper路径：**_ /brokers/ids/\[brokerId\]

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

## 8**.4 Controller epoch数据结构**

_**Zookeeper路径：**_ /controller\_epoch -&gt; int \(epoch\)

_**Controller epoch:**_ 自增数字，在controller每次选举时该数会加1

## 8**.5 Controller 注册信息数据结构**

\_\*\*Zookeeper路径：/controller

_**Controller:**_ Kafka在所有broker中选出一个broker做为controller。

_**Schema:**_

```
{ "fields":
 [ {"name": "version", "type": "int", "doc": "version id"},
   {"name": "brokerid", "type": "int", "doc": "id of the broker selected as controller"},
 ]
}
```

_**Example:**_

```
{
"version":1,
"brokerid":0,
"timestamp":"1476709166397"
}
```

## 8**.6 Consumer 注册信息数据结构**

\_\*\*Zookeeper路径：/consumers/\[groupId\]/ids/\[consumerId\]

_**Schema:**_

```
{ "fields":
    [ {"name": "version", "type": "int", "doc": "version id"},
      {"name": "pattern", "type": "string", "doc": "can be of static, white_list or     black_list"},
      {"name": "subscription", "type" : {"type": "map", "values": {"type": "int"},
       "doc": "a map from a topic or a wildcard pattern to the number of streams"} } ]
}
```

_**Example:**_

```
A static subscription:
{
"version": 1,
"pattern": "static",
"subscription": {"topic1": 1, "topic2": 2}
}

A whitelist subscription:
{
"version": 1,
"pattern": "white_list",
"subscription": {"abc": 1}
}

A blacklist subscription:
{
"version": 1,
"pattern": "black_list",
"subscription": {"abc": 1}
}
```

## 8**.7 Consumer owner\(所有者\)数据结构**

_**Zookeeper路径：**_ /consumers/\[groupId\]/owners/\[topic\]/\[partitionId\] -&gt; string \(consumerId\)

## 8**.8 Consumer offset数据结构**

_**Zookeeper路径：**_ /consumers/\[groupId\]/offsets/\[topic\]/\[partitionId\] -&gt; long \(offset\)

## 8**.9 Re-assign partitions\(分区重新分配\)数据结构**

\_\*\*Zookeeper路径：/admin/reassign\_partitions

_**Schema:**_

```
{
"fields":[
    {
    "name":"version",
    "type":"int",
    "doc":"version id"
    },
    {
    "name":"partitions",
    "type":{
        "type":"array",
        "items":{
            "fields":[
            {
            "name":"topic",
            "type":"string",
            "doc":"topic of the partition to be reassigned"
            },
            {
            "name":"partition",
            "type":"int",
            "doc":"the partition to be reassigned"
            },
            {
            "name":"replicas",
            "type":"array",
            "items":"int",
            "doc":"a list of replica ids"
            }
            ],
        }
        "doc":"an array of partitions to be reassigned to new replicas"
      }    
    }
  ]
}
```

_**Example:**_

```
{
"version": 1,
"partitions":
  [
    {
    "topic": "Foo",
    "partition": 1,
    "replicas": [0, 1, 3]
    }
  ]
}
```




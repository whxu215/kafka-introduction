## **7.1 Topic数据结构**

***Zookeeper路径：*** \/brokers\/topics\/\[topic\]

***Schema:***
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
***Example:***
```
{
"version": 1,
"partitions": {"0": [0, 1, 3] } }
}
```


## **7.2 Partition state数据结构**
***Zookeeper路径：*** /brokers/topics/[topic]/partitions/[partitionId]/state

***Schema:***

```
{ "fields":
    [ {"name": "version", "type": "int", "doc": "version id"},
      {"name": "isr","type": {"type": "array", "items": "int", "doc": "an array of the 
        id of replicas in isr"}
      },
      {"name": "leader", "type": "int", "doc": "id of the leader replica"},
      {"name": "controller_epoch", "type": "int", "doc": "epoch of the controller that last updated the leader and isr info"},
      {"name": "leader_epoch", "type": "int", "doc": "epoch of the leader"}
    ]
}
```

***Example:***

```

{

"version": 1,

"partitions": {"0": [0, 1, 3] } }

}

```








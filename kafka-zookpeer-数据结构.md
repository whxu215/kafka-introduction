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





## 6.1 Partition中的数据文件

每个partition有自己的数据文件目录。目录名：${topic}-${partitionId}

### 6.1.1 segment中的文件

对于一个partition，里面又有很多大小相等的segment数据文件（这个文件的具体大小可以在`config/server.properties`

中进行设置），这种特性可以方便old segment file的快速删除。

segment file：

* **组成：**由2部分组成，分别为index file和data file，这两个文件是一一对应的，后缀”.index”和”.log”分别表示索引文件和数据文件；
* **命名规则：**partition的第一个segment从0开始，后续每个segment文件名为上一个segment文件最后一条消息的offset,ofsset的数值最大为64位（long类型），20位数字字符长度，没有数字用0填充。如下图所示：

![](/assets/segment-file.png)

segment file中index与data file对应关系图:![](/assets/index-file.png)segment的索引文件中存储着大量的元数据，数据文件中存储着大量消息，索引文件中的元数据指向对应数据文件中的message的物理偏移地址。以索引文件中的`3，497`为例，在数据文件中表示第3个message（在全局partition表示第368772个message），以及该消息的物理偏移地址为497。

**注：**Partition中的每条message由offset来表示它在这个partition中的偏移量，这个offset并不是该Message在partition中实际存储位置，而是逻辑上的一个值（如上面的3），但它却唯一确定了partition中的一条Message（可以认为offset是partition中Message的id）。

### 6.1.2 message文件

message中的物理结构为：

![](/assets/message-format.png)

参数说明：

| 关键字 | 解释说明 |
| :--- | :--- |
| 8 byte offset | 在parition\(分区\)内的每条消息都有一个有序的id号，这个id号被称为偏移\(offset\),它可以唯一确定每条消息在parition\(分区\)内的位置。即offset表示partiion的第多少message |
| 4 byte message size | message大小 |
| 4 byte CRC32 | 用crc32校验message |
| 1 byte “magic” | 表示本次发布Kafka服务程序协议版本号 |
| 1 byte “attributes” | 表示为独立版本、或标识压缩类型、或编码类型 |
| 4 byte key length | 表示key的长度,当key为-1时，K byte key字段不填 |
| K byte key | 可选 |
| value bytes payload | 表示实际消息数据 |

  



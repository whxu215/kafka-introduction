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




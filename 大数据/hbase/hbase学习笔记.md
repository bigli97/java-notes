## 大纲

- 基本知识
- 基础指令
- 架构
- **HRegionServer**
- Hlog
- Hmaster

## 基础知识

**为什么HBase能存储海量的数据？**

因为它在HDFS的基础之上构建的，HDFS是分布式文件系统**。**HBase的数据其实也是存储在HDFS上的。

**已经有HDFS了，为什么还要用Hbase？**

> HDFS是可以存储海量的数据的，它就是为海量数据而生的。它也有明显的缺点：不支持随机修改，查询效率低，对小文件支持不友好。
> HDFS是文件系统，而HBase是数据库，其实也没可比性。「**HBase当做是MySQL，把HDFS当做是硬盘**。HBase只是一个NoSQL数据库，把数据存在HDFS上」。
> **总结**：HBase在HDFS之上提供了**高并发的随机写和支持实时查询**，这是HDFS不具备的。

**列式存储**

Hbase不同于MySQL就是列式存储，MySQL是行式存储，比如下图

![img](https://pic2.zhimg.com/80/v2-426bf33a570563f7a6a3ef66f2262e81_720w.jpg)

列式存储为下图

![img](https://pic2.zhimg.com/80/v2-d301231eb63ebae3b77f4993a3f3af7d_720w.jpg)

**为什么用列式存储**

从上图可以看出，存相同的数据，列式存储空间能够充分利用

在HBase里边，定位一行数据会有一个唯一的值，这个叫做**行键**(RowKey)。

在HBase里边，先有列族，后有列。

**什么是列族**？可以简单理解为：列的属性类别

**什么是列修饰符**？先有列族后有列，在列族下用列修饰符来标识一列。

![img](https://pic1.zhimg.com/80/v2-f2f6ef84781de84a62f08b9c9141fd74_720w.jpg)样例

![img](https://pic3.zhimg.com/80/v2-d4ba2c9b93ef66ff4bcb4c4a59f0eb6e_720w.jpg)放入具体的值

![img](https://pic4.zhimg.com/80/v2-0197bae0e9d1145d30b55e6e9a7ece1b_720w.jpg)这个跟上面一样

HBase表的每一行中，列的组成都是**灵活的**，行与行之间的列**不需要相同**。如图下：

![img](https://pic2.zhimg.com/80/v2-afb00e1e4548277e69e1488c78609881_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-a3e5fdbae582efcbc11ec51a61144456_720w.jpg)

**一个列族下可以任意添加列，不受任何限制**

数据写到HBase的时候都会被记录一个时间戳，这个时间戳被我们当做一个版本。比如说，我们修改或者删除某一条的时候，本质上是往里边新增一条数据，记录的版本加一了而已。

比如

![img](https://pic4.zhimg.com/80/v2-3ea852ef89c04d56b167628f8e277773_720w.jpg)

现在要把这条记录的值改为40，实际上就是多添加一条记录，在读的时候按照时间戳读最新的记录。在外界「看起来」就是把这条记录改了。

![img](https://pic1.zhimg.com/80/v2-796c9b57c5a35bcb4b50f31df90e00e8_720w.jpg)

HBase本质上其实就是**Key-Value**的数据库

![img](https://pic2.zhimg.com/80/v2-7a9051c5a86319e20157cb07c8c16169_720w.jpg)

Key由RowKey(行键)+ColumnFamily（列族）+Column Qualifier（列修饰符）+TimeStamp（时间戳--版本）+KeyType（类型）组成，而Value就是实际上的值。

如果我们要准确定位一条数据，那就得（RowKey+Column+时间戳）。

如果要删除一条数据怎么做？实际上也是增加一条记录，只不过我们在KeyType里边设置为“Delete”就可以了。



## 基础指令

![img](https://pic2.zhimg.com/80/v2-10f35013d2c29ad614a8094ef3c6b371_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-a52281c26754d853491f80e245c24352_720w.jpg)

主要是上面的都需要知道，指令最好**手敲**，下面的了解即可

## 架构

![img](https://pic1.zhimg.com/80/v2-055cc2821ece4d6e1546fd0ae877cc00_720w.jpg)

> 1、Client客户端，它提供了访问HBase的接口，并且维护了对应的cache来加速HBase的访问。
> 2、Zookeeper存储HBase的元数据（meta表），无论是读还是写数据，都是去Zookeeper里边拿到meta元数据**告诉给客户端去哪台机器读写数据**
> 3、HRegionServer它是处理客户端的读写请求，负责与HDFS底层交互，是真正干活的节点。

**总结大致的流程**就是：client请求到Zookeeper，然后Zookeeper返回HRegionServer地址给client，client得到Zookeeper返回的地址去请求HRegionServer，HRegionServer读写数据后返回给client。

![img](https://pic4.zhimg.com/80/v2-698a7cb1c3bb0131f5f13d08f18dba97_720w.jpg)

总结

> 1：HBase是一个NoSQL数据库，一般我们用它来存储海量的数据（因为它基于HDFS分布式文件系统上构建的）
> 2：HBase的一行记录由一个RowKey和一个或多个的列以及它的值所组成。先有列族后有列，列可以随意添加。
> 3：HBase的增删改记录都有「版本」，默认以时间戳的方式实现。
> 4：RowKey的设计如果没有特殊的业务性，最好设计为散列的，这样避免热点数据分布在同一个HRegionServer中。
> 5：HBase的读写都经过Zookeeper去拉取meta数据，定位到对应的HRegion，然后找到HRegionServer



## **HRegionServer**

![img](https://pic1.zhimg.com/80/v2-908566468f38782f5fa85bebd78a58d8_720w.jpg)HRegionServer内部

**HBase一张表的数据会分到多台机器上的**。那HBase是怎么切割一张表的数据的呢？用的就是**RowKey**来切分，其实就是表的横向切割。

![img](https://pic4.zhimg.com/80/v2-fc232ebaf7dee263af8dca2d383d6883_720w.jpg)

说白了就是一个HRegion上，存储HBase表的一部分数据。

![img](https://pic3.zhimg.com/80/v2-5dd5ac868bd6187d99ee34bab0f1472e_720w.jpg)

**图中的Store**：一个列族的数据是存储在一起的，所以一个列族的数据是存储在一个Store里边的。HBase是基于列族存储的（毕竟物理存储，一个列族是存储到同一个Store里的）

![img](https://pic4.zhimg.com/80/v2-be7aa77279502268bbeb155eabb3e2a7_720w.jpg)

**Store里边有啥**？有Mem Store、Store File、HFile，我们再来看看里边都代表啥含义。

![img](https://pic2.zhimg.com/80/v2-9f1dd33e86a53ba8012e3bc622d6761d_720w.jpg)

HBase在写数据的时候，会先写到**Mem Store**，当MemStore超过一定阈值，就会将内存中的数据刷写到硬盘上，形成**StoreFile**，而StoreFile底层是以**HFile**的格式保存，HFile是HBase中KeyValue数据的存储格式。

## Hlog

![img](https://pic4.zhimg.com/80/v2-ea753d751227ff13ba630128a11fb7a3_720w.jpg)

> 我们写数据的时候是先写到内存的，为了防止机器宕机，内存的数据没刷到磁盘中就挂了。我们在写Mem store的时候还会写一份HLog。
> 这个HLog是顺序写到磁盘的，所以速度还是挺快的
> HRegionServer是真正干活的机器（用于与hdfs交互），我们HBase表用RowKey来横向切分表
> HRegion里边会有多个Store，每个Store其实就是一个列族的数据（所以我们可以说HBase是基于列族存储的）
> Store里边有Men Store和StoreFile(HFile)，其实就是先走一层内存，然后再刷到磁盘的结构

## Hmaster

![img](https://pic3.zhimg.com/80/v2-381ced9f50969325fce81e5f75282512_720w.jpg)

> HMaster会处理 HRegion 的分配或转移。如果我们HRegion的数据量太大的话，HMaster会对拆分后的Region**重新分配RegionServer**。（如果发现失效的HRegion，也会将失效的HRegion分配到正常的HRegionServer中）
> **HMaster会处理元数据的变更和监控RegionServer的状态。**
> **RowKey的设计**
> RowKey是**唯一的**，毕竟它是行键，有了它我们才可以唯一标识一条数据的。
> 在HBase里边提供了三种的查询方式：
> 全局扫描
> 根据一个RowKey进行查询
> 根据RowKey过滤的范围查询
> **根据一个RowKey进行查询**
> RowKey是会按**字典序排序**的，我们HBase表会用RowKey来横向切分表。
> 无论是读和写我们都是用RowKey去定位到HRegion，然后找到HRegionServer。这里有一个很关键的问题：**那怎么知道这个RowKey是在这个HRegion上的**？
> HRegion上有两个很重要的属性：**start-key**和**end-key**。
> 我们在定位HRegionServer的时候，实际上就是定位我们这个RowKey在不在这个HRegion的start-key和end-key范围之内，如果在，说明我们就找到了。
>
> 这个时候会带来一个问题：由于我们的RowKey是以字典序排序的，如果我们对RowKey没有做任何处理，那就有可能存在**热点数据**的问题。

例如现在我们的RowKey如下：

![img](https://pic1.zhimg.com/80/v2-847808c1a6518daf4e7586d34d39af48_720w.png)

> Java3yxxx开头的RowKey很多，而其他的RowKey很少。如果我们有多个HRegion的话，那么存储Java3yxxx的HRegion的数据量是最大的，而分配给其他的HRegion数量是很少的。
>
> 关键是我们的查询也几乎都是以java3yxxx的数据去查，这会导致某部分数据会集中在某台HRegionServer上存储以及查询，而其他的HRegionServer却很空闲。
>
> **对RowKey散列**就好了，那分配到HRegion的时候就比较均匀，少了热点的问题。
> **生成RowKey时,尽量进行加盐或者哈希的处理,这样很大程度上可以缓解数据热点问题.**
> **根据RowKey范围查询**
> 以上是针对通过**RowKey单个查询**的业务的，如果我们是根据**RowKey范围查询**的，那没必要上面那样做。
> HBase将RowKey设计为字典序排序，如果不做限制，那很可能类似的RowKey存储在同一个HRegion中。那我正好有这个场景上的业务，那我查询的时候不是快多了吗？**在同一个HRegion就可以拿到我想要的数据了。**
> 举个例子：我们会间隔几秒就采集直播间热度，将这份数据写到HBase中，然后业务方经常要把主播的一段时间内的热度给查询出来。
> 我设计好的RowKey，将该主播的一段时间内的热度都写到**同一个HRegion上**，拉取的时候只要访问一个HRegionServer就可以得到全部我想要的数据了，那查询的速度就快很多。
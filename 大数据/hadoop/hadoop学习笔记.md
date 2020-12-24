## 大纲

- 基础知识
- HDFS
- NameNode
- DataNode
- SecondaryNameNode
- HDFS读写文件
- MapReduce
- Yarn
- Shell操作

## 基础知识

**是什么？**

以下来自官网

> The Apache™ Hadoop® project develops open-source software for reliable, scalable, distributed computing.
> The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-available service on top of a cluster of computers, each of which may be prone to failures.

总结为一句话就是**存储**海量数据和**分析**海量数据的工具。

- hadoop的框架最核心的设计就是：**HDFS**和**MapReduce**。
- HDFS为海量的数据提供了**存储**，则MapReduce为海量的数据提供了**计算**
- HDFS为一个分布式的，有冗余备份的，可以动态扩展的用来存储大规模数据的大硬盘。
- MapReduce为一个计算引擎，按照MapReduce的规则编写Map计算/Reduce计算的程序，可以完成计算任务。

**能做什么？**

- 大数据存储
- 分布式存储日志处理
- 擅长日志分析ETL:数据抽取到oracle、mysql、DB2、mongdb及主流数据库
- 机器学习: 比如Apache Mahout项目
- 搜索引擎:Hadoop + lucene实现
- 数据挖掘：目前比较流行的广告推荐，个性化广告推荐

## HDFS

用图解的方式理解

![img](https://pic3.zhimg.com/80/v2-b4133335f00ba0eaab0a1892da72b31a_720w.jpg)HDFS是干什么的

![img](https://pic1.zhimg.com/80/v2-5bd6e300a2f843f77ed1bfe7126042a4_720w.jpg)HDFS处理文件的思路

![img](https://pic4.zhimg.com/80/v2-aab493fc8a02668eadf0e4fea2d5b187_720w.jpg)

![img](https://pic4.zhimg.com/80/v2-3064be55c17267679af0576a00d35193_720w.jpg)HDFS架构

![img](https://pic3.zhimg.com/80/v2-7e81d28eb1cec1f90b76f978a40801b2_720w.jpg)HDFS读取过程

![img](https://pic3.zhimg.com/80/v2-310f2ef8ecdfc5dc71ad52f96b1046e2_720w.jpg)HDFS写入过程

**HDFS的实现思想**

1. 通过分布式集群来存储文件
2. 文件存储到集群中是切分为block去存的
3. 文件的block存放在若干个datanode节点上
4. hdfs中的文件与真实的block之间有映射关系，由namenode管理
5. 每个block集群中存储有多个副本，防止一台挂掉后数据丢失，提高数据可靠性，同时访问一个资源，资源多了，吞吐量也就大了，提高了访问吞吐量

## NameNode

> 如同一个**管理者**，负责文件**在哪些机器上存的，如何存的**。
> 在 HDFS 里， Name node 保存了整个文件系统信息，包括文件和文件夹的结构。HDFS 也是把文件和文件夹表示为 inode, 每个 inode 有自己的所有者，权限，创建和修改时间等等。
> HDFS 可以存很大的文件，所以每个文件都被分成一些 data block，存在不同机器上, name node 就负责记录一个文件有哪些 data block，以及这些 data block 分别存放在哪些机器上。
> Name nodes 还负责管理文件系统常用操作，比如创建一个文件，重命名一个文件，创建一个文件夹，重命名一个文件夹等。

**技术细节**

1. 在Hadoop1.0中，NameNode存在单点故障问题

2. NameNode维护HDFS中的元数据信息：

3. 1. 文件和Block之间关系的信息
   2. Block数量信息
   3. Block和DataNode之间的关系信息
   4. 元数据格式参照：FileName replicas block-Ids id2host。例如： /test/a.log,3,{b1,b2},[{b1:[h0,h1,h3]},{b2:[h0,h2,h4]}]

4. NameNode通过RPC心跳机制监测DataNode

5. 每一条元数据大概是**150B**大小

6. 元数据信息存储在内存以及文件中，内存中为实时信息，这样做的目的是为了快速查询

7. 元数据信息会持久化到NameNode节点的硬盘上，持久化目录的路径是由core-site.xml的dfs.tmp.dir属性来决定的。此参数如果不配置，默认是放在/tmp

8. 存储元数据的目录：dfs/name/current

9. 持久化的文件包括：

10. 1. fsimage：元数据镜像文件。存储某NameNode元数据信息，并不是实时同步内存中的数据。
    2. edits：操作日志文件,记录了NameNode所要执行的操作

11. 当有写请求时，NameNode会首先写该操作先写到磁盘上的edits文件中，当edits文件写成功后才会修改内存，并向客户端返回成功信号，而此时fsimage中的数据并没有发生改动。所以，fsimage中的数据并不是实时的数据，而是在达到条件时再进行更新，更新过程需要SecondaryNameNode参与

12. 无论是Hadoop1.0还是2.0，当HDFS启动,NameNode会做一次edits和fsimage合并的操作。这样做的目的是确保fsimage里的元数据更新。

13. 可以通过指令手动合并：hadoop dfsadmin -rollEdits

14. 当HDFS启动的时候，NameNode会将fsimage中的元数据信息加载到内存中

15. 当HDFS启动时，每个DataNode会向NameNode汇报自身的存储的信息，比如存储了哪些文件块，块大小，块id等。NameNode收到这些信息之后，会做汇总和检测，检测数据是否完整，复本数量是否达到要求，如果检测出现问题，HDFS会进入安全模式，在安全模式做数据或副本的复制，直到修复完成后，安全模式自动退出。

16. 如果HDFS处于安全模式，只能对外读服务，不能写服务。

## DataNode

> 存储 data block 的机器叫做 Data nodes.
> 在读写过程中，Data nodes 负责直接把用户读取的文件 block 传给 client，也负责直接接收用户写的文件。

**当我们读取一个文件时**

> HDFS client 联系 Name nodes，获取文件的 data blocks 组成、以及每个 data block 所在的机器以及具体存放位置
> HDFS client 联系 Data nodes, 进行具体的读写操作；
> 在读写一个文件时，当我们从 Name nodes 得知应该向哪些 Data nodes 读写之后，我们就直接和 Data node 打交道，不再通过 Name nodes.

**技术细节**

1. 在HDFS中，数据是存放在DataNode上面的，并且是以Block的形式存储的
2. 存储Block的目录：dfs/data/current/BP-1998993700-192.168.150.137-1537051327964/current/finalized/subdir0/subdir0
3. DataNode节点会不断向NameNode节点发送心跳报告
4. 在HDFS启动的时候，每个DataNode将当前存储的数据块信息告知NameNode节点
5. DataNode通过向NameNode主动发送心跳保持与其联系，默认是每隔3秒发送一次心跳信息
6. 心跳信息包含这个节点的状态以及数据块信息
7. 如果10分钟都没收到dn的心跳，则认为其已经lost，那么NameNode会copy这个DataNode上的Block到其他DataNode上
8. 在HDFS中，DataNode上存储的复本Replication是多复本策略。默认是三个
9. 如果是伪分布式模式，副本只能配置1个，因为如果副本数量>1，会导致HDFS一致处于安全模式而不能退出

## SecondaryNameNode

1. SecondaryNameNode并不是NameNode的热备份，而是协助者帮助NameNode进行元数据的合并，

2. 合并过程可能会造成极端情况下的数据丢失，此时可以从SecondaryNameNode中恢复部分数据，但是无法恢复全部

3. SecondaryNameNode是Hadoop1.0机制。**Hadoop2.0已经弃用**。如果是Hadoop2.0的伪分布式，还是会看到SecondaryNameNode进程；如果是Hadoop2.0的完全分布式，SecondaryNameNode已经舍弃

4. 合并时机：

5. 1. 根据配置文件设置的时间间隔：fs.checkpoint.period 默认3600秒
   2. 根据配置文件设置的edits log大小 fs.checkpoint.size 默认64MB
   3. 当HDFS启动时候,也会触发合并

6. 合并过程：

7. 1. 达到条件后SecondaryNameNode会将nn中的fsimage和edits文件通过网络拷贝过来
   2. 同时NameNode中会创建一个新的edits.new文件，新的读写请求会写入到这个edits.new中
   3. 在SecondaryNameNode中，会将拷贝过来的fsimage和edits加载到内存中，将数据进行合并，然后将合并后的数据写到一个新的文件fsimage.ckpt中
   4. 最后SecondaryNameNode将合并完成的fsimage.ckpt文件拷贝回NameNode中替换之前的fsimage，同时NameNode再将edtis.new重命名为edits

![img](https://pic1.zhimg.com/80/v2-1ab486a3f07a4803b8f8944f5dd5a660_720w.jpg)

## HDFS读写文件

**HDFS写文件**

假如上传一个200M的文件，从**client**上传到NameNode

1：client向namenode发起一个申请。先看文件是否重名。再看是否有权限，如果namenode同意，进行2

![img](https://pic4.zhimg.com/80/v2-def385fd8598b98c0bc1c82c92945c0f_720w.png)

2：将文件切块。先进行划线，此时文件还是一块，开输出流。请求上传第一个block块（0-128M）

![img](https://pic2.zhimg.com/80/v2-768062b996a2e850aa74bd34b1dada71_720w.png)

3：namenode返回三个datanode，表示采用三个节点存储数据。DN1为client最近的，DN2,3是DN1选出来的。两个问题怎么就最近？DN1怎么选？

![img](https://pic3.zhimg.com/80/v2-99711124982df3058f3da39947a3a066_720w.png)

4：client向datanode1请求建立通道，DN1向DN2，DN2向DN3

![img](https://pic2.zhimg.com/80/v2-7404894f290a7417ef03ea55b2b4d25d_720w.jpg)

串联通道，不是并联（为什么？）

![img](https://pic2.zhimg.com/80/v2-7c5924c56c3a4c9837ec5190153ac079_720w.jpg)

5：应答成功后，开始传数据。不是等1完了在给2传，有个队列，第一块（128M）的发完，发第二块

![img](https://pic4.zhimg.com/80/v2-1dec86d4ac53c627295d0f26813adf0b_720w.jpg)

> HDFS存储数据的最小单位是块
> 如果建立通道的过程中失败，抛出异常
> 如果传输数据的时候失败，上传失败
> 如果后两失败，传输还可以进行，他会去找DN7,8

![img](https://pic3.zhimg.com/80/v2-014ccd6a35e5ed90733a92f99a78366e_720w.jpg)

附个总图

![img](https://pic3.zhimg.com/80/v2-d2e958d63b77cb6b5d5bb4c11374e2e6_720w.jpg)

**HDFS读文件**

相较于写文件来说，由于少了副本的选择所以相对于简单一点

1：client向NameNode申请下载文件，还是看文件是否存在，然后看权限，有的话响应

![img](https://pic4.zhimg.com/80/v2-178d8dbd6c5fedc09dc6b0d3691bf257_720w.jpg)

2：开一个输入流，请求下载第一个block（0-128M）NameNode返回三个（个数取决于副本数量）DN，表示采用这三个节点的数据

3：client只会和第一个DN建立通道，应答成功后传输，DN2,3为备胎，三个有一个成功都能下载，都不成功下载失败，最后NameNode告诉client数据传输完毕。

![img](https://pic2.zhimg.com/80/v2-3c9a46f84ca8b7c84953b7e6fbb056cd_720w.jpg)

## MapReduce

由于它实操比较繁琐，做计算这块基本不直接用了，像spark之类的就帮他做了。这块的实操比较少，像它的worldcount，API操作我就不写了，想了解的话自行了解一下吧

**什么是MapReduce**

综合来看，MapReduce就是你有很多各种各样的蔬菜水果面包（Input），有很多厨师，不同的厨师分到了不同的蔬菜水果面包，自己主动去拿过来（Split），拿到手上以后切碎（Map），切碎以后给到不同的烤箱里，冷藏机里（Shuffle），冷藏机往往需要主动去拿，拿到这些东西存放好以后会根据不同的顾客需求拿不同的素材拼装成最终的结果，这就是Reduce，产生结果以后会放到顾客那边等待付费（Ticket），这个过程是Finalize。

所以MapReduce是六大过程，简单来说：**Input, Split, Map, Shuffle, Reduce, Finalize**。

![img](https://pic4.zhimg.com/80/v2-43a57d038df5998cdb777a3dafbb97af_720w.jpg)

MR框架对于程序猿的最大意义在于，不需要掌握分布式计算编程，不需要考虑分布式编程里可能存在的种种难题，比如任务调度和分配、文件逻辑切块、位置追溯、工作。这样，**程序员能够把大部分精力放在核心业务层面上，大大简化了分布式程序的开发和调试周期。**

**想了解Map的shuffle和Reduce的shuffle看过来，不然直接跳**

**Map的shuffle**

> Map端会处理输入数据并产生中间结果，这个中间结果会写到本地磁盘，而不是HDFS。每个Map的输出会先写到内存缓冲区中，当写入的数据达到设定的阈值时，系统将会启动一个线程将缓冲区的数据写到磁盘，这个过程叫做spill。（溢出，理解为内存溢出的部分写到磁盘的过程）
> 　　在spill写入之前，会先进行二次排序，首先根据数据所属的partition（分割）进行排序，然后每个partition中的数据再按key来排序。partition的目是将记录划分到不同的Reducer上去，以期望能够达到负载均衡，以后的Reducer就会根据partition来读取自己对应的数据。接着运行combiner(如果设置了的话)，combiner（整合）的本质也是一个Reducer，其目的是对将要写入到磁盘上的文件先进行一次处理，这样，写入到磁盘的数据量就会减少。最后将数据写到本地磁盘产生spill文件(spill文件保存在{mapred.local.dir}指定的目录中，Map任务结束后就会被删除)。
> 　　最后，每个Map任务可能产生多个spill文件，在每个Map任务完成前，会通过多路归并算法将这些spill文件归并成一个文件。至此，Map的shuffle过程就结束了。

**Reduce的shuffle**

> 主要包括三个阶段，copy、sort(merge)和reduce。
> 　　首先要将Map端产生的输出文件拷贝到Reduce端，但每个Reducer如何知道自己应该处理哪些数据呢？因为Map端进行partition的时候，实际上就相当于指定了每个Reducer要处理的数据(partition就对应了Reducer)，所以Reducer在拷贝数据的时候只需拷贝与自己对应的partition中的数据即可。每个Reducer会处理一个或者多个partition，但需要先将自己对应的partition中的数据从每个Map的输出结果中拷贝过来。
> 　　接下来就是sort阶段，也成为merge阶段，因为这个阶段的主要工作是执行了归并排序。从Map端拷贝到Reduce端的数据都是有序的，所以很适合归并排序。最终在Reduce端生成一个较大的文件作为Reduce的输入。
> 　　最后就是Reduce过程了，在这个过程中产生了最终的输出结果，并将其写到HDFS上。

![img](https://pic4.zhimg.com/80/v2-d74d9e6f3cd1e66c92723d0dc4a1da3b_720w.jpg)

参考文章：《Hadoop权威指南》

## Yarn

**什么是yarn？**

在古老的 Hadoop1.0 中，MapReduce 的 JobTracker 负责了太多的工作，包括资源调度，管理众多的 TaskTracker 等工作。这自然是不合理的，于是 Hadoop 在 1.0 到 2.0 的升级过程中，便将 JobTracker 的资源调度工作独立了出来，而这一改动，直接让 Hadoop 成为大数据中最稳固的那一块基石。**而这个独立出来的资源管理框架，就是 Yarn** 。

**为什么用yarn？**

防止集群累垮

**三个主要组件**

> ResourceManager；ApplicationMaster；NodeManager
> **ResourceManager**
> 整个系统有且只有一个 RM ，来负责资源的调度。它也包含了两个主要的组件：定时调用器(Scheduler)以及应用管理器(ApplicationManager)。
> **定时调度器(Scheduler)**：从本质上来说，定时调度器就是一种策略，或者说一种算法。当 Client 提交一个任务的时候，它会根据所需要的资源以及当前集群的资源状况进行分配。注意，它只负责向应用程序分配资源，并不做监控以及应用程序的状态跟踪。
> **应用管理器(ApplicationManager)**：应用管理器就是负责管理 Client 用户提交的应用。定时调度器（Scheduler）不对用户提交的程序监控，监控应用的工作正是由应用管理器（ApplicationManager）完成的。
> 主要作用如下：
> 1：处理客户端请求
> 2：监控NOdeManage
> 3：启动或监控ApplicationMaster
>
> **NodeManager**
> NodeManager 是 ResourceManager 在每台机器的上代理，负责容器的管理，并监控他们的资源使用情况（cpu，内存，磁盘及网络等），以及向 ResourceManager/Scheduler 提供这些资源使用报告。
> **ApplicationMaster**
> 每当 Client 提交一个 Application 时候，就会新建一个 ApplicationMaster 。由这个 ApplicationMaster 去与 ResourceManager 申请容器资源，获得资源后会将要运行的程序发送到容器上启动，然后进行分布式计算。
> 为什么是把运行程序发送到容器上去运行？因为海量数据移动成本太大，既然大数据难以移动，那我就把容易移动的应用程序发布到各个节点进行计算，这就是大数据分布式计算的思路

**Yarn执行流程**

![img](https://pic4.zhimg.com/80/v2-ec391c61052307b1bf519fcabd61180f_720w.jpg)

1. 客户端向yarn提交作业，首先找 RM 分配资源；
2. RM 接收到作业以后，会与对应的 NM 建立通信；
3. RM 要求 NM 创建一个 Container 来运行 ApplicationMaster 实例；
4. ApplicationMaster 会向 RM 注册并申请所需资源，这样 Client 就可以通过 RM 获知作业运行情况；
5. RM 分配给 ApplicationMaster 所需资源，ApplicationMaster 在对应的 NM 上启动 Container；
6. Container 启动后开始执行任务，ApplicationMaster 监控各个任务的执行情况并反馈给 RM；

其中 ApplicationMaster 是可插拔的，可以替换为不同的应用程序。

## Shell操作

**什么是shell？**

> Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
> Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

**shell脚本**

> Shell 脚本（shell script），是一种为 shell 编写的脚本程序。
> 业界所说的 shell 通常都是指 shell 脚本，但是shell 和 shell script 是两个不同的概念。

**HDFS命令分类**

可以分为3类

**本地--->HDFS（比如上传）**

**HDFS--->本地（比如下载）**

**HDFS---->HDFS（自己跟自己玩）**

本地--->HDFS（比如上传）

> put
> copyFromLocal(相当于put)
> moveFromLocal（剪切）
> appendTofile（追加比如将2.txt的内容追加到1.txt）
> HDFS--->本地（比如下载）
> get
> getmerge
> copyToLocal
> HDFS---->HDFS
> cp
> mv
> chown
> chgrp（更改所属群组）
> chmod
> mkdir
> du
> df
> cat
> rm
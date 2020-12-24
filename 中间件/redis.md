# nosql概述

## 为什么使用nosql

一句话：大数据时代的到来！

nosql处理大数据的情况

 

## 什么是nosql（不仅仅是sql）

not only sql

泛指非关系型数据库，随着web2.0的诞生！传统的关系型数据库很难对付！尤其是超大规模的高并发的社区！

暴露出来难以克服的问题。

 

## nosql的特点

数据之间没有关系，很好扩展

大数据量高性能（redis一秒写8万次，读取11万，性能高）

数据类型是多样的。不需要事先设计数据库

 

## 传统的RDBM

- -结构化组织
- -sql
- -数据和关系都存在单独的表里
- -严格的一致性
- -基础的事务

nosql

- 没有固定的查询语言
- CAP定理和BASE
- 高性能，高可用，高可扩展性



## 大数据时代的3V和3高

3v描述问题的。3高解决问题的

1、海量Volume

2、多样Variety

3、实时Velocity

三高，高并发，高可扩，高性能

 

## CAP理论

C：consistency强一致性

A：availability高可用性

P：partition tolerance分区容错性

![image-20201224163343765](https://gitee.com/leidl97/picture/raw/master/img/20201224163343.png)

CAP理论：3进2。一个分布式系统不可能同时满足一致性，可用性和分区容错性三个需求。最多同时满足两个

![image-20201224163403318](https://gitee.com/leidl97/picture/raw/master/img/20201224163403.png)

CA-----MYSQL

CP-----REDIS

 

BASE就是为了解决关系数据库强一致性引起的问题而引起可用性降低而提出的解决方案

 

基本可用Bastic Available

软状态soft

最终一致 Eventually consistent

 

它的思想是通过让系统放松对某一时刻的要求来换取系统整体伸缩性和性能上的改观。因为大型系统往往由于地域分布和极高性能的要求，不可能采用分布式事务来完成这些指标，要想获得这些指标，必须采用另外一种方式来完成。BASE就是解决这个问题的方法。

## nosql四大类

### KV键值对

新浪：redis

美团：redis+tair

阿里，百度：redis+memecache

 

### 文档型数据库（bson格式和json一样）

mongdb（一般要掌握）分布式存储的数据库，是nosql和关系型数据库的交集

 

### 列存储数据库

Hbase

分布式文件系统

 

### 图形关系数据库

不是存图形的，放的是关系，比如朋友圈社交网络，广告推荐

neo4j，infogrid

![image-20201224163642036](https://gitee.com/leidl97/picture/raw/master/img/20201224163642.png)

# 基础知识

## 是什么

Redis（远程字典服务器）是一个使用ANSIC编写的开源、支持网络、基于内存、可选持久性的键值对存储数据库。来源于维基百科

基于内存，断电丢失。持久化很重要（rdb，aof）

效率高，高速缓存

发布订阅系统

地图信息分析

计时器，计数器



## 能做什么

内存存储和持久化（RDB,AOF）

发布，订阅消息系统

定时器，计数器

事务的控制



## 五大类型

string字符串

hash哈希，类似Java里的map

list列表。链表

set集合

zset有序集合



## 特性

1、多样的数据类型

2、持久化

3、集群

4、事务



## 配置文件信息

1、日志有四个级别，debug级别最详细，越往后越少

2、默认的库有16个。select进行切换

3、maxmemory-policy设置过期策略

（1）volatille-lru:使用LRU算法溢出key，只对设置了过期时间的键。volatille（保护的）

（2）allkeys-lru:使用LRU算法移除key

（3）volatille-random：在过期集合中移除随机的key，只对设置了过期时间的键

（4）allkeys-random:溢出随机的key

（5）volatile-ttl:溢出那些TTL值最小的key，即那些最近要过期的key

（6）noevication:不进行移除，针对写操作，只是返回错误信息

4、其他配置信息

redis默认不是以守护线程方式运行，可以通过daemonize.no进行配置

当redis以守护线程的方式运行时，redis默认会把pid写入/var/run/redis.pid文件，可以通过pid file指定

指定redis的默认端口 port

绑定的主机地址 bind

当客户端闲置多长时间后关闭连接，如果为0，表示关闭该功能timeout

指定日志记录级别，一共有四个级别debug,verbose,notice,warning.默认为notice loglevel

日志记录方式，默认为标准输出，如果redis为守护线程启动，这里又为标准输出，日志会发送给/dev/null

本地数据库存放目录 dir /

redis密码默认关闭 requirepass

同一时间最大连接数，默认为无限制 maxclients

# 进阶知识

## 事务

[**https://redis.io/topics/transactions**](https://redis.io/topics/transactions)

#### **事务是什么**

可以一次执行多个命令，本质是一组命令的集合，一个事务中的命令都会序列化，按顺序地串行化执行而不会被其他命令插入，不许加塞

![image-20201224164351463](https://gitee.com/leidl97/picture/raw/master/img/20201224164351.png)

#### 事务能干什么

一个队列中，一次性，顺序性，排他性地执行一系列命令

 

#### 事务怎么用

![image-20201224164428339](https://gitee.com/leidl97/picture/raw/master/img/20201224164453.png)

#### **事务命令**

![image-20201224164515561](https://gitee.com/leidl97/picture/raw/master/img/20201224164515.png)

**全体连坐**，中途有一个错误（指编译期错误，添加队列时直接error），则事务执行失败

**冤头债主**（谁错找谁），中途有一个错误（指运行期错误，比如添加队列时为Queued。比如iner k1(k1为非数字)），则只有报错的任务执行失败

前两个代表redis对事务的支持为部分支持

watch监控：乐观锁/悲观锁/CAS（check and set）

表锁：一致性极强，并发性极差

悲观锁：对事情是悲观的，每次去拿数据的时候都认为别人会修改，为了杜绝这种情况，对表进行上锁，比如行锁，表锁等，读锁，写锁等。适用于多写的情况。

而乐观锁总是假设最好的情况，每次去拿数据的时候都认为别人不会修改。所以**不会上锁**，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。策略：提交的版本必须大于当前的版本才能提交。适用于多读的情况。

watch，类似乐观锁，监控一个或者多个key，如果在事务执行之前key被其他命令改动，那么事务将被打断

unwatch，取消对所有key的监控



#### 3阶段

开启：MULTI开始一个事务

入队：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面

执行：由EXEC命令触发事务

 

#### 3特性

单独的隔离操作：事务中所有的命令都会序列化，按顺序执行，事务在执行的过程中，不会被其他客户端发送来的命令请求打断

没有隔离级别：队列中的命令没有提交之前都不会实际操作被执行，因为事务提交之前任何事务都不会被实际执行

不保证原子性：redis同一个事务中如果有一条命令执行失败，其他的命令仍然会被执行，没有回滚。



## RDB和AOF

### RDB是什么

在指定时间间隔内将内存中的数据集快照写入磁盘

### **fork的作用**

复制一个和当前进程一样的进程，作为原进程的子进程

### **快照**

默认情况下，Redis将数据集的快照保存在磁盘上名为的二进制文件中dump.rdb。如果数据集中至少有M个更改，可以将Redis配置为使其每N秒保存一次数据集，或者可以手动调用SAVE或BGSAVE命令。

AOF是RDB的弥补，增强RDB，相互使用

### AOF（Append Only File）是什么

以日志的方式来记录每个写操作，只许追加文件不可以改写文件。换言之redis重启的话就是根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

保存的是appendonly.aof文件 在配置文件中默认是关闭的

当AOF的文件大小超过所设定的阈值时，redis就会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令bgrewriteaof

### **AOF的重写原理**

当AOF文件持续增长过大时，会fork出一条新进程来将文件重写（也是先写临时文件最后rename）遍历新进程的内存中数据，每一条记录有一个set语句。重写AOF文件的操作，并没有读取旧的AOF文件，而是将整个内存中的数据库内容用命令的方式重写了一个新的AOF文件，与快照相似

### **触发机制**

redis会记录上次重写时候的aof文件大小，默认是上次rewrite后大小的1倍且文件大于64M触发

![image-20201224165001248](https://gitee.com/leidl97/picture/raw/master/img/20201224165001.png)

重写的时候是否可用appendfsync，默认为no，保证数据安全性

 

同时开启两种持久化方式，当redis重启后会优先加载AOF文件来恢复原始的数据，因为通常情况下AOF数据比RDB保存的数据要完整。

建议不要只使用AOF。因为RDB更适合于备份数据库（AOF在不断变化不好备份）

快速重启，而且不会有AOF潜在的BUG，留着作为一个万一的手段

 

### **性能建议**

RDB文件只用作后备用途，建议只在slave上持久化RDB文件，建议15分钟去备份一次，只保留save 9001这条规则

如果Enable AOF（就是开AOF），在恶劣的环境下也只会丢失两秒之内的数据。代价是带来了持续的IO，rewrite的最后将过程中产生的数据写到新文件造成阻塞几乎是不可避免的。只要硬盘许可，尽量减少AOF rewrite的频率，AOF默认重写基础大小为64M就可以了，可以设到5G以上。默认超过原大小的100%时，可以改到适当的数值。

如果不EnableAOF，仅靠Master-Slave Replication实现高可用也可以，可以省掉一大部分IO，减少了rewrite对系统带来的波动。但是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Salve中的RDB文件，载入较新的那个。新浪运用的就是这种架构。

 

在运行情况下， Redis 以数据结构的形式将数据维持在内存中， 为了让这些数据在 Redis 重启之后仍然可用， Redis 分别提供了 RDB 和 AOF 两种持久化模式。

在 Redis 运行时， RDB 程序将当前内存中的数据库快照保存到磁盘文件中， 在 Redis 重启动时， RDB 程序可以通过载入 RDB 文件来还原数据库的状态。

RDB 功能最核心的是 rdbSave 和 rdbLoad 两个函数， 前者用于生成 RDB 文件到磁盘， 而后者则用于将 RDB 文件中的数据重新载入到内存中：

![image-20201224165158173](https://gitee.com/leidl97/picture/raw/master/img/20201224165158.png)

以下均来源于官网翻译

### RDB的优势

- RDB是Redis数据的非常紧凑的单文件时间点表示。RDB文件非常适合备份。例如，您可能希望在最近的24小时内每小时存档一次RDB文件，并在30天之内每天保存一次RDB快照。这使您可以在发生灾难时轻松还原数据集的不同版本。
- RDB对灾难恢复非常有用，它是一个紧凑的文件，可以传输到远程数据中心或Amazon     S3（可能已加密）上。
- RDB最大限度地提高了Redis的性能，因为Redis父进程为了持久化而需要做的唯一工作就是分叉一个孩子，其余所有工作都要做。父实例将永远不会执行磁盘I     / O或类似操作。
- 与AOF相比，RDB允许大型数据集更快地重启。

### RDB的缺点

- 如果您需要在Redis停止工作（例如断电后）时最大程度地减少数据丢失的机会，那么RDB不好。您可以在生成RDB的位置配置不同的*保存点*（例如，在至少五分钟之后，对数据集进行100次写入，但是您可以有多个保存点）。但是，通常您会每隔五分钟或更长时间创建一个RDB快照，因此，如果Redis出于任何原因在没有正确关闭的情况下停止工作，则应该准备丢失最新的数据分钟。
- RDB需要经常使用fork（）才能使用子进程将其持久化在磁盘上。如果数据集很大，Fork（）可能很耗时，并且如果数据集很大且CPU性能不佳，则可能导致Redis停止为客户端服务几毫秒甚至一秒钟。AOF还需要fork（），但您可以调整要重写日志的频率，而无需在持久性上进行权衡。

### AOF的优势

- 使用AOF     Redis更加持久：您可以使用不同的fsync策略：完全没有fsync，每秒fsync，每个查询fsync。使用fsync的默认策略，每秒的写入性能仍然很好（使用后台线程执行fsync，并且在没有进行fsync的情况下，主线程将尽力执行写入操作。）但是您只能损失一秒钟的写入时间。
- AOF日志是仅追加的日志，因此，如果断电，则不会出现寻道或损坏问题。即使由于某种原因（磁盘已满或其他原因）以半写命令结束日志，redis-check-aof工具也可以轻松修复它。
- Redis太大时，Redis可以在后台自动重写AOF。重写是完全安全的，因为Redis继续追加到旧文件时，会生成一个全新的文件，其中包含创建当前数据集所需的最少操作集，一旦准备好第二个文件，Redis会切换这两个文件并开始追加到新的那一个。
- AOF以易于理解和解析的格式包含所有操作的日志。您甚至可以轻松导出AOF文件。例如，即使您使用[FLUSHALL](https://redis.io/commands/flushall)命令刷新了所有错误[文件](https://redis.io/commands/flushall)，如果在此期间未执行日志重写，您仍然可以保存数据集，只是停止服务器，删除最新命令并重新启动Redis。

### AOF的缺点

- 对于同一数据集，AOF文件通常大于等效的RDB文件。
- 根据确切的fsync策略，AOF可能比RDB慢。通常，在将fsync设置为*每秒的情况下，*性能仍然很高，并且在禁用fsync的情况下，即使在高负载下，它也应与RDB一样快。即使在巨大的写负载情况下，RDB仍然能够提供有关最大延迟的更多保证。
- 过去，我们在特定命令中遇到过罕见的错误（例如，其中一个涉及阻止命令，例如[BRPOPLPUSH](https://redis.io/commands/brpoplpush)），导致生成的AOF在重载时无法重现完全相同的数据集。这些错误很少见，我们在测试套件中进行了测试，自动创建了随机的复杂数据集，然后重新加载它们以检查一切是否正常。但是，RDB持久性几乎是不可能的。为了更清楚地说明这一点：Redis     AOF通过增量更新现有状态来工作，就像MySQL或MongoDB一样，而RDB快照一次又一次地创建所有内容，从概念上讲更健壮。但是-1）应当注意，每次Redis重写AOF时，都会从数据集中包含的实际数据开始从头开始重新创建AOF，与始终附加AOF文件（或重写为读取旧的AOF而不是读取内存中的数据）相比，提高了对错误的抵抗力。2）我们从未收到过有关真实环境中检测到的AOF损坏的用户报告。

## 主从复制

以下摘自官网

行话：主从复制以master为主，slave为从

![image-20201224165406214](https://gitee.com/leidl97/picture/raw/master/img/20201224165406.png)

### 能干嘛

主要是读写分离，容灾恢复

 

### 怎么用

1、配从不配主

2、从库配置：salveof主库IP，主库端口。

每次master断开之后，都需要重新连接，除非配置进redis.conf文件

3、修改配置文件细节操作

拷贝多个redis.conf文件

![image-20201224165602813](https://gitee.com/leidl97/picture/raw/master/img/20201224165602.png)

开启守护进程daemonzie yes

![image-20201224165535403](https://gitee.com/leidl97/picture/raw/master/img/20201224165535.png)

指定端口

![image-20201224165625241](https://gitee.com/leidl97/picture/raw/master/img/20201224165625.png)

pid文件名字

![image-20201224165644837](https://gitee.com/leidl97/picture/raw/master/img/20201224165644.png)

log文件名字

![image-20201224165654520](https://gitee.com/leidl97/picture/raw/master/img/20201224165654.png)

durm.rdb名字

![image-20201224165726301](https://gitee.com/leidl97/picture/raw/master/img/20201224165726.png)

4、常用三招：一主二从，薪火相传，反客为主

##### 一主二从

![image-20201224165801156](https://gitee.com/leidl97/picture/raw/master/img/20201224165801.png)

![image-20201224170036573](https://gitee.com/leidl97/picture/raw/master/img/20201224170036.png)

80 81跟随主机成为slave，虽然主机在之前创建值k1但是两个从机会接手主机以前的值。主机有的它都有

从机不可以写新的值

![image-20201224170133948](https://gitee.com/leidl97/picture/raw/master/img/20201224170134.png)

假如主机死了，从机怎么办

两个从机还是slave

假如主机重新连接，设一个值，从机还能接收到，主从关系不会乱

假如从机死了，从机在连接。就成为master了

![image-20201224170238042](https://gitee.com/leidl97/picture/raw/master/img/20201224170238.png)

命令：SLAVEOF 127.0.0.1 6379

![image-20201224170311468](https://gitee.com/leidl97/picture/raw/master/img/20201224170311.png)

##### 薪火相传

如果所有的从机都连接主机，中心化会非常严重。负担会非常大

上一个主机的Slave是下一个slave的master。salve同样可以接收其他的连接和同步请求，这样就会减轻master的负担

中途转向，会清除之前的数据，重新拷贝最新的。保证数据的一致性和有效性

不论再怎么传，主从中只有一个master

##### 反客为主

SLAVEOF no one

![image-20201224170440679](https://gitee.com/leidl97/picture/raw/master/img/20201224170440.png)

从机通过这个命令会变成主机，不过另外一个主机还在等老领导（79）可以使用命令SLAVEOF 127.0.0.1 6380

来跟随这个新领导（80）.如果老领导（79）回来那么还是领导（master），但是为光杆司令



**复制原理**

slave连接到master会发送一个sync命令，master进到命令会会进行数据同步

全量复制，全部复制过来。

增量复制，master新增内容，salve会完成同步

## 哨兵模式

反客为主的自动版

![image-20201224170933432](https://gitee.com/leidl97/picture/raw/master/img/20201224170933.png)

sentinel.conf怎么写

`sentinel monitor host6379 127.0.0.1 6379 1`

加粗是自己起名字，随意写。最后一个数字1表示主机挂掉之后slave投票看让谁接替成为主机，得票数多少成为主机

启动哨兵

`redis-sentinel sentinel.conf`

如果主机挂了，会在两个从机中选出一个作为master，另一个从机去跟随。如果老领导回来那么也得跟随新选出的master

 

**复制延时**

由于所有的写操作都在Master上操作，然后更新到slave上，所以master同步到slave机器上有一定的延迟，当系统繁忙的时候，延迟问题会更加严重，slave机器数量增加也会使这个问题更加严重。



# jedis

jedis网址

https://mvnrepository.com/artifact/redis.clients/jedis

 

pom.xml文件需要进行添加jar包

```xml
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>3.0.1</version>
</dependency>
```

使用Java程序连接，需要注意将redis.conf文件下的bind修改![image-20201224171210905](https://gitee.com/leidl97/picture/raw/master/img/20201224171210.png)

否则会报如下错误

![image-20201224171756257](https://gitee.com/leidl97/picture/raw/master/img/20201224171756.png)

成功截图如下

![image-20201224171832977](https://gitee.com/leidl97/picture/raw/master/img/20201224171833.png)

API操作（）需要设置redis.conf的的bind

![image-20201224171913421](https://gitee.com/leidl97/picture/raw/master/img/20201224171913.png)

![image-20201224171952086](https://gitee.com/leidl97/picture/raw/master/img/20201224171952.png)

![image-20201224172026865](https://gitee.com/leidl97/picture/raw/master/img/20201224172026.png)

## 事务

![image-20201224172106836](https://gitee.com/leidl97/picture/raw/master/img/20201224172106.png)

![image-20201224172329271](https://gitee.com/leidl97/picture/raw/master/img/20201224172329.png)

本来有100，在刷时候成了5块钱，这次刷十块，放弃监控，返回false

## 主从复制

![image-20201224172417189](https://gitee.com/leidl97/picture/raw/master/img/20201224172417.png)

有可能会出现null，不是报错，是因为内存比磁盘要快，在执行一遍就可以了



## jedispool

需要多个jedis连接，就需要一个连接池了

![image-20201224172509472](https://gitee.com/leidl97/picture/raw/master/img/20201224172509.png)



# 安装和相关命令

下载解压(5.08)下载最新版需要gcc-c++最新版适配

 

安装gcc-c++

yum -y install gcc-c++

 

make&&make install

![image-20201224172647748](https://gitee.com/leidl97/picture/raw/master/img/20201224172647.png)

安装默认路径 /usr/local/bin

将配置文件拷贝到默认路径下

 cp /opt/redis-5.0.8/redis.conf myconf/

 

redis默认不是后台启动的，修改配置文件

![image-20201224172702864](https://gitee.com/leidl97/picture/raw/master/img/20201224172702.png)

redis-server myconf/redis.conf 

![image-20201224172710075](https://gitee.com/leidl97/picture/raw/master/img/20201224172710.png)

在开启一个命令进行测试

![image-20201224172724318](https://gitee.com/leidl97/picture/raw/master/img/20201224172724.png)

shutdown关闭

测试性能

redis-benchmark是一个压测工具（官方自带）

 

简单测试

测试100个并发连接 10w个请求

redis-benchmark -h localhost -p 6379 -c 100 -n 100000

![image-20201224172739884](https://gitee.com/leidl97/picture/raw/master/img/20201224172739.png)

命令

默认有16个数据库（配置文件中有）)

![image-20201224172753518](https://gitee.com/leidl97/picture/raw/master/img/20201224172753.png)

清空数据库（不要随便用）

flushdb/flushall 

![image-20201224172801570](https://gitee.com/leidl97/picture/raw/master/img/20201224172801.png)

DBSIZE（说明有两个Key）

![image-20201224172809260](https://gitee.com/leidl97/picture/raw/master/img/20201224172809.png)

**key命令**

keys *

 

exists key的名字，判断某个key是否存在

move key db 当前库就没有了

 

expire key 秒 为你的key设置过期时间，过期之后生命周期终结，不是访问不到（可以理解为定期删除）

![image-20201224172823563](https://gitee.com/leidl97/picture/raw/master/img/20201224172823.png)

ttl key的名字，-1表示永不过期

![image-20201224172829536](https://gitee.com/leidl97/picture/raw/master/img/20201224172829.png)

type key看你的key的类型

![image-20201224172835309](https://gitee.com/leidl97/picture/raw/master/img/20201224172835.png)

**string** **的常用命令**

set/get/del/append /strlen

![image-20201224172841754](https://gitee.com/leidl97/picture/raw/master/img/20201224172841.png)

incr/decr/incrby/decrby 数字才能加减

![image-20201224172906014](https://gitee.com/leidl97/picture/raw/master/img/20201224172906.png)

getrange/setrange  获取指定范围内的值

 

List命令

单值多value

lpush/rpush/lrange

左边进，正着进，反得出

右边进，正着进，正着出

![image-20201224173047054](https://gitee.com/leidl97/picture/raw/master/img/20201224173047.png)

hash命令操作

hset/hget/hmset/hmget/hgetall/hdel

![image-20201224173106161](https://gitee.com/leidl97/picture/raw/master/img/20201224173106.png)

hkeys key/hval key

![image-20201224173121618](https://gitee.com/leidl97/picture/raw/master/img/20201224173121.png)
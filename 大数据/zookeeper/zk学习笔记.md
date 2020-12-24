## 本文大纲

- 什么是ZK
- ZK能够干什么
- zookeeper的数据结构
- ZK的选举

## **什么是zookeeper？**

1：Zookeeper是Apache Hadoop的子组件之一

2：ZooKeeper主要服务于**分布式系统**。统一配置管理、统一命名服务、分布式锁、集群管理



## **zookeeper能够干什么**

ZooKeeper 是一个典型的**分布式数据一致性解决方案**，分布式应用程序可以基于 ZooKeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、分布式锁和分布式队列等功能。



## **zookeeper的数据结构**

可以看做是一颗**树**，每个节点叫做**ZNode**。每一个节点可以通过路径来标识，结构图如下：

![img](https://pic3.zhimg.com/80/v2-abcfb2ede0d822536320d362ac742042_720w.jpg)

ZNode分为两种类型

**短暂/临时**(Ephemeral)：当客户端和服务端断开连接后，所创建的Znode(节点)**会自动删除**

**持久**(Persistent)：当客户端和服务端断开连接后，所创建的Znode(节点)**不会删除**



## **zookeeper如何做到统一配置管理、统一命名服务、分布式锁、集群管理**

**1：统一配置管理**

假设我们现在有三个系统A、B、C，他们有三份配置，分别是ASystem.yml、BSystem.yml、CSystem.yml，这三份配置非常类似，很多的配置项几乎都一样。此时，如果我们要改变其中一份配置项的信息，很可能其他两份都要改。并且，改变了配置项的信息很可能就要重启系统。于是，我们希望把ASystem.yml、BSystem.yml、CSystem.yml相同的配置项抽取出来成一份公用的配置common.yml，并且即便common.yml改了，也不需要系统A、B、C重启。

![img](https://pic2.zhimg.com/80/v2-0712e692d69effbca9f1190e2f9a68d5_720w.jpg)

**做法**：我们可以将common.yml这份配置放在ZooKeeper的Znode节点中，系统A、B、C监听着这个Znode节点有无变更，如果变更了，**及时**响应

**2：统一命名服务**

比如说，现在我有一个域名[http://www.xxx.com](https://link.zhihu.com/?target=http%3A//www.xxx.com)，但我这个域名下有多台机器：

192.168.1.1

192.168.1.2

192.168.1.3

192.168.1.4

别人访问[http://www.xxx.com](https://link.zhihu.com/?target=http%3A//www.xxx.com)即可访问到我的机器，而不是通过IP去访问

![img](https://pic1.zhimg.com/80/v2-90b7b68bd92bb8f3d80a2cf7fab4c0dc_720w.jpg)

**3:分布式锁**

系统A、B、C都去访问/locks节点

![img](https://pic1.zhimg.com/80/v2-530d2d4585e0ac7a5495e90152fd5af0_720w.jpg)

访问的时候会创建带顺序号的临时/短暂(EPHEMERAL_SEQUENTIAL)节点，比如，系统A创建了id_000000节点，系统B创建了id_000002节点，系统C创建了id_000001节点。

![img](https://pic3.zhimg.com/80/v2-1722fc336822c4b636154b9d02ea9ad6_720w.jpg)

接着，拿到/locks节点下的所有子节点(id_000000,id_000001,id_000002)，判断自己创建的是不是最小的那个节点

如果是，则拿到锁。

释放锁：执行完操作后，把创建的节点给删掉

如果不是，则监听比自己要小1的节点变化

原理：

系统A拿到/locks节点下的所有子节点，经过比较，发现自己(id_000000)，是所有子节点最小的。所以得到锁

系统B拿到/locks节点下的所有子节点，经过比较，发现自己(id_000002)，不是所有子节点最小的。所以监听比自己小1的节点id_000001的状态

系统C拿到/locks节点下的所有子节点，经过比较，发现自己(id_000001)，不是所有子节点最小的。所以监听比自己小1的节点id_000000的状态…...等到系统A执行完操作以后，将自己创建的节点删除(id_000000)。通过监听，系统C发现id_000000节点已经删除了，发现自己已经是最小的节点了，于是顺利拿到锁….

系统B如上

**4：集群管理**

以我们三个系统A、B、C为例，在ZooKeeper中创建临时节点即可

![img](https://pic4.zhimg.com/80/v2-63426e4ffc44fe0d7eb2a7957a52903f_720w.jpg)

只要系统A挂了，那/groupMember/A这个节点就会删除，通过监听groupMember下的子节点，系统B和C就能够感知到系统A已经挂了。(新增也是同理)

除了能够感知节点的上下线变化，ZooKeeper还可以实现动态选举Master的功能。(如果集群是主从架构模式下)

原理也很简单，如果想要实现动态选举Master的功能，Znode节点的类型是带顺序号的临时节点(EPHEMERAL_SEQUENTIAL)就好了。

Zookeeper会每次选举最小编号的作为Master，如果Master挂了，自然对应的Znode节点就会删除。然后让新的最小编号作为Master，这样就可以实现动态选举的功能了。

## ZK选举

**1：集群至少由三个节点组成**

虽然说两个也可以，如果其中一个节点挂了，zookeeper就不能提供对外服务，失去了集群的意义。

**2：节点并非越多越好**

节点越多，使用的资源越多。

节点越多，ZooKeeper节点间花费的通讯成本越高，节点间互连的Socket也越多。影响ZooKeeper集群事务处理。

节点越多，造成脑裂的可能性越大。

**3：集群规模为奇数**

3台和4台都是只可以挂一台，因为过半性

解释3的一半为1.5以上最小整数为2，所以只能挂一台；4的一半为2以上最小整数为3，也是只能挂一台。

![img](https://pic2.zhimg.com/80/v2-bebf89e08897ec669a2d93e377de1b4d_720w.jpg)

**4：集群角色**

**Leader**

在一个zookeeper集群中**只能存在一个leader**，集群中事务请求的唯一调度者和处理者，leader根据事务ID保证事务处理的顺序性

如果一个集群中存在多个leader，这个现象称为脑裂，大集群裂变为小集群，会导致数据不同步，数据混乱。

**Follower**

只能处理非事务请求，如果收到事务请求转发给leader；参与leader选举；参与事务处理投票

**observer**

与follower相似，可以处理非事务请求；将事务请求转发给Leader服务器。但是它不会参与leader的选举，不会参与投票。

**如何成为观察者**：在zoo.cfg中添加如下属性：peerType=observer

**作用**：用于不影响集群事务处理能力的前提下提升集群的非事务处理能力。



**5：leader选举机制**

**1、选举投票必须在同一轮次中进行**

如果Follower服务选举轮次不同，不会采纳投票。

**2、数据最新的节点优先成为Leader**

数据的新旧使用事务ID判定，事务ID越大认为节点数据约接近Leader的数据，自然应该成为Leader。

**3、比较myid，id值大的优先成为Leader**

如果每个参与竞选节点事务ID一样，再使用myid做比较。myid是节点在集群中唯一的id，myid文件中配置。

**6：过半原则**

**Leader选举投票；事务提议投票；这些投票依赖过半原则。**

- **事务提议投票**
- 假设有3个节点组成ZooKeeper集群，客户端请求添加一个节点。Leader接到该事务请求后给所有Follower发起「创建节点」的提议投票。如果Leader收到了超过集群一半数量的反馈，继续给所有Follower发起commit。此时Leader认为集群过半了，就算自己挂了集群也是安全可靠的。
- **Leader选举投票**
- 假设有3个节点组成ZooKeeper集群，这时Leader挂了，需要投票选举Leader。当相同投票结果过半后Leader选出。
- **集群可用节点**
- ZooKeeper集群中每个节点有自己的角色，对于集群可用性来说必须满足过半原则。这个过半是指Leader角色 + Follower角色可用数大于集群中Leader角色 + Follower角色总数。
- 假设有5个节点组成ZooKeeper集群，一个Leader、两个Follower、两个Observer。**当挂掉两个Follower或挂掉一个Leader和一个Follower时集群将不可用。因为Observer角色不参与任何形式的投票。**

**7：场景实战**

**1：leader挂了选举过程**

假设有3节点组成的集群，分别是server.1（Follower）、server.2（Leader）、server.3（Follower）。此时server.2不可用了。集群会产生以下变化：

**状态变更**

所有Follower节点变更自身状态为LOOKING。投票内容就是自己节点的事务ID和myid。我们以(事务ID,myid)表示。

假设server.1的事务id是10，变更的自身投票就是（10, 1）；server.3的事务id是8，变更的自身投票就是（8, 3）。

**投票机制**

将变更的投票发给集群中所有的Follower节点。

server.1将（10, 1）发给集群中所有Follower，包括它自己。server.3也一样，将（8, 3）发给所有Follower。

所以server.1将收到（10, 1）和（8, 3）两个投票，server.3将收到（8, 3）和（10, 1）两个投票。

投票PK

比较规则：先比较事务id（Zxid），如果事务id相同则比较myid；超过一半节点才能成为leader。

对于server.1来说收到（10, 1）和（8, 3）两个投票，与自己变更的投票比较后没有一个比自身投票（10, 1）要大的，所以server.1维持自身投票不变。

对于server.3来说收到（10, 1）和（8, 3）两个投票，与自身变更的投票比较后认为server.1发来的投票要比自身的投票大，所以server.3会变更自身投票并将变更后的投票发给集群中所有Follower。

**统计投票**

server.1接收到来自server.1的（10, 1）投票和来自server.3的（8, 3）投票。

server.3同样接收到来自server.1的（10, 1）投票和来自server.3的（8, 3）投票。

此时server.1和server.3接收桶里的数据是这样的：

![img](https://pic1.zhimg.com/80/v2-ff0d8240e357ddec7a35bfd546636eb0_720w.jpg)

server.3经过PK后认为server.1的选票比自己要大，所以变更了自己的投票并重新发起投票。

server.1收到了来自server.3的（10, 1）投票;server.3收到了来自sever.3的（10, 1）投票。

此时server.1和server.3接收桶里的数据变成了这样：

![img](https://pic3.zhimg.com/80/v2-576d96d5c8541d03f5099b624369da62_720w.jpg)

基于ZooKeeper过半原则：桶内投票选举server.1作为Leader**出现2次**，满足了过半 2 > 3/2 即 2>1。

最后sever.1节点晋升为Leader，server.3变更为Follower。

**接收桶**

节点接收的投票存储在一个接收桶里，每个Follower的投票结果在桶内**只记录一次**。ZooKeeper源码中接收桶**用Map实现**。

下面代码片段是ZooKeeper定义的接收桶，以及向桶内写入数据。Map.Key是Long类型，用来存储投票来源节点的myid，Vote则是对应节点的投票信息。节点收到投票后会更新这个接收桶，也就是说桶里存储了所有Follower节点的投票并且**仅存最后一次的投票结果**。

```text
HashMap<Long, Vote> recvset = new HashMap<Long, Vote>();
recvset.put(n.sid, new Vote(n.leader, n.zxid, n.electionEpoch, n.peerEpoch));
```

**2：集群扩容选举**

假设目前有3个节点组成集群，分别是server.1（Follower）、server.2（Leader）、server.3（Follower），假设集群中节点事务ID相同。集群中新增server.4和server.5两个节点，首先修改server.4和server.5的zoo.cfg配置并启动。

**选举机制**

![img](https://pic3.zhimg.com/80/v2-b9c919261be7dc81c1b6f71303410f16_720w.png)

**脑裂现象的出现**

修改server.2zoo.cfg配置文件，增加server.4和server.5的配置并停止server.2服务。停止server.2后，Leader不存在了，集群中所有Follower会发起投票。当server.1和server.3发起投票时并不会将投票发给server.4和server.5，因为在server.1和server.3的集群配置中不包含server.4和server.5节点。相反，server.4和server.5会把选票发给集群中所有节点。也就是说对于server.1和server.3他们认为集群中只有3个节点。对于server.4和server.5他们认为集群中有5个节点。

根据过半原则，server.1和server.3很快会选出一个新Leader，我们这里假设server.3晋级成为了新Leader。但是我们没有启动server.2的情况下，因为投票不满足过半原则，server.4和server.5会一直做投票选举Leader的动作。截止到现在集群中节点状态是这样的：

![img](https://pic1.zhimg.com/80/v2-bd9553a1d174247ba5ae4f934e14f58c_720w.jpg)

**在启动leader后意想不到的事情发生了，出现两个Leader：**

![img](https://pic3.zhimg.com/80/v2-cf885df59ee544890b022f44d891cf2a_720w.jpg)

**如何避免**

ZooKeeper集群扩容时，如果**Leader节点最后启动**就可以避免这类问题发生。

因为在Leader节点重启前，所有的Follower节点zoo.cfg配置已经是相同的，他们基于同一个集群配置两两互联，做投票选举。
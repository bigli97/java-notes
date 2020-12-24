**安装前准备**

1. 关闭防火墙
2. 修改主机名
3. 配置/etc/hosts文件
4. 配置免密登录
5. 安装JDK
6. 安装zookeeper（集群式安装）
7. 解压Hadoop
8. selinux关闭掉，这是linux系统的一个安全机制，进入文件中将SELINUX设置为disabled

**安装步骤**

1：进入hadoop安装子目录etc/hadoop/下

2：修改hadoop-env.sh文件。source hadoop-env.sh使其生效

```text
export JAVA_HOME=/usr/local/jdk1.8
export HADOOP_CONF_DIR=/home/ldl/software/hadoop-2.7.1/etc/hadoop
```

![img](https://pic1.zhimg.com/80/v2-6c91ba61214f00e35efe22f530be3b68_720w.jpg)

**3：配置core-site.xml；hdfs-site.xml；mapred-site.xml；yarn-site.xml四个xml文件**

core-site.xml

```text
<!--指定hdfs的nameservice，，为整个集群起一个-->
 <property>
     <name>fs.defaultFS</name>
     <value>hdfs://ns</value>
 </property>
  <!--指定Hadoop数据临时存放目录-->
 <property>
     <name>hadoop.tmp.dir</name>
     <value>/home/ldl/software/hadoop-2.7.1/tmp</value>
 </property>
  <!--指定zookeeper的存放地址-->
 <property>
     <name>ha.zookeeper.quorum</name>
     <value>hadoop01:2181,hadoop02:2181,hadoop03:2181</value>
 </property>
```

**hdfs-site.xml**

```text
<!--执行hdfs的nameservice为ns，注意要和core-site.xml中的名称保持一致-->
 <property>
     <name>dfs.nameservices</name>
     <value>ns</value>
 </property>
  <!--ns集群下有两个namenode，分别为nn1, nn2-->
 <property>
     <name>dfs.ha.namenodes.ns</name>
     <value>nn1,nn2</value>
 </property>
  <!--nn1的RPC通信-->
 <property>
     <name>dfs.namenode.rpc-address.ns.nn1</name>
     <value>hadoop01:9000</value>
 </property>
 <!--nn1的http通信-->
 <property>
     <name>dfs.namenode.http-address.ns.nn1</name>
     <value>hadoop01:50070</value>
 </property>
  <!-- nn2的RPC通信地址 -->
 <property>
     <name>dfs.namenode.rpc-address.ns.nn2</name>
     <value>hadoop02:9000</value>
 </property>
  <!-- nn2的http通信地址 -->
 <property>
     <name>dfs.namenode.http-address.ns.nn2</name>
     <value>hadoop02:50070</value>
 </property>
 <!--指定namenode的元数据在JournalNode上存放的位置，这样，namenode2可以从journalnode集群里的指定位置上获取信息，达到热备效果-->
 <property>
     <name>dfs.namenode.shared.edits.dir</name>
     <value>qjournal://hadoop01:8485;hadoop02:8485;hadoop03:8485/ns</value>
 </property>
 <!-- 指定JournalNode在本地磁盘存放数据的位置 -->
 <property>
     <name>dfs.journalnode.edits.dir</name>
     <value>/home/ldl/software/hadoop-2.7.1/tmp/journal</value>
 </property>
 <!-- 开启NameNode故障时自动切换 -->
 <property>
     <name>dfs.ha.automatic-failover.enabled</name>
     <value>true</value>
 </property>
 <!-- 配置失败自动切换实现方式 -->
 <property>
     <name>dfs.client.failover.proxy.provider.ns</name>
 <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
 </property>
 <!-- 配置隔离机制 -->
 <property>
     <name>dfs.ha.fencing.methods</name>
     <value>sshfence</value>
 </property>
  <!-- 使用隔离机制时需要ssh免登陆 -->
 <property>
     <name>dfs.ha.fencing.ssh.private-key-files</name>
     <value>/root/.ssh/id_rsa</value>
 </property>
 <!--配置namenode存放元数据的目录，可以不配置，如果不配置则默认放到hadoop.tmp.dir下-->
 <property>
     <name>dfs.namenode.name.dir</name>
     <value>file:///home/ldl/software/hadoop-2.7.1/tmp/hdfs/name</value>
 </property>
  <!--配置datanode存放元数据的目录，可以不配置，如果不配置则默认放到hadoop.tmp.dir下-->
 <property>
     <name>dfs.datanode.data.dir</name>
     <value>file:///home/ldl/software/hadoop-2.7.1/tmp/hdfs/data</value>
 </property>
 <!--配置复本数量-->
 <property>
     <name>dfs.replication</name>
     <value>3</value>
 </property>
 <!--设置用户的操作权限，false表示关闭权限验证，任何用户都可以操作-->
 <property>
     <name>dfs.permissions</name>
     <value>false</value>
 </property>
```

**mapred-site.xml**

```text
<property>
     <name>mapreduce.framework.name</name>
     <value>yarn</value>
 </property>
```

**yarn-site.xml**

```text
<!--配置yarn的高可用-->
 <property>
     <name>yarn.resourcemanager.ha.enabled</name>
     <value>true</value>
 </property>
  <!--指定两个resourcemaneger的名称-->
 <property>
     <name>yarn.resourcemanager.ha.rm-ids</name>
     <value>rm1,rm2</value>
 </property>
  <!--配置rm1的主机-->
 <property>
     <name>yarn.resourcemanager.hostname.rm1</name>
     <value>hadoop01</value>
 </property>
 <!--配置rm2的主机-->
 <property>
     <name>yarn.resourcemanager.hostname.rm2</name>
     <value>hadoop02</value>
 </property>
  <!--开启yarn恢复机制-->
 <property>
     <name>yarn.resourcemanager.recovery.enabled</name>
     <value>true</value>
 </property>
  <!--执行rm恢复机制实现类-->
 <property>
     <name>yarn.resourcemanager.store.class</name>
     <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
 </property>
 <!--配置zookeeper的地址-->
 <property>
     <name>yarn.resourcemanager.zk-address</name>
     <value>hadoop01:2181,hadoop02:2181,hadoop03:2181</value>
 </property>
  <!--执行yarn集群的别名-->
 <property>
     <name>yarn.resourcemanager.cluster-id</name>
     <value>ns-yarn</value>
 </property>
 <!-- 指定nodemanager启动时加载server的方式为shuffle server -->
 <property>
     <name>yarn.nodemanager.aux-services</name>
     <value>mapreduce_shuffle</value>
 </property>
 <!-- 指定resourcemanager地址 -->
 <property>
     <name>yarn.resourcemanager.hostname</name>
     <value>hadoop01</value>
 </property>
```

**7：编辑slaves(可以决定哪些服务器是数据节点也就是DataNode)**

```text
hadoop01
hadoop02
hadoop03
```

**8：拷贝到其他节点**

```text
scp -r /home/ldl/software/hadoop-2.7.1/ hadoop02:/home/ldl/software/
scp -r /home/ldl/software/hadoop-2.7.1/ hadoop03:/home/ldl/software/
```

**9：配置环境变量**

```text
cd /etc/profile
source /etc/profile
```

**10：三台虚拟机启动zookeeper**

输入./zkServer.sh status查询状态显示“follower”或者“leader”正常

![img](https://pic1.zhimg.com/80/v2-097aae00b3d97917c0e14089f2a9e8fc_720w.png)

**11：三台虚拟机启动JournalNode**：

```text
hadoop-daemon.sh start journalnode
```

**JournalNode的作用**：使三台虚拟进行相互通信，如果没有该进程那么将不能进行namenode格式化

**12：在第一台节点上（主机上）格式化NameNode**：hdfs namenode -format/hadoop namenode -format

**说明**

> hadoop namenode -format跟hdfs namenode -format是一个东西，**作用完全相同**。
> 在hadoop1.x的时候，只有hadoop namenode -format一种方式，到了hadoop2.x之后，引入了新的hdfs namenode -format，作用完全相同，为了兼容老版本，原来的命令仍然可以使用。

**13：启动HDFS**

```text
start-dfs.sh/start-all.sh
```

**说明：**start-all.sh包括start-dfs.sh和start-yarn.sh

![img](https://pic4.zhimg.com/80/v2-d5a8206ced3fe1501061cb85aed6fa4f_720w.jpg)

启动之后如果要**设置主备**，格式化备用机的NameNode：hdfs namenode -bootstrapStandby

然后在启动备用机的NameNode：hadoop-daemon.sh start namenode（基于前一个命令）

启动RM的命令： yarn-daemon.sh start resourcemanager

**14、测试是否成功**

访问路径：[http://ip:50070/](https://link.zhihu.com/?target=http%3A//ip%3A50070/)

出现页面：主机为alive状态，其他两台为standby状态则为成功

![img](https://pic4.zhimg.com/80/v2-228fc7d25d12a96d419ea5152c9b951f_720w.jpg)

**可能出现的问题**

![img](https://pic3.zhimg.com/80/v2-791d15f9a94ec70fa4a8ff3ce05e0d86_720w.jpg)

> 原因：
> 从日志上看是因为 datanode的clusterID 和 namenode的clusterID 不匹配
>
> 根本原因：
> 其实，是因为第一次格式化namenode后，启动了hadoop集群，生成了clusterID=CID-96ed3103-6cea-49ef-9c3a-b2c056de269c（这是第一次格式化后生成的clusterID）。后来又因某种原因，重新格式化了namenode，又重新生成了另一个新的clusterID=CID-2b6d3a1f-659d-4029-887d-1a20b9a2ecc4。导致，后来重新启动hadoop集群的时候，其他节点还是用第一次生成的clusterID，导致，找不到新的集群的clusterID，报了一个找不到指定目录的异常。
> 3、根据配置的路径，找到其他的datanode节点下的VERSION文件
> 例如，我的VERSION文件在以下的目录中：
> /opt/module/hadoop-2.7.2/data/tmp/dfs/data/current 下
> 然后将其他datanode节点中的clusterID都修改为最后一次格式化后生成的clusterID

![img](https://pic2.zhimg.com/80/v2-b3555fb71cbed16b086a3d62052d9841_720w.jpg)

> 解决方案：检查网络状态，重启hadoop
> 实在解决不掉的问题就重新格式化namenode 。如果想重新格式化，那么需要先删除每台机器上的${hadoop.tmp.dir}指定路径下的所有内容，然后再格式化：最好也把logs目录下的内容也清空，因为日志内容已经是前一个废弃集群的日志信息了，留着也无用。
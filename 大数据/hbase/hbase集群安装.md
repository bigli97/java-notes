**安装前准备：zookeeper+JDK+Hadoop**

**安装步骤**

**1：上传并解压**

**2：修改conf/hbase.env.sh**

增加JAVA_HOME：export JAVA_HOME=/usr/local/jdk1.8

增加Zookeeper和Hbase的协调模式，hbase默认使用自带的zookeeper，如果需要使用外部zookeeper，需要先关闭：export HBASE_MANAGES_ZK=false

```text
export JAVA_HOME=/usr/local/jdk1.8
export HBASE_MANAGES_ZK=false
```

**3：配置hbase-site.xml，配置开启完全分布式模式**

```text
<property>
<name>hbase.rootdir</name>
<value>hdfs://ns/hbase</value>
</property>
<property>
<name>hbase.cluster.distributed</name>
<value>true</value>
</property>
<property>
<name>hbase.zookeeper.quorum</name>
<value>hadoop01:2181,hadoop02:2181,hadoop03:2181</value>
</property>
```

![img](https://pic1.zhimg.com/80/v2-655c7036090d58f7503ffa65ad373628_720w.jpg)

> hbase.rootdir: 这个参数是用来设置RegionServer 的共享目录，用来存放HBase数据。特别需要注意的是里面的 HDFS 地址是要跟 Hadoop 的 core-site.xml 里面的 fs.defaultFS 的 HDFS 的 IP 地址或者域名、端口**必须一致。**

4：**配置region服务器**

修改conf/regionservers文件,每个主机名独占一行，hbase启动或关闭时会按照该配置顺序启动或关闭主机中的hbase:

```text
hadoop01
hadoop02
hadoop03
```

![img](https://pic1.zhimg.com/80/v2-fa462c8aa0fbaa6cbcf1d7a41c3e00b0_720w.png)

**5：远程复制拷贝到其他节点上**

**6：启动Zookeeper服务，进入bin**

```text
 ./zkServer.sh start
```

**7：启动Hadoop**

```text
start-all.sh
```

**8：启动Hbase（别用start-hbase启动）**

```text
hbase-daemon.sh start thrift
hbase-daemon.sh start master
hbase-daemon.sh start regionserver
```

**9：查看各节点的java进程是否正确，或者通过浏览器访问[http://xxxxx:60010](https://link.zhihu.com/?target=http%3A//xxxxx%3A60010)来访问web界面，通过web界面管理hbase**

**需要注意的点**

**1：集群时间同步**

查看本机是否安装了ntpdate服务，安装时间同步软件

```text
yum install -y ntpdate
yum install -y ntp
```

同步时间

```text
ntpdate time.nist.gov
```

**2：启动要依次启动不能使用start.hbase.sh**

附加Hbase启停命令

```text
hbase-daemon.sh start thrift
hbase-daemon.sh start master
hbase-daemon.sh start regionserver
hbase-daemon.sh stop thrift
hbase-daemon.sh stop master
hbase-daemon.sh stop regionserver
```
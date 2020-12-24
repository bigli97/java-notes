**安装前准备**：**关闭防火墙，配置ssh免密登录**

[zookeeper官网](Apache ZooKeeperzookeeper.apache.org)

可以通过官网下载，自己有也可以。我用的是3.4.7

**1:上传zookeeper安装包**

**2:解压zookeeper安装包**

```text
 tar -xvf zookeeper-3.4.7.tar.gz 
```

**3:建议改名**

**4:进入conf目录**

```text
cd zookeeper-3.4.7/conf/
```

**5:将zoo_sample.cfg复制一份zoo.cfg**

因为Zookeeper在启动的时候会**自动寻找zoo.cfg**，根据其中的配置来启动存储数据

```text
cp zoo_sample.cfg zoo.cfg
```

**6:更改zoo.cfg 的配置**

```text
vi zoo.cfg 
```

在文件末端按如下配置

```text
dataDir=/home/ldl/software/zookeeper-3.4.7/data
dataLogDir=/home/ldl/software/zookeeper-3.4.7/log
server.1=192.168.11.131:2888:3888
server.2=192.168.11.132:2888:3888
server.3=192.168.11.133:2888:3888
```

最后关闭保存:wq!

![img](https://pic4.zhimg.com/80/v2-ed01cb28f35e182560f56439cbcd438f_720w.jpg)

1:编号要求是**数字**并且**不能重复**

2:原子广播端口号和选举端口号只要**不和当前已经使用的端口号冲突**即可

**7:创建目录data，log**

```text
mkdir -p data
mkdir -p log
```

**8:创建myid文件**

```text
 vi /home/ldl/software/zookeeper-3.4.7/data/myid 
```

**9:将zookeeper-3.4.7传到其余两台虚拟机中**

```text
 scp -r /home/ldl/software/zookeeper-3.4.7 hadoop02:/home/ldl/software/
 scp -r /home/ldl/software/zookeeper-3.4.7 hadoop03:/home/ldl/software/
```

**10:更改其余两台虚拟机的myid**

```text
 vi /home/ldl/software/zookeeper-3.4.7/data/myid 
```

**11:进入目录，启动服务**

```text
cd /home/ldl/software/zookeeper-3.4.7/bin/
./zkServer.sh start
./zkServer.sh start-foreground 附带信息（可以看报错信息）
```



可能出现的问题及解决方案

![img](https://pic1.zhimg.com/80/v2-0654447011aed72fcbca8d21d1918924_720w.png)

解决方案：找到myid所在的目录，删除version-2文件夹，在重新启动zookeeper
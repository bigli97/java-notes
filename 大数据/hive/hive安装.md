**1：启动zookeeper**

```text
zkServer.sh start
```

**2：启动Hadoop（包括hdfs和yarn）**

```text
start-all.sh
```

**3：解压hive安装包**

**4：改名（非必须）**

**5：配置环境变量**

```text
vim /etc/profile 
在文件末端加
export HIVE_HOME=/home/ldl/software/hive-1.2.1
export PATH=$PATH:$HIVE_HOME/bin
```

![img](https://pic3.zhimg.com/80/v2-e3eecfda3a3a838a85844e38be472c3e_720w.png)

**6：启动hive**

hive

![img](https://pic4.zhimg.com/80/v2-b39c2f36ee5620f096fd8f1c0d9f29f3_720w.png)
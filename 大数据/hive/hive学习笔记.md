

## 大纲

- 基本知识
- 基本指令
- 元数据
- 数据类型
- 架构
- 数据组织
- 内部表与外部表

## 基本知识

**为什么不用Hadoop进行开发**

1. 存在语言门槛，开发需要会Java语言
2. 需要了解底层原理
3. 开发调试不方便

**为什么使用Hive**

> hive终究还是mapreduce的框架，直接学习mapreduce所面临的问题
> 1.人员学习成本太高
> 2.项目周期要求短
> 3.MR实现复杂查询开发难度较大
> Hive的优势
> 1.接口更友好，使用HQL。就是类SQL语言法，学过MySQL就能进行开发
> 2.学习成本低，避免写MapReduce
> 3.扩展性好，自由扩展集群规模，支持用户自定义函数

**hive概述**

1. 是一个基于Hadoop的数据仓库工具。
2. 底层将sql语句转换为mapreduce来进行
3. **离线分析工具**，延迟比较高，不适合实时查询，比如进行日志分析

![img](https://pic4.zhimg.com/80/v2-e775fad1ff0b968494a8270ea71097a3_720w.jpg)

**OLTP和OLAP**

怎么突然蹦出来个这，这两个是什么

OLTP（Online Transaction Processing）联机事务处理系统。涵盖了日常操作，如购物，库存，制造，工资，注册，记账。处理系统就代表mysql，oracle等关系型数据库。

OLAP（Online Analytical Processing）联机分析处理系统。比如Hive，Hbase等

![img](https://pic2.zhimg.com/80/v2-acc2c9fa96785e4aab3cbfb0a159533d_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-2823e1b64ab2babc36128aee1f2eaa95_720w.jpg)

![img](https://pic1.zhimg.com/80/v2-d96b280c350151000cbe88767a70821c_720w.jpg)



## 基本指令

1：展示所有数据库

![img](https://pic4.zhimg.com/80/v2-568dc6986c3765bb43daab20a18308d7_720w.jpg)show databases；

2：创建park数据库

![img](https://pic2.zhimg.com/80/v2-1b0b699f5402f85ea775ef7b87d88629_720w.jpg)create database park；

3：进入park数据库

![img](https://pic2.zhimg.com/80/v2-dca68a37fa89a885b525f3d889e69595_720w.jpg)

4：显示所有表

![img](https://pic1.zhimg.com/80/v2-6bfee97a47bc341bde9326e6c227b27c_720w.jpg)

5：创建表

说明：

（1）hive里，表示字符串用的是**string**，不用char和varchar

（2）所创建的表，也是HDFS里的一个目录节点

![img](https://pic3.zhimg.com/80/v2-9d576e4bcbc0cd8c0cc34dbb4a7f483a_720w.jpg)

6：增加一条信息

说明：

（1）HDFS不支持数据的修改和删除，因此已经插入的数据**不能够再进行任何的改动**

（2）在Hadoop2.0版本后支持了数据追加。实际上，**insert into 语句执行的是追加操作**

（3）hive支持查询，行级别的插入。不支持行级别的删除和修改

（4）hive的操作实际是执行一个job任务，**调用的是Hadoop的MR**

（5）插入完数据之后，发现HDFS stu目录节点下多了一个文件，文件里存了插入的数据，因此，**hive存储的数据，是通过HDFS的文件来存储的。**

![img](https://pic2.zhimg.com/80/v2-577cb18dc470a4c7c968b90e965c4c91_720w.jpg)

7：查询信息

![img](https://pic2.zhimg.com/80/v2-7f79ecfc3628f000fdd44a2d85619ff5_720w.jpg)

8：删除表

![img](https://pic2.zhimg.com/80/v2-cd3f3c13828a3bfd6667ae3374c3e349_720w.jpg)

9：通过加载文件数据到对应的表里

说明：

在执行完这个指令之后，发现hdfs stu目录下多了一个1.txt文件。由此可见，**hive的工作原理实际上就是在管理hdfs上的文件**，把文件里数据抽象成二维表结构，然后提供hql语句供程序员查询文件数据

![img](https://pic3.zhimg.com/80/v2-85fe30efa0bc48af798257856656fb46_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-75f8881143bc17ac2061280e8e5a3bd9_720w.jpg)



## 元数据**概述**

1. hive用表的形式来管理hdfs上的文件数据。表名，表里有哪些字段，字段类型，哪张表存在那个数据下等这些表的信息，称之为hive的元数据信息。
2. hive默认使用derby数据库作为存储引擎，也就是说元数据信息不存在hdfs上，而是存在这里
3. hive安装完后，通常需要换掉元数据库，目前hive除了支持自带的derby外，只支持mysql元数据库
4. 元数据库的默认字符集为ISO8859-1

**derby存在的问题（为什么要替换掉元数据库？）**

1. derby是一种文件型的数据库，进入会检查当前目录下是否有metastore_db文件夹（存数据库数据）。有直接用，没有就创建，换目录，元数据就找不到了
2. derby为单用户数据库，无法支持多用户同时操作，导致在数据量大连接多的情况下产生大量连接积压

**替换元数据**

1.进入conf目录下，编辑新的配置文件，名字为：hive-site.xml,增加以下内容：

```text
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://hadoop01:3306/hive?createDatabaseIfNotExist=true</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name> 
        <value>root</value> 
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>root</value>
    </property>
</configuration>
```

**2.输入hive观察权限是否足够**

![img](https://pic1.zhimg.com/80/v2-a27b5d36af2f850df3c88003d2b7f5c4_720w.png)如果出现如图错误，表示当前操作mysql的用户权限不够

**3.若权限不够，则进入mysql为当前用户分配权限**

```text
执行：grant all privileges on *.* to 'root'@'hadoop01' identified by 'root' with grant option;
```

![img](https://pic2.zhimg.com/80/v2-785f94c0d5c7459484bcccc8fbb2dc5d_720w.png)

通过root向该用户赋予某个权限

```text
执行：grant all on *.* to 'root'@'%' identified by 'root';
```

![img](https://pic4.zhimg.com/80/v2-13981adbff0bac167f23b18ee469dd97_720w.png)

通过root向该用户赋予全部权限

```text
执行：flush privileges;
```

将当前user和privilige表中的用bai户信息/权限设du置从mysql库(MySQL数据库的内置库)中提取到内存里。用户数据和权限修改后，希望在不重启mysql的情况下直接生效，执行这个命令。

**4.进入数据库执行**

```text
create database hive character set latin1;
```

latin1就是ISO-8859-1的别名，因为元数据的默认字符集为这

**5.再次执行hive，看到如图输出信息，则成功**

![img](https://pic3.zhimg.com/80/v2-cdaf1fc4c8ffa4194bad22b4b1cc0322_720w.jpg)

**6.可以通过DBS 、TBLS、COLUMNS_V2、SDS这几张表来查看元数据信息**

![img](https://pic2.zhimg.com/80/v2-e2bd036dbb53e3b0cc62bcb006bd5455_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-afd00d59a2175779dcc26f489875afa2_720w.jpg)DBS 存放的数据库的元数据信息；TBLS存放的tables表信息；COLUMNS表存放的是列字段信息；SDS表存放的HDFS里的位置信息

## 刚提到了string，那么来说下数据类型

![img](https://pic2.zhimg.com/80/v2-0de7c8df4d1d2b3cbe95071963639eb1_720w.jpg)

## **架构**

![img](https://pic4.zhimg.com/80/v2-7519e1b7ca22fd86ae3cc81f19424e27_720w.jpg)

**从上图可以看出Hive内部架构由四部分构成**

> 1.**用户接口**（最上面一层CLI,JDBC/ODBC(Open Database Connectivity，开放数据库互连),Webui）
> CLI，Shell终端命令行，采用交互形式使用hive命令行与hive进行交互（学习，调试，生产最常用）
> 2.**跨语言服务**（thrift server提供了一种能力，让用户通过不同的语言去操纵hive）
> thrift是Facebook开发的一个软件框架，可以用来进行可扩展和跨语言的开发，hive继承了该服务，能让不同的编程语言调用hive的接口
> 3.**底层的driver**，驱动器的driver，编译器compiler，优化器optimizer，执行器executeor
> driver组件完成HQL查询语句从词法分析，编译，优化，以及生成逻辑执行计划的生成。生成的逻辑执行计划存储在HDFS中，并随后由mapreduce调用执行
> 4.**元数据存储系统**，RDBMS MYSQL
> Hive的元数据包括：表的名字，列，属性（内部表，外部表），数据所在的目录
> metastore默认自带derby数据库。数据存储目录不固定，数据库跟着hive走，极度不方便管理
> hive和metastore通过MySQL服务交互

**执行流程**

HiveQL通过命令行或者客户端进行提交，经过compiler编译器，运用metastore中的元数据进行类型检测和语法分析，生成一个逻辑方案，然后通过优化处理，产生一个MR任务



## 数据组织

> **1.hive的存储结构包括数据库，表，分区和表数据等。前三个对应HDFS上的目录。表数据对应HDFS目录下的文件**
>
> **2.hive的所有数据都存储在HDFS中，没有专门的存储格式，因为Hive是读模式。**
>
> **3.创建表的时候只需要告诉hive数据中的列分隔符和行分隔符，hive就可以解析数据**
> hive的默认列分隔符：crtl+a
> hive的默认行分隔符：换符行\n
>
> **4.hive包含以下数据模型**
> database：在 HDFS 中表现为${hive.metastore.warehouse.dir}目录下一个文件夹
> table:在 HDFS 中表现所属 database 目录下一个文件夹
> external table:与 table 类似，不过其数据存放位置可以指定任意 HDFS 目录路径
> partition:在 HDFS 中表现为 table 目录下的子目录
> bucket:在 HDFS 中表现为同一个表目录或者分区目录下根据某个字段的值进行hash散列之后的多个文件
> view:与传统数据库类似，只读，基于基本表创建

## 内部表和外部表

> 在hive中的表就是内部表，在内部表之上建立的表叫外部表
>
> **区别**
> 删除内部表，**删除表元数据和数据**
> 删除外部表，**删除元数据，不删除数据**
>
> **使用选择**
> 所有的数据处理都在hive中进行，那么倾向于内部表
> 和其他工具要针对相同的数据集做处理，外部表更合适
> hive仅仅只是对存储在HDFS上的数据提供了一种新的抽象。而不是管理HDFS上的数据
> 所以不管创建内部表还是外部表，都可以对hive表的数据存储目录中的数据进行增删操作
>
> **分区表和分桶表的区别**
> 分区和分桶都是细化数据管理，但是分区表是手动添加区分，由于 Hive 是读模式，所 以对添加进分区的数据不做模式校验，分桶表中的数据是按照某些分桶字段进行 hash 散列 形成的多个文件，所以数据的准确性也高很多
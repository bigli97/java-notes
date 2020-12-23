#### 什么是JDBC

就是连接Java和数据库之间交互的API

##### Driver接口

装载驱动:class.forname(“com.mysql.jdbc.Driver”)

##### Connection接口

连接数据库

```java
Connnection conn = 

DriverManager.getConnection(“jdbc:mysql://localhost:3306/db3”,”user”,”password”)
```

##### 常用方法:

createStatement():创建sql执行对象

prepareStatement(sql):对sql进行预编译

##### statement接口

两种statement类

Statement:用于发送简单的sql语句(不带参数)

Preparestatement:发送带有一个或者多个参数的sql语句,预编译,比上面的好能够防止sql注入,效率更高,所以一般都使用这

##### 常用的statement方法

`excute(String sql)`//运行语句,返回是否有结果集

`excuteQuery(String sql)`//运行select语句,返回resultset结果集

`executeupdate(String sql)/`/运行insert/delete/update操作,返回更新的行数

##### ReSultSet常用遍历方法

`getString(int index)/getString(String str)`

`getInt(int index)/getInt(String str)`

##### 使用后一次关闭对象及连接:

Resultset-->statement-->connection

 

#### 使用JDBC的步骤

1. 加载JDBC驱动程序
2. 建立数据库连接
3. 创建执行sql的语句preparestatement
4. 处理执行结果
5. 释放资源

**1:注册驱动(只做一次)**

方式一:

`class.forname(“com.mysql.jdbc.Driver”)`

推荐使用这种方式,不会对具体的驱动类产生依赖

方式二:

`DriverManager.reisterDriver(com.mysql.jdbc.Driver)`

会造成drivermanager中产生两个一样的驱动,会对具体的驱动类产生依赖

建立连接

```java
Connection conn = 

DriverManager.getConnection(“url”,”user”,”pwd”)
```

url用于标识数据库中的位置,通过url地址告诉jdbc程序连接那个数据库,url的写法为

Jdbc:mysql://localhost:8080/test    

↑            ↑                ↑             ↑

协议    子协议    主机端口号    数据库名

 

#### 事务的基本概念

要么执行成功,要么执行失败,是数据库操作的一个执行单元

事务开始于连接到数据库上,并执行一条DML语句

(insert/update/delete)

前一个事务结束后

事务结束于执行commit语句或者执行rollback语句

执行一条DDL语句(create),在这种情况下,会自动执行Commit语句

执行一条DCL语句(grant),在这种情况下,会自动执行Commit语句

执行了一条DML语句,该语句失败了,这种情况下,会为这个无效的DML语句执行rollback语句

 

#### 事务的四大特点

##### 原子性

表示事务所有的操作都是一个整体,要么全部成功,要么全部失败

##### 一致性

表示一个事务内有一个操作失败后,所有更改后的数据必须回滚到修改前的状态

##### 隔离性

事务查看数据时所属的状态,要么是另一并发事务修改他之前的状态,要么是另一事务修改它之后的状态,事务不会查看它中间的数据

##### 持久性

持久性事务完成之后,他对系统的影响是永久的

 

##### 事务级别从低到高

- 读取未提交
- 读取已提交
- 可重复读
- 序列化
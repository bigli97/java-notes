# 怎么学？

官方文档+b站视频

https://baomidou.com/

https://www.bilibili.com/video/BV17E411N7KN?from=search&seid=552730985864517174

 

## 首页说了一堆好的地方，总结以下

1、不影响原有的mybatis，无侵入

2、满足基本的CRUD

3、主键生成策略

4、提供分页插件、拦截插件等

 

## 如何学？

1、导入pom依赖，启动一个hello，world程序

2、研究如何配置，代码如何编写

3、扩展学习



# 快速上手

官网说了，学习这个需要熟悉Java开发，springboot以及maven

 

首先初始化sql数据，在mysql中加入对应数据（参照官网文档）

```mysql
DROP TABLE IF EXISTS user;

CREATE TABLE user
 (
     id BIGINT(20) NOT NULL COMMENT '主键ID',
     name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
     age INT(11) NULL DEFAULT NULL COMMENT '年龄',
     email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
     PRIMARY KEY (id)
 );
```

其对应的数据库 Data 脚本如下：

```mysql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
 (1, 'Jone', 18, 'test1@baomidou.com'),
 (2, 'Jack', 20, 'test2@baomidou.com'),
 (3, 'Tom', 28, 'test3@baomidou.com'),
 (4, 'Sandy', 21, 'test4@baomidou.com'),
 (5, 'Billie', 24, 'test5@baomidou.com');
```

编写项目，使用idea快速初始化springboot工程项目

不要同时导入mybatis和mybatis-plus，可能会冲突

![image-20201224181034784](https://gitee.com/leidl97/picture/raw/master/img/20201224181845.png)

传统方式：连接mybatis，配置mapper文件-service-controller

 

使用mybatis-plus之后

其实类似JPA的使用方式

创建对象

写接口Java类

就可以使用了

 

如图所示，类似JPA，知识语法上有些许不同

![image-20201224181247432](https://gitee.com/leidl97/picture/raw/master/img/20201224181840.png)

## 关键的测试类

踩过的两个坑

Ⅰ：首先需要与上面的目录结构相同，否则将无法注入mapper对象，会有报错提示

![image-20201224181259828](https://gitee.com/leidl97/picture/raw/master/img/20201224181816.png)

其次需要在类上加入@RunWith(SpringRunner.class)

以便开始的时候自动的创建应用上下文，否则将NPE

 

运行结果截图

![image-20201224181324239](https://gitee.com/leidl97/picture/raw/master/img/20201224181810.png)

插入，会自动生成ID

![image-20201224181344500](https://gitee.com/leidl97/picture/raw/master/img/20201224181804.png)

## 主键生成策略

uuid 自增id reids zookeeper 雪花算法

重点了解雪花算法（Twitter的snowflake算法）几乎全球唯一

![image-20201224181359974](https://gitee.com/leidl97/picture/raw/master/img/20201224181754.png) 

修改操作必须赋值ID

## 自动填充

创建时间，修改时间

1、数据库级别（工作中不允许修改数据库）

在数据库中新增两个字段，create_time;update_time。默认值设置current_time

2、代码级别

删除数据库的默认值

实体类字段加注解

![image-20201224181421733](https://gitee.com/leidl97/picture/raw/master/img/20201224181700.png)

```java
编写处理器写注解，实现处理器类重写两个方法，不要了忘了component注解，需要spring去管理
@Component
@Slf4j
publicclassMyMetaObjectHandlerimplementsMetaObjectHandler{
@Override
publicvoidinsertFill(MetaObjectmetaObject){
//插入策略
this.setFieldValByName("createTime",newDate(),metaObject);
this.setFieldValByName("updateTime",newDate(),metaObject);
}

@Override
publicvoidupdateFill(MetaObjectmetaObject){
//更新策略
this.setFieldValByName("updateTime",newDate(),metaObject);
}
}

重要的是pojo类中的tableFileID注解，更新字段是插入和更新的时候都执行的字段
@Data
@AllArgsConstructor
@NoArgsConstructor
publicclassUser{
//@TableId(type=IdType.INPUT)
privateLongid;
privateStringname;
privateIntegerage;
privateStringemail;
@TableField(fill=FieldFill.INSERT)
privateDatecreateTime;
@TableField(fill=FieldFill.INSERT_UPDATE)
privateDateupdateTime;
}
```

如图所示注解发挥作用

![image-20201224181530878](https://gitee.com/leidl97/picture/raw/master/img/20201224181651.png)



# 乐观锁

乐观锁顾名思义就是乐观， 认为做什么也不用上锁

按照官网来讲

当要更新一条数据的时候，希望这条记录没有被别人更新

乐观锁的实现方式

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时，     set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

 

先查询，获得版本号，更新带上，update时候version改变，version不对就失败

 

方法

1、数据库增加version字段

![image-20201224182443421](https://gitee.com/leidl97/picture/raw/master/img/20201224182520.png)

2、实体类更新，增加version注解

![image-20201224182533354](https://gitee.com/leidl97/picture/raw/master/img/20201224182533.png)

3、注册组件

```java
@EnableTransactionManagement//开启事务
@MapperScan("com.mapper")
@Configuration
publicclassMybatisPlusConfig{
//注册乐观锁插件
@Bean
publicOptimisticLockerInterceptoroptimisticLockerInterceptor(){
returnnewOptimisticLockerInterceptor();
}
}

上述方法为3.0.5方法，目前已弃用！下面为3.4.1方法
//注册乐观锁插件
@Bean
publicMybatisPlusInterceptormybatisPlusInterceptor(){
MybatisPlusInterceptorinterceptor=newMybatisPlusInterceptor();
interceptor.addInnerInterceptor(newOptimisticLockerInnerInterceptor());
returninterceptor;
}
```

千万不要忘了bean注解！！！

他会自动找到version（如果不为null）并拼接

![image-20201224182616135](https://gitee.com/leidl97/picture/raw/master/img/20201224182616.png)

如果中途遇到插队现象，那么将不会执行成功

```java
//乐观锁失败
@Test
publicvoidtestLockDefeat(){
//多线程操作
//查询用户信息
Useruser=userMapper.selectById(1L);
//修改用户信息
user.setName("leidl111");

//线程2，中途插队
Useruser2=userMapper.selectById(1L);
user2.setName("leidl222");
userMapper.updateById(user2);

//更新用户信息，如果没有乐观锁，就会覆盖之前的值
userMapper.updateById(user);
}

```

![image-20201224182639915](https://gitee.com/leidl97/picture/raw/master/img/20201224182639.png)



# 分页插件

1、处理器

*//分页*

`interceptor.addInnerInterceptor(newPaginationInnerInterceptor(DbType.MYSQL));`

 

2、编写测试类

```java
@Test

publicvoidselectAll(){

log.info("开始查询！");

//查询全部用户，条件为构造器

//当前页，页面大小

Page<User>userPage=newPage<>(2,5);

Page<User>userPage1=userMapper.selectPage(userPage,null);

log.info("结束！");

userPage1.getRecords().forEach(x->System.out.println(x));

}
```

相当于加了一个limit

![image-20201224182721387](https://gitee.com/leidl97/picture/raw/master/img/20201224182721.png)



# 逻辑删除

yml增加配置

mybatis-plus:

global-config:

db-config:

logic-delete-field:user-flag#全局逻辑删除的实体字段名(since3.3.0,配置后可以忽略不配置步骤2)

logic-delete-value:1#逻辑已删除值(默认为1)

logic-not-delete-value:0#逻辑未删除值(默认为0)



数据库增加字段，这次使用了user_flag对应yml中就是user-flag，pojo中为userFlag

![image-20201224182842500](https://gitee.com/leidl97/picture/raw/master/img/20201224182842.png)

user类代码，其中默认值采取自动填充生成

```java
@TableLogic
@TableField(fill=FieldFill.INSERT)
privateIntegeruserFlag;

自动填充代码
this.setFieldValByName("userFlag",0,metaObject);


测试代码
@Test
publicvoidlogicDelete(){
log.info("开始删除！");
userMapper.deleteById(1333650597286621186L);
}
```

结果，使用update语句进行了逻辑删除

![image-20201224182914569](https://gitee.com/leidl97/picture/raw/master/img/20201224182914.png)



# 条件构造器

代码

```java
@Test

publicvoidwrapper(){

QueryWrapper<User>wrapper=newQueryWrapper<>();

wrapper.eq("name","dali");

List<User>users=userMapper.selectList(wrapper);

users.forEach(System.out::println);

}
```

会自动拼接构造器中的内容 

![image-20201224183004991](https://gitee.com/leidl97/picture/raw/master/img/20201224183005.png)





# 代码生成器

```java
1、引入pom依赖
<!--模板生成器-->
<dependency>
<groupId>com.baomidou</groupId>
<artifactId>mybatis-plus-generator</artifactId>
<version>3.4.1</version>
</dependency>
<!--模板引擎-->
<dependency>
<groupId>org.apache.velocity</groupId>
<artifactId>velocity-engine-core</artifactId>
<version>2.2</version>
</dependency>


2、编写生成器类
@Slf4j
publicclassCodeGenerator{
publicstaticvoidmain(String[]args){
//代码生成器对象
AutoGeneratorautoGenerator=newAutoGenerator();
//配置策略

//1、全局配置
GlobalConfigglobalConfig=newGlobalConfig();
Stringproperty=System.getProperty("user.dir");
log.info("*****************"+property+"*****************");
globalConfig.setOutputDir(property+"/src/main/java");
globalConfig.setAuthor("dali");
//是否打开
globalConfig.setOpen(false);
//是否覆盖
globalConfig.setFileOverride(false);
globalConfig.setSwagger2(true);
autoGenerator.setGlobalConfig(globalConfig);

//数据源配置
DataSourceConfigdsc=newDataSourceConfig();
dsc.setUrl("jdbc:mysql://localhost:3306/db2020?useUnicode=true&characterEncoding=utf-8&useSSL=true&serverTimezone=UTC");
dsc.setDriverName("com.mysql.cj.jdbc.Driver");
dsc.setUsername("root");
dsc.setPassword("root");
dsc.setDbType(DbType.MYSQL);
autoGenerator.setDataSource(dsc);

//包配置
PackageConfigpc=newPackageConfig();
pc.setModuleName("test");
pc.setParent("com");
pc.setController("controller");
pc.setEntity("pojo");
pc.setMapper("mapper");
autoGenerator.setPackageInfo(pc);

//策略配置
StrategyConfigstrategy=newStrategyConfig();
strategy.setNaming(NamingStrategy.underline_to_camel);
strategy.setColumnNaming(NamingStrategy.underline_to_camel);
strategy.setEntityLombokModel(true);
strategy.setLogicDeleteFieldName("user_flag");
strategy.setInclude("user");

//自动填充配置
TableFillcreateTime=newTableFill("create_time",FieldFill.INSERT_UPDATE);
TableFillupdateTime=newTableFill("update_time",FieldFill.UPDATE);
ArrayList<TableFill>tableFills=newArrayList<>();
tableFills.add(createTime);
tableFills.add(updateTime);
strategy.setTableFillList(tableFills);

//乐观锁
strategy.setVersionFieldName("version");
//驼峰命名
strategy.setRestControllerStyle(true);
//链接
strategy.setControllerMappingHyphenStyle(true);

autoGenerator.setStrategy(strategy);
//执行
autoGenerator.execute();
}
```

结果如下

![image-20201224183105461](https://gitee.com/leidl97/picture/raw/master/img/20201224183105.png)



注释什么的还挺全（就是一堆报错，这个应该是设置策略的时候没考虑全面）

![image-20201224183119330](https://gitee.com/leidl97/picture/raw/master/img/20201224183119.png)

![image-20201224183123913](https://gitee.com/leidl97/picture/raw/master/img/20201224183123.png)
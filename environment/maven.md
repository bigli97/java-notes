是什么

管理Java类库的一个工具

 

安装

1、maven历史版本下载

https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/

 

2、配置环境变量

![image-20201223190243721](https://gitee.com/leidl97/picture/raw/master/img/20201223190243.png)

 

3、在dos命令窗口输入mvn --version验证。显示如下则成功

![image-20201223190309246](https://gitee.com/leidl97/picture/raw/master/img/20201223190309.png)

安装maven建议路径中无空格

 

修改配置文件conf文件夹下的setting.xml

更改本地仓库

<localRepository>D:\m2\repository</localRepository>

![image-20201223190334707](https://gitee.com/leidl97/picture/raw/master/img/20201223190334.png)

添加maven的默认JDK版本

```xml
<profiles>

<profile>  

​       <id>jdk-1.8</id>  

​      <activation>  

​      <activeByDefault>true</activeByDefault>  

​      <jdk>1.8</jdk>  

​      </activation>  

​      <properties>  

​      <maven.compiler.source>1.8</maven.compiler.source>  

​      <maven.compiler.target>1.8</maven.compiler.target>  

​      <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  

​      </properties>  

​     </profile> 

</profiles>
```



添加国内镜像源	

```xml
<mirrors>
  <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
```


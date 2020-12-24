#### 1、从官网下载安装包

https://tomcat.apache.org/download-80.cgi

 

#### 2、上传解压到/usr/local下

 

#### 3、配置环境变量（jdk是前提）

vi /etc/profile

\#tomcat

export CATALINA_HOME=/usr/local/tomcat-8.5.57

\#jdk

export JAVA_HOME=/usr/local/jdk1.8

export PATH=$PATH:$JAVA_HOME/bin

 

#### 4、配置tomcat的catalina.sh文件，进入tomcat的bin目录

vi catalina.sh

:/OS specific support(搜索这个然后在这行下面添加以下配置)

CATALINA_HOME=/usr/local/tomcat-8.5.57

JAVA_HOME=/usr/local/jdk1.8

![image-20201224113700930](https://gitee.com/leidl97/picture/raw/master/img/20201224113701.png)



####  5、安装tomcat服务（就不用每次去bin目录下启动了）

cp /usr/local/tomcat-8.5.57/bin/catalina.sh /etc/init.d/tomcat

启动tomcat服务

service tomcat start

停止

service tomcat stop

![image-20201224113610849](https://gitee.com/leidl97/picture/raw/master/img/20201224113634.png)
1:上传jdk压缩包

 

2:**解压jdk1.8**

tar -xvf jdk-8u91-linux-x64.tar.gz 

 

3:改名（不是必须步骤）

mv jdk1.8.0_91 jdk1.8

 

4:移动（不是必须步骤）

mv jdk1.8 /usr/local/

 

5:**修改环境变量**

vi /etc//profile

在文件尾部添加如下信息

export JAVA_HOME=/usr/local/jdk1.8

export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/ 

export PATH=$PATH:$JAVA_HOME/bin

注意第一行是jdk文件实际路径

 

6:**刷新环境变量配置**

source /etc/profile
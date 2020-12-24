# Linux下MySQL5.7的tar方式安装

**适合人群**

1. 刚好不能使用wget和yum命令的
2. 需要用tar安装的
3. root进行操作

**前言**

- 我的Linux安装的是**RedHat7.4**发行版本，其实我还是在推荐刚开始学习Linux的时候装centos，Redhat在使用的过程中会遇到许多问题，让你去注册它的账号才能去使用一些命令，总之是特别麻烦。那么我为什么装RedHat呢？没办法工作需要，MySQL安装方式也可以使用yum源傻瓜式安装，不过我建议初学者还是使用tar包安装方式比较好。方便理解其中的过程。之后遇到什么问题，找起来也会更加容易。我这篇文章安装MySQL全程**不需要yum命令**（当时还没进行换源操作）。



**安装前提**

1. 使用root用户操作。
2. 关闭防火墙（上传MySQL的tar包）
3. 修改网络配置（如果不用xShell，finalShell之类的工具可以不配，黑框框也非常不方便，而且实际过程中也不会允许你直接操作虚拟机，所以现在不配，以后也要配）

**安装步骤如下**

## 1.检查服务器是否已安装MySQL，如已安装则将其卸载:

rpm -qa|grep mysql （-q表示使用询问模式，当遇到任何问题时，rpm指令会先询问用户。-a表示查询所有套件。）[新机无mysql]

```text
whereis mysql
rm -rf /usr/lib64/mysql/
rm -rf /usr/share/mysql/
find / -name mysql
rm -rf /etc/selinux/targeted/active/modules/100/mysql
rm -rf /var/tmp/dracut.6oP6zK/initramfs/usr/lib64/mysql
```

## 2.检查服务器是否已安装Mariadb，如已安装则将其卸载:

```text
rpm -qa|grep mariadb
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
rpm -e --nodeps xxx(文件名) //卸载 （-e表示删除指定的套件）
rm -rf (路径）删除（-r表示迭代，-f表示不询问，强制删除）
```

## 3.上传MySQL包，并解压

上传可以通过XFtp或者FinalShell之类的软件完成，也可以yum的也可以使用yum -y install lrzsz，就可以直接拖了。我是用的第一种方式。

MySQL包怎么获得

（1）wget [https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz](https://link.zhihu.com/?target=https%3A//dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz)（没有换源的话wget也是不能用的，自己在网上下个吧）

（2）链接：[https://pan.baidu.com/s/1hhUYQA6BvCMkhHB6Rt2Vbw](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1hhUYQA6BvCMkhHB6Rt2Vbw) 提取码：wsuk

解压

```text
tar -zvxf mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz （-z表示通过gzip指令处理备份文件。
						       							-v表示显示指令执行过程。
						      				 			-x表示从备份文件中还原文件。
                                                       	-f表示指定备份文件。）
```

## 4.建议改名

```text
mv mysql-5.7.22-linux-glibc2.12-x86_64 mysql
```

## 5.移动（不是硬性的，不移动也不影响结果）

想清楚为什么linux安装程序 都要放到/usr/local目录下：点击下方链接

[原因]: https://link.zhihu.com/?target=https%3A//blog.csdn.net/u011495642/article/details/83655605

```text
mv mysql /usr/local/
```

## 6.添加MySQL组和用户

```text
groupadd mysql
useradd -r -g mysql mysql （-r表示建立系统账号，-g表示指定所属群组）
```

## 7.创建mysql数据目录，回到根目录

```text
cd /
mkdir -p data （-p表示如果没有才创建）
cd data/
mkdir -p mysql 
```

## 8.授权

```text
chown mysql:mysql -R /data/mysql
```

## 9.修改配置文件

```text
vi /etc/my.cnf
```

删除原来内容粘贴以下内容

```text
[mysqld]
bind-address=0.0.0.0
port=3306
user=mysql
basedir=/usr/local/mysql
datadir=/data/mysql
socket=/tmp/mysql.sock
log-error=/data/mysql/mysql.err
pid-file=/data/mysql/mysql.pid
#character config
character_set_server=utf8mb4
symbolic-links=0
```

完成之后按esc键+：wq退出

## 10.初始化MySQL

进入mysql的bin目录

```text
cd /usr/local/mysql/bin/
```

进行初始化（完了没提示属于正常现象）

```text
./mysqld --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql/ --datadir=/data/mysql/ --user=mysql --initialize
```

查看初始化密码（记得保存密码）**MySQL5.7之后默认密码不再是空**

```text
vi /data/mysql/mysql.err
```

## 11.启动MySQL服务

```text
service mysql start
```

登录MySQL

```text
mysql -uroot -p
```

**建议更改密码**

```text
SET PASSWORD = PASSWORD('root');
ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;（保证密码永不过期）
flush privileges;（MySQL用户数据和权限有修改后，希望在"不重启MySQL服务"的情况下直接生效，那么就需要执行这个命令。）
```

如果报错信息如下

**A.service mysql启动失败 提示unit not found 解决办法**

查看是否有mysql服务

```text
ll /etc/init.d/ | grep mysql
```

如果上面没有继续执行

```text
find / -name mysql.server
```

将找到的路径复制到 /etc/init.d/mysql路径中

```text
cp /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysql
```

然后重启mysql服务

**B.mysql command not found**

1. 增加软连接：ln -s /usr/local/mysql/bin/mysql /usr/bin
2. 找到mysql/bin的位置，正常情况下是 /usr/local/mysql/bin在最后添加：export PATH=$PATH:/usr/local/mysql/bin 保存退出。

使修改生效：source /etc/profile

## 12.额外---MySQL开机自启

如果不想每次开机都去启动MySQL服务，那么就设置吧

1. cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql 将服务文件拷贝到init.d下，并重命名为mysql
2. 确保 /etc/init.d/ 目录下有mysql.server
3. **赋予可执行权限 chmod +x /etc/init.d/mysql**
4. **添加服务 chkconfig --add mysql**
5. **显示服务列表 chkconfig --list 如果看到mysql的服务，并且3,4,5都是on的话则成功如果是off，则键入chkconfig --level 345 mysql on**
6. reboot重启电脑
7. netstat -na | grep 3306，如果看到有监听说明服务启动了
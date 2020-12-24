# docker的安装

本次安装采用发行版为centos7进行安装（必须是7以上）

安装官方链接https://docs.docker.com/engine/install/centos/

#### 自行安装

1.查看有没有安装docker

yum list installed | grep docker

有就卸载，没有就下一步

2.安装docker

yum -y install docker

3.启动docker

systemctl start docker

4.查看docker状态

systemctl status docker

![image-20201224155233178](https://gitee.com/leidl97/picture/raw/master/img/20201224155233.png)

5.通过运行hello-world 映像来验证是否正确安装了Docker Engine 。

$ sudo docker run hello-world

![image-20201224155253866](https://gitee.com/leidl97/picture/raw/master/img/20201224155253.png)

安装方法使用存储库进行安装

您可以根据需要以不同的方式安装Docker Engine：

- 大多数用户会 [设置Docker的存储库](https://docs.docker.com/engine/install/centos/#install-using-the-repository)并从中进行安装，以简化安装和升级任务。这是推荐的方法。
- 一些用户下载并[手动安装](https://docs.docker.com/engine/install/centos/#install-from-a-package) RPM软件包， 并完全手动管理升级。这在诸如在无法访问互联网的空白系统上安装Docker的情况下很有用。
- 在测试和开发环境中，一些用户选择使用自动 [便利脚本](https://docs.docker.com/engine/install/centos/#install-using-the-convenience-script)来安装Docker。

#### 使用存储库安装

在新主机上首次安装Docker Engine之前，需要设置Docker存储库。之后，您可以从存储库安装和更新Docker。

设置存储库

安装yum-utils软件包（提供yum-config-manager 实用程序）并设置**稳定的**存储库。

```bash
$ sudo yum install -y yum-utils
$ sudo yum-config-manager 
-add-repo  https://download.docker.com/linux/centos/docker-ce.repo
```

#### **下面为可选**

**可选：启用每晚或测试存储库。**

这些存储库包含在docker.repo上面的文件中，但默认情况下处于禁用状态。您可以在稳定存储库旁边启用它们。以下命令启用**每晚**存储库。

$ sudo yum-config-manager --enable docker-ce-nightly

要启用**测试**通道，请运行以下命令：

$ sudo yum-config-manager --enable docker-ce-test

您可以通过运行带有标志的命令来禁用**每晚**或**测试**存储库 。要重新启用它，请使用该标志。以下命令禁用**夜间**存储库。yum-config-manager--disable--enable

$ sudo yum-config-manager --disable docker-ce-nightly

#### 安装docker引擎

1. 安装最新版本的Docker Engine和容器，或转到下一步以安装特定版本：
             `$ sudo yum install docker-ce docker-ce-cli containerd.io`

   如果提示您接受GPG密钥，请验证指纹是否匹配 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35，如果是，则接受它。

   ##### 有多个Docker存储库吗？

   如果您启用了多个Docker存储库，则在未在yum installor yum update命令中指定版本的情况下进行安装或更新将始终安装可能的最高版本，这可能不适合您的稳定性需求。
             

   Docker已安装但尚未启动。docker创建该组，但没有用户添加到该组。

2. 要安装*特定版本*的Docker     Engine，请在存储库中列出可用版本，然后选择并安装。

   列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序，并被截断：
             

   `$ yum     list docker-ce --showduplicates | sort -r`

   `docker-ce.x86_64 3:18.09.1-3.el7               docker-ce-stable`
             

   `docker-ce.x86_64      3:18.09.0-3.el7               docker-ce-stable`
             

   `docker-ce.x86_64      18.06.1.ce-3.el7               docker-ce-stable`
             

   `docker-ce.x86_64      18.06.0.ce-3.el7               docker-ce-stable`
             
   返回的列表取决于启用的存储库，并且特定于您的CentOS版本（.el7在此示例中以后缀表示）。
             

   通过其完全合格的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如，docker-ce-18.09.1。

   `$ sudo yum install     docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING>     containerd.io`          

   Docker已安装但尚未启动。docker创建该组，但没有用户添加到该组。

   3.启动Docker       

   `$ sudo systemctl start docker`

   4.通过运行hello-world 映像来验证是否正确安装了Docker Engine 。
             

   `$ sudo do`cker run hello-world`
   `          

   此命令下载测试图像并在容器中运行。容器运行时，它会打印参考消息并退出。

   Docker Engine已安装并正在运行。您需要使用sudo来运行Docker命令。继续进行[Linux后安装，](https://docs.docker.com/engine/install/linux-postinstall/)以允许非特权用户运行Docker命令以及其他可选配置步骤。

   升级DOCKER引擎

   要升级Docker Engine，请按照[安装说明](https://docs.docker.com/engine/install/centos/#install-using-the-repository)，选择要安装的新版本。

   #### 卸载Docker 

   1. 卸载Docker Engine，CLI和Containerd软件包：

      `$ sudo yum remove docker-ce     docker-ce-cli containerd.io`

   2. 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷：

      `$ sudo rm -rf /var/lib/docker`

   您必须手动删除所有已编辑的配置文件。

# 基础知识

## 为什么会出现

为了避免部署失败。一次构件，处处运行

**轻量，高效，便捷**

**开发**交给**运维**的时候会出现相爱相杀的情况比如在开发那能跑通，到运维那里就不可以了

这就出现了环境与配置的问题

docker应运而生

就相当于将鱼和鱼缸+水都给打包过去。

不然换一台机器就得重来一次。开发人员利用docker可以消除编码时“在我的机器上能正常运行的问题”

以前是搬家，现在是搬楼

以前开发人员只交代码，现在还需要+运行文档+配置环境+运行环境+运行依赖包+操作系统发行版+内核

这些加起来叫镜像

## 理念

docker基于Go语言开源项目

能够通过对应用组件的封装，分发，部署，运行等生命周期的管理，使其能够做到一此封装，到处运行

软件清单替换为打包集装箱，只装一个docker只要一个镜像

## 是什么

解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术

## 能干什么

虚拟化安装

虚拟机就是带环境安装的一种解决方案

比如windows装Linux的系统，模拟的是整套操作系统

缺点就是1、启动慢，一两分钟。2、资源占用多。3、步骤多

Linux容器，相当于阉割版虚拟机，只需要软件工作所需的库资源和设置。系统变得轻量，高效

## 与传统虚拟化的不同之处

1、传统是虚拟出一套硬件后，在其上运行的一个完整的操作系统，在该系统上在运行所需应用进程

2、容器内的应用进程直接运行于宿主的内核，容器没有自己的内核，没有进行硬件虚拟。比传统更轻便

3、每个容器之间互相隔离，每个容器有自己的文件系统，容器进程间不会相互影响，能区分计算资源

## 三要素

**镜像，容器，仓库**

**镜像**就是**只读**的模板，可以创建docker容器，一个镜像可以创建很多容器

**容器**就是镜像的一个实例，是用镜像创建的运行实例，可以被启动开始停止删除，

每个容器都是相互隔离的，保证平台的安全性

容器可以看作简易版的Linux的环境（包括root用户权限，进程空间，用户空间和网络空间等）和运行在其中的应用程序。容器最上面一层是可读可写的

| docker | 面向对象 |
| ------ | -------- |
| 镜像   | 类       |
| 容器   | 对象     |

### 仓库集中存放镜像文件的场所

仓库和仓库注册服务器是有区别的。仓库注册服务器上往往放着多个仓库，每个仓库又包含了多个镜像，每个镜像都有不同的标签（tag）

仓库分为公开仓库和私有仓库两种形式

最大的公开仓库是docker hub

存放了数量庞大的镜像供用户下载

国内的公开仓库包括阿里云，网易云等

### 正确的理解镜像容器仓库的概念

docker本身是一个容器运行载体或称之为管理引擎，我们把应用程序和配置依赖打包形成一个可交付的运行环境，这个打包好的运行环境就类似image镜像文件。只有通过这个镜像文件才能生成docker容器。image文件可以看作是容器的模板。docker根据image文件生成容器的实例。同一个image文件，可以生成多个同时运行的容器实例。

image文件生成的容器实例，本身也是一个文件

一个容器运行一种服务，当我们需要的时候，就可以通过docekr客户端创建一个对应的运行实例，也就是我们的容器

至于仓储，就是放了一堆镜像的地方，我们可以把镜像发布到仓储中，需要的时候从仓储拉下来就可以了。

## 阿里云镜像加速

默认去docker hub就找，外网特别慢，所以要进行阿里云镜像加速配置

https://www.bilibili.com/video/BV1Vs411E7AR?p=9

**1. 安装／升级Docker客户端**

推荐安装1.10.0以上版本的Docker客户端，参考文档 [docker-ce](https://yq.aliyun.com/articles/110806)

**2. 配置镜像加速器**

针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```json
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://jxb6bo4h.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

docker info查看换源是否成功

 # 进阶知识

## 机制原理

docker run hello-world。run干了什么？

![image-20201224160412857](https://gitee.com/leidl97/picture/raw/master/img/20201224160412.png)

docker是一个client-server结构的系统，docker守护进程运行在主机上，然后通过socket连接从客户端访问，守护进程从客户端接收命令并运行在主机上的容器，容器是一个运行时环境，就是前面说到的集装箱

1、docker有比虚拟机更少的抽象层，docker不需要实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在cpu，内存利用率上docker将会在效率上有明显优势

2、docker利用的是宿主机的内核，而不需要Guest OS（虚拟机中的系统）。因此，当新建一个容器时，docker不需要和一个虚拟机一样重新加载一个操作系统内核。避免引寻，加载操作系统内核比较费时费资源的过程，当新建一个虚拟机的时候，虚拟机需要加载Guest OS，新建过程时分钟级别的，而docker直接利用宿主机的操作系统，省略了整个过程，因此新建一个docker容器只需要几秒钟。

|            | docker容器              | 虚拟机                      |
| ---------- | ----------------------- | --------------------------- |
| 操作系统   | 与宿主机共享OS          | 宿主机上运行虚拟机OS        |
| 存储大小   | 镜像小，便于存储和传输  | 镜像庞大（vmdk，vdi等）     |
| 运行性能   | 几乎无额外性能损失      | 操作系统额外的CPU，内存消耗 |
| 移植性     | 轻便，灵活，适应于Linux | 笨重，与虚拟化技术耦合度高  |
| 硬件亲和性 | 面向软件开发者          | 面向硬件运维者              |

## 镜像原理

UnionFS（联合文件系统）：Union文件系统是一种分层的，轻量级且高性能的文件系统，**它支持对文件系统的修改作为一次提交来一层层的叠加**。同时将不同目录挂载到同一个虚拟文件系统下。Union文件系统是docker镜像的基础，镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的的应用镜像

**特性**

一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

**镜像加载原理**

镜像实际上是由一层一层的文件系统组成，这种层级的文件系统UnionFS主要包含bootloader和kernel（内核），前者主要引导加载后者，Linux刚启动时会加载bootfs文件系统，**在docker镜像的最底层时bootfs**。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成后整个内核就都在内存中了，此时内存的使用权由bootfs转交给内核，此时系统也会卸载bootfs

rootfs在bootfs上，包含的就是典型Linux系统中的/dev，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，就比如centos等。（bootfs为了启动哪些文件，启动完就卸载）

对于一个精简的OS，rootfs很小，只需要包括**最基本的命令，工具和程序库**就可以了。底层直接使用HOST和kernel，自己只需要提供roofs就可以了，所以不同的Linux发行版，bootfs基本一致，rootfs会有差别，因此不同的Linux发行版可以共用bootfs

![image-20201224160548469](https://gitee.com/leidl97/picture/raw/master/img/20201224160548.png)

为什么要采用分层结构呢

最大的一个好处---**共享资源**

例子：比如有多个镜像都从相同的base镜像构建而来，那么宿主机只需要在磁盘上保存着一份base镜像

同时内存中也只需要加载一份base镜像，就可以为所有容器进行服务了，而且镜像的每一层都可以被共享

容器层之下的都叫镜像

镜像commit

相当于保存配置

docker run -it -p 8080:8080 tomcat

使用curl localhost:8080命令去验证

![image-20201224160629513](https://gitee.com/leidl97/picture/raw/master/img/20201224160629.png)

## 容器数据卷

### 作用

做持久化

### 特点

1、数据卷可以在容器之间**共享或重用数据**

2、卷中的更改可以直接生效

3、数据卷的更改不会包含在镜像的更新中

4、数据卷的生命周期一直持续到没有容器使用它为止

### 命令

用直接命令添加

docker run -it -v /宿主机绝对路径目录：/容器内目录 ---- 镜像名（v 卷的缩写）

docker run -it -v /myDataVolume:/dataVolumeContainer centos

宿主机和容器多出两个文件夹

![image-20201224160751344](https://gitee.com/leidl97/picture/raw/master/img/20201224160751.png)

![image-20201224160757930](https://gitee.com/leidl97/picture/raw/master/img/20201224160758.png)

在宿主机建立一个文件夹，会在容器机同步显示

 

### 进去发现没有权限

connot open directory.:permission denied

解决办法：在挂在目录后多加一个--privileged=true。比如说

docker run -it -v /myDataVolume:/dataVolumeContainer --privileged=true

 

docker run -it -v /宿主机绝对路径目录：/容器内目录:ro ---- 镜像名（v 卷的缩写）

docker run -it -v /myDataVolume:/dataVolumeContainer:ro centos

 

### DockerFile添加

1、根目录下新建mydocker文件夹

2、可在dockerfile中使用VOLUME指令来给镜像添加一个或多个数据卷

3、file构建

vim dockerfile

FROM centos

VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]

CMD echo "finshed---------success"

CMD /bin/bash

4、build后生成镜像

docker build -f /mydocker/Dockerfile -t dali/centos .

![image-20201224160844753](https://gitee.com/leidl97/picture/raw/master/img/20201224160844.png)

5、run容器

6、主机对于默认地址

### 什么是docekrfile

Java hello.java ----- hello.class

docker dockerfile ------ image 

容器间传递共享

## dockerfile

**构建三步骤**

编写，构建，执行

 

scratch所有镜像文件的祖先类

CMD 命令

有时候加/bin/bash，不加也没关系，文本后自动加/bin/bash

**是什么**

用来构建docker镜像的构建文件，是由一系列命令和参数构成的脚本

 

**构建过程**

基础知识

1、每条保留字指令都必须为大写字母且后面要至少跟随一个参数

2、指令按照从上到下顺序执行

3、#表示注释

4、每条指令都会创建一个新的镜像层，对镜像进行提交

 

docker执行docekrfile的大致流程

1、docker从基础镜像运行一个容器

2、执行一条指令并对容器做出修改

3、执行类似docker commit的操作提交一个新的镜像层

4、docker在基于刚提交的镜像运行一个新容器

5、执行dockerfile的下一条指令知道所有的指令都执行完成

 

从应用软件上来看，dockerfile，docker镜像与docker容器分别代表软件的三个不同阶段

docker是软件的原材料

docker镜像是软件的交付品

docker容器可认为是软件的运行态

dockerfilie面向开发，docker为交付标准，docker容器涉及部署与运维，三者缺一不可，合力充当docker体系的基石

![image-20201224160943929](https://gitee.com/leidl97/picture/raw/master/img/20201224160944.png)

他们各自的定义

1、dockerfile，需要定义一个dockerfile，dockerfile定义了进程需要的一切东西。涉及的内容包括执行代码或者是文件，环境变量，依赖包，运行时环境，动态链接库，操作系统发行版，服务进程和内核进程（当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制）等等

2、docker镜像。在用一个dockerfile定义后，docker build时会产生一个docker镜像，当运行docker镜像时，会真正开始提供服务

3、docker容器，容器是直接提供服务的

###  保留字指令

FROM

基础镜像，当前新镜像是基于哪个镜像的

MAINTAINER

作者名字+作者邮箱

RUN

系统构建时需要运行的命令

EXPOSE

对外服务的端口号

WORKDIR

指定在创建容器后，终端默认登录的进来工作目录，一个落脚点（没有指定就是根目录）

ENV

用来在构建镜像过程中设置环境变量

ADD

拷贝+解压缩

COPY

拷贝

VOLUME

保存数据并持久化

CMD

指定容器启动时执行的命令，但是只有最后一个生效，会被覆盖替换

ENTRYPOINT

指定容器启动时执行的命令

ONBUILD

父镜像不执行，子镜像执行

当构建一个被继承的dockerfile时运行命令，父镜像在被子继承后父镜像的ONBUILD触发

![image-20201224161030979](https://gitee.com/leidl97/picture/raw/master/img/20201224161031.png)

docker rm $(docker ps -a)集体删除容器

![image-20201224161137363](https://gitee.com/leidl97/picture/raw/master/img/20201224161137.png)

编写自定义文档

```bash
FROM centos
MAINTAINER dali<dali666@163.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "successful"
CMD /bin/bash

构建镜像
docker build -f /mydocker/Dockerfile2 -t mycentos:1.1 .
```

![image-20201224161214864](https://gitee.com/leidl97/picture/raw/master/img/20201224161214.png)

![image-20201224161218600](https://gitee.com/leidl97/picture/raw/master/img/20201224161218.png)

CMD和ENTRYPOINT镜像案例

都是指定一个容器启动时要启动的命令

可以有多个CMD指令，但只有最后一个生效，CMD会被run之后的参数替换

 curl 返回网页的form表单

 ```bash
FROM centos
RUN yum -y install curl
ENTRYPOINT ["curl","-s",https://www.baidu.com"]
ONBUILD RUN echo "father image onbuild--------------88"
 ```

![image-20201224161300022](C:/Users/93593/AppData/Roaming/Typora/typora-user-images/image-20201224161300022.png)

![image-20201224161306064](C:/Users/93593/AppData/Roaming/Typora/typora-user-images/image-20201224161306064.png)

docker build -t xxx .（不写-f + 路径表示默认读取当前路径下的Dockerfile）

 

一图胜千言

![image-20201224161343818](https://gitee.com/leidl97/picture/raw/master/img/20201224161344.png)

# 命令

## 基础命令

`docker version`

![image-20201224161440538](https://gitee.com/leidl97/picture/raw/master/img/20201224161440.png)

`docker info（对个人信息的一些描述，比上面显示的多）`

![image-20201224161454505](https://gitee.com/leidl97/picture/raw/master/img/20201224161454.png)

`docker --help`

![image-20201224161529208](https://gitee.com/leidl97/picture/raw/master/img/20201224161529.png)

## 镜像命令

鲸鱼背上集装箱

在蓝色的大海里-------宿主机系统win10

鲸鱼-------------docker

集装箱-----------容器实例（来自我们的镜像）

docker images（列出**本地**的镜像）【repsotory镜像的仓库源】

![image-20201224161617334](https://gitee.com/leidl97/picture/raw/master/img/20201224161617.png)

```bash
• -a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
• --digests :显示镜像的摘要信息；
• -f :显示满足条件的镜像；
• --format :指定返回值的模板文件；
• --no-trunc :显示完整的镜像信息；
• -q :只显示镜像ID
docker search tomcat（ 从Docker Hub查找镜像）
• --automated :只列出 automated build类型的镜像；
• --no-trunc :显示完整的镜像描述；
-s :列出收藏数不小于指定值的镜像。
```

![image-20201224161644564](https://gitee.com/leidl97/picture/raw/master/img/20201224161644.png)

```bash
docker pull xxx：TAG（下载镜像）
• -a :拉取所有 tagged 镜像
• --disable-content-trust :忽略镜像的校验,默认开启
没写版本号，默认下最新版本
```

![image-20201224161701000](https://gitee.com/leidl97/picture/raw/master/img/20201224161701.png)

```bash
docker rmi xxx：TAG（删除本地一个或多少镜像。）
• -f :强制删除；
• --no-prune :不移除该镜像的过程镜像，默认移除；
没写版本号，默认删除最新版本
```

![image-20201224161713847](https://gitee.com/leidl97/picture/raw/master/img/20201224161713.png)

## 容器命令

有镜像才能创建容器，所以先拉一个centos的容器

```bash
docker pull centos
1、新建并启动容器
docker run centos
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
• -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
• -d: 后台运行容器，并返回容器ID；
• -i: 以交互模式运行容器，通常与 -t 同时使用；（代表我要和你交互）
• -P: 随机端口映射，容器内部端口随机映射到主机的端口
• -p: 指定端口映射，格式为：主机(宿主)端口:容器端口
• -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；（代表弹出一个命令行）
• --name="nginx-lb": 为容器指定一个名称；
• --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
• --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
• -h "mars": 指定容器的hostname；
• -e username="ritchie": 设置环境变量；
• --env-file=[]: 从指定文件读入环境变量；
• --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
• -m :设置容器使用内存最大值；
• --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
• --link=[]: 添加链接到另一个容器；
• --expose=[]: 开放一个端口或一组端口；
--volume , -v: 绑定一个卷
```

![image-20201224161800030](https://gitee.com/leidl97/picture/raw/master/img/20201224161800.png)

```bash
docker ps
• -a :显示所有的容器，包括未运行的。
• -f :根据条件过滤显示的内容。
• --format :指定返回值的模板文件。
• -l :显示最近创建的容器。
• -n :列出最近创建的n个容器。
• --no-trunc :不截断输出。
• -q :静默模式，只显示容器编号。
• -s :显示总的文件大小。

退出容器
exit容器停止推出
ctrl+P+Q容器不停止退出
```

![image-20201224161819178](https://gitee.com/leidl97/picture/raw/master/img/20201224161829.png)

```bash
docker stop xxx 停止xxx容器
docker kill xxx 强制停止，瞬间杀死。stop有过程

docker rm 删除已经停止的容器
下图为批量删除
```

![image-20201224161828974](https://gitee.com/leidl97/picture/raw/master/img/20201224161829.png)

### 重要容器命令

1、以守护命令容器启动

```bash
docker run centos
docker run -d centos 
```

后台运行，必须有一个前台进程，容器的命令如果不是一直挂起的命令比如（top，tail），会自动退出

这是docker的机制问题，没有前台运行的应用就会自杀，因为它觉得没事可做

所以，最佳的解决方案是将你要运行的程序以前台进程的形式进行

```bash
docker run -d centos /bin/sh -c "while true;do echo hello dali;sleep 2;done"
```

2、查看docker日志

```bash
docker logs [OPTIONS] CONTAINER
• -f : 跟踪日志输出
• --since :显示某个开始时间的所有日志
• -t : 显示时间戳
--tail :仅列出最新N条容器日志
```

![image-20201224161956805](https://gitee.com/leidl97/picture/raw/master/img/20201224161956.png)

3、查看容器中运行的进程信息

```bash
docker top [OPTIONS] CONTAINER [ps OPTIONS]
```

4、查看容器内部细节

```bash
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
• -f :指定返回值的模板文件。
• -s :显示总的文件大小。
• --type :为指定类型返回JSON。
```

5、在运行的容器中执行命令（隔山打牛）

在外部操纵容器

```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
docker cp e954786a385b:/tmp/111.txt /root
• -d :分离模式: 在后台运行
• -i :即使没有附加也保持STDIN 打开
-t :分配一个伪终端
```

![image-20201224162045430](https://gitee.com/leidl97/picture/raw/master/img/20201224162045.png)

docker attach :连接到正在运行中的容器。

![image-20201224162113723](https://gitee.com/leidl97/picture/raw/master/img/20201224162610.png)

docker build 命令用于使用 Dockerfile 创建镜像。

**-f :**指定要使用的Dockerfile路径

**--tag, -t:** 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。

docker build -f /mydocker/Dockerfile -t dali/centos .



# docker常用软件安装

### 总体步骤

1、搜索镜像

2、拉取镜像

3、查看镜像

4、启动镜像

5、停止容器

6、移除容器

 

### 安装tomcat

docker hub 查找 tomcat

docker hub拉取tomcat镜像到本地

docker images 查看是否拉取到

使用tomcat镜像创建容器

 

### 安装mysql

docker pull mysql:5.7

![image-20201224162209751](https://gitee.com/leidl97/picture/raw/master/img/20201224162550.png)

构建容器

```bash
docker run -p 12345:3306 --name mysql -v /mysql/conf:/etc/mysql/conf.d -v /mysql/logs:/logs -v /mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --privileged=true -d mysql:5.7 
```

输入用户名密码验证mysql

将容器中的mysql数据导入宿主机中

rm删除容器

rmi删除镜像



### redis

![image-20201224162300717](https://gitee.com/leidl97/picture/raw/master/img/20201224162300.png)



### 将镜像传到阿里云

```bash
docker commit -a dali -m "new centos 1.2 from 1.1"  71079f792507 mycentos:1.2
要输入下图的container id，而不是image id
```

![image-20201224162335847](https://gitee.com/leidl97/picture/raw/master/img/20201224162335.png)

![image-20201224162355881](https://gitee.com/leidl97/picture/raw/master/img/20201224162355.png)

创建镜像仓库 

推送到阿里云

![image-20201224162401047](https://gitee.com/leidl97/picture/raw/master/img/20201224162401.png)

验证用户名

```bash
docker login --username= registry.cn-hangzhou.aliyuncs.com/leidl/mycentos
```

版本标签的处理，相当于备份发送

```bash
docker tag c89df15ddc13 registry.cn-hangzhou.aliyuncs.com/leidl/mycentos:1.2
```

推到阿里云上

```bash
docker push registry.cn-hangzhou.aliyuncs.com/leidl/mycentos:1.2
```

![image-20201224162516557](https://gitee.com/leidl97/picture/raw/master/img/20201224162516.png)

id与推上去的id相同表示成功
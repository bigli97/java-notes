# 相关概念

#### 是什么

远端仓库，可以多人编辑代码，进行版本控制

# 安装（2.27.0）

安装网址https://git-scm.com/download

1、点击next

![image-20201223191405738](https://gitee.com/leidl97/picture/raw/master/img/20201223191405.png)

2、选择合适的路径

![image-20201223191428077](https://gitee.com/leidl97/picture/raw/master/img/20201223191428.png)

3、默认配置就可以

![image-20201223191448987](https://gitee.com/leidl97/picture/raw/master/img/20201223191449.png)

4、点击next

![image-20201223191508430](https://gitee.com/leidl97/picture/raw/master/img/20201223191508.png)

5、选择默认，点击next

![image-20201223191524403](https://gitee.com/leidl97/picture/raw/master/img/20201223191524.png)

6、配置环境变量，使用默认

第二种配置是“从命令行以及第三方软件进行Git”。该选项被认为是安全的，因为它仅向PATH添加了一些最小的Git包装器，以避免使用可选的Unix工具造成环境混乱。

您将能够从Git Bash，命令提示符和Windows PowerShell以及在PATH中寻找Git的任何第三方软件中使用Git。这也是推荐的选项。

![image-20201223191537046](https://gitee.com/leidl97/picture/raw/master/img/20201223191537.png)

7、SSL证书，选择默认点击next

![image-20201223191547660](https://gitee.com/leidl97/picture/raw/master/img/20201223191547.png)

8、配置行结束符，选择默认

![image-20201223191600499](https://gitee.com/leidl97/picture/raw/master/img/20201223191600.png)

9、配置终端模拟器，选择默认

![image-20201223191619709](https://gitee.com/leidl97/picture/raw/master/img/20201223191619.png)

10、选择默认配置

![image-20201223191636560](https://gitee.com/leidl97/picture/raw/master/img/20201223191636.png)

11、额外的配置，使用默认

![image-20201223191701020](https://gitee.com/leidl97/picture/raw/master/img/20201223191701.png)

12、配置实验选项，默认不勾选

![image-20201223191712884](https://gitee.com/leidl97/picture/raw/master/img/20201223191712.png)

13、等待完成

![image-20201223191727303](https://gitee.com/leidl97/picture/raw/master/img/20201223191727.png)

14、在桌面鼠标右键点击Git Bash Here，显示此页面则安装成功

![image-20201223191743683](https://gitee.com/leidl97/picture/raw/master/img/20201223191743.png)



# 在eclipse中的使用

1、help->eclipse marketplace

搜索egit，进行安装

![image-20201223191851471](https://gitee.com/leidl97/picture/raw/master/img/20201223191851.png)

2、help-install

输入http://download.eclipse.org/egit/updates/

![image-20201223191911389](https://gitee.com/leidl97/picture/raw/master/img/20201223191911.png)

3、全选

![image-20201223191954837](https://gitee.com/leidl97/picture/raw/master/img/20201223191954.png)

4、点击next

![image-20201223192103748](https://gitee.com/leidl97/picture/raw/master/img/20201223192103.png)

5、windows->perferences

Team->git->Configuration

名字的key ：user.name ； value：是你的github用户名。

邮箱的key：user.email ; value:你的登陆GitHub邮箱账号.

![image-20201223192122589](https://gitee.com/leidl97/picture/raw/master/img/20201223192122.png)

接下来git一个项目

1、file->import，选择如图

![image-20201223192153864](https://gitee.com/leidl97/picture/raw/master/img/20201223192153.png)

 

2、选择如图

![image-20201223192211684](https://gitee.com/leidl97/picture/raw/master/img/20201223192211.png)

3、将信息填好下一步

![image-20201223192227611](https://gitee.com/leidl97/picture/raw/master/img/20201223192227.png)

4、选默认

![image-20201223192244157](https://gitee.com/leidl97/picture/raw/master/img/20201223192244.png)

5、选目录

![image-20201223192306548](https://gitee.com/leidl97/picture/raw/master/img/20201223192306.png)

6、选第三个

![image-20201223192332089](https://gitee.com/leidl97/picture/raw/master/img/20201223192332.png)

7、finished

![image-20201223192357198](https://gitee.com/leidl97/picture/raw/master/img/20201223192357.png)

8、成功如图

![image-20201223192408409](https://gitee.com/leidl97/picture/raw/master/img/20201223192408.png)

# 将本地项目上传

利用tortoisegit上传

1、生成ssh key

![image-20201223192444307](https://gitee.com/leidl97/picture/raw/master/img/20201223192444.png)

2、生成ssh密钥

ssh-keygen -t rsa -b 4096 -C "bigli971010@gmail.com"

按3次回车

![image-20201223192535034](https://gitee.com/leidl97/picture/raw/master/img/20201223192535.png)

在默认路径生成

/c/Users/you/.ssh/id_rsa

![image-20201223192547517](https://gitee.com/leidl97/picture/raw/master/img/20201223192547.png)

3、随便拿一个文档编辑器打开**id****_rsa.pub**，记事本也行，粘入github中

![image-20201223192602665](https://gitee.com/leidl97/picture/raw/master/img/20201223192602.png)

成功截图如下

![image-20201223192629901](https://gitee.com/leidl97/picture/raw/master/img/20201223192629.png)

4、创建一个仓库

![image-20201223192658002](https://gitee.com/leidl97/picture/raw/master/img/20201223192658.png)

5、点进去，复制ssh的内容

![image-20201223192719248](https://gitee.com/leidl97/picture/raw/master/img/20201223192719.png)

6、将你要上传的项目文件夹，点击右键，选择GIT Create respository here

![image-20201223192740055](https://gitee.com/leidl97/picture/raw/master/img/20201223192740.png)

7、此时，再点击项目文件夹右键就会出现git commit--->master，点击进入

![image-20201223192800045](https://gitee.com/leidl97/picture/raw/master/img/20201223192800.png)

8、点击Manage

![image-20201223192823356](https://gitee.com/leidl97/picture/raw/master/img/20201223192823.png)

![image-20201223192835552](https://gitee.com/leidl97/picture/raw/master/img/20201223192835.png)

9、填完点击确定

![image-20201223192856748](https://gitee.com/leidl97/picture/raw/master/img/20201223192856.png)

如果出现这个，是因为本地文件将会被覆盖，请先提交。commit 提示的文件， 就可以成功push了。还是不行就一直点左下角。然后在push一次，这代表你需要解决冲突。

![image-20201223192916843](https://gitee.com/leidl97/picture/raw/master/img/20201223192916.png)

成功截图

![image-20201223192939627](https://gitee.com/leidl97/picture/raw/master/img/20201223192939.png)

github仓库查看

![image-20201223193009021](https://gitee.com/leidl97/picture/raw/master/img/20201223193009.png)

# 公用git配置

如果想同时配置github和gitee，改怎么配置？

1、生成具体的密钥

```bash
ssh-keygen -t rsa -C "xxx@xxx.xxx" -f ~/.ssh/id_rsa_gitee 

ssh-keygen -t rsa -C "yyy@yyy.yyy" -f ~/.ssh/id_rsa_github
```

- -t 用来指定加密算法为 rsa

- -C 后面是个注释信息,并不一定要和你 Git 账户的邮箱或者 Git 账户名保持一致,随便写什么都行

- -f 密钥生成目录及文件名

 

2、添加私钥.因为Git会默认只读取到id_rsa,为了让SSH识别新的私钥，需将其添加到SSH agent中

```bash
ssh-agent bash

ssh-add ~/.ssh/id_rsa_gitee 

ssh-add ~/.ssh/id_rsa_github
```

![image-20201223193505404](https://gitee.com/leidl97/picture/raw/master/img/20201223193505.png)

3、编辑config文件( windows目录 /C/Users/username/.ssh ),没有这个文件的话新建一个

```xml
# gitee

Host gitee 

  HostName gitee.com 

  PreferredAuthentications publickey 

  IdentityFile ~/.ssh/id_rsa

  user git

# github

Host github

  HostName github.com 

  PreferredAuthentications publickey 

  IdentityFile ~/.ssh/github-rsa 

  user git
# 配置文件参数

# Host：对识别的模式，进行配置对应的的主机名和ssh文件

# HostName：登录主机的主机名

# PreferredAuthentications：设置登录方式，publickey公钥，改成password则要输密码

# IdentityFile：私钥全路径名
```

 

4、将生成的ssh复制到具体的gitee和github中

![image-20201223193647219](https://gitee.com/leidl97/picture/raw/master/img/20201223193647.png)

5、测试是否成功

ssh -T git@gitee.com 

ssh -T git@github.com

![image-20201223193701016](https://gitee.com/leidl97/picture/raw/master/img/20201223193701.png)
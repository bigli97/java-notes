![image-20201223155040038](https://gitee.com/leidl97/picture/raw/master/img/20201223155040.png)

#### 1.安装node.js

https://nodejs.org/en/download/

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152324.png" alt="image-20201223144158108" style="zoom: 50%;" />

#### 2、安装picgo

https://github.com/Molunerfinn/PicGo/releases

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223153316.png" alt="image-20201223153316816" style="zoom:50%;" />

#### 3、安装gitee插件（搜索gitee安装即可）

![image-20201223153342823](https://gitee.com/leidl97/picture/raw/master/img/20201223153342.png)

###### tip

搜不到怎么办（原因未知）？

手动安装，打开cmd进入C:\Users\你的用户名\AppData\Roaming\PicGo这个目录下

输入`npm install picgo-plugin-gitee-uploader`命令 ，然后重启picgo即可

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152348.png" alt="image-20201223145021228" style="zoom:50%;" />

#### 4、创建一个仓库（可以参照图片）

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152352.png" alt="image-20201223151302976" style="zoom:50%;" />

#### 5、获取token

右上角点头像选择设置左侧栏有一个私人令牌

![image-20201223153419990](https://gitee.com/leidl97/picture/raw/master/img/20201223153420.png)

点击生成新令牌（默认提交就可以），记得保存，token只会显示一次

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223153426.png" alt="image-20201223153426105" style="zoom: 80%;" />

#### 6、进行picGo的设置（按照如图设置）

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223153436.png" alt="image-20201223153436825" style="zoom:50%;" />

下图为github的。使用cdn加速（https://cdn.jsdelivr.net/gh/【用户名】/【仓库名】）

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152430.png" alt="image-20201223151424355" style="zoom:50%;" />

#### 7、上传图片测试

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152434.png" alt="image-20201223151455723" style="zoom:50%;" />

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152437.png" alt="image-20201223151502797" style="zoom:50%;" />

#### 失败的话看这里

大部分原因是因为网络端口设置问题，按照下列步骤依次排查

1. 首先检查你的互联网状态

2. 检查你的server开启了没有

3. 重启应用

4. 看报错日志或者更新下软件

   <img src="https://gitee.com/leidl97/picture/raw/master/img/20201223153540.png" alt="image-20201223153540565" style="zoom:50%;" />

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152450.png" alt="image-20201223151650010" style="zoom:50%;" />

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152454.png" alt="image-20201223151715609" style="zoom:50%;" />

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152500.png" alt="image-20201223151724986" style="zoom:50%;" />

#### 关于代理模式

picgo只支持简单代理模式，只可以写一个代理服务器地址，需要输入用户名密码的就不能用了

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152515.png" alt="image-20201223152018298" style="zoom:50%;" />

<img src="https://gitee.com/leidl97/picture/raw/master/img/20201223152507.png" alt="image-20201223152030433" style="zoom:50%;" />
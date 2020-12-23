#### 定义

HTML，超文本标记语言，写给浏览器的语言，也是网上最广泛的语言，最新版本为HTML5

HTML标签元素

一对开始一对结束<></>。推荐小写

规范的HTML页面

**1：文档声明**

在<html>之前，要写文档声明语句<!DOCTYPE HTML>，也    可以写成小写。作用:告诉浏览器该文档遵循html规范。

**2：标题**

<t**i**tle>标题内容</title>。作用：使用户看起来更友好

**3：页面编码**

通常选用**UTF-8**。设置网页编码的语句是<meta     charset=”utf-8”>,在<head></head>中定义

**4：常用标签及属性**

1:换行符<br>

2:分割线<hr>

3:段落符<p></p>

可以通过align属性进行设施，有三种对齐方式，左对齐    （left），右对齐（right），居中对齐（center）

4：标题<h1></h1>...<h6></h6>

h1-h6，数字越大，标题越小

5：文本格式化

粗体：<b/>或者<strong/>

斜体：<i/>或者<em/>

删除线:<s/>或者<del/>

下划线:<u/>

6:超链接<a/>

属性href，作用指明超链接要链接到的网址。

属性target表示链接的打开方式，属性值包括_blank(空    白页)，_self(当前页)，_top(当前页)

格式：<a href="#" target="_blank">百度一下</a> 

7：锚点

作用：使用户跳到文档的某个部分

操作方式：1 ： 在 页 面 中 的 某 个 位 置 设 置 锚 点  <a name='top" id="top"></a>  2 ： 通 过 超 链 接 跳 转 到 锚 点 的 某 个 位 置  <a href="#top"> 回 到 顶 部 </a> 

9:地址

当我们放入一个文件的时候，要写明他的地址

当前目录：直接写文件名。比如该图，即”李白凤求凰.jpg    “所在的目录。

在img.html中引入李白这张图片，那么就直接写就行。

![image-20201223194451199](https://gitee.com/leidl97/picture/raw/master/img/20201223194451.png)

../回到上一层目录。在这张图中../指的就是web文件    夹。

../day02。则进入上一层目录的web下的day02文件夹中。

如果进入day0101文件夹的xxx.jpg，那么为    day0101/xxx.jpg。

10：列表

列表分为无序列表和有序列表

无序列表：<ul><li/><ul>

属性：type。

值：disc实心圆，cirle空心圆，square实心方

有序列表：<ol><li/></ol>

属性：type。值：1，A,a，i,I

Start 排序的起点数值

11:特殊字符及表示方法

<(<),>(>),空格（&nbsp）

12:图像地图(图像热区)<map>

我们点击图片中的某个部分，就会链接到别的地方

第一步：先定义<map>

<map name="map名字" id="map名字">（浏览器解析方式    不同，可能找的值也不同，写两个）

第二步：定义图片中可操作区域

<area alt="圆形" shape="circle" coords="坐标X,坐    标Y,半径R" href="资源路径">

<area alt="矩形" shape="rect" coords="左上角X,左    上角Y,右下角X,右下角Y" href="资源路径">

第三步：定义<img>

<img alt="图片名字" src="图片路径" width="图片大小        " usemap="#map名字">

13：表格<table>

由多个tr，th，td组成，表格格式如下

以下为几个常用的表格标签

<caption>    定义表格标题

<tr>   定义行

<td>   定义一个单元格

<th>   定义表格表头

<thead>  定义表格的表头

<tbody>   定义表格主体

<tfoot>  定义表格的表注（底部）

通常很少使用<tbody>、<thead>、<tfoot>标签，因为浏览器对它们的支持不好。

常用属性

| 属性        | 值     | 说明                 |
| ----------- | ------ | -------------------- |
| width       | px、 % | 指定表格的宽度       |
| height      | px、%  | 表格的高度           |
| border      | px     | 指定表格边框的宽度   |
| cellspacing | px、 % | 指定单元格之间的距离 |

14：表单<form>

常用属性

action:表单提交的地址

method：提交的方式(get在url中显示，post消息正文    中)

文本域

<input type="text"> 

密码域

<input type="password">

按钮分为三种：

普通按钮<input type=”button”>

提交按钮<input type=”submit”>

重置按钮<input type=”reset”>

 

上传文件

利用这项功能，在form标签中要指定method属性，要把    method指定为post

文本域<textarea>

属性:name元素的名称

rows 指定的文本框高度

cols 指文本框的宽度
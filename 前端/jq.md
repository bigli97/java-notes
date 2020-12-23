

# 基础知识

#### 一：什么是jQuery

它是js的一个轻量级框架，它是开源的项目。jquery目前是比较流行的一个JQuery框架。（核心理念：write less，do more）。

 

#### 二：jQuery的特点

- 简化js代码
- 可以像css一样获取元素
- 可以直接修改页面样式
- 解决兼容性问题
- 强大的选择器
- 出色的DOM封装
- 可靠的事件处理机制



#### 三：jQuery的引入

<script type="text/javascript" src="jq所在的路径"></script>



#### 四：jQuery$的介绍

​    $是 jQuery的简写 $("#div")=jQuery("#div")



#### 五：jQuery对象和js对象的转换

​    把js对象转成jq对象

​    var jq = $(minput);

​    将jq对象转成js对象

​    第一种

​    var js1 = $jq[0];

​    第二种

​    var js2 = $jq.get(0);



#### 六：jQuery选择器(记住自己顺手的就行)

1：基本选择器（重要）

> ​    标签名选择器
>
> ​    $("标签名")
>
> ​    id选择器
>
> ​    $("#id")
>
> ​    类选择器
>
> ​    $(".class")
>
> ​    分组选择器
>
> ​    $("a,div,p")
>
> ​    所有元素
>
> ​    $("*")

2：层级选择器

> ​    $("div span") 匹配div下所有的span
>
> ​    $("div>span") 匹配div下所有子元素span
>
> ​    $("div+span") 匹配div后面紧邻的span兄弟元素
>
> ​    $("div~span") 匹配div后面所有的span兄弟元素
>
> ​    1.获取所有兄弟元素
>
> ​    $("#two~div").siblings("div")
>
> ​    2.获取元素哥哥元素
>
> ​    $("#two~div").prev("div")
>
> ​    3.获取元素的哥哥们元素
>
> ​    $("#two~div").prevAll("div")
>
> ​    4.获取元素弟弟元素
>
> ​    $("#two~div").next("div")
>
> ​    5.获取元素的弟弟们元素
>
> ​    $("#two~div").nextAll("div")

3：过滤选择器

> ​    $("div:first") 匹配所有div中的第一个元素
>
> ​    $("div:last") 匹配所有div中的最后一个元素
>
> ​    $("div:even") 匹配所有div中偶数div 从0开始
>
> ​    $("div:odd") 匹配所有的奇数 从0开始
>
> ​    $("div:eq(n)") 匹配所有div中下标为n元素从0开始
>
> ​    $("div:lt(n)") 匹配所有div中下标小于n元素 从0开始
>
> ​    $("div:gt(n)") 匹配所有div中下标大于n元素 从0开始
>
> ​    $("div:not(.one)) 匹配所有div中 class不为one的元素

4：内容选择器

> ​    $("div:has(p)") 匹配所有包含p标签的div
>
> ​    $("div:empty") 匹配所有空的div
>
> ​    $("div:parent") 匹配所有非空的div
>
> ​    $("div:contains('id')") 匹配包含文本内容id的div

5：可见选择器

> ​    $("div:hidden") 匹配所有隐藏的div元素
>
> ​    $("div:visible") 匹配所有可见的div元素

6：属性选择器

> $("div[id]") 匹配有id属性的div元素
>
> $("div[id='d1']") 匹配id属性等于d1的div
>
> $("div[id!='di']") 匹配id属性不等于d1的div

7：子元素选择器

> $("div:nth-child(n)") n从1开始,匹配div中第n个子元素
>
> $("div:first-child") n从1开始,匹配div中第1个子元素
>
> $("div:last-child") n从1开始,匹配div中最后一个子元素

8：表单选择器(注意前面加:)

> $(":input")匹配所有的input 文本框 密码框 单选 复选框 下拉选文本域 button
>
> $(":password") 匹配所有密码框
>
> $(":selected") 匹配所有被选中的下拉选option
>
> $(":checked") 匹配所有被选中的 单选/复选/下拉选
>
> $("input:checked") 匹配所有被选中的单选和复选

 

#### 七：jQuery样式操作

获取div的样式

$('div').css('width');

设置div的样式

$('div').css({width:'30px',color:'red'});

 

#### 八：DOM操作

​    创建元素

​        var $d = $("<div>abc</div>");

​    添加元素

​        $("#big").append($d)//最后面

​        $("#big").prepend($d)//最前面

​    插入元素

​        兄弟元素.after($d)//插入到兄弟元素的后面

​        兄弟元素.before($d)//插入到兄弟元素的前面

​    删除元素

​        通过自己删除 $("#id").remove();

​        通过上级删除$("#big").remove($d);$("#big").remove("#id");

​    修改样式

​        $("#id").css("width","40px")//设置宽度

​        $("#id").css("width")//获取高度

​    属性

​        $("#id").attr("id","xx")//设置id属性

​        $("#id").attr("id")//获取id

​    文本

​        $("#id").text("xxx")//设置元素文本

​        $("#id").text()//获取文本

​    html

​        $("#id").html("xxx")//设置元素html代码

​        $("#id").html()//获取html代码

 

#### 九：层级函数

得到上级父元素 

元素.parent()     

得到所有的子元素 

元素.children()

​    获取第n个子元素 

元素.children().eq(n)

​    得到查询的第1个 

$("div").eq(0)

​    查询M元素中id为aa的子元素 

M元素.children("#aa")

 

#### 十：删除元素

remove():当某个节点用remove()方法删除后，该节点所包含的所有后代节点将同时被删除。这个方法的返回值是一个指向已被删除的节点的引用，因此可以在以后再使用这些元素。

detach():detach()和remove()一样，也是从DOM中去掉所有匹配的元素。但需要注意的是，这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。与remove()不同的是，所有绑定的事件、附加的数据都会保留下来。

empty():严格地讲，empty()方法并不是删除节点，而是清空节点，它能清空元素中的所有后代节点。



#### 十一：获取，设置文本和内容和值的方法

1)html()

 2)text()

 3)val()

 

#### 十二：常用的合成事件

1)hover(函数1，函数2)

 当鼠标移入的时候执行函数1，移出的时候执行函数2

2)toggle()--切换（隐藏的时候显示，显示的时候隐藏）

 

#### 十三：事件对象

事件对象能干什么

通过事件对象可以阻止冒泡型事件、阻止元素的默认行为，获得鼠标的坐标、

获得键盘按键所对应的字符或码值、获得鼠标的按键类型、获得事件源、    获得事件类型

事件对象举例

>  1)event.type属性--获取事件类型
>
>  2)event.preventDefault();--阻止元素的默认行为
>
>  3)event.stopPropagation();--阻止冒泡型事件
>
>  4)event.target---获取事件源,返回的是DOM对象
>
>  5)event.pageX和event.pageY 获得鼠标的X,Y坐标
>
>  6)return false，既阻止元素的默认行为，又阻止冒泡型事件
>
>  7)event.which,获得鼠标键 1，左，2中 3右
>
>  8)event.keycode

 

#### 十四：模拟事件

trigger("事件名")

比如说模拟点击事件

$("input").trigger("click");

 

#### 十五：jQuery动画

第一对：显示隐藏

show(毫秒)

hide(毫秒)

toggle()--在show()和hide()之间进行切换，自动判断隐藏还是显示

第二对：淡入和淡出

fadeIn(毫秒)

fadeOut(毫秒)

第三对：上滑和下滑

slideDown(毫秒)

slideUp(毫秒)

slideToggle()--收起的时候显示，显示的时候收起

自定义动画

$(this).animate({"width":"300px","height":"250px"},1000)

$(this).animate({"width":"200px","height":"150px"},1000)

 

#### 十六：jQuery事件

>  blur() 元素失去焦点
>
>  focus() 元素获得焦点
>
>  change() 表单元素的值发生变化
>
>  click() 鼠标单击
>
>  dblclick() 鼠标双击
>
>  mouseover() 鼠标进入（进入子元素也触发）
>
>  mouseout() 鼠标离开（离开子元素也触发）
>
>  mouseenter() 鼠标进入（进入子元素不触发）
>
>  mouseleave() 鼠标离开（离开子元素不触发）
>
>  hover() 同时为mouseenter和mouseleave事件指定处理函数
>
>  mouseup() 松开鼠标
>
>  mousedown() 按下鼠标
>
>  mousemove() 鼠标在元素内部移动
>
>  keydown() 按下键盘
>
>  keypress() 按下键盘
>
>  keyup() 松开键盘
>
>  load() 元素加载完毕
>
>  ready() DOM加载完成
>
>  resize() 浏览器窗口的大小发生改变
>
>  scroll() 滚动条的位置发生变化
>
>  select() 用户选中文本框中的内容
>
>  submit() 用户递交表单
>
>  toggle() 根据鼠标点击的次数，依次运行多个函数
>
>  unload() 用户离开页面



# 进阶知识

#### 一：$(function(){})和window.onload=function(){}区别?

- 加载时机不同，$(function(){})优先于window.onload=function(){}先执    行
- 执行的次数不同，window.onload=function(){}只会执行最后一个绑定的函    数。$(function(){})可以绑定多个函数来分别执行。
-  jQuery--文档树加载完成的时候执行，不会等待突破资源的加载完成
- js--文档树加载完成后，必须等待所有的关联资源（包括图像、音频）加载    完毕后才会调用。
- 需要注意的是：如果同时使用的话先执行onload



#### 二：jQuery对象和DOM对象之间的关系，他们之间如何去转换？

##### 什么是DOM对象？

使用JavaScript中的方法获取页面中的元素返回的对象就是dom对象。比如使用document.getElement*系列的方法返回的就是dom对象。

##### 什么是jQuery对象？

元素返回的对象就是jQuery对象。比如使用$()方法返回对象都是jquery对象。

##### 关系

使用DOM方法来查询得到的结果是DOM对象，它只能访问DOM对象中所提供的属性和方法。

使用jQuery查询得到的结果是jQuery对象，它只能访问jQuery对象中所提供的属性和方法。

也就是说jQuery对象不能调用DOM对象的属性和方法，反之也一样。

##### 转换(基础总结中也提到)

jQuery查询出来的结果是一个对象数组，就算根据id来进行查询就是返回的是一个对象数组。数组内存放的是DOM对象。

jQuery对象转换成DOM对象，通过[下标]就可以转，或者get(下标)方法来取出数据就是DOM对象

DOM对象转换成jQuery对象，通过$()工厂函数就可以了，把DOM对象作为参数传给$()函数，那么就把DOM对象转换成了jQuery对象。

##### 错误的用法

$("div").innerHTML;//jquery对象不能调用dom方法

document.getElementById("btnShowDiv").show();//dom对象不能调用jquery方法。



#### 三：关于Boolean中为真假的问题(补充)

真:非0的数字,字符串,true,函数,object,[],{}都是真的

假:就记住6个为假其余都真 0,NaN,空字符串,null,false,undefined

 

#### 四：jQuery的$问题

$有什么作用？

其实美元符号$只是”jQuery”的别名，它是jQuery的选择器

 

##### 如果JavaScript的框架中也使用$作为简写怎么办

其中某些框架也使用 $ 符号作为简写（就像 jQuery），如果您在用的两种不同的框架正在使用相同的简写符号，有可能导致脚本停止运行，所以jQuery 的团队考虑到了这个问题，并实现了 noConflict() 方法。

 

##### jQuery noConflict() 方法

noConflict() 方法会释放会 $ 标识符的控制，这样其他脚本就可以使用它了。

1:使用$.noConflict()释放后仍然可以通过全名替代简写的方式来使用 jQuery

2:也可以创建自己的简写，只需要返回一个变量就可以使用，比如说

```js
var jq = $.noConflict();

jq(document).ready(function(){

 jq("button").click(function(){

  jq("p").text("jQuery 仍在运行！");

 });

});
```

3:如果还想在jQuery 代码块使用 $ 简写，那么可以把 $ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 $ 符号了 - 而在函数外，依旧不得不使用 "jQuery"：

```js
$.noConflict();

jQuery(document).ready(function($){

 $("button").click(function(){

  $("p").text("jQuery 仍在运行！");

 });

}); 
```

#### 五：jQuery中，get()和post()提交的区别

1：get()使用GET方法来进行异步提交.get()使用GET方法来进行异步提    交.post()使用POST方法来进行异步提交

2：get请求方式将参数跟在url后进行传递用户可见 post请求则是作为    http消息的实体内容发送给服务器，用户不可见

3：post传输数据比get大 

4：get请求的数据会被浏览器缓存 不安全

 

#### 六：radio单选组的第二个元素为当前选中的值，该怎么取

$('input[type=radio]')[1].checked=true

 

# 选择题

1:下面哪句话是正确的？b

·A:如需使用 jQuery，您必须在 [www.jquery.com](http://www.jquery.com) 购买 jQuery 库

·B:如需使用 jQuery，您能够引用 Google 的 jQuery 库

·C:如需使用 jQuery，您无需做任何事情。大多数浏览器都内建了 jQuery 库。

 

2:下面哪个 jQuery 函数用于在文档结束加载之前阻止代码运行？a

A:$(document).ready()

B:$(document).load()

C:$(body).onload()

 

3:哪个 jQuery 方法用于添加或删除被选元素的一个或多个类？a

A:toggleClass()

B:switchClass()

C:altClass()

D:switch()



4:$("div#intro .head") 选择器选取哪些元素？c

A:id="intro" 或 class="head" 的所有 div 元素

B:class="intro" 的任何 div 元素中的首个 id="head" 的元素

C:id="intro" 的首个 div 元素中的 class="head" 的所有元素

 

5:jQuery 是 W3C 标准吗？b

A:Yes

B:No

 

6:在JQuery中被誉为工厂函数的是(  )c

A.ready( )

B.function( )

C.$( )

D.next( )

 

7:腾讯QQ号从10000开始，目前最高位10位，(  )可以匹配QQ号 b

A:/^[1-9][0-9]{4,10}$/

B:/^[1-9][0-9]{4,9}$/

C:/^\d{5,10}$/

D:/^\d[5,10]$/

 

8:jQuery中，对于如下代码：

```html
<div class=”c”>
    <div style=”display:none;”>a</div>
    <div style=”display:none;”>b</div>
    <div style=”display:none;”>c</div>
    <div class="c" style=”display:none;”>d</div>
</div>
<div class=”c” style=”display:none;”>e</div>
<div class=”c” style=”display:none;”>f</div
```



使用如下jQuery选择器：

```js
var $x = $(“.c :hidden”);
var $y = $(“.c:hidden”);
var x_len = $x.length;
var y_len = $y.length;
```

执行以上代码，x_len和y_en两个变量的值分别是_ 和 _ (A)

A:4,3

B:3,4

C:7,3

D:3,7

 

9:下列(  )字符串不能匹配/^(.[a-zA-Z]{2,3}){1,2}$/正则表达式。d

A:.com

B:.com.cn

C:comcn

D:.com.cn.cn

 

10:在下列的选项中，不能够实现图片隐藏效果的是(  )B

A:$("img").hide(1000);

B:$("img").slideDown(1000);

C:$("img").fadeOut(1000);

D:$("img").slideUp(1000);

 

11:在jQuery中，下列关于$( )方法的说法错误的选项是(  )C

A:$(document)的含义是把document对象转换成jQuery对象

B:$( )方法可以用来选择元素、把DOM对象转换成jQuery对象，或根据HTML    字符串创建Jquery对象

C:$( )方法额返回值一定是一个jQuery对象

D:$( )方法的参数可以是选择器、DOM元素或HTML代码

 

12：在jQuery中，关于下面这段代码的效果描述正确的是(  )b

HTML代码：

```html
<button id="go">开始动画</button>
<div id="block">区域</div>
```

jQuery代码：

```js
	$(document).ready(function( ){
	$("#go").click(function( ){                   					$("#block").animate({width:"300"},				 {queue:false,duration:4000})
	               .animate({fontSize:'10em'},2000)
	               .animate({borderWidth:5},2000);
	});
	});
```

A.单击按钮之后，首先div的宽度变为300像素，然后变化字体，最后变化    边框的效果

B.单击按钮之后，在div的宽度变为300像素的同时也在变化字体，一旦字    体改变完毕后，边框的变化就开始

C.单击按钮之后，首先div的宽度变为300像素，然后同时变化字体和边框

D.单击按钮之后，首先变化字体，然后变化边框的效果，最后div的宽度先    变为300像素

 

13：当DOM加载完成后要执行的函数，下面哪个是正确的?( ) C

A、jQuery(expression, [context]) B、jQuery(html,[ownerDocument])

C、jQuery(callback)        D、jQuery(elements)

 

14：下面哪一个是用来追加到指定元素的末尾的?( )   C

insertAfter() B、append()  C、appendTo() D、after()

 

15：下面哪种不属于jquery的筛选? ( ) B

过滤        B、自动        C、查找        D、串联

 

16： 为每一个指定元素的指定事件(像click) 绑定一个事件处理器函数，下面哪个是用来实现该功能的? ( ) B

trgger (type)    B、bind(type)      C、one(type)       D、bind

 

17：在一个表单中，如果想要给输入框添加一个输入验证，可以用下面的哪个事件实现? ( D)

A、hover(over ,out) B、keypress (fn)    C、change()    D、change(fn)

 

18：以下 jquery 对象方法中，使用了事件委托的是(  ) D

bind            B. 、mousedown    C、change     D、on

19：元素的type属性的取值不可以是( ) C

image          B、checkbox    C、select       D、button

 

20:在jquery中指定一个类，如果存在就执行删除功能，如果不存在就执行添加功能，下面哪一个是可以直接完成该功能的？（） C

A、removeClass()             B、deleteClass()

C、toggleClass(class)        D.addClass()

 

21：在Jquery中，既可绑定两个或多个事件处理器函数，以响应被选元素的轮流的 click 事件，又可以切换元素可见状态的方法是(  )   B

hide( )          B. toggle( )         

hover( )         D.slideUp( )

 

22：以下jQuery代码运行后，对应的HTML代码变为( ) B

HTML代码：<p>你好</p>

jQuery代码：$(“p”).append(“<b>快乐编程</b>”);

A. <p>你好</p><b>快乐编程</b>

B. <p>你好<b>快乐编程</b></p>

C. <b>快乐编程</b><p>你好</p>

D. <p><b>快乐编程</b>你好</p>

 

# 填空题

1：jquery中$(this).get(0)的写法和_______是等价的    $(this)[0])

 

2:现有一个表格，如果想要匹配所有行数为偶数的，用_实现，奇数的用 ___实现even odd

 

3：在jquery 中,当鼠标指针悬停在被选元素上时要运行的两个方法，实现        该操作的是____ $(selector).hover(inFunction,outFunction)

 

4:在三个<ul>元素中,分别添加多个<li>元素,通过jQuery中的子元素选择        器,将这三个<ul>元素中的第一个<li>元素隐藏,代码是 ______

$("li:first-child").hide();

 

5: jQuery中的选择器大致分为:________ . ________ ._____ .___________

基本选择器，层次选择器，过滤选择器，表单选择器

 

6:______方法用于处理命名冲突 conflict()

 

# 简答题

1： jQuery的美元符号$有什么作用？

其实美元符号$只是”jQuery”的别名，它是jQuery的选择器

Html代码

$(document).ready(function(){});

当然你也可以用jQuery来代替$，如下代码：

Html代码

jQuery(document).ready(function(){});

jQuery中就是通过这个美元符号来实现各种灵活的DOM元素选择的，例    如    $(“#main”)即选中id    为main的元素。

 

2：window.onload()函数和jQuery中的document.ready()有什么区别？

1.执行时间

window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行。

$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。

$(document).ready()在 window.onload之前执行

2.编写个数不同

window.onload不能同时编写多个，如果有多个window.onload方法，只会执行一个

$(document).ready()可以同时编写多个，并且都可以得到执行

3.简化写法

window.onload没有简化写法

 $(document).ready(function(){})可以简写成$(function(){});

4.浏览器兼容性

$(document).ready()可以跨浏览器，例如在使用ajax请求的时候自动会处理兼容

5.出现地方不同

window.onload是js标准，可出现在任何js脚本中

$(document).ready只有在jq库中出现

 

3：jquery和js对象如何转化？

​    把js对象转成jq对象

​    var jq = $(minput);

​    将jq对象转成js对象

​    第一种

​    var js1 = $jq[0];

​    第二种

​    var js2 = $jq.get(0);

 

4：阐述一下js和jquery的关系？

1：jQuery是一个 js框架,封装了js的属性和方法。让用户使用起来    更加便利,并且增强了 js的功能.

2：使用原生 js是要处理很多兼容性的问题(注册事件等)，由jQuery    封装了底层，就不用处理兼容性问题

3：原生的js的dom和事件绑定和Ajax等操作非常麻烦，jQuery封装    以后操作非常方便。
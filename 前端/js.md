# 基础知识

#### 什么是JS

JavaScript。1995年发布：网景公司 livescript 同年更名为JavaScript。目的是与web页面交互，给页面添加行为。

#### JS的特点

JavaScript属于脚本语言，不需要编译，由浏览器解释执行

JavaScript可以插入到html页面中，由浏览器执行

JavaScript基于面向对象，弱类型语言

平台无关性：只与浏览器有关，与操作环境无关

安全性和简单性

#### JavaScript和Java的关系

就像雷峰塔和雷锋的关系，JavaScript借Java名气推广

##### 优点

交互性：可以直接与用户进行交互

安全性：只能在浏览器中进行，不允许访问本地磁盘中的其                他资源

##### 缺点

浏览器的兼容问题

不能打开和保存计算机上的文件

#### 在html页面中添加JS代码的方式

与css类似，3种，内联，内部，外部

引号嵌套

双引号中可以写单引号，单引号中可以写双引号，

双引号中    不可以嵌套双引号，单引号中不可以嵌套单引号

#### 变量的声明

因为js属于弱类型语言，所以声明变量的时候不需要指定变    量的类型，直接赋值即可，可以加var也可以不加（在函数    的外部，加不加var都是全局变量，在函数的内部，加var    是局部变量，不加var是全局变量，如果在外部和内部都定    义了一个相同的全局变量，那么在函数外部使用的话，函数    内部的全局变量会覆盖函数外部的全局变量）

#### 数据类型

Js有6种数据类型，包括5种基本数据类型    （Number,String,Boolean,Undefined,Null），和引用数据    类型，引用类型（Object，Array，Function，Date，RegExp）

**1：数值类型（number）**

在js中所有数值都是浮点型，在使用过程中会自动转换类型

NaN：不是一个数，和哪个数都不相等

**2：字符串类型（string）**

通过单引号或双引号修饰一个字符串，单引号和双引号不能    解析变量

**3：布尔类型（boolean）**

两个值true和false

**4：undefined**

此类型只有一个值undefined，变量声明但是没有定义的话，    类型和值同为undefined

**5：null**

此类型也只有一个值，值为null

需要注意的是数组类型和null返回的数据类型都是object

#### undefined和null深层探究

他们都表示的是一个无效的值，因此在JS中对他俩调用toString（）方法时会得到异常的结果

null字面意思就是空值，他表示一个对象被人为的重置为空    对象，而非一个变量原始的状态。在内存中表示就是，栈中    的变量没有指向堆中的对象

![image-20201223195306518](https://gitee.com/leidl97/picture/raw/master/img/20201223195306.png)

当一个对象被赋值为null后，原来的对象在内存中就变为了    游离状态，GC会择机回收对象并释放内存，因此，如果需要    释放某个对象，就将变量设置为null，即表示该对象已经被清空

##### typeof(null)为object的原因：

null有属于自己的类型Null，而不属于Object类型，typeof    之所以会判定为object类型是因为JavaScript数据类型在底层都是以二进制的形式表示的，二进制前三位为0会被typeof判断为对象类型，而null二进制恰好都是0，因此，null被误判为object类型

undefined的字面意思就是未定义的意思，就是此处应该有一个值，但是还没有定义，它表示一个变量最原始的状态，而非人操作的结果,会在下面四种场景中出现

1：变量被声明了，但没有赋值时，就等于undefined

2：调用函数时，应该提供的参数没有提供，该参数等于undefined

3：对象没有赋值的属性，该属性的值为undefined

4：函数没有返回值（void）时，默认返回undefined

比如下面的例子

```js
var i;//i undefined

function f(x){}//f() undefined

var o = new Object()//o.属性 undefined

void function f(){}//undefined
```

数据类型之间的转换

在这个表中null转数值为0并非NaN!!!

![image-20201223195955644](https://gitee.com/leidl97/picture/raw/master/img/20201223195955.png)

函数转换：parseInt（），parseFloat（），toString（）

强类型转换：Boolean（），Number（），String（）

弱类型转换：== ,- ,+ ,if()

parseInt（）转换规则

1：忽略字符串前面的空格，直到找到第一个非空格字符

2：如果第一个字符不是数字或者负号，返回NaN

3：空字符串返回NaN

4：如果第一个是数字，会继续解析第二个字符，直到解析完所有后续字符或者遇到了第一个非数字字符

5：如果字符串以0x开头且后面跟数字字符，就会将其转换为16进制数，同样八进制（以数字0开头）也一样

 

#### 运算符==和===

== 比较规则

在转换不同数据类型时，遵循以下基本规则：

1：如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值（false为0，true为1）

2：如果有一个操作数是字符串，另一个操作数是数值，在比较相等性之前先将字符串转换为数值

3：如果一个操作数为对象，另一个操作数不是，则调用对象的valueof和toString方法，用得到的基本类型值按照前面的规则比较

4：如果两个操作数都是对象，则比较他们是不是同一个对象，如果两个操作数都指向同一个对象，则返回true，否则返回false

下面时几个例子

1：[]==[]//false

两个对象，类型相同，不同的实例对象，所以为false

2：[]==![]//true

**类型不同**，左侧为对象，右侧为布尔，进行转化

[]转化为基本数据类型使用toString方法，值为””

![]为false，可以通过Boolean(![])验证，在转为数字

“”转数字为0，false转数字为0，所以相等

3：[]==true//false

不同类型，转换。[]同上转为””在转数字为0，true为1

所以为false

4：[]==false，同上所以为true

5：null==0//false

undefined == “undefined”//false

**在比较相等性之前null和undefined不会被转换为其他类型**

6：Function==Object//true

特别的：undefined==null为true，原因如下

ECMAScript认为undefined是从null派生出来的，所以把它们定义为相等的，还有一种类似的解释

ECMAScript 规范认为，既然 null 和 undefined 的行为很相似，并且都表示一个无效的值，那么它们所表示的内容也 具有相似性，即有 undefined==null//true

**不要试图通过转换数据类型来解释这个结论**，因为

null转为数值为0

undefined转为数值为NaN

如果这样的话他就为fasle

 

===比较规则

1:如果类型不同则一定不相等

2:undefined===null为false

3:如果两个值都引用一个对象或者函数，那么相等

**逗号运算符**

**取逗号的最后一个**

var a=10;

​    var b=20;

​    var c=30;

​    d=(a+b,b+c);

​    alert(d);

d为50

全局函数：不需要对象去调用的函数

**eval（“字符串”）**

使用eval可以实现不改变页面源码的情况下，添加或者修改    页面的逻辑

eval作用：可以执行字符串中的js代码

#### **定时器**

setInterval(参数1：函数，参数2：间隔时间单位为毫秒)

#### typeof

获取某个变量的类型

typeof可能出现的类型6种

Number/String/Boolean/Undefined/Object/Function

123              //Number

'abc'         //String

true                //Boolean

undefined      //Undefined

null            //Object

{ }          //Object

[ ]          //Object

函数名          //Function

 

#### delete

用于删除数组中的内容，数组长度不变，相当于把删除            的数变为了null

#### 语句

if中的条件会自动转换为布尔类型

for循环为 var i，不支持增强for循环

#### 函数

两种声明方法

1(常用)：function 名字(参数列表){}

2：var 名字 = function(参数列表){}

函数的参数

1：当实际参数个数大于形参个数，多余实际参数会被省略

2：当实际参数个数小于形参个数，形参多余的被定义为NaN

#### 事件

onclick 单击事件

onblclick 双击事件

oncontextmenu 鼠标右击

clientX 获取鼠标指针相对于浏览器的水平坐标

clientY 获取鼠标指针相对于浏览器的垂直坐标

onmouseover 鼠标移入事件

onmouseout 鼠标移出事件

onfocus 获得焦点事件

onblur 失去焦点事件

onchange 文本区发生改变的时候，产生的事件

onselect 当文本区被选中的时候，产生的事件

onreset 当点击form表单按钮的时候，产生的事件

#### Window

常用属性：

**1：onload 先加载页面，在执行js代码**

方法

**2：alert()警告框**

**3：confirm()确定框**

**4:prompt（）对话框**

**5:navigator(了解)**

返回浏览器的代码名字：navigator.appCodeName

返回浏览器的名称：navigator.appName

返回浏览器的版本：navigator.appVersion

返回浏览器的语言：navigator.language

返回浏览器的编译平台：navigator.platform

返回浏览器的用户表头：navigator.userAgent

**6：screen（记住）**

返回屏幕的高度：window.screen.height

返回屏幕的宽度：window.screen.width

返回屏幕的可用高度（不包括底部任务栏）：window.screen.availHeight

返回屏幕的可用宽度（不包括底部任务栏）： window.screen.availWidth

返回屏幕的可用高度（不包括任务栏）： window.screen.availHeight

返回文档的可用高度（不包括底部的任务栏）： window.innerHeight

返回文档的可用宽度（不包括任务栏）：window.innerWidth

**4：history**

go（1）前进

​    go（-1）后退

​    back（）后退

​    forward（）前进

**5：location（了解）**

返回的是一个完整URl地址：window.location.href

返回的是一个路径名：window.location.pathname

返回当前的URL协议：window.location.protocol

返回是端口号：window.location.port

返回的是主机名：window.location.hostname

DOM

定义了访问和操作html文档的标准方法

**如何访问节点：**

通过id查找

document.getElementById(字符串)

通过标签名查找，返回的是数组

document.getElementsByTagName(字符串)

通过name属性查找

document.getElementsByName(字符串);

**往元素中添加文本和添加html代码**

d.innerText=”<h1>aaa</h1>”

d.innerHTML=”<h1>aaa</h1>”

**往页面中添加一个新的div****5：location（了解）**

返回的是一个完整URl地址：window.location.href

返回的是一个路径名：window.location.pathname

返回当前的URL协议：window.location.protocol

返回是端口号：window.location.port

返回的是主机名：window.location.hostname

DOM

定义了访问和操作html文档的标准方法

**如何访问节点：**

通过id查找

document.getElementById(字符串)

通过标签名查找，返回的是数组

document.getElementsByTagName(字符串)

通过name属性查找

document.getElementsByName(字符串);

**往元素中添加文本和添加html代码**

d.innerText=”<h1>aaa</h1>”

d.innerHTML=”<h1>aaa</h1>”

**往页面中添加一个新的div**

DOM可以对html增删改

增：document.createElement(tag);

dom.appendChild(sondom);

dom.insertBefore(newdom,targetdom);

删：

 document.removeChild();

改：

 document.replaceChild();

查：

getElementById()

getElementsByTagName()

getElementsByName();

正则

使用正则的应用场景：1，查找内容2，校验文本

两种创建方式：

**直接创建，第一个代表正则，第二个代表模式**

模式：g 全局查找，i忽略大小写

![image-20201223195519935](https://gitee.com/leidl97/picture/raw/master/img/20201223195520.png)

**通过对象创建，第一个是正则表达式，第二个是模式**

**正则函数**

1：reg.exec(字符串)

如果添加g是全局查找，没有则返回null，如果不加g，则每次都是得到的第一个

2：reg.test(字符串)

判断目标字符串是否符合规则

**字符串的函数**

![image-20201223195535892](https://gitee.com/leidl97/picture/raw/master/img/20201223195607.png)

# 进阶知识

#### 一：闭包（说白了就是函数里套函数）

闭包就是一个函数引用另外一个函数的变量，因为变量被引用着所以不会被回收，因此可以用来封装一个私有变量。这是优点也是缺点，不必要的闭包只会徒增内存消耗！另外使用闭包也要注意变量的值是否符合你的要求，因为他就像一个静态私有变量一样。下面代码有陷阱

```js
function createFunctions(){

 var result = new Array();

 for (var i=0; i < 10; i++){

  result[i] = function(){

   return i;

  };

 }

 return result;

}

var funcs = createFunctions();

for (var i=0; i < funcs.length; i++){

 console.log(funcs[i]());

}
```

乍一看，以为输出 0-9 ，万万没想到输出10个10？

这里的陷阱就是：函数带()才是执行函数！ 单纯的一句 var f = function() { alert('Hi'); }; 是不会弹窗的，后面接一句 f(); 才会执行函数内部的代码。上面代码翻译一下就是：

```js
var result = new Array(), i;

result[0] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！

result[1] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！

...

result[9] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！

i = 10;

funcs = result;

result = null;

console.log(i); // funcs[0]()就是执行 return i 语句，就是返回10

console.log(i); // funcs[1]()就是执行 return i 语句，就是返回10

...
console.log(i); // funcs[9]()就是执行 return i 语句，就是返回10
```

为什么只垃圾回收了 result，但却不收了 i 呢？ 因为 i 还在被 function 引用着啊。好比一个餐厅，盘子总是有限的，所以服务员会去巡台回收空盘子，但还装着菜的盘子他怎么敢收？ 当然，你自己手动倒掉了盘子里面的菜（=null），那盘子就会被收走了，这就是所谓的内存回收机制。

至于 i 的值怎么还能保留，其实从文章开头一路读下来，这应该没有什么可以纠结的地方。盘子里面的菜，吃了一块不就应该少一块吗？

 

#### 二：JS中的常用DOM操作(都需要知道)

通过元素的ID名获取元素：getElementById();

通过元素的Class名获取元素集合：getElementsByClassName();

通过元素的标签名获取元素集合：getElementsByTagName();

通过元素的name名获取元素集合：getElementsByName();

创建一个元素节点：createElement();

放入元素节点：appendChild（）;

给元素设置属性：setAttribute(属性，属性值);

获取或设置HTML内容：innerHTML;

获取或设置元素的文本：innerText;

#### 三：正则需要知道的知识点

两种写法

I:var regexp=/....../模式

II:var regexp=new RegExp(“......”,“模式”);

模式有：g,全局匹配。i,不区分大小写。m可以进行多行匹配

常用方法

```js
I:regexp.text(str) 返回值是true和false，测试字符串是否符合此规则

var str="CATs";

var regexp = new RegExp("cat","gi");

var r = regexp.test(str);

document.write(r);

r为true

II:regexp.exec(str) 在字符串中查找并保存查找结果，返回字符串第一次出现的位置

var str="this is cat,this is cat!";

var regexp = new RegExp("cat","g");

var r = regexp.exec(str);

document.write(r.index);

结果为8
```

常见的字符及含义(都是重要的)

\w:字母数字或者下划线[a-zA-Z0-9_]

\d:用于匹配一个从0到9的数字[0-9]

^x:以x开头的

$:以...结尾的

*:重复的为任意次

+:重复的至少一次

{n,m}匹配至少n次，至多m次

字符串String和RegExp对象

```js
Str.match(regexp)使用正则模式查找字符串，并返回查找结果数组

var str="THis is that cat";

var regexp = /th\w{2}/gi;

var r = str.match(regexp);

document.write(r);

结果：THis,that

str.replace(regexp,”abc”)用指定的字符串替换匹配的字符串
```

#### 四：作用域

```js
alert(a); //弹出：function a(){alert(4);}

var a=1; //预解析中的a改为了1

alert(a); //弹出1

function a(){alert(2);}//函数声明，没有改变a的值。什么也没发生。

alert(a); //继续弹出1，因为a在预处理库里面的值没有被改过。

var a=3; //预处理中a的值变为3

alert(a); //弹出3

function a(){alert(4);} //函数声明，什么也没有发生

alert(a); //继续弹出3

a(); //报错 a is not a function
```

第一步: 预编译他会先找一些关键字存储到内存中。 比如var function 参数等等他找到var a先看左边，不看右边.上来都给他一个未定义 var a = undefined;要是function他就直接替换了 比如上面a从undefined直接变成了方法 他根本不考虑后面的值 

第二步: 在一步步执行代码 要是遇见表达式（表达式就是 var a = xxx）他才会重新替换或者赋值

#### 五：关于全局变量和局部变量

在方法内部写var的都是局部。在方法外面的都是全局变量。要是在方法里面不加var,那他改变的就是全局的值.

**值得注意的是：特别注意的就是在JS里面只有方法有作用域。for和if里面都没有作用域**

#### 六：关于return的问题

```js
fn1();

function fn1(){

  return function(){

    alert(1);

  }

}

这个返回的值就是function(){alert(1)}

 

fn1()();

function fn1(){

  return function(){

    alert(1);

  }

}

这个返回的值就是1
```

#### 七：小知识点总结

**小知识点1：**

parseInt(“123a123”)//输出123,他会输出第一个非数字之前的数字串

**小知识点2：**

```js
var f = function g(){

return 23;

};

typeof g();

报错，g没有被定义
```

**小知识点3：**

```js
var x = 1;

if (function f(){}) {

 x += typeof f;

}else{

 x++;

}

x;

结果为："1object"
```

同样的

if(function f(){}){console.log(f);}

结果为：f is not defined

# 选择题

1.除了一些常规的运算符之外，Javascript还提供了一些特殊的运算符。下面不属于Javascript特殊运算符的是：（） 

A.delete

B.size

C.new

D.typeof  

标准答案：B

 

2.以下关于Javascript中事件的描述中，不正确的是：（）

A.click——鼠标单击事件

B.focus——获取焦点事件 

C.mouseOver——鼠标指针移动到事件源对象上时触发的事件 

D.change——选择字段时触发的事件  

标准答案：D 

 

3.考察以下程序片段: var n = newNumber(3456); alert(n.toFixed(2)); 以下选项正确的是：(    )

A.输出34 B.输出 56 C.输出 3456.00 D.输出 345600  标准答案：C

四舍五入用toFixed(多少位),不四舍五入，可以round(6.66*100)/100来实现

预测以下代码片段的输出结果:

function add(i){ var k = i+10; alert(k);}

function add(i) { var k = i+20; alert(k);}

add(10);  

A.40 B.20 C.30 D.程序出错     标准答案：C

 

预测以下代码片段的输出结果:

```js
var student = new Object(); 

student.study = function()window.alert(“开始学习了”);}

study();  
```

A.输出“开始学习了” 

B.程序出错。不能在实例化对象之后，再添加方法

C.程序出错。study()方法不能直接调用。应该用student来调用

D.程序出错。给student.study赋值时，右边的函数必须有名字  

标准答案：C

 

考察以下程序片段: 

```js
varstr = “32px”;

var str1 = str.slice(-2); 

alert(str); 
```

alert(str1); 以下选项正确的是？ 

A.依次输出”px” “px”

B.依次输出”32” “32”

C.依次输出”32px” “px”

D.依次输出”32px” “32px”  

标准答案：C

slice() 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。上面为从第二个位置开始

Slice的另外一个例子在本例中，我们将提取从位置 6 到位置 11 的所有字符：

<script type="text/javascript">

var str="Hello happy world!"

document.write(str.slice(6,11))

</script>

输出：

happy

考察以下程序片段: 

```js
var str = “12px”; 

var s =str.indexof(“2”); 

alert(s);
```

以下选项正确的是？ 

 A.输出 1 B.输出 2 C.输出 p D.输出 12  标准答案：A

8.考察以下程序片段:

```js
function Person() {

} 

Person.prototype.move = function() {

alert(this.name+“移动”);}

function Student(name) {  

this.name = name; } 

Student.prototype.study = function() {

alert(this.name+”学习”); }

Student.prototype = new Person(); 

var st =new Student(“张三丰”);

st.study();

st.move();
```

以下选项正确的是？ 

A.依次输出”张三丰学习”“百晓生移动”

B.依次输出”张三丰学习”“移动”

C.输出”张三丰学习”，之后程序出错

D.程序出错，什么都不能输出

标准答案：D



9.以下不属于Javascript原始类型的是：（）

A.string 

B.number

C.function 

D.boolean  

标准答案：C 

 

10.以下哪段代码不能正确创建函数？ 

A.function show(text){ alert(text); } 

B.var showFun = function show(text){alert(text); } 

C.var showFun = function(text){alert(text); } 

D.var showFun =newfunction("text" , "alert(text)"};  

标准答案：D 



11.Javascript是如何实现继承的？  

A.创建父类对象作为子类的原型（prototype）

B.使用extends关键子继承父类 

C.创建子类对象作为父类的原型（prototype）

D.使用class关键子继承父类

标准答案：A

 

12.在JavaScript中，下列哪段代码能够在1秒之后执行表达式expression？  

A.window.setTimeout(1000，expression)；

B.window.setTimeout(expression，1)；

C.window.setTimeout(1，expression)；

D.window.setTimeout(expression，1000)；

 

13.以下哪个选项中的方法全部属于window对象：(    ) 

A.alert,clear,close

B.clear,close,open

C.alert,close,confirm

D.alert,setTimeout,write  

标准答案：C

Clear与write不是

 

14.与window对象无关的属性是下列哪项：(    ) 

A.top 

B.self 

C.left 

D.frames  

标准答案：C

 

15.如何在浏览器的状态栏放入一条消息：(    ) 

A.statusbar = "put your messagehere" 

B.window.status = "put your messagehere"

C.window.status("put your messagehere") 

D.status("put your messagehere")  

标准答案：B 



16.关于以下两个陈述的描述中，正确的是：（）

陈述1：window对象的confirm方法用于显示一个包括相关信息以及Yes和No这两个按钮的对话框。

陈述2：window对象的alert方法用于弹出一个提示窗口，显示提示信息。（）

A.陈述1正确，陈述2错误

B.陈述1错误，陈述2正确

C.陈述1和陈述2均正确

D.陈述1和陈述2均错误  

标准答案：B

 

17.下列不是document对象的属性的是：(    ) 

A.anchors B.forms C.location D.image  标准答案：D

 

18.下列说法有误的是（）  

A.event是window对象的一个属性，所以可以直接引用event对象

B.不同的浏览器事件处理的方式可能不同

C.对于同一事件，子对象的事件处理函数会覆盖父对象的事件处理函数 

D.事件可以增强用户与页面的交互  

标准答案：C



19.事件是按照DOM层次结构的由高到低顺序依次触发，则该事件流属于（ ） 

A.冒泡型 

B.捕获型

C.DOM型

D.BOM型

标准答案：B

 

20.预测以下代码片段运行结果:

var reg = /^\w+,Java\w*$/ 

var str = “Hello,JavaScript!”;

var b = str.match(reg);

document.write(b);

输出Hello,JavaScript! B.输出Java C.输出 null D.输出false  

标准答案：C



21.一年有12个月。现要求月份的正确格式为: 1,2,….9,10,11,12。以下哪个正则表达式可以符合要求？ 

A./^[1-12]$/ 

B./^[1-9]\d?$/

C./^([1-9]︱1[0-2])$/

D./^\d︱11︱12︱10$/

标准答案：C

22.考察以下代码片段，预测输出结果（ ） 

```js
＜script＞   

function handleEvent()  

{ alert("我被点击了！");}  

 document.form1.button1.onclick =handleEvent; 

＜/script＞ 

＜body＞   

＜form name=”form1”＞    

＜INPUT type="button" name="button1" value="测试按钮" /＞  

＜/form＞ 

＜/body＞  
```

A.输出 “我被点击了” 

B.没有错误，但也没有任何输出。

C.出现错误，没有任何输出。 

D.出现错误，但输出 “我被点击了”  

标准答案：C



23.除了一些常规的运算符之外，Javascript还提供了一些特殊的运算符。下面不属于Javascript特殊运算符的是：（） 

A.delete

B.size

C.new

D.typeof  

标准答案：B



24.考察以下代码片段，预测输出结果（ ）

var str 

alert(typeof str); 

A..string 

B..undefined;

C..object 

D..String;

标准答案：B 



# 案例合集

主要是掌握代码里面的方法和知识点还有思想，最好肯定还是不看代码敲出来

1：排序数组

数组排序按照Unicode来排，所以往往需要自定义排序规则

方法就是：数组名.sort(排序规则，也就是一个函数)

```js
var arr = [1,55,23,6,44,8];
    function sortfun(a,b){
        return a-b;
    }
alert(arr.sort(sortfun));
```



2：实现如下效果

![image-20201223200630273](https://gitee.com/leidl97/picture/raw/master/img/20201223200630.png)

大体思路：将页面分为三个div，分别为大中小

大的div里为1000*1000在图中为绿色

中的div里为10行，第一行放一个小div，第二行放两个...

```js
function run(){
		var bigdiv = document.getElementById("big");
		for(var i=0;i<10;i++){
			bigdiv.innerHTML+="<div id='m"+i+"' class='middle'></div>";
			var middlediv = document.getElementById("m"+i);
			for(var j=0;j<=i;j++){
				middlediv.innerHTML+="<div class='small'>"+j+"</div>";
			} 
		}
	}
```



3：轮播图(1)

需要定时器setInterval(function(){},毫秒数)//每毫秒数执行一次函数

获得图片标签数组(利用DOM操作)

遍历数组中的图片(for循环)

count自增取余图片数找到显示的图片，该显示的显示，该隐藏的隐藏

```js
	var count = 0;
	setInterval(function(){
		//得到页面中的所有图片元素
		var imgs = document.getElementsByTagName("img");
		count++;
		//遍历每一张图片
		for(var i = 0;i<imgs.length;i++){
			var img = imgs[i];
			//找到需要显示的元素
			if(i==count%3){
				img.style.display="block";
			}else{
				//需要隐藏的元素
				img.style.display="none";
			}
		}
	},2000)
```



轮播图(2)(扩展)

轮播图1只能算作闪动图，而2才能称作轮播图

先把n张图片设置为绝对定位

遍历每张图片设置距左距离，实现一横行的效果

轮播图实现的要点在于两个定时器的使用，其中move函数移动完一张图片后就停止了，而另外一个计时器则控制move计时器在规定时间内走一次，由鼠标移入事件控制定时器的关闭

要想实现整张图片动，需要让每张小图片动起来，使用for循环实现

获得图片标签，获得图片距左距离(img.style.left)设置每次调用函数减多少像素，在这里需要注意的是这里不需要判断是不是NaN，因为在设置为一行的时候已经有初始的值

```js
//页面加载完，初始化图片位置
	onload = function(){
		var imgs = document.getElementsByTagName("img");
		for(var i =0;i<imgs.length;i++){
			var img = imgs[i];
			img.style.left = i*200+"px";
		}
	}
	function move(){
		var moveId = setInterval(function(){
			var imgs = document.getElementsByTagName("img");
			/* 遍历每张图片，让图片的位置-5px */
			for(var i =0;i<imgs.length;i++){ 
				var img = imgs[i];
				//得到之前的位置
				var oldLeft = parseInt(img.style.left);
				oldLeft-=5;
				//移出屏幕后，放在所有图片的后面
				if(oldLeft<=-200){
					oldLeft = 400;
					/* 图片移动一格就停止 */
					clearInterval(moveId);
				}
				img.style.left=oldLeft+"px";
			}
		},1000/60)
	}
	//每隔两秒移动一次图片
	var myId;
	//让两秒倒计时的定时器停止
	function stop(){
		clearInterval(myId);
	}
	function start(){
		myId = setInterval(function(){
			move();
		},2000);
	}
	//一运行就开始移动图片
	start();
	//离开页面停止
	onblur = function(){
		stop();
	}
	//获取焦点继续
	onfocus = function(){
		start();
	}
```





4：选择变换城市

首先创建城市数组(二维数组)

使用onchange()来实现

在主城市中设置对应的value值，设置为数组下标的值(0,1,2)

通过选择对应城市获得数组下标的值，遍历城市数组

将遍历到的信息添加到第二个下拉选中(使用appendChild())

需要注意的是在添加的时候还需要清空下拉选中的内容最简单的方法就是innerHTML=”<option>请选择</option>”

```js
var cities = [["唐山","秦皇岛","邯郸"],["哈尔滨","齐齐哈尔","佳木斯"],["青					岛","济南","烟台"]];
	function changeAction(){
		var s1 = document.getElementById("s1");
		var index = s1.value;
		//通过数组下标找到对应的城市信息
		var s2 = document.getElementById("s2");
		//清除之前s2中的内容
		s2.innerHTML="<option>请选择</option>"
		//判断选择的省份非请选择
		if(!undefined){
			for(var i=0;i<cities[index].length;i++){
				var op = document.createElement("option");
				op.innerText=cities[index][i];
				s2.appendChild(op);
			}
		}
	}

```



5：订单练习(实现全选和总额的计算)

这个练习中主要是使用到了事件源

全选：需要两步

得到全选和各个子选项的标签

遍历子选项标签将全选的选中状态分别赋给各个自选项标签(item.checked=all.checked)

求总额：需要两步

当你点击哪一个就获取哪一个的value值(前提为选中状态)并计算求和(用到事件源)

由于value是String类型，所以将得到的值强转为浮点型数据后赋给p标签

```js
function allfn(){
		//先得到全选标签
		var all = document.getElementById("all");
		//得到所有商品
		var items = document.getElementsByName("item");
		//遍历每一个商品
		for(var i=0;i<items.length;i++){
			var item = items[i];
			//让每一个input状态等于全选状态
			item.checked=all.checked;
		}
		calfn();
	}
	function calfn(){
		//得到所有商品
		var all = document.getElementsByName("item");
		//准备总金额的变量
		var totalmoney=0;
		//遍历,把打勾的金额相加
		for(var i=0;i<all.length;i++){
			var shop = all[i];
			//判断是否选中
			if(shop.checked){
				//选中
				//得到选中的金额
				var money = parseFloat(shop.value);
				totalmoney+=money;
			}else{
				//未选中
			}
		}
		var p = document.getElementsByTagName("p")[0];
		p.innerText='总价：'+totalmoney+'元';
	}

```



6：计算器的实现(同样用到事件源)

首先搭建好计算器页面

获取INPUT事件源

事件源分为三部分=，c，数字和其他运算符(每个标签都有对应的value值)

最后将键入的结果拼接到显示区域中

```js
function calfn(){
		//获取事件源
		var obj = event.target||event.srcElement;
		//alert(obj.nodeName);
		//找出按钮
		if(obj.nodeName=="INPUT"){
			//得到p标签
			var p = document.getElementById("text");
			if(obj.value=="="){
				//等号
				p.innerText=eval(p.innerText);
			}else if(obj.value=="c"){
				//清除按钮
				p.innerText="";
			}else{
				//数字和运算符
				p.innerText+=obj.value;
			}
		}
	}

```
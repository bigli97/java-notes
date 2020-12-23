# 总结

### 什么是jsp

Sun公司制定的一种服务器端动态页面技术规范

### 为什么需要jsp

Servlet对于逻辑处理是非常方便的,但是对于页面的展现是非常的麻烦的,jsp诞生是为了解决servlet页面展现麻烦的问题的

### jsp的特点

Jsp展现非常方便,但是业务逻辑处理非常麻烦

### jsp的原理

在我们访问jsp文件的时候,并没有直接去执行jsp文件,而是被服务器所拦截执行了jspservlet的类,此类会将jsp文件转义成对应的servlet去执行,所以jsp的本质还是servlet

### jsp页面构成(7个)

##### 静态内容:html

##### 常用指令集

> Page
>
> Language:声明jsp所支持的语言(过度设计)
>
> Import:转译的时候导入的包
>
> Pageencoding:客户端展现的编码格式
>
> Session:true代表使用session对象(默认),false代表不使用session对象
>
> Errorpage:jsp运行错误自动跳转到指定的页面
>
> Include:有两种方式(后面介绍)
>
> Taglib指令:jstl(标准标签库,后面介绍)

##### 表达式

<%=变量名/方法名%>后面不能加分号

##### scriptlet

<%java代码%>会被转译到service方法中,局部代码

##### 声明

全局代码声明<%!声明方法或者变量%>会被转译成全局代码

**注意**:jsp定义的方法和变量可以使用private,public修饰符也可以使用static,将其变成类属性或者类方法,但是不能使用abstract修饰部分的方法,因为抽象方法会导致jsp对应servlet变成抽象类,导致无法实例化

##### 动作

<jsp:动作名 属性=”属性值”/>

##### 注释

1)Java格式注释(客户端源码看不见)

编译器会忽略掉此类注释中的内容

<%--jsp注释,可多行 --%>

<%//Java单行注释%>

<%/*Java多行注释*/%>

<%/**Java特有的注释*/%>

2)html风格注释(客户端源码能看见)

编译器会执行注释中的代码

<!--html风格注释-->等价于out.println(“<!--html风格注释-->”)

这种注释方式不好的地方在于当前页面注释信息太多的时候会增加服务器的负荷,还有注释信息需要在网上进行传输,从而降低效    率,内部程序员的测试数据一般不能写在这种注释中,以免泄露

 

### jsp的执行过程

1:转译:jsp-->servlet

2:编译:servlet-->.class

3:执行:.class

第一次访问jsp响应速度较慢,后面请求时响应速度快

 

### 九大内置对象

#### 四个作用域

Pagecontext:管理页面属性

Request:请求对象,使用方式和servlet一致

Session:就是session对象

Application:就是servletcontext对象

#### 两个输出

Out:输出对象

Response:输出对象,比原本的servlet中的response对象多了缓冲区,效率更高

#### 三个打酱油

Page:他就是指当前jsp页面本身,有点像类中的this指针

Config:ServletConfig(获得初始值用到)

Exception:异常对象(摒弃了)

 

### jsp的生命周期(也能说servlet的生命周期)

实例化,初始化,调用,销毁

实例化:

##### 什么是实例化?

容器调用servlet的构造器,创建servlet对象

##### 什么时候实例化?

**情况1**:默认情况下,容器收到请求之后才会创建servlet实例(容器只会创建一个实例)

**情况2**:容器启动之后立即创建servlet实例,需要配置

load-on-startup参数

初始化:

##### 什么是初始化?

容器在实例化之后,会调用servlet实例中的init方法

(该方法只执行一次)

Genericservlet已经提供了一个init方法的一个简单实现,所以我们不写这个方法也可以

如果要实现自己的初始化逻辑,我们只需要重写

genericservlet的init()方法

调用(就绪)

##### 什么是就绪?

容器收到请求的时候,会调用servlet实例的service方法来处理请求,service方法是这样实现的:依据请求的类型,调用对应的doxxx方法,比如get方式调用deGet方法,post方式调用doPost方法(这些方法只是抛出了一个异常)

我们写一个servlet可以是重写httpservlet的service方法,或者重写httpservlet的doGet方法或者doPost方法

**销毁**:

容器在删除servlet实例之前,会调用该实例的destroy方法,该方法只调用一次genericservlet已经实现了destroy方法

 

### 转发和重定向的区别

##### 1:是否能共享request对象

转发可以,重定向不行

注:当容器收到请求之后会立即创建request对象和

response对象当响应发送完毕,容器会立即删除这两个对象,也就是说,request对象和response对象生存时间是一次请求和响应期间存在

转发是一次请求,而重定向是两次请求

##### 2:地址栏地址有无变化

转发不变,重定向改变

##### 3:目的地有无限制

转发有,重定向没有

 

### 能否在jsp脚本中定义方法

不能,脚本相当于方法,不能在方法里定义方法

<%! public void method(){}%>//可以声明

<% public void method()%{}%>//编译出错

 

### Java bean

一个标准的Java bean有三个条件

1:共有的类

2:具有不带参数的公共构造方法

3:具有set()和get()方法

4:私有属性

 

### jsp的标准动作(有5个属于扩展)

1:useBean:

创建一个Javabean的一个实例

`<jsp:useBean id=”stu”    class=”web.Student”`

`Scope=”page/request/session/application”>`

2:setProperty:

给Javabean的属性赋值

`<jsp:setProperty name=”stu” property=”stuName”`

`Value=”zhangsan”>`

`<jsp:setProperty name=”stu” property=”stuName”`

`param=”txtname”>`

value和param不能同时使用

3:getProperty:

获得Javabean的属性值

`<jsp:getProperty name=”stu” property=”stuName”/>`

4:forword:内部跳转,相当于request

`getRequestDispatcher().forword(request,response)`

`<jsp:forword page:”要转发的文件路径”/>`

请求转发(自带return)

注意:执行forword指令转发请求时,客户端请求参数不会丢失,表面上他是将用户请求转发到另一个新的页面,实际上只是采用了新的界面来对用户生成响应,所以说,请求还是一次请求,作用请求参数和请求属性都不会改变

5:include:包含

`静态引入:<%@ include file=””%>`

特点:在翻译阶段执行,也就是jsp在转换成servlet的阶段进行的,相当于编译一个文件,不能出现相同的变量,耦合度高

(了解)动态引入:

`<jsp:include page=”” flush=”true”>`

特点:在请求阶段执行,相当于编译两个文件,可以包含同名的变量,耦合度低,被导入的的编译指令失去作用,只是插入被导入页面的body内容

flush属性用语指定输出缓存是否转移到被导入文件中。如果指定为true，则包含在被导入文件中，如果指定为false，则包含在原文件中

 注意:使用<jsp:include>动作通常是包含那些经常改动的文件，因为被包含的文件改动不会影响到包含文件，因此不需要对包含文件进行重新编译

 

### 表达式语言

El:expression language

##### 什么是el?

他是一套简单的运算规则,用于给jsp标签赋值,也可以脱离jsp标签直接使用

语法格式:${表达式}

表达式=运算符+操作数

运算符:跟Java比较多了一个empty

${empty “”}true

相同情况还有空数组,null,不存在的变量

##### 操作数:

常量:布尔型,整型,浮点型,字符串(单双引都可以),null

##### 变量:

1:指的是放在四个标准范围里的属性(page,request

Session,application)

2:在标准范围内的搜索顺序:就上面那顺序

3:怎么取得变量值:点运算符,还可以用[]

<%request.setAttribute(“name”,”list”)%>

${requestScope.name}

${requestScope[“name”]}

##### El表达式适用的场合

1:可以在静态文本中使用

2:与自定义标签结合使用

3:和Javabean结合使用

 

### Jsp中存在的多线程问题(了解)

当客户端请求某一个jsp文件时,服务端把该jsp文件编译成一个class文件,并创建一个该类的实例,然后创建一个线程处理客户端的请求,如果有多个客户端同时请求该jsp文件,则服务端会创建多个线程,每个客户端请求对应一个线程,以多线程的方式可大大降低对系统的资源需求,提高系统并发量和响应时间

对jsp可能用到的变量说明如下

实例变量:实例变量是在堆中分配的,并属于该实例所有线程共享,所以不是线程安全的

Jsp提供的8个类变量

Jsp用到的out,request,response,session,config,page,

pagecontent是线程安全的(因为每个线程对应的request和response对象都是不一样的,不存在共享问题),application在整个系统内被使用,所以不是线程安全的

局部变量:局部变量在堆栈中分配,因为每个线程都有自己的堆栈空间,所以是线程安全的

静态类:静态类不用被实例化,就可以直接使用,不是线程安全的

外部资源:在程序中可能会有多个线程和进程同时操作同一个资源,此时也要注意同步问题

# 选择题大全

1:如果我们的提交方式是post,在httpservlet没有dopost方法时,将会出现哪种错误(d)

A:404    B:500    C:400    D:405

 

2:不能在不同用户之间共享数据的方法是(d)

A:利用文件系统                    B:利用数据库

C:通过servletcontext对象        D:通过cookie

 

3:j2ee中,httpsession接口位于(d)包中

A:javax.servlet                    B:javax.servlet.http.session

C:javax.servlet.session            D:javax.servlet.http

 

4:web应用中,常用的会话跟踪方式不包括(b)

A:隐藏表单域                    B:有状态http协议

C:cookie                            D:url重写

 

5:一个servlet类文件必须发布在虚拟目录的什么文件下?(b)

A:root                            B:WEB-INF/classes

C:WEB-INF/lib                    D:WEB-INF/

 

6:什么是动态的网页(b)

A:支持动态效果的                B:可以交互的

C:可以运行脚本的                D:可以看电影的

 

7:JSP文件test.jsp文件如下所示，运行时，将发生（D）。

 

<% String str = null;%>

   str is <%=str%>

A:编译阶段出现错误

B:翻译阶段出现错误

C:执行字节码时发生错误

D:运行后，浏览器上显示：str is null

 

8:在JSP页面中进行访问控制时，一般会使用JSP的( B )内置对象实现对用户的会话跟踪

A:request    B:session    C:response    D:application

 

9:在一个jsp页面中包含了这样一种页面元素，<% int i = 10; %>这是（ B ）

A:表达式        B:小脚本        C:指令        D:注释

 

10:在JSP中，以下（ C ）技术最适合实现购物车的存储

A:page        B:request        C:session        D:application

 

11:以下JSP关键代码的运行效果为（ A ）

<%

​     Map map=new HashMap();

​     map.put("a","Java");

​     map.put("b","JSP");

​     map.put("a","C#");

​     request.setAttribute("map",map);

%>

${map.b}<br/>

${map["a"]}

A:jSP c#        B:JSP JAVA     C:运行出现错误        D:什么都不输出

 

12:以下JSP代码片段的输出结果是（ D ）

<%

​     String getName(String name){

​          return name.subString(0,3);

​     }

%>

姓名：<%=getName("齐德龙东强")%>

A:姓名

B:姓名:齐德

C:姓名:齐德龙

D:编译错误

 

13:login.jsp页面代码如下：

```html

<form action="display.jsp">

     <input type="text" name="u1" value="admin1"/>

     <input type="text" name="u2" value="admin2"/>

     <input type="submit" value="登录"/>

</form>
```

display.jsp页面代码如下：

```html
<%

​     request.setAttribute("x","admin3");

​     request.getRequestDispatcher("success.jsp").forward(request,response);

%>

success.jsp页面代码如下：

<%=request.getParameter("u1")%>

<%=request.getAttributer("x")%>
```

A:admin1 admin2

B:admin1 null

C:admin1 admin3

D:null admin3

 

14:如下JSP代码输出集合中各元素，横线处应填写（ AC ）。（选择二项）

```html
<%

​     List<String> strs= new ArrayList<String>();

​     strs.add("北京");

​     strs.add("上海");

​     strs.add("浙江");

​     request.setAttribute("strs",strs);

%>

<c:forEach var="strList" items="___________">

​     <c:out value="________"></c:out>

</c:forEach>
```

A.${strs},${strList}

B.${strList},${strs}

C.${requestScope.strs},${strList}

D.${strList}, ${requestScope.strs}

 

15:在JSP中，假设表单的method="post"，在发送请求时中文乱码处理的正确做法是（ A ）（选择一项）

A.request.setCharacterEncoding("utf-8");

B.response.setCharacter("utf-8");

C.request.setContentType("text/html;charset=utf-8");

D.response.setContentType("text/html;charset=utf-8");

 

16:有如下jsp代码,输出为B

<%

​    int a = 5;

​         pageContext.setAttribute("a", "123");

​    request.setAttribute("a","456");

​    session.setAttribute("a","789");

%>

${a }

A:5

B:123

C:456

D:789

 

17:在JSP中，只有一行代码:<%=AB%>,运行将输出( D )

A:A B

B:AB

C:113

D:没有任何输出

 

18:JSP中有三大类标签，分别是( C )

A:HTML标记  JSP标记  Servlet标记

B:CSS标记  HTML标记  Javascript标记

C:动作标记  脚本标记  指令标记

D:指令标记 脚本标记 HTML标记

 

19:有三个JSP文件如下D

1.jsp

<a href="2.jsp?user=svse">To 2.jsp</a>

2.jsp

<%Stringuser=request.getParameter("user");%>

<jsp:forward page="3.jsp"/>

3.jsp

<%=request.getParameter("user")%>

页面中输出 (  )

A:报错

B:什么都没有

C:null

D:svse

 

20:在JSP中，给定以下JSP代码片段，运行结果是（  ）

<% int x=5; %>

<% ! int x=7; %>

<%!

  Int getX(){

​      returnx;

}

%>

<% out.print(“X1=” x);   %>

<% out.print(“X2=”getX()); %>

A:X1=5 X2=7

B:X1=5 X2=5

C:X1=7 X2=7

D:X1=7 X2=5

 

21:JSP EL 表达式：${user.loginName}执行效果等同于（  ）A

A:<%=user.getLoginName()%>

B:<%user.getLoginName();%>

C:<%=user.loginName%>

D:<%user.loginName;%>



# 简答题大全

##### **jsp的原理**

在我们访问jsp文件的时候,并没有直接去执行jsp文件,而是被服务器所拦截执行了jsp servlet的类,此类会将jsp文件转译成对应的servlet去执行,所以jsp的本质还是servlet

 

##### **jsp的页面构成**

1:静态内容:html

2:常用指令集

3:表达式:<%=变量名/方法名%>后面不能加分号

4:scriptlet:<%java代码%>会被转译到service方法中,局部代码

5:声明:全局代码声明<%!声明方法或者变量%>会被转译成全局代码

6:动作:<jsp:动作名 属性=”属性值”/>

7:注释

 

##### **jsp的执行过程**

1:转译:jsp-->servlet

2:编译:servlet-->.class

3:执行:.class

 

**##### jsp九大内置对象**

四个作用域

Pagecontext:管理页面属性

Request:请求对象,使用方式和servlet一致

Session:就是session对象

Application:就是servletcontext对象

两个输出

Out:输出对象

Response:输出对象,比原本的servlet中的response对象多了缓冲区,效率更高

三个打酱油

Page:他就是指当前jsp页面本身,有点像类中的this指针

Config:ServletConfig(获得初始值用到)

Exception:异常对象(摒弃了)

 

##### **Servlet生命周期**

实例化,初始化,调用,销毁

实例化:

什么是实例化?

容器调用servlet的构造器,创建servlet对象

什么时候实例化?

情况1:默认情况下,容器收到请求之后才会创建servlet实例(容器只会创建一个实例)

情况2:容器启动之后立即创建servlet实例,需要配置

load-on-startup参数

初始化:

什么是初始化?

容器在实例化之后,会调用servlet实例中的init方法

(该方法只执行一次)

Genericservlet已经提供了一个init方法的一个简单实现,所以我们不写这个方法也可以

如果要实现自己的初始化逻辑,我们只需要重写

genericservlet的init()方法

调用(就绪)

什么是就绪?

容器收到请求的时候,会调用servlet实例的service方法来处理请求,service方法是这样实现的:依据请求的类型,调用对应的doxxx方法,比如get方式调用deGet方法,post方式调用doPost方法(这些方法只是抛出了一个异常)

我们写一个servlet可以是重写httpservlet的service方法,或者重写httpservlet的doGet方法或者doPost方法

销毁:

容器在删除servlet实例之前,会调用该实例的destroy方法,该方法只调用一次genericservlet已经实现了destroy方法

 

##### 转发和重定向的区别

1:是否能共享request对象

转发可以,重定向不行

注:当容器收到请求之后会立即创建request对象和

response对象当响应发送完毕,容器会立即删除这两个对象,也就是说,request对象和response对象生存时间是一次请求和响应期间存在

转发是一次请求,而重定向是两次请求

2:地址栏地址有无变化

转发不变,重定向改变

3:目的地有无限制

转发有,重定向没有

**include静态引入和动态引入的区别**

静态引入:<%@ include file=””%>

特点:在翻译阶段执行,也就是jsp在转换成servlet的阶段进行的,相当于编译一个文件,不能出现相同的变量,耦合度高

动态引入:

<jsp:include page=”” flush=”true”>

特点:在请求阶段执行,相当于编译两个文件,可以包含同名的变量,耦合度低,被导入的的编译指令失去作用,只是插入被导入页面的body内容

flush属性用语指定输出缓存是否转移到被导入文件中。如果指定为true，则包含在被导入文件中，如果指定为false，则包含在原文件中

 注意:使用<jsp:include>动作通常是包含那些经常改动的文件，因为被包含的文件改动不会影响到包含文件，因此不需要对包含文件进行重新编译

 

##### **servlet的优势**

1:具有优良的跨平台性

2:可移植性良好:Java编写,servlet API完善,企业编写的servlet程序可轻松移植到其他服务器中

3:执行效率高:servlet请求到来时激活servlet,处理完成等待请求后等待新请求,新请求产生新线程而不是进程

4:使用方便:可轻松处理HTML表单数据,并读取和设置http头,处理cookie,跟踪会话

 

##### **jsp和servlet的区别**

1:jsp经编译后就变成了servlet(jsp本质就是servlet.jvm能识别Java的类,不能识别jsp代码,web容器将jsp代码编译成jvm能够识别的Java类)

2:jsp更擅长于页面显示,servlet更擅长于逻辑控制

3:servlet中没有内置对象,jsp是servlet的简化,而servlet则是个完整的Java类,这个类的service方法用于生成对客户端的响应

对于静态的HTML标签,servlet都必须使用页面输出流逐行输出

 

##### cookie和session的区别

从存储方式上比较

Cookie只能存储字符串，如果要存储非ASCII字符串还要对其编码。

Session可以存储任何类型的数据，可以把Session看成是一个容器

从隐私安全上比较

Cookie存储在浏览器中，对客户端是可见的。信息容易泄露出去。如果使用Cookie，最好将Cookie加密

Session存储在服务器上，对客户端是透明的。不存在敏感信息泄露问题。

从有效期上比较

Cookie保存在硬盘中，只需要设置maxAge属性为比较大的正整数，即使关闭浏览器，Cookie还是存在的

Session的保存在服务器中，设置maxInactiveInterval属性值来确定Session的有效期。并且Session依赖于名为JSESSIONID的Cookie，该Cookie默认的maxAge属性为-1。如果关闭了浏览器，该Session虽然没有从服务器中消亡，但也就失效了。

从对服务器的负担比较

Session是保存在服务器的，每个用户都会产生一个Session，如果是并发访问的用户非常多，是不能使用Session的，Session会消耗大量的内存。

Cookie是保存在客户端的。不占用服务器的资源。像baidu、Sina这样的大型网站，一般都是使用Cookie来进行会话跟踪。

从浏览器的支持上比较

如果浏览器禁用了Cookie，那么Cookie是无用的了！

如果浏览器禁用了Cookie，Session可以通过URL地址重写来进行会话跟踪。

从跨域名上比较

Cookie可以设置domain属性来实现跨域名

Session只在当前的域名内有效，不可跨域名

 

#####  get请求和post请求的特点

1:get请求

哪一些情况下，浏览器会发送get请求

​    情形1：表单默认的提交方式

​    情形2：直接在浏览器地址栏输入某个地址

​    情形3：点击链接

​    特点

​    特点一：会将请求参数添加到请求资源路径的后面，所以只能提交少量的数据

​    所以请求行大约只能放2k左右的数据

​    特点二：会将请求参数显示在浏览器地址栏，非常不安全(因为有一些网络设备，比如    路由器会记录请求地址)

2.post请求

​    a.哪一些情况下，浏览器会发送post请求

​    将表单的method设置为post

​    特点一:会将请求参数添加到实体内容里面，所以可以提交大量的数据

​    特点二:不会将请求参数显示在浏览器地址栏，相对安全一些

​    不管什么请求都不会对数据进行加密处理

​    所以，对于敏感数据，一定要加密
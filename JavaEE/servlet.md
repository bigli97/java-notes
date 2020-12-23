# 总结

#### 什么是servlet

Sun公司指定用来扩展web服务器功能的组件规范

##### 什么叫扩展web服务器功能

Web服务器只能处理静态资源的请求(即需要事先将html文件及相关的图片等资源准备好,不能够处理动态资源的请求(即需要计算,生成相应的html),所以需要扩展web服务器的功能Servlet可以扩展web服务器功能,当web服务器收到请求之后,可以调用servlet来处理动态资源请求

 

#### servlet是如何运行的(看看图)

比如在地址栏输入http://localhost:8080/day01/hello,发生了什么

1:浏览器根据ip和port建立连接

2:浏览器将请求相关的数据打包,,即创建数据请求数据包,然后发送给servlet容器

3:servlet容器解析请求数据包,并且将解析到的数据存放到request对象里面,同时,还会创建一个response对象

4:servlet容器会依据请求路径找到对应的servlet配置,然后容器将此servlet实例化

5:servlet容器会调用service方法(会将response和reqeust作为参数传递进来,开发人员可以调用request的方法来获得请求数据包中的数据(比如获取请求参数值),也可以将处理结果写在response对象里面)

6:容器从response对象中去除处理结果,然后创建响应数据包并发送给浏览器

7:浏览器解析响应数据包,并生成响应界面

 

#### Servlet优势

1:具有优良的跨平台性

2:可移植性良好:Java编写,servlet API完善,企业编写的servlet程序可轻松移植到其他服务器中

3:执行效率高:servlet请求到来时激活servlet,处理完成等待请求后等待新请求,新请求产生新线程而不是进程

4:使用方便:可轻松处理HTML表单数据,并读取和设置http头,处理cookie,跟踪会话

 

#### 怎样理解servlet单实例多线程?(扩展)

不同的不同的用户同时对同一个业务（如注册）发出请求，那这个时候容器里产生的有是几个servlet实例呢？

只有一个servlet实例,一个servlet是在第一次被访问时加载到内存并实例化的,同样的业务请求共享一个servlet实例,不同的业务请求一般对应不同的servlet

由于servlet默认是以多线程模式执行的,所以在编写代码的时候需要细致的考虑多线程的安全性问题

Servlet单实例多线程机制

Servlet采用多线程来处理多个请求同时访问,servlet依赖于一个线程池来服务请求,线程池其实是一系列的工作者线程集合,servlet使用一个调度线程来管理工作者线程

当容器收到一个servlet请求,调度线程从线程池中选出一个工作者线程,将请求传递给该工作者线程,然后由该线程来执行servlet的service方法,当这个线程正在执行的时候,容器收到另一个请求,调度线程同样从线程池中选出另外一个工作者线程来服务新的请求,容器并不关心这个请求是否访问的是同一个servlet,当容器收到对同一servlet多个请求的时候,那么servlet的service方法将在多线程中并发执行

Servlet容器默认单实例多线程的方式来处理请求,这样减少servlet实例的开销,提升了对请求的响应时间,对于tomcat可以在service.xml里通过<connector>元素设置线程池线程的数目

 

#### Servlet和jsp的区别

1:jsp经编译后就变成了servlet(jsp本质就是servlet.jvm能识别Java的类,不能识别jsp代码,web容器将jsp代码编译成jvm能够识别的Java类)

2:jsp更擅长于页面显示,servlet更擅长于逻辑控制

3:servlet中没有内置对象,jsp是servlet的简化,而servlet则是个完整的Java类,这个类的service方法用于生成对客户端的响应

对于静态的HTML标签,servlet都必须使用页面输出流逐行输出

 

#### 两种请求方式(区别见jsp总结)

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

 

#### session和cookie的区别

##### 从存储方式上比较

Cookie只能存储字符串，如果要存储非ASCII字符串还要对其编码。

Session可以存储任何类型的数据，可以把Session看成是一个容器

##### 从隐私安全上比较

Cookie存储在浏览器中，对客户端是可见的。信息容易泄露出去。如果使用Cookie，最好将Cookie加密

Session存储在服务器上，对客户端是透明的。不存在敏感信息泄露问题。

##### 从有效期上比较

Cookie保存在硬盘中，只需要设置maxAge属性为比较大的正整数，即使关闭浏览器，Cookie还是存在的

Session的保存在服务器中，设置maxInactiveInterval属性值来确定Session的有效期。并且Session依赖于名为JSESSIONID的Cookie，该Cookie默认的maxAge属性为-1。如果关闭了浏览器，该Session虽然没有从服务器中消亡，但也就失效了。

##### 从对服务器的负担比较

Session是保存在服务器的，每个用户都会产生一个Session，如果是并发访问的用户非常多，是不能使用Session的，Session会消耗大量的内存。

Cookie是保存在客户端的。不占用服务器的资源。像baidu、Sina这样的大型网站，一般都是使用Cookie来进行会话跟踪。

##### 从浏览器的支持上比较

如果浏览器禁用了Cookie，那么Cookie是无用的了！

如果浏览器禁用了Cookie，Session可以通过URL地址重写来进行会话跟踪。

##### 从跨域名上比较

Cookie可以设置domain属性来实现跨域名

Session只在当前的域名内有效，不可跨域名



# 选择题大全

1:在java Web应用开发中，Servlet程序需要在（B）文件中配置

A:JSP

B:web.xml

C:struts.xml

D:servlet.xml

 

2:以下代码中可以正确设置客户端请求编码为UTF-8 的是（A）

A:request.setCharacterEncoding("UTF-8")

B:request.setCharset("UTF-8")

C:request.setContentType("UTF-8")

D:request.setEncoding("UTF-8")

 

3:在WEB应用程序开发中，有时会出现Tomcat端口号已经被占用的情况，为此我们需要修改配置文件，下列选项中修改正确的是（ B ）

A:在tomcat目录\bin文件夹\server.xml文件中，修改 Connection的port

B:在tomcat目录\conf文件夹\server.xml文件中，修改Connector的port

C:在tomcat目录\bin文件夹\server.xml文件中，修改Connector的port

D:在tomcat目录\conf文件夹\server.xml文件中，修改 Connection的port

 

4:如果要把一个“accp”字符串信息放在session对象里，则下列正确的是（A ）

A:session.setAttribute("message","accp");

B:session.setAttribute(message,"accp");

C:session.setAttribute("accp","message");

D:session.setAttributes("message","accp");

 

5:假设session对象中存放了一个Book对象，即：

session.setAttribute("book",new Book()) , 则取出Book对象的正确语句是（ B ）

A:Book book = session.getAttribute("book")

B:Book book = (Book)session.getAttribute("book")

C:Book book = session.getValue("book")

D:Book book = (Book)session.getValue("book")

 

6:在JSP中，以下可以实现请求转发的是（ D ）

A:request.getRequestDispatcher("list.jsp");

B:response.getRequestDispatcher("list.jsp");

C:response.getRequestDispatcher("list.jsp").forward(request,response);

D:request.getRequestDispatcher("list.jsp").forward(request,response);

 

7:web.xml中预先对Servlet进行初始化设置的代码如下：

<init-param>

 <param-name>myWord</param-name>

 <param-value>hello</param-value>

</init-param>

则如下获取初始化参数的语句正确的是（ A ）

A:String myWord = getInitParameter("myWord");

B:String myWord = getInitParameter("hello");

C:String myWord = getInit("myWorld");

D:String myWord= getInit("hello");

 

8:以下代码片段是使用cookie存储数据，横线处填写（ D ）可以在look.jsp页面显示”用户名：Jack"

<%

 response.addCookie(new Cookie("uname","Jack");

__(1)

%>

look.jsp页面部分代码

<%

Cookie[]cookies=(2)__

String user="";

​     if(cookies !=null){

for(int i =0;i<cookies.length;i++){

​         if(cookies[i].getName().equals("uname"))

user = cookies[i].getValues();

​     }

}

 out.print("用户名：+user);

%>

A:(1)request.getRequestDispatcher("look.jsp").forward(request,respons    e);

(2)response.getCookies()

B:(1)request.getRequestDispatcher("look.jsp").forward(request,respons    e)

(2)request.getCookies();

C:(1)response.sendRedirect("look.jsp")

(2)response.getCookies();

D:(1)response.sendRedirect("look.jsp")

(2)request.getCookies();

 

9:以下关于Servlet生命周期说法错误的是（ C ）

A:Servlet容器根据Servlet类的位置加载Servlet类，成功加载后，由容器创建Servlet的实例

B:对于每一个Servlet实例，init()方法只被调用一次

C:当Servlet容器接收到客户端请求时，调用 Servlet的service()方法以及destory（）方法处理客户端请求

D:servlet的实例是由servlet容器创建的，所以实例销毁也由容器完成

 

10:自定义标签的配置文件放在_D

A:WebRoot

B:lib

C:classes

D:WEB-INF

 

11:J2EE中，Servlet API为使用Cookie,提供了（ A ）类

A:javax.servlet.http.Cookie

B:javax.servlet.http.HttpCookie

C:javax.servlet. Cookie

D:javax.servlet.http.HttpCookie

 

12:servletContext对象是由(c)创建的

A:由servlet容器负责创建,对于每个http请求,servlet都会创建一个servletcontext对象

B:由javaweb应用本身负责为自己创建一个servletcontext对象

C:由servlet容器负责创建,对于每个Javaweb应用,在启动时都会创建一个servletcontext对象

D:由用户访问的时候自己创建

 

13:给定一个 Servlet 的doGet方法中的代码片段，如下：

request.setAttribute(“name”,”zhang”);

response.sendRedirect(“http://localhost:8080/servlet/MyServlt”);

那么在 MyServlet中可以使用（ D ）方法把属性 name的值取出来

A:Stringstr=request.getAttribute(“name”);

B:Stringstr=(String)request.getAttribute(“name”);

C:Objectstr=request.getAttribute(“name”);

D:无法取出来

 

14:过滤器使用__才能继续传递到下一个过滤器B

A:request.getRequestDispatcher().forward(request,response);

B:doFilter()

C:doPut()

D:doChain()

 

15:以下代码能否编译通过，假如能编译通过，运行时得到什么输出结果A

<%

request.setAttribute("count",new Integer(0));

Integer count =request.getAttribute("count") ;

%>

<%=count %>

A:编译不通过

B:可以编译运行，输出0

C:编译通过，但运行时抛出ClassCastException

D:可以编译通过，但运行无输出

 

16:现在session中没有任何属性，阅读下面2个JSP中的代码，将分别输出

<%

out.println(session.getAttribute("svse"));

%>

<%

session.invalidate();

out.println(session.getAttribute("svse"));

%>

A:null, 异常信息

B:null, null

C:异常信息，异常信息

D:异常信息，null

 

17:Servlet 可以在以下（  ）三个不同的作用域存储数据A

A:请求、会话和上下文

B:响应、会话和上下文

C:请求、响应和会话

D:请求、响应和上下文

 

18:以下哪个参数不属于c:forEach标签 (  )D

A:var

B:begin

C:end

D:delims

 

19:http是一个(  )协议A

A:无状态

B:有状态

C:状态良好的

D:万维网

 

20:http://localhost:8080/web/show.jsp?name=svse下列取得请求参数值正确的是A

A:{param.name}

B:{name}

C:{parameter.name}

D:{param.get("name")}


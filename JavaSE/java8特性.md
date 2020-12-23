虽然现在jdk已经已经到了14【截至2020-08-15】，但是jdk8仍然是使用**最广**的版本，我写了一些常用的，主要是公司用Java8其实还比较多，而且学Java以来，也没有研究过版本的问题，这种只是还是有必要了解一下的，然后靠实战熟练应用

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/593522b00b9741c29ed0c14faf23a243~tplv-k3u1fbpfcp-zoom-1.image)

## 1、lambda表达式

要说jdk8有什么新特性，首先想到的也就是lambda表达式了，又叫函数式编程。作用总结为一句话**简化匿名内部类的写法**

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa9193fc874a40fe83704cc4ad502bdd~tplv-k3u1fbpfcp-zoom-1.image)

> 在线程那一块应该深有体会

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/889b832942b543d9998f59b36bc84f6e~tplv-k3u1fbpfcp-zoom-1.image)

> lambda表达式写法

经过对比，是不是感觉方便许多。在举个经常用的例子，**上面是无参的，下面来个带参**的

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7ece444b0f64e73bf1f16e00c10062f~tplv-k3u1fbpfcp-zoom-1.image)

> 先定义方法，实现方法一样，写法不一样

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/941510c049a8463eb7982b16027f814c~tplv-k3u1fbpfcp-zoom-1.image)

## 2、函数式接口

使用 @FunctionalInterface 标识，**有且仅有一个抽象方法**，可被隐式转换为 lambda 表达式。这个的作用就是，你接口这么写了，就也可以用lambda表达式了。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/30951612bb884452ac673f034e271afe~tplv-k3u1fbpfcp-zoom-1.image)

定义接口方法

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bb8c799c309644a189b63a7555a2f326~tplv-k3u1fbpfcp-zoom-1.image)

## 3、接口默认方法

Java8之前接口的耦合度大，如果改变接口中的方法，那么就得修改所有实现类的方法。所谓**牵一发而动全身**。Java8提供了一个新特性可以解决该问题。可定义default修饰的默认方法。下图Main就算实现Interface接口也不需要实现foo方法

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a6e757e7cc1344158bc8df515d8a291e~tplv-k3u1fbpfcp-zoom-1.image)

好处：默认方法可以为接口添加新的方法，而不会破坏已有的接口的实现。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d531769d5e354ec581e64e676d06514d~tplv-k3u1fbpfcp-zoom-1.image)

可见就算没重写foo()方法也不会编译报错

## 4、引用

在说第一个lambda表达式的时候有一个简便写法，就是使用了引用，没发现的可以上去看看。方法引用使用一对冒号 :: 可以使语言的构造更紧凑简洁，减少冗余代码。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5ef5bc1f242f421a9bbc8dcf692986c2~tplv-k3u1fbpfcp-zoom-1.image)

## 5、日期

之前的日期方法Date有很多问题

(1)非线程安全，想研究为什么的可以网上找下资料，这里就不阐述了。

在阿里巴巴Java开发手册的第一章第六节——并发处理中也有明确说明

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5560efc3baa44feb931ca67d9282b210~tplv-k3u1fbpfcp-zoom-1.image)

(2)设计差，Date日期类不止一个包

(3)时间处理麻烦，大多情况下还得搭配SimpleDateFormat类使用

(4)没有国际化，不提供时区支持

**白话：当时谁也没想到Java可以发展的这么好，当初设计的时候没想到这么多，所以导致设计不合理，现在用新的就对了**

作为弥补在Java8中增加了java.time包。这个包涵盖了所有处理日期，时间，日期/时间，时区，时刻（instants），过程（during）与时钟（clock）的操作。Local(本地) − 简化了日期时间的处理，没有时区的问题。**总之就是很牛逼，比Date好用。**下面是常用的API

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8f84265d8fde45e295f7bc4c97e52e9f~tplv-k3u1fbpfcp-zoom-1.image)

Local(本地) − 简化了日期时间的处理，没有时区的问题。Zoned(时区) − 通过制定的时区处理日期时间

Java8的新特性其实还有许多，比如**Stream API、Optional 类、Nashorn, JavaScript 引擎**等，如果想研究的不妨网上在找找资料，我就总结这么多吧，如果有问题请在评论区指正，我会持续更新一些干货，如果帮到你了就点个关注吧~

------------------------------------- 更新线 9/3 -------------------------------

这次更Stream，之前以为Stream是什么IO流，平时也没用过，所以觉得难学，难懂

Stream是Java8引入的全新概念，**它用来处理集合中的数据**，也可以理解为一种高级集合。

Java8之前，对集合中的数据进行处理是一件非常棘手的事情，也不是因为代码难写，而是因为代码繁琐。stream只需要告诉它怎么处理，它就会自动，把结果返回给你，我们也无需编写复杂又容易出错的多线程代码了。

## 前情回顾

> 今天偶然看见Java8的stream，气的一拍大腿。前两天需求刚好用，如果当时我stream提前学的话，代码能简洁不少。那个需求是这样的：前端需要用到关于地铁费的value值，偷懒将数值写死的话也可以运行，但随之而来的是魔法值的问题，而且如果数据库对应的值改变，那么是肯定不行的（虽说数据库的值也不会轻易改变），但这其实是很不好的习惯，我们应该杜绝这种现象。然后我又在后端处理这些数据，从数据库按地铁费查出来对应的值。存到集合中，最后遍历与前端传过来的值比较，存在返回前端true，否则false。虽然代码也不多，但如果换成stream的话就香多了。人的本性有一个就是惰性，有更简单的为嘛不用。接下来，就看看神奇的Stream吧

在JAVA8之前的传统编程方式，如果我们需要操作一个集合数据，就如我上述情况，则需要使用集合提供的API，通过一个循环去获取集合的元素，这种访问数据的方式会使代码显得臃肿，JAVA8新引入的Stream类，用于重新封装集合数据，通过使用流式Stream代替常用集合数组、list和map的遍历操作可以极大的提高效率

stream在我理解看来就像是一个工具一样，可以帮我们分析处理数据，极其的好用，其效率在数据量比较庞大的时候，Stream可以为我们节省大量的时间，数据量小的时候并不明显。

写一段伪代码(按照我上述需求)

```
List<对象类型> lists = 返回的list集合
//Java8之前遍历
for (int i = 0, length = lists.size(); i < length; i++) {
    数据库的值 = subwayValue.get(i).getValue();
    if(Integer.valueOf(前端的值).equals(数据库的值)){
        flag = true;
	break;
     }
}
//现在遍历，只需一行,返回值类型为boolean
flag = lists.stream().noneMatch(i -> i.getValue().equals('前端的值'));
```

除此之外，stream还有许多好用的API（各举一个，主要还是得靠自己研究）

筛选和切片

筛选岁数大于20的人

```
list.stream().filter(item -> iteam.getAge()>20).forEach(System.out:println);
```

筛选前5条数据

```
list.stream().limit(5).forEach(System.out:println);
```

排序

```
List<Integer> list = ArrayList.asList(一堆没有顺序的数字);
Stream<Integer> stream = list.stream();
stream.sorted().forEach(System.out:println);
//对象排序
对象集合.stream().sorted((e1,e2) -> Integer
                             .compare(e1.getAge(),e2.getAge()))
                             .forEach(System.out:println);
```

匹配和查找

```
//其中上面的第一个例子就是匹配
//查找所有人的数量
list.stream().count()
```

归约

```
//计算所有数的总和
List<Integer> list = ArrayList.asList(一堆数字);
list.stream().reduce(0,Integer::sum)
//计算对象值的总和
List<对象类型> list = 返回对象的方法;
list.stream().map(对象类型::getValue).reduce(数值类型::sum)
```

收集

```
//返回一个list
List<对象类型> list = 返回对象的方法;
List<对象类型> listStream = list.stream()
                    .filter(e -> e.getValue()>20)
                    .collect(Collectors.toList());
//返回一个set
List<对象类型> list = 返回对象的方法;
List<对象类型> listStream = list.stream()
                    .filter(e -> e.getValue()>20)
                    .collect(Collectors.toSet());
```



重要的不是人家有什么，而是你会用什么
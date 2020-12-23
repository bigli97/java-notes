## 前言

vue第一次接触的时候是在我个人毕设中，前端框架这块当时是bootstrap+vue。当时也没用到复杂的东西，比如webpack和cli这些，这次公司项目中接触到vue，看见一些脚手架之类的东西，认为还是有必要看一下的，在之前到公司之前也就零零碎碎开始了解了，借此机会整理有关vue的内容

## 这篇文章的意义

我只是提到了vue应该学什么内容，哪些内容是重要的，去哪学，怎么学效率高，经验总是别人的，如果你想真正学会什么东西，还需要静下心来学习，学习不能速成，它是一种积累。

## 前言酸溜溜不如看正文

vue是一个前端框架，类似Java中的spring框架。回到主题，我当时学习spring框架的时候首先是系统性的学习了一遍，在之后就是利用spring框架独立搞了几个项目出来。也算是spring框架的入门了。不过也花费了不少时间，对于时间紧张的打工人来说，作为一个后端不可能跟spring一样系统性的学一遍。但也不能不学，真指望用jq行走天下吗？**但咱搞后端的凭什么要明白前端框架？**其实干IT这行也没必要分那么清，就算你现在完全不碰前端，也不保证以后不碰吧，干IT，注苦逼。**进了这个行业就注定要不断学习**，并且vue作为前端最热门的框架，我认为还是有必要了解的。

## 后端程序员学到什么程度？

这个应该就是接下来的问题了，关于这个问题网上给出的并不多，我把我理解的分享出来，当然掌握越多越好

- 起码vue的代码到我手里我能看懂
- 熟悉掌握一些项目中常用技术比如双向绑定，事件调用，异步传输那些
- 理解组件化开发，前端模块化，webpack，cli
- 项目可以不做，但你要门清

## 为什么学vue，jq不香吗？

等你学了vue，jq是真的不香。vue的最大特点：**数据和页面解耦。**你能想象jq的$冲突是什么感觉吗，还查不出原因呢的那种。反正我是遇到过，这样看来vue的块级控制完全没有这种顾虑

## 我没有用vue做过项目，怎么学效率高？

很多人都是认可在项目中学习，你没做过，没关系，我做过呀。所以我也会告诉你vue需要学的一些核心东西，如果你想系统性的学习，推荐b站，我目前以前看一半了(90来集),他讲的细，所以干货你得自己总结，我就经常看视频打瞌睡，爱，回到了高中的感觉。学习就是这么枯燥且无味

## vue项目核心技术有哪些？

当时我用的springboot做了一个在线看小说的系统，前端用的bootstrap+vue

当时vue主要比较多的就是双向绑定、异步获取数据、将数据遍历展现到页面、事件的绑定与监听

我现在接手的这个项目中涉及的很全，有单独的一个vue前端体系，等我吃透了分享出来

## 双向绑定必会

什么是双向绑定，你绑个对象其中的值，在input框显示出来，改哪边，另一边也会变，这就是双向绑定。

## 双向绑定的原理MVVM

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3d5e75fb79634aee91d2f2272f753d68~tplv-k3u1fbpfcp-zoom-1.image)

MVVM全名Model-View-ViewModel
1、M表示抽离出的obj(new Vue)
2、V表示DOM(html + css)
3、VM表示监听M修改V，是双向绑定的要点

## MVVM怎么工作的？

ViewModel通过bind让obj中的dom能够实时显示，在通过listener监听dom事件，通过method来修改数据
**说白了也就是监听+绑定实现的**

## vue对象中的实例

**el**：类型string，决定你管理哪一块dom，这也是vue的一个优势点，整个代码按块管互不影响。前两天开发需求的时候我引入JQuery使用AJAX出现了$冲突的情况，那么多的代码根本也找不出冲突的点，也不知道冲突的原因，虽然最后通过换命名解决，但处理这种问题也会降低工作效率。

**data**：类型object，vue中需要存放的数据对象，这里可以存放的对象类型也是比较丰富的

**method**：类型function，定义一些方法，可以指令调用，也可以在其他地方调用

### 钩子函数



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/12c020da279e4eb493f7e7b19046ff13~tplv-k3u1fbpfcp-zoom-1.image)

也就是**生命周期**，不论是谁的生命周期也都是从创建到销毁，之前的servlet，线程之类的也都一样，所以东西学多了，感觉很多都是相通的。通过钩子函数可以了解到元素分别在不同的函数中加载情况，这样的话可以自定义函数选择在那个生命周期中加载。（自己写下相关代码会更清楚）

beforecreated：el和data均未加载

created：data完成初始化，el没有

beforemount：完成el和data初始化

mounted：完成挂载

## 计算方法computed

如果在进行前端数据**计算的时候推荐使用computed**。相较于methods，前者**只在缓存加载一次**，性能更高，相当于静态属性。

## let和var

总结一句话：**以后用let不要用var**
细细道来：事实上var的设计可以看成JavaScript语言设计上的错误，这种错误多半不能修复和移除，有个人修复了这个问题，他添加了一个关键字：let，所以可以将let看作更完美的var

## 为什么不推荐用var？（在细点）

js没有像Java一样是强类型语言，所以涉及一个作用域的问题。Java中可以在for循环中每次都int i，但var i不行。

js中使用var来证明一个变量，作用域主要和函数的定义有关

针对于其他定义来说是没有作用域的，比如if，for，所以开发中会遇到一些问题，别入下图永远都是最后一个按钮被点击

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4ab7ef9d741d410c8068f7c31fa335d4~tplv-k3u1fbpfcp-zoom-1.image)

虽然说闭包可以解决作用域的问题，但是会影响工作效率，**程序员应该把时间用到业务逻辑上，不应该承担语言所带来的缺陷。**



### const修饰变量

const也就**相当于Java中的final**，不可修改，必须初始化。一般用于定义Vue对象，和一些常量。其中的标识(zhi)符不能改变其中指向，能改变指向的值。经常使用的const app = new Vue(){}



### es6属性的简写

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/99cb668134f146908ae86a5aba74faab~tplv-k3u1fbpfcp-zoom-1.image)

**什么时候方法的()可以省略**

必须满足两个条件，类似下图

1、事件监听

2、方法没有参数

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2dcdf6e57ef43778f881944e733f10b~tplv-k3u1fbpfcp-zoom-1.image)

当条件为false的时候，v-if不会存在dom中，v-show会出现，加了一个样式dispaly：none。就是F12查看的时候有没有
使用建议：**如果频繁切换使用v-show，只切换一次v-if**

let i **in** 遍历的对象，此时i是当前**索引**
let i **of** 遍历的对象，此时i是当前**对象**

### 如果需要以下三个需求应该怎么写代码

1. 筛选数组中小于100的数字
2. 将筛选出的结果x2
3. 将结果进行相加

最直接的方法就是套的写，官方叫做**链式编程**，也叫**函数式编程**

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/496b2973952247408712f0bb083a3919~tplv-k3u1fbpfcp-zoom-1.image)

如果了解过Java8新特性的也可以采用**[lambda编程](https://juejin.cn/post/6907436136780546062)**，更加简洁明了

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f671524860b4410a0cae2d794a6b902~tplv-k3u1fbpfcp-zoom-1.image)

## 组件化开发部分

### 什么是组件化？

如果人面对复杂问题的时候，我们的处理方式：
1、任何人的处理信息逻辑的能力都是**有限的**，所以在面临非常复杂的问题的时候，不太可能搞定
2、人天生有一种能力就是**将问题去拆分**，在项目管理中WBS是一个重要的内容。思想其实是一致的。那就是将一个复杂的问题，拆分成很多小问题去处理，那么再大的问题只要分的足够细化，也就迎刃而解了

**组件化也是类似的思想**
1、如果将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得十分复杂，通过这两天做需求也是能感觉出来，jsp中不仅包括Java代码和html代码，还包括css样式，js函数，都在一个页面，找起来麻烦，代码观赏性也不佳。非常不利于后续的管理和扩展。
2、如果能将一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么后续的管理与维护就非常容易了。

**组件不能直接访问vue中的实例，组件中的数据必须传function类型，不能是对象类型。**当时设计的时候就不允许，都放到vue实例中，那么vue实例就会变得非常臃肿。vue组件有自己保存数据的地方---template

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4233589847b04536a1ebe66eeeaff80e~tplv-k3u1fbpfcp-zoom-1.image)

### 为什么data中需要传一个函数？

假如有一种情况，你定义了一个计数器组件，点击增加或者减少**互不影响**，为什么，这时候函数的作用就体现出来了，每一次使用组件的时候总会调用一个return去返回一个新的对象，所以是互不影响，这也是为什么使用函数的原因。

## 父子组件的通信

父通过**props**向子传递

子通过**事件**向父发送信息

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38b4e686a8d242a7b224b6dd0d8f98b5~tplv-k3u1fbpfcp-zoom-1.image)

### 什么是webpack？

前端模块化打包工具。关键字：**打包**

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/86a6fdc6caf34612853a0026c1459013~tplv-k3u1fbpfcp-zoom-1.image)



### 什么是模块化？

有组织的把一个大文件拆成独立并互相依赖的小模块。就是**大文件拆成小文件**
模块内部有许多私有属性，只向外暴露一部分公开的接口(如可以修改私有属性的方法等)



### 模块化的好处？

- 避免命名冲突(减少命名空间污染)
- 更好的分离, 按需加载
- 更高复用性
- 高可维护性



### 什么是打包？

打包工具可以让我们在开发网页的时候使用import export require，像后端程序员（说的正是在下）那样进行模块化开发。



目标就是**打包成让浏览器能读懂的语言**，支持模块化
处理关系依赖网



### 什么时候用gulp？

工程简单没有用到模块化
只需要简单的合并压缩





### 与webpack的区别？

gulp强调的是前端流程的自动化，模块化不是它的核心
webpack更强调模块化开发管理，文件压缩合并是附带的功能



webpack依赖node环境

webpack只需要打一个文件就可以了，会自动处理依赖文件，本来浏览器是不支持的，webpack自己实现了js代码

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d8534b340d6b49e7b403759f1c1feb46~tplv-k3u1fbpfcp-zoom-1.image)

基本使用指令：`webpack ./src/main.js ./dist`

简单命令：webpack，它会自动去找webpack.config.js，内容如下

```
module.exports = {
         //入口文件 
         entry:'./main.js', 
         //出口 
         output:{ path:__dirname, filename:'build.js'//打包之后的文件,html模板中引入此文件 }, };
```

平常使用npm打包一般不使用上面的，打包指令：`npm run build`

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9b2799bf6eca4eccbcc7d8769120fe5f~tplv-k3u1fbpfcp-zoom-1.image)

<script src="" />不是模块化思想，那么如何解耦

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d786781ad754cd3bb11b044439c887a~tplv-k3u1fbpfcp-zoom-1.image)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d67890cdd104f60b89b9940802f1903~tplv-k3u1fbpfcp-zoom-1.image)

将html部分，css部分，js部分抽离到vue文件中

```
<template> //html </template>
<style> //css</style>
<script>//js</script>
```

**简单的目录结构**

> -index.html
> -main.js 入口文件
> -App.vue vue文件
> -package.json 工程文件(项目依赖、名称、配置)，npm init –yes 生成此文件
> -webpack.config.js webpack的配置文件

webpack的又一核心plugin



### plugin与loader的区别？

loader主要用于转换某类型的模块，是一个转换器
plugin是插件，是对webpack本身的扩展，是一个扩展器





### plugin的使用步骤

1、通过npm安装（某些内置的不需要安装）
2、在中webpack.config.js的plugins配置插件



## 脚手架部分

### 什么是cli

在使用vue开发大型应用时，需要考虑代码目录结构，项目结构和部署，热加载，代码单元测试等

脚手架的作用就是**帮助我们去完成需要手动完成的工作**，可快速搭建webpack配置

**使用前提---Node**

安装node.js要求8.9以上

NPM node包管理工具

**使用了webpack模板**

会对所有资源进行压缩优化操作

开发过程中提供了一套完整的功能，能够使我们开发过程更加高效

npm install webpack -g 全局安装webpack

**使用**

安装脚手架(3的安装方式)

npm install -g @vue/cli

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38ca73c49624438d8467463b19039bcb~tplv-k3u1fbpfcp-zoom-1.image)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7a3580fdd2a4c0d8cebf64215d029e8~tplv-k3u1fbpfcp-zoom-1.image)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c517e0ef489431a87381b6d9c42918e~tplv-k3u1fbpfcp-zoom-1.image)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59114c455bc94f19a3dc1ba1b2335582~tplv-k3u1fbpfcp-zoom-1.image)

runtime+compiler和runtime-only（推荐）应该怎么选择

**vue运行过程**

template--->解析成 ast(抽象语法树)--->编译成 render --->转成 虚拟dom --->转成真实dom---UI

runtime-only直接就到了render阶段，所以快，性能更高，代码量更少

## 结语

要学的还有很多，能更好利用时间的方式就是将来在项目中去学习，等我做完这个新项目后再来分享关于vue的一些干货，可能到时候视野又会不一样吧
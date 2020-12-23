有句话一直是程序猿的梗：最烦不写文档注释的人，也最讨厌写文档注释

## 这个框架有什么好处，为什么要学它？

1、为前端**提供接口数据**。在前后端分类的项目中，前后端数据同步问题一直是一个头疼的问题，导致前后端人员经常意见不合，撕b甚至大打出手

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/72b2cd5700114a919e0515771ee963bb~tplv-k3u1fbpfcp-zoom-1.image)

2、**实时更新文档**。在我们增加一个字段或者改变某些信息的时候总是没有实时更新文档的习惯，导致后期维护造成很大的困扰，占用不必要的工作时间（可以清楚看到各个接口及含义，后端改，实时同步）

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/34a9a7b999994d82abc3d4b1131c0ec2~tplv-k3u1fbpfcp-zoom-1.image)

3、可以**在线测试**。这个功能相当于postman，可以自己模拟参数，然后测试。

输入具体的参数后，点击Execute运行测试

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/26564e6d47d544708aa8785bfa51ca8a~tplv-k3u1fbpfcp-zoom-1.image)

Swagger2的出现就是为了从根本上解决上述问题。它作为一个规范和完整的框架，可以用于生成、描述、调用和可视化 RESTful 风格的 Web 服务

## 好处说完了，怎么使用呢？

其实很简单，**引入一个pom依赖，写一个配置类就可以使用了**。下面为实操部分

1、首先创建一个spring boot项目

这个就不详细展开了，IDEA有专门的模板，maven目录结构就可以

2、引入pom依赖

```
<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger2</artifactId>
<version>2.9.2</version>
</dependency>
 
<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger-ui</artifactId>
<version>2.9.2</version>
</dependency>
```

3、编写配置为swagger2，使用注解@EnableSwagger2

```
@EnableSwagger2
@Configuration
publicclassSwagger2Config{
 }
```

4、访问地址

http://localhost:9000/swagger-ui.html

localhost:9000

页面就出来了， 看似没有配置，实际使用的为默认配置

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/480e8d9bf8dc4f6fba503a5986a5834e~tplv-k3u1fbpfcp-zoom-1.image)

到这里只代表这个UI和你的代码联系起来了，至于怎么去显示，显示哪部分，还需要你具体进行配置

**首先可以先验证下注释怎么生成的**

首先新建一个pojo类，我用的User类

```
package swagger.demo.pojo;


import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

/**
 * @author dali
 * @date 2020-12-02 16:38
 * @Description
 */

public class User {
    @ApiModelProperty("姓名")
    private String name;
    @ApiModelProperty("电话")
    private String phone;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }
}
```

因为它默认扫描为全部的包，重启启动后可以看见如图

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/89ea94d9f3024b918ff1c4a7c9df46a6~tplv-k3u1fbpfcp-zoom-1.image)

这里面的注解都是@APIxxx,感兴趣的可以一一测试

> @Api：修饰整个类，描述Controller的作用
> @ApiOperation：描述一个类的一个方法或者说一个接口
> @ApiParam：单个参数描述
> @ApiModel：用对象来接收参数
> @ApiProperty：用对象接收参数时，描述对象的一个字段
> @ApiResponse：HTTP响应其中1个描述
> @ApiResponses：HTTP响应整体描述
> @ApiIgnore：使用该注解忽略这个API
> @ApiError ：发生错误返回的信息
> @ApiImplicitParam：描述一个请求参数，可以配置参数的中文含义，还可以给参数设置默认值
> @ApiImplicitParams：描述由多个
> @ApiImplicitParam 注解的参数组成的请求参数列表

### 我连注释都不想写，@ApiModelProperty("原来注释")前面加个注解我难道会更想写？

其实此言差矣，更多时候只是不想去实时更新注释，在第一次编写开发的时候，往往不会有这种“道德低下的人”。即使你不想写， 公司往往会进行代码审查，你也躲不过，所以只是一个习惯问题，适应适应就好了。



### 下面说一下，配置类中如何进行自定义配置

```
package swagger.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.env.Environment;
import org.springframework.core.env.Profiles;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

/**
 * @author dali
 * @date 2020-12-02 15:08
 * @Description
 */
@Configuration
@EnableSwagger2
public class Swagger2Config {
    //配置swagger的实例
    @Bean
    public Docket docket(Environment environment){
        return new Docket(DocumentationType.SWAGGER_2)
                //这个可以自定义页面出现的标题，描述等信息
                .apiInfo(this.apiInfo())
                //是否开启swagger2，可以用作环境判断使用
                .enable(true)
                //筛选
                .select()
                //扫描的包
                .apis(RequestHandlerSelectors.basePackage("swagger.demo.controller"))
                //构建项目
                .build();
    }


    public ApiInfo apiInfo(){
        Contact contact = new Contact("bigli","url","123@qq.com");
        return new ApiInfo("标题",
                "描述",
                "v1.0",
                "urn:tos",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList());
    }
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a417df2b3be442b28f54d79375b8d655~tplv-k3u1fbpfcp-zoom-1.image)

> 其中的apiInfo这个方法改变的是这块内容

需要写Docket这个方法来实现自定义配置，我只是使用了一部分配置，可以自行探索，**主要应该学会举一反三，到具体项目中在变动**

## 小结

swagger2就**相当于一个实时同步文档**，给前端提供接口的插件。将平常写注释前面加个注解就可以了，非常的方便，但是生产环境为了安全和运行效率，需要关闭

### 思考一个问题，我们该如何让他在生产环境中关闭，在开发或者其他环境中开启？

1、定义环境，在配置文件中。以[application.propertie](http://application.properties/)s为例

```
spring.profiles.active=dev
```

2、获取到当前环境

```
Profiles profile = Profiles.of("dev");
```

3、用当前获取的环境判断有没有

```
boolean b = environment.acceptsProfiles(profile);
```

4、传入enable方法中
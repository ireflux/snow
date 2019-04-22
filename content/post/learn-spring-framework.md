---
title: "Learn Spring Framework"
date: 2018-3-18T23:34:00+8:00
lastmod: 2018-3-18T23:34:00+8:00
draft: false
categories: ["Java"]
tags: ["Spring学习之路"]
author: "sherry"
---
# 前言

Recently,I was learning the Spring Framework.

At first, I learn it from Spring official website of the guide. When I completed the first few guides, I find that I didn’t even understand why. So I had to give up.

After that, I looked for other guides to learn and saw many examples of IOC(Inversion of Control), DI(Dependency Injection), and AOP(Aspect Oriented Programming). I probably understand, but I still can't use the Spirng framework. on the forum, Someone told me that I only need more practice. But I don't even know it how to work, e.g. annotation.

<!--more-->

Obviously, I encountered resistance.

One day, I searched "Why is learning Spring Framework so hard" on search engines, I found the answer in quora. The high-vote answer is very good and inspiring. So I decided to relearn the spring framework from the official website. Just like a ring. Three months later, I returned to the origin.

Thanks for that article again!

- - -

以下是可能会用到的资料：

Spring Framework Documentation
> https://docs.spring.io/spring/docs/

Spring官方建议使用Maven、Gradle之类的包管理工具，因此官网上隐藏了下载地址，不过还是可以通过链接访问的

Spring-framework-release，截止博文发表之前，版本号为spring-framework-5.0.4.RELEASE
> https://repo.spring.io/release/org/springframework/spring/

Spring核心容器依赖于Common-logging的JAR包，下载地址如下：
> https://commons.apache.org/proper/commons-logging/download_logging.cgi

## 原文摘录

__原文链接:__ http://springtutorials.com/spring-tutorial-1/

> A simple question for you before I begin:
>
> Can you count from 1 to 20?
>
> Here, Let me help you. Go ahead and read it out loud.
>
> One. Two. Three. Four. Five. Six. Seven. Eight. Nine. Ten.
> Eleven. Twelve. Thirteen. Fourteen. Fifteen. Sixteen. Seventeen. Eighteen. Nineteen. Twenty.
>
> Do you see any pattern in the numbers above? Anything that rhymes with the words or the way you count them?
>
> Some of these end with ‘teen’ in them but is there anything else?
>
> Unlearn everything you know about numbers and see if you can spot anything that will help you see it?
>
> (Hint: There is none)
>
> My daughter was 3 (she is 7 now) when she had to go through this exercise and she found it very challenging.
>
> I think she was 4 when she started to count a little more…
>
> Twenty One. Twenty Two. Twenty Three. Twenty Four. Twenty Five. Twenty Six. Twenty Seven. Twenty Eight. Twenty Nine. Thirty.
>
> Did she see any pattern now? Of course, she did. ‘Twenty‘ seemed to show up everywhere.
>
> Counting from thirty to forty was not any different now. She was over the learning curve and needed few corrections here and there.
>
> She spent almost a full year learning to count from 1 to 20.
> But she spent only a few months going from 20 to 100.
>
> Let me clarify this. Counting from 1 to 20 is the hardest thing you can do.
>
> There is no pattern here.
>
> You have to learn it the hard way. Get over this curve and life becomes easier.
>
> Take that as a big lesson in life. Start anything and you have to go through the grind. Once you are past the initial struggle, things get easier.
>
> Can you predict what I am going to say next:
>
> Learning Spring Framework (or anything new) is no different from learning to count.
>
> It is challenging at first but becomes easier as you progress. You will make mistakes. We all do. Have some patience.
>
> As a kid, It took me a while to learn the colors of the rainbow in its correct order. And then my best friend gave me a pattern – ROYGBIV or VIBGYOR. I still remember it decades later.
>
> As a Spring Developer, you want to learn why things work the way they work. The syntax, the annotations, the examples will become easier to understand that way. A pattern will emerge sooner or later from the initial randomness.
>
> It is worth the effort to go through this randomness. A good foundation is the core of any learning. Find your own randomness.

### Building a RESTful Web Service

**UPDATE:** 初学的时候不懂，感觉有点奇怪，原来这是Spring boot里的内容 =。=

UPDATE(2018.12.28)：现在再回过头来看，纯粹达到会使用的话也没那么复杂，真是难了不会，会了不难，原本想要做成一个学习系列，用来督促学习的，现在看来当初的计划也失败了，就这样结束吧。

哦对了，下文基本上是官方文档的英译中。嗯，就这样吧。

源文档：https://spring.io/guides/gs/rest-service/

部分代码摘录如下：

#### src/main/java/hello/Greeting.java

```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```

#### src/main/java/hello/GreetingController.java

```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```

1. `@RequestMapping`注解确保对/greeting的HTTP请求映射到greeting()方法中，默认映射所有HTTP操作，所以即使不指定GET，PUT还是POST也没关系.若想指定映射方式可使用`@RequestMapping(method=GET)`的方式.

2. `@RequestParam`将查询字符串`name`的值绑定到`greeting()`方法的`name`参数中.查询字符参数为可选,默认为`required=true`.

3. 传统的MVC控制器和上面的RESTful Web服务控制器之间的一个主要区别在于HTTP响应主体的创建方式.这个RESTful Web服务控制器只需填充并返回一个Greeting对象,而不是依赖视图技术将问候数据的服务器端呈现呈现给HTML.对象数据将作为JSON直接写入HTTP响应.

4. `@RestController`为Spring 4的新注解,它将类标记为控制器,其中每个方法都返回一个域对象而不是视图.它是`@Controller`和`@ResponseBody`的缩写.

#### src/main/java/hello/Application.java

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

`@SpringBootApplication`是一个方便的注释，它增加了以下所有内容：

- `@Configuration`将类标记为应用程序上下文的bean定义的来源。

- `@EnableAutoConfiguration`通知Spring Boot根据类路径设置，其他bean和各种属性设置开始添加bean。

- 通常你会为Spring MVC应用程序添加`@EnableWebMvc`，但Spring Boot在类路径中看到spring-webmvc时会自动添加。 这将该应用程序标记为Web应用程序并激活关键行为，例如设置DispatcherServlet。

- `@ComponentScan`告诉Spring在hello包中查找其他组件，配置和服务，以便找到控制器。

### 测试

打包：
> mvn compile
>
> mvn package

测试运行：
> java -jar gs-rest-service-0.1.0.jar

访问：http://localhost:8080/greeting

或者加入参数：http://localhost:8080/greeting?name=User
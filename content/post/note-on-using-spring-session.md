---
title: "Spring Session 使用笔记"
date: 2019-11-23
lastmod: 2019-11-23
draft: false
categories: ["Clavicula Salomonis"]
tags: ["Spring Session","Spring"]
author: "sherry"
---
Spring Session 的配置非常简单，根据[官方文档](https://docs.spring.io/spring-session/docs/current/reference/html5/guides/boot-redis.html)的说明，只需配置依赖及配置文件即可使用，无需任何代码相关的配置项。

首先需要添加依赖项:

```xml
<dependencies>
	<!-- ... -->

	<dependency>
		<groupId>org.springframework.session</groupId>
		<artifactId>spring-session-data-redis</artifactId>
	</dependency>
</dependencies>
```

<!--more-->

然后在 application.properties 中添加以下内容：

```properties

spring.redis.host=localhost # Redis server host.
spring.redis.password= # Login password of the redis server.
spring.redis.port=6379 # Redis server port.

spring.session.store-type=redis # Session store type.
```

至此就配置完成了，现在就可以写两个简单的方法去测试一下。例如以下：

```java
    @GetMapping(value = "/set")
    public String setSession(HttpSession session){
        session.setAttribute("username","sherry");

        return "ok";
    }

    @GetMapping(value = "/get")
    public String getSession(HttpSession session){
        return (String) session.getAttribute("username");
    }
```

然后使用 maven package 将其打成 jar 包，若需要跳过测试，可使用 `mvn -Dmaven.test.skip=true package`，或者在 pom.xml 中添加插件依赖：

```xml
<plugins>
    <!-- ... -->

	<plugin>
	    <groupId>org.apache.maven.plugins</groupId>
		    <artifactId>maven-surefire-plugin</artifactId>
		    <configuration>
			    <skipTests>true</skipTests>
		    </configuration>
	</plugin>
</plugins>
```

然后开启两个服务：

```bash
$ nohup java -jar app.jar --server.port=8080 &
$ nohup java -jar app.jar --server.port=8081 &
```

现在在 8080 里面 set，在 8081 里面 get，会发现也可以得到结果。

这里顺便歪个楼，如果服务在前台启动，比如说启动 redis 时未将 redis.conf 中的 daemonize 设置为 yes。此时可以使用 `CTRL + Z` 将其挂在后台，但此时服务会被暂停，使用 `fg` 可以将其调回前台继续运行(若后台有多个，默认调回最后启动的那个)。也可以使用 `bg` 使其后台继续运行。使用 `jobs` 可查看在后台运行的服务列表。

到此为止就算完成了，但还可以更进一步，在这两个服务前面加一层 Nginx 来做反向代理，还可以设置权重。首先来修改 nginx 的配置文件，一般来说需要修改 nginx.conf，由于我现在的环境是 debian，有一个默认的配置文件位于 /etc/nginx/sites-enabled 下的 default 文件，不同的发行版文件目录结构可能不大一样，可以根据自身情况自行查找。可根据以下示例来写：

```conf
upstream wanmei.ml{
    server 127.0.0.1:8080 weight=1;
    server 127.0.0.1:8081 weight=2;
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name localhost;
    location / {
        proxy_pass http://wanmei.ml;
        proxy_redirect default;
    }
}
```

然后 `nginx -s reload` 重新加载配置文件。现在再来访问此服务器的 ip 地址，流量就会经过 nginx 了，可以查看位于 /var/log/nginx 下的 access.log 日志进行确认。

## 参考资料

1. [nginx documentation](http://nginx.org/en/docs/)  
2. [扫盲 Linux＆UNIX 命令行——从“电传打字机”聊到“shell 脚本编程”](https://program-think.blogspot.com/2019/11/POSIX-TUI-from-TTY-to-Shell-Programming.html)
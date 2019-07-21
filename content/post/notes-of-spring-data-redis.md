---
title: "Spring Data Redis 学习笔记" 
date: 2019-07-21
lastmod: 2019-07-21
draft: false
categories: ["Clavicula Salomonis"]
tags: ["spring"]
author: "sherry"
---
Redis官网上有一些推荐的 redis client，在 Java 编程语言分类下，截至发文前，有三个 client 被官方推荐，分别是：[Jedis](https://github.com/xetorthio/jedis)， [lettuce](https://github.com/lettuce-io/lettuce-core)， [Redisson](https://github.com/mrniko/redisson)， Spring Boot 2.X 中默认集成了 lettuce。

以下是[官方](https://github.com/lettuce-io/lettuce-core)对 lettuce 的描述，摘录如下：
> Lettuce is a scalable thread-safe Redis client for synchronous, asynchronous and reactive usage. Multiple threads may share one connection if they avoid blocking and transactional operations such as BLPOP and MULTI/EXEC. Lettuce is built with netty. Supports advanced Redis features such as Sentinel, Cluster, Pipelining, Auto-Reconnect and Redis data models.

<!--more-->

因此可以看出，lettuce 是线程安全的，基于 netty，在不包含阻塞和事务的情况下多个线程还可以共用一个连接。相比之下 Jedis 的实现上是直连的 redis server，多线程环境下是非线程安全的。

言归正传，在开始之前，首先需要在 pom.xml 文件中添加以下依赖：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-pool2</artifactId>
</dependency>
```

其中值得一提的是，如果不添加 commons-pool2，启动 Spring Application 会报错，大意是找不到 commons-pool2 之类的 blahblahblah...可以添加以下配置来解决，但还是建议直接加上这个依赖。

```java
@Bean
public LettuceConnectionFactory redisConnectionFactory(){
    return new LettuceConnectionFactory(new RedisStandaloneConfiguration("ip", 6379));
}
```

连接 redis 及 lettuce pool 的配置：

```properties
spring.redis.database=0
spring.redis.password=
spring.redis.port=6379
spring.redis.host= host ip
spring.redis.lettuce.pool.min-idle=5
spring.redis.lettuce.pool.max-idle=10
spring.redis.lettuce.pool.max-active=8
spring.redis.lettuce.pool.max-wait=1ms
spring.redis.lettuce.shutdown-timeout=100ms
```

redis 中包含的几种数据结构大都在 Spring Data Redis 中能够找到对应，基本上见文知意，不再赘述。以下是我测试时的 Demo，仅供参考：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class RedisDemoApplicationTests {

	@Autowired
	private StringRedisTemplate stringRedisTemplate;

	@Test
	public void contextLoads() {
		ValueOperations<String,String> valueOperations= stringRedisTemplate.opsForValue();
		valueOperations.set("valueOperations","value");
		stringRedisTemplate.expire("valueOperations",100, TimeUnit.SECONDS);
		System.out.println("-------------valueOperations -> key: valueOperations, value: value--------------");
		System.out.println("value:"+valueOperations.get("valueOperations")+"\texpire:"+stringRedisTemplate.getExpire("valueOperations"));

		ListOperations<String,String> listOperations = stringRedisTemplate.opsForList();
		listOperations.leftPushAll("listOperations","list","Operation","s");
		stringRedisTemplate.expire("listOperations",90, TimeUnit.SECONDS);
		System.out.println("-------------listOperations -> key: listOperations, value: list, Operation, s--------------");
		System.out.println("values:"+listOperations.range("listOperations",0,-1)+"\texpire:"+stringRedisTemplate.getExpire("listOperations"));

		HashOperations<String,String,String> hashOperations = stringRedisTemplate.opsForHash();
		hashOperations.put("hashOperations","hash","Operations");
		stringRedisTemplate.expire("hashOperations",80, TimeUnit.SECONDS);
		System.out.println("-------------hashOperations -> key: hashOperations, value: hash: Operation--------------");
		System.out.println("value:"+hashOperations.get("hashOperations","hash")+"\texpire:"+stringRedisTemplate.getExpire("hashOperations"));

		SetOperations<String,String> setOperations = stringRedisTemplate.opsForSet();
		setOperations.add("setOperations","1","2","1");
		stringRedisTemplate.expire("setOperations",70, TimeUnit.SECONDS);
		System.out.println("-------------setOperations -> key: setOperations, value: 1, 2, 1--------------");
		System.out.println("values:"+setOperations.members("setOperations")+"\texpire:"+stringRedisTemplate.getExpire("setOperations"));

		ZSetOperations<String,String> zSetOperations = stringRedisTemplate.opsForZSet();
		zSetOperations.add("zSetOperations","zSet",10);
		stringRedisTemplate.expire("zSetOperations",60, TimeUnit.SECONDS);
		System.out.println("-------------zSetOperations -> key: zSetOperations, value: zSet score: 10--------------");
		System.out.println("value:"+zSetOperations.range("zSetOperations",0,-1)+"\texpire:"+stringRedisTemplate.getExpire("zSetOperations")+"\tscore:"+zSetOperations.score("zSetOperations","zSet"));
	}
}
```

其他方法可参考 Spring Data Redis 的 [API文档](https://docs.spring.io/spring-data/redis/docs/current/api/org/springframework/data/redis/core/RedisTemplate.html)

## 参考资料

[lettuce github](https://github.com/lettuce-io/lettuce-core)  
[RedisTemplate](https://docs.spring.io/spring-data/redis/docs/current/api/org/springframework/data/redis/core/RedisTemplate.html)  
[Spring Data Redis](https://docs.spring.io/spring-data/redis/docs/2.1.9.RELEASE/reference/html/)

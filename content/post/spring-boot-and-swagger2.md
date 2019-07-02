---
title: "Spring Boot 整合 Swagger2" 
date: 2019-07-01
lastmod: 2019-07-01
draft: false
categories: ["Clavicula Salomonis"]
tags: ["Spring"]
author: "sherry"
---
关于 Spring Boot 整合 Swagger2，网络上有很多详细的文章，此文仅作个学习记录。

首先需要引入 Swagger2 的依赖，截止发文前版本号如下：

```xml
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

<!--more-->

然后写一个 Swagger2 的配置类：

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .pathMapping("/")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.demo.spider.controller"))
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
                        .title("Swagger")
                        .description("接口信息……")
                        .version("1.0")
                        .contact(new Contact("sherry's blog","https://wanmei.ml/snow","sherry@wanmei.com"))
                        .license("MIT License")
                        .licenseUrl("https://choosealicense.com/licenses/mit/")
                        .build());
    }
}
```

此时，启动项目，然后进入 `http://[ip]:8080/swagger-ui.html` 应该就可以看到页面了，但是还没有显示任何接口，需要在接口上加上注解以保证 swagger2 能扫描到，如下所示：

```java
@RestController
@Api(tags = "城市相关接口")
@RequestMapping(value = "/city")
public class CitiesController {

    @Autowired
    private ICitiesService citiesService;

    @GetMapping(value = "/list")
    @ApiOperation("查询城市列表")
    List<Cities> getList(){
        return citiesService.getList();
    }

    @GetMapping(value = "/{id}")
    @ApiOperation("根据id查询城市位置信息")
    @ApiImplicitParam(name = "id", value = "编号", defaultValue = "1", required = true)
    Cities getCityById(@PathVariable Integer id){
        return citiesService.getLocationById(id);
    }
}
```

此处涉及到的 API 及解释如下所示：
1. @Api：用于标记 Controller 的功能
2. @ApiOperation：用于标记一个方法的功能。
3. @ApiImplicitParam：用于描述一个参数所代表的意思，默认值及是否必填等相关信息（这里的 `required = true` 只是在文档中表明必填，不会对接口产生作用），若有多个参数，多个 `@ApiImplicitParam` 注解需要放在一个 `@ApiImplicitParams` 注解中

还可以对实体类进行标记：

```java
@Data
@EqualsAndHashCode(callSuper = false)
@Accessors(chain = true)
@ApiModel
public class Cities implements Serializable {

    private static final long serialVersionUID = 1L;

    @ApiModelProperty(value = "编号")
    private Integer id;

    @ApiModelProperty(value = "城市名称")
    private String city;

    @ApiModelProperty(value = "位置")
    private String location;
}
```
1. @ApiModelProperty：对一个属性进行标记

更多[描述](https://springfox.github.io/springfox/docs/current/#overriding-descriptions-via-properties)及[示例](https://springfox.github.io/springfox/docs/current/#property-file-lookup)点击链接即可。

除 `swagger-ui.html` 之外，还有一些默认的文档路径，详情见[此处](https://springfox.github.io/springfox/docs/current/#customizing-the-swagger-endpoints)

## 参考资料
[Springfox Reference Documentation](https://springfox.github.io/springfox/docs/current/)
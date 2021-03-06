---
layout: post
title:  "Spring Boot"
author: "eric"
description: "微服务框架spring boot在明道人事模块中的应用."
---

微服务框架spring boot在明道人事模块中的应用.以下为简介及基本使用姿势

### spring boot(微服务 流行)

### maven(构建 生产)

Boot 的目的是帮助开发人员很容易的创建出独立运行的基于 Spring 框架的应用。
选择最适合的 Spring 子项目和第三方开源库进行整合,只需要非常少的配置就可以快速运行起来

{% highlight markdown %}
Spring Boot 包含的特性如下：

* 创建可以独立运行的 Spring (J2EE规范) 应用。
* 直接嵌入 Tomcat 或 Jetty 服务器，不需要部署 WAR 文件。
* 提供推荐的基础 POM 文件来简化 Apache Maven 配置。
* 尽可能的根据项目依赖来自动配置 Spring 框架。
* 没有代码生成，也没有 XML 配置文件(java 注解的方式)。
{% endhighlight %}

对新手无需任何门槛，只要懂Maven会看文档就能亦步亦趋的开始一个新项目。

``` javascript
@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```

<p class="lead">
要进行打包和分发的工程会依赖于像Maven或Gradle这样的构建系统。为了简化依赖图，Boot的功能是模块化的，通过导入Boot所谓的“starter”模块，
可以将许多的依赖添加到工程之中。为了更容易地管理依赖版本和使用默认配置，框架提供了一个parent POM，工程可以继承它。
Spring Boot工程的样例POM文件定义
</p>
### POM程序清单
``` javascript
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```
### 开发Spring Boot应用

Boot提供了许多的“starter”模块，它们定义了一组依赖，这些依赖能够添加到构建系统之中，从而解析框架及其父平台所需的特定类库。
值得强调的一点就是当开发Web应用，尤其是RESTful Web服务的时候，如果包含了spring-boot-starter-web依赖，
它就会为你提供启动嵌入式Tomcat容器的自动化配置，并且提供对微服务应用有价值的端点信息，如服务器信息、应用指标（metrics）以及环境详情。

### 程序清单
``` javascript
@RestController
public class DemoController {

    @RequestMapping("/")
    public String index() {
        return "hello world";
    }
}
```

### 属性文件

属性文件是最常见的管理配置属性的方式。
Spring Boot 提供的 SpringApplication 类会搜索并加载 application.properties 文件来获取配置属性值。
SpringApplication 类会在下面位置搜索该文件。
上面的顺序也表示了该位置上包含的属性文件的优先级。优先级按照从高到低的顺序排列。
可以通过“spring.config.name”配置属性来指定不同的属性文件名称。也可以通过“spring.config.location”来添加额外的属性文件的搜索路径。
如果应用中包含多个 profile，可以为每个 profile 定义各自的属性文件，按照“application-{profile}”来命名。
对于配置属性，可以在代码中通过“@Value”来使用

### 通过“@Value”来使用配置属性

``` javascript
@RestController
@EnableAutoConfiguration
public class Application {
 @Value("${name}")
 private String name;
 @RequestMapping("/")
 String home() {
 return String.format("Hello %s!", name);
 }
```
### 数据访问

我们可以基于各种目的来构建微服务，但有一点是肯定的，那就是大多数都需要读取和写入数据库的能力。
Spring Boot使数据库集成变成了一项非常简单的任务
，因为它具有自动配置Spring Data以访问数据库的能力。
只需在你的工程中将spring-boot-starter-data-jpa包含进来，
Boot的自动配置引擎就能探测到你的工程需要数据访问功能，
并且会在Spring应用上下文中创建必要的Bean，
使用了Spring Data来支持来实现数据访问。

``` javascript
<dependencies>
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-mongodb</artifactId>
        <version>1.10.0.RELEASE</version>
    </dependency>
</dependencies>
```


### Maven构建Spring Boot框架的可执行Jar包

在spring boot里，很吸引人的一个特性是可以直接把应用打包成为一个jar/war，
然后这个jar/war是可以直接启动的，不需要另外配置一个Web Server。
单独的JAR包，然后通过Java -jar <name>.jar --spring.profiles.active=test 命令运行。

SpringBootMaven插件为Maven提供SpringBoot支持，它允许你打包可执行jar或war存档，然后就地运行应用。

Maven用户可以继承spring-boot-starter-parent项目来获取合适的默认设置。该父项目提供以下特性：
1、默认编译级别为Java 1.6
2、源码编码为UTF-8
3、一个依赖管理节点，允许你省略普通依赖的 <version>标签，继承自 spring-boot-dependenciesPOM。
      合适的资源过滤
4、合适的插件配置（exec插件，surefire，Git commitID，shade）
5、针对 application.properties和application.yml 的资源过滤
6、最后一点：由于默认配置文件接收Spring风格的占位符（ ${...} ），Maven  filtering改用@..@ 占位符（你可以使用Maven属性 resource.delimiter来覆盖它）。

``` javascript
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
<a href="http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#using-boot-importing-configuration">指南Spring Boot Reference Guide</a>



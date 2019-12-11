---
layout:     post
title:      springboot中高级开发面试题整理
subtitle:    springboot中高级开发面试题整理
date:       2019-12-10
author:     漆黑的夜漫天的星
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Spring
    - Springboot
    - 面试题
    - Springboot面试题
---


#### 1. springboot是什么？  
多年来，随着新功能的增加，spring 变得越来越复杂。访问spring官网页面，我们就会看到可以在我们的应用程序中使用的所有 Spring 项目的不同功能。如果必须启动一个新的 Spring 项目，我们必须添加构建路径或添加 Maven 依赖关系，配置应用程序服务器，添加 spring 配置。因此，开始一个新的 spring 项目需要很多努力，因为我们现在必须从头开始做所有事情。  
Spring Boot 提供一站式解决方案，主要是简化了使用 Spring 的难度，简省了繁重的配置，提供了各种启动器，开发者能快速上手。    

#### 2.springboot和spring有什么区别？  
spring和springboot相同之处就是核心的技术完全是一样的，不同的是springboot基于spring，简化了配置，新增了一些其他如简化配置、自动配置、内嵌服务容器，使开发者在能快速上手，提高了工作效率。
#### 3. springboot有什么优点？为什么要用它？
- 独立运行  
Spring Boot而且内嵌了各种servlet容器，Tomcat、Jetty等，现在不再需要打成war包部署到容器中，Spring Boot只要打成一个可执行的jar包就能独立运行，所有的依赖包都在一个jar包内。  
- 简化配置  
spring-boot-starter-web启动器自动依赖其他组件，简少了maven的配置。  
- 自动配置  
Spring Boot能根据当前类路径下的类、jar包来自动配置bean，如添加一个spring-boot-starter-web启动器就能拥有web的功能，无需其他配置。  
- 无代码生成和XML配置  
Spring Boot配置过程中无代码生成，也无需XML配置文件就能完成所有配置工作，这一切都是借助于条件注解完成的，这也是Spring4.x的核心功能之一。  
- 应用监控  
Spring Boot提供一系列端点可以监控服务及应用，做健康检测。  

#### 4.Spring Boot 的核心配置文件有哪几个？它们的区别是什么？
**Spring Boot 中有以下两种配置文件**：bootstrap (.yml 或者 .properties) 和 application (.yml 或者 .properties)  
**bootstrap/ application 的区别**：Spring Cloud 构建于 Spring Boot 之上，在 Spring Boot 中有两种上下文，一种是 bootstrap,，另外一种是 application,，bootstrap 是应用程序的父上下文，也就是说 bootstrap 加载优先于 applicaton。bootstrap 主要用于从额外的资源来加载配置信息，还可以在本地外部配置文件中解密属性。这两个上下文共用一个环境，它是任何Spring应用程序的外部属性的来源。bootstrap 里面的属性会优先加载，它们默认也不能被本地相同配置覆盖，因此，对比 application配置文件，bootstrap配置文件具有以下几个特性：      
boostrap 由父 ApplicationContext 加载，比 applicaton 优先加载  
boostrap 里面的属性不能被覆盖      
bootstrap/ application 的应用场景：    
application 配置文件这个容易理解，主要用于 Spring Boot 项目的自动化配置     
bootstrap 配置文件有以下几个应用场景：  
使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息  
一些固定的不能被覆盖的属性   
一些加密/解密的场景  　  

#### 5.Spring Boot 的配置文件有哪几种格式？它们有什么区别？
.properties 和 .yml，它们的区别主要是书写格式不同。另外，.yml 格式不支持 @PropertySource 注解导入配置。

#### 6.Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？
启动类上面的注解是@SpringBootApplication，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：  
@SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。  
@EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。  
@ComponentScan：Spring组件扫描。  
 
#### 7.运行 Spring Boot 有哪几种方式？
1）打包用命令或者放到容器中运行  
2）用 Maven/Gradle 插件运行  
3）直接执行 main 方法运行    

#### 8.Spring Boot 有哪几种读取配置的方式？
- 读取application文件：  
@Value注解读取方式;  
@ConfigurationProperties注解读取方式;  
- 读取指定文件：  
资源目录下建立config/db-config.properties:  
@PropertySource+@Value注解读取方式：注意：@PropertySource不支持yml文件读取。
@PropertySource+@ConfigurationProperties注解读取方式：
- Environment读取方式：
以上所有加载出来的配置都可以通过Environment注入获取到：  
总结，从以上示例来看，Spring Boot可以通过@PropertySource,@Value,@Environment,@ConfigurationProperties来绑定变量。  

#### 9. springboot的自动配置原理是什么？
 在spring程序main方法中 添加@SpringBootApplication或者@EnableAutoConfiguration 
会自动去maven中读取每个starter中的spring.factories文件 该文件里配置了所有需要被创建spring容器中的bean  

#### 10. springboot的启动器是怎么会事？
Starters可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成Spring及其他技术，而不需要到处找示例代码和依赖包。如你想使用Spring JPA访问数据库，只要加入spring-boot-starter-data-jpa启动器依赖就能使用了。Starters包含了许多项目中需要用到的依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。  
Spring Boot官方的启动器都是以spring-boot-starter-命名的，代表了一个特定的应用类型。第三方的启动器不能以spring-boot开头命名，它们都被Spring Boot官方保留。一般一个第三方的应该这样命名，像mybatis的mybatis-spring-boot-starter。  

#### 11. 开启springboot特性的方式有哪些？
- 继承spring-boot-starter-parent项目  
```
<parent>  
<groupId>org.springframework.boot</groupId>   
<artifactId>spring-boot-starter-parent</artifactId>
<version>1.5.6.RELEASE</version>
</parent>
```
- 导入spring-boot-dependencies项目依赖  

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    <dependencies>
</dependencyManagement>   

```
spring Boot依赖注意点： 属性覆盖只对继承有效  
Spring Boot依赖包里面的组件的版本都是和当前Spring Boot绑定的，如果要修改里面组件的版本，只需要添加如下属性覆盖即可，但这种方式只对继承有效，导入的方式无效。  
```
<properties>
        <slf4j.version>1.7.25<slf4j.version>
    </properties>  
    
```
如果导入的方式要实现版本的升级，达到上面的效果，这样也可以做到，把要升级的组件依赖放到Spring Boot之前。  
需要注意，要修改Spring Boot的依赖组件版本可能会造成不兼容的问题。    

#### 12.Spring Boot 支持哪些日志框架？推荐和默认的日志框架是哪个？
Spring Boot支持Java Util Logging,Log4j2,Lockback作为日志框架，如果你使用starters启动器，Spring Boot将使用Logback作为默认日志框架。无论使用哪种日志框架，Spring Boot都支持配置将日志输出到控制台或者文件中。
spring-boot-starter启动器包含spring-boot-starter-logging启动器并集成了slf4j日志抽象及Logback日志框架  

#### 13.Spring Boot中的监视器是什么？
Spring boot actuator是spring启动框架中的重要功能之一。Spring boot监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。
有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为HTTP URL访问的REST端点来检查状态。    

#### 14.springboot中常用的starter的组件有哪些.
spring-boot-starter-parent //boot项目继承的父项目模块.  
spring-boot-starter-web //boot项目集成web开发模块.  
spring-boot-starter-tomcat //boot项目集成tomcat内嵌服务器.  
spring-boot-starter-test //boot项目集成测试模块.  
mybatis-spring-boot-starter //boot项目集成mybatis框架.  
spring-boot-starter-jdbc //boot项目底层集成jdbc实现数据库操作支持.  
其他诸多组件,可到maven中搜索,或第三方starter组件到github上查询 .....  

#### 15.SpringBoot starter工作原理：
SpringBoot在启动时扫描项目依赖的jar包，寻找包含spring.factories文件的jar
根据spring.factories配置加载AutoConfigure
根据@Conditional注解的条件，进行自动配置并将bean注入到Spring Context  


#### 16.springboot项目需要兼容老项目（spring框架）,该如何实现.
集成老项目spring框架的容器配置文件即可:  
spring-boot一般提倡零配置.但是如果需要配置,也可增加:
@ImportResource({"classpath:spring1.xml" , "classpath:spring2.xml"})  
注意:resources/spring1.xml位置.  

#### 17.Spring Boot 配置加载顺序？
使用 Spring Boot 会涉及到各种各样的配置，如开发、测试、线上就至少 3 套配置信息了。Spring Boot 可以轻松的帮助我们使用相同的代码就能使开发、测试、线上环境使用不同的配置。  
在 Spring Boot 里面，可以使用以下几种方式来加载配置：  
1、properties文件；  
2、YAML文件；  
3、系统环境变量；  
4、命令行参数；  
配置属性加载的顺序如下：数字小的优先级越高，即数字小的会覆盖数字大的参数值  
1、开发者工具 `Devtools` 全局配置参数；  
2、单元测试上的 `@TestPropertySource` 注解指定的参数；  
3、单元测试上的 `@SpringBootTest` 注解指定的参数；  
4、命令行指定的参数，如 `java -jar springboot.jar --name="Java技术栈"`；  
5、命令行中的 `SPRING_APPLICATION_JSONJSON` 指定参数, 如 `java -Dspring.application.json='{"name":"Java技术栈"}' -jar springboot.jar`  
6、`ServletConfig` 初始化参数；  
7、`ServletContext` 初始化参数；  
8、JNDI参数（如 `java:comp/env/spring.application.json`）；  
9、Java系统参数（来源：`System.getProperties()`）；  
10、操作系统环境变量参数；  
11、`RandomValuePropertySource` 随机数，仅匹配：`ramdom.*`；  
12、JAR包外面的配置文件参数（`application-{profile}.properties（YAML）`）  
13、JAR包里面的配置文件参数（`application-{profile}.properties（YAML）`）  
14、JAR包外面的配置文件参数（`application.properties（YAML）`）  
15、JAR包里面的配置文件参数（`application.properties（YAML）`）  
16、`@Configuration`配置文件上 `@PropertySource` 注解加载的参数；  
17、默认参数（通过 `SpringApplication.setDefaultProperties` 指定）；  

#### 18.保护 Spring Boot 应用有哪些方法？
1.在生产中使用HTTPS  
2.使用Snyk检查你的依赖关系  
3.升级到最新版本  
4.启用CSRF保护  
5.使用内容安全策略防止XSS攻击  
6.使用OpenID Connect进行身份验证  
7.管理密码？使用密码哈希！  
8.安全地存储秘密  
9.使用OWASP的ZAP测试您的应用程序  
10.让你的安全团队进行代码审查  

#### 19.如何使用 Spring Boot 实现异常处理？
Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。  

#### 20.如何重新加载Spring Boot上的更改，而无需重新启动服务器？ 
这可以使用DEV工具来实现。通过这种依赖关系，您可以节省任何更改，嵌入式tomcat将重新启动。   
Spring Boot有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。Java开发人员面临的一个主要挑战是将文件更改自动部署到服务器并自动重启服务器。开发人员可以重新加载Spring Boot上的更改，而无需重新启动服务器。这将消除每次手动部署更改的需要。Spring Boot在发布它的第一个版本时没有这个功能。这是开发人员最需要的功能。DevTools模块完全满足开发人员的需求。该模块将在生产环境中被禁用。它还提供H2数据库控制台以更好地测试应用程序。   

 参考：  
 https://blog.csdn.net/Kevin_Gu6/article/details/88547424  
 https://www.cnblogs.com/lingboweifu/p/11797926.html  
 https://blog.51cto.com/14185725/2364360?cid=729911    
 
<!-- # 如果觉得有用，不妨关注一下我的公众号，随时随地刷刷题  
![QDSCB8.jpg](https://s2.ax1x.com/2019/12/10/QDSCB8.jpg) -->
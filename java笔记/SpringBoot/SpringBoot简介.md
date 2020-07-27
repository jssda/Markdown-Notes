# 使用SpringBoot

## 一、 什么是SpringBoot?

Spring Boot 是一个框架， 一种全新的编程规范， 他的产生简化了框架的使用， 所谓简
化是指简化了 Spring 众多框架中所需的大量且繁琐的配置文件， 所以 Spring Boot 是一个服
务于框架的框架， 服务范围是简化配置文件。所以从本质上来说， Spring Boot 其实就是 Spring
框架的另一种表现形式  

## 二、 SpringBoot特征

1. 使用 Spring Boot 可以创建独立的 Spring 应用程序
2. 在Spring Boot中直接嵌入了Tomcat、Jetty、Undertow等Web容器， 所以在使用SpringBoot做 Web 开发时不需要部署 WAR 文件
3. 通过提供自己的启动器(Starter)依赖， 简化项目构建配置
4. 尽量的自动配置 Spring 和第三方库
5. 提供了生产就绪特征， 如： 度量指标， 运行状况检查和外部化配置
6. 绝对没有代码生成， 也不需要 XML 配置文件  

## 三、 SpringBoot版本介绍

- SNAPSHOT： 快照版， 即开发版。
- CURRENT： 最新版， 但是不一定是稳定版。
- GA： General Availability， 正式发布的版本。  

## 四.、SpringBoot项目结构介绍

### 1.  POM文件

#### 1.1 继承

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.7.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

Spring Boot 的父级依赖， 只有继承它项目才是 Spring Boot 项目。
spring-boot-starter-parent 是一个特殊的 starter(启动器)， 它用来提供相关的 Maven 默认依赖。 使用它之后， 常用的包依赖可以省去 version 标签。  

#### 1.2依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

启动器依赖,web启动器会包含常用得依赖坐标

#### 1.3插件

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

spring-boot-maven-plugin 插件是将 springboot 的应用程序打包成 jar 包的插件。 将所有应用启动运行所需要的 jar 包都包含进来， 从逻辑上将具备了独立运行的条件。 当运行"mvnpackage"进行打包后， 使用"java -jar"命令就可以直接运行。  

### 2. 启动类

Spring Boot 的启动类的作用是启动 Spring Boot 项目， 是基于 Main 方法来运行的。注意： 启动类在启动时会做注解扫描(@Controller、 @Service、 @Repository......)， 扫描位置为同包或者子包下的注解， 所以启动类的位置应放于包的根下。

#### 2.1 启动类和启动器的区别

- 启动类表示项目的启动入口  
- 启动器表示 jar 包的坐标  

#### 2.2创建启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // 必须添加,添加之后才是一个springBoot的启动类
public class SpringboottestApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringboottestApplication.class, args);
    }

}
```

### 3. 启动器

Spring Boot 将所有的功能场景都抽取出来， 做成一个个的 starter(启动器)， 只需要在项目里面引入这些 starter 相关场景的所有依赖都会导入进来， 要用什么功能就导入什么场景，在 jar 包管理上非常方便， 最终实现一站式开发。  

Spring Boot 提供了多达 44 个启动器。

- spring-boot-starter
  这是 Spring Boot 的核心启动器， 包含了自动配置、 日志和 YAML。

- spring-boot-starter-actuator
  帮助监控和管理应用。

- spring-boot-starter-web
  支持全栈式 Web 开发， 包括 Tomcat 和 spring-webmvc。

- spring-boot-starter-amqp
  通过 spring-rabbit 来支持 AMQP 协议（Advanced Message Queuing Protocol） 。

- spring-boot-starter-aop
  支持面向方面的编程即 AOP， 包括 spring-aop 和 AspectJ。

- spring-boot-starter-artemis
  通过 Apache Artemis 支持 JMS 的 API（Java Message Service API） 。

- spring-boot-starter-batch
  支持 Spring Batch， 包括 HSQLDB 数据库。

- spring-boot-starter-cache
  支持 Spring 的 Cache 抽象。

- spring-boot-starter-cloud-connectors
  支持 Spring Cloud Connectors， 简化了在像 Cloud Foundry 或 Heroku 这样的云平台上连接服务。

- spring-boot-starter-data-elasticsearch
  支持 ElasticSearch 搜索和分析引擎， 包括 spring-data-elasticsearch。

- spring-boot-starter-data-gemfire
  支持 GemFire 分布式数据存储， 包括 spring-data-gemfire。
  spring-boot-starter-data-jpa  支持 JPA（Java Persistence API） ， 包括 spring-data-jpa、 spring-orm、 Hibernate。

- spring-boot-starter-data-mongodb
  支持 MongoDB 数据， 包括 spring-data-mongodb。

- spring-boot-starter-data-rest
  通过 spring-data-rest-webmvc， 支持通过 REST 暴露 Spring Data 数据仓库。

- spring-boot-starter-data-solr
  支持 Apache Solr 搜索平台， 包括 spring-data-solr。

- spring-boot-starter-freemarker
  支持 FreeMarker 模板引擎。

- spring-boot-starter-groovy-templates
  支持 Groovy 模板引擎。

- spring-boot-starter-hateoas
  通过 spring-hateoas 支持基于 HATEOAS 的 RESTful Web 服务。

- spring-boot-starter-hornetq
  通过 HornetQ 支持 JMS。

- spring-boot-starter-integration
  支持通用的 spring-integration 模块。

- spring-boot-starter-jdbc
  支持 JDBC 数据库。

- spring-boot-starter-jersey
  支持 Jersey RESTful Web 服务框架。

- spring-boot-starter-jta-atomikos
  通过 Atomikos 支持 JTA 分布式事务处理。

- spring-boot-starter-jta-bitronix
  通过 Bitronix 支持 JTA 分布式事务处理。

- spring-boot-starter-mail
  支持 javax.mail 模块。

- spring-boot-starter-mobile
  支持 spring-mobile。  

- spring-boot-starter-mustache
支持 Mustache 模板引擎。

- spring-boot-starter-redis
支持 Redis 键值存储数据库， 包括 spring-redis。

- spring-boot-starter-security
支持 spring-security。

- spring-boot-starter-social-facebook
支持 spring-social-facebook

- spring-boot-starter-social-linkedin
支持 pring-social-linkedin

- spring-boot-starter-social-twitter
支持 pring-social-twitter

- spring-boot-starter-test
支持常规的测试依赖， 包括 JUnit、 Hamcrest、 Mockito 以及 spring-test 模块。

- spring-boot-starter-thymeleaf
支持 Thymeleaf 模板引擎， 包括与 Spring 的集成。

- spring-boot-starter-velocity
支持 Velocity 模板引擎。

- spring-boot-starter-websocket
支持 WebSocket 开发。

- spring-boot-starter-ws
支持 Spring Web Services。

- spring-boot-starter-actuator
增加了面向产品上线相关的功能， 比如测量和监控。

- spring-boot-starter-remote-shell
增加了远程 ssh shell 的支持。

- spring-boot-starter-jetty  

引入了 Jetty HTTP 引擎（用于替换 Tomcat） 。

- spring-boot-starter-log4j
支持 Log4J 日志框架。

- spring-boot-starter-logging
引入了 Spring Boot 默认的日志框架 Logback。

- spring-boot-starter-tomcat
引入了 Spring Boot 默认的 HTTP 引擎 Tomcat。

- spring-boot-starter-undertow
引入了 Undertow HTTP 引擎（用于替换 Tomcat） 。  

### 4配置文件

Spring Boot 提供一个名称为 application 的全局配置文件， 支持两种格式 properteis 格式与 YAML 格式。  

#### 4.1 Properties 格式  

配置 Tomcat 监听端口  

```properties
server.port=8888
```

#### 4.2YAML 格式

YAML 格式配置文件的扩展名可以是 yaml 或者 yml  

##### 4.2.1基本格式要求

- 大小写敏感
-  使用缩进代表层级关系
-  相同的部分只出现一次  

#### 4.3配置文件存放位置  

-  当前项目根目录中  
- 当前项目根目录下的一个/config 子目录中
- 项目的 resources 即 classpath 根路径中
- 项目的 resources 即 classpath 根路径下的/config 目录中  

#### 4.4配置文件加载顺序  

##### 4.4.1不同格式的加载顺序

如果同一个目录下，有 application.yml 也有 application.properties，默 认先读取application.properties。
如果同一个配置属性，在多个配置文件都配置了，默认使用第 1 个读取到的， 后面读取的不覆盖前面读取到的。

##### 4.4.2不同位置的加载顺序

###### 4.4.2.1 当前项目根目录下的一个/config 子目录中(最高)

- config/application.properties
- config/application.yml

###### 4.4.2.2 当前项目根目录中(其次)

- application.properties
- application.yml

###### 4.4.2.3 项目的 resources 即 classpath 根路径下的/config 目录中(一般)

- resources/config/application.properties
- resources/config/application.yml

###### 4.4.2.4 项目的 resources 即 classpath 根路径中(最后)

- resources/application.properties
- resources/application.yml  

#### 4.5配置文件中的占位符

##### 4.5.1占位符语法

语法： ${}  

##### 4.5.2占位符作用

- "${}"中可以获取框架提供的方法中的值如： random.int 等。
-  占位符可以获取配置文件中的键的值赋给另一个键作为值。

##### 4.5.3生成随机数

- ${random.value} - 类似 uuid 的随机数， 没有"-"连接
- ${random.int} - 随机取整型范围内的一个值
- ${random.long} - 随机取长整型范围内的一个值
- ${random.long(100,200)} - 随机生成长整型 100-200 范围内的一个值
- ${random.uuid} - 生成一个 uuid， 有短杠连接
- ${random.int(10)} - 随机生成一个 10 以内的数
- ${random.int(100,200)} - 随机生成一个 100-200 范围以内的数  

#### 4.6bootstrap 配置文件

##### 4.6.1bootstrap 配置文件介绍

Spring Boot 中有两种上下文对象， 一种是 bootstrap, 另外一种是 application, bootstrap是应用程序的父上下文， 也就是说 bootstrap 加载优先于 applicaton。 bootstrap 主要用于从额外的资源来加载配置信息， 还可以在本地外部配置文件中解密属性。 这两个上下文共用一个环境， 它是任何 Spring 应用程序的外部属性的来源。 bootstrap 里面的属性会优先加载，它们默认也不能被本地相同配置覆盖。

##### 4.6.2bootstrap 配置文件特征

- boostrap 由父 ApplicationContext 加载， 比 applicaton 优先加载。
- boostrap 里面的属性不能被覆盖。

##### 4.6.3bootstrap 与 application 的应用场景

application 配置文件主要用于 Spring Boot 项目的自动化配置。bootstrap 配置文件有以下几个应用场景。

- 使用 Spring Cloud Config 配置中心时， 这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息。
- 一些固定的不能被覆盖的属性。
- 一些加密/解密的场景。

### 5 Spring Boot 的核心注解  

#### 5.1 @SpringBootApplication

是 SpringBoot 的启动类。此注解等同于@Configuration+@EnableAutoConfiguration+@ComponentScan 的组合。

#### 5.2 @SpringBootConfiguratio

@SpringBootConfiguration 注解是@Configuration 注解的派生注解， 跟@Configuration注解的功能一致， 标注这个类是一个配置类， 只不过@SpringBootConfiguration 是 springboot的注解， 而@Configuration 是 spring 的注解

#### 5.3 @Configuration

通过对 bean 对象的操作替代 spring 中 xml 文件

#### 5.4 @EnableAutoConfiguration

Spring Boot 自动配置（ auto-configuration） ： 尝试根据你添加的 jar 依赖自动配置你的Spring 应用。是@AutoConfigurationPackage 和@Import(AutoConfigurationImportSelector.class)注解的组合。

#### 5.5 @AutoConfigurationPackage

@AutoConfigurationPackage 注解， 自动注入主类下所在包下所有的加了注解的类（@Controller， @Service 等)， 以及配置类（@Configuration）

#### 5.6 @Import({AutoConfigurationImportSelector.class})

直接导入普通的类导入实现了 ImportSelector 接口的类导入实现了 ImportBeanDefinitionRegistrar 接口的类

#### 5.7 @ComponentScan

组件扫描， 可自动发现和装配一些 Bean。

#### 5.8 @ConfigurationPropertiesScan

@ConfigurationPropertiesScan 扫描配置属性。 @EnableConfigurationProperties 注解的作
用是使用 @ConfigurationProperties 注解的类生效  

## 五、 Spring Boot 整合 Web 层技术  

### 5.1 整合Servlet

#### 5.1.1 通过注解扫描Servlet

创建一个servlet

```java
@WebServlet(name = "first", urlPatterns = "/first")
public class FirstServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("first Servlet");
        super.doGet(req, resp);
    }
}
```

修改启动类, 添加一个注解

```java
@SpringBootApplication
@ServletComponentScan // 此注解会扫描通过webServlet注解配置的servlet类, 并实例化为bean对象
public class HelloWorldApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldApplication.class, args);
    }
}
```

#### 5.1.2 通过方法注册 Servlet

创建一个servlet

```java
@WebServlet(name = "second", urlPatterns = "/second")
public class SecondServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("second Servlet");
        super.doGet(req, resp);
    }
}
```

创建一个配置类, 或者在启动类中添加对应方法

```java
@SpringBootConfiguration
public class Config {

    @Bean
    public ServletRegistrationBean<SecondServlet> getServletRegistrationBean(){
        ServletRegistrationBean<SecondServlet> bean = new ServletRegistrationBean<>(new SecondServlet());
        bean.addUrlMappings("/second");
        return bean;
    }

}
```

### 5.2 整合Filter

整合Filter和整合Servlet是一样的, 第一种方法就是通过@WebFilter(name="", urlParten={"*.do","*.action"})注解配置, 另一种就是通过配置类注册

### 5.3 整合Listener

同整合Servlet、Filter

## 六、 Spring Boot 整合视图层技术  

### 6.1 整合jsp

#### 6.1.1 修改pom，添加`jsp`引擎和`jstl`标签库

```xml
<!--添加 jsp 引擎， SpringBoot 内置的 Tomat 中没有此依赖-->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
<!--添加 JSTL 坐标依赖-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
```

添加完依赖之后，可在main下创建webapp目录，编写jsp文件。

#### 6.1.2 配置视图解析器

可通过多种方式配置，配置文件或者配置类。

### 6.2 整合Thymeleaf技术

#### 6.2.1 Thymeleaf介绍

Thymeleaf 的主要目标是将优雅的自然模板带到开发工作流程中， 并将 HTML 在浏览器中正确显示， 并且可以作为静态原型， 让开发团队能更容易地协作。 Thymeleaf 能够处理HTML， XML， JavaScript， CSS 甚至纯文本。

长期以来,jsp 在视图领域有非常重要的地位,随着时间的变迁,出现了一位新的挑战者:Thymeleaf,Thymeleaf 是原生的,不依赖于标签库.它能够在接受原始 HTML 的地方进行编辑和渲染.因为它没有与Servelet规范耦合,因此Thymeleaf模板能进入jsp所无法涉足的领域。  

#### 6.2.2 Thymeleaf基本使用

1. pom中添加Thymeleaf依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   ```

   

2. 在指定html中添加命名空间

   命名空间： xmlns:th="http://www.thymeleaf.org"  

#### 6.2.3 基本语法

1. 字符串与变量输出操作

   ```html
   <span th:text="${msg}"></span>
   ```

2. input中的value

   ```html
   <input th:value="value" />
   ```

3. Thymeleaf中的内置对象

   语法：

   1. 引用内置对象需要使用#
   2. 大部分内置对象的名称都以 s 结尾。 如： strings、 numbers、 dates  

   字符串：

   | 表达式                                                    | 作用                                                         |
   | --------------------------------------------------------- | ------------------------------------------------------------ |
   | ${#strings.contains(msg,'T')}                             | 判断字符串是否包含指定的子串， 如果包含返回 true， 否则返回 false |
   | ${#strings.startsWith(msg,'a')}                           | 判断当前字符串是否以子串开头， 如果是返回 true， 否则返回 false |
   | ${#strings.endsWith(msg,'a')}                             | 判断当前字符串是否以子串结尾， 如果是返回 true， 否则返回 false |
   | ${#strings.length(msg)}                                   | 返回字符串的长度                                             |
   | ${#strings.isEmpty(key)}                                  | 判断字符串是否为空， 如果为空返回 true， 否则返回 false      |
   | ${#strings.substring(msg,2,5)}                            | 截取子串， 用户与 jdk String 类下 SubString 方法相同         |
   | ${#strings.toUpperCase(msg)} ${#strings.toLowerCase(msg)} | 字符串转大小写。                                             |

   日期：

   | 表达                                                         | 作用                                            |
   | ------------------------------------------------------------ | ----------------------------------------------- |
   | ${#dates.format(key)}                                        | 格式化日期， 默认的以浏览器默认语言为格式化标准 |
   | ${#dates.format(key,'yyyy/MM/dd')}                           | 按照自定义的格式做日期转换                      |
   | ${#dates.year(key)} <br />${#dates.month(key)} <br />${#dates.day(key)} | Year： 取年 <br />Month： 取月 <br />Day： 取日 |

4. 条件判断

   语法为  th:if

5. 开关语句

   语法为 th:swich/th:case

   th:switch / th:case 与 Java 中的 switch 语句等效， 有条件地显示匹配的内容。 如果有 多个匹配结果只选择第一个显示。 th:case="*"表示 Java 中 switch 的 default， 即没有 case 的值为 true 时则显示 th:case="*" 的内容

6. 迭代遍历

   th:each

   ```html
   <tr th:each="m : ${map}">
       <td th:text="${m.value.id}"></td>
       <td th:text="${m.value.name}"></td>
       <td th:text="${m.value.age}"></td>
       <td th:text="${m.key}"></td>
   </tr>
   ```

   

   th:each 状态变量
   1) index:当前迭代器的索引 从 0 开始
   2) count:当前迭代对象的计数 从 1 开始
   3) size:被迭代对象的长度
   4) odd/even:布尔值， 当前循环是否是偶数/奇数 从 0 开始
   5) first:布尔值， 当前循环的是否是第一条， 如果是返回 true 否则返回 false
   6) last:布尔值， 当前循环的是否是最后一条， 如果是则返回 true 否则返回 false  

7. 操作域对象

   请求域：

   ​	`<span th:text="${#httpServletRequest.getAttribute('msg')}"></span>`

   ​	`<span th:text="${#request.getAttribute('req')}"></span>`

   session域：

   ​	`<span th:text="${session.ses}"></span><br/>`

   ​	`<span th:text="${#session.getAttribute('ses')}"></span><br/>`

   ServletContext 域

   ​	`<span th:text="${application.app}"></span>`

   ​	`<span th:text="${#servletContext.getAttribute('app')}"></span>`

8. URL表达式语法

   ```html
   <span th:text="${#servletContext.getAttribute('app')}"></span>
   ```

   绝对路径：

   ```html
   <a th:href="@{http://www.baidu.com}">绝对路径</a>
   ```

   相对路径：

   ```html
   <a th:href="@{/show}">相对路径</a> <!--相对于当前项目的根-->
   
   <a th:href="@{~/project2/resourcename}">相对于服务器的根</a> <!--当前项目的根的上一级目录-->
   ```
   
9. URL中传递参数

   ```html
   <a th:href="@{/show?id=1&name=zhangsan}">普通 URL 格式传参</a>
   <a th:href="@{/show(id=1,name=zhangsan)}">普通 URL 格式传参</a>
   <a th:href="@{'/show?id='+${id}+'&name='+${name}}"> 普 通 URL 格 式 传 参
   </a>
   <a th:href="@{/show(id=${id},name=${name})}">普通 URL 格式传参</a>
   
   
   <a th:href="@{/show/{id}(id=1)}">restful 格式传参</a>
   <a th:href="@{/show/{id}/{name}(id=1,name=admin)}">restful 格 式 传 参
   </a>
   <a th:href="@{/show/{id}(id=1,name=admin)}">restful 格式传参</a>
   <a th:href="@{/show/{id}(id=${id},name=${name})}">restful 格式传参</a>
   ```

10. 配置文件中配置thymeleaf

    ```properties
    spring.thymeleaf.prefix=classpath:/templates/suibian/
    spring.thymeleaf.suffix=.html
    spring.thymeleaf.mode=HTML5 #配置视图模板类型， 如果视图模板使用的是 html5 需要配置
    # 默认编码就是utf-8
    spring.thymeleaf.encoding=utf-8 
    # 响应类型配置
    spring.thymeleaf.servlet.content-type=text/html 
    
    # 配置页面缓存, 默认会使用浏览器缓存
spring.thymeleaf.cache=false
    ```
    
    

## 七、 SpringBoot 访问静态资源页面

在 SpringBoot 项目中没有我们之前常规 web 开发的 WebContent（ WebApp） ， 它只有src 目录。在 src/main/resources 下面有两个文件夹， static 和 templates。SpringBoot 默认在 static目录中存放静态页面， 而 templates 中放动态页面。

### 7.1  static 目录

Spring Boot 通过 classpath/static 目录访问静态资源。 注意存放静态资源的目录名称必须
是 static。

### 7.2 templates 目录

在 Spring Boot 中不推荐使用 jsp 作为视图层技术， 而是默认使用 Thymeleaf 来做动态页
面。 Templates 目录这是存放 Thymeleaf 的页面。

### 7.3  静态资源存放其他位置

Spring Boot 访问静态资源的位置

- classpath:/META‐ INF/resources/
- classpath:/resources/
- classpath:/static/
- classpath:/public/  

可自定义静态资源位置。方法有多种，配置文件、或者配置类。



## 八.、Spring整合持久层技术

### 8.1 Spring整合jdbc技术

#### 8.1.1 添加启动器

添加jdbc启动器

```xml
<!--jdbc启动器-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

添加mysql驱动

```xml
<!--jdbc驱动-->
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.38</version>
</dependency>
```

添加外置数据源

```xml
<!--使用alibaba数据源-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.12</version>
</dependency>
```

#### 8.1.2 配置外部数据源

##### Spring中的配置方式

创建配置属性

```properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test?useUnicode=true
&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=root
```

使用@PropertySource注解加载属性文件, 使用@Value注解注入到字段中, 使用@Bean注解将Bean对象交给Spring容器管理.

```java
package pers.jssd.learn.jdbctest.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.PropertySource;

import javax.sql.DataSource;

/**
 * jdbc注解配置类
 *
 * @author jssdjing@gmail.com
 */
@Configuration
@PropertySource("classpath:/jdbc.properties")
public class JdbcConfiguration {

    @Value("${jdbc.driverClassName}")
    private String driverClassName;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;


    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);

        return dataSource;
    }

}

```

在我们使用数据源的时候, 直接注入dataSource即可

##### SpringBoot中的配置方式

在SpringBoot的配置文件application中添加配置

```properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=root
```

添加配置信息

```java
/**
 * jdbc注解配置类
 *
 * @author jssdjing@gmail.com
 */
@SpringBootConfiguration
public class JdbcConfiguration {

    @Bean
    @ConfigurationProperties(prefix = "jdbc") // 此注解会读取application配置文件中的前缀为jdbc的字段, 注入到	  dadtaSource的同名set方法中, 比如将username注入到了datasource.setUsername方法中.
    public DataSource dataSource() {

        return new DruidDataSource();
    }
}
```

#### 8.1.3 使用SpringBoot默认的数据源

在SpringBoot1.x版本中的spring-boot-starter-jdbc启动器中默认使用的是org.apache.tomcat.jdbc.pool.DataSource 作为数据源

在 Spring Boot2.x版本中的spring-boot-starter-jdbc启动器中默认使用的是com.zaxxer.hikariDataSource 作为数据源  

使用默认数据源的时候, 不需要配置我们自己的jdbc属性了, 直接使用内置的配置项. 在application中配置如下

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false
spring.datasource.username=root
spring.datasource.password=root
```

此时使用数据源可以直接注入使用了.

使用这种方式想换数据源也非常简单, 配置中添加type属性即可, 如下

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
```

### 8.2 Spring整合MyBatis

#### 8.2.1  添加启动器依赖

```xml
<dependencies>
    <!--前端模板-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <!--web启动器-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--mybatis启动器-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.2</version>
    </dependency>

    <!--mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
    <!--Druid 数据源依赖-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.12</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

#### 8.2.2 配置MyBatis逆向工程

添加插件

```xml
<!--mybtais逆向工程配置-->
<!--配置 Generator 插件-->
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.5</version>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.38</version>
        </dependency>
    </dependencies>
    <!--指定配置文件的路径-->
    <configuration>
        <configurationFile>${project.basedir}/src/main/resources/generatorConfig.xml</configurationFile>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
    </configuration>
</plugin>
```

修改配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="mysqlTable" targetRuntime="MyBatis3">
        <!-- 自动识别数据库关键字，默认false，如果设置为true，根据SqlReservedWords中定义的关键字列表；
        一般保留默认值，遇到数据库关键字（Java关键字），使用columnOverride覆盖 -->
        <property name="autoDelimitKeywords" value="true"/>
        <!-- beginningDelimiter/endingDelimiter: 指明数据库的用于标记数据库对象名的符号，比如ORACLE是双引号，MYSQL默认是`反引号  -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!--格式化java代码-->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!--格式化XML代码-->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!--添加序列化-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
        <!--添加toString-->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>

        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/test"
                        userId="root" password="root">
         <!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
         <property name="serverTimezone" value="CST"/>
        </jdbcConnection>
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="pers.jssd.springboot.learn.mybatislearn.pojo"
                            targetProject=".\src\main\java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="pers.jssd.springboot.learn.mybatislearn.mapper"
                         targetProject=".\src\main\java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="pers.jssd.springboot.learn.mybatislearn.mapper"
                             targetProject=".\src\main\java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="" tableName="user">
         <!-- 插入的时候可取得自增的主键id-->
         <generatedKey column="userId" sqlStatement="Mysql" identity="true"/>
      </table>
    </context>
</generatorConfiguration>
```

运行maven中的插件命令，生成对应的pojo类和mapper

配置相对应的资源打包

```xml
<resources>
    <!--将mapper下的xml打包到resource目录下-->
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
    </resource>
    <resource>
        <directory>src/main/resource</directory>
        <includes>
            <include>**/*.yml</include>
            <include>**/*.properties</include>
        </includes>
    </resource>
</resources>
```

启动类添加扫描mapper路径配置

```java
package pers.jssd.springboot.learn.mybatislearn;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("pers.jssd.springboot.learn.mybatislearn.mapper")
public class MybatisLearnApplication {

    public static void main(String[] args) {
        SpringApplication.run(MybatisLearnApplication.class, args);
    }

}
```

#### 8.2.3 MyBatis的其他配置项

```yml
mybatis: 
	#扫描 classpath 中 mapper 目录下的映射配置文件， 针对于映射配置文件放到了 resources 目录下，当mapper。xml放到resource下需要配置此项
	mapper-locations: classpath:/mapper/*.xml
	#定义包别名， 使用 pojo 时可以直接使用 pojo 的类型名称不用加包名
	type-aliases-package: com.bjsxt.springbootmybatis.pojo
```

## 九、Spring中的异常处理

### 9.1 自定义错误页面

直接在template中添加一个名字为error.html 的文件，名字必须为error

### 9.2 通过@ExcetpionHandler处理异常

在产生异常的相同处理器中，添加异常处理方法。

```java
@Controller
public class TestController {

    @GetMapping("/")
    public String exceptionTest1() {
        String str = null;
        str.length();
        return "ok";
    }

    @ExceptionHandler
    public ModelAndView nullPointException(Exception e){
        ModelAndView mv = new ModelAndView();
        mv.addObject("error", e);
        mv.setViewName("nullError");
        return mv;
    }

}
```

### 9.3 通过@ControllerAdvice配置全局异常

在9.2中的方法，只能在相同Controller中处理异常，不能全局处理。使用ControllerAdvice可以配置全局异常。

```java
package pers.jssd.springboot.learn.exceptionlearn.exception;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

/**
 * 全局异常配置
 *
 * @author jssdjing@gmail.com
 */
@ControllerAdvice
public class GlobalException {

    @ExceptionHandler
    public ModelAndView nullPointException(Exception e){
        ModelAndView mv = new ModelAndView();
        mv.addObject("error", e);
        e.printStackTrace();
        mv.setViewName("nullError");
        return mv;
    }

}
```

注意：当有局部异常的时候，全局异常会被覆盖。

### 9.4 通过SimpleMapping ExceptionResolver  处理异常

配置一个简单异常解析器，可以在产生异常的时候，自动映射到指定页面

```java
package pers.jssd.springboot.learn.exceptionlearn.config;

import org.springframework.boot.SpringBootConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.web.servlet.handler.SimpleMappingExceptionResolver;

import java.util.Properties;

/**
 * @author jssdjing@gmail.com
 */
@SpringBootConfiguration
public class ExceptionConfig {

    @Bean
    public SimpleMappingExceptionResolver getSimpleMappingExceptionResolver() {
        Properties properties = new Properties();
        properties.put("java.lang.NullPointerException", "nullError");
        SimpleMappingExceptionResolver simpleMappingExceptionResolver = new SimpleMappingExceptionResolver();
        simpleMappingExceptionResolver.setExceptionMappings(properties);
        return simpleMappingExceptionResolver;
    }

}
```

注意：这种方式不能携带异常信息

### 9.5 自定义异常解析器处理异常

```java
package pers.jssd.springboot.learn.exceptionlearn.config;

import org.springframework.boot.SpringBootConfiguration;
import org.springframework.ui.Model;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author jssdjing@gmail.com
 */
@SpringBootConfiguration
public class Exception2Config implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView mv = new ModelAndView();
        if (e instanceof java.lang.NullPointerException) {
            mv.setViewName("nullError");
        }
        mv.addObject("error",e.toString());
        return mv;
    }
}
```

## 十、SpringBoot整合测试框架

SpringBoot2.x 使用 Junit5 作为测试平台  

添加坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.junit.vintage</groupId>
            <artifactId>junit-vintage-engine</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

其中排除了junit-vintage-engine，这个东西是兼容`junit3` 和`junit4`用的，SpringBoot2.x之后，官方更建议我们使用Junit5

使用方式如下，如果使用脚手架工具创建，会自动生成。

```java
package pers.jssd.springboot.learn.exceptionlearn;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ExceptionLearnApplicationTests {

    @Test
    void contextLoads() {
    }

}
```

## 十一、服务端数据校验

Spring Boot 中使用了 Hibernate-validator 校验框架。  

### 11.1 实体对象校验

修改实体类添加校验规则

```java
package pers.jssd.springboot.learn.verifylearn.pojo;

import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.NotNull;

/**
 * 用户实体类
 *
 * @author jssdjing@gmail.com
 */
public class User {

    @NotNull
    private Integer userid;

    @NotBlank
    private String username;

    @NotEmpty
    private String password;

    private static final long serialVersionUID = 1L;

    public Integer getUserid() {
        return userid;
    }

    public void setUserid(Integer userid) {
        this.userid = userid;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username == null ? null : username.trim();
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password == null ? null : password.trim();
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append(getClass().getSimpleName());
        sb.append(" [");
        sb.append("Hash = ").append(hashCode());
        sb.append(", userid=").append(userid);
        sb.append(", username=").append(username);
        sb.append(", password=").append(password);
        sb.append(", serialVersionUID=").append(serialVersionUID);
        sb.append("]");
        return sb.toString();
    }
}
```

在Controller中开启校验

```java
package pers.jssd.springboot.learn.verifylearn.controller;

import org.omg.CORBA.FieldNameHelper;
import org.springframework.beans.propertyeditors.FileEditor;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.ObjectError;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import pers.jssd.springboot.learn.verifylearn.pojo.User;

import java.util.List;

/**
 * @author jssdjing@gmail.com
 */
@Controller
@RequestMapping("/user")
public class UserController {

    @PostMapping("addUser")
    public String addUser(@Validated User user, BindingResult result) {
        System.out.println("user = " + user);
        if (result.hasErrors()) {
            List<ObjectError> allErrors = result.getAllErrors();
            for (ObjectError error : allErrors) {
                FieldError fieldError = (FieldError) error;
                String field = fieldError.getField();
                String defaultMessage = fieldError.getDefaultMessage();
                System.out.println(field + ": " + defaultMessage);
            }
            return "addUser";
        }


        return "ok";
    }

}
```

在视图层展示校验提示

```html
<form th:action="@{/user/addUser}" method="post">
    <label for="username">用户姓名:</label> <input id="username" type="text" name="username">
    <span th:if="${user}">
        <span th:errors="${user.username}"></span>
    </span>
<!--    <span th:errors="${user.username}"></span>-->
    <label for="password">密码: </label><input id="password" type="password" name="password"/>
<!--    <span th:errors="${user.password}"></span>-->
    <input type="submit" value="提交">
</form>
```

### 11.2 常用校验规则

1. @NotNull: 判断基本数据类型的对象类型是否为 null
2. @NotBlank: 判断字符串是否为 null 或者是空串(去掉首尾空格)。
3. @NotEmpty: 判断集合是否为空。
4. @Length: 判断字符的长度(最大或者最小)
5. @Min: 判断数值最小值
6. @Max: 判断数值最大值
7. @Email: 判断邮箱是否合法  


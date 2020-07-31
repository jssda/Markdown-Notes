# Maven配置

下载maven, 配置环境变量自不必多说

## 添加本地仓库

找到maven安装目录, 进入其中的conf目录

打开settings

添加一个标签

```xml
<localRepository>本地仓库绝对路径</localRepository>
```

## 添加阿里云远程仓库

找到merrors标签, 添加一个镜像

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

## 常用命令

a)   clean：       清理，编译后的目录；

b)   compile：    编译，只编译main目录，不编译test中的代码；

c)   test-compile：编译test目录中的代码；

d)   test：        运行test里边的代码；

e)   package：    打包

​             i.     java项目->打成jar包；

​            ii.     web项目->打成war包；

f)    install：发布项目到本地仓库，用在打jar包上，打成jar包可以被其他项目使用；

g)   tomcat:run：一键构建项目；



## 命令生命周期

a)   Clean    生命周期：clean；

b)   Default   生命周期：compile、test-compile、test、package、install

c)   Site      生命周期：site命令，生成项目描述文档静态页；

d)   不同的生命周期可以一起执行，例如先clean 再compile；

###  命令执行顺序

a)   在相同生命周期中的命令才会受执行顺序的影响；

b)   Default生命周期中的命令执行顺序：

compile <- test-compile <- test <- package <- install；

### 添加编译插件

```xml
<build>
    <plugins><!-- 插件列表 -->
        <plugin><!-- 通过坐标获取插件 -->
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.0</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## 添加tomcat7插件

在plugins中添加

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
</plugin>
```

## scope范围说明

a)   compile（默认值）：在编译、运行、测试、打包都有效；

b)   provided：编译、测试时有效，运行、打包无效；

c)   test：仅在测试时有效；

d)   runtime：测试、运行、打包时有效；

e)   system：不推荐使用，使用system作用域不会去本地仓库寻找依赖，要指定本地路径；

![image-20191105175551920](https://raw.githubusercontent.com/jssda/picbed/master/image-20191105175551920.png)

## Tomcat热部署

更改本地tomcat下的配置文件

a)   修改tomcat -> conf-> tomcat-users.xml 配置文件；

b)   在tomcat-users标签中加入以下代码：

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="tomcat" password="tomcat" roles="manager-gui, manager-script"/>
```

a)   启动tomcat；

b)   修改项目中pom.xml，在tomcat7的插件上添加以下代码：

```xml
<configuration>
    <!--url是本项目部署路径-->
    <url>http://localhost:8080/manager/text</url>
    <username>tomcat</username>
    <password>tomcat</password>
</configuration>
```

a)   右键项目-> run as -> maven build… 在goals中使用tomcat7:deploy或tomcat7:redeploy来进行热部署；

​             i.     tomcat7:deploy，第一次部署时使用。

​            ii.     tomcat7:redeploy，非第一次部署时使用。

b)   部署完毕，打开浏览器测试；

## 配置文件放行

```xml
<!-- 配置文件放行 -->
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <!-- 放行main/resources下的配置文件 如果不放行spring mybatis等配置文件不会被打到包里从而导致错误 -->
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
```


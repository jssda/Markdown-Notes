# IDEA配置tomcat项目并运行

## 此次配置使用的环境

- JDK1.8
- tomcat 8
- idea 2019.2

## 配置步骤

### 创建项目

新建一个项目

![1566615035089](https://raw.githubusercontent.com/jssda/picbed/master/1566615035089.png)

![1566615124900](https://raw.githubusercontent.com/jssda/picbed/master/1566615124900.png)

下一步

填写项目名

![1566615230980](https://raw.githubusercontent.com/jssda/picbed/master/1566615230980.png)

完成.

### 配置目录及项目

初始项目结构

![1566615310946](https://raw.githubusercontent.com/jssda/picbed/master/1566615310946.png)

在WEB-INF目录下新建两个目录 一个是classes目录, 一个是lib目录

![1566615431585](https://raw.githubusercontent.com/jssda/picbed/master/1566615431585.png)

打开项目配置

![1566615450779](https://raw.githubusercontent.com/jssda/picbed/master/1566615450779.png)

配置模块, 将输出路径改为classes文件夹

![1566615589683](https://raw.githubusercontent.com/jssda/picbed/master/1566615589683.png)

切换到Dependencies配置, 添加一个依赖目录

![1566615644434](https://raw.githubusercontent.com/jssda/picbed/master/1566615644434.png)

选择你刚刚创建的lib文件夹

![1566615741776](https://raw.githubusercontent.com/jssda/picbed/master/1566615741776.png)

依赖配置完成

![1566615785687](https://raw.githubusercontent.com/jssda/picbed/master/1566615785687.png)

打开Artifacts, 为你部署好的项目起一个名字

![1566615904656](https://raw.githubusercontent.com/jssda/picbed/master/1566615904656.png)

### 配置运行环境

![1566615955315](https://raw.githubusercontent.com/jssda/picbed/master/1566615955315.png)

添加tomcat服务器

![1566615979429](https://raw.githubusercontent.com/jssda/picbed/master/1566615979429.png)

![1566616020487](https://raw.githubusercontent.com/jssda/picbed/master/1566616020487.png)

配置tomcat目录

![1566616063479](https://raw.githubusercontent.com/jssda/picbed/master/1566616063479.png)

选择你tomcat安装的目录

![1566616083977](https://raw.githubusercontent.com/jssda/picbed/master/1566616083977.png)

配置完之后, 顺便给你的tomcat服务器起一个名字

![1566616188960](https://raw.githubusercontent.com/jssda/picbed/master/1566616188960.png)

配置部署的包

![1566616215559](https://raw.githubusercontent.com/jssda/picbed/master/1566616215559.png)

添加之后, 可以看到. 刚刚在项目设置里配置的Artificial添加到了这里

![1566616281696](https://raw.githubusercontent.com/jssda/picbed/master/1566616281696.png)

## 运行项目

运行项目, 会自动弹出浏览器

![1566616798310](https://raw.githubusercontent.com/jssda/picbed/master/1566616798310.png)

此时, 配置完成
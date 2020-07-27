# Tomcat服务器介绍和使用

## 服务器软件的概念和作用

所谓服务器软件其实就是代码编写的一个可以根据用户请求实时的调用
执行对应的逻辑代码的一个容器。在普通用户看来就是一个安装程序。我们只需
要将服务器在操作系统上进行安装，并将我们事先编写好的逻辑处理代码根据规
则放到服务器的指定位置，启动服务器，那么服务器就自动的会根据接收到请求
调用并执行对象的逻辑代码进行处理。 

## Tomcat服务器的下载和安装

下载地址: [http://tomcat.apache.org/](http://tomcat.apache.org/)

### 下载选项

下载的时候建议下载对应的zip压缩包

注意, 找到download下的core中的windows zip包

### 安装

下载成功之后, 解压到没有中文的目录中.

## 目录结构介绍

- \bin目录: 存放启动和关闭 Tomcat 的可执行文件 
- \conf: 存放 Tomcat 的配置文件 
- \lib 存放库文件 
- \logs 存放日志文件 
- \temp 存放临时文件 
- \webapps 存放 web 应用 
- \work 存放 JSP 转换后的 Servlet 文件 

## 校验安装

1. 打开bin目录, 然后双击startup.bat文件.
2. 打开浏览器, 在地址栏输入localhost:8080/
3. 如果出现tomcat广告页安装成功

## 常见问题

1. 环境变量配置: [百度经验](https://jingyan.baidu.com/article/6dad5075d1dc40a123e36ea3.html)

2. 闪退，启动闪退问题一般是因为JDK的环境变量配置有问题，参照 1 进行重新进行配置，如果还是闪退使用： 

   在 bin\startup.bat 文件中的第一行前面加入：

   ```
   SET JAVA_HOME=JDK 目录
   SET CATALINA_HOME=解压后 Tomcat 的目录
   ```

   这样，运行 startup.bat 就可以正常启动 tomcat 服务器，如
   果想点击 shutdown.bat 关闭服务器，同样在 shutdown.bat 文
   件中加入上面的两行即可 

## 怎样注册为windows服务

启动cmd, 切换到tomcat中的bin目录下

使用命令 

```bash
service.bat install 服务名
```

可注册服务
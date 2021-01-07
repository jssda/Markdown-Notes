# Jenkins 部署整合 gitLab 

### 环境

CentOS 7

war包安装

已安装 java 环境, 并配置环境变量

### 安装步骤

- 获取安装源

  ```shell
  sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  ```

- 安装公钥

  ```shell
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
  ```

- 安装Jenkins

  ```shell
  yum install jenkins
  ```

- 修改配置

  ```shell
  vim /etc/sysconfig/jenkins
  ```

  修改如下:

  ```shell
  JENKINS_PORT="8081"  #Jenkins服务访问端口
  ```

- 启动 Jenkins

  ```shell
  service jenkins start
  ps -ef | grep jenkins
  ```

### 设置

浏览器访问相对应的 ipAddress:port 访问

登录按照提示打开对应的文件, 获取密码. 登录, 初始化完成之后, 选择安装推荐插件. 后续按照需求添加自定义插件.

安装完成之后, 创建用户即可.

点击系统管理, 插件管理

![image-20210104161152140](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104161152140.png)

可选插件搜索 GitLab

点击安装

登录 GitLab , 生成一个访问Tokens.

![image-20210104161803416](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104161803416.png)

复制这个 Tokens.



剪辑 Jenkins  的系统管理, 系统配置. gitlab

![image-20210104161942068](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104161942068.png)

配置GitLab，”Connection Name”随便填，“Git Host URL”填GitLab的访问地址，然后点“Add”——“jenkins”，如下所示：

![image-20210104162417002](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104162417002.png)

弹出框中, 类型选择 API Token, 然后将你复制的 Token 填入 Api Token 中. 

![image-20210104162627947](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104162627947.png)

选择刚刚添加的 API Token

![image-20210104162713912](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104162713912.png)

点击 TestConnection 测试链接, 如果返回 success, 则成功. 

### 项目配置

创建一个任务

![image-20210104162944153](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104162944153.png)

在安装 Jenkins 的服务器上, 生成一个 SSH 密钥.

ssh-keygen -t rsa -C "<注释说明>" -b 4096

![image-20210104163522098](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104163522098.png)

复制 .pub 中的公钥内容.

打开, gitlab 中的 ssh key 选项卡, 公钥粘贴过去

![image-20210104163753487](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104163753487.png)

添加完成之后是这个样子

![image-20210104163817218](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104163817218.png)

进入项目

源码管理, 粘贴ssh仓库地址, 添加凭据. 

![image-20210104164215462](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104164215462.png)

添加完之后不会报错就对了

![image-20210104164435363](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104164435363.png)

### 触发器配置

![image-20210104165129238](https://raw.githubusercontent.com/jssda/picbed/master/image-20210104165129238.png)


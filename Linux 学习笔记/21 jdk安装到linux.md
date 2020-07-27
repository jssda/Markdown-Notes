# CentOS7 安装jdk(tar包版本)

1. 官网下载jdk

   ```
   jdk-8u221-linux-x64.tar.gz
   ```

2. 启动ftp工具, 将此包上传到linux中, 目录为/tmp

3. 解压此包

   ```
   tar -zxvf jdk-8u221-linux-x64.tar.gz
   ```

4. 将此包复制到/usr/local/文件夹中, 并且改名为jdk

   ```
   cp jdk-8u221-linux-x64 /usr/local/jdk/ -r
   ```

5. 配置环境变量

6. 编辑/etc/profile文件, 在最后添加

   ```
   export JAVA_HOME=/usr/local/jdk
   export JRE_HOME=${JAVA_HOME}/jre
   export CLASSPATH=.:${JAVA_HOME}/lib/tools.jar:${JRE_HOME}/lib/dt.jar
   export PATH=${JAVA_HOME}/bin:${JAVA_HOME}/jre/bin:$PATH
   ```

7. 保存退出

8. 使profile文件生效

   ```
   source /etc/profile
   ```

   
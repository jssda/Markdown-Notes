# 配置zookeeper伪集群

## 环境

1. CentOS7
2. 虚拟机
3. ip: 192.168.2.203
4. zookeeper3.5.6
5. 必须有java环境, 本机使用jkd1.8

## 下载zookeeper

网址https://zookeeper.apache.org/releases.html

下载下来后使用ftp工具上传给linux服务器

### 配置zookeeper

1. 解压到安装目录

   ```
   tar -zxvf apache-zookeeper-3.5.6.tar.gz -C /usr/local/zookeeper/
   # 切换到安装目录
   cd /usr/local/zookeeper/
   # 改一个名字
   mv apache-zookeeper-3.5.6/ zk01
   ```

2. 集群部署, 多拷贝几份

   ```
   cp zk01/ zk02 -r
   cp zk01/ zk03 -r
   ```

3. 创建 Zookeeper 的数据目录  

   ```
   cd zk01
   mkdir data
   ```

   ![image-20191211172903593](https://raw.githubusercontent.com/jssda/picbed/master/image-20191211172903593.png)

   重复, 直到三个zookeeper应用全部创建

4. 创建zookeeper应用myid标识

   ```
   echo 1 >> ./zk01/data/myid
   echo 2 >> ./zk02/data/myid
   echo 3 >> ./zk03/data/myid
   ```

5. 配置zoo.cfg文件

   ```
    cd zk01/conf
    cp zoo_sample.cfg zoo.cfg
    vim zoo.cfg
   ```

6. 改目录, 端口

   ![image-20191211173440358](https://raw.githubusercontent.com/jssda/picbed/master/image-20191211173440358.png)

   末尾添加

   ![image-20191211173713227](https://raw.githubusercontent.com/jssda/picbed/master/image-20191211173713227.png)

   server.x: x就是myid中存储的标识

   ip:端口1:端口2: ip就是zookeeper应用的ip, 端口1是投票端口, 端口2是选举端口

   重复, 配置zk02和zk03. 分别修改data目录. 修改应用端口.为2182, 2183

   添加集群配置信息, 就是底下那三行, 不用做修改

7. 逐个启动zookeeper

   如果没有关闭防火墙的话, 关闭防火墙

   ![image-20191211185925213](https://raw.githubusercontent.com/jssda/picbed/master/image-20191211185925213.png)

   

   
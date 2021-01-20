# CentOS7 安装mysql5.7.tar.gz

1. 下载

2. 上传到linux服务器

3. 解压到/usr/local目录

   ```
   tar -zxvf mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
   ```

4. 改名为mysql

   ```
   mv mysql-5.7.28-linux-glibc2.12-x86_64 mysql
   ```

5. 添加用户和组

   -r标识创建一个系统账户, -g标识指定用户组

   ```
   [root@server02 mysql]# groupadd mysql
   [root@server02 mysql]# useradd -r -g mysql mysql
   [root@server02 mysql]# id mysql
   uid=997(mysql) gid=1001(mysql) 组=1001(mysql)
   ```

6. 初始化 

   ```
   ./bin/mysqld --user=mysql --basedir=/usr/local/mysql--datadir=/usr/local/mysql/data --initialize
   ```

   注意观察, 生成了临时密码



7. 进入 support-files.

8. 运行命令

   ```shell
   ./mysql.server start
   ```

   注意此处, 如果安装目录不是`/usr/local/mysql`, 则需要编辑 mysql.server 文件, 将其中的 basedir 和 datadir 路径改了.

9. 修改密码

   ```
   bin/mysql -u root -p
   password
   # 密码使那个生成的临时密码
   
   set password=password("新密码");
   ```

   

### 配置service服务模式

与下方配置的systemd服务模式二选一即可

service 服务设置

1. 设置开机启动

   ```
   cp support-files/mysql.server /etc/init.d/mysql
   
   chmod +x /etc/init.d/mysql 
   
   chkconfig --list mysql
   ```

2. 开启服务

   ```
   service mysql start
   ```

3. 给外网ip配置访问权限

   ```
   grant all privileges on *.* to 'root'@'%' identified by '密码';
   flush privileges;
   ```


### 配置 systemd 服务模式(推荐)

vim /etc/systemd/system/mysql.service

#### 添加如下内容

```shell
[Unit]
Description=MySQL Server 5.7.33
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
User=mysql
Group=mysql

PIDFile=/usr/local/mysql/data/jssd-server.pid
ExecStart=/usr/local/mysql/support-files/mysql.server start
ExecReload=/usr/local/mysql/support-files/mysql.server restart
ExecStop=/usr/local/mysql/support-files/mysql.server stop
PrivateTmp=false

[Install]
WantedBy=multi-user.target
```

#### 测试

```shell
systemctl daemon-reload
systemctl start mysql
systemctl sop mysql
systemctl status mysql
```



参考文章: https://blog.csdn.net/LLLCSDN_001/article/details/113357100
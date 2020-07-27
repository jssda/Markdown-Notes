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

7. 设置开机启动

   ```
   cp support-files/mysql.server /etc/init.d/mysql
   
   chmod +x /etc/init.d/mysql 
   
   chkconfig --list mysql
   ```

8. 开启服务

   ```
   service mysql start
   ```

9. 修改密码

   ```
   bin/mysql -u root -p
   password
   # 密码使那个生成的临时密码
   
   set password=password("新密码");
   ```

10. 给外网ip配置访问权限

    ```
    grant all privileges on *.* to 'root'@'%' identified by '密码';
    flush privileges;
    ```

    
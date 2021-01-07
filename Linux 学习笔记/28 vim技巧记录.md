# VIM技巧记录

## 一、字符串替换

：s/vivian/sky/ 替换当前行第一个 vivian 为 sky 


：s/vivian/sky/g 替换当前行所有 vivian 为 sky 

：n，$s/vivian/sky/ 替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky 

：n，$s/vivian/sky/g 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky 



## 二、用户

### 2.1 添加用户

useradd 只创建用户，创建完了用 passwd lilei 去设置新用户的密码。

adduser 会创建用户，创建目录，创建密码（提示你设置），做这一系列的操作。

 useradd、userdel 这类操作更像是一种命令，执行完了就返回。而 adduser 更像是一种程序，需要你输入、确定等一系列操作

### 2.2 删除用户

deluser是delete和user的缩写，delete是英语“删除”的意思，user是“用户”的意思。

```shell
userdel newname1
```

单单用deluser命令，不加参数的话，只会删除用户，但是不会删除在/home目录中的用户家目录。如果你想要连此用户的家目录也一并删除，可以加上 –remove-home 这个参数，如下：

deluser –remove-home newname

这样，不仅删除了newname这个用户，连/home/newname这个目录也会删除。

### 2.3 修改用户

usermod 修改用户

```shell
-l：对用户重命名，但是/home目录中的用户家目录名不会改变，需要手动修改。
-g：修改用户所在群组
```

可以一次将一个用户添加到多个群组，就用 -G 参数（大写的G）。用法如下：

```shell
usermod -G friends,happy,funny newname
```

### 2.4 群组

- addgroup
- groupadd
- delgroup 
- groupdel
- groups username  查看用户所在组

### 2.5 注意

adduser和deluser命令只是Debian一族（包括Ubuntu）才有的命令。其他的LInux发行版，一般来说，添加用户和删除用户是用useradd和userdel命令。
而且，用useradd添加用户之后，在默认的情况下，该账号是暂时被封锁的， 也就是说，该账号是无法登录，须要用passwd命令来给新创建的用户设置密码之后才可以使用。
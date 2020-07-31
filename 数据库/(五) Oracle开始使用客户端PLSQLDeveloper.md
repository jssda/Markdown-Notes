[TOC]

## Oracle自带客户端

打开开始菜单, 可以找到Oracle安装菜单( 本系统为win10, win7也可以找到, 只是画面不同 ). 点击SQL Plus, 就可打开自带客户端. 是一个黑窗口.

![1563789331063](https://raw.githubusercontent.com/jssda/picbed/master/1563789331063.png)

![1563789430792](https://raw.githubusercontent.com/jssda/picbed/master/1563789430792.png)

这个时候就可以登陆了, 登录的时候, 如果选用的是sys用户登录, 那么必须使用sysdba身份或者sysoper身份登录, 输入密码的时候, 是不会显示的, 这是Oracle的保护机制

![1563789490321](https://raw.githubusercontent.com/jssda/picbed/master/1563789490321.png)

可以看到, SQLPlus打开是类似cmd的, 这种情况一般意味着可以使用cmd窗口打开. 当然, 需要配好环境变量.

右键我的电脑, 点击属性. 打开高级系统设置

![1563790187607](https://raw.githubusercontent.com/jssda/picbed/master/1563790187607.png)

进入环境变量

![1563790212018](https://raw.githubusercontent.com/jssda/picbed/master/1563790212018.png)

进入配置path路径

![1563790235658](https://raw.githubusercontent.com/jssda/picbed/master/1563790235658.png)

一般可以看到, Oracle在安装的时候自动给配好了, 我们不需要操心. 这里告诉使用cmd打不开sqlplus的童鞋,可以手动配置此目录. 注意, 配置的话需要配到bin目录为止

![1563790401931](https://raw.githubusercontent.com/jssda/picbed/master/1563790401931.png)

好, 环境变量没问题. win+r打开运行窗口, 输入cmd, 打开cmd窗口

![1563790466218](https://raw.githubusercontent.com/jssda/picbed/master/1563790466218.png)

输入sqlplus 登录用户名/登录密码 as 身份信息就可登录成功. 默认是以nomal登录, 登录sys的时候, 必须使用sysdba或者sysoper登录

![1563790878545](https://raw.githubusercontent.com/jssda/picbed/master/1563790878545.png)

![1563790985305](https://raw.githubusercontent.com/jssda/picbed/master/1563790985305.png)

登录之后就可执行各种sql命令.

## 使用第三方客户端PL/SQL Developer

自行下载PL/SQL Developer安装包, 本文不予赘述

安装过程如下

![1563791154756](https://raw.githubusercontent.com/jssda/picbed/master/1563791154757.png)

![1563791183129](https://raw.githubusercontent.com/jssda/picbed/master/1563791183130.png)



![1563791209751](https://raw.githubusercontent.com/jssda/picbed/master/1563791209751.png)

经典安装即可

![1563791236701](https://raw.githubusercontent.com/jssda/picbed/master/1563791236701.png)

安装完毕

![1563791264168](https://raw.githubusercontent.com/jssda/picbed/master/1563791264168.png)

登录用户

![1563791401901](https://raw.githubusercontent.com/jssda/picbed/master/1563791401901.png)

整个界面如下

![1563791450111](https://raw.githubusercontent.com/jssda/picbed/master/1563791450111.png)

右侧Objcet选项框中有各种对象信息, 比如类型, 表, 用户, 视图, 序列等等

![1563791539458](https://raw.githubusercontent.com/jssda/picbed/master/1563791539458.png)

打开sql编辑窗口

![1563791622748](https://raw.githubusercontent.com/jssda/picbed/master/1563791622748.png)

查询一条语句, 选中执行, 可以看到查询结果

![1563792017182](https://raw.githubusercontent.com/jssda/picbed/master/1563792017182.png)
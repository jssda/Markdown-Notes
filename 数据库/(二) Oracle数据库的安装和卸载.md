# 安装

## 安装版本

此次安装为Oracle11g, 以图片形式列出安装步骤

![1563694310249](https://raw.githubusercontent.com/jssda/picbed/master/1563694362808.png)

![1563694362808](https://raw.githubusercontent.com/jssda/picbed/master/1563694362808.png)

下一个界面, 基本都会选择第一个, 第二个仅会安装一个实例, 用于连接数据库, 在工作时候连接公司数据库时候可能会安装这个, 学习用的化安装第一个就没问题了

![1563694411634](https://raw.githubusercontent.com/jssda/picbed/master/1563694411634.png)

这个很简单, 一般桌面类就行了

![1563694559201](https://raw.githubusercontent.com/jssda/picbed/master/1563694559201.png)

Oracle安装的基目录可以自己选择, 软件位置选择的目录中不能包含空格, 所以Program Files文件夹就不能使用, 数据库版本选择标准版, 用于学习, 字符集改为Unicode字符集. 全局数据库名可更改, 管理口令自己设置, 过于简单可能会提示, 不过不用担心, 忽略就行

![1563694780136](https://raw.githubusercontent.com/jssda/picbed/master/1563694780136.png)

![1563694960532](https://raw.githubusercontent.com/jssda/picbed/master/1563694960532.png)

![1563694977668](https://raw.githubusercontent.com/jssda/picbed/master/1563694977668.png)

然后会经历漫长的等待

![1563695013604](https://raw.githubusercontent.com/jssda/picbed/master/1563695013604.png)

可能会让你安装这个, 安装就行了

![1563695603679](https://raw.githubusercontent.com/jssda/picbed/master/1563695603679.png)

接着等

![1563695674897](https://raw.githubusercontent.com/jssda/picbed/master/1563695674897.png)

完成之后会有提示

![1563696102067](https://raw.githubusercontent.com/jssda/picbed/master/1563696102067.png)

给两个账户设置口令

![1563696074378](https://raw.githubusercontent.com/jssda/picbed/master/1563696074378.png)

确认完成安装

# 卸载

需要注意的是, Oracle应该避免频繁的卸载安装软件

1. 停止使用Oracle的服务

   停用oracle服务, 进入计算机管理, 在服务中, 找到orcale开头的所有服务, 右击选择停止

2. 运行卸载 Oracle 数据库程序 

   在开始菜单中找到 Oracle 安装产品， 点击运行 Oracle 自带的卸载程序 Universal Installer工具卸载。 

3. 删除使用oracle的服务

   开始菜单中， 找到 Universal Installer， 运行 Oracle Universal Installer， 单击卸载产品，在产品清单窗口中， 单击全部展开， 除了 OraDb11g_home1 外， 勾选其他项目， 单击删除，根据软件提示单击下一步最终完成卸载 

4. 删除注册表中的Oracle相关项

5. 在 命 令 窗 口 ， 输 入 regedit ， 打 开 注 册 表 ， 依 次 展 开HKEY_LOCAL_MACHINE\SOFTWARE， 找到 oracle， 删除之。 

   依次展开 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services 中， 删除所有
   oracle 开头的项 

   依次展开HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application， 删除
   所有 oracle 开头的项。 

   扩展删除（以下不是必须的注册表删除项） ， 如果安装不成功可以自己也把这些删除了 

   在 HKEY_CLASSES_ROOT， 删除以 Ora、 Oracle、 Orcl 或 EnumOra 为前缀的键 

   删除HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\MenuOrder\Start Menu\Programes 中所有以Oracle开头的键

   删除HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI中除Microsoft ODBC for Oracle注册表键以外所有的含有Oracle的键

6. 删除Oracle环境变量

7. 打开资源管理器， 在地址栏中输入“%userprofile%\「开始」 菜单\程序” 回车， 删除安
   装的 Oracle 目录。 然后再到地址栏中输入“%allusersprofile%\「开始」 菜单\程序” 回车，
   删除安装的 Oracle 目录。 

8. 重启计算机

9. 删除基目录

   如果在 Program Files\Oracle 目录存在， 则删除 Program Files\Oracle 目录 

   
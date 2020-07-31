#  Oracle的数据导入与导出

## 数据库导入导出需要注意

1. 目标数据库要与源数据库有着相同的表空间
2. 目标数据在进行导入时, 用户名尽量相同(这样保证用户的权限级别相同)
3. 目标数据库每次再进行数据导入前, 应该做好数据备份, 以防数据丢失
4. 弄清是导入导出到相同版本还是不同版本(oracle10g, 还是Oracle11g)
5. 目标数据导入前, 弄清楚是数据覆盖(替换),还是仅插入新数据或替换部分数据表
6. 确定目标数据库磁盘空间是否足够容纳新数据, 是否需要扩充表空间
7. 导入导出时注意字符集是否相同, 一般Oracle数据库字符集只有一个, 并且固定, 一般不会改变
8. 确定操作者的权限

## 导出数据格式介绍

Dmp格式: .dmp是二进制文件, 可跨平台, 还能包含权限, 效率好.

Sql格式: .sql格式的文件, 可用文本编辑器查看, 通用性比较好, 效率不如第一种. 适合小数据量导入导出. 尤其注意的是表中不能有大字段(blob, clob, long), 如果有, 会报错

pde格式: .pde格式的文件为PL/SQL Developer自有的文件格式, 只能用PL/SQL Developer工具导入导出, 不能用文本编辑器查看

## 传统方式exp(导出)和(imp)导入

### 命令执行方式

该命令需要再操作系统的命令窗口执行, 而非sqlPlus. 再使用导入或导出命令时, 在命令的后侧不要添加分号

### 命令格式

exp|imp 用户名/密码@连接地址:端口/服务名 file=路径/文件名.dmp full=y | tables(tablename, tablename…) | owner(user1, user2, user3)

- full=y: 表示全库导出. 可以不写, 默认为no, 只导出用户下的对象
- tables: 表示导出哪些表
- owner: 导出该用户下的信息
- full | table | owner: 只能使用一种

### 示例

将HR用户下的对象导出

```
exp hr/oracle@localhost:1521/orcl file=d:/1.dmp
```

## 使用工具导出与导入

![1564823916930](https://raw.githubusercontent.com/jssda/picbed/master/1564823916930.png)

这里有导入和导出的选项

![1564824193568](https://raw.githubusercontent.com/jssda/picbed/master/1564824193568.png)
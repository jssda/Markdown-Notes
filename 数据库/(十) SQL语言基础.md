[TOC]

# SQL语言介绍

## 1. 什么是SQL语言

1. SQL: Structured Query Language. 结构化查询语言
2. SQL是最重要的关系数据库操作语言, 是所有关系数据库管理系统的标准语言
3. SQL语言是非过程化的语言, 只需要告诉做什么, 不需要关注怎么做, 简单

## 2. SQL语言可以做什么

- 增删改查(CRUD)
- 操作数据库对象(用户, 表格, 序列, 索引….)
- 操作用户权限和角色授予跟取消
- 事务(Transaction)管理

## 3. SQL语言的分裂

1. DQL, Data Query Language, 数据查询语言, 执行数据库的查询操作, select
2. DML, Data Manipulation Language, 数据操作语言, 操作表格中的数据, 执行增删改, insert, delete, update
3. DDL, Data Definition Language, 数据定义语言, 用于操作数据库对象, create, alter, drop
4. DCL, Data Control Language, 数据控制语言, 操作用户权限, grant, revoke
5. TCL, Transaction Control Language, 事务控制语言, 用于管理事务, commit, rollback

# Oracle中HR用户的介绍

在接下来练习过程中使用的是Oracle给提供的实例用户HR, 里边的表作为练习使用

## 解锁HR用户

1. 登录sys用户

   ![1564223285703](https://raw.githubusercontent.com/jssda/picbed/master/1564223285703.png)

2. 在User中找到HR, 点击Edit

   ![1564223355439](https://raw.githubusercontent.com/jssda/picbed/master/1564223355439.png)

3. 解锁

   ![1564223403213](https://raw.githubusercontent.com/jssda/picbed/master/1564223403213.png)

4. 登录测试

   ![1564223446587](https://raw.githubusercontent.com/jssda/picbed/master/1564223446587.png)

5. 查看表结构

   ![1564223508617](https://raw.githubusercontent.com/jssda/picbed/master/1564223508617.png)

6. 使用命令查看表结构

   打开一个新的command window, 输入指令 desc countries, 就可以查看表的结构

   ![1564223622737](https://raw.githubusercontent.com/jssda/picbed/master/1564223622737.png)


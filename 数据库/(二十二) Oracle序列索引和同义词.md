[TOC]

# 序列

## 什么是序列

序列是用户创建的数据库对象, 序列会产生唯一的整数. 序列的一个典型的用途是创建一个主键的值, 它对于每一行必须是唯一的. 序列由一个Oracle内部程序产生并增加或减少.

序列是一个节省时间的对象, 因为它可以减少应用程序中产生序列程序的代码量. 序列号独立于表被存储和产生. 因此, 相同的序列可以被多个表使用.

## 创建序列

### 通过DDL语句创建序列

```sql
create sequence sequence_name
	[increment by n]
	[start with n]
	[{maxvalue n | nomaxvalue}]
	[{minvalue n | nominvalue}]
	[{cycle | nocycle}]
	[{cache n | nocache}];
```

1. increment by n 指定序列号的间隔, 默认递增量为1
2. start with n 起始量
3. maxvalue n. 指定序列能产生的最大值
4. nomaxvalue (默认值)对于升序序列指定10^27为最大值. 对于降序序列指定-1为最大值
5. minvalue n 指定最小序列值
6. nominvalue (默认选项)对于升序序列指定为1, 对于降序序列指定-(10^26)为最小值
7. cycle | nocycle 执行序列在达到最大最小值之后, 是否循环产生
8. cache n | nocache 指定Oracle服务器预先分配多少值, 并且保存在内存中(默认缓冲20个)

示例: 创建一个序列名称为: dept_seq, 增长间隔为10, 从120开始, 最大值为9999, 不缓存, 不循环使用

```sql
SQL> create sequence dept_seq increment by 10 start with 120 maxvalue 9999 nocache nocycle;
Sequence created
```

### 通过PL/SQL Developer工具创建

![1564712467993](https://raw.githubusercontent.com/jssda/picbed/master/1564712467993.png)

填写完毕Apply即可

![1564712492451](https://raw.githubusercontent.com/jssda/picbed/master/1564712492451.png)

## 操作序列

### 查询序列

```sql
select sequence_name, increment_by, max_value, min_value, last_number from user_sequences;
```

### 使用序列

NEXTVAL和CURRVAL伪列

- nextval返回下一个可用的序列的值, 它每次返回一个唯一的被引用的值, 即使对于不同的用户也是如此
- CURRVAL 获得当前的序列值
- 在currval获得一个值以前, nextval对该序列必须发布

```sql
SQL> select employees_seq.currval from dual;
select employees_seq.currval from dual
ORA-08002: 序列 EMPLOYEES_SEQ.CURRVAL 尚未在此会话中定义

SQL> select employees_seq.nextval from dual;
   NEXTVAL
----------
       207

SQL> select employees_seq.nextval from dual;
   NEXTVAL
----------
       208

SQL> select employees_seq.currval from dual;
   CURRVAL
----------
       208

SQL> select employees_seq.currval from dual;
   CURRVAL
----------
       208
```

示例: 在location_id为2500中插入一个新部门名称Suppot

```sql
SQL> insert into departments(department_id, department_name, location_id) values (departments_seq.nextval, 'Suppot', 2500);
1 row inserted
```

###  修改和删除序列

#### 修改序列原则

- 必须是被修改序列的所有这, 或者由alter权限
- 用alter sequence 语句, 只有以后的序列数会受到影响
- 用alter sequence语句, start with选项不能被更改, 为了以不同的数重新开始一个序列, 该序列必须被重新删除和重新创建

示例: 将dept_seq序列中的增长量修改20, 最大值修改为999999

```sql
SQL> alter sequence dept_seq increment by 20 maxvalue 999999 nocache nocycle;
Sequence altered

SQL> select dept_seq.nextval from dual;
   NEXTVAL
----------
       130

SQL> select dept_seq.nextval from dual;
   NEXTVAL
----------
       150
```

### 删除序列

使用drop语句删除一个序列

示例

```sql
SQL> drop sequence dept_seq;
Sequence dropped
```

# 索引

## 什么是索引

在关系型数据库中, 索引是一种单独的, 物理的对数据库表中一列或多列的值进行排序的一种结构, 他是某个表中一列或若干列值的集合和相应的指向表中物理标识这些值的数据页的逻辑指针清单. 索引的作用相当于图书的目录, 可以根据图书的页码快速找到所需的内容. 

索引提供对表中行的直接和快速访问, 它的目的是用以索引的路径快速定位数据以减少磁盘I/O. 索引由Oracle服务器自动使用和维护, 索引逻辑的和物理的独立于他们的索引表, 这意味着索引可以在任何时候被创建或删除, 并且不会影响基表或其他的索引, 当删除表时, 其他的索引也会被删除.

## 索引的类型

### 唯一性索引

当你在一个表中定义一个列为主键, 或者定义一个唯一键为约束时. Oracle服务器自动创建该索引, 索引的名字习惯上时约束的名字

### 非唯一性索引

由用户创建, 例如, 可以创建一个Foreign key列索引用于一个查询中的连接来改进数据取回的速度

## 创建索引的方式

- 自动: 在一个表的定义中, 当定义一个primary key 或 unique约束时, 一个唯一索引被自动创建
- 手动: 用户能够在列上创建非唯一性索引来加速对行的访问

## 如何使用索引

过多也不是好事

在表上建立更多的索引并不意味着更快的查询, 在带索引的表上被提交的每个DML操作, 意味着索引必须更新; 与表联系的索引越多, 对Oracle数据库影响越大, Oracle数据库在每次DML操作之后, 都会更新所有的索引

### 什么时候创建索引

- 一个列包含一个大范围的值
- 一个列包含很多的空值
- 一个或多个列经常同时在一个where字句中或一个连接条件被使用
- 表很大, 并且经常的查询期望取回少于百分之2到4的行.

### 什么时候不创建索引

- 表很小
- 不经常在查询中作为条件被使用的列
- 大多数查询期望被取回多于表中百分之2到4的行
- 表经常被更新
- 被索引的列作为表达式的一部分被引用

## 操作索引

### 创建单行索引

示例: 为employees表中的last_name创建一个索引并命名为emp_index

```sql
SQL> create index emp_index on employees(last_name);
Index created
```

### 创建复合索引

示例: 为departments表创建一个包括manager_id与location_id复合索引并命名为dept_man_loc

```sql
SQL> create index dept_man_loc on departments(manager_id, location_id);
Index created

-- 当查询条件中有manager_id和location_id时, 此索引会发生作用
```

### 创建函数索引

示例: 为departments表中的department_name创建一个大写函数的索引dept_upper.

```sql
SQL> create index dept_upper on departments(upper(department_name));
Index created
-- 当查询的where条件中有upper(department_name))关键字时候, 此索引会生效
```

## 查询索引

- USER_INDEXS 数据字典视图包含索引和他唯一的名字
- USER_IND_COLUMNS视图包含索引名, 表名, 列名

```sql
select ic.index_name, ic.column.name,
	   ic.column_position col_pos, ix.uniqueness
from user_indexes ix, user_ind_columns ic
where ic.index_name = ix.index_name
and ic.table_name='EMPLOYEES';
```

## 删除索引

使用drop语句删除索引

```sql
SQL> drop index emp_index;
Index dropped
```

# 同义词

## 什么是同义词

同义词可以除去对象名必须带的方案限制, 并提供一个可替换表名, 视图名, 序列名和存储过程名或其他对象名. 该方法对具有特别长的对象名字很有用. 同义词是对象的别名

## 创建同义词

```sql
SQL> create synonym em for employees;
Synonym created

-- 这是em就是employees表了
```

## 删除同义词

使用drop语句删除

```sql
SQL> drop synonym em;
Synonym dropped
```




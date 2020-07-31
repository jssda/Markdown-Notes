[TOC]

# Oracle 创建表

## Oracle表命名规则

- 必须以字母开头
- 长度不能超过30个字符
- 避免使用Oracle的关键字
- 只能使用A-Z, a-z, 0-9, _#$

## 使用带有特殊符号的表名

Oracle在创建表时, 表名会自动转换大小写, Oracle对表名大小写不敏感. 如果在定义表名时含有特殊符号, 或者用小写字母来定义表名则需要在表名两侧添加双引号

## 使用客户端创建表

![1563885704373](https://raw.githubusercontent.com/jssda/picbed/master/1563885704373.png)

![1563885965197](https://raw.githubusercontent.com/jssda/picbed/master/1563885965197.png)

# 数据库中的约束

## 约束的作用

约束用于规定表中的数据规则, 如果存在违反约束的数据行为, 行为会被约束终止

## 约束类型

### 主键约束 (Primary Key Constraint)

唯一性, 非空性

### 唯一约束 (Unique Constraint)

唯一性, 可以空, 但只能有一个

### 检查约束 (Check Constraint)

对该数据的范围, 格式的限制, (如: 年龄, 性别等)

### 非空约束 (Not NULL Constraint)

该列不允许包含空值

### 外键约束 (Foreign key Constraint)

需要建立两表间的关系并引用主表的列

# 数据库中表的关系

设计关系数据库的一个重要部分时将数据元素划分位相关的表, 我们可以根据数据本身的关联性, 将不同的表之间的数据集合聚在一起. 注意: 无论在表与表之间建立了什么样的关系, 决定数据之间是否有关系的不是表, 而是数据本身.

表与表之间一般存在三种关系: 即一对一, 一对多, 多对多关系

## 一对多

一对多关系时建立两张表之间的关系, 一个表中的一条数据可以对应另一个表中的多条数据. 记住, 外键永远在多方. 外键允许重复, 允许含有空值.

![1563886813175](https://raw.githubusercontent.com/jssda/picbed/master/1563886813175.png)

使用PL/SQL Developer工具建立两张表

### T_CLASSROOM表

![1563887149949](https://raw.githubusercontent.com/jssda/picbed/master/1563887149949.png)

添加列

![1563887166080](https://raw.githubusercontent.com/jssda/picbed/master/1563887166080.png)

添加约束

![1563887182328](https://raw.githubusercontent.com/jssda/picbed/master/1563887182328.png)

### T_STUDENT表

![1563887563860](https://raw.githubusercontent.com/jssda/picbed/master/1563887563860.png)

添加列

![1563887595081](https://raw.githubusercontent.com/jssda/picbed/master/1563887595081.png)

添加主键和外键约束

![1563887628705](https://raw.githubusercontent.com/jssda/picbed/master/1563887628705.png)

## 一对一

一对一关系是建立在一对多的基础之上， 外键可以在任何一方， 需要让外键一方具备唯一约束 

![1563887689685](https://raw.githubusercontent.com/jssda/picbed/master/1563887689685.png)

### T_USER表

![1563887800009](https://raw.githubusercontent.com/jssda/picbed/master/1563887800009.png)

添加列

![1563887864816](https://raw.githubusercontent.com/jssda/picbed/master/1563887864816.png)

添加约束

![1563887891122](https://raw.githubusercontent.com/jssda/picbed/master/1563887891122.png)

## T_ROLE表

![1563887977768](https://raw.githubusercontent.com/jssda/picbed/master/1563887977768.png)

添加列

![1563888479025](https://raw.githubusercontent.com/jssda/picbed/master/1563888479025.png)

添加约束, 一对一外键约束应该是唯一的

![1563888494851](https://raw.githubusercontent.com/jssda/picbed/master/1563888494851.png)

## 多对多

需要建立一个中间表， 中间表里建立两个列， 然后需要用这两个列作为这个表的联合主键， 然后每个列在作为外键参照各自的表的主键 

例如: 商品和订单之间的关系

![1563888553942](https://raw.githubusercontent.com/jssda/picbed/master/1563888553942.png)

### T_ORDER表

![1563888613270](https://raw.githubusercontent.com/jssda/picbed/master/1563888613270.png)

创建列

![1563888657376](https://raw.githubusercontent.com/jssda/picbed/master/1563888657376.png)

建立约束

![1563888701341](https://raw.githubusercontent.com/jssda/picbed/master/1563888701341.png)

### T_ITEM表

![1563888748252](https://raw.githubusercontent.com/jssda/picbed/master/1563888748252.png)

创建列

![1563888795770](https://raw.githubusercontent.com/jssda/picbed/master/1563888795770.png)

创建约束

![1563888835273](https://raw.githubusercontent.com/jssda/picbed/master/1563888835273.png)

### T_ORDER_ITEM表

![1563889046205](https://raw.githubusercontent.com/jssda/picbed/master/1563889046205.png)

创建列

![1563889019207](https://raw.githubusercontent.com/jssda/picbed/master/1563889019207.png)

创建约束, 注意此处应该是联合主键

![1563889171621](https://raw.githubusercontent.com/jssda/picbed/master/1563889171621.png)
# MyBatis 运行原理

## 运行过程中涉及到的类或者接口

### Resources类

用于加载MyBatis核心配置文件

### XMLConfigBuilder类

用于解析xml文件(核心配置文件)

### Configuration类

用于存放xml文件解析后的结果

### SqlSessionFactory接口

SqlSession对象的创建工厂

### DefaultSqlSessionFactory类

是SqlSessionFactory接口的实现类, 创建时需要使用Configuration对象

### SqlSession接口

是MyBatis操作的核心

### DefaultSqlSession类

是sqlSession接口的实现类

### TransactionFactory 接口

用于生产Transaction对象

### Transaction接口

用于表示操作数据库的事务对象

### Excutort接口

是MyBatis的核心执行器, 类似于jdbc中的Statement, 常用的实现类是SimpleExecutor

### SimpleExecutor类

是Executort执行器的实现类

## 文字描述

当MyBatis运行开始时, 先要通过Resources加载核心配置文件, 之后使用XMLConfigBuilder对配置文件进行解析, 将解析结构封装到Configuration中. 接着, 使用Configuration对象构建一个DefaultSqlSessionFactory对象, 至此, SqlSession工厂构建完成.

接下来, 通过工厂对象调用openSession方法创建SqlSession对象, 在这个过程中, 需要通过TransactionFactory生成Transaction对象, 并且, 还需要创建核心执行器Executor对象, 之后, 通过这些对象来创建DefaultSqlSession对象, 至此, SqlSession对象创建成功

之后, 通过SqlSession对象执行相应的操作, 如果执行成功, 调用commit方法提交事务, 如果执行失败, 调用rollback回滚事务. 最后, 调用close方法贯标SqlSession对象.
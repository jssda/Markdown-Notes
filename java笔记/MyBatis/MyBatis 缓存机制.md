# MyBatis的缓存机制

- 缓存用于提高查询的效率. 
- MyBatis 的缓存是使用 SQL 标签的 ID 作为缓存的唯一标识
  的. 执行相同的标签可以使用缓存. 不同的标签不能使用
  缓存. 
- MyBatis 中有两种缓存机制. 

## 一级缓存

- 默认开启. 线程级别的缓存, SqlSession 的缓存 
- 在一个 SqlSession 生命周期中有效. SqlSession 关闭,
  缓存清空. 
- 基于session对象的缓存. 当在一定的时间之内, 叫用两次id一样的方法, 则只会执行一次查询, 第一次查询的数据会缓存

## 二级缓存

- 进程级别的缓存, SqlSessionFactory 的缓存 
- 在一个 SqlSessionFactory 生命周期中有效.可以在多个
  SqlSession 生命中期中共享 
- 默认关闭, 需要使用的时候, 要为某个命名空间开启二级
  缓存(在 mapper.xml 中配置`<cache>`) 
- 实体类必须实现序列化接口, 不然会报异常

## 开启二级缓存

再对应的mapper.xml中配置`<cache/>`

```xml
<cache/>
```


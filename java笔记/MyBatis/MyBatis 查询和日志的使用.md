# MyBatis使用

## 1. MyBatis中常用的三个查询方法

### 1.1 selectList

用于查询多条数据的情况, 返回值是一个list集合. 如果没有查到任何数据, 返回没有元素的空集合(不是null)

### 1.2 selectOne

用于查询单挑数据的情况, 返回值是一个对象, 如果没有查到任何数据, 返回null

### 1.3. selectMap

用于查询多条数据的情况, 多条数据要形成一个Map集合, 需要指定哪个属性为key, 如果查不到, 返回一个空Map集合, 不是null

## 2. 带参查询

### 2.1 一个参数

我们查询sql语句的时候, 难免会传递一些参数. 这个时候, 就需要配置mapper中的参数了. 

想要使用参数, 我们在xxxMapper.xml中, select语句应该这样配置

```xml
<!--
        使用#{index}的方式获取索引
        也可以使用#{param1, param2...}的方式获取索引
      -->
<select id="selById" resultType="User" parameterType="int">
    select *
    from myuser
    where id = #{0};
</select>
```

### 2.2 多个参数

```xml
<!--
        如果需要传递多个参数, 则需要传递一个对象或者一个map集合
        获取参数属性使用#{参数名字} 或者 使用map的时候, #{key}
      -->
<select id="selByNameSex" resultType="user" parameterType="user">
    select *
    from myuser
    where name = #{name}
    and sex = #{sex}
</select>

<select id="selByNameSex2" resultType="user" parameterType="map">
    select *
    from myuser
    where name = #{username}
    and sex = #{sex}
</select>
```

### 2.3 测试方法

传递参数的使用, 调用sqlSession的相应函数, 传递两个参数, 第一个是执行的sql方法, 第二个就是参数

```java
@Test
public void testSelOne() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-cfg.xml");
    SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession session = sessionFactory.openSession();
    //        User user = session.selectOne("pers.jssd.mapper.UserMapper.selById", 1005);
    //        System.out.println(user);

    //        User u = new User();
    //        u.setName("seo");
    //        u.setSex("女");
    //        User user = session.selectOne("pers.jssd.mapper.UserMapper.selByNameSex", u);
    //        System.out.println("user = " + user);

    Map<String, String> map = new HashMap<>();
    map.put("username", "seo");
    map.put("sex", "女");
    User user = session.selectOne("pers.jssd.mapper.UserMapper.selByNameSex2", map);
    System.out.println("user = " + user);

    session.close();
}
```



# MyBatis的日志支持

## 1.1 基本使用

将日志文件log4j.properties放到src目录下, 运行测试方法

可以看到执行过程. 日志级别为DEBUG

日志配置文件

```properties
# Set root category priority to INFO and its only appender to CONSOLE.
log4j.rootCategory=DEBUG, CONSOLE
#log4j.rootCategory=INFO, CONSOLE, LOGFILE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
# 此处是单独配置控制台的日志级别
#log4j.appender.CONSOLE.Threshold=INFO
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=- %m%n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.Threshold=INFO
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n

```

日志输出

```properties
- PooledDataSource forcefully closed/removed all connections.
- PooledDataSource forcefully closed/removed all connections.
- PooledDataSource forcefully closed/removed all connections.
- PooledDataSource forcefully closed/removed all connections.
- Opening JDBC Connection
- Created connection 1276611190.
- Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@4c178a76]
- ==>  Preparing: select * from myuser where id = 1002; 
- ==> Parameters: 
- <==      Total: 1
User{id=1002, name='seo', register=Tue Apr 03 07:21:20 CST 2018, integral=90, sex='女'}
- Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@4c178a76]
- Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@4c178a76]
- Returned connection 1276611190 to pool.
```

运行过程如下: 

1. 强制清空数据库连接池信息. 
2. 打开JDBC连接. 创建连接对象. 
3. 设置事务为手动提交
4. 执行查询语句
5. 设置事务为自动提交
6. 关闭JDBC连接
7. 归还数据库连接到连接池

### 1.2 局部调整日志级别, 定制日志的输出

在log4j的配置文件中, 添加如下信息

```properties
# 提高整体的日志输出级别
log4j.rootCategory=ERROR, CONSOLE

# 单独设置一个方法的日记级别为DEBUG
log4j.logger.pers.jssd.mapper.UserMapper.selAll=DEBUG
# 单独设置一个类的日记级别为DEBUG
log4j.logger.pers.jssd.mapper.UserMapper=DEBUG
# 单独设置一个包的日记级别为DEBUG
log4j.logger.pers.jssd.mapper=DEBUG
```

## 2. `<settiongs>`开启对log4j的支持

### 2.1 MyBatis中日志配置

在myBatis的核心配置文件中, 添加settings设置

```xml
<!--  settings标签是configuration的子标签, 但必须放在environments标签之前  -->
<settings>
    <!--  开启对log4g的支持  -->
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

### 2.2 MyBatis中的其他配置

这些是极其重要的调整, 它们会修改 MyBatis 在运行时的行为方式。下面这个表格描述了设置信息,它们的含义和默认值。 

| 设置参数                       | 描述                                                         | 有效值                                                       | 默认值                                                       |
| ------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| cacheEnabled                   | 这个配置使全局的映射器启用或禁用 缓存。                      | true \| false                                                | true                                                         |
| lazyLoadingEnabled             | 全局启用或禁用延迟加载。当禁用时, 所有关联对象都会即时加载。 | true \| false                                                | true                                                         |
| aggressiveLazyLoading          | 当启用时, 有延迟加载属性的对象在被 调用时将会完全加载任意属性。否则, 每种属性将会按需要加载。 | true \| false                                                | true                                                         |
| multipleResult<br/>SetsEnabled | 允许或不允许多种结果集从一个单独 的语句中返回(需要适合的驱动) | true \| false                                                | true                                                         |
| useColumnLabel                 | 使用列标签代替列名。 不同的驱动在这 方便表现不同。 参考驱动文档或充分测 试两种方法来决定所使用的驱动。 | true \| false                                                | true                                                         |
| useGeneratedKeys               | 允许 JDBC 支持生成的键。 需要适合的 驱动。 如果设置为 true 则这个设置强制 生成的键被使用, 尽管一些驱动拒绝兼 容但仍然有效(比如  Derby) | true \| false                                                | False                                                        |
| autoMappingBehavior            | 指定 MyBatis 如何自动映射列到字段/ 属性。PARTIAL 只会自动映射简单, 没有嵌套的结果。FULL 会自动映射任  意复杂的结果(嵌套的或其他情况) 。 | NONE, PARTIAL, FULL                                          | PARTIAL                                                      |
| defaultExecutorType            | 配置默认的执行器。SIMPLE 执行器没 有什么特别之处。REUSE 执行器重用 预处理语句。BATCH 执行器重用语句 和批量更新 | SIMPLE REUSE BATCH                                           | SIMPLE                                                       |
| defaultStatement<br/>Timeout   | 设置超时时间, 它决定驱动等待一个数 据库响应的时间。          | Any positive integer                                         | Not Set (null)                                               |
| safeRowBounds<br/>Enabled      | Allows using RowBounds on nested statements.                 | true \| false                                                | False                                                        |
| mapUnderscore<br/>ToCamelCase  | Enables automatic mapping from classic database column names A_COLUMN to  camel case classic Java property names aColumn. | true \| false                                                | False                                                        |
| localCacheScope                | MyBatis uses local cache to prevent circular references and speed up  repeated nested queries. By default (SESSION) all queries executed during a  session are cached. If localCacheScope=STATEMENT local session will be used just  for statement execution, no data will be shared between two different calls to  the same SqlSession. | SESSION \| STATEMENT                                         | SESSION                                                      |
| jdbcTypeForNull                | Specifies the JDBC type for null values when no specific JDBC type was  provided for the parameter. Some drivers require specifying the column JDBC type  but others work with generic values like NULL, VARCHAR or OTHER. | JdbcType enumeration. Most common are: NULL, VARCHAR and OTHER | OTHER                                                        |
| lazyLoadTrigger<br/>Methods    | Specifies which Object's methods trigger a lazy load         | A method name list separated by commas                       | equals,clone,<br/>hashCode,toString                          |
| defaultScripting<br/>Language  | Specifies the language used by default for dynamic SQL generation. | A type alias or fully qualified class name.                  | org.apache.ibatis.<br/>scripting.xmltags.<br/>XMLDynamic<br/>LanguageDriver |
| callSettersOnNulls             | 当结果集中含有Null值时是否执行映射对象的setter或者Map对象的put方法。此设置对于原始类型如int,boolean等无效。 | true \| false                                                | false                                                        |
| logPrefix                      | Specifies the prefix string that MyBatis will add to the logger names. | Any String                                                   | Not set                                                      |
| logImpl                        | Specifies which logging implementation MyBatis should use. If this setting  is not present logging implementation will be autodiscovered. | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \|  NO_LOGGING | Not set                                                      |
| proxyFactory                   | Specifies the proxy tool that MyBatis will use for creating lazy loading  capable objects. | CGLIB \| JAVASSIST                                           | CGLIB                                                        |


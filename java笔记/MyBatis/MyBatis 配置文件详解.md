# MyBatis的两个配置文件详解

## MyBatis核心配置文件详解

### 1. `<configuration>`

这是配置文件的根元素, 所有的其他元素都要在这个标签下使用

### 2. `<environments>`

用于管理所有的环境, 并可以指定默认使用的哪个环境, 通过default属性来指定

### 3.`<environment>`

用于配置环境, id属性用于唯一标识当前环境

### 4. `<transactionManager>`

用于配置事务管理器

#### type属性

用于指定MyBatis采用何种方式管理事务

- JDBC(一般使用的方式): 表示MyBatis采用与原生JDBC一致的方式管理事务
- MANAGED: 表示将事务管理交给其他容器进行, 例如Spring

### 5. `<DateSource>`

用于配置数据源. 也就是配置数据库连接. 

#### type属性

用于设置MyBatis是否使用连接池技术

- POOLED, 表示采用连接池技术
- UNPOOLED, 表示每次都会开启和关闭连接, 不使用连接池技术
- JNDI, 使用其他容器(例如 Spring) 提供数据源

### 6. `<settings>`

配置一些MyBatis其重要的调整, 它们会修改 MyBatis 在运行时的行为方式。

### 7. `<properties>`

这些是外部化的, 可替代的属性, 这些属性也可以配置在典型的 Java 属性配置文件中, 或者通过 properties 元素的子元素来传递。例如: 

```xml
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

也可以这样配置. 加载外部的资源配置

```xml
<properties resource="db.properties"/>
```

在使用这些全局属性的时候, 需要这样调用

```xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

### 8. `<typeAliases>`标签

#### 8.1 类型别名

类型别名是为 Java 类型命名一个短的名字。它只和 XML 配置有关, 只用来减少类完全限定名的多余部分。

```xml
<!-- 此设置必须放在settings设置之后 -->
<typeAliases>
    <!-- alias可以不写, 默认会变成类的名字 -->
    <typeAlias type="pers.jssd.pojo.User" alias="User"/>
</typeAliases>
```

此时在设置返回类型的时候, 可以这样设置

```xml
<mapper namespace="pers.jssd.mapper.UserMapper">
    <select id="selAll" resultType="User">
        select * from myuser
    </select>
    <!--使用的时候, 不区分大小写-->
    <select id="selById" resultType="User">
        select * from myuser where id = 1002;
    </select>
</mapper>
```

#### 8.2 包别名

也可以写成包的名字, 别名省略. 就可以直接使用包下面所有的类名. 就像java中引入了一个包一样

```xml
<typeAliases>
    <!--        <typeAlias type="pers.jssd.pojo.User" alias="User"/>-->
    <package name="pers.jssd.pojo"/>
</typeAliases>
```

#### 8.3 内建别名的使用

对于普通的 Java 类型,有许多内建的类型别名。它们都是大小写不敏感的,由于重载的名字,要注意原生类型的特殊处理。 

|    别名    | 映射的类型 |
| :--------: | :--------: |
|   _byte    |    byte    |
|   _long    |    long    |
|   _short   |   short    |
|    _int    |    int     |
|  _integer  |    int     |
|  _double   |   double   |
|   _float   |   float    |
|  _boolean  |  boolean   |
|   string   |   String   |
|    byte    |    Byte    |
|    long    |    Long    |
|   short    |   Short    |
|    int     |  Integer   |
|  integer   |  Integer   |
|   double   |   Double   |
|   float    |   Float    |
|  boolean   |  Boolean   |
|    date    |    Date    |
|  decimal   | BigDecimal |
| bigdecimal | BigDecimal |
|   object   |   Object   |
|    map     |    Map     |
|  hashmap   |  HashMap   |
|    list    |    List    |
| arraylist  | ArrayList  |
| collection | Collection |
|  iterator  |  Iterator  |


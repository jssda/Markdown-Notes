# MyBatis环境搭建

## 1. 创建项目

创建一个javaproject

## 2. 导入jar包

1. MyBatis核心jar包: 
   - mybatis-3.2.7.jar
2. 依赖包:
   - asm-3.3.1.jar  字节码解析包, 被cglib依赖
   - cglib-2.2.2.jar   动态代理的实现
   - commons-logging-1.1.1.jar  日志包
   - javassist-3.17.1-GA.jar  字节码解析包
   - log4j-1.2.17.jar  日志包
   - log4j-api-2.0-rcl.jar  日志
   - log4j-core-2.0-rc1.jar   日志
   - slf4j-api-1.7.5.jar  日志
   - slf4j-log4j12-1.7.5.jar  日志
3. 驱动包
   - mysql-connector-java-5.1.30.jar  数据库连接驱动

## 3. MyBatis核心配置文件

### 要求

核心配置是一个xml文件, 命名无要求, 位置无要求, 一般叫mybatis.xml, 放在src目录下

### dtd约束

官方提供的dtd约束

```xml-dtd
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
```

### 配置文件内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--        default 用于指定那个环境, 为以下环境之一-->
    <environments default="dev">
        <!--            配置开发环境, id为环境识别编码-->
        <environment id="dev">
            <!--                配置事务管理器
                                type: 用于设置mybatis采用什么方式管理事务
                                jdbc表示采用和JDBC一样的方式管理
            -->
            <transactionManager type="JDBC"> </transactionManager>
            <!--
                数据源/连接池, 用于配置连接池和数据库连接的参数
                type: 用于设置mybatis是否采用连接池技术
                POOLED表示MyBatis采用连接池技术
            -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/test"/>
                <property name="username" value="root"/>
                <property name="password" value="1624022009.a"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

## 4. MyBatis映射文件配置

在 MyBatis 中, 推荐使用 mapper 作为包名, 我们只需要写
一个映射配置文件即可. UserMapper.xml, 用于定义要执行
的 SQL 语句, 同时设定返回结果的类型. 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace: 命名空间, 可以随意定义, 一般情况下要写全限定路径
    MyBatis管理SQL语句是通过namespace+id来定位的
-->
<mapper namespace="pers.jssd.mapper.UserMapper">
    <!--
        select标签用于编写查询语句
        id: sql语句的唯一标识, 类比为方法名
        resultType: 用于设定返回结果的类型(全限定路径)
        如果返回结果是集合, 要写集合泛型的类型
    -->
    <select id="selAll" resultType="pers.jssd.pojo.User" >
        select * from myuser;
    </select>
</mapper>
```

同时, 需要在核心配置文件中添加, 放在`</environments>`之后

```xml
<!-- 扫描mapper文件 -->
<mappers>
	<mapper resource="com/bjsxt/mapper/UserMapper.xml" />
</mappers>
```

## 5. 代码完成, 进行测试

需要引入junit4的包, 然后运行此类就行

```java
package pers.jssd.test;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;
import pers.jssd.pojo.User;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author jssdjing@gmail.com
 */
public class TestSelAll {
    @Test
    public void testSelAll() throws IOException {
        // 加载MyBatis核心配置文件
        InputStream is = Resources.getResourceAsStream("mybatis.xml");
        // 构建SqlSessionFactory工厂对象
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
        // 通过工厂打开SqlSession
        SqlSession session = factory.openSession();
        // 通过session执行查询操作
        List<User> list = session.selectList("pers.jssd.mapper.UserMapper.selAll");
        System.out.println(list);
        // 关闭资源
        session.close();
    }
}
```


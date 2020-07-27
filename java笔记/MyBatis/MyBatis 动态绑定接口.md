# MyBatis动态绑定接口

## 提供接口

首先, 在mapper包下提供相应的接口

```java
package pers.jssd.mapper;

import pers.jssd.pojo.User;

import java.util.List;

/**
 * @author jssdjing@gmail.com
 */
public interface UserMapper {
    List<User> selAll();
}
```

## 提供和接口相互绑定的xml文件

此处注意, 命名空间必须是绑定的接口的全限定路径, 不能更改. id必须和绑定的接口中方法相同

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="pers.jssd.mapper.UserMapper">
    <select id="selAll" resultType="User">
        select *
        from myuser
    </select>
</mapper>
```

## 测试方法

```java
@Test
public void selectAll() {
    SqlSession session = MyBatisUtil.getSession();

    UserMapper mapper = session.getMapper(UserMapper.class);
    List<User> users = mapper.selAll();
    for (User user : users) {
        System.out.println("user = " + user);
    }

    session.close();
}
```

## 多参数解决方案

### 索引传递方法

在mapper中的接口如果是这样定义的

```java
/**
     * 通过id和name查询对象
     *
     * @param id   用户id
     * @param name 用户姓名
     * @return 返回查找到的User对象
     */
User selByIdName(int id, String name);
```

在Mapper中配置参数的时候, 不配置ParameType类型. 取的参数的时候, 用索引方式取得传递的参数

```xml
<select id="selByIdName" resultType="User">
    select * from myuser where id = #{0} and name = #{1}
</select>
```

测试方法:

```java
@Test
public void testParams() {
    SqlSession session = MyBatisUtil.getSession();

    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selByIdName(1002, "seo");
    System.out.println("user = " + user);

    session.close();
}
```

### 注解方法

在接口中取得参数的时候, 加上注解

```java
/**
     * 通过id和name查询对象
     *
     * @param id   用户id
     * @param name 用户姓名
     * @return 返回查找到的User对象
     */
User selByIdName(@Param("id") int id, @Param("name") String name);
```

对应的xml中, 就可以用注解配置的别名取得参数. 事实上, MyBatis内部将注解和参数封装成了一个map进行传递了进去

```xml
<select id="selByIdName" resultType="User">
    select * from myuser where id = #{id} and name = #{name}
</select>
```


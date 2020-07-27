# MyBatis操作DML

## 配置默认事务处理

我们默认使用JDBC的事务处理模式

```xml
<transactionManager type="JDBC"/>
```

## 配置mapper中的操作

```xml
<insert id="insUser" parameterType="user">
    insert into myuser(id, name, register, integral, sex)
    values (default, #{name},  #{register}, #{integral}, #{sex})
</insert>
```

## 封装MyBatis工具类

```java
package pers.jssd.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

/**
 * @author jssdjing@gmail.com
 */
public class MyBatisUtil {

    private static SqlSessionFactory factory = null;

    static  {
        try {
            InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-cfg.xml");
            factory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 取得sqlSession对象
     * @param autoCommit 是否设置事务自动提交, false则不自动提交, true则自动提交
     * @return SqlSession
     */
    public static SqlSession getSession(boolean autoCommit) {
        SqlSession session = null;
        if (factory != null) {
            session = factory.openSession(autoCommit);
        }
        return session;
    }

    /**
     * 取得sqlSession对象. 默认事务不会自动提交
     * @return SqlSession
     */
    public static SqlSession getSession() {
        return getSession(false);
    }
}
```

## 测试代码

注意此处插入数据的时候, 需要手动提交事务和回滚, 否则事务默认回滚. 可以在取得SqlSession的时候, 使用openSesiion(true) 来不关闭自动提交.

```java
@Test
public void insertUser() {
    User user = new User(0, "233", new Date(), 11, "女");
    SqlSession session = MyBatisUtil.getSession();
    int num = session.insert("pers.jssd.mapper.UserMapper.insUser", user);
    if (num >= 1) {
        session.commit();
    } else {
        session.rollback();
    }
    session.close();
}
```

## 更新和删除

```xml
<update id="updUser" parameterType="user">
    update myuser
    set name = #{name}
    where id = #{id}
</update>

<delete id="delUser" parameterType="int">
    delete from myuser where id = #{0}
</delete>
```

测试代码: 

```java
@Test
public void updateUser() {
    User user = new User(2005, "144", new Date(), 15, "女");
    SqlSession session = MyBatisUtil.getSession();
    int num = session.update("pers.jssd.mapper.UserMapper.updUser", user);
    if (num >= 1) {
        session.commit();
        System.out.println("成功");
    } else {
        session.rollback();
        System.out.println("失败");
    }
    session.close();
}

@Test
public void deleteUser() {
    SqlSession session = MyBatisUtil.getSession();
    int num = session.delete("pers.jssd.mapper.UserMapper.delUser", 2005);
    if (num >= 1) {
        session.commit();
    } else {
        session.rollback();
    }
    session.close();
}
```

## 总结

事实上, 所有的增删改操作, 在底层调用的都是jdbc中的executeUpdate方法. 操作是一样的. 
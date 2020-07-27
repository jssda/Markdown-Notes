# MyBatis 中的动态SQL查询

## 动态查询简介

> MyBatis 的一个强大的特性之一通常是它的动态 SQL 能力。如果你有使用 JDBC 或其他相似框架的经验,你就明白条件地串联 SQL  字符串在一起是多么的痛苦,确保不能忘了空格或在列表的最后省略逗号。动态 SQL 可以彻底处理这种痛苦。 
>
> 通常使用动态 SQL 不可能是独立的一部分,MyBatis 当然使用一种强大的动态 SQL 语言来改进这种情形,这种语言可以被用在任意映射的 SQL  语句中。 
>
> 动态 SQL 元素和使用 JSTL 或其他相似的基于 XML 的文本处理器相似。在 MyBatis 之前的版本中,有很多的元素需要来了解。MyBatis  3 大大提升了它们,现在用不到原先一半的元素就能工作了。MyBatis 采用功能强大的基于 OGNL 的表达式来消除其他元素。 
>
> - if  
> - choose (when, otherwise)  
> - trim (where, set)  
> - foreach 

## if标签和where标签的使用

mapper接口中定义动态查询接口, 参数均可为空, 为空则表示不查此属性. 

```java
/**
     * 动态查询, 如果有其中的一个就可以查询, 如果没有, 则查询所有
     * @param id 查询id
     * @param name 查询name
     * @return 返回查询到的用户对象的List容器
     */
List<User> selParams(@Param("id") Integer id, @Param("name") String name);
```

对应的mapper.xml

```xml
<select id="selParams" resultType="User">
    select * from myuser
    <where>
        <if test="name!=null and name != ''">
            and name = #{name}
        </if>
        <if test="id != null and id != 0">
            and id = #{id}
        </if>
    </where>
</select>
```

### if 标签实现动态查询

用于进行条件判断, test 属性用于指定判断条件. 为了拼接
条件, 在 SQL 语句后强行添加 1=1 的恒成立条件 

```xml
<select id="sel" resultType="user">
    select * from t_user where 1=1
    <if test="username != null and username != ''">
        and username=#{username}
    </if>
    <if test="password != null and password != ''">
        and password=#{password}
    </if>
</select>
```

### `<where>` 配合 `<if>`实现动态查询

while用于管理where字句, 有如下功能

- 如果没有条件, 不会生成where关键字
- 如果有条件, 会自动添加where关键字
- 如果第一个条件中有and, 去除之

```xml
<select id="selParams" resultType="User">
    select * from myuser
    <where>
        <if test="name!=null and name != ''">
            and name = #{name}
        </if>
        <if test="id != null and id != 0">
            and id = #{id}
        </if>
    </where>
</select>
```

## choose, when, otherwise 的使用

这是一套标签, 功能类似与switch...case... 所以, 只有其中的一条语句会执行

mapper接口: 

```java
/**
     * 动态查询其中的一个属性, 从id开始, 哪个属性有, 就查哪个属性, 都没有就查所有
     * @param user 查询属性的用户
     * @return 返回一个封装所有用户的list
     */
List<User> selForOne(User user);
```

对应的Mapper.xml

```xml
<select id="selForOne" resultType="User" parameterType="user">
    select * from myuser
    <where>
        <choose>
            <when test="name != null and name != ''">
                and name = #{name}
            </when>
            <when test="register != null">
                and register = #{register}
            </when>
            <otherwise>
                and 1=1
            </otherwise>
        </choose>
    </where>
</select>
```

## set标签的使用

> 用于维护update语句中的set字句, 功能如下

- 满足条件时, 会自动添加set关键字
- 会去除set字句中多余的逗号
- 不满足条件时, 不会生成set关键字

```xml
<update id="updUser" parameterType="user">
    update myuser
    <set>
        id = #{id},
        <if test="name != null and name != ''">
            name = #{name},
        </if>
        <if test="register != null">
            register = #{register},
        </if>
    </set>
    where id = #{id}
</update>
```

## trim标签

用于在前后添加或删除一些内容. where set就是用这个东西来实现的

- prefix, 在前面添加内容
- prefixOverrides, 从前面去除内容
- suffix, 向后面添加内容
- suffixOverrides, 从后面去除内容 

```xml
<update id="updUserUseTrim" parameterType="user">
    update myuser
    <trim prefix="set" prefixOverrides="" suffix="" suffixOverrides="," >
        name = #{name},
    </trim>
    where id = #{id}
</update>
```

## bind标签

bind用于对数据进行再加工

```xml
<select id="likeSelUserByName" resultType="user" parameterType="string">
    <bind name="name" value="'%' + name + '%'"/>
    select * from myuser where name like #{name}
</select>
```

## foreach标签

主要用于在sql语句中遍历集合参数, 在in查询中使用

- collection: 待遍历的集合
- open: 设置开始符号
- item: 迭代变量
- separator: 项目分隔符
- close: 设置结束符号 

```xml
<select id="inSqlUserById" parameterType="list" resultType="user">
    select * from myuser where id in
    <foreach collection="list" open="(" separator="," close=")" item="item">
        #{item}
    </foreach>
</select>
```



## include标签和sql标签

`<sql>` 用于提取一段sql语句, `<include>`标签用于引用sql语句

比如

```xml
<sql id="myuser">
    id, name, register, integral, sex
</sql>

<select id="inSqlUserById" parameterType="list" resultType="user">
    select <include refid="myuser"/> from myuser where id in
    <foreach collection="list" open="(" separator="," close=")" item="item">
        #{item}
    </foreach>
</select>
```


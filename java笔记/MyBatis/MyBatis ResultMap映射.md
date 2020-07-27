# MyBatis 的列名映射

## 解决列名和属性名不一致问题

如果查询时使用 resultType 属性, 表示采用 MyBatis 的
Auto-Mapping(自动映射)机制, 即相同的列名和属性名会自
动匹配. 因此, 当数据库表的列名和类的属性名不一致时,
会导致查不到数据. 解决该问题可以有两种方式: 

### 列别名

查询时, 可以通过列别名的方式将列名和属性名保持一致,
继续使用自动映射, 从而解决该问题. 但是较为麻烦. 

```xml
<select id="selAll" resultType="user">
    select id id1, username username1, password password2 from t_user
</select>
```

### 使用resultMap

`<resultMap>`用于自定义映射关系, 可以由程序员自主制定
列名和属性名的映射关系. 一旦使用 resultMap, 表示不再
采用自动映射机制 

```xml
<resultMap type="user" id="umap">
    <!-- id用于映射主键 -->
    <id column="id" property="id1" />
    <!-- 非主键使用result映射 -->
    <result column="username" property="username1" />
    <result column="password" property="password1" />
</resultMap>
<select id="selAll" resultMap="umap">
	select * from t_user
</select>
```

column表示再数据库中的列名, property表示在pojo包中的映射属性名.


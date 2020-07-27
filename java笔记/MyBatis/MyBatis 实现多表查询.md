# MyBatis的多表查询

## 业务装配实现多表查询(多对一)

mapper层只做单表查询操作, 在server层进行手动装配, 实现关联查询的结果

### 实体类

创建班级类(Clazz)和学生类(Student), 并在 Student 中添
加一个 Clazz 类型的属性, 用于表示学生的班级信息 

### mapper层

提供 StudentMapper 和 ClazzMapper, StudentMapper 查询所
有学生信息, ClazzMapper 根据编号查询班级信息 

```xml
<mapper namespace="pers.jssd.mapper.ClazzMapper">
    <select id="selById" resultType="clazz" parameterType="int">
        select * from t_class where id = #{0}
    </select>
</mapper>
```

```xml
<mapper namespace="pers.jssd.mapper.StudentMapper">
    <select id="selAll" resultType="student">
        SELECT * from t_student
    </select>
</mapper>
```

### service层

调用 mapper 层, 先查询所有学生, 再根据每个学生的班级
编号查询班级信息, 手动进行组装, 称之为业务装配. 

```java
@Override
public List<Student> selAll() {
    SqlSession session = MyBatisUtil.getSession();
    StudentMapper studentMapper = session.getMapper(StudentMapper.class);
    ClazzMapper clazzMapper = session.getMapper(ClazzMapper.class);
    List<Student> students = studentMapper.selAll();
    for (Student student : students) {
        student.setClazz(clazzMapper.selById(student.getCid()));
    }
    return students;
}
```

### 测试代码

```java
public static void main(String[] args) {
    StudentService studentService = new StudentServiceImpl();
    List<Student> students = studentService.selAll();
    System.out.println("students = " + students);
}
```

## 配置Mapper中的ResultMap实现N+1查询（多对一）

在StudentMapper中使用`<association>` 设置装配

association: 用于关联一个对象

- property: 指定要关联的对象
- select: 设定要继续引用的查询, namespace+id
- column: 查询时需要传递的列

```xml
<resultMap id="studentResultMap" type="student">
    <!--传递的列不能省略, 不然查询不到值-->
    <result property="cid" column="cid"/>
    <association property="clazz" select="pers.jssd.mapper.ClazzMapper.selById" column="cid"/>
</resultMap>
<select id="selAll" resultType="student" resultMap="studentResultMap">
    SELECT * from t_student
</select>
```

如此配置, 则查询的时候, 调用selAll的方法, 查询Student的时候, 会根据传递过去的值, 同时将clazz查询。那么，在service层就不用手动绑定查询了。

## 配置Mapper中的ResultMap实现联合查询（多对一）

使用sql语句进行联合查询。使用resultMap进行查询属性的映射

xml配置如下：

```xml
<resultMap id="studentResultMap" type="student">
    <!--property 是java类中属性名字，column是查询语句中的名字，或者说是别名-->
    <id property="id" column="sid"/>
    <result property="name" column="sname"/>
    <result property="age" column="age"/>
    <result property="gender" column="gender"/>
    <result property="cid" column="cid"/>
    <!--将association当作是另一个resultMap，javaType当作是type。column是传递的id-->
    <association property="clazz" javaType="clazz" column="cid">
        <id property="id" column="cid"/>
        <result property="name" column="cname"/>
        <result property="room" column="room"/>
    </association>
</resultMap>
<select id="selAll" resultType="student" resultMap="studentResultMap">
    select s.id   sid,
    s.name sname,
    age,
    gender,
    cid,
    c.id   cid,
    c.name cname,
    room
    from t_student s
    inner join t_class c on cid = c.id;
</select>
```

此时，运行查询测试方法，会发现，只执行了一条sql语句，与N+1模式不同。N+1模式执行了多表的N次查询再加一次查询。

## resultMap的N+1方式实现多表查询（一对多）

与多对一方式不同的就是，xml配置的时候使用的不是association标签了，而是一个collection标签。

xml配置如下

```xml
<resultMap id="clazzResultMap" type="clazz">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="room" column="room"/>
    <collection property="stus" select="pers.jssd.mapper.StudentMapper.selByCid" column="id"/>
</resultMap>

<select id="selAll" resultType="clazz" resultMap="clazzResultMap">
    select * from t_class
</select>
```

## resultMap的关联方式实现多表查询（一对多）

同样的，使用resultMap定义映射关系，并通过collection指定集合属性泛型的映射关系，可以把collection看成一个resultMap使用。ofType属性表示集合的泛型，可以写全限定路径或者别名

```xml
<resultMap id="clazzResultMap" type="clazz">
        <id property="id" column="cid"/>
        <result property="name" column="cname"/>
        <result property="room" column="room"/>
        <collection property="stus" javaType="list" ofType="student" column="id">
            <id property="id" column="sin"/>
            <result property="name" column="sname"/>
            <result property="age" column="age"/>
            <result property="gender" column="gender"/>
            <result property="cid" column="cid"/>
        </collection>
    </resultMap>

    <select id="selAll" resultType="clazz" resultMap="clazzResultMap">
        select s.id sid,
               s.name sname,
               age,
               gender,
               cid,
               c.id cid,
               c.name cname,
               room
        from t_student s
                 inner join t_class c on cid = c.id;
    </select>
```

## 使用auto-mapping机制实现多表查询

通过 MyBatis 的 Auto-Mapping 机制及数据库查询时的别
名结合, 可以方便的实现多表查询. 

SQL 语句中, 别名出现特殊符号时, 必须进行处理. MySQL
可以使用(``)符号, Oracle 可以使用("")符号. 

```xml
<!--mysql-->方式
<mapper namespace="com.bjsxt.mapper.StudentMapper">
    <select id="selAll" resultType="student">
        select s.id, s.name, s.age, s.gender, s.cid, c.id `clazz.id`,
        c.name `clazz.name`, c.room `clazz.room`
        from t_student s
        left join t_class c
        on s.cid=c.id
    </select>
</mapper>
```


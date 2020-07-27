[TOC]

# MyBatis 注解开发

## 注解开发

- 注解是用于描述代码的代码. 例如:@Test(用于描述方法
  进行 junit 测试), @Override(用于描述方法的重写),
  @Param(用于描述属性的名称) 
- 注解的使用风格: @xxx(属性), 使用前必须先导包 
- 使用注解一般用于简化配置文件. 但是, 注解有时候也不
  是很友好(有时候反而更麻烦), 例如动态 SQL. 
- 关于注解的属性 
  - 属性的设定方式是: 属性名=属性值
- 关于属性值的类型
  - 基本类型和 String, 可以直接使用双引号的形式 
  - 数组类型, name={值 1, 值 2, ...}; 如果数组元素只有
    一个, 可以省略大括号 
  - 对象类型, name=@对象名(属性) 
  - 如果属性是该注解的默认属性, 而且该注解只配置这
    一个属性, 可以将属性名省略 
- 注解和配置文件可以配合使用

## 常用注解

@Select: 类似于select标签

@Insert: 类似于insert标签

@Update: 类似于update标签

@Delete: 类似于delete标签

### 例如: 

mapper包中接口这样定义: 

```java
@Select("select * from t_student")
List<Student> selAll();
@Insert("insert into t_student values (default, #{name}, #{age},
#{gender}, #{cid})")
int insStu(Student student);
@Update("update t_student set age=#{1} where id=#{0}")
int updStu(int id, int age);
@Delete("delete from t_student where id=#{0}")
int delStu(int id);
```

## 其他标签

@Results: 类似于resultMap标签

@Result: 类似于resultMap的子标签 id和result. 使用id属性来标识是否为主键

@One: 类似于association标签

@Mang: 类似于collection标签

### 示例: 

在ClazzMapper中有接口如此定义

```java
@Select("select * from t_class")
@Results(value = {
    @Result(property = "id", column = "id", id = true),
    @Result(property = "name", column = "name"),
    @Result(property = "room", column = "room"),
    @Result(property = "stus", many = @Many(select = "pers.jssd.mapper.StudentMapper.selByCid"), column = "id")
})
List<Clazz> selAll();
```

StudentMapper接口中有根据cid查询学生的定义

```java
@Select("select * from t_student where cid = #{0}")
List<Student> selByCid(int cid);
```

可查出关联学生类的clazz
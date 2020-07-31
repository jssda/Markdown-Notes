# Spring简介

> Spring :Spring Framework
>
> 创始人：Rod Johnson

## 为什么学习Spring

1. 我们现在使用的mvc模式，发现层和层之间的耦合性太高了
2. 目前书写代码的后期的扩展性不高
3. Spring可以解决层和层之间得耦合问题

## Spring源码和文档链接

```
https://github.com/spring-projects/spring-framework/tags

https://repo.spring.io/libs-release-local/org/springframework/spring/
```



## Spring IOC

>  **Ioc—Inversion of Control，即“控制反转”，不是什么技术，而是一种设计思想** 

实现Spring IOC, 需要导入Spring得几个jar包

(commons-logging被其他几个包所依赖)

![1571297535466](https://raw.githubusercontent.com/jssda/picbed/master/1571297535466.png)

### 到底什么是IOC?

一般情况下, 当我们要新建一个对象得时候, 需要new对象. 但是, 使用new对象的方式, 耦合性太高了. 

那么, 我们将创建对象这个过程, 配置成xml文件,在解析xml得时候, 创建对象. 只要一解析xml文件, xml中配置的所有对象都创建了. 起到了解耦的效果. 

1. 在src下配置applicationContext.xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
       <!--Student  stu =new Student()-->
       <bean id="stu" class="pers.jssd.practise01.Student"></bean>
   </beans>
   ```

   注意, 配置了bean, 就表示在解析这个xml的时候, 创建了一个student对象, id就是变量名

2. 解析xml文档

   ```java
   public static void main(String[] args) {
       ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
       Student stu = app.getBean("stu", Student.class);
   
   }
   ```

这个过程, 就是IOC, 控制反转

> 控制: 就是指的的Spring创建对象的过程
>
> 反转：创建对象这个操作本身是程序员自己做的，现在反交给Spring IOC进行创建

### IOC创建对象的方式

1. 使用无参构造器

   ```xml
   <bean id="stu" class="pers.jssd.practise01.Student"></bean>
   ```

2. 使用有参构造器

   ```xml
    <bean id="stu2" class="pers.jssd.practise02.Student">
        <!--配置构造器参数的顺序可以改变, 参数位置通过index属性配置-->
        <constructor-arg name="name" index="0" type="java.lang.String" value="张三"/>
        <constructor-arg name="sex" value="男"/>
        <constructor-arg name="age" value="18"/>
   </bean>
   ```

   name表示构造器中形参的名字, index表示构造器中的参数位置, type表示类型(通过这三个属性, 即可以定位使用哪个构造器, 避免构造器重载时候识别不出)

3. 使用工厂模式构造

   假如我有一个接口People

   ```java
   public interface People {
       void eat();
   
       void run();
   }
   ```

   两个实现类

   ```java
   public class Student implements People {
   
       public Student() {
           System.out.println("学生创建");
       }
   
       @Override
       public void eat() {
           System.out.println("学生吃");
       }
   
       @Override
       public void run() {
           System.out.println("学生跑");
       }
   }
   
   public class Teacher implements People {
   
       @Override
       public void eat() {
           System.out.println("老师吃");
       }
   
       public Teacher() {
           System.out.println("老师创建");
       }
   
       @Override
       public void run() {
           System.out.println("老师跑");
       }
   }
   ```

   一个创建工厂

   ```java
   public class Factory {
       public People getInstance(String param) {
           if ("teacher".equals(param)) {
               return new Teacher();
           }
           if ("student".equals(param)) {
               return new Student();
           }
           return null;
       }
   }
   ```

   那么, 取得student可以这样配置

   ```xml
   <bean id="factory" class="pers.jssd.practise03.Factory"/>
   
   <bean id="stu3" factory-bean="factory" factory-method="getInstance">
       <constructor-arg name="param" value="student"/>
   </bean>
   ```

   假如getInstance是一个静态方法的时候, 不需要使用factory-bean标签来引入, 可直接这样配置

   ```xml
   <bean id="teacher" class="pers.jssd.practise03.Factory" factory-method="getInstance">
       <constructor-arg name="param" value="teacher"/>
   </bean>
   ```

   

## Spring DI

> 依赖注入: Dependency Injection
>
> 作用：给创建好的对象中的全局的属性或者对象进行赋值的操作

### 一些概念

1. 依赖:一个类在另一个类中作为全局属性时
2. 注入:通过外部给自己属性(或其他内容)进行赋值
3. DI的意义: 解除类与类之间高耦合性，给对象中全局对象赋值的操作
4. Spring帮助创建对象的过程叫做IoC,创建对象时给对象中全局对象赋值叫做DI,所以认为IoC和DI是同一个事情

### 什么是DI?

IOC是创建对象, DI就是给对象符值

Spring中, 给对象符值有三种方法

1. 使用set方法

   我有一个班级类

   ```java
   public class clazz{
   	private String cNo;
       private String cName;
       
       ...带参构造方法, 不带参构造, setget方法...
   }
   ```

   一个学生类

   ```java
   public class Student {
   
       private String name;
       private String sex;
       private int age;
       private Clazz clazz;
       
       ...带参构造方法, 不带参构造, setget方法...
   }
   ```

   使用set方法给创建的对象符值

   注意: 当给一个非基本类型(除了String)符值的时候, 需要用ref属性, 而不是value属性

   ```xml
   <bean id="clazz" class="pers.jssd.practise04.Clazz">
       <constructor-arg name="cNo" value="1"/>
       <constructor-arg name="cName" value="计算机"/>
   </bean>
   
   <bean id="student" class="pers.jssd.practise04.Student">
       <property name="name" value="张三"/>
       <property name="age" value="18"/>
       <property name="sex" value="男"/>
       <property name="clazz" ref="clazz"/>
   </bean>
   ```

2. 使用带参构造器

   使用带参构造器就是`<constructor-arg>`标签

3. 自动注入

   自动注入有三种方式, byName, byType, constructor

   1. 使用byName的时候, Spring会在已经配置的Bean中查找和Student类中的clazz属性名一致的类, 进行自动的引入.
   2. 使用byType的时候, 会根据类型自动的引入
   3. 使用constructor的时候, 根据构造方法自动引入. (强烈不推荐, 必须有相匹配的构造方法)
   4. byName和byType底层调用的是set方法, constructor底层调用的是构造方法

   ```xml
   <bean id="student" class="pers.jssd.practise04.Student" autowire="byName">
       <property name="name" value="刘浩"/>
       <property name="age" value="18"/>
       <property name="sex" value="男"/>
   </bean>
   ```


### Spring DI注入不同的值

A、基本类型（String）

​	使用value注入

B、对象类型

    使用ref注入

```xml
<bean id="student" class="pers.jssd.practise04.Student">
    <property name="name" value="刘浩"/>
    <property name="age" value="18"/>
    <property name="sex" value="男"/>
    <property name="clazz" ref="clazz"/>
</bean>
```

C、数组

```xml
<property name="arr">
    <array>
        <value>1</value>
        <value>2</value>
        <value>3</value>
    </array>
</property>
```

D、List

```xml
<property name="list">
    <list>
        <value>1</value>
        <value>2</value>
        <value>3</value>
    </list>
</property>
```

E、set

```xml
<property name="set">
    <set>
        <value>1</value>
        <value>2</value>
        <value>3</value>
    </set>
</property>
```

F、Map

```xml
<property name="map">
    <map>
        <entry key="1" value="10"/>
        <entry key="2" value="20"/>
        <entry key="3" value="30"/>
    </map>
</property>
```


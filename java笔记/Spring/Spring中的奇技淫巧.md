# Spring 中的奇技淫巧

## Bean 的定义

SpringBean、JavaBean，都是一个对象。

JavaBean：属性是私有的，需要提供对应属性的getter、setter方法。

SpringBean：由Spring管理的对象。可以理解为由Spring生成的对象，均为SpringBean。

### SpringBean 定义方式

1. xml配置定义。 由Bean标签定义一个Bean， 然后Spring容器生成。需要使用ClassPathXmlApplicaitonContext类加载配置文件。

   bean中通过构造方法生成

2. @Bean注解定义一个bean；需要使用AnnotationConfigApplicationContext类，加载配置类。

   手动new出来，更加灵活。

3. 配置类中可以通过 @ComponentScan("pers.jssd") 注解，扫描指定包，包下所有的对象都会注册为bean对象。

4. 通过BeanDefinition对象定义一个bean。 前三种方式其实都是使用第四种方式定义的。

   ```java
   @Test
   public void beanDefinitionTest() {
       AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext();
       annotationConfigApplicationContext.refresh();
   
       AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
       beanDefinition.setBeanClass(User.class);
   
       annotationConfigApplicationContext.registerBeanDefinition("user", beanDefinition);
       Object user = annotationConfigApplicationContext.getBean("user");
       System.out.println("user = " + user);
   }
   ```

5. 使用FactoryBean顶一个bean

   ```java
   package pers.jssd.springboottest.bean;
   
   import org.springframework.beans.factory.FactoryBean;
   import pers.jssd.springboottest.bean.pojo.User;
   
   /**
    * @author wangjingjing@bonc.com.cn
    * @date 2021/3/11 15:25
    */
   public class UserFactoryBean implements FactoryBean<User> {
   
       @Override
       public User getObject() throws Exception {
           return new User();
       }
   
       @Override
       public Class<?> getObjectType() {
           return User.class;
       }
   }
   ```

   ```java
   @Test
   public void factoryBeanTest() {
       AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext();
       annotationConfigApplicationContext.refresh();
   
       AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
       beanDefinition.setBeanClass(UserFactoryBean.class);
   
       annotationConfigApplicationContext.registerBeanDefinition("factory", beanDefinition);
       Object user = annotationConfigApplicationContext.getBean("factory");
       System.out.println("user = " + user);
       Object user1 = annotationConfigApplicationContext.getBean("&factory");
       System.out.println("user1 = " + user1);
   }
   ```

   结果如下：

   ```
   user = pers.jssd.springboottest.bean.pojo.User@27f981c6
   user1 = pers.jssd.springboottest.bean.UserFactoryBean@47d9a273
   ```

   通过FactoryBean定义一个bean之后， 如果把这个Factorybean注册到Spring容器中，他其实会注册两个bean对象，一个是FactoryBean这个对象，另一个是他生产的对象。FactoryBean对象的注册名称为 `&生产的对象名称 `。

6. Supplier对注册的Bean扩展

   ```java
   @Test
   public void supplierTest() {
       AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext();
   
       // registerBean方法实际上也是调用了beanDefinition, 但是在这里, 我们提供了一个 如何构造的方法
       annotationConfigApplicationContext.registerBean(User.class, new Supplier<User>() {
           @Override
           public User get() {
               User user = new User();
               user.setName("111");
               return user;
           }
       });
   
       annotationConfigApplicationContext.refresh();
   
       User user = annotationConfigApplicationContext.getBean("user", User.class);
       System.out.println("user.getName() = " + user.getName());
       System.out.println("user = " + user);
   }
   ```

### 单例和原型

默认情况下, SpringBean 的scope是singleton, 是单例的. 这个单例并不是说他的单例池中只能有一个对象, 而是相同的名字获取到的bean对象都是同一个, 换一个名字他依然可以新建一个对象. 可通过scope属性, 更改bean为prototype, 原型的. 原型的就是同一个名字, 两次获取, 可以获取到不同的对象. 

#### 单例池

单例池是一个ConcurrentHashMap, 变量名字是singletonObject, 存储的key是String, value是Object. key其实就是beanName. 非懒加载的单例Bean.


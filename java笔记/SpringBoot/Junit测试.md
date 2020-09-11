# 1. Junit4和Junit5区别

| junit4                       | junit5                             | 特点                                                   |
| :--------------------------- | :--------------------------------- | :----------------------------------------------------- |
| @Test                        | @Test                              | 声明一个测试方法                                       |
| @BeforeClass                 | @BeforeAll                         | 在当前类的所有测试方法之前执行。注解在【静态方法】上   |
| @AfterClass                  | @AfterAll                          | 在当前类中的所有测试方法之后执行。注解在【静态方法】上 |
| @Before                      | @BeforeEach                        | 在每个测试方法之前执行。注解在【非静态方法】上         |
| @After                       | @AfterEach                         | 在每个测试方法之后执行。注解在【非静态方法】           |
| @RunWith(SpringRunner.class) | @ExtendWith(SpringExtension.class) | 类class定义上                                          |

# 2. 其他常用注解

```java
@AutoConfigureMockMvc // 加载mockMVC测试框架，可自动注入mockMVC类
@SpringBootTest // SpringBoot测试环境
@ExtendWith(SpringExtension.class) // 加载SpringMVC容器
```

## 2.1 @Transactional

该注解加在方法上可以使单元测试进行事务回滚，以保证数据库表中没有因测试造成的垃圾数据，因此保证单元测试可以反复执行；
但是笔者不建议这么做，使用该注解会破坏测试真实性。请参考这篇文章详细理解：

## 2.2 @ExtendWith(@RunWith注解)

- RunWith方法为我们构造了一个的Servlet容器运行运行环境，并在此环境下测试。然而为什么要构建servlet容器？因为使用了依赖注入，注入了MockMvc对象，而在上一个例子里面是我们自己new的。
- 而@AutoConfigureMockMvc注解，该注解表示mockMvc对象由spring 依赖注入构建，你只负责使用就可以了。这种写法是为了让测试在servlet容器环境下执行。

简单的说：**如果你单元测试代码使用了“依赖注入@Resource”就必须加上@ExtendWith，如果你不是手动new MockMvc对象就加上**

## 2.3 @SpringBootTest 注解

是用来创建Spring的上下文ApplicationContext，保证测试在上下文环境里运行。单独使用@SpringBootTest不会启动servlet容器。所以**只是使用SpringBootTest 注解，不可以使用@Resource和@Autowired等注解进行bean的依赖注入**。（准确的说是可以使用，但被注解的bean为null）。

## 2.4 `@WebMvcTest`注解

当使用SpringBootTest的时候，Junit会加载web容器中所有的bean对象，我们可以使用WebMvcTest来只加载Controller层的Bean对象。
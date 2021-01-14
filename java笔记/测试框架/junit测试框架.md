# JUnit使用

## 测试命名规范

java 类, 均在类名后加 Test. java方法, 均在方法前加test.



## junit测试框架

在junit4和junit5中，注解的写法有些许变化。

| junit4                       | junit5                             | 特点                                                         |
| :--------------------------- | :--------------------------------- | :----------------------------------------------------------- |
| @Test                        | @Test                              | 声明一个测试方法                                             |
| @BeforeClass                 | @BeforeAll                         | 在当前类的所有测试方法之前执行。注解在【静态方法】上         |
| @AfterClass                  | @AfterAll                          | 在当前类中的所有测试方法之后执行。注解在【静态方法】上       |
| @Before                      | @BeforeEach                        | 在每个测试方法之前执行。注解在【非静态方法】上               |
| @After                       | @AfterEach                         | 在每个测试方法之后执行。注解在【非静态方法】                 |
| @RunWith(SpringRunner.class) | @ExtendWith(SpringExtension.class) | 类class定义上                                                |
| @Ignore                      | @Disabled                          | 用于禁用测试类或测试方法; 类似于JUnit4的@Ignore。这个注解不能继承。 |




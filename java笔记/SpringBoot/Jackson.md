# 1. Jackson引入之后加载了什么

当我们在Spring Boot应用中集成了jackson的类库之后，如下的一些HttpMessageConverter将会被加载。

| 实现类                               | 功能说明                                                     |
| :----------------------------------- | :----------------------------------------------------------- |
| StringHttpMessageConverter           | 将请求信息转为字符串                                         |
| FormHttpMessageConverter             | 将表单数据读取到MultiValueMap中                              |
| XmlAwareFormHttpMessageConverter     | 扩展与FormHttpMessageConverter，如果部分表单属性是XML数据，可用该转换器进行读取 |
| ResourceHttpMessageConverter         | 读写org.springframework.core.io.Resource对象                 |
| BufferedImageHttpMessageConverter    | 读写BufferedImage对象                                        |
| ByteArrayHttpMessageConverter        | 读写二进制数据                                               |
| SourceHttpMessageConverter           | 读写java.xml.transform.Source类型的对象                      |
| MarshallingHttpMessageConverter      | 通过Spring的org.springframework,xml.Marshaller和Unmarshaller读写XML消息 |
| Jaxb2RootElementHttpMessageConverter | 通过JAXB2读写XML消息，将请求消息转换为标注的XmlRootElement和XmlType连接的类中 |
| MappingJacksonHttpMessageConverter   | 利用Jackson开源包的ObjectMapper读写JSON数据                  |
| RssChannelHttpMessageConverter       | 读写RSS种子消息                                              |
| AtomFeedHttpMessageConverter         | 和RssChannelHttpMessageConverter能够读写RSS种子消息          |

# 2. 常用json处理类库性能比较

在序列化方面，阿里巴巴的fastJson性能最优越，尤其是大并发量的情况下。

FastJson>=Jackson>Gson

反序列化，三种类库基本类似。

**FastJson可能会有一些安全问题**。

# 3. Jackson经常用到的一些注解

## 3.1 时间处理

`@JsonFormat(pattern="yyyy/MM/dd HH:mm:ss", timezone="GMT+8")` 序列化时候会用到

`@dataFormat`:反序列化时候会用到， 可配置在全局中，也就是说前端传入的时间格式可以全局固定。

## 3.2 序列化个性配置

`@JsonPropertyOrder(value={"content","title","name"})`：可配置序列化顺序

`@JsonProperty`：自定义序列化名称

`@JsonInclude(JsonInclude.Include.NON_NULL)`：不需要字段为空的数据序列化。

`@JsonIgnore` 排除某个属性不做序列化与反序列化

# 4. 手动转换

除了在spring框架内实现自动的前后端JSON数据与java对象的转换，我们还可以使用jackson自己写代码进行转换。

```
//jackson的ObjectMapper 转换对象
ObjectMapper mapper = new ObjectMapper();
//将某个java对象转换为JSON字符串
String jsonStr = mapper.writeValueAsString(javaObj);
//将jsonStr转换为Ademo类的对象
Ademo ademo = mapper.readValue(jsonStr, Ademo.class);
```

当JSON字符串代表的对象的字段多于类定义的字段时，使用readValue会抛出UnrecognizedPropertyException异常，在类的定义处加上@JsonIgnoreProperties(ignoreUnknown = true)可以解决这个问题。
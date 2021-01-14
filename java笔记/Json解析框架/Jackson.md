# Jackson 

每次使用 Jackson 都要百度一遍, 用完就忘, 用时再查, 效率低的一批. 做个备忘笔记, 方便以后回顾.



## 核心模块

- jackson-core，核心包，提供基于"流模式"解析的相关 API，它包括 JsonPaser 和 JsonGenerator。 Jackson 内部实现正是通过高性能的流模式 API 的 JsonGenerator 和 JsonParser 来生成和解析 json。

- jackson-annotations，注解包，提供标准注解功能；

- jackson-databind ，数据绑定包， 提供基于"对象绑定" 解析的相关 API （ ObjectMapper ） 和"树模型" 解析的相关 API （JsonNode）；基于"对象绑定" 解析的 API 和"树模型"解析的 API 依赖基于"流模式"解析的 API。

## 数据绑定对象

`ObjectMapper`

ObjectMapper可以从字符串，流或文件中解析JSON，并创建表示已解析的JSON的Java对象。 将JSON解析为Java对象也称为从JSON反序列化Java对象。

ObjectMapper也可以从Java对象创建JSON。 从Java对象生成JSON也称为将Java对象序列化为JSON。

Object映射器可以将JSON解析为自定义的类的对象，也可以解析置JSON树模型的对象。

之所以称为ObjectMapper是因为它将JSON映射到Java对象（反序列化），或者将Java对象映射到JSON（序列化）。



- ObjectMapper 主要用于对 Java 对象（比如 POJO、List、Set、Map 等等）进行序列化与反序列化。
- ObjectMapper 除了能在 json 字符串与 Java 对象之间进行转换，还能将 json 字符串与 JsonNode 进行转换。



## API

| **Java 对象与 Json 字符串的转换**                      |                                                              |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| String writeValueAsString(Object value)                | 1、用于将任何 Java 对象（如 POJO、List、Set、Map等）序列化为 json 字符串，如果对象中某个属性的值为 null，则默认也会序列化为 null；2、如果 value 为 null，返回序列化的结果也返回 null |
| byte[] writeValueAsBytes(Object value)                 | 将 java 对象序列化为 字节数组                                |
| writeValue(File resultFile, Object value)              | 将 java 对象序列化并输出指定文件中                           |
| writeValue(OutputStream out, Object value)             | 将 java 对象序列化并输出到指定字节输出流中                   |
| writeValue(Writer w, Object value)                     | 将 java 对象序列化并输出到指定字符输出流中                   |
| T readValue(String content, Class<T> valueType)        | 1、从给定的 JSON 字符串反序列化为 Java 对象；2、content 为空或者为 null，都会报错 3、valueType 表示反序列化的结果对象，可以是任何 java 对象，比如 POJO、List、Set、Map 等等. |
| T readValue(byte[] src, Class<T> valueType)            | 将 json 内容的字节数组反序列化为 java 对象                   |
| T readValue(File src, Class<T> valueType)              | 将本地 json 内容的文件反序列化为 java 对象                   |
| T readValue(InputStream src, Class<T> valueType)       | 将 json 内容的字节输入流反序列化为 java 对象                 |
| T readValue(Reader src, Class<T> valueType)            | 将 json 内容的字符输入流反序列化为 java 对象                 |
| T readValue(URL src, Class<T> valueType)               | 通过网络 url 地址将 json 内容反序列化为 java 对象            |
| **Json 字符串内容反序列化为 Json 节点对象**            |                                                              |
| JsonNode readTree(String content)                      | 将 JSON 字符串反序列化为 JsonNode 对象，即 json 节点对象     |
| JsonNode readTree(URL source)                          | 对网络上的 json 文件进行反序列化为 json 节点对象             |
| JsonNode readTree(InputStream in)                      | 对 json 文件输入流进行反序列化为 json 节点对象               |
| JsonNode readTree(byte[] content)                      | 对 json 字节数组反序列化为 json 节点对象                     |
| JsonNode readTree(File file)                           | 将本地 json 文件反序列为为 json 节点对象                     |
| **Java 对象与 Json 节点对象的转换**                    |                                                              |
| T convertValue(Object fromValue, Class<T> toValueType) | 将 Java 对象（如 POJO、List、Map、Set 等）序列化为 Json 节点对象。 |
| T treeToValue(TreeNode n, Class<T> valueType)          | json 树节点对象转 Java 对象（如 POJO、List、Set、Map 等等）TreeNode 树节点是整个 json 节点对象模型的根接口。 |

## JsonNode

1、JsonNode 表示 json 节点，整个节点模型的根接口为 TreeNode，json 节点主要用于手动构建 json 对象。

2、JsonNode 有各种数据类型的实现类，其中最常用的就是 ObjectNode 与 ArrayNode，前者表示 json 对象，后者表示 json 对象数组。

3、json 节点对象可以通过 JsonNodeFactory 创建，如 JsonNodeFactory.instance.objectNode();



| **JsonNode json节点通用方法**                                |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| JsonNode get(String fieldName)                               | 用于访问对象节点的指定字段的值，如果此节点不是对象、或没有指定字段名的值，或没有这样名称的字段，则返回 null。 |
| JsonNode get(int index)JsonNode get(int index)               | 访问数组节点的指定索引位置上的值，对于其他节点，总是返回 null.如果索引小于0，或等于、大于节点大小，则返回 null，对于任何索引都不会引发异常。 |
| boolean isArray()                                            | 判断此节点是否为 {@link ArrayNode} 数组节点                  |
| boolean isObject()                                           | 如果此节点是对象节点，则返回 true，否则返回 false            |
| int size()                                                   | 获取 json 节点的大小，比如 json 对象中有多少键值对，或者 json 数组中有多少元素。 |
| ObjectNode deepCopy()                                        | json 节点对象深度复制，相当于克隆                            |
| Iterator<String> fieldNames()                                | 获取 json 对象中的所有 key                                   |
| Iterator<JsonNode> elements()                                | 如果该节点是JSON数组或对象节点，则访问此节点的所有值节点,对于对象节点，不包括字段名（键），只包括值，对于其他类型的节点，返回空迭代器。 |
| boolean has(int index)                                       | 检查此节点是否为数组节点，并是否含有指定的索引。             |
| boolean has(String fieldName)                                | 检查此节点是否为 JSON 对象节点并包含指定属性的值。           |
| ***将 json 属性的值转为 java 数据类型***                     |                                                              |
| int asInt() int asInt(int defaultValue)                      | asInt()：尝试将此节点的值转换为 int 类型，布尔值 false 转换为 0,true 转换为 1。如果不能转换为 int（比如值是对象或数组等结构化类型），则返回默认值 0 ，不会引发异常。asInt(int defaultValue)：设置默认值 |
| boolean asBoolean() boolean asBoolean(boolean defaultValue)  | 尝试将此节点的值转换为 Java 布尔值，0以外的整数映射为true，0映射为false，字符串“true”和“false”映射到相应的值。如果无法转换为布尔值（包括对象和数组等结构化类型），则返回默认值 false，不会引发异常。可以自己设置默认值。 |
| asText(String defaultValue)String asText()                   | 如果节点是值节点（isValueNode 返回 true），则返回容器值的有效字符串表示形式，否则返回空字符串。 |
| long asLong()long asLong(long defaultValue)                  | 与 asInt 同理                                                |
| double asDouble() double asDouble(double defaultValue)       | 尝试将此节点的值转换为 double，布尔值转换为0.0（false）和1.0（true），字符串使用默认的Java 语言浮点数解析规则进行解析。如果表示不能转换为 double（包括对象和数组等结构化类型），则返回默认值 0.0,不会引发异常。 |
| BigInteger bigIntegerValue()                                 | 返回此节点的整数值（BigDecimal），当且仅当此节点为数字时（isNumber}返回true）。对于其他类型，返回 BigInteger.ZERO。 |
| boolean booleanValue()                                       | 用于访问 JSON 布尔值（值文本“true”和“false”）的方法,对于其他类型，始终返回false |
| BigDecimal decimalValue()                                    | 返回此节点的浮点值 BigDecimal, 当且仅当此节点为数字时（isNumber 返回true）,对于其他类型，返回 BigDecimal.ZERO |
| double doubleValue()                                         | 返回此节点的64位浮点（双精度）值，当且仅当此节点为数字时（isNumber返回true），对于其他类型，返回0.0。 |
| float floatValue()                                           | 返回此节点的32位浮点值，当且仅当此节点为数字时（isNumber返回true），对于其他类型，返回0.0。 |
| int intValue() long longValue() Number numberValue() short shortValue() String textValue() | 返回此节点的 int、long、Number、short、String 值。           |
| **ObjectNode 对象节点常用方法**                              |                                                              |
| ObjectNode put(String fieldName, String v) ObjectNode put(String fieldName, int v) | 1、将字段的值设置为指定的值，如果字段已经存在，则更新值，value 可以为 null.2、其它 8 种基本数据类型以及 String、BigDecimal、BigInteger 都可以 put，但是没有 Date 类型3、Date 日期类型只能通过 Long 长整型设置 |
| ArrayNode putArray(String fieldName)                         | 构造新的 ArrayNode 子节点，并将其作为此 ObjectNode 的字段添加。 |
| ObjectNode putNull(String fieldName):                        | 为指定字段添加 null 值，put                                  |
| ObjectNode putObject(String fieldName)                       | 构造新的 ObjectNode 字节的，并将其作为此 ObjectNode 的字段添加。 |
| *替换与删除元素*                                             |                                                              |
| JsonNode replace(String fieldName, JsonNode value)           | 将特定属性的值替换为传递的值，字段存在时更新，不存在时新增   |
| JsonNode set(String fieldName, JsonNode value)               | 设置指定属性的值为 json 节点对象，字段存在时更新，不存在时新增，类似 replace 方法 |
| JsonNode setAll(Map<String,? extends JsonNode> properties)   | 同时设置多个 json 节点                                       |
| JsonNode setAll(ObjectNode other)                            | 添加给定对象（other）的所有属性，重写这些属性的任何现有值.   |
| ArrayNode withArray(String propertyName)                     | 将 json 节点转为 json 数组对象                               |
| ObjectNode with(String propertyName)                         | 将 json 节点转为 ObjectNode 对象                             |
| JsonNode remove(String fieldName)                            | 删除指定的 key，返回被删除的节点                             |
| ObjectNode remove(Collection<String> fieldNames)             | 同时删除多个字段                                             |
| ObjectNode removeAll()                                       | 删除所有字段属性                                             |
| JsonNode without(String fieldName):                          | 删除指定的 key，底层也是 remove                              |
| ObjectNode without(Collection<String> fieldNames)            | 同时删除多个字段，底层也是 removeAll                         |
| **ArrayNode 数组节点常用方法**                               |                                                              |
| ArrayNode add(String v)                                      | 将指定的字符串值添加到此 json 数组的末尾，其它数据类型也是同理。除了可以添加 String 类型，还有 Java 的 8 种基本数据类型，以及 BigDecimal、BigInteger、JsonNode 类型。 |
| ArrayNode addAll(ArrayNode other)                            | 用于添加给定数组的所有子节点                                 |
| ArrayNode addNull()                                          | 该方法将在此数组节点的末尾添加 null 值。                     |
| ArrayNode addArray()                                         | 构造新的 ArrayNode 子节点，并将其添加到此数组节点的末尾      |
| ObjectNode addObject()                                       | 构造一个新的 ObjectNode 字节的，并将其添加到此数组节点的末尾 |



## 注解使用

### 3.1. @JsonProperty

使用在`JavaBean`的字段上，指定一个字段用于`JSON`映射，默认情况下映射的`JSON`字段与注解的字段名称相同。该注解有三个属性：
 （1）`value`：用于指定映射的`JSON`的字段名称。常用。
 （2）`index`：用于指定映射的`JSON`的字段顺序。
 （3）`defaultValue`：定义为元数据的文本默认值。注意：`core databind`不使用该属性，它目前只公开给扩展模块使用。

```css
@JsonProperty(value = “user_name”)
```

### 3.2. @JsonIgnore

可用于字段、getter/setter、构造函数参数上，作用相同，都会对相应的字段产生影响。**使相应字段不参与序列化和反序列化。**也就是说，向“getter”添加注释会禁用“setter”。除非setter有`@JsonProperty`注解，在这种情况下，这被认为是一个“分割属性”，启用了“setter”，但没有“getter”(“只读”，因此属性可以从输入读取，但不是写输出)。

### 3.3. @JsonIgnoreProperties

该注解是类注解。该注解在Java类和JSON不完全匹配的时候使用。
 （1）在序列化为JSON的时候，`@JsonIgnoreProperties({"prop1", "prop2"})`会忽略pro1和pro2两个属性。
 （2）在从JSON反序列化为Java类的时候，`@JsonIgnoreProperties(ignoreUnknown=true)`会忽略所有没有Getter和Setter的属性，也就是忽略类中不存在的字段。

### 3.4. @JsonIgnoreType

该注解是类注解，序列化为JSON的时候会排除所有指定类型的字段。

### 3.5. @JsonInclude

用于定义在序列化时是否不应包含某些“非值”（null值或空值）的注解。可以用于每个字段上，也可以用于类上（表示用于类的所有属性）。



```kotlin
//忽略类中值为null的字段
@JsonInclude(value = JsonInclude.Include.NON_NULL)
//忽略类中值为空的字段。对于字符串，即忽略null或空字符串
@JsonInclude(Include.NON_EMPTY)
```

### 3.6. @JsonFormat

用于字段上，预期类型行为的通用注释；例如，可以用来指定序列化日期/时间值时使用的格式。
 `java.util.Date`使用如下，`java.sql.Date`类似。（注意时区问题，这里添加了`timezone = "GMT+8"`）



```kotlin
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;
public class DateModel {
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
    private Date date;

    public Date getDate() {return date;}
    public void setDate(Date date) {this.date = date;}
}
```



```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.util.Date;
public class Test {
    public static void main(String[] args) throws JsonProcessingException {
        DateModel dateModel = new DateModel();
        dateModel.setDate(new Date());
        ObjectMapper mapper = new ObjectMapper();
        System.out.println(mapper.writeValueAsString(dateModel));
        //{"date":"2020-01-01 12:16:54"}
    }
}
```

但是注意如果`JavaBean`中的时间字段使用的是`JDK8`新增的时间日期（`LocalDate / LocalTime / LocalDateTime`）字段的话，直接这样使用是不起作用的。我们需要添加其他匹配，具体可参考`GitHub`上的说明：[Jackson格式化JDK8日期](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FFasterXML%2Fjackson-modules-java8)
 （1）添加`jackson-datatype-jsr310`的`maven`配置，`SpringBoot`的`web`模块会自动引入。
 （2）需要进行模块注册。具体看下面的示例代码。



```kotlin
import com.fasterxml.jackson.annotation.JsonFormat;
import java.time.LocalDateTime;
public class DateModel {
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime date;

    public LocalDateTime getDate() {return date;}
    public void setDate(LocalDateTime date) {this.date = date;}
}
```



```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.time.LocalDateTime;
public class Test {
    public static void main(String[] args) throws JsonProcessingException {
        DateModel dateModel = new DateModel();
        dateModel.setDate(LocalDateTime.now());
        ObjectMapper mapper = new ObjectMapper();
        //自动发现并注册模块
        mapper.findAndRegisterModules();
        System.out.println(mapper.writeValueAsString(dateModel));
        //{"date":"2020-01-01 12:40:08"}
    }
}
```

### 3.7. @JsonPropertyOrder

和@JsonProperty的index属性类似，指定属性序列化时的顺序。

### 3.8. @JsonRootName

类注解。用于指定`JSON`根属性的名称。生成的`JSON`如下所示：



```json
{"Teacher":{"id":2,"name":"wangwu","age":35}}
```

**示例代码：**



```kotlin
@JsonRootName("Teacher")
public class Teacher {
    private Long id;
    private String name;
    private Integer age;
    @JsonIgnore//转换为JSON时不需要的字段，用在属性上。
    private String sex;

    //省略Setter/Getter方法

    @Override
    public String toString() {
        return "Teacher{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
    }
}
```



```dart
        Teacher teacher = new Teacher();
        teacher.setId(2L);
        teacher.setName("wangwu");
        teacher.setAge(35);
        teacher.setSex("男");

        ObjectMapper mapper = new ObjectMapper();
        //开启包装根植的配置
        mapper.enable(SerializationFeature.WRAP_ROOT_VALUE);

        //将Java对象转换为JSON字符串
        String teacherStr = mapper.writeValueAsString(teacher);
        System.out.println(teacherStr);
        //{"Teacher":{"id":2,"name":"wangwu","age":35}}

        //开启了根包装之后，生成的json字符串和java类不对应了，
        //所以在反序列化为java类的时候会报错，关闭该属性不会报错，但是值会为空
        mapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);

        //将JSON字符串转换为Java对象
        Teacher tea = mapper.readValue(teacherStr, Teacher.class);
        System.out.println(tea);
        //Teacher{id=null, name='null', age=null, sex='null'}
```

### 3.9. @JsonAnySetter和@JsonAnyGetter

这两个属性是用来在序列化和反序列化的时候多余字段可以通过`Map`来回转换。也就是`JSON`中的字段比对应的`JavaBean`中的字段多，可以在`JavaBean`中使用一个`Map`字段来接收多余的`JSON`字段。

#### 3.9.1. @JsonAnyGetter

（1）用在非静态方法上，没有参数，方法名随意（可以直接写在`Getter`方法上）。
 （2）方法返回值必须是`Map`类型。
 （3）在一个实体类中仅仅用在一个方法上。
 （4）序列化的时候`JSON`字段的`key`就是返回`Map`的`key`，`value`就是`Map`的`value`。

#### 3.9.2. @JsonAnySetter

（1）用在非静态方法上，注解的方法必须有两个参数，第一个是`JSON`字段中的`key`，第二个是`value`，方法名随意（注意这个方法不是`Setter`方法）。
 （2）也可以用在`Map`对象属性上面，建议用在`Map`对象属性上面。
 （3）反序列化的时候将对应不上的字段全部放到`Map`里面。

#### 3.9.3. 示例代码



```tsx
import com.fasterxml.jackson.annotation.*;
import lombok.Data;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
/**
 * @author wangbo
 * @date 2019/11/16 10:47
 */
@Data
public class Student {
    private Long id;
    private String name;
    private Integer age;
    //自定义字段
    private Map<String, Object> other = new HashMap();

    @JsonAnyGetter
    public Map<String, Object> getOther() {
        return other;
    }

    @JsonAnySetter
    public void setOther(String key, Object value) {
        this.other.put(key, value);
    }
}
```



```swift
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

/**
 * @author wangbo
 * @date 2019/11/16 16:54
 */
public class Test {
    public static void main(String[] args) throws IOException {
        Map<String,Object> map = new HashMap<>();
        map.put("id", 1L);
        map.put("name", "菲菲");
        map.put("age", 20);
        map.put("score", 90);
        map.put("sex", "女");

        ObjectMapper mapper = new ObjectMapper();
        String s = mapper.writeValueAsString(map);//序列化
        System.out.println(s);
        //{"score":90,"sex":"女","name":"菲菲","id":1,"age":20}

        Student student = mapper.readValue(s, Student.class);//反序列化
        System.out.println(student);
        //Student(id=1, name=菲菲, age=20, other={score=90, sex=女})
        String s1 = mapper.writeValueAsString(student);//序列化
        System.out.println(s1);
        //{"id":1,"name":"菲菲","age":20,"score":90,"sex":"女"}
    }
}
```

### 3.10. @JsonNaming

该注解放在类上。序列化的时候该注解可将驼峰命名的字段名转换为下划线分隔的小写字母命名方式的`key`。反序列化的时候可以将下划线分隔的小写字母`key`转换为驼峰命名的字段名。



```css
@JsonNaming(PropertyNamingStrategy.SnakeCaseStrategy.class)
```

**代码示例：**



```kotlin
import com.fasterxml.jackson.databind.PropertyNamingStrategy;
import com.fasterxml.jackson.databind.annotation.JsonNaming;
import lombok.Data;
/**
 * @author wangbo
 * @date 2019/11/16 10:47
 */
@Data
@JsonNaming(PropertyNamingStrategy.SnakeCaseStrategy.class)
public class Student {
    private Long appId;
    private String nickName;
    private Integer nowAge;
}
```



```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.IOException;
/**
 * @author wangbo
 * @date 2019/11/16 16:54
 */
public class Test {
    public static void main(String[] args) throws IOException {
        Student student = new Student();
        student.setAppId(1L);
        student.setNickName("zhangsan");
        student.setNowAge(20);

        ObjectMapper mapper = new ObjectMapper();
        String s = mapper.writeValueAsString(student);//序列化
        System.out.println(s);
        //{"app_id":1,"nick_name":"zhangsan","now_age":20}
        Student student1 = mapper.readValue(s, Student.class);//反序列化
        System.out.println(student1);
        //Student(appId=1, nickName=zhangsan, nowAge=20)
    }
}
```

### 4. Jackson配置

这里有三个方法，`configure`方法接受配置名和要设置的值，`Jackson 2.5`版本新加的`enable`和`disable`方法则直接启用和禁用相应属性，推荐使用后面两个方法。



```cpp
// 美化输出
mapper.enable(SerializationFeature.INDENT_OUTPUT);
// 强制JSON空字符串("")转换为null对象值
mapper.enable(DeserializationFeature.ACCEPT_EMPTY_STRING_AS_NULL_OBJECT);

// 允许序列化空的POJO类（否则会抛出异常）
mapper.disable(SerializationFeature.FAIL_ON_EMPTY_BEANS);
// 把java.util.Date, Calendar输出为数字（时间戳）
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
// 在遇到未知属性的时候不抛出异常
mapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);

// 在JSON中允许C/C++ 样式的注释(非标准，默认禁用)
mapper.configure(JsonParser.Feature.ALLOW_COMMENTS, true);
// 允许没有引号的字段名（非标准）
mapper.configure(JsonParser.Feature.ALLOW_UNQUOTED_FIELD_NAMES, true);
// 允许单引号（非标准）
mapper.configure(JsonParser.Feature.ALLOW_SINGLE_QUOTES, true);
// 强制转义非ASCII字符
mapper.configure(JsonGenerator.Feature.ESCAPE_NON_ASCII, true);
// 将内容包裹为一个JSON属性，属性名由@JsonRootName注解指定
mapper.configure(SerializationFeature.WRAP_ROOT_VALUE, true);
```



## SpringBoot中的配置

1. spring.jackson.date-format= 配置日期序列化和反序列化格式, `yyyy-MM-dd HH:mm:ss`.
2. spring.jackson.default-property-inclusion= 控制序列化期间包含的属性。配置了Jackson的JsonInclude.Include枚举中的一个值，若配置一般配置`non_null`表示序列化时忽略属性为null的值，`always, non_null, non_absent, non_default, non_empty`
3. spring.jackson.deserialization.*= Jackson反序列化的开关，取值`true, false`
4. spring.jackson.generator.*= 开启关闭jackson的生成器，取值`true, false`
5. spring.jackson.joda-date-time-format= # 配置日期格式序列化为string的格式，不配置默认使用date-format配置
6. spring.jackson.locale= 本地化配置
7. spring.jackson.mapper.*= 开启或者关闭jackson，取值`true, false`
8. spring.jackson.parser.*= 开启关闭jsonson的解析器 ，取值`true, false`
9. spring.jackson.property-naming-strategy=配置json的key值和实体属性名称之间的转换关系，值一般为PropertyNamingStrategy类中常数或者实现PropertyNamingStrategy子类的全限定名
10. spring.jackson.serialization.*= Jackson序列化的开关，取值`true, false`
11. spring.jackson.time-zone= 格式化日期时使用的时区。例如，“America / Los_Angeles”或“GMT + 10”
12. spring.jackson.visibility.*= 修改实体类属性域的可见性



## 自定义序列化反序列化

### 序列化

当Jackson 默认序列化和反序列化的类不能满足实际需要，可以自定义新的序列化和反序列化的类。

自定义序列化类：自定义的序列化类需要直接或间接继承StdSerializer 或JsonSerializer，同时需要利用JsonGenerator 生成json，重写方法serialize，示例如下：

```java
public static class CustomSerializer extends StdSerializer<Person> {
    protected CustomSerializer() {
        super(Person.class);
    }

    @Override
    public void serialize(Person person, JsonGenerator jgen, SerializerProvider provider) throws IOException {
        jgen.writeStartObject();
        jgen.writeNumberField("age", person.getAge());
        jgen.writeStringField("name", person.getName());
        jgen.writeStringField("msg", "已被自定義序列化");
        jgen.writeEndObject();
    }
}
```

JsonGenerator 有多种write 方法以支持生成复杂的类型的json，比如writeArray，writeTree 等。若想单独创建JsonGenerator，可以通过JsonFactory() 的createGenerator。

自定义反序列化类：自定义的反序列化类需要直接或间接继承StdDeserializer 或StdDeserializer，同时需要利用JsonParser 读取json，重写方法deserialize，示例如下：

```java
public static class CustomDeserializer extends StdDeserializer<Person> {
    protected CustomDeserializer() {
        super(Person.class);
    }

    @Override
    public Person deserialize(JsonParser jp, DeserializationContext ctxt)
            throws IOException, JsonProcessingException {
        JsonNode node = jp.getCodec().readTree(jp);
        Person person = new Person();
        int age = (Integer) ((IntNode) node.get("age")).numberValue();
        String name = node.get("name").asText();
        person.setAge(age);
        person.setName(name);
        return person;
    }
}
```

JsonParser 提供很多方法来读取json 信息， 如isClosed(), nextToken(), getValueAsString()等。若想单独创建JsonParser，可以通过JsonFactory() 的createParser。

定义好自定义序列化类和自定义反序列化类，若想在程序中调用它们，还需要注册到ObjectMapper 的Module，示例如下：

```java
@Test
public void test9() throws IOException {
    ObjectMapper mapper = new ObjectMapper();
    // 生成 module
    SimpleModule module = new SimpleModule("myModule");
    module.addSerializer(new CustomSerializer());
    module.addDeserializer(Person.class, new CustomDeserializer());
    // 註冊 module
    mapper.registerModule(module);
    // 造數據
    Person person = new Person();
    person.setName("Tom");
    person.setAge(40);
    person.setDate(new Date());
    System.out.println("序列化");
    String jsonString = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(person);
    System.out.println(jsonString);
    System.out.println("反序列化");
    Person deserializedPerson = mapper.readValue(jsonString, Person.class);
    System.out.println(deserializedPerson);
}
```

或者也可通过注解方式加在java 对象的属性，方法或类上面来调用它们：

- @JsonSerialize（使用= CustomSerializer.class）
- @JsonDeserialize（使用= CustomDeserializer.class）



## 树模型处理

Jackson 也提供了树模型(tree model)来生成和解析json。若想修改或访问json 部分属性，树模型是不错的选择。树模型由JsonNode 节点组成。程序中常常使用ObjectNode，ObjectNode 继承于JsonNode，示例如下：



```java
@Test
public void test10() throws IOException {
    ObjectMapper mapper = new ObjectMapper();
    //構建 ObjectNode
    ObjectNode personNode = mapper.createObjectNode();
    //添加/更改屬性
    personNode.put("name", "Tom");
    personNode.put("age", 40);
    ObjectNode addressNode = mapper.createObjectNode();
    addressNode.put("zip", "000000");
    addressNode.put("street", "Road NanJing");
    //設置子節點
    personNode.set("address", addressNode);
    System.out.println("構建 ObjectNode:\n" + personNode.toString());
    //通過 path 查找節點
    JsonNode searchNode = personNode.path("name");
    System.out.println("查找子節點 name:\n" + searchNode.asText());
    //刪除屬性
    ((ObjectNode) personNode).remove("address");
    System.out.println("刪除後的 ObjectNode:\n" + personNode.toString());
    //讀取 json
    JsonNode rootNode = mapper.readTree(personNode.toString());
    System.out.println("Json 轉 JsonNode:\n" + rootNode);
    //JsonNode 轉換成 java 對象
    Person person = mapper.treeToValue(personNode, Person.class);
    System.out.println("JsonNode 轉對象:\n" + person);
    //java 對象轉換成 JsonNode
    JsonNode node = mapper.valueToTree(person);
    System.out.println("對象轉 JsonNode:\n" + node);
}
```

打印输出：

```shell
構建 ObjectNode:
{"name":"Tom","age":40,"address":{"zip":"000000","street":"Road NanJing"}}
查找子節點 name:
Tom
刪除後的 ObjectNode:
{"name":"Tom","age":40}
Json 轉 JsonNode:
{"name":"Tom","age":40}
JsonNode 轉對象:
JackSonTest.Person(name=Tom, age=40, date=null, height=0)
對象轉 JsonNode:
{"name":"Tom","age":40,"date":null,"height":0}
```













































































# 参考文章

https://juejin.cn/post/6844904166809157639

https://blog.csdn.net/wangmx1993328/article/details/88598625

https://www.jianshu.com/p/204f5fbd3363

https://blog.csdn.net/NDKHBWH/article/details/98848770

https://www.mdeditor.tw/pl/p3OQ/zh-hk
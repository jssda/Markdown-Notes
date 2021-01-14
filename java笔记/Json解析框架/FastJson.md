# FashJson

## 序列化api

```java
 // 将Java对象序列化为JSON字符串，支持各种各种Java基本类型和JavaBean
    public static String toJSONString(Object object, SerializerFeature... features);

    // 将Java对象序列化为JSON字符串，返回JSON字符串的utf-8 bytes
    public static byte[] toJSONBytes(Object object, SerializerFeature... features);

    // 将Java对象序列化为JSON字符串，写入到Writer中
    public static void writeJSONString(Writer writer, 
                                       Object object, 
                                       SerializerFeature... features);

    // 将Java对象序列化为JSON字符串，按UTF-8编码写入到OutputStream中
    public static final int writeJSONString(OutputStream os, // 
                                            Object object, // 
                                            SerializerFeature... features);

```



## 反序列化api

```java
// 将JSON字符串反序列化为JavaBean
    public static <T> T parseObject(String jsonStr, 
                                    Class<T> clazz, 
                                    Feature... features);

    // 将JSON字符串反序列化为JavaBean
    public static <T> T parseObject(byte[] jsonBytes,  // UTF-8格式的JSON字符串
                                    Class<T> clazz, 
                                    Feature... features);

    // 将JSON字符串反序列化为泛型类型的JavaBean
    public static <T> T parseObject(String text, 
                                    TypeReference<T> type, 
                                    Feature... features);

    // 将JSON字符串反序列为JSONObject
    public static JSONObject parseObject(String text);
```



## 定制序列化

fastjson支持多种方式定制序列化。

- 通过@JSONField定制序列化
- 通过@JSONType定制序列化
- 通过SerializeFilter定制序列化
- 通过ParseProcess定制反序列化



### 使用@JSONField配置

可以把@JSONField配置在字段或者getter/setter方法上。例如：

```java
 public class VO {
      @JSONField(name="ID")
      private int id;
 }
```

或者

```java
 public class VO {
      private int id;

      @JSONField(name="ID")
      public int getId() { return id;}

      @JSONField(name="ID")
      public void setId(int value) {this.id = id;}
 }
```



**JSONField 介绍**

注意：1、若属性是私有的，必须有set*方法。否则无法反序列化。

```java
package com.alibaba.fastjson.annotation;

public @interface JSONField {
    // 配置序列化和反序列化的顺序，1.1.42版本之后才支持
    int ordinal() default 0;

     // 指定字段的名称
    String name() default "";

    // 指定字段的格式，对日期格式有用
    String format() default "";

    // 是否序列化
    boolean serialize() default true;

    // 是否反序列化
    boolean deserialize() default true;
}
```

**使用format配置日期格式化**

```java
 public class A {
      // 配置date序列化和反序列使用yyyyMMdd日期格式
      @JSONField(format="yyyyMMdd")
      public Date date;
 }
```

**使用serialize/deserialize指定字段不序列化**

```java
 public class A {
      @JSONField(serialize=false)
      public Date date;
 }

 public class A {
      @JSONField(deserialize=false)
      public Date date;
 }
```

### 使用serializeUsing制定属性的序列化类

在fastjson 1.2.16版本之后，JSONField支持新的定制化配置serializeUsing，可以单独对某一个类的某个属性定制序列化，比如：

```java
public static class Model {
    @JSONField(serializeUsing = ModelValueSerializer.class)
    public int value;
}

public static class ModelValueSerializer implements ObjectSerializer {
    @Override
    public void write(JSONSerializer serializer, Object object, Object fieldName, Type fieldType,
                      int features) throws IOException {
        Integer value = (Integer) object;
        String text = value + "元";
        serializer.write(text);
    }
}
```

### JSONField jsonDirect

在fastjson-1.2.12版本中，JSONField支持一个新的配置项jsonDirect，它的用途是：当你有一个字段是字符串类型，里面是json格式数据，你希望直接输入，而不是经过转义之后再输出。

#### Model

```java
import com.alibaba.fastjson.annotation.JSONField;

public static class Model {
    public int id;
    @JSONField(jsonDirect=true)
    public String value;
}
```

#### Usage

```java
Model model = new Model();
model.id = 1001;
model.value = "{}";

String json = JSON.toJSONString(model);
Assert.assertEquals("{\"id\":1001,\"value\":{}}", json);
```

### 使用@JSONType配置

和JSONField类似，但JSONType配置在类上，而不是field或者getter/setter方法上。

### 通过SerializeFilter定制序列化

通过SerializeFilter可以使用扩展编程的方式实现定制序列化。fastjson提供了多种SerializeFilter：

- PropertyPreFilter 根据PropertyName判断是否序列化
- PropertyFilter 根据PropertyName和PropertyValue来判断是否序列化
- NameFilter 修改Key，如果需要修改Key,process返回值则可
- ValueFilter 修改Value
- BeforeFilter 序列化时在最前添加内容
- AfterFilter 序列化时在最后添加内容

以上的SerializeFilter在JSON.toJSONString中可以使用。

```java
  SerializeFilter filter = ...; // 可以是上面5个SerializeFilter的任意一种。
  JSON.toJSONString(obj, filter);
```

**PropertyFilter 根据PropertyName和PropertyValue来判断是否序列化**

```
  public interface PropertyFilter extends SerializeFilter {
      boolean apply(Object object, String propertyName, Object propertyValue);
  }
```

可以通过扩展实现根据object或者属性名称或者属性值进行判断是否需要序列化。例如：

```
    PropertyFilter filter = new PropertyFilter() {

        public boolean apply(Object source, String name, Object value) {
            if ("id".equals(name)) {
                int id = ((Integer) value).intValue();
                return id >= 100;
            }
            return false;
        }
    };

    JSON.toJSONString(obj, filter); // 序列化的时候传入filter
```

**PropertyPreFilter 根据PropertyName判断是否序列化**

和PropertyFilter不同只根据object和name进行判断，在调用getter之前，这样避免了getter调用可能存在的异常。

```
 public interface PropertyPreFilter extends SerializeFilter {
      boolean apply(JSONSerializer serializer, Object object, String name);
  }
```

**NameFilter 序列化时修改Key**

如果需要修改Key,process返回值则可

```
public interface NameFilter extends SerializeFilter {
    String process(Object object, String propertyName, Object propertyValue);
}
```

fastjson内置一个PascalNameFilter，用于输出将首字符大写的Pascal风格。 例如：

```
import com.alibaba.fastjson.serializer.PascalNameFilter;

Object obj = ...;
String jsonStr = JSON.toJSONString(obj, new PascalNameFilter());
```

**ValueFilter 序列化是修改Value**

```
  public interface ValueFilter extends SerializeFilter {
      Object process(Object object, String propertyName, Object propertyValue);
  }
```

**BeforeFilter 序列化时在最前添加内容**

> 在序列化对象的所有属性之前执行某些操作,例如调用 writeKeyValue 添加内容

```
  public abstract class BeforeFilter implements SerializeFilter {
      protected final void writeKeyValue(String key, Object value) { ... }
      // 需要实现的抽象方法，在实现中调用writeKeyValue添加内容
      public abstract void writeBefore(Object object);
  }
```

**AfterFilter 序列化时在最后添加内容**

> 在序列化对象的所有属性之后执行某些操作,例如调用 writeKeyValue 添加内容

```
  public abstract class AfterFilter implements SerializeFilter {
      protected final void writeKeyValue(String key, Object value) { ... }
      // 需要实现的抽象方法，在实现中调用writeKeyValue添加内容
      public abstract void writeAfter(Object object);
  }
```









## 反序列化

### JSONField alternateNames

在fastjson在1.2.21版本中提供了一个借鉴自gson的特性，支持反序列化时使用多个不同的字段名称，使用的方式是配置JSONField的alternateNames。

**Demo**

```java
public static class Model {
    public int id;

    @JSONField(alternateNames = {"user", "person"})
    public String name;
}

String jsonVal0 = "{\"id\":5001,\"name\":\"Jobs\"}";
String jsonVal1 = "{\"id\":5382,\"user\":\"Mary\"}";
String jsonVal2 = "{\"id\":2341,\"person\":\"Bob\"}";

Model obj0 = JSON.parseObject(jsonVal0, Model.class);
assertEquals(5001, obj0.id);
assertEquals("Jobs", obj0.name);

Model obj1 = JSON.parseObject(jsonVal1, Model.class);
assertEquals(5382, obj1.id);
assertEquals("Mary", obj1.name);

Model obj2 = JSON.parseObject(jsonVal2, Model.class);
assertEquals(2341, obj2.id);
assertEquals("Bob", obj2.name);
```

### ParseProcess

定制反系列化API [ParseProcess](https://www.w3cschool.cn/fastjson/fastjson-parseprocess.html)





































# 参考文章

官方wiki: https://github.com/alibaba/fastjson/wiki/JSON_API_cn

https://www.w3cschool.cn/fastjson/fastjson-api.html
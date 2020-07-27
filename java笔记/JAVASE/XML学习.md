# 	XML学习

## XML概念和体系

### XML概念

> XML指可扩展标记语言(EXtensible Markup Language)
>
> XML没有预定义标签, 需要自行定义标签

### XML的特点

1. XML数据以纯文本格式存储
2. 实现不同应用程序之间的数据通信
3. 实现不同平台间的数据通信
4. 实现不同平台间的数据共享
5. 使用XML将不同的程序, 不同的平台之间联系起来

### XML的作用

数据存储和数据传输

### XML和HTML之间的差异

XML主要作用是数据存储和传输(传输)

HTML 主要作用是用来显示数据 (显示)

### 一个标准的XML文档

树状结构

```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
	<book id="1001">
    	<name>java开发实战强化</name>
        <author>张小三</author>
        <price>98.5</price>
    </book>
</books>
```

### XML文件的体系

1. XHTML 可扩展标识语言
2. WSDL:  Web Services Description Language  网络服务描述语言, 这种文档可描述某个Web service
3. WAP 和 WML 手持设备标记语言
4. RSS (广泛用于网上新闻频道): Really Simple Syndication 简易信息聚合, 使用RSS订阅能更块地获取信息, 网站提供RSS输出, 有利于让用户获取网站内容的最新更新
5. RDF 和 OWL: 重要的语义网技术是为资产管理, 企业整合及网络数据的共享和重用提供的一个框架
6. SMIL 同步多媒体集成语言, 他是由万维网联盟规定的多媒体操纵语言, 最新的SMIL版本是2001年8月推出的SMIL2.0 版本, 它通过时许排列对声音, 影像, 文字及图形文件进行顺序安排, 然后将这些媒体表现看起来是同步的. 

## XML的基本语法

### 基本语法

1. 有且只有一个根元素
2. XML文档声明必须放在文档的第一行
3. 所有标签必须成对出现
4. XML的标签**严格区分**大小写
5. XML必须正确嵌套
6. XML中的属性值必须加引号
7. XML中, 一些特殊字符需要使用"实体"
8. XML中可以应用适当的注释   格式为: <!--注释内容-->

### XML元素

> XML元素指的是开始标签到结束标签的部分

一个元素可以包含: 

- 其他元素
- 文本
- 属性
- 属性值
- 以上的混合

### XML的命名规则

1. 名称可以包含字母, 数字及其他字符
2. 名称不能以数字或者标点符号开始
3. 名称不能以字母xml开始
4. 名称不能包含空格

### XML格式

1. data作为属性

   ```xml
   <note date="10/01/2008">
       <to>Tove</to>
       <from>Jani</from>
       <heading>Reminder</heading>
       <body>Don't forget me this weekend!</body>
   </note>
   ```

2. data作为一整个元素

   ```xml
   <note>
       <date>10/01/2008</date>
       <to>Tove</to>
       <from>Jani</from>
       <heading>Reminder</heading>
       <body>Don't forget me this weekend!</body>
   </note>
   ```

3. data作为一个根元素, 最建议使用

   ```xml
   note>
       <date>
           <day>10</day>
           <month>01</month>
           <year>2008</year>
       </date>
       <to>Tove</to>
       <from>Jani</from>
       <heading>Reminder</heading>
       <body>Don't forget me this weekend!</body>
   </note>
   ```

   为什么避免使用XML属性? 

   - 属性不能包含多个值(元素可以)
   - 属性不能包含树结构(元素可以)
   - 属性不容易扩展(为未来的变化)

## 命名空间

> 在 XML 中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

### 使用前缀来避免命名冲突

例如下面两个xml

```xml
<h:table>
   <h:tr>
   <h:td>Apples</h:td>
   <h:td>Bananas</h:td>
   </h:tr>
</h:table>
```

```xml
<f:table>
   <f:name>African Coffee Table</f:name>
   <f:width>80</f:width>
   <f:length>120</f:length>
</f:table>
```

现在，命名冲突不存在了，这是由于两个文档都使用了不同的名称来命名它们的 < table > 元素 (< h:table > 和 < f:table >)。

### 使用命名空间

```xml
<h:table xmlns:h="http://www.w3.org/TR/html4/">
   <h:tr>
   <h:td>Apples</h:td>
   <h:td>Bananas</h:td>
   </h:tr>
</h:table>
```

与仅仅使用前缀不同，我们为 < table > 标签添加了一个 xmlns 属性，这样就为前缀赋予了一个与某个命名空间相关联的限定名称。

### XML Namespace (xmlns) 属性

XML 命名空间属性被放置于元素的开始标签之中，并使用以下的语法：

```xml
xmlns:namespace-prefix="namespaceURI"
```

**注释：**用于标示命名空间的地址不会被解析器用于查找信息。其惟一的作用是赋予命名空间一个惟一的名称。不过，很多公司常常会作为指针来使用命名空间指向实际存在的网页，这个网页包含关于命名空间的信息。



## XML验证

> 有正确的XML被称为"形式良好"的XML

### DTD约束

> DTD（文档类型定义）的作用是定义 XML 文档的合法构建模块。

DTD可声明在XML文档中, 也可通过外部引入

#### 内部的 DOCTYPE 声明

假如 DTD 被包含在您的 XML 源文件中，它应当通过下面的语法包装在一个 DOCTYPE 声明中：

```xml
<!DOCTYPE root-element [element-declarations]>
```

带有 DTD 的 XML 文档实例（请在 IE5 以及更高的版本打开，并选择查看源代码）：

```xml-dtd
<?xml version="1.0"?>
<!DOCTYPE note [
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
]>
<note>
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>"Don't forget me this weekend"</body>
</note>
```

解释如下

1. !DOCTYPE note (第二行): 定义此文档是note类型的文档
2. !ELEMENT note: 定义note元素有四个元素: "to, from, heading, body”
3. !ELEMENT to: 定义to元素为"#PCDATA"类型
4. !ELEMENT from: 定义from 元素为"#PCDATA"类型
5. !ELEMENT heading素为"#PCDATA"类型
6. !ELEMENT body: 定义from 元素为"#PCDATA"类型

#### 外部文档声明

假如 DTD 位于 XML 源文件的外部，那么它应通过下面的语法被封装在一个 DOCTYPE 定义中：

```xml
<!DOCTYPE root-element SYSTEM "filename">
```

这个 XML 文档和上面的 XML 文档相同，但是拥有一个外部的 DTD: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
	<to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Dot't forget me this weekend!</body>
</note>
```

这是包含DTD的note.dtd文件

```dtd
<!ELEMENT note (to, from, heading, body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
```

#### PCDATA数据格式

PCDATA 的意思是被解析的字符数据( parsed character data). 

可把字符数据想象为XML元素的开始标签与结束标签之间的文本 PCDATA 是会被解析器解析的文本, 这些文本将被解析器检查实体以及标记.  文本中的标签会被当做标记来处理, 而实体会被展开. 不过, 被解析的字符数据不应该包含任何&, < 或 >等字符. 需要用实体来表示.

#### CDATA数据格式

CDATA 的意思是字符数据（character data）。

**CDATA 是不会被解析器解析的文本。**在这些文本中的标签不会被当作标记来对待，其中的实体也不会被展开。

#### 元素格式

1. 空元素  <!ELEMENT element-name EMPTY>
2. 可以为任何内容 <!ELEMENT element-name ANY>
3. 最少出现一次的元素 <!ELEMENT element-name (child-name+)>
4. 出现零次或多次的元素 <!ELEMENT element-name (child-name*)>
5. 出现零次或一次的元素<!ELEMENT element-name (child-name?)>
6. 声明"非.../即..."类型的内容<!ELEMENT element-name (child-name1 | child-name2 | …)>
7. 声明混合型的内容<!ELEMENT element-name (#PCDATA | to | from | heading)*>

#### 属性约束格式

> 属性通过ATTLIST 声明来进行声明

DTD属性声明用如下格式

<!ATTLIST element-name attribute-name attribute-type attribute-value>

```xml-dtd
<!ATTLIST payment type CDATA "check">
<!--实例如下-->
<payment type="check"/>
```

属性类型: 

| 类型               | 描述                          |
| :----------------- | :---------------------------- |
| CDATA              | 值为字符数据 (character data) |
| (*en1*\|*en2*\|..) | 此值是枚举列表中的一个值      |
| ID                 | 值为唯一的 id                 |
| IDREF              | 值为另外一个元素的 id         |
| IDREFS             | 值为其他 id 的列表            |
| NMTOKEN            | 值为合法的 XML 名称           |
| NMTOKENS           | 值为合法的 XML 名称的列表     |
| ENTITY             | 值是一个实体                  |
| ENTITIES           | 值是一个实体列表              |
| NOTATION           | 此值是符号的名称              |
| xml:               | 值是一个预定义的 XML 值       |

默认属性值可以使用下列值

| 值           | 解释           |
| :----------- | :------------- |
| 值           | 属性的默认值   |
| #REQUIRED    | 属性值是必需的 |
| #IMPLIED     | 属性不是必需的 |
| #FIXED value | 属性值是固定的 |

列举属性值

<!ATTLIST element-name attribute-name (en1|en2|..) default-value>

### XML Schema约束

#### 简介

- XML Schema是基于XML的DTD代替者
- XML Schema是可描述XML文档的结构
- XML Schem语言也可作为XSD(XML Schema Definition) 来引用

#### 什么是XML Schema

> XML Schema 的作用是定义XML文档的合法构建模块, 类似DTD

XML Schema: 

- 定义可出现在文档中的元素
- 定义可出现在文档中的属性
- 定义哪个元素是子元素
- 定义子元素的顺序
- 定义子元素的数目
- 定义元素是否为空, 或者是否可包含文本
- 定义元素和属性的数据类型
- 定义元素和属性的默认值以及固定值

#### XML Schema 是 DTD的继任者

- XML Schema可针对未来的需求进行扩展
- XML Schema 更完善, 功能更强大
- XML Schema 基于XML编写
- XML Schema支持数据类型
- XML Schema支持命名空间

#### XML Schema使用XML语法编写, 本身就是XML

- 不必学习新的语言
- 可使用XML编辑器来编写Schema文件
- 可使用XML解析器来解析Schema文件
- 可使用XML DOM来处理Schema文件
- 可通过XSLT来转换Schema文件

#### 如何使用

一个简单的XML文档

```xml
<?xml version="1.0"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

下面这个例子是一个名为"note.xsd”的XML Schema文件, 它定义了上面哪个note.xml文档的元素

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
targetNamespace="http://www.w3schools.com"
xmlns="http://www.w3schools.com"
elementFormDefault="qualified">

<xs:element name="note">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="to" type="xs:string"/>
            <xs:element name="from" type="xs:string"/>
            <xs:element name="heading" type="xs:string"/>
            <xs:element name="body" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

将此note.xml文档包含xml Schema的约束, 使用如下

```xml
<?xml version="1.0" encoding="utf-8"?>
<note xmlns="http://www.w3schools.com"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.w3schools.com note.xsd">
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>
```

#### XSD-元素

> schema元素是每一个xml Schema的根元素

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema>
...
...
</xs:schema>
```

<schema> 元素一般都包含属性. 一个schema声明往往是这样的

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
	targetNamespace="http://www.runoob.com"
	xmlns="http://www.runoob.com"
	elementFormDefault="qualified">
>
...
...
</xs:schema>
```

解释如下: 

1. xmlns:xs="http://www.w3.org/2001/XMLSchema"

   	此片段显示schema中用到的元素和数据类型来自命名空间"http://www.w3.org/2001/XMLSchema”中. 同时它还规定了来自命名空间"http://....…." 的元素和数据类型应该使用前缀xs:, 相当于命名空间的别名


   2. targetNamespace="http://www.runoob.com”

        此片段显示被此schema定义的元素(note to from heading body) 来自命名空间:"http:www.runoob.com”.

   3. elementFormDefault=“qualified”

      指出任何xml实例所使用的且在此schema中声明过的元素必须被命名空间限定

在XML文档中引用Schema文档是这样的

```xml
<?xml version="1.0" encoding="utf-8"?>
<note xmlns="http://www.runoob.com"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.runoob.com note.xsd">

<to>Tove</to>
<form>Jani</form>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
```

解释如下: 

1. xmlns=“http://www.runoob.com”

   规定了默认命名空间的声明, 此声明会告知schema验证器, 在此XML文档中使用的所有元素都被声明于"http://www.runoob.com”这个命名空间

2. xmlns:xsi=“http://www.w3.org/2001/XMLSchema-instance”

   一旦声明了此命名空间, 就可以使用schemaLocation属性了, 此属性有两个值, 第一个值是需要的命名空间, 第二个值是供命名空间使用的xml schema的位置

#### XSD-简易属性

> 简易属性指那些只包含文本的元素. 它不会包含任何其他的元素或属性.

定义简易元素:

<xs:element name=“xxx” type=“yyy” />

xxx: 指元素的名称, yyy指元素的数据类型, XML Schema拥有很多内建的数据类型

- xs:string
- xs:decimal
- xs:integer
- xs:boolean
- xs:date
- xs:time

例如: 

```xml
<lastname>Refsnes</lastname>
<age>36</age>
<databorn>1970-03-27</databorn>
```

xsd定义如下

```xml
<xs:element name="lastname" type="xs:string"/>
<xs:element name="age" type="xs:integer"/>
<xs:element name="dateborn" type="xs:date"/>
```

简易元素的默认值和固定值

缺省值定义

```xml
<xs:element name="color" type="xs:string" default="red"/>
```

固定值是不能被更改的

```xml
<xs:element name="color" type="xs:string" fixed="red"/>
```

#### xsd-对值的限定

> 限定(restriction) 用于为xml元素或者属性定义可接受的值, 对xml元素的限定被称为 facet

限定age元素, 0<age<120

```xml
<xs:element name="age">
	<xs:sipleType>
    	<xs:restriction base="xs=integer">
        	<xs:minInclusive value="0"/>
            <xs:maxInclusive value="120"/>
        </xs:restriction>
    </xs:sipleType>
</xs:element>
```

对一组值的限定

如果把XML元素的内容限制为一组可接受的值, 我们使用枚举约束. 

下面例子定义了一个带有限定的名为"Car”的元素, 可接受的值只有"Audi  Golf  BWM”

```xml
<xs:element name="car">
	<xs:simplyType>
    	<xs:restriction base="xs:string">
        	<xs:enumeration value="Audi"/>
            <xs:enumeration value="Golf"/>
            <xs:enumeration value="BWM"/>
        </xs:restriction>
    </xs:simplyType>
</xs:element>
```

也可以写成这样

```xml
<xs:element name="car" type="carType"/>
<xs:simpleType name="carType">
	<xs:restriction base="xs:string">
    	<xs:enumeration value="Audi"/>
        <xs:enumeration value="Gold"/>
        <xs:enumeration value="BWM" />
    </xs:restriction>
</xs:simpleType>
```

这种写法中, 类型"carType"也可以被其他元素使用, 因为它不是car元素的组成部分

对一系列值得限定

我们可以使用模式约束对xml元素得内容进行一系列得限定

限定"letter"元素, 可接受得值只有小写字母a-z其中得一个

```xml
<xs:element name="letter">
	<xs:simpleType>
    	<xs:restriction base="xs:string">
        	<xs:pattern value="[a-z]"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

pattern中可填放正则表达式

对空白字符得限定

下面例子, 定义了一个带有限定得名为"address”得元素. 这个whiteSpace 限定设置为"preserve", 这意味着XML处理器不会移除任何一个空白字符

```xml
<xs:element name="address">
	<xs:simpyType>
    	<xs:restriction base="xs:string">
        	<xs:whiteSpace value="preserve"/>
        </xs:restriction>
    </xs:simpyType>
</xs:element>
```

限定"address"元素移除所有空格: 

```xml
<xs:element name="address">
	<xs:simpleType>
    	<xs:restriction  base="xs:string">
        	<xs:whiteSpace value="replace"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

限定元素"address"将所有得空格只保留一个空格, 类似HTML

```xml
<xs:element name="address">
	<xs:simpleType>
    	<xs:restriction base="xs:string">
        	<xs:whiteSpace value="cellaase"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

对长度得限定:

使用length, maxLength, minLength来限定一个元素中值得长度

限定名为"password"得元素, 长度必须精确到8位

```xml
<xs:element name="password">
	<xs:simpeType>
    	<xs:restriction base="xs:string">
        	<xs:length value="8"/>
        </xs:restriction>
    </xs:simpeType>
</xs:element>
```

#### 数据类型得限定表格

| 限定           | 描述                                                      |
| :------------- | :-------------------------------------------------------- |
| enumeration    | 定义可接受值的一个列表                                    |
| fractionDigits | 定义所允许的最大的小数位数。必须大于等于0。               |
| length         | 定义所允许的字符或者列表项目的精确数目。必须大于或等于0。 |
| maxExclusive   | 定义数值的上限。所允许的值必须小于此值。                  |
| maxInclusive   | 定义数值的上限。所允许的值必须小于或等于此值。            |
| maxLength      | 定义所允许的字符或者列表项目的最大数目。必须大于或等于0。 |
| minExclusive   | 定义数值的下限。所允许的值必需大于此值。                  |
| minInclusive   | 定义数值的下限。所允许的值必需大于或等于此值。            |
| minLength      | 定义所允许的字符或者列表项目的最小数目。必须大于或等于0。 |
| pattern        | 定义可接受的字符的精确序列。                              |
| totalDigits    | 定义所允许的阿拉伯数字的精确位数。必须大于0。             |
| whiteSpace     | 定义空白字符（换行、回车、空格以及制表符）的处理方式。    |

#### 复合元素

> 复合元素指包含其他元素及/或属性得XML元素

四种类型得复合元素

- 空元素
- 包含其他元素得元素
- 仅包含文本得元素
- 包含元素和文本得元素

如何定义复合元素

```xml
<employee>
	<firstname>John</firstname>
    <lastname>Smith</lastname>
</employee>
```

现在我们用xsd来定义此元素

sequence 意味着此元素必须包含指定得顺序

```xml
<xs:element name="employee">
	<xs:complexTyep>
    	<xs:sequence>
        	<xs:element name="firstname" type="xs:string"/>
            <xs:element name="lastname" type="xs:string" />
        </xs:sequence>
    </xs:complexTyep>
</xs:element>
```

以上也可以这样写

```xml
<xs:element name="employee" type="personinfo"/>

<xs:complexType name="personinfo">
	<xs:sequence>
    	<xs:element name="firstname" type="xs:string"/>
        <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
</xs:complexType>
```

## XML解析

### DOM解析

> JAVA提供的默认解析XML文档的方式, 实现了w3c中的标准, 应用最广泛.

dom是个功能强大的解析工具，适用于小文档. 它会把整篇xml文档装载进内存中，形成一颗文档对象树

简单使用

```java
/**
 * DOM解析测试类
 *
 * @author jssd
 * Create 2019-07-10 18:31
 */
public class Test {
	public static void main(String[] args) {
		// 1. 取得文档建立工厂
		DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
		// 2. 获取文档建立对象
		DocumentBuilder documentBuilder;
		try {
			documentBuilder = documentBuilderFactory.newDocumentBuilder();
			// 3. 通过文档建立对象取得文档对象
			Document document = documentBuilder.parse("books.xml");
			// 4. 通过元素名, 取得元素对象
			NodeList noteList = document.getElementsByTagName("books");
			// 查看取得元素对象个数
			System.out.println("根元素个数" + noteList.getLength());
			System.out.println("====================查看元素属性======================");
			for (int i = 0; i < noteList.getLength(); i++) {
				// 取得单个对象
				Node item = noteList.item(i);
				// 取得所有属性
				NamedNodeMap attributes = item.getAttributes();
				// 循环遍历所有属性, 并输出
				System.out.println(item.getNodeName() + "节点的所有属性如下: ");
				for (int j = 0; j < attributes.getLength(); j++) {
					Node node = attributes.item(j);
					System.out.println(node.getNodeName() + ": " + node.getNodeValue());
				}
				System.out.println("=============查看元素内节点或文本内容============");
				// 取得所有子元素节点
				NodeList childNodes = item.getChildNodes();
				System.out.println("childNodes.getLength() = " + childNodes.getLength());
				/*
				DOM解析的过程中, 可以看到字节点中有空的文本节点, 我们查看的时候需要过滤出来
				 */
				for (int j = 0; j < childNodes.getLength(); j++) {
					Node node = childNodes.item(j);
					if (node.getNodeType() == Node.ELEMENT_NODE) {
						// 查看节点名和节点值
						System.out.println("childNodes.item(j) = " + node.getNodeName() + ":" + node.getNodeValue());
					}
				}
			}
			
		} catch (ParserConfigurationException | SAXException | IOException e) {
			e.printStackTrace();
		}
	}
}
```

### SAX解析

> 和DOM一样, 同样是JAVA原生组件, 全称是simple API for XML

SAX 是一种以事件驱动的XML API, SAX去DOM不同的是它边扫描边解析, 自顶向下依次解析, 由于边扫描边解析, 所以它解析XML具有速度快, 占用内存少的优点



简单使用: 

```java
/**
 * @author jssd
 * Create 2019-07-10 19:14
 */
public class Test {
   public static void main(String[] args) {
      // 1. 取得SAX解析器工厂
      SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
      // 2. 从sax解析器工厂中取得解析器
      SAXParser saxParser;
      try {
         saxParser = saxParserFactory.newSAXParser();
         // 3. 重写DefaultHandler类, 使用DefaultHandler类中的方法, 解析XML
         BookHandler bookHandler = new BookHandler();
         // 4. 开始解析
         saxParser.parse(new File("books.xml"), bookHandler);
      } catch (ParserConfigurationException | SAXException | IOException e) {
         e.printStackTrace();
      }
   }
}
```

使用SAX的时候, 需要有个类继承DefaultHandler

```java
/**
 * @author jssd
 * Create 2019-07-10 19:19
 */
public class BookHandler extends DefaultHandler {
   // 文档开始解析的时候调用
   @Override
   public void startDocument() throws SAXException {
      super.startDocument();
      System.out.println("文档开始解析");
   }

   // 文档解析结束的时候调用
   @Override
   public void endDocument() throws SAXException {
      super.endDocument();
      System.out.println("文档解析结束");
   }

   // 元素开始解析的时候调用
   @Override
   public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
      super.startElement(uri, localName, qName, attributes);
      System.out.println("元素属性: ");
      if (attributes.getLength() != 0) {
         for (int i = 0; i < attributes.getLength(); i++) {
            System.out.println("attributes.getQName() + attributes.getValue() = " +
                  attributes.getQName(i) + ": " + attributes.getValue(i));
         }
      }
      System.out.println("元素名: " + qName);
   }

   // 元素解析结束的时候调用
   @Override
   public void endElement(String uri, String localName, String qName) throws SAXException {
      super.endElement(uri, localName, qName);
   }

   // 解析XML中的文本时候调用
   @Override
   public void characters(char[] ch, int start, int length) throws SAXException {
      super.characters(ch, start, length);
      String str = new String(ch, start, length);
      if(!"".equals(str.trim())) { // 过滤空的文本节点
         System.out.println("文本内容为: " + str);
      }
   }
}
```

### JDOM解析

> JDOM是第三方插件, 不属于JAVA原生解析方式, 所以需要导入jar包
>
> 它基于树型结构，利用纯 Java 的技术对 XML 文档实现解析。所以只适合于 Java
> 语言 

简单步骤: 

1. 创建一个SAXBuilder 对象
2. 调用SAXBuilder对象的build方法, 得到Document对象(通过IO流)
3. 获取根节点
4. 获取根节点的直接子节点的集合
5. 遍历集合

简单使用:
    注意: 使用的时候导包不要导错了

```java
/**
 * JDom 学习测试类
 * @author jssd
 * Create 2019-07-11 14:01
 */
public class Test {
	public static void main(String[] args) {
		// 取得SAXBuilder对象
		SAXBuilder saxBuilder = new SAXBuilder();
		try {
			// 取得整个文档
			Document document = saxBuilder.build("books.xml");
			// 从文档中取得根节点
			Element rootElement = document.getRootElement();
			// 从根节点中取出子节点
			List children = rootElement.getChildren();
			for (Object child : children) {
				Element element = (Element) child;
				// 查看子节点的属性
				List attributes = element.getAttributes();
				for (Object attribute : attributes) {
					Attribute att = (Attribute) attribute;
					System.out.println("att.getName = " + att.getName() + ", att.getValue = " + att.getValue());
				}
			}

			// 查看子节点的内容
			for (Object child : children) {
				Element element = (Element) child;
				List subChild = element.getChildren();
				for (Object o : subChild) {
					Element subEle = (Element)o;
					System.out.println("subEle.getName() = " + subEle.getName());
					System.out.println("subEle.getValue() = " + subEle.getValue());
				}
			}

		} catch (JDOMException | IOException e) {
			e.printStackTrace();
		}
	}
}
```

### dom4j解析

> DOM4J 是一个 Java 的 XML API， 是 JDOM 的升级品，用来读写 XML 文件的 

解析步骤: 

1. 创建SAXReader对象
2. 调用read方法
3. 获取根元素
4. 通过迭代器遍历直接节点

简单使用: 

```java
/**
 * @author jssd
 * Create 2019-07-11 14:31
 */
public class Test {
	public static void main(String[] args) {
		SAXReader saxReader = new SAXReader();
		try {
			// 取得文档
			Document document = saxReader.read("books.xml");
			// 取得根元素
			Element rootElement = document.getRootElement();
			// 取得根元素之下的子元素
			for (Iterator iter = rootElement.elementIterator(); iter.hasNext(); ) {
				Element bookEle = (Element) iter.next();
				// 获取子元素的属性
				for (Iterator iterator = bookEle.attributeIterator(); iterator.hasNext(); ) {
					Attribute bookAttr = (Attribute) iterator.next();
					System.out.println("属性的名字: " + bookAttr.getName() + ", 属性的值: " + bookAttr.getValue());
				}

				for (Iterator iterator = bookEle.elementIterator(); iterator.hasNext(); ) {
					Element element = (Element) iterator.next();
					System.out.println("子节点的值: " + element.getName() + ", 子节点的值: " + element.getText());
				}
			}
		} catch (DocumentException e) {
			e.printStackTrace();
		}
	}
}
```

### 四种解析方式比较

1. DOM解析

   形成了树的结构, 有助于更好的理解, 掌握, 且代码容易编写. 解析过程中, 树结构保存在内存中, 方便修改

2. SAX解析

   采用事件驱动模式, 对内存的耗费比较小. 适用于只处理XML文件中的数据时

3. JDOM解析

   仅使用具体类, 而不使用接口, API大量使用了Collections类

4. DOM4J解析

   JDOM的一种只能分支, 它合并了许多超出基本XML文档表示的功能.  它使用接口和抽象基本类方法. 具有性能优异, 灵活性好, 功能强大和极端易用的特点.  开源.

### DOM4J配合XPATH快速选择一个元素

我们使用DOM4J解析XML的时候, 有的时候元素层数比较多, 难道要一个一个元素的取得进去吗? 显然, XML层数多了之后, 就不现实了. 所以我们有了XPATH技术.

> XPath即为XML路径语言，它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言。XPath基于XML的树状结构，提供在数据结构树中找寻节点的能力。起初 XPath 的提出的初衷是将其作为一个通用的、介于XPointer与XSLT间的语法模型。但是 XPath 很快的被开发者采用来当作小型查询语言。
> 

小知识: 使用chrome浏览器打开一个网页的时候, 按f12 查看源代码, 右键网页中html元素, 可以copyXpath

简单使用: 

假如我有一个XML如下

```xml
<?xml version="1.0" encoding="utf-8" ?>
<books xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="books.xsd">
    <book id="1111">
        <name>java 实战开发</name>
        <author>jssd</author>
        <price>1000</price>
    </book>
    <book id="1001">
        <name>java开发实战</name>
        <author>张小三</author>
        <price>98.5</price>
    </book>
</books>
```

查找指定元素: 

```java
public class Test {
	public static void main(String[] args) {
		// 获取DOM4J中的SAXReader对象
		SAXReader saxReader = new SAXReader();
		Document read;
		try {
			// 读取整个文档
			read = saxReader.read("books.xml");
			// 我想要选中id=1001中book的author节点
			Node node = read.selectSingleNode("//book[@id=1001]/author");
			System.out.println("node.getName() + \", \" + node.getText() = " + node.getName() + ", " + node.getText());
		} catch (DocumentException e) {
			e.printStackTrace();
		}
	}
}
```

XPath使用非常简单, 就是配合DOM4J进行选择节点, 关于XPath语法, 本文不做介绍, 请查阅相关API
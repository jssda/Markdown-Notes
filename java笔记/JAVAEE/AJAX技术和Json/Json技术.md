# Json技术

## 什么是Json

> JSON: JavaScript Object Notation(JavaScript 对象表示法)

- JSON 比 XML 更小、更快，更易解析。
- JSON 是轻量级的文本数据交换格式
- JSON 独立于语言 
- JSON 具有自我描述性，更易理解
- JSON 是存储和交换文本信息的语法，类似 XML。

## Json简介

- JSON 是 JavaScript Object Notation 的缩写。
- 这个格式由 Douglas Crockford 提出。
- 被设计用于可读的数据交换。
- 它是从 JavaScript 脚本语言中演变而来。
- 文件名扩展是 **.json**。
- JSON 的网络媒体类型是 **application/json**。
- 统一标示符类型（Uniform Type Identifier）是 public.json。

## 简单示例

1. 所有的json格式的js对象都需要使用大括号括起来, 代表他是一个json格式的js对象
2. 键值对之间只能使用冒号, 不能使用等于号
3. 使用方括号保存数组，数组值使用 ,（逗号）分割。
4. JSON的值
   - 数字（整数或浮点数）
   - 字符串（在双引号中）
   - 逻辑值（true 或 false）
   - 数组（在方括号中）
   - 对象（在花括号中）
   - null

```json
{
    "book": [
        {
            "id":"01",
            "language": "Java",
            "edition": "third",
            "author": "Herbert Schildt"
        },
        {
            "id":"07",
            "language": "C++",
            "edition": "second",
            "author": "E.Balagurusamy"
    }]
}
```

## JSON字符串转换成js对象

```js
var jsonStr = "{uid:18,uname:'王五', age:18,fav:'看电影'}";

// 建议使用这种方式
eval("var obj2 = " + jsonStr);

// 如果是一个标准的jsonStr, 可以使用这种方式
var obj3 = eval("(" + jsonStr + ")");

// 也可以使用JSON对象
var obj4 = JSON.parse(jsonStr);

// JSON对象还可以将一个js对象转换成一个JSON的字符串
var jsonStr = JSON.stringify(obj1);
```


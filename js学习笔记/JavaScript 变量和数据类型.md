## Js的引入方式

### 内部引入

```html
<script type="text/javascript">
    /*网页中的弹框*/
    alert("js的学习课程")
</script>
```

### 外部引入

```html
<!--type为引入的文件类型  src为引入的文件url   charset为引入文件的编码-->
<!--注意: 内部引入和外部引入不能二合一, 也就是说不能把js代码, 写入到引入代码中-->
<script type="text/javascript" src="js/test.js" charset="utf-8"></script>
```

## Js中的变量

### 变量的声明

```js
var variableName = value
```

### 声明变量注意的事项

1. js中的变量的名称和java中标识符的命名保持一致就可
以了。 
2. js中变量名称是可以重复的，但是后者的名称会把前者
  的名称值覆盖 
3. js中末尾即使没有分号结束也是可以的，但是不推荐大
  家这样书写。 

## Js中的数据类型

js中的数据类型是弱类型. 所有的变量都使用var来定义, 但是, 定义的数据是有类型的. 可以使用typeof关键字类查看一个变量的类型. typeof关键字就像是java中instanceof关键字

### 基本数据类型

- number: 数字类型, 类似java中的int+double+float类型
- string: 字符串类型. js中没有单引号和双引号的区别, 所以没有char类型, 只有string类型
- boolean: 布尔数据类型
- object: 对象数据类型 比如 var data = new Data();

### 特殊数据类型

- undefined: 未定义类型. 常见于声明变量但是没有符值的变量
- NaN: 不是一个数字类型  not a number. 常见于将字符串强转为number类型的时候, 相当于报一个数字转换异常
- null: 空对象类型. 属于object类型, 但是是一个空的对象




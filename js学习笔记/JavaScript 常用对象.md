# Js中的常用对象

## Dater日期对象

```js
// 新建对象
var date = new Date();

// 本星期的第几天
date.getDay();

// 本月的第几天
date.getDate();

// 返回月份 0-11
date.getMonth();

// 返回1900 年到现在的差值
date.getYear();

// 返回现在是哪一年
date.getFullYear();

// 返回本地时间
date.toLocaleString();
```

## 数学函数对象

```js
// 获取随机数 随机范围是[0-1]
var ran = Math.random() * 1000;

// 向上取整
Math.ceil()

// 向下取整
Math.floor()

// 获取4位数的验证码
Math.random() * 9000 + 1000;
```

## Sting对象

```js
// 获取下标为2的内容
str.charAt(2);

// 获取下标
str.indexOf("a");

// 字符串截取
str.substr(2,4);
str.substr(1);
str.substring(2,4);

// 按照指定字符分割
str.split("-");
```

## 全局对象

Global对象可以直接使用

```js
// 把字符串当作js代码进行执行
var b = "var c = 1 + 1";
eval(b);

// 检查某个值是否是数字
isNaN("123a");
```

## 数组对象

### 数组得声明方式

方式一:

```js
var arr1 = new Array();
```

方式二:

```js
var arr2 = new Array(2); // 2是数组得长度. 这个时候输出数组, 会显示数组两个元素为empty
```

方式三:

```js
var arr3= new Array(1, "你好", new String(), true);
```

方式四

```js
// 这个用法和java相同
var arr4 = ["jssd", 123, new Date(), false];
```

### 数组得使用

js中数组的下标可以不连续, 如果没有值就是empty

```js
function demo() {
    var arr=[];
    arr[0] = "李时珍的皮"; // 自动扩容至length = 1;
    arr[1] = true; // 自动扩容至length=2
    arr[6] = 12; // 自动扩容至length=7, 前面没有赋值的数组为empty
}
```

### 数组的扩容

```js
function demo() {
    var arr=["jssd", 123, new Date(), false];
    // 扩大数组, 未赋值的为empty
    arr.length = 10;
    // 缩小数组 超出的部分会被丢弃
    arr.length = 2;
}
```

### 数组的遍历

方式一

```js
function demo() {
    var arr =  ["jssd", 123, new Date(), "测试一下"];
    for (var i = 0; i < arr1.length; i++) {
        alert(arr1[i]);
    }
}
```

方式二

```js
 var arr =  ["jssd", 123, new Date(), "测试一下"];
    for (let x in arr1) { // x是数组的下标
        console.log(arr[x]);
    }
```

方式三

```js
var arr = ["jssd", 123, new Date(), "测试一下"]
for (let x of arr1) {
    console.log(x);
}
```

### 数组中的常用方法

```js
// 向数组的末尾添加一个或更多的元素, 并返回心得长度
arr.push("我们");

// 删除并返回数组的最后一个元素
arr.pop();

//向数组的开头添加一个或更多的元素, 并返回新的长度
arr.unshift("你好");

// 一出第一个元素
arr.shift();

// splice函数
arr.splice(1, 2); // 从下标1开始删除, 删除2个元素
arr.splice(1, 0, "你", "好"); // 从下标1开始, 删除0个元素, 添加两个元素. 实现了再指定位置添加元素的功能
```


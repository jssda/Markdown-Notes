# DOM获得操作对象的属性

W3C教程链接: [https://www.w3school.com.cn/jsref/dom_obj_all.asp](https://www.w3school.com.cn/jsref/dom_obj_all.asp)

## 直接操作元素的属性

###  获得属性的值

```js
var inp1 = docuement.getElementById("inp1");

var type = inp1.type;

// 获得的值, 是实时更改的值, 不是元素的默认值
var value = inp1.value;
```

### 操作属性的值

```js
var inp1 = document.getElementById("inp1");
inp1.value = "更改之后的值";
```

## 通过元素对象获取元素的值

### 获取属性的值

```js
var inp1 = document.getElementById("inp1");

//获取的是元素的默认值,不是元素的实时动态值
let type = inp1.getAttribute("type");
let value = inp1.getAttribute("value");
```

### 操作属性的值

```js
var inp1 = document.getElementById("inp1");

inp1.setAttribute("type", "button");
```

# DOM操作css样式

## 获得css样式

```js
let div = document.getElementById("div1");

let w1 = div.style.width;
let hi = div.style.height;
```

## 操作css样式

操作css样式的时候, 只能更改内联的style

```js
// 操作属性的时候, 需要用到驼峰格式
div.style.width = "200px";
div.style.backgroundColor="red";
```

### 通过操作class类操作元素样式

```js
// 通过增加操作类操作元素样式
// div2类是写好的, 如果没有写好, 需要创建一个link元素, 添加引入
div.className="div2";
```


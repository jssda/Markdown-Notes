# JQuery操作页面的文本对象和值

## JQuery操作文本对象

```js
var div = $("#div1");

// 获得指定元素节点的文本内容, 类似于DOM中的innerHTML
var ht = div.html();

// 只是获得文本内容, 不含有HTML标签
var te = div.text();
```

## 获得表单中的值

```js
var var1 = $("#inp1").val();

// 操作表单中的值
$("#inp1").val("新的值");
```

## 特殊情况

select, textarea两个标签获得其中的值的时候, 不是使用文本对象获得其值, 而是使用val()方法获得其中的值.
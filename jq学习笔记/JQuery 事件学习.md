# JQuery的事件学习

## 页面加载事件

jq中的页面加载操作要比原生js中window.onload加载要快. 同时, jq中的页面加载方法可以写好几个

```js
// 第一种写法
$(function() {})

// 第二种写法
jquery(function(){})

// 第三种写法
$(document).ready(function() {})
```

## 使用bind绑定事件

```js
$("#bu2").bind('click', function() {})

// bind事件绑定支持json数据格式绑定
$("#bu2").bin({
    'click':function() {},
    'dbclick':function() {}
})
```

## 一次事件绑定

一次事件绑定就是只执行一次的事件, 第二次执行的时候会失效

```js
$("#bu2").one("click", function() {})
```

## 事件的解绑

```js
// 解绑所有事件
$("#bu2").unbind();

// 解绑指定事件
$("#bu2").unbind("click");
```

## trigger事件

trigger就是触发. 使用此函数会触发一个事件

```js
// 此操作触发了bu1的click事件, 相当于点击了bu1
$("#bu1").trigger("click");
```

## live 绑定事件

live和bind使用方法一样, 但是, 只在1.7和其之前支持. 

live能够使新添加的节点也能动态的绑定上方法. 

## on绑定事件

在jq 1.7之后, 不建议使用live方法, 在1.9之后, 去除了live绑定

on事件绑定代替live绑定

```js
// jQuery1.9之前的版本我们可以这样写：

$("a").live("focus",function(){
  this.blur();
}); 
//jQuery1.9之后由于live被删除了，所以应该这样写：
// 此时是将focus事件绑定到document中出现的a标签以及未来出现的a标签之上
$(document).on("focus","a",function(){
  this.blur();
}); 　
```

## 其他事件

给input绑定一个单击事件

```js
$("#input1").onclick(function() {})

// 其他所有的js事件和onclick使用相同
```


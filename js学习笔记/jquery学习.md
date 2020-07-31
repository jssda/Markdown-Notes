# jquery选择器

> 学习方式, 照着列表一个个敲一遍, 记个大概, 知道有这么个选择器. 之后使用的时候查查w3c, 就会用了

## 元素选择器

```js
$("p"); // 取得p标签
$("#p1"); // 取得id为p1的元素
$(".p1"); //取得class=p1的元素
```

## 属性选择器

```js
$("[href]"); //选择属性带有href的元素
$("[href]='#'"); //选择href=#的元素
$("[href]!='#'"); //选择href!=#的元素
$("[href]$='.jpg'"); //选择所有href属性以.jpg结尾的元素
```

## css选择器

```js
$("#p1").css("background-color", "red"); //将背景色设置为红色
```

## 其他选择器

```js
$(".class1.class2"); //选择 class=class1 且 class=class2 的元素

$("p:first"); //第一个p元素
$("p:last"); //最后一个p元素
$("p:even"); //偶数p元素 从0开始
$("p:odd"); //奇数p元素 从0开始

$("p:empty"); //无子元素节点或者无文本节点的所有元素
$("p:not(:empty)");//所有不为空的元素

$("p:eq(4)"); //下标为4的p元素, 从0开始
$("p:gt(2)"); //下标大于2的p元素, 从0开始
$("p:lt(3)"); //下标小于3的p元素, 从0开始

$(":header"); // 所有标题元素, <h1>-<h6>
$("p:contains(text)"); //包含指定字符串的元素
$("p:hidden"); //隐藏的p元素
$("p:visible"); //可见的p元素

$("#p1, #p2, #p3"); // 分组选择, 选取id为p1, p2, p3的元素, 也可为标签, class等

$(":input"); //所有input元素
$(":text"); //所有input[type=text]的元素 text可更换为password,radio,checkbox,submit,reset,image...等
$(":selected"); //用于下拉列表被选中
$(":checked"); //用于单选框或多选框被选中
```

# jquery事件函数

> 事件处理程序指的是当 HTML 中发生某些事件时所调用的方法。术语由事件“触发”（或“激发”）经常会被使用。

## 文档加载函数

```js
//相当于在body中使用onload=
//在页面完全加载后才会调用
$(document).ready(function() {
   //函数内容 
});
```

## 点击事件函数

```js
$("button").click(function() {..some code... } ); //在buttion中添加onclick事件
```

## 隐藏元素函数

```js
$("#p1").hide(); //将id=p1的元素隐藏
```

## 失去焦点函数

```js
$("selector").blur(function() {...some code...}); //添加onblur事件
```

## 数据改变触发函数

```js
$("selector").change();
```

## 鼠标双击事件函数

```js
$("selector").dblclick();
```

## 其他简单函数

> 类似以上简单事件函数还有很多, 例如

```js
focus(); //焦点函数
keydown(); //键盘按键按下事件
keypress(); //键盘按键按下再弹起事件
keyup(); //键盘按键弹起事件
mousedown(); //鼠标按下事件
mouseenter(); //鼠标进入元素事件, 当鼠标进入被选元素的子元素时候, 不会起作用
mouseleave(); //鼠标离开元素事件, 当鼠标离开被选元素的子元素时候, 不会起作用
mousemove(); //鼠标在元素中移动事件
mouseover(); //鼠标进入元素事件, 当鼠标进入被选元素的子元素时候, 也会起作用
mouseout(); //鼠标离开元素事件, 当鼠标离开被选元素的子元素时候, 也会起作用
mouseup(); //鼠标按下之后弹起事件
resize(); //当窗口调整大小发生时候, 触发resize事件
scroll(); //当用户滚动指定的元素时，会发生 scroll 事件
select(); //当 textarea 或文本类型的 input 元素中的文本被选择时，会发生 select 事件
submit(); //提交表单时候, 会触发submit事件
$("selector").error(); //当元素遇到错误（没有正确载入）时，发生 error 事件
load(); //指定的元素（及子元素）已加载时，会发生 load() 事件。
unload(); //用户离开页面时候, 会发生unload方法
```

## 元素绑定事件方法

```js
$("selector").bind(functionName, function);// 为指定元素绑定指定事件方法, 动态添加的元素没有效果
$("selector").live(childSelector, functionName, function); // 为指定元素绑定指定事件方法, 动态添加的元素也有效果
$("selector").one(functionName, function); //为指定元素绑定事件, 但是事件只能使用一次
$("selector").unbind(); //移除通过bind添加的方法
$("selector").die(); //此方法移除所有通过 live() 方法向指定元素添加的一个或多个事件处理程序
```

## 子元素事件附加方法

```js
$("selector").delegate(childSelector, functionName, function);
// childSelector 是selector的子元素
// functionName 是为childSelector添加的事件名称
/*
	为子元素添加相应的事件绑定, 如果是动态添加的子元素, 也可绑定相应的事件方法
*/
//例如: 
$("div").delegate("$('p')", "click", function() {
    $(this).css("color", "red");
}); //此方法绑定了div中所有的p元素, 点击之后color变成红色
// 于此对应, 有undelegate()方法
```

## 事件轮转方法

```js
$("selector").toggle(function1, function2, ...,functionn); //可绑定一个或多个方法, 使其中的方法轮转执行
// 例如
$("p").toggle(
  function(){
  $("body").css("background-color","green");},
  function(){
  $("body").css("background-color","red");},
  function(){
  $("body").css("background-color","yellow");}
); //此方法会使body变换颜色, 每执行一次toggle方法, 变一次颜色. 第一次green, 第二次red....
```

## 事件激发方法

```js
$("selector").trigger(event);//激发selector的event事件
$("button").click(function(){
  $("input").trigger("select");
}); // 此方法执行时候, 会激发input的select方法
```

## 事件属性

> 以下属性执行时候, 需要为方法传递一个event事件参数
>
> 示例可查看w3c: 
>
> [查看事件属性举例]: http://www.w3school.com.cn/jquery/event_preventdefault.asp	"w3c出品"
>
> 

```js
event.pageX, event.pageY //相对于文档左边缘和上边缘的鼠标指针位置

event.preventDefault(); //阻止事件的默认行为, 例如阻止submit事件默认提交行为
$("a").click(function(event){
  event.preventDefault();
});

event.result(); //事件返回的结果
$("button").click(function(e) {
  $("p").html(e.result);
});

event.target(); //返回该事件触发的DOM元素
$("p, button, h1, h2").click(function(event){
  $("div").html("Triggered by a " + event.target.nodeName + " element.");
});

event.timeStamp// 该属性返回从1970年1月1日到目前的时间戳
$("button").click(function(event){
  $("span")html(event.timeStamp);
});

event.type //返回触发的事件类型
$("p").bind('click dblclick mouseover mouseout',function(event){
  $("div").html("Event: " + event.type);
});

event.which //返回按下按键的按键序号, 也就是按下了哪个键
```



## jQuery 名称冲突

jQuery 使用 $ 符号作为 jQuery 的简介方式。
某些其他 JavaScript 库中的函数（比如 Prototype）同样使用 $ 符号。
使用noConflict()方法来为$改变名字

```js
var jq = jQuery.noConflict(); //将$改为jq 
```

# jquery DOM操作

> jQuery 中非常重要的部分，就是操作 DOM 的能力。
>
> jQuery 提供一系列与 DOM 相关的方法，这使访问和操作元素和属性变得很容易

## jquery 文档操作

1. addClass();

```js
addClass(); // 向某个元素添加一个类
$("button").click(function(){
  $("p:first").addClass("intro");
});
```
2. 元素之外追加

```js
after(); //在指定元素之后插入内容, 外部插入
//插入文本
$("button").click(function(){
  $("p").after("<p>Hello world!</p>");
});
//使用方法插入
$(selector).after(function(index)); //index 接收selector的index位置

insertAfter(); //将指定内容插入到匹配的元素之后
$("button").click(function(){
  $("<span>Hello world!</span>").insertAfter("p");
});

before(); // 和after()使用相同, 在元素之前插入, 外部插入

insertBefore(); // 和insertAfter使用相同
```
3. 元素之内追加

```js
append(); //在指定元素之后插入内容, 内部插入
//插入文本
$("button").click(function(){
  $("p").append(" <b>Hello world!</b>");
});
//使用方法插入
$(selector).append(function(index,html)); 
//index 接受selector的index位置, html 可选。接收选择器的当前 HTML

appendTo(); //追加内容到某个元素, 和append功能相同
$("button").click(function(){
  $("<b>Hello World!</b>").appendTo("p");
}); // 将内容添加到P元素的末尾

prepend(); //在元素的最前边追加

prependTO(); // 和appendTo() 方法使用相同
```
4. 删除移动元素和属性

```js
detach(); //删除一个匹配的元素, 包括事件, 子元素, 文本, 并返回. 此方法可以用来进行移动节点

empty(); //删除匹配元素的所有内容

clone(flag); //返回一个事件的副本, flag为boolean值, 是否复制事件处理器

remove(); //和detach();方法使用相同, 不同的是remove不会保留事件对象

removeAttr(); //所有匹配的元素中移除指定的属性。

removeClass(); //从所有匹配的元素中删除全部或者指定的类。

replaceAll();//用匹配的元素替换所有匹配到的元素。
$(".btn1").click(function(){
   $("p").replaceAll("<b>Hello world!</b>");
});

replaceWith(); //方法用指定的 HTML 内容或元素替换被选元素
$(".btn1").click(function(){
   $("p").replaceWith("<b>Hello world!</b>");
});
$(selector).replaceWith(function()); //使用函数来替换

toggleClass(); //从匹配的元素中添加或删除一个类。

unwrap(); // 移除并替换所选元素的父元素

wrap(); //用指定的内容, 把所选元素匹配起来
$(".btn1").click(function(){
   $("p").wrap("<div></div>");
});

wrapAll(); //把所有匹配的元素用指定内容包裹起来, 和wrap()不同的是, wrapAll全部包裹, wrap是, 分别包裹

wrapinner(); //将匹配的元素内容用指定的内容包裹起来
```

5. 查看元素

```js
attr(); //返回或设置selector的属性
$(selector).attr(attribute, [value]); 
//value可选, 如果传入了value, 则是设置, 否则是获取属性

hasClass(); //检查是否包含某个类

html(); //相当于innerHTML

text(); //相当于innerText

val(); //匹配元素的值
```

## jquery 属性操作

![1558705333222](https://raw.githubusercontent.com/jssda/picbed/master/1558705333222.png)

## jquery 的css操作

```js
css(); //设置或返回匹配元素的样式属性。

height(); width(); //设置或获取元素的高或宽, 返回number, 并不是字符串

offset(); //返回一个对象, 相对文档的位置
$(".btn1").click(function(){
  x=$("p").offset();
  $("#span1").text(x.left);
  $("#span2").text(x.top);
});

offsetParent(); // 方法返回最近的祖先定位元素。
$("button").click(function(){
  $("p").offsetParent().css("background-color","red");
});

position(); //返回相对父元素的位置, 相对位置

scrollLeft(); //设置或返回匹配元素相对滚动条左侧的偏移。
scrollTop(); //设置或返回匹配元素相对滚动条顶部的偏移。

```


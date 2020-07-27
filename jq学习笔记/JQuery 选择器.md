[TOC]

# JQuery选择器

jq虽然使js的库, 但是对于dom来说, jq和js分别有自己的一套独立的方法, 不能混用

jq中的选择器类型和css的选择器规则基本相同, 学会css选择器, jq的选择器很容易上手

## 基本选择器

### id选择器

使用jq选择一个id为zh的input标签

```js
// jq获得id为zh的inp标签
let inp2 = $("#zh");
// 返回一个Object类型
alert(inp2);
```

### jq和js之间对象的转换

js转成jq  直接使用$(变量)进行转换

jq转成js  jq本身就是js对象组成的一个数组, 所以标量[下标]就可转换

```js
// js选择id为zh的input标签
let inp1 = document.getElementById("zh");
// 返回一个InputHTMLElement类型
alert(inp1);
// 将js对象转换成jq对象
let inp3 = $(inp1);
alert(inp3);

// jq获得id为zh的inp标签
let inp2 = $("#zh");
// 返回一个Object类型
alert(inp2);
// 将jq对象转成一个js对象
let inp4 = inp2[0];
alert(inp4);
```

### 标签选择器

```js
let inp = $("input");
alert(inp);
// 获取这个数组中的一个js对象
alert(inp[0]);
// 获取这个数组中的一个jq对象
alert(inp.eq(0));
```

### class选择器

html代码

```html
<div class="notMe">div class="notMe"</div>
<div class="myClass">div class="myClass"</div>
<span class="myClass">span class="myClass"</span>
```

jq代码

```js
$(".myClass");
```

结果

```html
[ <div class="myClass">div class="myClass"</div>, <span class="myClass">span class="myClass"</span> ]
```

### 通用选择器

```js
$(*)
```

### 分组选择器

不同选择器之间使用逗号分隔

```
$(p, #id, .class);
```

## 层级选择器

### 后代选择器

选择指定元素的所有后代元素

```js
$("form input")
```

### 子元素选择器

选择指定元素的直系子元素

```js
$("form > input")
```

### 相邻兄弟元素

和css一样, 只能选择与指定元素相邻的兄弟元素

```js
$("label + input")
```

### 所有兄弟元素

选择所有兄弟元素

```js
$("form ~ input")
```

## 基本伪类选择器

存在一个html代码

```html
<ul>
    <li>List  Item1</li>
    <li>List  Item2</li>
    <li>List  Item3</li>
    <li>List  Item4</li>
</ul>	

<ul>
    <li>List  Item1</li> 
    <li>List  Item1</li>
</ul>
```

### :first选择器

```js
// 选择第一个li
/*
	使用$(ul li)的时候, 相当于使用$(li). jq会把所有的li当作一个整体, 所以再使用$(ul li:first)选择器的使用, 只选择到了第一个ul中的第一个li
*/
$(ul li:first);
// 效果等同于
$(li:first);
```

### :last选择器

```js
// 选择最后一个li

$(ul li:last);
// 道理和:first选择器相同, 只选择到了第二个ul的第二个li
// 效果等同于 
$(li:last);
```

### :not筛选选择器

使用not可以过滤掉一些不需要的元素

```js
// 选择不是没有被选择的checkbox表单元素
$(input:not(checked));
```

### :even 选择器

选择偶数

### :odd选择器

选择奇数

### :eq选择器

选择指定位数的元素

### :gt, :lt

:gt  大于指定数字

:lt  小于指定数字

### 标题选择器

选择所有的h1  h2 ... 标题元素

```js
$(":header").css("background", "#EEE");
```

### 动画选择器

选择所有正在执行动画的元素

```js
$("div:animated");
```

## 子元素选择器

### 指定子元素选择器

```js
// 可选择指定位置的子元素
:nth-child(even)
:nth-child(odd)
// 匹配指定规则
:nth-child(3n)
:nth-child(2)
:nth-child(3n+1)
:nth-child(3n+2)

// 例如选择所有的ul下的第一个li
$(ul li:nth-child(1)); // 与first不同, 这个标签会选择所有的ul下的第一个li, 而不是将所有li看作是一个整体取第一个
```

### 第一个子元素

```js
$("ul li:first-child")
```

### 最后一个子元素

```js
$("ul li:last-child")
```

### 只匹配具有一个子元素的子元素

```js
$("ul li:only-child")
```

## 属性选择器

### 查找含有指定属性的选择器

```js
// 查找所有含有id属性的选择器
$("div[id]");
```

### 查找指定属性等于/不等于某个值的选择器

```js
$("input[name='newsletter']");
$("input[name!='newsletter']")
```

### 查找以某个字符串开头或结尾或包含某个字符串的选择器

```js
// 以news开头的name属性的元素
$("input[name^='news']")
// 指定字符串结尾
$("input[name$='letter']")
// 包含指定元素
$("input[name*='man']")
```

### 复合属性选择器

```js
$("input[id][name$='man']")
```

## 表单选择器

### :input

匹配所有input, textarea, select和button元素. 

```js
$(":input");// 能够匹配到所有form下的表单属性, 包括textarea, select和buttion
// 和input标签不同
$("input");// 不能匹配textarea等属性
```

### :text

```js
// 匹配input下的type=text的元素
$(":text");
// 等同于这么用
$("input[type=text]");

// 类似的还有:password, :radio, :checkbox, :submit, :image, :reset, :button, :file, :hidden
```

### :hidden

能够匹配所有不可见元素, 或者type为hidden的元素. display:none的元素也能匹配

## 其他一些选择器

### :empty 选择器

匹配所有不包含子元素或者文本的空元素

HTML代码

```html
<table>
  <tr><td>Value 1</td><td></td></tr>
  <tr><td>Value 2</td><td></td></tr>
</table>
```

```js
$("td:empty")
```

结果

```html
[ <td></td>, <td></td> ]
```


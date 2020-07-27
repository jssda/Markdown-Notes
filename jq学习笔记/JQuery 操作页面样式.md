# JQuery操作页面的样式

## 获得一个css样式

```js
// 获得id为div1的元素对象
var div = $("#div1");
// 获得css样式
var wid = div.css("width");
var height = div.css("height");
```

## 操作css样式

```js
// 操作样式
div.css("width", "300px");
div.scc("height", "600px");

// 使用json格式设置样式
div.css({'width':'300px', 'height':'600px', 'backgroudColor':'red'});
```

## 通过操作CSS类来操作样式

### 使用属性操作

class属于元素的一个属性, 所以, 可以通过操作class属性来增加或者删除css的类

```js
$("#bu1").click(function(){
    //获得div对象 --通过添加类的方式添加css样式
    $("#div1").attr("class","div");
});
```

### 可以直接操作css类

```js
$("#div1").addClass("div");
// 也可以删除
$("#div1").removeClass("div");

toggleClass(class, switch);
// 如果switch=true, 则加上相应的class, 如果为false, 则删除.

// 每点击三下加上一次 'highlight' 类

// HTML 代码:
<strong>jQuery 代码:</strong>

// jQuery 代码:
var count = 0;
$("p").click(function(){
    $(this).toggleClass("highlight", count++ % 3 == 0);
});
```


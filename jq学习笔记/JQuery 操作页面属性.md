# JQuery操作页面属性

## 获得页面的属性

```js
//获得元素对象
var tex=$("#inp1");

//获得元素对象的属性
var te=tex.attr("type");
var cl =tex.attr("class");

//获得元素固有的属性值
var val =tex.attr("value");

//获得文本框实时输入的值
var val2=tex.val();
```

## 使用attr获取value和使用val()方法获取value的区别

使用attr("value”)获得的value值使页面的默认值, 不是页面的实时动态的值, 就像使用js调用getAttribute("value”)获得的值是一样的. 

使用val() 获得的值, 是动态的值, 用户如果再页面对value值进行了改变, 使用val() 也能获取到. 和js调用 variable.value的操作是一样的.

## 操作页面的属性

```js
tex.attr("type", "buttion");
```

### 支持json格式的操作

```js
tex.attr({'type':'button', 'value':'测试按钮'});
```

### 操作属性值值相同的属性

```js
// checked 的属性和它的值是相同的, 都是checked.
// 设置一个表单的元素是否checked
$("#fav").attr("checked");
// 也可以这样操作
$("#fav").prop("checked", true); // 设置表单为checked
```


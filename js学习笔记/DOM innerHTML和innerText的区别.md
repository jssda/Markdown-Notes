# 操作文本结点

## 存在一个div

```html
<div id="div1">
	<span>我们不一样</span> 
</div>
```



## 获得元素节点

```js
let div = document.getElementById("div1");
```

## 使用innerHTML

可见输出: `<span>我们不一样</span>`

```js
var inn = div.innerHTML;
console.log(inn);
```

## 使用InnerText

可见输出: 我们不一样

```js
var inn2 = dov.innerText;
console.log(inn2);
```

## 可使用innerHTML添加一些标签信息

```js
//此时添加上的信息, h1标签会当作html中的标签存在.
div.innerHTML="<h1> 我们没什么不一样 </h1>"

// <h1>标签会当作一个文本被添加
div.innerText="<h1> 我们没什么不一样 </h1>";
```

## js操作文本节点注意事项

一般操作双标签的文本节点的时候, 使用innerHTML或者innerText. 操作单标签中的数据, 使用value属性. 比如 对于一个input标签来说, 其中的内容就是value属性. 

但是双标签存在几个例外. select标签, textares标签均使用value获取其中的文本内容.
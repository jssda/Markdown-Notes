# JQuery操作页面的元素

## 创建一个新的元素

```js
var p = $("<p>这是一个段落</p>");
```

## 在div中的最后追加一个元素

```js
$("#div1").append(p);
// 等用于这样写
p.appendTo("#div1");
```

## 在div中的最前表添加一个元素

```js
$("#div1").preppend(p);
// 也可以这样写
p.preppendTo("#div1");
```

## 在div的后面添加一个元素

```js
$("#div1").after(p);
// 也可以这样写
p.insertAfter("#div1");
```

## 在div的前面添加一个元素

```js
$("#div1").before(p);
// 也可以这样写
p.insertBefore("#div1");
```

## 替换指定节点

```js
$("div p:nth-child(1)").replaceWith(p);
// 也可以这样写
p.replaceAll("div p:nth-child(1)");
```

## 删除指定的节点元素

```js
$("#div1").remove();
```

## 清空其中的所有内容

```js
$("#div1").empty();
```


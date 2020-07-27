# DOM获取元素对象的方式

## 直接获得对象的方式

### 通过id名获得

```js
var div = document.getElementById("div1");
```

### 通过标签名获得

```js
/*返回一个HTMLCollection对象*/
var inps = document.getElementByTagName("input");

// 获取其中一个, 获取到一个HTMLInputElement对象
var inp = inps[0];
```

### 通过name属性获得

```js
var inps = document.getElementByName("inp");
```

### 通过class的名称获得

```js
document.getElementByClassName("inp1");
```

## 间接获得对象的方式


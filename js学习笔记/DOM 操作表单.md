# DOM操作表单

DOM操作表单, 对于一个后台程序员来说是很重要的

## 两个属性

### readonly

故名思意, readonly属性只能看到数据, 但是不可以操作数据.

```html
<input type="text" readonly="readonly"/>
```

### disabled

不能使用, disabled也是不能操作属性. 不仅如此, 整个表单都会变成灰色. 而且在提交的时候, 具有disabled属性的input不能被提交至服务器.

```html
<input type="text" disabled="disabled"/>
```

## 控制表单提交的方式

### 使用submit提交按钮

```html
<!-- check() 是一个具有返回值的js函数. 如果返回值为false, 则不会提交 -->
<input type="submit" onsibmit="return check()"/>
```

### 使用button控制提交

```js
// 通过dom操作表单进行提交
let form = document.getElementById("form1")l
form.submit();

// 另一种方法
document.form.submit();
```




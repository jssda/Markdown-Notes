## Location对象

```js
var href=window.location.href; // 获得当前url
var port = window.location.port; // 获得当前端口
var hostname=window.location.hostname; // 获得当前服务器主机名
var host = window.location.host; //获得当前主机名加端口

// 修改当前url地址
window.location.href = "https://www.baidu.com";
// 刷新网页
window.location.reload();
```

## History 对象

```js
// 返回历史记录的数量
var len = window.history.length

// 后退按钮
window.history.back();

// 前进按钮
window.history.forword();

// 前进或后退指定步数
window.histroy.go(-1);
```

## Screen对象

```js
// 获得当前屏幕的分辨率
var he = window.screen.height;
var wi = window.screen.width;
```

## Navigator对象

```js
// 返回由客户机发送服务器的user-agent头部的值
window.navigator.userAgent; // 请求头部的用户代理信息
```


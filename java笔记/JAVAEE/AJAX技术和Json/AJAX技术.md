# AJAX技术

## 什么是AJAX?

> AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
>
> AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

目前浏览器向服务器发起请求的方式有 form 标签，超链接，
还有脚本语言(js\jquery)中的window.location.href 方式和表单提交函数。但是不管哪种提交方式，浏览器都会将接收到的响应内容
覆盖在请求网页中显示. 假如需要在局部位置显示服务器的响应, 就需要AJAX的使用了

## Ajax 运行原理

Http服务是通过浏览器发送一个请求到服务器. 服务器响应到浏览器. 浏览器显示响应的信息. 

Ajax只是通过js里边的Ajax引擎发送一个HTTP请求到服务器, 服务器本身并不知道这个请求是浏览器还是Ajax引擎发过来的. 它只会处理之后发送回去.所以 一个Ajax发送的请求, 会经过服务器处理之后, 信息发送回Ajax引擎. Ajax引擎对接收到的响应信息处理, 通过js显示. 所以就变成了异步的信息交互

## Ajax使用流程

1. 创建一个ajax引擎
   - 创建Ajax引擎的时候注意浏览器兼容性
   - IE5和IE6使用的是ActiveXObject对象
   - 其他的浏览器使用的是XMLHTTPRequest对象
2. 声明监听ajax状态码的事件
   - ajax状态码有5种状态
   - 0: 当readyState状态码为0的时候, 表示ajax引擎对象创建
   - 1: ajax 打开引擎请求的服务器url地址和请求方式
   - 2: 发送请求
   - 3: 正在接收响应
   - 4: 响应接收完成
3. 监听ajax状态码的事件会执行多次, 每次ajax状态码的改变都会执行一次. 所以在判断状态码为4的时候, 执行ajax进行完成的时候的事件. 
4. 打开请求链接
5. 发送Ajax请求

## Get方式发送Ajax请求

```js
function submit() {
    // 创建Ajax引擎对象, 即xhr-xmlHttpRequest
    var ajax;
    if (window.XMLHttpRequest) { // 火狐浏览器兼容设置
        ajax = new XMLHttpRequest();
    } else if (window.ActiveXObject){ // IE兼容配置
        ajax = new ActiveXObject("Microsoft.XMLHTTP");
    }

    // 声明Ajax事件监听
    ajax.onreadystatechange= function () {
        // 判断Ajax状态码
		/*
                0: 当readyState状态码为0的时候, 表示ajax引擎对象创建
                1: 表示请求创建但是未发送. ajax.open
                2: 发送请求 ajax.send
                3: 正在接受响应内容
                4: 响应内容接收完毕
        */
        if (ajax.readyState === 4) {
            // 判断响应状态码
            if (ajax.status === 200) {
                // 获取响应内容
                var responseText = ajax.responseText;
                var div1 = document.getElementById("div1");
                div1.innerHTML = responseText;
            }
        }
    };

    // get方式打开ajax请求链接
    ajax.open("get", "my?username=jssd&password=123456");
    // 发送请求对象
    ajax.send(null);
}
```

## Post方式发送Ajax请求

和get方式发送请求基本差不多, 但是打开链接的时候, 需要指明请求方式. 发送请求对象的时候需要传递参数

```js
// get方式打开ajax请求链接
ajax.open("post", "my");

//设置请求参数为键值对方式, 只需要在post方式中声明
ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
// 发送请求对象
ajax.send("name=jssd&password=123456");
```

## Ajax的异步和同步

在ajax使用open方法的时候, 他的第三个参数是个可选的Boolean变量. 默认为true, 开启异步支持. 如果手动传入一个false, 那么, 会使用同步设置. 

### 异步

可以看到, 在Ajax中, 有一个手动触发的函数, 在执行这个手动触发的函数的同时, 会添加一个事件函数. 这个事件函数手动触发的函数之间是怎么运行的呢? 事件函数的运行是根据状态码的改变而触发的, 状态码的改变是在手动触发的函数中完成的. 默认的异步情况下. 这两个函数是互不干涉的. 这就是异步执行.  假如函数中有这样一句代码

```js
// get方式打开ajax请求链接
ajax.open("post", "my");

//设置请求参数为键值对方式, 只需要在post方式中声明
ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
// 发送请求对象
ajax.send("name=jssd&password=123456");

// 测试异步执行
document.getElementById("div1").innerText = "能够触发";
```

注意在send发送之后, 会触发状态码改变函数. 默认异步的情况下, 不管状态码改变函数是否执行完, 服务器是否响应完毕, 都会继续向下执行改变div的那句代码. 也就是说, 两个函数是两个线程执行的. 是不同步的.

### 同步

与异步恰恰相反, 在xhr执行send函数之后, 这个线程会阻塞. 等待状态码改变触发的那个函数执行完毕之后, 会唤醒阻塞的函数继续执行. 也就是说, 等服务器响应完成之后, div改变的代码才会执行, 假如服务器一致没有响应, 这句代码就一直不会执行.

## ajax的封装

有一个我自己封装的Ajax. 

```js
/**
 * 我自己封装的Ajax方式
 *
 * @param method 请求方式 get或者post
 * @param url 请求的url, 不包括请求的参数
 * @param parameters 请求的参数格式为(name=userName&password=userPassword)
 * @param callBack 回调函数. 此函数接收一个服务器响应文本的参数.
 * @param asyn 是否异步进行Ajax请求, 默认是异步的.
 */
function myAjax(method, url, parameters, callBack, asyn) {
    if (!asyn) {
        asyn = true;
    }

    // 创建Ajax引擎对象
    var ajax;
    if (window.XMLHttpRequest) { // 火狐浏览器兼容设置
        ajax = new XMLHttpRequest();
    } else if (window.ActiveXObject){ // IE兼容配置
        ajax = new ActiveXObject("Microsoft.XMLHTTP");
    }

    // 声明Ajax事件监听
    ajax.onreadystatechange= function () {
        if (ajax.readyState === 4) {
            // 判断响应状态码
            if (ajax.status === 200) {
                var responseText = ajax.responseText;
                callBack(responseText);
            }
        }
    };

    if (method.toLowerCase() === 'get') {
        ajax.open(method, url + "?" + parameters, asyn);
        ajax.send(null);
    } else if (method.toLowerCase() === 'post') {
        ajax.open(method, url, asyn);
        ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        ajax.send(parameters);
    }
}
```

使用的时候先引入, 在调用

```html
<script type="text/javascript" src="ajaxUtil.js"></script>
<script type="text/javascript">
    function submit() {
        myAjax("get", "my", "name=jssd&password=123456", function (responseText) {
            // 获取响应内容
            var div1 = document.getElementById("div1");
            div1.innerHTML = responseText;
        });
    }
</script>
```

## 在JQuery中使用Ajax

### load方法

load方法有三个参数, 必须在JQuery对象上触发, 发送Ajax请求

格式: load(url, [data], [callback])

1. url: 请求路径.

2. data: 请求参数. 参数格式为JSON

   假如没有参数, 默认格式为get, 有参数则变为POST

3. callback: 执行成功返回的状态码为200的时候, 回调的函数. 此回调函数有三个参数

   1. 返回值: 一般为JSON, 看服务器返回什么
   2. 状态status: 其值可能是succuss, error, notmodify, timeout
   3. ajax引擎对象xhr: XMLHttpRequest

示例: 

```jsp
<head>
    <title>JQuery学习Ajax</title>

    <style type="text/css">
        #div1 {
            height: 150px;
            background-color: antiquewhite;
            color: black;
        }
    </style>

    <script type="text/javascript" src="js/jquery-1.9.1.js"></script>
    <script type="text/javascript">
    $(function () {
        $("#sub").click(function () {
            alert("触发一下");
            $("#div1").load("my", {"name": "jssd", "password": "123456"}, function (responseText, status, xhr) {
                $(this).html(responseText + "<br>");
                $(this).html($(this).html() + "状态: " + status + "<br>");
                $(this).html($(this).html() + "xhr: " + xhr + "<br>");
            });
        });
    })
</script>

</head>
<body>
<h2>load()方法</h2>
<input type="button" id="sub" value="测试ajax提交服务">
<div id="div1">
</div>

</body>
```

### get方法

get方式发送的就是一个get请求

格式: jQuery.get(url, [data], [calback], [type])

- 前三个参数和load一样. 
- type: 返回内容格式.
  - xml
  - html
  - script
  - json
  - text
  - _default(默认为json格式)

使用: 

```html
<script type="text/javascript">
    function submit() {
    myAjax("get", "my", "name=jssd&password=123456", function (responseText) {
        // 获取响应内容
        var div1 = document.getElementById("div1");
        div1.innerHTML = responseText;
    });
}
</script>
```

### post方法

和get一致. 就是post方法使用的是post请求方式

### $.ajax方法

比get方法和post方法更加强大

格式: jQuery.ajax(settings); setting设置使用json格式设置多项数据

settings常用参数列表

1. async: 是否异步true则异步, false同步
2. type: 请求方式: get, post
3. url: 请求路径
4. data: 请求参数
5. dataType: 设置服务器数据返回类型
6. beforeSend 在发送请求之前调用，并且传入一个XMLHttpRequest作为参数。  
7. error 在请求出错时调用。传入XMLHttpRequest对象，描述错误类型的字符串以及一个异常对象（如果有的话）  
8. dataFilter  在请求成功之后调用。传入返回的数据以及"dataType"参数的值。并且必须返回新的数据（可能是处理过的）传递给success回调函数。  
9. success 当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。  
10. complete 当请求完成之后调用这个函数，无论成功或失败。传入XMLHttpRequest对象，以及一个包含成功或错误代码的字符串。

### 表单序列化

serialize();

将表单所有的内容转换成一个字符串. 字符串的格式为key=value&key2=value

### jQuery.parseJSON(json)

将一个json字符串转换成一个jq对象.


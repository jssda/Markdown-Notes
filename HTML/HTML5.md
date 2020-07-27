# HTML5简介

## 相对于HTML4.0.1来说的区别

1. 去除了不常用的标签
2. 增加了很多语义标签
3. 表单中结合的JS使用

## 缺点

对于PC端的不同浏览器兼容性不同, 需要有各种浏览器进行测试

火狐, 谷歌, IE, 苹果

## HTML5表单增强项

```html
<form action="" method="get">

    <p>邮箱标签: <input type="email"></p>
    <p>数字标签: <input type="number"></p>
    <p>滑动条标签: <input type="range"></p>
    <p>搜索框标签: <input type="search"></p>
    <p>日期框: <input type="date"></p>
    <p>星期框: <input type="week"></p>
    <p>月份框: <input type="month"></p>
    <p>颜色框: <input type="color"></p>
    <p>网址框: <input type="url"></p>

    <p><input type="submit">
        <input type="reset"></p>
</form>
```

## HTML5中表单增强属性

- placeholder: 表单内部属性提示
- autofocus: 自动获得焦点
- max: 最大值
- min: 最小值
- minlength: 最小程度
- maxlength: 最大长度

```html
<form action="">
    <p>用户名: <input type="text" placeholder="用户名"></p>
    <p>年龄: <input type="number" max="120" min="0"></p>
    <p>密码: <input type="password" minlength="6" maxlength="20"></p>

    <p><input type="submit">
        <input type="reset"></p>
</form>
```

## HTML5结构增强

参考博客:[https://www.cnblogs.com/fly_dragon/archive/2012/05/
25/2516142.html ](https://www.cnblogs.com/fly_dragon/archive/2012/05/
25/2516142.html )

```html
<!--头部模块-->
<header></header>
<!--中间提示-->
<nav></nav>
<!--中间的展现-->
<div class="center">
<div class="login">
</div>
</div>
<!--底部模块-->
<footer></footer>
```

## HTML5音频与视频增强

```html
<!--引入音频的标签, 可使用autoplay属性自动播放-->
<audio src="img/1.mp3" controls="controls">
改网页不支持媒体标签
</audio>

<audio>
    <source src="img/1.mp3"></source>
    <source src="img/1.ogg"></source>
    改网页不支持媒体标签
</audio>

<!--引入视频的标签-->
<video src="img/movie.mp4" controls="controls"
width="300px" height="300px"></video>

<video>    
    <source src="img/movie.mp4"></source>
    <source src="img/movie.ogg"></source>
    <source src="img/movie.webm"></source>
    改网页不支持媒体标签
</video>

<!--多媒体标签 自动播放-->
<embed src="img/1.mp3"></embed>
<embed src="img/movie.mp4" width="500px"
height="500px">
</embed>
```

## HTML5中的其他标签

### 类似于自定义列表的标签

```html
<figure>
	<img src="img/1.jpg" />
    <figcaption>IT程序员</figcaption>
</figure>
```

### 展示文章细节标签

```html
<details>
	<summary>总结</summary>
    <p>
        描述信息
    </p>
</details>
```

### 使用底色着重标记标签

```html
<mark>这个是着重标签</mark>
```

### 刻度标签

- max:规定的最大值
- min:规定最小值
- value：当前的值
- low：自己定义的最小值 
- high: 自己定义的最大值

小于或大于自己定义的值之后会变颜色

```html
<meter max="100" min="0" value="10" low="20" high="80"></meter>
```

### 进度条

```html
<progress max="100" value="40"></progress>
```

### 列表选择

```html
<datalist id="city">
    <option value="IBM">IBM</option>
    <option value="IBM1">IBM1</option>
    <option value="IBM2">IBM2</option>
    <option value="IBM3">IBM3</option>
</datalist>
```

### 画布标签

```html
<canvas id="mycat"></canvas>

<script>
	var ca = document.getElementById("mycat");
    var te = ca.getContext("2d");
    // 背景颜色
    te.fillStyle="#FF000";
    // 绘制图形的大小
    te.fillRect(0,0,80,100);
</script>
```


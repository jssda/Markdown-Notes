# CSS入门

## 为什么学习css(why)

1. HTML虽然可以在一定程度上修饰页面, 但是页面的整体不够美观
2. HTML进行网页的书写, 重复代码比较多, 后期的维护不太好

## 什么是css(what)

CSS是层叠样式表(Sascading style sheets)的英文字母缩写

## CSS引入方式

### 行内样式

```html
<p style="color:red">这里是红色的</p>
```

### 内嵌样式

```html
<head>
    <style>
        p{
            /*字体颜色*/
            color: red;
            /*字体大小*/
            font-size: 20px;
            /*字体加粗*/
            font-weight: bold;
        }
    </style>
</head>
```

### 外联样式

rel: 当前的文档和引入的文档的关系 

href:是指引入文档的关系 

```html
<link rel="stylesheet" type="text/css" href="css/css1.css"/>
```

### 导入式

不建议使用, 了解即可

```html
<style>
	@import url("css/css1.css");
</style>
```

### CSS中三种引入方式遵循就近原则

不相同的样式会进行样式的叠加， 相同的样式会采用就近的
原则。样式跟随距离自己近的风格 .

也就是说, 新的样式会把旧的样式替换
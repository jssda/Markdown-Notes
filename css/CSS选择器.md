# CSS的选择器

## 常用选择器

### 通用选择器

代表该页面中的所有元素

```css
* {
    /*color: red;*/
    /*width: 200px;*/
    /*height: 200px;*/
    margin: 0;
    padding: 0;
}
```

### 元素选择器

选择一个标签元素

```css
div{
    width: 200px;
    height: 200px;
    /*背景颜色*/
    background-color: aqua;

    /*边框的粗细, 风格, 颜色*/
    border: 1px solid blue;
}
```

### id选择器

d选择器保证唯一

id命名规范:
            

- 必须是  数字, 字母, 下划线, 中划线组成
  
- 没有$, 不能数字开头

```css
#f_div{
    background-color: #acffe7;
}
```

###  类选择器

不唯一, 选择一个类别

```css
.div_1{
    background-color: pink;
}
```

### 三种选择器的优先级

id选择器 > class选择器 > 标签选择器

权重: 100 	20					1

## 其他选择器

### 后代选择器

可选择它的后代元素, 包括孙子元素

```css
div span {
    color: #9ba2ff;
}
```

### 子选择器

与后代选择器的不同是, 子选择器只会选择它的直系字元素

```css
div > span {
    color: cornflowerblue;
}
```

### 相邻兄弟选择器

选择指定元素的相邻的下边的元素

```css
#f_div + div{

}
```

### 全部兄弟选择器

后面所有的兄弟元素都会被选择

```css
#f_div~ div{}
```

### 伪类选择器

四种伪类:
           

1. a:link 未访问的链接
2. a:visited 以访问的链接
   
3.  a:hover 鼠标移动到链接上
    
4.  a:active 选定的链接
               

如果四种伪类要组合使用, 必须要有一定的顺序
            

- a:hover 必须被置于a:link 和 a:visited之后, 才是有效的
  
- a:active 必须被置于a:hover之后才是有效的

```css
a:hover{}
```


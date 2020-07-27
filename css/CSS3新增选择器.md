# CSS3选择器

## 子元素选择伪类

### 获得子元素中的第一个元素

```css
.div_1>p:first-child{}
```

### 获得子元素的最后一个元素

```css
.div_1>p:last-child{}
```

### 获得具体的一个子元素

```css
/*获得第二个子元素*/
.div_1>p:nth-child(2) {}
```

## 获得奇/偶数子元素

```css
.div_1>p:nth-child(even){}
.div_1>p:nth-child(odd){}
```

### 获得空元素对象

```css
.div>p:empty{}
```

## input标签中的伪类

### 获得焦点的伪类

```css
input:focus{}
```

### 被选择的伪类

```css
input:checked{}
```

## 在选择的对象中的内容之前/后插入

```css
div1:before{
    content: url(img/1.jpg);
}

div1:after{
    content: "你好"
}
```

## 属性选择器

选择类型为text的input标签

```css
input[type='text']{}
```

选择使用某几个字母开头的name

```css
/*选择https开头的name*/
input[name^='https']
/*选择com结尾的name*/
input[name$='com']
```

## 选择器的种类

### 基础选择器

1. 通用选择器 *
2. id选择器 #id
3. class选择器 .divclass
4. 标签选择器   div{}

### 关系选择器

1. 后代选择器 空格
2. 子元素选择器 >
3. 临近兄弟选择器 +
4. 所有兄弟选择器 ~

### 伪类选择器

1. 鼠标滑动选择器 hover
2. 访问过的链接  visited
3. input焦点 focus
4. 等等…..

### 伪对象选择器

1. before
2. after

### 属性选择器

input[type=‘text’]


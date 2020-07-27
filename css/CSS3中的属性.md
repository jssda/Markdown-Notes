# CSS3中的属性

## 圆角命令

让边框变成圆角

```css
border-radius: 100px;

border-radios: 10px 20px 30px 40px;
```

 ## 旋转角度

```css
transform: rotate(45deg);
/*retate是顺时针旋转角度*/
```

## 元素从当前位置移动

```css
transform: translate(50px, 100px);
/*把元素从左侧移动 50 像素，从顶端移动 100 像素*/
```

## 改变大小

```css
/* 把宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍。*/
transform: scale(2,4);
```

## 元素翻转

```css
/*围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度。*/
transform: skew(30deg,20deg);
```

## 矩阵旋转

matrix() 方法把所有 2D 转换方法组合在一起。

matrix() 方法需要六个参数，包含数学函数，允许您：旋转、缩放、移动以及倾斜元素。

```css
/*各个属性分别为: x缩放, x旋转, x平移, y缩放, y旋转, y平移*/
transform:matrix(0.866,0.5,-0.5,0.866,0,0);
-ms-transform:matrix(15,0.5,-0.5,0.866,0,0); /* IE 9 */
-moz-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Firefox */
-webkit-transform:matrix(1,0.5,-0.5,0.866,0,0); /* Safari and Chrome */
-o-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Opera */
```

## 元素阴影

```css
/*x平移  y平移  阴影度  颜色*/
box-shadow: 0px 0px 70px indianred;
```

## 动画

```css
@keyframes aj{
    /*起始状态*/
    from{}
    /*终止状态*/
    to{}
}

@keyframes aj{
    0%{transform: scale(1)}
    50%{transform: scale(1.1)}
    100%{transform: scale(1)}
}
```

## 调用动画

```css
 animation: 1s aj infinite;
/*调用时间  动画效果名称  是否重复*/
```

## 过渡

```css
transition:width 2s, height 2s;
/*transition有四个属性
transition	简写属性，用于在一个属性中设置四个过渡属性
transition-property	规定应用过渡的 CSS 属性的名称。
transition-duration	定义过渡效果花费的时间。默认是 0。
transition-timing-function	规定过渡效果的时间曲线。默认是 "ease"。
transition-delay	规定过渡效果何时开始。默认是 0。
*/
```


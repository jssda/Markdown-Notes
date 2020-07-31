# CSS定位

## 绝对定位

position: absolute;

- 定位离开之后, 之前的位置会释放之前的位置. 
- 更改一个div的位置之后, 可能会影响其他div
- 基于外
  层父级标签来说 

## 相对定位

position: relative;

- 定位离开之后, 不会释放之前的位置. 只是移动到了新的位置. 
- 相对于自身原来的位置.

## 固定定位

postition: fixed;

- 相对与浏览器界面.
- 多用于广告

## 默认定位

postion: static: 初始的定位操作

## 层级概念

使用z-index: -1; 可将一个div置于底层.

# CSS盒子模型

![1565951915730](https://raw.githubusercontent.com/jssda/picbed/master/1565951915730.png)

## 特点

一个完整的盒子模型, 具有外边距. 边框, 内边距和内容四个元素

## 内边距

内边距是内容和border之间的距离

```css
padding: 50px;
/* 设置上下左右的巨鹿*/
padding: 30px 50px; /* 上下30, 左右50 */
padding: 10px 20px 30px 40px;  /*上右下左*/
```

## 外边距

外边距是盒子和盒子之间的距离. 外边距可以用来进行定位使用.

```css
margin:50px;
/*也可以单独设置*/
margin-top: 20px;
```


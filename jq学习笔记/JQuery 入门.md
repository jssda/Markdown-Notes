# JQuery入门

## JQuery概述

### 为什么使用JQuery(JS遇到的问题)

- 选择器功能弱
- DOM操作繁琐至极
- 浏览器兼容性不好
- 动画效果弱

### 什么使JQuery(概念)

JQuery (JavaScriptQuery) JavaScript代码库

官方网站: [http://jquery.com/](http://jquery.com/)

## JQuery版本简介

1. 1.x版本: 兼容ie678,使用最为广泛的，官方只做BUG维护，功能
   不再新增。因此一般项目来说，使用1.x版本就可以了，最
   终版本： 1.12.4 (2016年5月20日) 
2. 2.x版本: 不兼容ie678，很少有人使用，官方只做BUG维护，功
   能不再新增。 如果不考虑兼容低版本的浏览器可以使用2.x，
   最终版本： 2.2.4 (2016年5月20日) 
3. 3.x版本: 不兼容 ie678，只支持最新的浏览器。除非特殊要求，
   一般不会使用 3.x 版本的， 很多老的 jQuery 插件不支持这
   个版本。 目前该版本是官方主要更新维护的版本。 

## JQuery中$的基本含义

### 作用一

$(function)

1. 相当于window.onload=function(){}
2. 功能比window.onload更强大
   1. window onload一个页面只能写一个, 但是可以写多个$()而不冲突
   2. window onload 要等整个页面加载完之后再执行(包括图片, 超链接, 音视频等等).
3. 完整形式使$(document).ready(…);
4. $也可以写作jQuery. 比如jquery(function)

### 作用二

$(selector), 当作选择器的标识符号

jQuery具有强大的选择器功能
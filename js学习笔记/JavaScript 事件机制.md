# Event机制

事件是可以被 JavaScript 侦测到的行为 

网页中的每个元素都可以产生某些可以触发 JavaScript
函数的事件。 比方说， 我们可以在用户点击某按钮时产生
一个 onClick 事件来触发某个函数 

事件通常与函数配合使用， 这样就可以通过发生的事件来
驱动函数执行 

## 有哪些事件

| 属性        | 当以下情况发生时, 出现此事件   |
| ----------- | ------------------------------ |
| onabort     | 图像加载被中断                 |
| onblur      | 元素失去焦点                   |
| onchange    | 用户改变域的内容               |
| onclick     | 鼠标单击某个对象               |
| ondbclick   | 鼠标双击某个对象               |
| onerror     | 当加载文档或图像时发生某个错误 |
| onfocus     | 元素获得焦点                   |
| onkeydown   | 某个键盘的键被按下             |
| onkeypress  | 某个键盘的键被按下或者按住     |
| onkeyup     | 某个键盘的键被松开             |
| onload      | 某个页面或图像被完全加载       |
| onmousedown | 某个鼠标按键被按下             |
| onmousemove | 鼠标被移动                     |
| onmouseout  | 鼠标从某元素移开               |
| onmouseover | 鼠标移动到某元素之上           |
| onmouseup   | 鼠标某个按键被松开             |
| onreset     | 重置按钮被点击                 |
| onresize    | 窗口或框架被调整尺寸           |
| onselect    | 文本被选定                     |
| onsubmit    | 提交按钮被点击                 |
| onunload    | 用户退出页面                   |

## 事件使用示例

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script>
			function demo1(){
				alert("单击事件的执行");	
			}
			function demo2(){				
				alert("双击事件的执行");
			}			
			function demo3(){				
				//alert("我来了")				
				alert("我离开了");
			}
			function demo4(){
				console.log("鼠标移动")
			}
			function demo5(){		
				alert("键盘按下");	
			}
			function demo6(){	
				alert("内容改变");	
			}
			function demo7(){
				alert("页面加载完成");
			}
		</script>
	</head>
	<body onload="demo7()">
		
		<!--单击事件-->
		<input type="button" name="" id="" value="单击操作" onclick="demo1();demo2()" /> <br /><br /><br />
		<!--双击事件-->
		<input type="button" name="" id="" value="双击事件" ondblclick="demo2()" /><br /><br /><br />
		<!--鼠标事件   onmouseover：鼠标放上事件   onmouseout:鼠标离开事件  onmousemove:鼠标移动事件 -->
		<div style="width: 300px; height: 300px; background-color: red;"  onmousemove="demo4()"></div><br /><br /><br />
		<!--键盘事件  onkeyup="demo5()" 键盘弹起-->
		<input type="text"  onkeydown="demo5()" /><br /><br /><br />
		<!--焦点事件  onfocus:获得焦点事件  onblur：失去焦点事件  -->
		<input type="text" name="" id="" value=""  onchange="demo6()" />
	</body>
</html>

```


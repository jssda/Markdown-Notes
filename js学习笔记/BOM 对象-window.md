## BOM对象window的一些方法

### window对象的常用属性

| closed      | 返回窗口是否已被关闭。       |
| ----------- | ---------------------------- |
| innerheight | 返回窗口的文档显示区的高度。 |
| innerwidth  | 返回窗口的文档显示区的宽度。 |
| name        | 设置或返回窗口的名称。       |

### window对象常用方法

1. alert(); 显示带有一段消息和一个确认按钮的警告框。
2. blur(); 把键盘焦点从顶层窗口移开。
3. confirm(): 返回一个boolean值, 含有确认取消的弹框
4. prompt();  返回一个String对象, 能输入内容的弹框
5. setTimeout(“getTime()”, 1000);  定时器, 1000毫秒之后, 执行getTIme() 函数
6. setInterval(“getTime()”, 1000); 定时器, 每隔1000sm, 执行一次getTime() 函数
7. clearInterval() clearTimeout(); 清除定时器函数
8. open("https:www.baidu.com"); 打开一个标签页的方法
9. close(); 关闭当前标签页
# prototype属性

prototype属性使得js能够实现继承的效果

## js中的对象

js中的一个方法, 也是js中的一个对象, 就像是java里边的类

比如

```js
function user(age, sex){
    this.name = "张三":
    this.age = age;
    this.sex = sex;
    this.eat = function() {
        alert("我是user中的吃的方法");
    }
}
```

我们可以说, 这是js中的一个类, 也可以说, 这是js中的一个方法.

## 灵活的js对象

js的对象非常灵活, 就像js的数组一样, 可以随意的增添一些属性

```js
// 我新建了一个对象
user u = new user(10, "男");

// 这个user类中没有身高的属性, 我可以给他添上.
u.height = 165;

// 注意, 此时添加上的只是在js对象中的添加, 不是js类中的添加. js类中, 依旧没有身高的属性.
```

## 使用prototype实现继承

可以看到, js的简单灵活一目了然, 但是, 他怎么实现继承的呢?实际上, 他就是通过prototype属性来实现的.

prototype中, 所有的属性会被继承下来

```js
// 新建一个类
function user2() {
    this.run = function() {
        alert("我是user2中的跑的方法");
    }
}

// 使user继承user2
user.prototype = new user2();

// 此时, user类就继承了user2
var us = new user(12, "男");
us.run();
```


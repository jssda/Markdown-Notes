# 一、Vue.js 简介

> Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

- vue.js是目前为止最火的一个前端框架，和Angular.js和React.js并称为前端三大主流框架。
- vue.js是一套用于构建用户界面的框架，只专注于视图层，所以易于上手。

# 二、vue.js学习

## 2.1 引入vue.js

### 2.1.1 开发环境引入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

### 2.1.2 生产环境引入

```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

## 2.2 基础使用

### 2.2.1 字符模板

vue.js是响应式的，我们可以使用简洁的语法讲一个vue对象挂载到div元素中。代码如下

```html
<!--html-->

<div id="app">
    {{ message }}
</div>

```

```js
# js代码

var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello world'
    }
})
```

此时，在名为app的div元素中，会显示“Hello world”字符。vue是响应式的，你可以在控制台中更改app.message='other message'观察改变。

### 2.2.2  v-bind绑定attribute

除了文本插值，我们可以使用v-bind指令动态绑定一个属性。代码如下

```html
<!--html-->
<div id="app" v-bind:title="thisTitle">
    {{ message }}
</div>
```

```js
# js

let app = new Vue({
    el: '#app',
    data:{
        message:'Hello Vue',
        thisTitle: '这是一个title内容'
    }
})
```

### 2.2.3 条件判断

`v-if`指令可以进行条件判断。

```html
<!--html-->
<div id="app" v-bind:title="thisTitle" v-if="display">
    {{ message }}
</div>
```

```js
# js

let app = new Vue({
    el: '#app',
    data:{
        message:'Hello Vue',
        thisTitle: '这是一个title内容',
        display:true
    }
})
```

### 2.2.4 循环

v-for指令可循环输出一个json对象

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

### 2.2.5 绑定事件

v-on指令可绑定事件

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

### 2.2.6 绑定表单

v-model指令可以绑定一个表单，将表单输入的数据转换为vue对象中的属性数据。同样，也是响应式的。

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

### 2.2.7 模块化使用

```html
<ol id="test-template">
    <test-label v-for="item in groceryList" v-bind:i="item" v-bind:key="item.id"></test-label>
</ol>
<script type="application/javascript">

    Vue.component('test-label',{
        props:['i'],
        template:'<li> {{ i.text }} </li>'
    })

    let app = new Vue({
        el: '#test-template',
        data: {
            groceryList: [
                { id: 0, text: '蔬菜' },
                { id: 1, text: '奶酪' },
                { id: 2, text: '随便其它什么人吃的东西' }
            ]
        }
    });
</script>
```

注意，绑定得时候，必须绑定一个key

## 2.3 Vue实例

### 2.3.1 创建一个实例

通过`new Vue({...})`可以创建一个Vue实例，其中，可以通过传递一些json参数来对Vue实例进行初始化。

```html
<script type="application/javascript">    
    
    //创建一个vue实例
    
    let vm = new Vue({
        //选项
    });
</script>
```

### 2.3.2 响应式绑定

当vue实例绑定一个容器的时候，他是响应式的，当对原始的数据进行改变的时候，vue实例也会改变。

```html
<div id="div1">
    <p> {{ abc }}</p>
</div>


<script type="application/javascript">
    let data = {abc : 1}

    let vm = new Vue({
        el:'#div1',
        data:data
    })

    console.log(data.abc == vm.abc); // true
    data.abc = 2; // vm.abc == 2
    vm.abc = 3; // data.abc == 3
</script>
```

**备注1：**当使用`Object.freeze(data)`这个方法的时候，会阻断data和vue实例的绑定。

**备注2：**当新添加一个vue实例的属性时，不能绑定。如`vm.b = hi` 当对vm.b进行更改的时候，并不会响应式的更改视图。

### 2.3.3 实例的原始属性

> 除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$`，以便与用户定义的 property 区分开来。

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

[官方API](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B-property)

### 2.3.4 生命周期方法

在Vue实例创建过程中，有很多阶段。通过生命周期方法，可在Vue实例创建期间做一些事情。常用生命周期方法例如：[`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。

实例被创建之后会执行如下代码。

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

### 2.3.5 生命周期图示

<img src="vue%E5%AD%A6%E4%B9%A0.assets/lifecycle.png" alt="生命周期图示（来自vue官方网站）" style="zoom:50%;" />

## 2.4 模板语法

> Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

### 2.4.1 插值

#### 2.4.1.1 文本

最常用的就是`Mustache`语法，及双大括号语法。

```html
<span>Message: {{ msg }}</span>
```

使用`v-once`指令可以进行一次插值，之后不可更改。

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

#### 2.4.1.2 原始HTML

双大括号语法只能解析出普通文本，不能解析出原始的HTML，为了解析出原始的HTML，需要用到`v-html`指令。

```html
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

#### 2.4.1.3 HTML的attribute绑定

可以使用`v-bind`绑定一个html属性。

```html
<div v-bind:id="dynamicId"></div>
```

**注意：**当使用`v-bind`绑定一个布尔值的数据时，如果绑定的值是`null`、`undefined`或`false`，这个属性就不会渲染进HTML中。

#### 2.4.1.4 JavaScript绑定

对于三目运算符或者任意的**单个表达式**来说，都可以放在双大括号的语法中进行绑定js数据。例如

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

### 2.4.2 指令

> 指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** 。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

#### 2.4.2.1 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute。`v-on`可以传递哪个事件的参数。

```html
<a v-bind:href="url">...</a>

<button v-on:click="doSomething">tick me!</button>
```

#### 2.4.2.2 动态参数

> 从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数

```html
<a v-bind:[attributeName]="url"> ... </a>
```

如上代码，当vue实例中绑定的data有一个属性attributeName为href时，这个`a`标签将会绑定href属性。

当动态绑定的参数值为`null`时，会自动移除这个动态绑定参数。其他任何非`null`的值都将触发一个警告。

#### 2.4.2.3 修饰符

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

#### 2.4.2.4 常用指令缩写

`v-on`缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

`v-bind`缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

## 2.5 计算属性和监听器

###  2.5.1 计算属性

计算属性可以作为一个已有属性的getter方法。

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

`reversedMessage`可以作为一个属性使用，这个属性却依赖于`message`属性。

默认情况下，计算属性只提供一个getter方法，但是你也可以提供一个setter方法。

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

现在再运行 `vm.fullName = 'John Doe'` 时，setter 会被调用。

很显然，使用方法也可以实现这个功能，但是计算属性和方法是不一样的。计算属性只会在对应的属性更改之前运行一遍，之后会访问缓存取得上一次运行的值。直到对应的属性响应式的更改了，他才会重新计算，将新值存入缓存。基于这个原理，如果你在计算属性中返回一个`new Date()`，他不是动态的。而方法是动态的，他会一直执行。

### 2.5.2 监听器

监听器可以监听一个属性的更改，类似于HTML中的`onChange`方法。

```html

<div id="div">
    <input v-model="testAttribute" type="text">
    <p>{{ tips }}</p>
</div>

<script type="application/javascript">

    let vm = new Vue({
        el: '#div',
        data: {
            testAttribute: '',
            tips:''
        },
        watch: {
            // 当testAttribute发生改变的时候,这个函数会运行.
            testAttribute: function (newValue, oldValue) {
                console.log('newValue=' + newValue);
                console.log('oldValue=' + oldValue);
                this.tips='你在干什么?';
            }
        }
    })
</script>
```

## 2.6 绑定Class与Style

### 2.6.1 绑定HTML Class

> 操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们。

#### 1、对象语法

> 我们可以传给 `v-bind:class` 一个对象，以动态地切换 class。

```html
<div id="app" v-bind:class="{ blackClass:isError }">
这里有字
</div>
<script type="application/javascript">

    let vm = new Vue({
        el:'#app',
        data:{
            isActive:false,
            isError:true
        }
    })

</script>
```

当isError是false的时候，blackClass就不会渲染。 这种方法是显示的绑定一个class对象，这个对象不一定要内联，我们可以吧这个对象写道data数据中去，直接绑定对于的属性。

#### 2、数组语法

将一个或多个属性数组绑定个`bind:class`时候，可以用数组，此时，绑定的会是属性对应的值。

```html
<div id="app" v-bind:class="[test1, test2]"> <!--这里会渲染两个class，分别为：testClass，testClass2-->
这里有字
</div>

<script type="application/javascript">
    let vm = new Vue({
        el:'#app',
        data:{
            test1: 'testClass',
            test2: 'testClass2'
        }
    })
</script>
```

当然，数组中也可以存放一个对象。

```php+HTML
<div id="app" v-bind:class="[{test1:isActive}, test2]">
```

### 2.6.2 绑定内联样式

#### 1、对象语法

直接绑定一个数据对象，可将对象渲染成`style`。

```html
<div v-bind:style="styleObject"></div>
<script type="application/javascript">
let vm = new Vue({
        el:'#app',
        data: {
          styleObject: {
            color: 'red',
            fontSize: '13px'
          }
		}
    })
</script>
```

**注意：**当用到需要添加css前缀的属性时，vue.js会自动添加。

## 2.7 条件渲染

### 2.7.1 `v-if`

#### 1、基本使用

使用`v-if`进行基本的条件判定。

```html
    <div id="app">
        <p v-if="show">wang is so HandSome</p>
        <p v-else>ho no</p>
    </div>

</head>
<body>
<script type="application/javascript">
    let vm = new Vue({
        el:'#app',
        data:{
            show:null
        }
    })
</script>
```

#### 2、`template`元素

`v-if`是一个指令，所以他必须要添加到元素上，如果想使用不可见的包裹元素，这个时候可以使用`template`标签。

```html
<template v-if="show">
    <p>wang is so HandSome</p>
</template>
```

此例在渲染时，`template`标签将不可见。

#### 3、 `v-else`

> `v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

```html
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

#### 4、 `v-else-if`

> **2.1.0 新增**

此属性的使用方法和`v-else`基本一致，不同的是此属性可以连续使用。

类似于 `v-else`，`v-else-if` 也必须紧跟在带 `v-if` 或者 `v-else-if` 的元素之后。

#### 5、`key`属性

> Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

切换 `loginType` 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 `placeholder`。这样并不符合实际需求。当使用`key`元素的时候，Vue会认为他们是独立的，就不会重新渲染了。

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

### 2.7.2 `v-show`

#### 1、基本使用

```html
<h1 v-show="ok">Hello!</h1>
```

此标签在`Dom`中仅仅是切换了`CSS`的`display`属性。

#### 2、与`v-if`的对比

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

## 2.8 列表渲染

### 2.8.1 `v-for`

#### 1、遍历数组

我们可以用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的**别名**。`in`关键字也可以被`of`取代。

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
<script type="application/javascript">
    var example1 = new Vue({
      el: '#example-1',
      data: {
        items: [
          { message: 'Foo' },
          { message: 'Bar' }
        ]
      }
})
</script>
```

使用索引

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

#### 2、遍历一个对象

遍历一个对象的值

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>

<script type="application/javascript">
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
</script>
```

遍历一个对象的`key`和值

```html
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

添加第三个参数作为索引

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

### 2.8.2 维护索引

当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` attribute：

```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

### 2.8.3 更新检测

#### 1、变更方法

data中的数组是可更新的，你可以使用以下方法对数组中的数据进行更改。

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

#### 2、替换数组

变更方法，顾名思义，会变更调用了这些方法的原始数组。相比之下，也有非变更方法，例如 `filter()`、`concat()` 和 `slice()`。它们不会变更原始数组，而**总是返回一个新数组**。当使用非变更方法时，可以用新数组替换旧数组：

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

## 2.9 事件处理

### 2.9.1 监听事件

可使用`v-on`指令来监听一个事件

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
<script type="application/javascript">
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</script>
```

将`v-on`绑定的方法写在`method`中。

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```

### 2.9.2 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。	

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

> 2.1.4新增

```html
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

> 2.3.0新增

Vue 还对应 [`addEventListener` 中的 `passive` 选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)提供了 `.passive` 修饰符。

```html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

这个 `.passive` 修饰符尤其能够提升移动端的性能。

### 2.9.3 按键修饰符

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

你可以直接将 [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的任意有效按键名转换为 kebab-case 来作为修饰符。

```html
<input v-on:keyup.page-down="onPageDown">
```

在上述示例中，处理函数只会在 `$event.key` 等于 `PageDown` 时被调用。

常用按键别名如下

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

### 2.9.4 系统修饰键

> 2.1.0新增

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

> 请注意修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。如果你想要这样的行为，请为 `ctrl` 换用 `keyCode`：`keyup.17`。

### 2.9.5 [`.exact` 修饰符](https://cn.vuejs.org/v2/guide/events.html#exact-修饰符)

> 2.5.0 新增

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

### 2.9.6 鼠标按钮修饰符

> 2.2.0 新增

- `.left`
- `.right`
- `.middle`

这些修饰符会限制处理函数仅响应特定的鼠标按钮。
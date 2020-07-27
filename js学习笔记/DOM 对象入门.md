# DOM入门

## 什么是DOM

dom就是解析html文档, 将html文档解析成一个document对象. 这个document对象就是DOM

## DOM节点分类

1. 元素节点(element node): 每个元素就是一个元素节点, 比如 p元素节点
2. 属性节点(attribute node): 每个属性也是一个节点, 比如href属性节点
3. 文本节点(text node): 元素中包含的文本, 也是一个节点.

## DOM节点之间的关系

- 父子关系(parent-child): html元素作为父级元素, head和body元素作为子级元素
- 兄弟关系(Sibling): head元素节点和body元素节点互为兄弟关系

## DOM操作的内容

1. 查询元素(进行操作元素节点, 或者元素的属性, 文本)
2. 操作文本
3. 操作属性
4. 操作css样式(一个特殊的属性style)
5. 操作元素


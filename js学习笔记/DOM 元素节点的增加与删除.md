# DOM元素节点的增加与删除

## 创建节点的方法

```js
document.createElement("div");
```

## 加入节点的方法

```js
// 在末尾追加的方法插入节点
parentNode.appendChild(childElement); 

// 在指定节点之前插入节点
parentNode.inertBefore(newNode, beforeNode);
```

## 删除节点的方法

```js
// 删除一个字节点
parentNode.removeChild(childNode);

// 删除一个节点本身
parentNode.remove();
```

## 替换节点的方法

```js
parentNode.replaceChild(newNode, oldNode);
```




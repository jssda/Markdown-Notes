# HashMap 源码分析

## 位运算

### 与运算

按位 都为1, 结果为1

![image-20210317174246411](https://raw.githubusercontent.com/jssda/picbed/master/image-20210317174246411.png)

10 & 5 = 0

### 或运算

只要又一个为1, 结果为1

![image-20210317174406316](https://raw.githubusercontent.com/jssda/picbed/master/image-20210317174406316.png)

10 | 5 = 15

### 异或运算

两个不一样, 结果为1

![image-20210317174511636](https://raw.githubusercontent.com/jssda/picbed/master/image-20210317174511636.png)

10 ^ 5 = 15



## Hash值

也叫散列值

map中, Hash就是一个int值.

存储Hash值的数组, 就叫Hash表.

Hash通过运算可算出数组的index.---Hash函数.



### Hash冲突

如果Hash冲突, 怎么解决

1. 重新Hash
2. 开放地址. 如果当前index已经存放了hash, 那么找index+1, 或者index-1的位置.
3. 溢出区. 如果hash冲突了, 全部去溢出区里找
4. 链地址法. hash表的做法. 

### Hash表中的Hash函数

```java
index = (index.length - 1) & hash
```

hash表中的长度, 要求是2的幂次方. 如果不传, 默认是16.

这个hash函数, 是对长度取余的位运算. 

hash表中扩容, 原长度*2. 位运算则是左移一位.

此哈希函数获取到的角标, 在扩容的时候, 不需要重新计算hash. 新的角标位, 要么在原来的位置, 要么在原来的位置+扩容的大小.

## 扩容

### 扩容时机

#### jdk1.7

判断是否达到阈值 (0.75*数组长度), 并且产生了hash冲突的时候, 会扩容.

扩容之后添加元素.

#### jdk1.8

先添加元素, 判断是否达到阈值. 达到则扩容. 

### 怎么扩容

#### jdk1.7

头插法. 

如果多线程, 则会链表经过多个线程之后, 会形成环形链表.

#### jdk1.8

尾插法

如果对应角标位是单向链表, 则将单向链表进行数据迁移

如果对应角标位是红黑树, 将双向链表进行数据迁移



当链表到达8个的时候, 先转换为双向链表, 然后树化.



扩容逻辑

1. 当此下标位只有一个元素的时候, 计算新下标, 录入.
2. 当此下标位是一个单向node节点时候. hash值&旧数组长度. 如果获取到的值是0, 则下标位不变, 如果获取到的值是1, 则新下标位为原下标位加扩充长度.
3. 如果是一个树节点, 则由双向链表拆分两个双向链表. 当链表长度小于6, 则去树化.

如果多线程, 会丢失数据.



jdk1.8的treeNode可以包含两种接口, 树结构和双向链表接口.


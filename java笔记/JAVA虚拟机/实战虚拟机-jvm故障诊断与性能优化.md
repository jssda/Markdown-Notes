jdk1.7 内存模型

![img](https://raw.githubusercontent.com/jssda/picbed/master/9439016-9989bdf68f246587.png)

jdk1.8 内存模型

![img](https://raw.githubusercontent.com/jssda/picbed/master/20190618112318202.png)

java1.8 中，彻底移除了永久区，使用元数据区来代替永久区。元数据区关联内存的大小，他是根据情况动态改变的。

元空间存储类的元信息，静态变量和常量池等并入堆中。相当于永久代的数据被分到了堆和元空间中。
## HashMap 为什么不是线程安全的

HashMap执行put方法的大致过程为

	1. 获取 key value
 	2. 计算 hash
 	3. 通过 hash & table.length - 1 求余计算下标未知
 	4. 判断下标位置是否为空
 	5. table[index] = entry(key, value, null)

当同时有多个线程调用put方法的时候, 就有可能好几个线程判断下标位置为空, 导致多个线程覆盖数据.

## HashTable 为什么可以线程安全

HashTable和HashMap的区别是, HashTable在每一个方法上都加上了 Synchronized 关键字, 他们都是同步方法. 所以在多个线程访问同一方法的时候, 他们实际上是串行的, 不会产生线程安全问题.

所以说, HashTable 加锁, 那是整个hash表加锁.

## ConcurrentHashMap 怎么加锁的?

由上可知, HashMap 的线程安全问题, 主要就是由于多个线程访问 Hash 表的一个桶时候导致的, 那么对整个 Hash 表加锁固然可以, 但是这样很明显就降低了效率. 如果我们对单一的桶加锁, 那么岂不是可以大大的增加 Hash 表的效率. ConcurrentHashMap 就是这种分段锁.

### CHM 1.7 怎么实现的

CHM1.7 中, 每一个桶不是一个 node 组成的链表了,  而是一个 segment 对象. 每一个对象里边是一个小型的 Hash 表.  当 put 的时候, 会锁定 segment 对象, 这样就实现了分段, 但不是最优的, 最优的当然是每一个桶都加锁.


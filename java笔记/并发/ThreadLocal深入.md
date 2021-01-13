# 一、深入ThreadLocal

## 1.1 使用示例

```java
package pers.jssd.ThreadLocal01;

/**
 * TheadLocal使用
 *
 * @author 
 * @date 2020/8/1 15:00
 */
public class ThreadLocalUse {

    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + SharedVariable.getBean());
                System.out.println(Thread.currentThread().getName() + SharedVariable.getBean());
            }).start();
        }
    }

}

class SharedVariable {

    private static final ThreadLocal<Bean> threadLocal = ThreadLocal.withInitial(Bean::new);

    public static Bean getBean() {
        return threadLocal.get();
    }
}

class Bean {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

运行结果

![image-20210113152542585](https://raw.githubusercontent.com/jssda/picbed/master/image-20210113152542585.png)


## 1.2 简单理解

ThreadLocal是如何在一个线程中使用自己的线程副本呢？既然是线程副本，那肯定要存储在线程中。

当我们使用ThreadLocal存储一个变量的时候，也就是`set`的时候，ThreadLocal会首先取得当前线程`Thread.currentThread`，之后获取当前线程中的一个`ThreadLocalMap`对象，这个对象就是用来存值的。当然，假如我们第一个次`set`，当前线程肯定没有东西啊，那就创建一个ThreadLocalMap，存储到当前线程中。然后把要存储的值存储到当前线程中的ThreadLocalMap中，key为当前使用的ThreadLocal。

当我们获取值的时候，也就是`get`的时候，ThreadLocal也是先获取当前线程，然后从当前线程中获取ThreadLocalMap，值就在ThreadLocalMap中存储着呢，ThreadLocalMap的Key就是当前ThreadLocal。

说白了，使用ThreadLocal存储线程本地变量，也就是存储到了线程中，只是通过ThreadLocal封装了一下。我们自己也可以将一个Map存储到线程中，当然 选择key的时候，也是一个工具类，像ThreadLocal一样，能够通过这个工具类获取到map中存储的值。


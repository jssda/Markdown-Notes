## 附录：Java 虚拟机历史

JDK 版本升级不仅仅体现在语言和功能特性上，还包括了其编译和执行的 Java 虚拟机的升级。

- 1996 年，JDK 1.0 发布时，提供了纯解释执行的 Java 虚拟机实现：Sun Classic VM。
- 1997 年，JDK 1.1 发布时，虚拟机没有做变更，依然使用 Sun Classic VM 作为默认的虚拟机。
- 1998 年，JDK 1.2 发布时，提供了运行在 Solaris 平台的 Exact VM 虚拟机，但此时还是用 Sun Classic VM 作为默认的 Java 虚拟机。
- 2000 年，JDK1.3 发布，默认的 Java 虚拟机由 Sun Classic VM 改为 Sun HotSopt VM，而 Sun Classic VM 则作为备用虚拟机。
- 2002 年，JDK 1.4 发布，Sun Classic VM 退出商用虚拟机舞台，直接使用 Sun HotSpot VM 作为默认虚拟机一直到现在。

## 什么是虚拟机

我们都知道在 Windows 系统上一个软件包装包是 exe 后缀的，而这个软件包在苹果的 Mac OSX 系统上是无法安装的。类似地，Mac OSX 系统上软件安装包则是 dmg 后缀，同样无法在 Windows 系统上安装。

为什么不同系统上的软件无法安装，这是因为操作系统底层的实现是不一样的。对于 Windows 系统来说，exe 后缀的软件代码最终编译成 Windows 系统能识别的机器码。而 Mac OSX 系统来说，dmg 后缀的软件代码最终编译成 Mac OSX 系统能识别的代码。

Java 语言并不直接将代码编译成与系统有关的机器码，而是编译成一种特定的语言规范，这种语言规范我们称之为字节码。无论 Java 程序要在 Windows 系统，还是 Mac OSX 系统，抑或是 Linux 系统，它首先都得编译成字节码文件，之后才能运行。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20181212194650912-14632118.png)

由图可见,  jvm 虚拟机运行的不仅仅是 java 字节码文件, 假如你使用 PHP 写一段代码, 然后用特定的编译程序, 编译成符合 jvm 规范的字节码文件, 那么 jvm 就可以让他跑起来.

所以虽然名字是 Java 虚拟机，但 Java 虚拟机与 Java 语言没有直接关系，它只按照 Java 虚拟机规范去读取 Class 文件，并按照规定去解析、执行字节码指令，仅此而已。



## 从源代码到机器码, 中间发生了什么

编译器可以分为：**前端编译器、JIT 编译器和AOT编译器**

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20181212194436295-283823349.png)

### 前端编译器：源代码到字节码

我们使用的 javac 工具, 就是一个前端编译器, 它是用来将 java 文件, 编译成字节码文件的一个工具。

javac 编译器的处理过程可以分为下面四个阶段：

第一个阶段：词法、语法分析。在这个阶段，JVM 会对源代码的字符进行一次扫描，最终生成一个抽象的语法树。简单地说，在这个阶段 JVM 会搞懂我们的代码到底想要干嘛。就像我们分析一个句子一样，我们会对句子划分主谓宾，弄清楚这个句子要表达的意思一样。

第二个阶段：填充符号表。我们知道类之间是会互相引用的，但在编译阶段，我们无法确定其具体的地址，所以我们会使用一个符号来替代。在这个阶段做的就是类似的事情，即对抽象的类或接口进行符号填充。等到类加载阶段，JVM 会将符号替换成具体的内存地址。

第三个阶段：注解处理。我们知道 Java 是支持注解的，因此在这个阶段会对注解进行分析，根据注解的作用将其还原成具体的指令集。

第四个阶段：分析与字节码生成。到了这个阶段，JVM 便会根据上面几个阶段分析出来的结果，进行字节码的生成，最终输出为 class 文件。

我们一般称 javac 编译器为前端编译器，因为其发生在整个编译的前期。常见的前端编译器有 Sun 的 javac，Eclipse JDT 的增量式编译器（ECJ）。

### JIT 编译器：从字节码到机器码

JIT编译器, 是 Just-In-Time 的缩写。我们叫做及时编译器。

当我们拿到 java 字节码的时候，有两种运行方式， 第一就是解释执行，这种方式比较慢，他是执行的过程中解释字节码成为机器码，然后让机器执行。另一种模式就是编译执行，直接将字节码转换成机器码，那他每次执行的时候，就不需要一次一次的解释了。

两种方式各有优缺点，解释执行启动速度快，但是运行速度相对与编译执行要慢。

在 java 中，默认的执行模式是混合模式，使用`java -version`指令可以看到编译模式是 mixed 模式。在这种模式下，解释器和编译器是同时运作的。

![image-20210105163532059](https://raw.githubusercontent.com/jssda/picbed/master/image-20210105163532059.png)

在 HostSpot 虚拟机中，有个热点代码的概念，就是说如果你的一段代码，被反复调用超过了某个阈值，他就是热点代码。就是说他是频繁使用的代码。在混合模式中，对于这类代码，HostSpot 会用 JIT 编译器将他编译，其他的会使用解释器执行。

而 JIT 编译器中，也存在两个级别的编译，client 级别和 server 级别。我们通常称之为 c1 模式和  c2 模式。c1 模式中，会对编译的代码做简单的优化。c2 模式中，会对编译的代码做大量的优化，以达到运行效率更高的效果。

### AOT 编译器

AOT 编译器的基本思想是：在程序执行前生成 Java 方法的本地代码，以便在程序运行时直接使用本地代码。



## 虚拟机内存结构

这个内存结构官方叫做 运行时数据区

jdk 1.7 中的内存模型和 jdk1.8 中的内存模型略有不同。

### JDK1.7 中的内存结构

![java7内存模型](https://raw.githubusercontent.com/jssda/picbed/master/9439016-9989bdf68f246587.png)

- 共享数据区就是线程共享的数据区。

- 方法区中存放由 JIT 编译之后产生的代码数据、类信息、常量信息、静态变量等。常量池顾名思义就是存放常量的。方法区结合垃圾回收机制来看的话，可以称作为永久代。

- 堆是存放 java 对象的地方。基本所有的 new 出来的对象，都放在堆中（并不是全部，栈上也是可以放对象的，可以通过参数开启这个功能）。  堆结合垃圾回收机制来看的话，堆需要划分为 年轻代和老年代，对象就存在这两个代中。
- 程序计数器是线程私有的，他记录了当前程序运行到的那一行代码的地址。如果程序执行的是本地方法（native），pc计数器的值为 undefined。
- 虚拟机栈是线程私有的，他保存了线程当前的变量引用。栈中有一个 栈帧的概念，一个栈帧就相当于一个方法，栈帧中由局部变量表（存储方法的参数，局部变量），操作数栈（操作的数据临时存储，比如1+1=2，2就存储在操作数栈中），动态链接（不需要过多了解），方法出口信息（返回值啊，产生的异常抛出信息啊之类的）。
- 本地方法栈和虚拟机栈类似，只不过他是存放本地方法的栈。



### JDK 1.8 中的内存结构

![java8中的内存结构](https://raw.githubusercontent.com/jssda/picbed/master/20190618112318202.png)



java8 中， 去除了方法区的概念，改而换用了元空间。元空间中和方法区类似，但是元空间是直接和物理内存关联的，理论上说物理内存多大，元空间就多大，当然这个也是可以配置的。

- 元数据区关联内存的大小，他是根据情况动态改变的。

- 元空间存储类的元信息，静态变量和常量池等并入堆中。相当于方法区的数据被分到了堆和元空间中。
- java8 中去除了方法区，也就没有了永久代的概念。

## java 内存模型 JMM

java的内存模式是 jvm 的内存逻辑规范。

![img](https://raw.githubusercontent.com/jssda/picbed/master/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwOTIxMTgyMzM3OTA0)

JMM 模型，就是图片这样

**JMM决定一个线程对共享变量的写入何时对另一个线程可见**。从抽象的角度来看，JMM定义了线程和主内存之间的抽象关系：**线程之间的共享变量存储在主内存（main memory）中，每个线程都有一个私有的本地内存（local memory），本地内存中存储了该线程以读/写共享变量的副本**。本地内存是JMM的一个抽象概念，并不真实存在。它涵盖了缓存，写缓冲区，寄存器以及其他的硬件和编译器优化。

按照这个图所示，我们提出一个问题，是不是只要共享变量的副本数据实时刷新到主内存，我们就能保证线程安全呢？

其实不是的，JMM 只是一个逻辑模型，我们需要再研究一个物理机的内存架构。

![img](https://raw.githubusercontent.com/jssda/picbed/master/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwOTIxMTgzMDEzNTcw)

cpu 操作内存的时候中间是有缓存的，高速缓存缓冲了 CPU 的高速从主内存中的读取写入操作。

#### 共享对象的可见性

当多个线程同时操作同一个共享对象时，如果没有合理的使用volatile和synchronization关键字，一个线程对共享对象的更新有可能导致其它线程不可见。

想象一下，线程A要操作一个对象，那么他会先把对象加载到线程栈中（线程本地内存），操作这个对象的时候，会通过 CPU 高速缓存，加载到 CPU 进行操作。如果这个时候线程B也做了这个操作，那么A的CPU高速缓存中和B的CPU高速缓存中的值就有可能不一样。

这个问题我们叫做共享对象可见性。

要解决共享对象可见性这个问题，我们可以使用java volatile关键字。 Java’s volatile keyword. volatile 关键字可以保证变量会直接从主存读取，而对变量的更新也会直接写到主存。volatile原理是基于CPU内存屏障指令实现的。

#### 竞争现象

如果多个线程共享一个对象，如果它们同时修改这个共享对象，这就产生了竞争现象。

线程A和线程B共享一个对象obj。假设线程A从主存读取Obj.count变量到自己的CPU缓存，同时，线程B也读取了Obj.count变量到它的CPU缓存，并且这两个线程都对Obj.count做了加1操作。此时，Obj.count加1操作被执行了两次，不过都在不同的CPU缓存中。

如果这两个加1操作是串行执行的，那么Obj.count变量便会在原始值上加2，最终主存中的Obj.count的值会是3。然而下图中两个加1操作是并行的，不管是线程A还是线程B先flush计算结果到主存，最终主存中的Obj.count只会增加1次变成2，尽管一共有两次加1操作。

要解决上面的问题我们可以使用java synchronized代码块。synchronized代码块可以保证同一个时刻只能有一个线程进入代码竞争区，synchronized代码块也能保证代码块中所有变量都将会从主存中读，当线程退出代码块时，对所有变量的更新将会flush到主存，不管这些变量是不是volatile类型的。

## 类加载机制

当编译器将源代码编译成字节码的时候，虚拟机便可以将字节码读取到内存中，从而进行解析运行等过程。这个过程我们叫做 java虚拟机的类加载机制。 JVM 执行 class 字节码的过程可以分为七个阶段：**加载、验证、准备、解析、初始化、使用、卸载。**



### 加载

下面是对于加载过程最为官方的描述。

> 加载阶段是类加载过程的第一个阶段。在这个阶段，JVM 的主要目的是将字节码从各个位置（网络、磁盘等）转化为二进制字节流加载到内存中，接着会为这个类在 JVM 的方法区创建一个对应的 Class 对象，这个 Class 对象就是这个类各种数据的访问入口。

其实加载阶段用一句话来说就是：把代码数据加载到内存中。

### 验证

当 JVM 加载完 Class 字节码文件并在方法区创建对应的 Class 对象之后，JVM 便会启动对该字节码流的校验，只有符合 JVM 字节码规范的文件才能被 JVM 正确执行。这个校验过程大致可以分为下面几个类型：

- **JVM规范校验。**JVM 会对字节流进行文件格式校验，判断其是否符合 JVM 规范，是否能被当前版本的虚拟机处理。例如：文件是否是以 `0x cafe bene`开头，主次版本号是否在当前虚拟机处理范围之内等。
- **代码逻辑校验。**JVM 会对代码组成的数据流和控制流进行校验，确保 JVM 运行该字节码文件后不会出现致命错误。例如一个方法要求传入 int 类型的参数，但是使用它的时候却传入了一个 String 类型的参数。一个方法要求返回 String 类型的结果，但是最后却没有返回结果。代码中引用了一个名为 Apple 的类，但是你实际上却没有定义 Apple 类。

当代码数据被加载到内存中后，虚拟机就会对代码数据进行校验，看看这份代码是不是真的按照JVM规范去写的。



### 准备（重点）

当完成字节码文件的校验之后，JVM 便会开始为类变量分配内存并初始化。这里需要注意两个关键点，即内存分配的对象以及初始化的类型。

- **内存分配的对象。**Java 中的变量有「类变量」和「类成员变量」两种类型，「类变量」指的是被 static 修饰的变量，而其他所有类型的变量都属于「类成员变量」。在准备阶段，JVM 只会为「类变量」分配内存，而不会为「类成员变量」分配内存。「类成员变量」的内存分配需要等到初始化阶段才开始。

  例如下面的代码在准备阶段，只会为 factor 属性分配内存，而不会为 website 属性分配内存。

  ```java
  public static int factor = 3;
  public String website = "www.cnblogs.com/chanshuyi";
  ```

- **初始化的类型。**在准备阶段，JVM 会为类变量分配内存，并为其初始化。但是这里的初始化指的是为变量赋予 Java 语言中该数据类型的零值，而不是用户代码里初始化的值。

  例如下面的代码在准备阶段之后，sector 的值将是 0，而不是 3。

  ```java
  public static int sector = 3;
  ```

- 但如果一个变量是常量（被 static final 修饰）的话，那么在准备阶段，属性便会被赋予用户希望的值。例如下面的代码在准备阶段之后，number 的值将是 3，而不是 0。

  ```java
  public static final int number = 3;
  ```

### 解析

当通过准备阶段之后，JVM 针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符 7 类引用进行解析。这个阶段的主要任务是将其在常量池中的符号引用替换成直接其在内存中的直接引用。

其实这个阶段对于我们来说也是几乎透明的，了解一下就好。



### 初始化（重点）

到了初始化阶段，用户定义的 Java 程序代码才真正开始执行。在这个阶段，JVM 会根据语句执行顺序对类对象进行初始化，一般来说当 JVM 遇到下面 5 种情况的时候会触发初始化：

- 遇到 new、getstatic、putstatic、invokestatic 这四条字节码指令时，如果类没有进行过初始化，则需要先触发其初始化。生成这4条指令的最常见的Java代码场景是：使用new关键字实例化对象的时候、读取或设置一个类的静态字段（被final修饰、已在编译器把结果放入常量池的静态字段除外）的时候，以及调用一个类的静态方法的时候。
- 使用 java.lang.reflect 包的方法对类进行反射调用的时候，如果类没有进行过初始化，则需要先触发其初始化。
- 当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。
- 当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类。
- 当使用 JDK1.7 动态语言支持时，如果一个 java.lang.invoke.MethodHandle实例最后的解析结果 REF_getstatic,REF_putstatic,REF_invokeStatic 的方法句柄，并且这个方法句柄所对应的类没有进行初始化，则需要先出触发其初始化。

看到上面几个条件你可能会晕了，但是不要紧，不需要背，知道一下就好，后面用到的时候回到找一下就可以了。

### 使用

当 JVM 完成初始化阶段之后，JVM 便开始从入口方法开始执行用户的程序代码。这个阶段也只是了解一下就可以。

### 卸载

当用户程序代码执行完毕后，JVM 便开始销毁创建的 Class 对象，最后负责运行的 JVM 也退出内存。这个阶段也只是了解一下就可以。



### 加载顺序实例

#### 例子1

```java
public class Book {
    public static void main(String[] args)
    {
        System.out.println("Hello ShuYi.");
    }

    Book()
    {
        System.out.println("书的构造方法");
        System.out.println("price=" + price +",amount=" + amount);
    }

    {
        System.out.println("书的普通代码块");
    }

    int price = 110;

    static
    {
        System.out.println("书的静态代码块");
    }

    static int amount = 112;
}
```

输出的顺序为：

```
书的静态代码块
Hello ShuYi.
```

简单分析一下，首先根据上面说到的触发初始化的5种情况的第4种（当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类），我们会进行类的初始化。

在我们代码中，我们只知道有一个构造方法，但实际上Java代码编译成字节码之后，是没有构造方法的概念的，只有类初始化方法 和 对象初始化方法 。

那么这两个方法是怎么来的呢？

- 类初始化方法。编译器会按照其出现顺序，收集类变量的赋值语句、静态代码块，最终组成类初始化方法。**类初始化方法一般在类初始化的时候执行。**

上面的这个例子，其类初始化方法就是下面这段代码了：

```java
    static
    {
        System.out.println("书的静态代码块");
    }
    static int amount = 112;
```

- 对象初始化方法。编译器会按照其出现顺序，收集成员变量的赋值语句、普通代码块，最后收集构造函数的代码，最终组成对象初始化方法。**对象初始化方法一般在实例化类对象的时候执行。**

上面这个例子，其对象初始化方法就是下面这段代码了：

```java
    {
        System.out.println("书的普通代码块");
    }
    int price = 110;
    System.out.println("书的构造方法");
    System.out.println("price=" + price +",amount=" + amount);
```

类初始化方法 和 对象初始化方法 之后，我们再来看这个例子，我们就不难得出上面的答案了。

但细心的朋友一定会发现，其实上面的这个例子其实没有执行对象初始化方法。

因为我们确实没有进行 Book 类对象的实例化。如果你在 main 方法中增加 new Book() 语句，你会发现对象的初始化方法执行了！

#### 例子2

```java
class Grandpa
{
    static
    {
        System.out.println("爷爷在静态代码块");
    }
}    
class Father extends Grandpa
{
    static
    {
        System.out.println("爸爸在静态代码块");
    }

    public static int factor = 25;

    public Father()
    {
        System.out.println("我是爸爸~");
    }
}
class Son extends Father
{
    static 
    {
        System.out.println("儿子在静态代码块");
    }

    public Son()
    {
        System.out.println("我是儿子~");
    }
}
public class InitializationDemo
{
    public static void main(String[] args)
    {
        System.out.println("爸爸的岁数:" + Son.factor);	//入口
    }
}
```

输出结果为：

```
爷爷在静态代码块
爸爸在静态代码块
爸爸的岁数:25
```

也许会有人问为什么没有输出「儿子在静态代码块」这个字符串？

**这是因为对于静态字段，只有直接定义这个字段的类才会被初始化（执行静态代码块）。**因此通过其子类来引用父类中定义的静态字段，只会触发父类的初始化而不会触发子类的初始化。

对面上面的这个例子，我们可以从入口开始分析一路分析下去：

- 首先程序到 main 方法这里，使用标准化输出 Son 类中的 factor 类成员变量，但是 Son 类中并没有定义这个类成员变量。于是往父类去找，我们在 Father 类中找到了对应的类成员变量，于是触发了 Father 的初始化。
- 但根据我们上面说到的初始化的 5 种情况中的第 3 种（当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化）。我们需要先初始化 Father 类的父类，也就是先初始化 Grandpa 类再初始化 Father 类。于是我们先初始化 Grandpa 类输出：「爷爷在静态代码块」，再初始化 Father 类输出：「爸爸在静态代码块」。
- 最后，所有父类都初始化完成之后，Son 类才能调用父类的静态变量，从而输出：「爸爸的岁数：25」。



#### 例子3

```java
class Grandpa
{
    static
    {
        System.out.println("爷爷在静态代码块");
    }

    public Grandpa() {
        System.out.println("我是爷爷~");
    }
}
class Father extends Grandpa
{
    static
    {
        System.out.println("爸爸在静态代码块");
    }

    public Father()
    {
        System.out.println("我是爸爸~");
    }
}
class Son extends Father
{
    static 
    {
        System.out.println("儿子在静态代码块");
    }

    public Son()
    {
        System.out.println("我是儿子~");
    }
}
public class InitializationDemo
{
    public static void main(String[] args)
    {
        new Son(); 	//入口
    }
}
```

输出结果为：

```
爷爷在静态代码块
爸爸在静态代码块
儿子在静态代码块
我是爷爷~
我是爸爸~
我是儿子~
```

让我们仔细来分析一下上面代码的执行流程：

- 首先在入口这里我们实例化一个 Son 对象，因此会触发 Son 类的初始化，而 Son 类的初始化又会带动 Father 、Grandpa 类的初始化，从而执行对应类中的静态代码块。因此会输出：「爷爷在静态代码块」、「爸爸在静态代码块」、「儿子在静态代码块」。
- 当 Son 类完成初始化之后，便会调用 Son 类的构造方法，而 Son 类构造方法的调用同样会带动 Father、Grandpa 类构造方法的调用，最后会输出：「我是爷爷」、「我是爸爸」、「我是儿子~」。

#### 例子4

```java
public class Book {
    public static void main(String[] args)
    {
        staticFunction();
    }

    static Book book = new Book();

    static
    {
        System.out.println("书的静态代码块");
    }

    {
        System.out.println("书的普通代码块");
    }

    Book()
    {
        System.out.println("书的构造方法");
        System.out.println("price=" + price +",amount=" + amount);
    }

    public static void staticFunction(){
        System.out.println("书的静态方法");
    }

    int price = 110;
    static int amount = 112;
}
```

输出如下：

```
书的普通代码块
书的构造方法
price=110,amount=0
书的静态代码块
书的静态方法
```

下面我们一步步来分析一下代码的整个执行流程。

在上面两个例子中，因为 main 方法所在类并没有多余的代码，我们都直接忽略了 main 方法所在类的初始化。

但在这个例子中，main 方法所在类有许多代码，我们就并不能直接忽略了。

- 当 JVM 在准备阶段的时候，便会为类变量分配内存和进行初始化。此时，我们的 book 实例变量被初始化为 null，amount 变量被初始化为 0。
- 当进入初始化阶段后，因为 Book 方法是程序的入口，根据我们上面说到的类初始化的五种情况的第四种（当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类）。所以JVM 会初始化 Book 类，即执行类构造器 。
- JVM 对 Book 类进行初始化首先是执行类构造器（按顺序收集类中所有静态代码块和类变量赋值语句就组成了类构造器 ），后执行对象的构造器（按顺序收集成员变量赋值和普通代码块，最后收集对象构造器，最终组成对象构造器 ）。

对于 Book 类，其类构造方法（）可以简单表示如下：

```java
static Book book = new Book();
static
{
    System.out.println("书的静态代码块");
}
static int amount = 112;
```

于是首先执行`static Book book = new Book();`这一条语句，这条语句又触发了类的实例化。于是 JVM 执行对象构造器 ，收集后的对象构造器 代码：

```java
{
    System.out.println("书的普通代码块");
}
int price = 110;
Book()
{
    System.out.println("书的构造方法");
    System.out.println("price=" + price +", amount=" + amount);
}
```

于是此时 price 赋予 110 的值，输出：「书的普通代码块」、「书的构造方法」。而此时 price 为 110 的值，而 amount 的赋值语句并未执行，所以只有在准备阶段赋予的零值，所以之后输出「price=110,amount=0」。

当类实例化完成之后，JVM 继续进行类构造器的初始化：

```java
static Book book = new Book();  //完成类实例化
static
{
    System.out.println("书的静态代码块");
}
static int amount = 112;
```

即输出：「书的静态代码块」，之后对 amount 赋予 112 的值。

- 到这里，类的初始化已经完成，JVM 执行 main 方法的内容。

```java
public static void main(String[] args)
{
    staticFunction();
}
```

即输出：「书的静态方法」。

#### 总结

从上面几个例子可以看出，分析一个类的执行顺序大概可以按照如下步骤：

- **确定类变量的初始值。**在类加载的准备阶段，JVM 会为类变量初始化零值，这时候类变量会有一个初始的零值。如果是被 final 修饰的类变量，则直接会被初始成用户想要的值。
- **初始化入口方法。**当进入类加载的初始化阶段后，JVM 会寻找整个 main 方法入口，从而初始化 main 方法所在的整个类。当需要对一个类进行初始化时，会首先初始化类构造器（），之后初始化对象构造器（）。
- **初始化类构造器。**JVM 会按顺序收集类变量的赋值语句、静态代码块，最终组成类构造器由 JVM 执行。
- **初始化对象构造器。**JVM 会按照收集成员变量的赋值语句、普通代码块，最后收集构造方法，将它们组成对象构造器，最终由 JVM 执行。

如果在初始化 main 方法所在类的时候遇到了其他类的初始化，那么就先加载对应的类，加载完成之后返回。如此反复循环，最终返回 main 方法所在类。

### 类加载器

在类加载机制的加载阶段，我们需要把将字节码加载到内存中，而这个过程是使用类加载器加载的。

#### 类加载器的类型

- 启动类加载器：负责将存放在`<JAVA_HOME>\lib`目录中的，或者被`-Xbootclasspath`参数所指定的路径中的，并且是虚拟机识别的类库加载到虚拟机内存中。启动类加载器无法被Java程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给引导类加载器，那直接使用null代替即可。
- 扩展类加载器：这个加载器由`sun.misc.Launcher$ExtClassLoader`实现，负责加载`<JAVA_HOME>\lib\ext`目录下的，或者被`java.ext.dirs`系统变量所指定的路径中的所有类库，开发者可以直接使用扩展类加载器。rt.jar
- 应用程序类加载器：这个类加载器是由`sun.misc.Launcher$AppClassLoader`实现的。由于这个类加载器是`ClassLoader`中的`getSystemClassLoader`方法的返回值，所以也叫系统类加载器。它负责加载用户类路径上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。
- 自定义类加载器（程序员自己编写的）

#### 双亲委派机制

上图，直接看类加载器的层次结构

![image-20210106160305206](https://raw.githubusercontent.com/jssda/picbed/master/image-20210106160305206.png)

这种自底向上检查是否加载，自顶向下尝试加载的这种模式，就是双亲委派机制。

#### 双亲委派机制的缺陷与突破

// TODO



## 垃圾回收机制

不同的虚拟机有不同的垃圾回收机制的实现，这里主要说明HotSpot虚拟机的垃圾回收机制

### 确定谁是垃圾

 Java 虚拟机判断垃圾对象使用的是：GC Root Tracing 算法。其大概的过程是这样：从 GC Root 出发，所有可达的对象都是存活的对象，而所有不可达的对象都是垃圾。

可以看到这里最重要的就是 GC Root 这个集合了，其实 GC Root 就是一组活跃引用的集合。但是这个集合又与一般的对象集合不太一样，这些集合是经过特意筛选出来的，通常包括：

- 所有当前被加载的 Java 类
- Java 类的引用类型静态变量
- Java 类的运行时常量池里的引用类型常量
- VM的一些静态数据结构里指向GC堆里的对象的引用
- 等等

简单地说，GC Root 就是经过精心挑选的一组活跃引用，这些引用是肯定存活的。那么通过这些引用延伸到的对象，自然也是存活的。

### 三种垃圾回收算法

#### 复制算法

复制算法。复制算法的核心思想是将原有的内存空间分为两块，每次只使用一块，在垃圾回收时，将正在使用的内存中的存活对象复制到未使用的内存块中。之后清除正在使用的内存块中的所有对象，之后交换两个内存块的角色，完成垃圾回收。该算法的缺点是要将内存空间折半，极大地浪费了内存空间。

#### 标记清除法

标记清除算法。从名字可以看到其分为两个阶段：标记阶段和清除阶段。一种可行的实现方式是，在标记阶段，标记所有由 GC Root 触发的可达对象。此时，所有未被标记的对象就是垃圾对象。之后在清除阶段，清除所有未被标记的对象。标记清除算法最大的问题就是空间碎片问题。如果空间碎片过多，则会导致内存空间的不连续。虽说大对象也可以分配在不连续的空间中，但是效率要低于连续的内存空间。

#### 标记压缩法

标记压缩算法。标记压缩算法可以说是标记清除算法的优化版，其同样需要经历两个阶段，分别是：标记结算、压缩阶段。在标记阶段，从 GC Root 引用集合触发去标记所有对象。在压缩阶段，其则是将所有存活的对象压缩在内存的一边，之后清理边界外的所有空间。



对比一下这三种算法，可以发现他们都有各自的优点和缺点。

标记清除算法虽然会产生内存碎片，但是不需要移动太多对象，比较适合在存活对象比较多的情况。而复制算法虽然需要将内存空间折半，并且需要移动存活对象，但是其清理后不会有空间碎片，比较适合存活对象比较少的情况。而标记压缩算法，则是标记清除算法的优化版，减少了空间碎片。



### 分代思想

试想一下，如果我们单独采用任何一种算法，那么最终的垃圾回收效率都不会很好。其实 JVM 虚拟机的建造者们也是这么想的，因此在实际的垃圾回收算法中采用了分代算法。

所谓分代算法，就是根据 JVM 内存的不同内存区域，采用不同的垃圾回收算法。例如对于存活对象少的新生代区域，比较适合采用复制算法。这样只需要复制少量对象，便可完成垃圾回收，并且还不会有内存碎片。而对于老年代这种存活对象多的区域，比较适合采用标记压缩算法或标记清除算法，这样不需要移动太多的内存对象。

试想一下，如果没有采用分代算法，而在老年代中使用复制算法。在极端情况下，老年代对象的存活率可以达到100%，那么我们就需要复制这么多个对象到另外一个内存区域，这个工作量是非常庞大的。

在这里我们再深入地聊一聊新生代里采取的垃圾回收算法。如我们上面所说，新生代的特点是存活对象少，适合采用复制算法。而复制算法的一种最简单实现便是折半内存使用，另一半备用。但实际上我们知道，在实际的 JVM 新生代划分中，却不是采用等分为两块内存的形式。而是分为：Eden 区域、from 区域、to 区域 这三个区域。那么为什么 JVM 最终要采用这种形式，而不用 50% 等分为两个内存块的方式？

要解答这个问题，我们就需要先深入了解新生代对象的特点。根据IBM公司的研究表明，在新生代中的对象 98% 是朝生夕死的，所以并不需要按照1:1的比例来划分内存空间。所以在HotSpot虚拟机中，JVM 将内存划分为一块较大的Eden空间和两块较小的Survivor空间，其大小占比是8:1:1。当回收时，将Eden和Survivor中还存活的对象一次性复制到另外一块Survivor空间上，最后清理掉Eden和刚才用过的Eden空间。

通过这种方式，内存的空间利用率达到了90%，只有10%的空间是浪费掉了。而如果通过均分为两块内存，则其内存利用率只有 50%，两者利用率相差了将近一倍。

### 分区思想

分代思想按照对象的生命周期长短将其分为了两个部分（新生代、老年代），但 JVM 中其实还有一个分区思想，即将整个堆空间划分成连续的不同小区间。

每一个小区间都独立使用，独立回收，这种算法的好处是可以控制一次回收多少个区间，可以较好地控制 GC 时间。



### 垃圾回收器

总的来说，Java 虚拟机的垃圾回收器可以分为四大类别：**串行回收器、并行回收器、CMS 回收器、G1 回收器。**

jdk1.8 默认使用的是 Parallel Scavenge 和 Parallel Old 垃圾收集器。

jdk1.9 之后，默认垃圾收集器为 G1.

#### 串行回收器

串行回收器是指使用单线程进行垃圾回收的回收器。因为每次回收时只有一个线程，因此串行回收器在并发能力较弱的计算机上，其专注性和独占性的特点往往能让其有更好的性能表现。

串行回收器可以在新生代和老年代使用，根据作用于不同的堆空间，分为新生代串行回收器和老年代串行回收器。

**新生代串行回收器**

串行收集器是所有垃圾回收器中最古老的一种，也是 JDK 中最基本的垃圾回收器之一。

在新生代串行回收器中使用的是复制算法。在串行回收器进行垃圾回收时，会触发 Stop-The-World 现象，即其他线程都需要暂停，等待垃圾回收完成。因此在某些情况下，其会造成较为糟糕的用户体验。

使用 `-XX:+UseSerialGC` 参数可以指定使用新生代串行收集器和老年代串行收集器。

**老年代串行回收器**

在老年代串行回收器中使用的是标记压缩算法。其与新生代串行收集器一样，只能串行、独占式地进行垃圾回收，因此也经常会有较长时间的 Stop-The-World 发生。

但老年代串行回收器的好处之一，就是其可以与多种新生代回收器配合使用。若要启用老年代串行回收器，可以尝试以下参数：

- `-XX:UseSerialGC`：新生代、老年代都使用串行回收器。
- `-XX:UseParNewGC`：新生代使用 ParNew 回收器，老年代使用串行回收器。
- `-XX:UseParallelGC`：新生代使用 ParallelGC 回收器，老年代使用串行回收器。

#### 并行回收器

并行回收器在串行回收器的基础上做了改进，其使用多线程进行垃圾回收。对于并行能力强的机器，可以有效缩短垃圾回收所使用的时间。

根据作用内存区域的不同，并行回收器也有三个不同的回收器：新生代 ParNew 回收器、新生代 ParallelGC 回收器、老年代 ParallelGC 回收器。

**新生代 ParNew 回收器**

新生代 ParNew 回收器工作在新生代，其只是简单地将串行回收器多线程化，其回收策略、算法以及参数和新生代串行回收器一样。

新生代 ParNew 回收器同样使用复制的垃圾回收算法，其垃圾收集过程中同样会触发 Stop-The-World 现象。但因为其使用多线程进行垃圾回收，因此在并发能力强的 CPU 上，其产生的停顿时间要短于串行回收器。

但在单 CPU 或并能能力弱的系统中，并行回收器效果会因为线程切换的原因，其实际表现反而不如串行回收器。

要开启新生代 ParNew 回收器，可以使用以下参数：

- `-XX:+UseParNewGC`：新生代使用 ParNew 回收器，老年代使用串行回收器。
- `-XX:UseConcMarkSweepGC`：新生代使用 ParNew 回收器，老年代使用 CMS。
- `-XX:ParallelGCThreads`：指定 ParNew 回收器的工作线程数量。

**新生代 Parallel GC 回收器**

新生代 Parallel GC 回收器与新生代 ParNew 回收器非常类似，其也是使用复制算法，都是多线程、独占式的收集器，也会导致 Stop-The-World。但其余 ParNew 回收器的一个重大不同是：其非常注重系统的吞吐量。

之所以说新生代 Parallel GC 回收器非常注重系统吞吐量，是因为其有一个自适应 GC 调节策略。我们可以使用 `-XX:+UseAdaptiveSizePolicy` 参数打开这个策略，在这个模式下，新生代的大小、Eden 和 Survivor 的比例、晋升老年代的对象年龄等参数都会被自动调节，已达到堆大小、吞吐量、停顿时间的平衡点。

Parallel GC 回收器提供了两个重要参数用于控制系统的吞吐量。

- `-XX:MaxGCPauseMillis`：设置最大垃圾收集停顿时间。在 ParallelGC 工作时，其会自动调整响应参数，将停顿时间控制在设置范围内。为了达到目的，其可能会使用较小的堆，但这会导致 GC 较为频繁。
- `-XX:GCTimeRatio`：设置吞吐量大小，其实一个 0 - 100 的整数。假设 GCTimeRatio 的值为 n，那么系统将不花费超过 1/(1+n) 的时间用于垃圾手机。比如 GCTimeRatio 值为 19，那么系统用于垃圾收集的时间不超过 1 /(1+19) = 5%。默认情况下，它的取值是 99，即不超过 1% 的时间用于垃圾收集。

新生代 Parallel GC 回收器可以使用以下参数启用：

- `-XX:+UseParallelGC`：新生代使用 Parallel 回收器，老年代使用串行回收器。
- `-XX:+UseParallelOldGC`：新生代使用 ParallelGC 回收器，老年代使用 ParallelOldGC 回收器。

**老年代 ParallelOldGC 回收器**

老年代 ParallelOldGC 回收器也是一种多线程并发的回收器，与新生代 ParallelGC 收集器一样，其也是注重吞吐量的收集器，只不过其是作用于老年代。

ParallelOldGC 回收器使用的是标记压缩算法，只有在 JDK 1.6 中才可以使用。我们可以使用`-XX:UseParallelOldGC`参数在新生代中使用 ParallelGC 收集器，在老年代中使用 ParallelOldGC 收集器。参数 `-XX:ParallelGCThreads`也可以用于设置垃圾回收时的线程数量。



#### CMS 回收器

与 ParallelGC 和 ParallelOldGC 不同，CMS 回收器主要关注系统停顿时间。CMS 回收器全称为 Concurrent Mark Sweep，意为标记清除算法，其是一个使用多线程并行回收的垃圾回收器。

**工作步骤**

CMS 的主要工作步骤有：初始标记、并发标记、预清理、重新标记、并发清除和并发充值。其中初始标记和重新标记是独占系统资源的，而其他阶段则可以和用户线程一起执行。

在整个 CMS 回收过程中，默认情况下会有预清理的操作，我们可以关闭开关 `-XX:-CMSPrecleaningEnabled` 不进行预清理。因为重新标记是独占 CPU 的，因此如果新生代 GC 发生之后，立刻出发一次新生代 GC，那么停顿时间就会很长。为了避免这种情况，预处理时会刻意等待一次新生代 GC 的发生，之后在进行预处理。

**主要参数**

启动 CMS 回收器刻意使用参数：`-XX:+UseConcMarkSweepGC`，线程并发数量刻意通过 `-XX:ConcGCThreads` 或 `-XX:ParallelCMSThreads` 参数设定。

此外，我们还可以设置 `-XX:CMSInitiatingOccupancyFraction` 来指定老年代空间使用阈值。当老年代空间使用率达到这个阈值时，会执行一次 CMS 回收，而不像其他回收器一样等到内存不够用的时候才进行 GC。

我们之前说过标记清除算法的缺点是会产生内存碎片，因此 CMS 回收器会产生较多内存碎片。我们可以使用 `XX:+UseCMSCompactAtFullCollection` 参数让 CMS 在完成垃圾回收后，进行一次内存碎片整理。使用 `-XX:CMSFullGCsBeforeCompaction` 参数设置进行多少次 CMS 回收后，进行一次内存压缩。

此外，如果希望使用 CMS 回收 Perm 区，那么则可以打开 `-XX:+CMSClassUnloadingEnabled` 开关。打开该开关后，如果条件允许，那么系统会使用 CMS 的机制回收 Perm 区 Class 数据。



#### G1 回收器

G1 回收器是 JDK 1.7 中使用的全新垃圾回收器，从长期目标来看，其是为了取代 CMS 回收器。

G1 回收器拥有独特的垃圾回收策略，和之前所有垃圾回收器采用的垃圾回收策略不同。从分代看，G1 依然属于分代垃圾回收器。但它最大的改变是使用了分区算法，从而使得 Eden 区、From 区、Survivor 区和老年代等各块内存不必连续。

在 G1 回收器之前，所有的垃圾回收器其内存分配都是连续的一块内存，如下图所示。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20190103110255822-1768341324.png)

而在 G1 回收器中，其将一大块的内存分为许多细小的区块，从而不要求内存是连续的。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20190103110304049-1810895125.png)

从上图可以看到，每个Region被标记了 E、S、O 和 H，说明每个 Region 在运行时都充当了一种角色。所有标记为 E 的都是 Eden 区的内存，它们散落在内存的各个角落，并不要求内存连续。同理，Survivor 区、老年代（Old）也是如此。

从上图我们还可以看到 H 是以往算法中没有的，它代表 Humongous。这表示这些 Region 存储的是巨型对象（humongous object，H-obj），当新建对象大小超过 Region 大小一半时，直接在新的一个或多个连续 Region 中分配，并标记为 H。

堆内存中一个 Region 的大小可以通过 `-XX:G1HeapRegionSize` 参数指定，大小区间只能是1M、2M、4M、8M、16M 和 32M，总之是2的幂次方。如果G1HeapRegionSize 为默认值，即把设置的最小堆内存按照2048份均分，最后得到一个合理的大小。

**工作步骤**

G1 收集器的收集过程主要有四个阶段：

- 新生代 GC
- 并发标记周期
- 混合收集
- 如果需要，可能进行 FullGC

新生代 GC 与其他垃圾收集器的类似，就是清空 Eden 区，将存活对象移动到 Survivor 区，部分年龄到了就移动到老年代。

并发标记周期则分为：初始标记、根区域扫描、并发标记、重新标记、独占清理、并发清理阶段。其中初始标记、重新标记、独占清理是独占式的，会引起停顿。并且初始标记会引发一次新生代 GC。在这个阶段，所有将要被回收的区域会被 G1 记录在一个称之为 Collection Set 的集合中。

混合回收阶段会首先针对 Collection Set 中的内存进行回收，因为这些垃圾比例较高。G1 回收器的名字 Garbage First 就是这个意思，垃圾优先处理的意思。在混合回收的时候，也会执行多次新生代 GC 和 混合 GC，从而来进行内存的回收。

必要时进行 Full GC。当在回收阶段遇到内存不足时，G1 会停止垃圾回收并进行一次 Full GC，从而腾出更多空间进行垃圾回收。

**相关参数**

打开 G1 收集器，我们可以使用参数：`-XX:+UseG1GC。

设置目标最大停顿时间，可以使用参数：`-XX:MaxGCPauseMillis`。

设置 GC 工作线程数量，可以使用参数：`-XX:ParallelGCThreads`。

设置堆使用率触发并发标记周期的执行，可以使用参数：`-XX:InitiatingHeapOccupancyPercent`。



## 调优参数

### 堆栈空间配置

#### 堆配置

我们使用 -Xms 设置堆的初始空间大小，使用 -Xmx 设置堆的最大空间大小。

```shell
java -Xms20m -Xmx30m GCDemo
```

在上面的命令中，我们设置 JVM 的初始堆大小为 20M，最大堆空间为 30M。

#### 年球代

在 JDK1.8 中，堆分为年轻代和老年代。JVM 提供了参数 -Xmn 来设置年轻代内存的大小，但没有提供参数设置老年代的大小。但其实老年代的大小就等于堆大小减去年轻代大小。

```shell
java -Xms20m -Xmn10M GCDemo
```

上面的命令中，我们设置 JVM 堆初始大小为20M。其中年轻代的大小为 10M，那么剩下的就是老年代的大小，有 10M了。 我们可以给上述命令加上`-XX:+PrintGCDetails` 参数来查看内存区域的分配信息。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20181207095438941-1817334689.png)

如上图所示，我们可以看到老年代的大小为 10M。

#### Eden区

在年轻代中，分为三个区域，分别是：eden 空间、from 空间、to 空间。如果要设置这部分的大小，那么就使用 -XX:SurvivorRatio 这个参数，该参数设置 eden / from 空间的比例关系，该参数的公式如下：

```shell
-XX:SurvivorRatio = eden/from = eden/to
```

例如我们的年轻代有 10 M，而我们设置 -XX:SurvivorRatio 参数为 2。也就是说 `eden / from = eden / to = 2`。这里教一个快速计算的方法，我们假设 eden = 2，那么 from = 1，to = 1，那么 eden + from + to = 10M。这样就可以算出每一份大小是 10/4 = 2.5M。所以 Eden 区 = 2.5 * 2 = 5M，from 区是 2.5 M，to 区是 2.5 M。

#### 永久代（JDK1.7）

在 JDK 1.8 之前，所加载的类信息都放在永久代中。我们用 -XX:PermSize 设置永久代初始大小，用 -XX:MaxPermSize 设置永久代最大大小。

```shell
java -XX:PermSize10m -XX:MaxPermSize50m -XX:+PrintGCDetails GCDemo
```

在上面的启动参数中，我们设置永久代初始大小为 10M，最大大小为 50M。我们在 JDK1.7 的环境下运行上面的命令，会看到如下的 GC 日志。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20181207095506791-418389801.png)

在上图中，我们可以看到永久代的大小为我们设置的 10M。

#### 元空间（JDK1.8）

在 JDK 1.8 之前，所有加载的类信息都放在永久代中。但在 JDK1.8 之时，永久代被移除，取而代之的是元空间（Metaspace）。在元空间这块内存中，有两个参数很相似，它们是： -XX:MetaspaceSize 和 -XX:MaxMetaspaceSize。

```shell
java -XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=50m -XX:+PrintGCDetails GCDemo
```

上面的命令中，我们设置 MetaspaceSize 为 10M，MaxMetaspaceSize 为 50M。但其实它们并不是设置初始大小和最大大小的。

![img](https://raw.githubusercontent.com/jssda/picbed/master/595137-20181207095516031-1551698717.png)

从上面的执行结果可以看到，Metaspace 空间的大小为 2.6M 左右，并不是我们设置的 10M。那是因为 MetaspaceSize 设置的是元空间发生 GC 的初始阈值。当达到这个值时，元空间发生 GC 操作，这个值默认是 20.8M。而 MaxMetaspaceSize 则是设置元空间的最大大小，默认基本是机器的物理内存大小。虽然可以不设置，但还是建议设置一下，因为如果一直不断膨胀，那么 JVM 进程可能会被 OS kill 掉。

#### 栈空间

栈空间是每个线程各自有的一块区域，如果栈空间太小，也会导致 StackOverFlow 异常。而要设置栈空间大小，只需要使用 -Xss 参数就可以。

```shell
java -Xss2m GCDemo
```

上面的启动命令设置最大栈空间为 2M。

#### 直接内存

在 JVM 中还有一块内存，它独立于 JVM 的堆内存，它就是：直接内存。我们可以使用 -XX:MaxDirectMemorySize 设置最大直接内存。如果不设置，默认为最大堆空间，即 -Xmx。

```
java -XX:MaxDirectMemorySize=50m GCDemo
```

上面的启动命令设置直接内存最大值为 50M。

当直接内存使用达到设置值时，就会触发垃圾回收。如果不能有效释放足够空间，就会引发直接内存溢出导致系统的 OOM。

#### 总结

|          参数           |                   含义                    |
| :---------------------: | :---------------------------------------: |
|          -Xms           |                初始堆大小                 |
|          -Xmx           |                最大堆空间                 |
|          -Xmn           |              设置新生代大小               |
|    -XX:SurvivorRatio    | 设置新生代eden空间和from/to空间的比例关系 |
|      -XX:PermSize       |              方法区初始大小               |
|     -XX:MaxPermSize     |              方法区最大大小               |
|    -XX:MetaspaceSize    |          元空间GC阈值（JDK1.8）           |
|  -XX:MaxMetaspaceSize   |         最大元空间大小（JDK1.8）          |
|          -Xss           |                  栈大小                   |
| -XX:MaxDirectMemorySize |      直接内存大小，默认为最大堆空间       |

### 查看 JVM 参数

#### 打印显式参数 

```
-XX:+PrintVMOptions
```

该参数表示程序运行时，打印虚拟机接受到的命令行显式参数。我们用下面的命令运行程序：

```
java  -XX:+UseSerialGC -XX:+PrintVMOptions com.chenshuyi.ClassLoadDemo
```

输出结果：

```
VM option '+UseSerialGC'
VM option '+PrintVMOptions'
Hello, I'm chenshuyi
```

可以看到我们设置了`+UseSerialGC`和`+PrintVMOptions`两个参数，最后运行时也将这两个参数打印出来了。

#### 打印显式隐式参数 

```shell
-XX:+PrintCommandLineFlags
```

该参数打印传递给虚拟机的显式和隐式参数。我们用下面的命令运行程序：

```shell
java  -XX:+UseSerialGC -XX:+PrintCommandLineFlags com.chenshuyi.ClassLoadDemo
```

输出结果：

```shell
-XX:InitialHeapSize=134217728 -XX:MaxHeapSize=2147483648 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseSerialGC
Hello, I'm chenshuyi
```

可以看到程序不仅输出了我们显式设置的参数，还将虚拟机默认的参数打印了出来，包括初始堆大小（134217728），最大堆大小（2147483648）等。

#### 打印所有系统参数

```shell
-XX:+PrintFlagsFinal
```

该参数会打印所有的系统参数的值。我们用下面的命令运行程序：

```shell
java  -XX:+UseSerialGC -XX:+PrintFlagsFinal com.chenshuyi.ClassLoadDemo  > jvm_flag_final.txt
```

之后打开 jvm_flag_final.txt 文件，可以看到有 800 多行，这是因为程序将虚拟机的所有参数都打印了出来。下面列几个我们常用的参数看看就可以：

```
...
uintx InitialHeapSize := 134217728 {product}
...
uintx MaxMetaspaceSize = 18446744073709547520 {product}
...
uintx MetaspaceSize = 21807104 {pd product}
```

从上面我们列出的部分参数可以看到，上面三个参数分别是设置初始堆大小、元空间最大大小、初始元空间大小。

最后，让我们来总结一下，加强记忆。

- -XX:+PrintVMOptions 程序运行时，打印虚拟机接受到的命令行显式参数。
- -XX:+PrintCommandLineFlags 打印传递给虚拟机的显式和隐式参数。
- -XX:+PrintFlagsFinal 打印所有的系统参数的值

### 追踪类信息

我们都知道 JVM 在启动的时候会去加载类信息，那么我们怎么得知他加载了哪些类，又卸载了哪些类呢？我们这一节就来介绍四个 JVM 参数，使用它们我们就可以清晰地知道 JVM 的类加载信息。

为了方便演示，我们使用下面的程序作为本次的演示程序。

```java
/**
 * @author chenshuyi
 * @date 2018.09.30
 */
public class ClassLoadDemo {
    public static void main(String[] args) {
        String name = "chenshuyi";
        System.out.println("Hello, I'm " + name);
    }
}
```

#### 跟踪类的加载和卸载

```shell
 -verbose:class
```

我们使用下面的参数运行程序：

```shell
java -verbose:class com.chenshuyi.ClassLoadDemo > class_load_info.txt
```

之后我们打开 class_load_info.txt 文件。因为该文件太大，所以我们节选一部分：

```shell
...省略...
[Loaded java.util.ArrayList from /Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/jre/lib/rt.jar]
...省略...
[Loaded java.util.HashMap from /Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/jre/lib/rt.jar]
...省略...	
[Loaded com.chenshuyi.ClassLoadDemo from file:/Users/yurongchan/Yosemite/Code/practice/target/classes/]
...省略...	
```

从上面可以看到 JVM 分别加载了 ArrayList、HashMap 类，以及我们自己定义的 ClassLoadDemo 类。

#### 跟踪类的加载

```shell
 -XX:+TraceClassLoading
```

该参数与可以显示类的加载信息，输出的结果和 `-verbose:class` 一样，但比起它少了类的卸载信息。

#### 跟踪类的卸载

```shell
 -XX:+TraceClassUnloading
```

该参数与可以显示类的加载信息，输出的结果和 `-verbose:class` 一样，但比起它少了类的加载信息。

但实际上我们通过上面这个简单的例子，没有发现类的卸载信息，或许是例子太简单了，所以没有这个。

#### -XX:+PrintClassHistogram

该参数表示遇到 Ctrl-Break 后打印类实例的柱状信息，与 jmap -histo 功能相同。

了解了这些参数，能够让我们更好地了解哪些类已经被加载到 JVM 中，从而更好地排查问题。

最后来个总结，加强记忆：

|           参数           |        含义        |
| :----------------------: | :----------------: |
|      -verbose:class      | 跟踪类的加载和卸载 |
|  -XX:+TraceClassLoading  |    跟踪类的加载    |
| -XX:+TraceClassUnloading |    跟踪类的卸载    |
| -XX:+PrintClassHistogram |  显示类信息柱状图  |

### GC 日志配置

说到 Java 虚拟机，不得不提的就是 Java 虚拟机的 GC（Garbage Collection）日志。而对于 GC 日志，我们不仅要学会看懂，而且要学会如何设置对应的 GC 日志参数。今天就让我们来学习一下 Java 虚拟机中所有与 GC 日志有关的参数。相信掌握了这些参数之后，对于大家线上打印 GC 日志是有不少帮助的。

为了能够更直观地显示出每个参数的作用，我们将以下面的 Demo 为例子去设置 GC 日志参数。

```java
public class GCDemo {
    public static void main(String[] args) {
        // allocate 4M space
        byte[] b = new byte[4 * 1024 * 1024];
        System.out.println("first allocate");
        // allocate 4M space
        b = new byte[4 * 1024 * 1024];
        System.out.println("second allocate");
    }
}
```

在上面的程序中，我们两次分配了 4M 的内存空间。为了认为制造 GC，我们启动时的 JVM 参数固定加上下面几个参数：

```shell
-XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8
```

- -XX:+UseSerialGC 表示强制使用Serial+SerialOld收集器组合
- -Xms20m 表示堆空间初始大小为 20 M。
- -Xmx20m 表示堆空间最大大小为 20 M。
- -Xmn10m 表示新生代大小为 10M。
- -XX:SurvivorRatio=8 表示Eden:Survivor=8:1

经过上面这个设置，此时我们的堆空间的内存比例情况如下：Eden区 8M，FromSurvivor 1M，ToSurvivor 1M，老年代 10M。

下面就让我们来看看 GC 的参数有哪些吧。

#### 打印GC日志

在 GC 日志参数中，最简单的一个参数就是打印 GC 日志：-XX:PrintGC。我们用下面的命令运行程序：

```shell
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC com.chenshuyi.GCDemo
```

输出结果：

```
first allocate
second allocate
[GC (Allocation Failure)  4767K->4374K(19456K), 0.0045179 secs]
```

可以看到程序在第一次分配数组空间的时候发生了 GC，并且把 GC 前后以及堆空间大小都打印了出来。该日志显示 GC 前堆空间使用量为 4767K（4M左右）。GC 后堆空间为 4374K，当前可用堆大小为 19456K。

但你会发现使用 PrintGC 参数打印出来的日志比较简单，无法查看更详细的信息。如果你要查看更详细的信息，那么就需要下面这个参数。

#### 打印详细GC日志

如果要查看更加详细的 GC 日志，那么就要使用 -XX:+PrintGCDetails 参数。下面我们使用该参数运行程序：

```shell
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC com.chenshuyi.GCDemo
```

程序输出：

```shell
first allocate
second allocate
[GC (Allocation Failure) [DefNew: 4603K->278K(9216K), 0.0036744 secs] 4603K->4374K(19456K), 0.0037100 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
Heap
 def new generation   total 9216K, used 4538K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
  eden space 8192K,  52% used [0x00000007bec00000, 0x00000007bf0290e0, 0x00000007bf400000)
  from space 1024K,  27% used [0x00000007bf500000, 0x00000007bf545920, 0x00000007bf600000)
  to   space 1024K,   0% used [0x00000007bf400000, 0x00000007bf400000, 0x00000007bf500000)
 tenured generation   total 10240K, used 4096K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
   the space 10240K,  40% used [0x00000007bf600000, 0x00000007bfa00010, 0x00000007bfa00200, 0x00000007c0000000)
 Metaspace       used 2649K, capacity 4486K, committed 4864K, reserved 1056768K
  class space    used 286K, capacity 386K, committed 512K, reserved 1048576K
```

从上面的日志可以看出，该参数能打印出更加详细的 GC 信息，包括：年轻代的信息、永久代的信息。

```shell
[GC (Allocation Failure) [DefNew: 4603K->278K(9216K), 0.0036744 secs] 4603K->4374K(19456K), 0.0037100 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
```

该参数还会在退出之前打印出整个堆的详细信息：

```shell
Heap
 def new generation   total 9216K, used 4538K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
  eden space 8192K,  52% used [0x00000007bec00000, 0x00000007bf0290e0, 0x00000007bf400000)
  from space 1024K,  27% used [0x00000007bf500000, 0x00000007bf545920, 0x00000007bf600000)
  to   space 1024K,   0% used [0x00000007bf400000, 0x00000007bf400000, 0x00000007bf500000)
 tenured generation   total 10240K, used 4096K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
   the space 10240K,  40% used [0x00000007bf600000, 0x00000007bfa00010, 0x00000007bfa00200, 0x00000007c0000000)
 Metaspace       used 2649K, capacity 4486K, committed 4864K, reserved 1056768K
  class space    used 286K, capacity 386K, committed 512K, reserved 1048576K
```

#### GC前后打印堆信息

上面两个命令基本上可以应付 90% 的使用场景了，但有时候我们在 GC 前后还想获取更加详细的信息。那么我们可以使用 PrintHeapAtGC 参数，该参数会在 GC 前后打印堆信息。

使用下面的命令运行程序：

```
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintHeapAtGC com.chenshuyi.GCDemo
```

输出结果：

```
first allocate
second allocate
{Heap before GC invocations=0 (full 0):
 def new generation   total 9216K, used 4767K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
  eden space 8192K,  58% used [0x00000007bec00000, 0x00000007bf0a7e98, 0x00000007bf400000)
  from space 1024K,   0% used [0x00000007bf400000, 0x00000007bf400000, 0x00000007bf500000)
  to   space 1024K,   0% used [0x00000007bf500000, 0x00000007bf500000, 0x00000007bf600000)
 tenured generation   total 10240K, used 0K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
   the space 10240K,   0% used [0x00000007bf600000, 0x00000007bf600000, 0x00000007bf600200, 0x00000007c0000000)
 Metaspace       used 2646K, capacity 4486K, committed 4864K, reserved 1056768K
  class space    used 286K, capacity 386K, committed 512K, reserved 1048576K
Heap after GC invocations=1 (full 0):
 def new generation   total 9216K, used 278K [0x00000007bec00000, 0x00000007bf600000, 0x00000007bf600000)
  eden space 8192K,   0% used [0x00000007bec00000, 0x00000007bec00000, 0x00000007bf400000)
  from space 1024K,  27% used [0x00000007bf500000, 0x00000007bf545950, 0x00000007bf600000)
  to   space 1024K,   0% used [0x00000007bf400000, 0x00000007bf400000, 0x00000007bf500000)
 tenured generation   total 10240K, used 4096K [0x00000007bf600000, 0x00000007c0000000, 0x00000007c0000000)
   the space 10240K,  40% used [0x00000007bf600000, 0x00000007bfa00010, 0x00000007bfa00200, 0x00000007c0000000)
 Metaspace       used 2646K, capacity 4486K, committed 4864K, reserved 1056768K
  class space    used 286K, capacity 386K, committed 512K, reserved 1048576K
}
```

仔细看一下，会发现在 GC 发生前后都打印了一次堆空间信息。

通过这个参数，我们可以详细了解每次 GC 时堆空间的详细信息。

#### 打印GC发生的时间

```shell
-XX:+PrintGCTimeStamps
```

这个参数非常简单，就是在每次 GC 日志的前面加上一个时间戳。这个时间戳表示 JVM 启动后到现在所逝去的时间。

使用下面的参数运行程序：

```
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC -XX:+PrintGCTimeStamps com.chenshuyi.GCDemo
```

输出结果：

```
first allocate
second allocate
0.130: [GC (Allocation Failure)  4767K->4374K(19456K), 0.0051351 secs]
```

上面日志第 3 行中的「0.130」就是该 GC 发生的时间。

#### 打印应用程序的执行时间

```
-XX:+PrintGCApplicationConcurrentTime
```

使用下面的命令运行程序：

```
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC -XX:+PrintGCApplicationConcurrentTime com.chenshuyi.GCDemo
```

运行结果：

```
first allocate
second allocate
Application time: 0.0371892 seconds
[GC (Allocation Failure)  4767K->4374K(19456K), 0.0040074 secs]
Application time: 0.0010712 seconds
```

#### 打印应用由于GC而产生的停顿时间

```shell
-XX:+PrintGCApplicationStoppedTime
```

使用下面的命令运行程序：

```
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC -XX:+PrintGCApplicationStoppedTime com.chenshuyi.GCDemo
```

运行结果：

```
first allocate
second allocate
[GC (Allocation Failure)  4767K->4374K(19456K), 0.0045644 secs]
Total time for which application threads were stopped: 0.0047873 seconds, Stopping threads took: 0.0000329 seconds
```

可以看到最后一行打印出了因为 GC 而暂停的时间。

#### 保存GC日志

这个参数可以将 GC 日志输出到文件中保存起来。

使用下面的参数运行程序：

```
java -XX:+UseSerialGC -Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8 -XX:+PrintGC -XX:+PrintReferenceGC -Xloggc:gc.log com.chenshuyi.GCDemo
```

运行之后在本目录会生成一个 gc.log 文件，打开该文件：

```
Java HotSpot(TM) 64-Bit Server VM (25.181-b13) for bsd-amd64 JRE (1.8.0_181-b13), built on Jul  7 2018 01:02:31 by "java_re" with gcc 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00)
Memory: 4k page, physical 8388608k(45132k free)

/proc/meminfo:

CommandLine flags: -XX:InitialHeapSize=20971520 -XX:MaxHeapSize=20971520 -XX:MaxNewSize=10485760 -XX:NewSize=10485760 -XX:+PrintGC -XX:+PrintGCTimeStamps -XX:+PrintReferenceGC -XX:SurvivorRatio=8 -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseSerialGC 
0.124: [GC (Allocation Failure)  4767K->4374K(19456K), 0.0047748 secs]
```

可以看到堆的相关信息，以及 GC 的信息。

#### 总结

除了上面这些参数，还有可以查看弱引用的参数：-XX:+PrintReferenceGC。它跟踪软引用、弱引用、虚引用和Finallize队列的信息，但是使用场景较为狭窄。基本上掌握上面的几个常用的 GC 日志参数就足够排查使用，最重要的是弄清楚每个参数的作用和用法。

最后用列表的形式总结一下，加深一下印象。

|                 参数                  |                       含义                       |
| :-----------------------------------: | :----------------------------------------------: |
|              -XX:PrintGC              |                    打印GC日志                    |
|          -XX:+PrintGCDetails          | 打印详细的GC日志。还会在退出前打印堆的详细信息。 |
|          -XX:+PrintHeapAtGC           |              每次GC前后打印堆信息。              |
|        -XX:+PrintGCTimeStamps         |                打印GC发生的时间。                |
| -XX:+PrintGCApplicationConcurrentTime |              打印应用程序的执行时间              |
|  -XX:+PrintGCApplicationStoppedTime   |          打印应用由于GC而产生的停顿时间          |
|         -XX:+PrintReferenceGC         |   跟踪软引用、弱引用、虚引用和Finallize队列。    |
|                -XLoggc                |             将GC日志以文件形式输出。             |

### JDK 性能监控

#### 查看虚拟机进程：jps

jps 命令可以列出所有的 Java 进程。如果 jps 不加任何参数，可以列出 Java 程序的进程 ID 以及 Main 函数短名称，如下所示。

```
$ jps
6540 Jps
64447 Main
```

除此之外，还可以指定下面的参数自定义输出信息：

| 参数 |            含义            |
| :--: | :------------------------: |
|  -q  |    指定jps只输出进程ID     |
|  -m  |  输出传递给Java进程的参数  |
|  -l  |    输出主函数的完整路径    |
|  -v  | 显示传递给Java虚拟机的参数 |

#### 虚拟机统计信息：jstat 

jstat 用于观察 Java 堆信息的详细情况，其基本使用语法位：

```
jstat -<option> [-t] [-h<lines>] <vmid> [<interval>] [<count>]]
```

其中 option 可以由以下值构成。

|       参数        |                             含义                             |
| :---------------: | :----------------------------------------------------------: |
|      -class       |      监视类装载、卸载数量、总空间以及类装载所耗费的时间      |
|        -gc        | 监视Java堆状况，包括Eden区、两个Survivor区、老年代、永久代等的容量、已用空间、GC时间合计等信息 |
|    -gccapacity    | 监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大、最小空间 |
|      -gcutil      | 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比 |
|     -gccause      |   与-gcutil功能一样，但是会额外输出导致上一次GC产生的原因    |
|      -gcnew       |                       监视新生代GC状况                       |
|  -gcnewcapacity   | 监视内容与-gcnew基本相同，输出主要关注使用到的最大、最小空间 |
|      -gcold       |                       监视老年代GC状况                       |
|  -gcoldcapacity   | 监视内容与-gcold基本相同，输出主要关注使用到的最大、最小空间 |
|  -gcpermcapacity  |               输出永久代使用到的最大、最小空间               |
|     -compiler     |             输出JIT编译器编译过的方法、耗时信息              |
| -printcompilation |                   输出已经被JIT编译的方法                    |

-t 参数表示输出时间戳、-h 参数表示在多少行后输出一个表头、vmid 则是虚拟机的进程ID、interval 和 count 表示输出间隔以及输出次数。

例如：我们用jstat命令来监视一个LVMID为2365的JVM进程。

```
$ jstat -gcutil 2365 
  S0     S1     E      O      P     YGC     YGCT    FGC    FGCT     GCT   
  0.00   0.00  12.05   0.00  14.17      0    0.000     0    0.000    0.000
```

其中每个选项的意义如下：

|   参数    |                             含义                             |
| :-------: | :----------------------------------------------------------: |
|  S0、S1   |             表示Survivor0、Survivor1，还未使用。             |
|     E     |                表示Eden区使用了12.05%的空间。                |
|     O     |                     表示老年代还未使用。                     |
|     P     |                 表示永久代使用了14.17%的空间                 |
| YUC、YGCT | 表示从程序运行以来一共发生了0次Minor GC（YGC，Young GC），总共耗时0秒。 |
| FGC、FGCT | 表示从程序运行以来一共发生了0次Full GC（FGC，Full GC），总共耗时0秒。 |

#### 查看虚拟机参数：jinfo

jinfo 可以用来查看正在运行的 Java 应用程序的扩展参数，甚至支持在运行时，修改部分参数。它的基本语法是：

```
jinfo <option> <pid>
```

执行例子，查询 CMSInitiatingOccupancyFraction 参数值

```
$ jinfo -flag CMSInitiatingOccupancyFraction 2618
-XX:CMSInitiatingOccupancyFraction=-1
```

#### 导出堆到文件：jmap

jmap 是一个多功能命令，可以生成 Java 程序的 Dump 文件，也可以查看堆内对象实例的统计信息、查看 ClassLoader 的信息以及 finalizer 队列。

```
jmap [option] vmid
```

option参数：

| 选项           | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| -dump          | 生成Java堆转储快照。格式为：-dump:[live, ]format=b,file=<filename>其中live子参数说明是否只dump出存活的对象 |
| -finalizerinfo | 显示在F-Queue队列中等待Finalizer线程执行finalize方法的对象   |
| -heap          | 显示Java堆详细信息。如使用哪种垃圾收集器、参数配置、分代状况等 |
| -histo         | 显示堆中对象的统计信息，包括类、实例数量、合计容器           |
| -permstat      | 以ClassLoader为统计口径显示永久代内存状态                    |
| -F             | 当虚拟机进程对-dump没有响应时，可使用此选项强制生成dump快照  |

option参数详解：

-dump
 生成Java堆转储快照。格式为：-dump:[live, ]format=b,file=<filename> live指明是否只dump出存活的对象,format指定输出格式，file指定文件名



执行样例，使用 jmap 生成一个正在运行的 Eclipse 的 dump 快照文件的例子。例子中的2618是通过jps名称查询到的LVMID。

```
$ jmap -dump:format=b,file=Desktop/dump.bin 2618
Dumping heap to /Users/yurongchan/Desktop/dump.bin ...
Heap dump file created
```

#### 堆分析工具：jhat 

jhat 命令用于分析 Java 应用的对快照内存。Sun JDK 提供了 jhat 命令与 jmap 搭配使用，来分析 jmap 生成的堆转储快照。jhat 内置了一个微型的 HTTP/HTML 服务器，生成 dump 文件的分析结果后，可以在浏览器中查看。下面我们用 jhat 来分析上面生成的 dump.bin 文件：

```
$ jhat dump.bin 
Reading from dump.bin...
Dump file created Sun May 15 23:04:19 CST 2016
Snapshot read, resolving...
Resolving 13822 objects...
Chasing references, expect 2 dots..
Eliminating duplicate references..
Snapshot resolved.
Started HTTP server on port 7000
Server is ready.
```

不过一般情况下不用 jhat 命令来分析 dump 文件，主要有以下两个原因：一是一般不会再部署应用的服务器上分析 dump 文件，因为分析工作是一个耗时而且消耗硬件资源的过程。另一个原因是 jhat 的分析功能还比较简陋，比起后面介绍的 VisualVM 等工具还差得很多。

#### 查看线程堆栈：jstack

jstack 命令用于导出 Java 应用程序的线程堆栈。jstack命令格式：

```
jstack [option] vmid
```

下面使用jstack查看一个线程对战的例子：

```
nobody $ jstack -l 2618
2016-05-15 23:39:04
Full thread dump Java HotSpot(TM) 64-Bit Server VM (24.79-b02 mixed mode):
"Attach Listener" daemon prio=5 tid=0x00007f83228e6000 nid=0x280b waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   Locked ownable synchronizers:
    - None
"DestroyJavaVM" prio=5 tid=0x00007f832387e800 nid=0x1303 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   Locked ownable synchronizers:
    - None
```

#### 远程主机信息收集：jstatd

jstad 命令用于收集远程主机信息。

#### 多功能命令行：jcmd

jcmd 命令可以针对给定的 Java 虚拟机执行一条命令。

### JVM 图形化监控工具

Ctrl+R 打开运行, 输入 `Jvisualvm `, 即可启动自带的图形化监控工具。



## 性能调优

减少minor gc的频率、将转移到老年代的对象数量降低到最小以及减少full gc的次数，调优的关键是找到性能的瓶颈

![图片.png](https://raw.githubusercontent.com/jssda/picbed/master/161415e8adf436e4)

### 调优指标

 javaVersion                     /** Java版本号 */
         runTime                         /** 程序运行时间(ms) */
         loadedClassCount               /** JVM当前加载类数量 */
         unloadedClassCount             /** JVM已卸载类数量 */
         heapTotal                       /** 堆内存大小(字节) */
         heapUsed                       /** 堆内存已使用(字节) */
         heapUsedPercent                 /** 堆内存使用率 */
         nonHeapTotal                   /** 堆外内存大小(字节) */
         nonHeapUsed                     /** 堆外内存已使用(字节) */
         nonHeapUsedPercent             /** 堆外内存使用率 */
         edenTotal                       /** Eden区大小(字节) */
         edenUsed                       /** Eden区已使用(字节) */
         edenUsedPercent                 /** Eden区使用率 */
         edenPeakUsedPercent             /** Eden区使用率峰值(从上次采集统计) */
         survivorTotal                   /** Survivor区大小(字节) */
         survivorUsed                   /** Survivor区已使用(字节) */
         survivorUsedPercent             /** Survivor区已使用率 */
         survivorPeakUsedPercent         /** Survivor区已使用率峰值(从上次采集统计) */
         oldTotal                       /** 老区大小(字节) */
         oldUsed                         /** 老区已使用(字节) */
         oldUsedPercent                 /** 老区已使用率峰值 */
         oldPeakUsedPercent             /** 老区已使用率峰值(从上次采集统计) */
         permTotal                       /** 永久区大小(字节) */
         permUsed                       /** 永久区已使用(字节) */
         permUsedPercent                 /** 永久区使用率 */
         permPeakUsedPercent             /** 永久区使用率峰值(从上次采集统计) */
         codeCacheTotal                 /** CodeCache区大小(字节) */
         codeCacheUsed                   /** CodeCache区已使用(字节) */
         codeCacheUsedPercent           /** CodeCache区使用率 */
         codeCachePeakUsedPercent       /** CodeCache区使用率峰值(从上次采集统计) */
         ygcName                         /** young gc名称 */
         ygc                             /** young gc次数 */
         ygcTime                         /** young gc总时间 (ms)*/
         fgcName                         /** full gc名称 */
         fgc                             /** full gc次数 */
         fgcTime                         /** full gc总时间 (ms)*/
         threadCount                     /** JVM当前线程数量 */
         threadPeakCount                 /** JVM线程数量峰值 */
         userThreadCount                 /** JVM当前用户线程数量 */
         deadLockedThreadCount           /** JVM死锁线程数量 */

### 调优手段

```
1.使用JDK提供的内存查看工具，如JConsole和Java VisualVM
2.控制堆内存各个部分所占的比例
3.采用合适的垃圾收集器
```

#### 手段1：内存查看工具和GC日志分析

```
-verbose.gc：显示GC的操作内容。打开它，可以显示最忙和最空闲收集行为发生的时间、收集前后的内存大小、收集需要的时间等。
-xx:+printGCdetails：详细了解GC中的变化。
-XX:+PrintGCTimeStamps：了解这些垃圾收集发生的时间，自JVM启动以后以秒计量。
-xx:+PrintHeapAtGC：了解堆的更详细的信息。
```

#### 手段2：针对新生代和旧生代的比例

```
如果新生代太小，会导致频繁GC，而且大对象对直接进入旧生代引发full gc
如果新生代太大，会诱发旧生代full gc，而且新生代的gc耗时会延长
建议新生代占整个堆1``/3``合适，相关JVM参数如下：
-Xms:初始堆大小
-Xmx:最大堆大小
- Xmn:新生代大小
-XX:PermSize=n:持久代最大值
-XX:MaxPermSize=n:持久代最大值
-XX:NewRatio=n:设置新生代和旧生代的比值。如:为3，表示新生代与旧生代比值为1：3，新生代占整个新生代旧生代和的1``/4
```

#### 手段3：针对Eden和Survivor的比例

```
如果Eden太小，会导致频繁GC
如果Eden太大，会导致大对象直接进入旧生代，降低对象在新生代存活时间
-XX:SurvivorRatio=n:新生代中Eden区与两个Survivor区的比值。注意Survivor区有两个。如：3，表示Eden：Survivor=3：2，一个Survivor区占整个年轻代的1``/5
-XX:PretenureSizeThreshold：直接进入旧生代中的对象大小，设置此值后，大于这个参数的对象将直接在旧生代中进行内存分配。
-XX:MaxTenuringThreshold：对象转移到旧生代中的年龄，每个对象经历过一次新生代GC（Minor GC）后，年龄就加1，到超过设置的值后，对象转移到旧生代。
```

#### 手段4：采用正确的垃圾收集器

```
通过JVM参数设置所使用的垃圾收集器参考前面的介绍，这里关注其他一些设置。``#并行收集器设置-XX:ParallelGCThreads=n:设置并行收集器收集时并行收集线程数
-XX:MaxGCPauseMillis=n:设置并行收集最大暂停时间，仅对ParallelScavenge生效
-XX:GCTimeRatio=n:设置垃圾回收时间占程序运行时间的百分比，仅对Parallel Scavenge生效``#并发收集器设置-XX:CMSInitiatingOccupancyFraction：默认设置下，CMS收集器在旧生代使用了68%的空间后就会被激活。此参数就是设置旧生代空间被使用多少后触发垃圾收集。注意要是CMS运行期间预留的内存无法满足程序需要，就会出现concurrent mode failure，这时候就会启用Serial Old收集器作为备用进行旧生代的垃圾收集。
-XX:+UseCMSCompactAtFullCollection：空间碎片过多是标记-清除算法的弊端，此参数设置在FULL GC后再进行一个碎片整理过程
-XX:CMSFullGCsBeforeCompaction：设置在若干次垃圾收集之后再启动一次内存碎片整理
```

### 原则和步骤：

**我们需要记住下面的原则：**

```
1、多数的Java应用不需要在服务器上进行GC优化；
2、多数导致GC问题的Java应用，都不是因为我们参数设置错误，而是代码问题；
3、在应用上线之前，先考虑将机器的JVM参数设置到最优（最适合）；
4、减少创建对象的数量；
5、减少使用全局变量和大对象；
6、GC优化是到最后不得已才采用的手段；
7、在实际使用中，分析GC情况优化代码比优化GC参数要多得多；
```

#### 进行监控和调优的一般步骤为：

1，监控GC的状态

​      使用各种JVM工具，查看当前日志，分析当前JVM参数设置，并且分析当前堆内存快照和gc日志，根据实际的各区域内存划分和GC执行时间，觉得是否进行优化；

2，分析结果，判断是否需要优化

​        如果各项参数设置合理，系统没有超时日志出现，GC频率不高，GC耗时不高，那么没有必要进行GC优化；如果GC时间超过1-3秒，或者频繁GC，则必须优化；
注：如果满足下面的指标，则一般不需要进行GC：

```
Minor GC执行时间不到50ms；
Minor GC执行不频繁，约10秒一次；
Full GC执行时间不到1s；
Full GC执行频率不算频繁，不低于10分钟1次；
```

3, 调整GC类型和内存分配

​      如果内存分配过大或过小，或者采用的GC收集器比较慢，则应该优先调整这些参数，并且先找1台或几台机器进行beta，然后比较优化过的机器和没有优化的机器的性能对比，并有针对性的做出最后选择；

4，不断的分析和调整

​       通过不断的试验和试错，分析并找到最合适的参数

#### GC分析 命令调优

GC日志分析

摘录GC日志一部分（前部分为年轻代gc回收；后部分为full gc回收）：

```
2016-07-05T10:43:18.093+0800: 25.395: [GC [PSYoungGen: 274931K->10738K(274944K)] 371093K->147186K(450048K), 0.0668480 secs] [Times: user=0.17 sys=0.08, real=0.07 secs] 
2016-07-05T10:43:18.160+0800: 25.462: [Full GC [PSYoungGen: 10738K->0K(274944K)] [ParOldGen: 136447K->140379K(302592K)] 147186K->140379K(577536K) [PSPermGen: 85411K->85376K(171008K)], 0.6763541 secs] [Times: user=1.75 sys=0.02, real=0.68 secs]
```

通过上面日志分析得出，PSYoungGen、ParOldGen、PSPermGen属于Parallel收集器。其中PSYoungGen表示gc回收前后年轻代的内存变化；ParOldGen表示gc回收前后老年代的内存变化；PSPermGen表示gc回收前后永久区的内存变化。young gc 主要是针对年轻代进行内存回收比较频繁，耗时短；full gc 会对整个堆内存进行回城，耗时长，因此一般尽量减少full gc的次数

young gc 日志:

![图片.png](https://raw.githubusercontent.com/jssda/picbed/master/161415e8ae193732)

full gc 日志

![图片.png](https://raw.githubusercontent.com/jssda/picbed/master/161415e8ae088a6d)







# 参考文章

https://www.cnblogs.com/chanshuyi/p/jvm_serial_00_why_learn_jvm.html

https://juejin.cn/post/6844903557108334605
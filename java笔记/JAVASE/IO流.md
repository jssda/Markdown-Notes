# Java IO流

> I  input       O output
>
> IO流就是输入输出流, java中的数据源都是通过IO流来进行操作的
>
> 流是一串动态的抽象的数据集合

## 流的分类

1. 流的方向

   输入流

   ​		InputStream   Reader

   输出流

   ​		OutputStream    Writer

2. 处理数据单元

   字节流 

   ​		InputStream	OutputStream

   字符流

   ​		Reader	Writer

3. 功能不同

   节点流: 数据源和程序直接相连的流

   处理流: 数据源和程序经过了包装, 简化了流的操作

## 流的体系

### 字节流体系

![1561943850512](https://raw.githubusercontent.com/jssda/picbed/master/1561943850512.png)

### 字符流体系

![1561943903610](https://raw.githubusercontent.com/jssda/picbed/master/1561943903610.png)

## 四大基本抽象类

### 字节输入流

InputStream: 从文件中读取字节数据到程序

### 字节输出流

OutputSream: 从程序中输入字节数据到输出端

### 字符输入流

Reader: 从文件中读取字符数据到程序

### 字符输出流

Writer: 从程序中读取字符数据到输出端

## 文件流

### 文件输入流

FileInputStream

```java
FileInputStream fis = null;
try {
    // 初始化文件输入流. 两种初始化方法, 都对
    //fis = new FileInputStream(new File("D:\\IOTest\\source.txt"));
    fis = new FileInputStream("D:/IOTest/source.txt");
    System.out.println("fis.available() = " + fis.available());
    // 读取一个字节
    int read = fis.read();
    System.out.println("read = " + (char) read);
    // 查看剩余可读字节数
    System.out.println("fis.available() = " + fis.available());
    // 全部读出来
    while ((read = fis.read()) != -1) {
        System.out.print((char) read);
    }
} catch (IOException e) {
    e.printStackTrace();
} finally {
    // 最后关闭输入流
    if (fis != null) {
        try {
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

各种读取方法

```java
int read(); // 读取一个字节, 读到末尾返回-1

int read(byte[] b); // 读取数据内容到byte数组中, 没有数据返回-1
byte[] b = new byte[1024];
int length;
while ((length = fis.read(b)) != -1) {
    System.out.println(new String(b, 0, length));
}

// 读取指定长度到数组, 返回读取长度, 没有读取到文件末尾返回-1
int read(byte[] b, int off, int len);
```

### 文件输出流

FileOutputSream

```java
FileOutputStream fos = null;
try {
    // 初始化文件输出流, 第二个参数是是否追加的意思
    fos = new FileOutputStream("D:/IOTest/target.txt", false);
    // 写入文件一个字节
    fos.write(97);
    String str = "I love you baby";
    // 写入文件一个字节数组
    fos.write(str.getBytes());
    // 刷新流到文件
    fos.flush();
} catch (IOException e) {
    e.printStackTrace();
} finally { // 最后不要忘了关闭流
    try {
        if (fos != null) {
            fos.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### 文件字符输入流

```java
// 创建
FileReader reader = new FileReader("D:/IOTest/source.txt");
// 读取
int b = reader.read(); // 读到的是字符的ASCII值
// 输出
System.out.println((char) b);

while((b = reader.read()) != -1){
    System.out.println((char) b);
}

char[] cbuf = new char[1024];
int len = 0;
while(len = reader(cbuf) != -1) {
    System.out.println(new String(cbuf, 0, len));
}
```

### 文件字符输出流

```java
FileWriter fw = null;
try {
    // 初始化文件输出流, 第二个参数是是否追加的意思
    fos = new FileWriter("D:/IOTest/target.txt", false);
    // 写入文件一个字节
    fw.write(97);
    String str = "I love you baby";
    // 写入文件一个字节数组
    fw.write(str.getBytes());
    // 刷新流到文件
    fw.flush(); // 记得一定要刷新, 不刷新的话写不进去
} catch (IOException e) {
    e.printStackTrace();
} finally { // 最后不要忘了关闭流
    try {
        if (fos != null) {
            fw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## 缓冲流

### 缓冲字节流

与其他流使用相似, 不同的是, 将其他流传入缓冲流的构造方法中, 使用如下

```java
FileInputStream fis = null;
FileOutputStream fos = null;
BufferedInputStream bis = null;
BufferedOutputStream bos = null;

try {
    fis = new FileInputStream("D:/IOTest/resource.txt");
    fos = new FileOutputStream("D:/IOTest/target.txt");
    bis = new BufferedInputStream(fis);
    bos = new BufferedOutputStream(fos);

    byte[] buf = new byte[1024];
    int length;
    while ((length = bis.read(buf)) != -1) {
        bos.write(buf, 0, length);
    }
    bos.flush();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    try {
        if (bis != null) {
            bis.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
    try {
        if (bos != null) {
            bos.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### 缓冲字符流

与字节流使用相似, 多了几个方法

```java
BufferedReader br = new BufferedReader(new FileReader("D:/IOTest/resource.txt"));
br.readLine() : String;  // 读取一行, 没读到返回null
BufferedWriter bw = new BufferedWriter(new FileWriter("D:/IOTest/target.txt"));
bw.newLine(); // 写入一个行分隔符
```

## 转换流

InputStreamReader: 将字节数据转换成字符数据

OutputStreamWriter: 将字符数据转换成字节数据

使用如下

从键盘中读取数据, 存储的磁盘

```java
public static void main(String[] args) {
    // 新建输入流
    InputStream is = System.in;
    // 转换成字符数据
    InputStreamReader isr = new InputStreamReader(is, StandardCharsets.UTF_8);
    // 提高效率
    BufferedReader bf = new BufferedReader(isr);
    // 新建输出流
    BufferedWriter bw = null;
    try {
        bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("D:/IOTest/system.txt"), StandardCharsets.UTF_8));
        String line;
        int length = 0;
        while (!"over".equals(line = bf.readLine())) {
            bw.write(line);
            bw.newLine();
            bw.flush();
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (bw != null) {
                bw.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            bf.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

## 打印流

使用打印流, 所有数据都会转换成String类型存储

### 字节打印流

```java
PrintStream ps = System.out;
ps.println("测试一下");
ps.println(20);
```

### 字符打印流

和字节打印流基本一致, 不同的是必须手动关闭流

```java
try (PrintWriter pw = new PrintWriter("D:/IOTest/pw.txt")) {
    pw.println("测试数据");
    pw.println(true);
    pw.flush(); // 必须手动刷新
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

## 数据流

不同于打印流, 数据流写入的数据, 会保存原有数据格式, 是int型数据, 就会保存int型数据

### 数据输出流

```java
private static void writeData() {
    try (DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(new FileOutputStream("D:/IOTest/data.txt")))) {
        dos.writeInt(100);
        dos.writeDouble(3.1415);
        dos.writeBoolean(true);
        dos.writeUTF("这是一些测试数据");
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

数据读入流

```java
private static void readData() {
    try (DataInputStream dis = new DataInputStream(new BufferedInputStream(new FileInputStream("D:/IOTest/data.txt")))){
        System.out.println(dis.readInt());
        System.out.println("dis.readDouble() = " + dis.readDouble());
        System.out.println("dis.readBoolean() = " + dis.readBoolean());
        System.out.println("dis.readUTF() = " + dis.readUTF());
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## 对象流

对象流就是序列化, 序列化要点

- static静态变量不能序列化
- transient 修饰的变量不能序列化
- 序列化的对象的所有成员变量都需要序列化

序列化之后序列化的编号的作用

​	当一个对象被写入到磁盘中时, 修改此对象的类的源码, 比如增加一个属性, 那么, 此时读取磁盘中的对象的时候, 会报告序列化版本错误. 因为磁盘中的对象跟程序中的类不是一个版本.
​	当添加序列号时候, 就可以直接读取, 此时读取磁盘中的对象的时候, 检查的不是对象和类是否一致, 检查的是对象的序列号

### ObjectOutputStream

写对象, 将对象的内存状态写入磁盘, 也叫做序列化

使用方法:

存在一个Person类, 实现了Serializable接口.  Serializable接口只是标识此类可被序列化, 并没有什么抽象方法

```java
public class Person implements Serializable {
	private String name;
	private int age;

	public Person() {
	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
    
	@Override
	public String toString() {
		return "Person{" +
				"name='" + name + '\'' +
				", age=" + age +
				'}';
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
}

```

将Person类的对象persion写入磁盘

```java
ObjectOutputStream oos = null;
try {
    oos = new ObjectOutputStream(new FileOutputStream("D:/IOTest/Object.txt"));
    oos.writeObject(new Person("王京京", 21));
} catch (IOException e) {
    e.printStackTrace();
} finally {
    try {
        if (oos != null) {
            oos.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### ObjectInputStream

读对象, 将对象的内存状态读入内存, 也叫反序列化

将写入的Person类读出来

```java
ObjectInputStream ois = null;
try {
    ois = new ObjectInputStream(new FileInputStream("D:/IOTest/Object.txt"));
    Person person = (Person) ois.readObject();
    System.out.println("person = " + person);
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
} finally {
    try {
        if (ois != null) {
            ois.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## 字节数组流

### ByteArrayOutputStream

将对象存储到字节数组中并保存

```java
private static byte[] writeToByteArr() {
   ByteArrayOutputStream baos = new ByteArrayOutputStream();
   ObjectOutputStream oos = null;
   try {
      oos = new ObjectOutputStream(baos);
      oos.writeInt(100);
      oos.writeObject(new Person("王京京", 21, 1));
   } catch (IOException e) {
      e.printStackTrace();
   } finally {
      try {
         if (oos != null) {
            oos.close();
         }
      } catch (IOException e) {
         e.printStackTrace();
      }
   }
   return baos.toByteArray();
}
```

### ByteArrayInputStream

将对象从字节数组中读出来

```java
private  static  void readFromByteArr(byte[] buf) {
   ByteArrayInputStream bais = null;
   ObjectInputStream ois = null;
   try {
      bais = new ByteArrayInputStream(buf);
      ois = new ObjectInputStream(bais);
      System.out.println("ois.readInt() = " + ois.readInt());
      System.out.println("ois.readObject() = " + ois.readObject());
   } catch (IOException | ClassNotFoundException e) {
      e.printStackTrace();
   } finally {
      try {
         if (ois != null) {
            ois.close();
         }
      } catch (IOException e) {
         e.printStackTrace();
      }
   }
}
```

## 装饰设计模式

> Decorator, 装饰模式, 可用此模式代替继承来扩展类功能, 更加灵活多变

### 结构

![装饰器模式](https://raw.githubusercontent.com/jssda/picbed/master/%E8%A3%85%E9%A5%B0%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

### 代码测试

举个例子

我有一个ICar汽车接口, 只有move方法

```java
package pers.jssd.decorator;

/**
 * 汽车接口
 *
 * @author jssd
 * Create 2019-07-06 14:08
 */
public interface ICar {
	/**
	 * 汽车接口的移动方法
	 */
	void move();
}

```

现在, 我又一个正常的汽车类Car , 实现了ICar接口

```java
package pers.jssd.decorator;

/**
 * @author jssd
 * Create 2019-07-06 14:09
 */
public class Car implements ICar {
	@Override
	public void move() {
		System.out.println("普通汽车的移动, 在陆地上");
	}
}
```

同时, 我也有另一个ICar的实现类, SuperCar, 此类不仅实现了ICar接口, 其内部还有ICar实现类的引用. 每次创建此类的时候, 向此类传递一个ICar, SuperCar的move方法, 执行的就是它持有的引用的move的方法

```java
package pers.jssd.decorator;

/**
 * ICar 的装饰器类
 *
 * @author jssd
 * Create 2019-07-06 14:09
 */
public class SuperCar implements ICar  {
	private ICar car;

	public SuperCar(ICar car) {
		this.car = car;
	}

	@Override
	public void move() {
		car.move();
	}
}
```

现在, 我要为car添加一个飞行的功能, 直接继承SuperCar装饰器类, 添加功能

```java
package pers.jssd.decorator;

/**
 * 添加一个飞行的功能
 * @author jssd
 * Create 2019-07-06 14:11
 */
public class FlyCar extends SuperCar {

   public FlyCar(ICar car) {
      super(car);
   }

   @Override
   public void move() {
      super.move();
      fly();
   }

   // 添加自己的功能
   public void fly() {
      System.out.println("我会飞, 飞高高");
   }
}
```

再添加一个游泳的功能

```java
package pers.jssd.decorator;

/**
 * 添加一个游泳的功能
 * @author jssd
 * Create 2019-07-06 14:14
 */
public class WaterCar extends SuperCar {
   public WaterCar(ICar car) {
      super(car);
   }

   @Override
   public void move() {
      super.move();
      intoWater();
   }

   // 添加自己的功能
   public void intoWater() {
      System.out.println("我会游泳");
   }
}
```

好了, 我现在测试一下
我有一个汽车, 添加了一个飞行的功能, 又添加了一个潜水的功能. 当然, 我还可以继续添加功能, 每次添加的时候继承SuperCar装饰器类, 写上自己的功能, 重写move方法就行了. 实现了功能的分类吧, 解耦了, 这就是优点

```java
package pers.jssd.decorator;

/**
 * 装饰器模式测试类
 *
 * @author jssd
 * Create 2019-07-06 14:08
 */
public class DecoratorTest {
   public static void main(String[] args) {
      ICar car = new WaterCar(new FlyCar(new Car()));
      car.move();
   }
}
```

### 缺点

每次添加一个小功能, 都要创建一个对象, 这样会降低性能

### 和IO流的联系

可以发现, 这个例子中, car的使用,和IO流的使用很相似. IO流中就使用了装饰器模式

再IO流中, 很明显我们发现InputStream, OutputStream, Reader, Writer都是抽象类

FileInputStream, FileOutputStream, FileReader, FileWriter都是具体的构件类

FilterInputStream, FilterOutputStream是装饰器类, 具体功能都继承了这些装饰器类

BufferedInputSream, BufferedOutputStream是具体的功能类

## Apache Commens-io使用

### IOUiles使用

```java
// toString 方法, 以指定字符串直接输出流中的内容
String str = IOUtils.toString(new FileInputStream("D:/IOTest/data.txt"), "UTF-8");
System.out.println(str);

// write方法, 把数据写到流中
String string = "又是一些测试数据";
IOUtils.write(string, new FileOutputStream("D:/IOTest/data1.txt"), "UTF-8");

// copy(inputSteam, OutputSream); 将输入流复制到输出流中
// read(char[], inputStream); 将流中的数据读入到char[]中
```

### FileUtiles的使用

```java
// copyFile 复制文件
FileUtils.copyFile(new File("D:/IOTest/data1.txt"), new File("D:/IOTest/data1copy.txt"));

// copyURLToFile(final URL source, final File destination)  把一个URL拷贝到本地
URL url = new URL("https://www.baidu.com");
FileUtils.copyURLToFile(url, new File("D:/IOTest/baidu.html"));

// copyDirectoryToDirectory  复制目录到另一个目录中

//readFileToString  读文件到String中
```

很多有用的方法自行查看API
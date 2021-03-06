# Java内存模型

## 1.概述

Java内存模型(即Java Memory Model，简称JMM)本身是一种抽象的概念，并不真实存在。

用来定义一个**一致的、跨平台**的内存模型，是缓存一致性协议，用来定义数据读写的规则。

它描述的是一组规则或规范，通过这组规范定义了程序中各个变量（包括实例字段，静态字段和构成数组对象的元素）的访问方式**(为了防止出现数据安全问题)。**



Java 内存模型试图屏蔽各种硬件和操作系统的内存**访问差异**，以实现让 Java 程序在**各种平台下都能达到一致的内存访问效果。**



### 定义

JMM定义了线程和主内存之间的抽象关系：线程之间的**共享变量存储在主内存**（Main Memory）中，每个**线程都有一个私有的本地内存**，本地内存中存储了该线程以读/写共享变量的副本。



###  主内存与工作内存

**由来：**

处理器上的寄存器的读写的速度比内存快几个数量级，为了解决这种速度不匹配，在它们之间加入了**高速缓存**。

加入高速缓存带来了一个新的问题：**缓存数据一致性问题**。如果多个缓存共享同一块主内存区域，那么多个缓存的数据可能会不一致，需要一些协议来解决这个问题。

#### Java中的内存模型：

![img](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCEf6378467ee144281931e9549601ba4ba/705)

所有的变量都存储在主内存中，每个线程还有自己的工作内存 ，工作内存存储在**高速缓存或者寄存器**中，**保存了该线程使用的变量的主内存副本拷贝。**

线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。

![img](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE7ab2f4785b834bb1929befea1658f6b8/706)

- 主内存

主要存储的是Java实例对象，所有线程创建的实例对象都存放在主内存中，不管该实例对象是成员变量还是方法中的本地变量(也称局部变量)，当然也包括了共享的类信息、常量、静态变量。由于是共享数据区域，多条线程对同一个变量进行访问可能会发现线程安全问题。

- 工作内存

主要存储当前方法的所有本地变量信息(工作内存中存储着主内存中的变量副本拷贝)，每个线程只能访问自己的工作内存，即线程中的本地变量对其它线程是不可见的，就算是两个线程执行的是同一段代码，它们也会各自在自己的工作内存中创建属于当前线程的本地变量，当然也包括了字节码行号指示器、相关Native方法的信息。注意由于工作内存是每个线程的私有数据，线程间无法相互访问工作内存，因此存储在工作内存的数据不存在线程安全问题。



在 JDK1.2 之前，Java 的内存模型实现总是从**主存**（即共享内存）读取变量，是不需要进行特别的注意的。而在当前的 Java 内存模型下，线程可以把变量保存**本地内存**（比如机器的寄存器）中，而不是直接在主存中进行读写。这就可能造成一个线程在主存中修改了一个变量的值，而另外一个线程还继续使用它在寄存器中的变量值的拷贝，造成**数据的不一致**。

![JMM(Java内存模型)](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE2be213506f0649f7b38a6652ba09c980/707)

要解决这个问题，就需要把变量声明为 **`volatile`** ，这就指示 JVM，这个变量是**共享且不稳定**的，每次使用它都到主存中进行读取。

所以，**`volatile` 关键字 除了防止 JVM 的指令重排 ，还有一个重要的作用就是保证变量的可见性。**

![volatile关键字的可见性](http://note.youdao.com/yws/public/resource/7d81e6a39024a96dd86efacf29f4ca80/xmlnote/WEBRESOURCE471431935ef948e5be80082c576b5d3d/708)





## 2. 内存模型三个特性

1. **原子性** : 一个的操作或者多次操作，要么所有的操作全部都得到执行并且不会收到任何因素的干扰而中断，要么所有的操作都执行，要么都不执行。`synchronized` 可以保证代码片段的原子性。
2. **可见性** ：当一个变量对共享变量进行了修改，那么另外的线程都是**立即可以看到**修改后的最新值。`volatile` 关键字可以保证**共享变量**的可见性。
3. **有序性** ：在本线程内观察，所有操作都是有序的。在一个线程观察另一个线程，所有操作都是无序的，无序是因为发生了指令重排序。代码在执行的过程中的先后顺序，Java 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序。`volatile` 关键字可以禁止指令进行重排序优化。




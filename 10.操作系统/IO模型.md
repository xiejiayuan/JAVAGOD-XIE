## I/O

**从计算机结构的视角来看的话，**  **I/O 描述了计算机系统与外部设备之间通信的过程。**



为了保证操作系统的稳定性和安全性，一个进程的地址空间划分为 **用户空间（User space）** 和 **内核空间（Kernel space ）** 。

**从应用程序的视角来看的话，我们的应用程序实际上只是发起了 IO 操作的调用而已，具体 IO 的执行是由操作系统的内核来完成的。**

当应用程序发起 I/O 调用后，会经历两个步骤：

1. 内核等待 **I/O 设备准备好数据**
2. 内核将数据从**内核空间拷贝到用户空间。**



## 常见的IO模型

- 同步阻塞IO
- 同步非阻塞IO
- IO多路复用
- 信号驱动IO
- 异步IO



## 阻塞与非阻塞，同步与异步

获取消息的方式是同步/异步的。

线程的状态是阻塞还是非阻塞的。



## IO多路复用的三种机制

让单个进程可以监视多个文件描述符，一旦某个描述符就绪（一般是读就绪或写就绪），能够通知程序进行相应的读写操作

> 就是用更少的资源完成更多的事


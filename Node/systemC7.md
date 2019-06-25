# 第七章 设备(I/O)管理

## 7.1 设备管理概述

​	操作系统的设备管理，又称为I/O管理，它负责管理设备和控制I/O传输操作。

​	计算机系统中使用的设备分为存储设备，I/O设备和通信设备三类。操作系统设计的目标之一是提高系统资源的利用率，第二个目标是方便用户的使用。I/O管理应使用户摆脱具体的，复杂的物理设备特性的束缚，提供方便灵活的，使用外设的手段。

### 7.1.1 设备管理的功能

I/O设备管理应该具有以下功能：

- 状态跟踪

  为了能对设备实施分配和控制，系统必须能快速地跟踪设备状态。

- 设备分配

  系统将设备分配给进程，使用完毕后系统将其及时收回。

- 设备控制

  每个设备都响应带有参数的特定的I/O指令。

### 7.1.2 设备独立性

​	所谓设备独立性是指用户在编制程序时所使用的设备与实际使用的设备无关，也就是在用户程序中仅使用**逻辑设备名。** **逻辑设备名**是用户**自己指定的设备名**，**它是暂时的**，可更改的。物理设备名是系统提供的标准名称，它是永久的，不可更改的。设备管理的任务之一就是把逻辑设备名转换成物理设备名。

​	设备独立性的优点：方便用户；提高设备利用率；提高系统的可适应性和可扩展性。

### 7.1.3 设备控制块

记录设备的硬件特性，连接和使用情况等信息的数据结构称为设备控制块dcb。

## 7.2 缓冲技术

### 7.2.1 缓冲概述

中断技术和通道技术可以缓解CPU和I/O设备间速度不匹配的问题，为了进一步解决这一矛盾还必须引入缓冲技术。

**1.什么是缓冲**

**缓冲**是在**两种不同速度的设备之间传输信息时平滑传输过程的常用手段**。缓冲可以用缓冲器和软件缓冲来实现。由于硬件实现比较贵，大都采用**软件缓冲**。软件缓冲是**指在I/O操作期间用来临时存放I/O数据的一块存储区域**。

**2.利用缓冲技术进行I/O操作**

缓冲的**工作原理**是**在进程请求I/O传输时，利用缓冲区来临时存放I/O传输信息，以缓解传输信息的源设备和目标设备之间速度不匹配的问题**。

**3.缓冲技术解决的问题**

1. 解决速度差异的问题
2. 协调传输数据大小不一致的问题
3. 应用程序的拷贝语义问题

### 7.2.2 常用的缓冲技术

**1.双缓冲**

双缓冲描述了**缓冲管理中最简单的一种方案**。它对于**一个具有低频率活动的I/O系统是比较有效的**。在该方案下，为输入\输出分配两个缓冲区。这两个缓冲区可以用于输入数据，也可用于输出数据；还可既用于输入，又用于输出数据。

**2.缓冲池**

缓冲池由**主存中的一组缓冲区组成**，其中**每个缓冲区的大小一般等于物理记录的大小**。在缓冲池中各个缓冲区作为系统公共资源**为进程所共享**，**并由系统进行统一分配和管理**。缓冲池中的缓冲区既可以用于输出，也可用于输入。

使用缓冲池的主要原因是**避免在消费者多次访问相同数据时会重复产生相同数据的问题**。如当用户程序要多次读相同文件块时，I/O系统不必从磁盘反复读取磁盘块，而是可以采用缓冲池作为高速缓存保留最近访问过的块，准备为将来所用。

## 7.3设备分配

### 7.3.1 设备分配概述

根据设备特性的不同，设备分配有静态分配和动态分配两种方式。设备分配常用的分配算法有先请求先服务和优先最高者优先这两种分配算法。

1.静态分配和动态分配

从设备分配的角度看，外部设备可以分为独占设备和共享设备两类：对独占设备一般采用静态分配；而共享设备则采用动态分配方法。

2.I/O设备分配算法

- 先请求先服务
- 优先级最高者优先

### 7.3.2 独享分配

采用的设备分配技术有独享分配，共享分配和虚拟分配三种。

独享设备是让一个应用程序在整个运行期间独占使用的设备。独占设备采用独享分配方式或称为静态分配方式。即在一个应用程序执行前，分配它所要使用的这类设备；当程序处理完毕时，收回分配给它的这类设备。静态分配方式实现简单，且不会发送死锁，但采用这种分配方式时外部设备利用率不高。

### 7.3.3 共享分配

外部设备中如磁盘等直接存取设备都能进行快速的直接存取。它们往往不是让一个应用程序独占而是被多进程共同使用，或者说，这类设备是共享设备。

### 7.3.4 虚拟分配

利用系统中的便于共享的，快速的存储设备来替代不适合共享的，慢速的字符设备，采用预先收存，延迟发送的方式来改造独占设备。

### 

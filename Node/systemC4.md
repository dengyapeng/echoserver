* [第四章 进程及进程管理](#第四章-进程及进程管理)
      * [1.顺序程序的特点](#1顺序程序的特点)
      * [2.并发程序及特点](#2并发程序及特点)
         * [3.1进程的定义](#31进程的定义)
         * [3.3进程的状态及变迁](#33进程的状态及变迁)
         * [3.3.2进程状态变迁图](#332进程状态变迁图)
         * [3.4 进程控制](#34-进程控制)
         * [3.4.1 进程的创建与撤销](#341-进程的创建与撤销)
         * [3.4.2进程的阻塞和唤醒](#342进程的阻塞和唤醒)
         * [3.5 进程之间的约束关系](#35-进程之间的约束关系)
            * [3.5.1进程竞争与合作](#351进程竞争与合作)
            * [3.5.2进程互斥的概念](#352进程互斥的概念)
            * [3.5.3进程同步的概念](#353进程同步的概念)
      * [5.进程互斥与同步的实现](#5进程互斥与同步的实现)
         * [5.3进程同步的实现](#53进程同步的实现)
         * [5.4生产者-消费者问题](#54生产者-消费者问题)
      * [6.进程通信(IPC Interprocess Communication )](#6进程通信ipc-interprocess-communication-)
         * [6.1进程通信的概念](#61进程通信的概念)
         * [6.2进程通信方式](#62进程通信方式)
      * [7.线程概念及特点](#7线程概念及特点)
         * [7.1线程的概念](#71线程的概念)
            * [进程和线程的区别和联系](#进程和线程的区别和联系)
         * [7.2线程的特点与状态](#72线程的特点与状态)
         * [7.3操作系统的并发机制实例(UNIX)](#73操作系统的并发机制实例unix)
            * [创建进程及应用实例](#创建进程及应用实例)
            * [共享内存及应用实例](#共享内存及应用实例)
      * [8 进程调度](#8-进程调度)
            * [调度方式](#调度方式)
            * [静态优先数和动态优先数](#静态优先数和动态优先数)
            * [循环轮转调度](#循环轮转调度)



# 第四章 进程及进程管理

## 1.顺序程序的特点

- 顺序性；顺序程序在处理机上是**按照程序所规定的顺序执行**的，即每一个操作必须在下一个操作开始之前结束。
- 封闭性；程序一旦执行，其计算结果不受外界因素的影响。
- 可再现性；顺序程序执行的最后输出只与初始条件有关。给定相同的输入条件，程序重复执行一定会得到相同的结果。



## 2.并发程序及特点

###2.1并发程序定义

​	若干个程序同时在系统中运行，这些**程序的执行在时间上是重叠的**，**一个程序**的执行**尚未结束**，**另一个程序**的执行**已经开始**，即使这种重叠是很小的一部分，也称这几个程序是**并发执行**的。

###2.2并发程序的特点

​	程序并发执行的**优点**：**提高了系统的处理能力和计算机的利用率**，但当程序并发执行时，同时也产生了许多的新问题。

**缺点**：

* 失去了程序的封闭性

  如果一**个程序的执行可以改变另一个程序的变量**，那么，后者的输出就可能有**依赖于这两个程序执行的相对速度**，也就是失去了程序的封闭性特点。

* 程序与计算不在一一对应

  **程序**和**计算**是两个不同的概念，前者是**指令的有序集合**，为**静态概念**；后者是**指令序列在处理机上的执行过程**，为**动态概念**。

* 程序并发执行时的相互制约关系

  当并发执行的程序之间需要协同操作来完成一个共同的任务时，它们之间具有直接的相互制约关系，且这样的程序之间有一定的逻辑联系。

##3.进程的概念

### 3.1进程的定义

进程有以下定义：

- 一个正在执行中的程序
- 一个正在计算机上执行的程序实例
- 能分配给处理器并由处理器执行的实体
- **一个程序在给定活动空间和初始环境下，在一个处理机上的执行过程**

国内对程序这一概念做了如下描述：进程是指一个具有一定独立功能的程序关于某个数据集合的一次运行活动。



###3.2进程和程序的联系和区别

区别如下：

- **程序**是指令的有序集合，是个**静态概念**，其本身没有任何运行的含义。而**进程**是程序在处理机上的一次执行过程，是一**动态概念**。**程序**可以作为一种**软件资料长期保存**，而**进程是有一定生命期的**，动态地产生和消亡。
- 进程是一个能够**独立运行的单位**，能与其他进程并行地活动。
- 进程是**竞争**计算机系统**有限资源**的**基本单位**，也是进行**处理机调度**的**基本单位**。

联系：若干不同的进程可以包含相同的程序。这句话的意思是：用同一程序对不同的数据先后或同时加以处理，就对应于多个进程。



### 3.3进程的状态及变迁

###3.3.1 进程的基本状态

在一个进程的活动期间至少具备3种基本状态，即：

- 就绪状态(ready)

  该进程获得了除CPU之外所有的资源，一旦得到CPU的控制权，就可以立即运行。

- 运行状态(running)

  进程得到中央处理器控制权，该进程对应的程序正在处理机上运行。

- 等待状态(wait)

  进程正在等待某一事件发生而暂时停止执行，这时即使给它CPU控制权，也无法执行，又称为阻塞状态。

### 3.3.2进程状态变迁图

![](C:\Users\dell\Desktop\MarkDonw笔记\操作系统笔记\进程状态变迁图.PNG)

###3.4进程控制块

定义：为了描述**一个进程和其他进程以及系统资源的关系**，刻画一个**进程**在各个不同时期**所处的状态**，人们采用了一个与进程相联系的数据块，称为**进程控制块(PCB)**。进程控制块是一个数据结构，是标识进程存在的实体。

|    PCB结构    |
| :-----------: |
|   进程状态    |
| 当前队列指针  |
|  进程优先级   |
| CPU现场保护区 |
|   通信信息    |
|   家族连续    |
| 占有资源清单  |



对表中的各项说明：

- **进程标识符**

  每个进程都有唯一的标识符，在**创建一个进程时，由创建者给出进程的标识符**。另外，为了便于系统管理，进程还有一个**内部标识符(id号)**。

- **进程的状态**

  该项说明**进程当前所处的状态**。

- **当前队列指针**

  该项登记了处于**同一状态的下一个PCB的地址**，以此将处于同一状态的所有进程勾链起来。

- 进程优先级(priority)

  进程的**优先级**反应了进程要求CPU的紧迫程度。

- **CPU现场保护区**

  当进程由于某种原因释放处理机时，**CPU现场信息被保存在PCB的该区域中**，以便在该进程重新获得处理机后继续执行。

- 通信信息

  指每个进程在运行过程中与其他进程进行通信时所记录的有关信息。

- **家族联系**

  有的系统允许一个进程创建自己的子进程，这样，会组成一个进程家族。在PCB中必须指明本进程与家族的联系，如它的子进程和父进程的标识符。

- 占有资源清单

### 3.4 进程控制

​	**进程管理**的功能应包括：**进程控制，进程调度，实现进程之间同步协调和通信**。

​	进程控制负责控制进程状态的变化。操作系统的核心具有创建进程，撤销进程，使进程等待和唤醒等功能。这些功能是由具有特定功能的程序组成的，而且通过**原语操作**来实现控制和管理的目的。原语是一种**特殊的系统调**用，它可以完成一个特定的功能，一般为外层软件所调用，其特点是**原语执行时不可中断**，所以**原语操作具有原子性**，即它是不可再分的。

​	用于进程控制的原语有：**创建原语，撤销原语，阻塞原语，唤醒原语等**。

### 3.4.1 进程的创建与撤销

- 进程创建

  用户不能直接创建进程，而只能通过操作系统提供的进程创建原语，以系统请求方式向操作系统申请创建一个进程。无论是系统或是用户创建进程必须调用创建原语来实现。

- 进程撤销

  进程撤销的功能包括：撤销本进程，撤销一个指定标识符的进程或撤销一组子进程，后面两个撤销命令只能用于父进程撤销子进程。

  它的功能是将当前运行的进程的PCB结构归还到PCB资源池，所占用的资源归还给父进程，然后转进程调度程序。

### 3.4.2进程的阻塞和唤醒

- 进程阻塞

  阻塞命令的功能是停止调用进程的执行，将CPU现场保留到该进程的PCB现场保护区；然后，改变其状态为“等待”，并插入到等待chan的等待队列；最后使控制转向进程调度。

- 进程唤醒

  唤醒命令的功能是当进程等待的事件发生时，唤醒等待该事件的进程，将其从等待队列中移除，插入到就绪队列中。

### 3.5 进程之间的约束关系

#### 3.5.1进程竞争与合作

​	进程之间的相互制约关系可分为两种情况：一种是**由于竞争系统资源**而引起的间接相互制约关系；另一种是**由于进程之间存在共享数据而**引起的直接相互制约关系。

​	进程在以下两种情况下需要协作：

- 信息共享

  多个用户可能对同样的信息感兴趣，所以操作系统必须提供支持，以允许对这些资源类型的并发访问。由于对信息的共享，这些进程是合作进程。

- 并行处理

  如果一个任务在逻辑上可以分为多个子任务，这些子任务可以并发执行以加快该任务的处理速度。由于这些子任务是为了完成一个整体任务而并发执行的，它们之间一定有直接的相互制约关系，这些进程称为合作进程。

  ​	
#### 3.5.2进程互斥的概念

​	同步和互斥的区别和联系：**进程同步**是通过进程之间的通信来实现的，它们之间需要交换信息以便达到协调的目的。进程同步**广义的定义**是指对于**进程操作的时间顺序所加的某种限制**。例如，“**操作A应在操作B之前执**行”。在这些同步规则中有一个**比较特殊的规则**是“**多个操作绝不能在同一时刻执行**”，如“操作A和操作B不能在同一时刻执行”，这种同步规则称为**互斥**。所以，**同步是一个大的范畴，互斥是同步的一个特例，**但二者又是有区别的。

1. 临界资源

   例如进程共享打印机，进程共享公共变量。

   通常把一次仅允许一个进程使用的资源称为临界资源。许多物理设备具有这种性质。

	. 临界区	

   一组进程共享某一临界资源，这组进程中的每一个进程对应的程序中**都包含了一个访问该临界资源的程序段**。在每个进程中，**访问该临界资源的那段程序**能够从概念上分离出来，**称为临界区或临界段**。

   临界区是进程中对公共变量进行访问与修改的程序段，称为相对于该公共变量的临界区。

   临界区概念需要注意的三点：

   - 临界区是针对某一临界资源而言的
   - 相对于某临界资源的临界区个数就是共享该临界资源的进程个数
   - 相对于同一公共变量的若干个临界区，必须是互斥地进入，即一个进程执行完毕且出了临界区，另一个进程才能进入它的临界区。

3. 互斥

   进程互斥可描述为：在操作系统中，**当某一进程正在访问某一存储区域时**，就**不允许其他进程来读出或修改该存储区的内容**，否则，就会发生后果无法估计的错误。**进程之间的这种相互制约关系称为互斥**。



#### 3.5.3进程同步的概念

​	同步概念**强调的是保证进程之间操作的先后次序的约束**，要保证这一点相对而言比较复杂。

1. 什么是同步？

   所谓同步，就是并发进程在一些关键点上可能需要互相等待与互通消息，这种相互制约的等待与互通消息称为进程同步。同步的实质是**使各合作进程的行为保持某种一致性或不变关系。**



##4 .同步机构

 ###4.1锁和上锁，开锁操作

  在锁同步机构中，对应于每一个共享的临界资源都要有一个单独的锁位。常用锁位值为“0”表示资源可用，而用“1”表示资源已被占用。

 ###4.2信号灯和P,V操作

  **信号灯**是一个确定的二元组**(s,q)**，**s是一个具有非负初值的整型变量**，**q是一个初始状态为空的队列**。整型s代表资源的实体或并发进程的状态，操作系统利用信号灯的状态对并发进程和共享资源进行管理。信号灯的值只能通过**P,V操作**原语来改变，由用户程序给出信号灯的初值，其后用户不可更改其值。

  P,V操作：

  1. P操作

     P(s)是一个不可分割的原语操作，即取信号灯值减1，若相减结果小于0，则调用P(s)的进程被阻，并插入到该信号灯的等待队列中，否则可以继续执行。

     P操作的主要动作：**s值减1；若相减结果大于或等于0，则进程继续执行；若相减结果小于0，该进程被封锁，并将它插入到该信号灯的等待队列中，然后转进程调度程序。**

  2. V操作

     V(s)是一个不可分割的原语操作，即取信号灯值加1，若相加结果大于0，进程继续执行，否则，唤醒在信号灯等待队列上的一个进程。

     V操作的主要功能：**s值加1；若相加结果大于0，进程继续执行；若相加结果小于或等于0，则从该信号灯的等待队列中移出一个进程，解除它的等待状态，然后返回本进程继续执行。**

## 5.进程互斥与同步的实现

###5.1上锁原语和开锁原语实现进程互斥

过程：若上锁原语顺利执行，则进程可进入临界区；在完成对临界资源的访问后再执行开锁原语，以释放该临界资源。

###5.2信号灯及p,v操作实现进程互斥

过程：设互斥信号灯，一般记为mutex，赋初值为1，表示初始化时临界资源未被占用；

​	  将进入临界区的操作置于P(mutex)和V(mutex)之间，即可实现进程互斥。

### 5.3进程同步的实现

1.合作进程的执行次序

2.共享缓冲区的合作进程的同步

### 5.4生产者-消费者问题

​	生产者-消费者问题是一类同步问题的抽象描述。当系统中经常使用某一资源时，可以看作是消耗，且将该进程称为消费者。而当某个进程释放资源时，则它就相当于一个生产者。生产者和消费者的**同步关系是禁止生产者向满的缓冲区输送产品，也禁止消费者从空的缓冲区中提取物品。**

​	在生产者-消费者问题中，信号灯具有两种功能。其一，**它是跟踪资源的计数器**；其二，**它是协调生产者和消费者之间的同步器。**

​	为解决这一类问题，应该设置两个同步信号灯：一个表示空缓冲区的数目，用empty表示，其初值为有界缓冲区的大小n；另一个表示满缓冲区的数目，用full表示，其初值为0。由于有界缓冲区是一个临界资源，必须互斥使用，所以，还应设置一个互斥信号灯mutex，其初值为1。

```c
producer()
{
    while(生产未完成)
    {
		生产一个产品；
         p(empty);
         p(mutex);
         insert_item(item);
         v(mutex);
         v(full);
    }
}

consumer()
{
    while(还有继续消费)
    {
        p(full);
        p(mutex);
        item=remove_item();
        v(mutex);
        v(empty);
        消费一个产品
    }
}
```

**mutex初值为1，empty初值为N，full初值为0**。(该缓冲区能放置N+1件产品) 


## 6.进程通信(IPC Interprocess Communication )

### 6.1进程通信的概念

为了实现进程间更有效，无需共享存储器支持的同步，应采用直接的进程通信方式。所谓进程通信是指进程之间可直接以比较高的效率传递较多数据的信息交换方式。

### 6.2进程通信方式

1. 消息缓冲通信
2. 信箱通信



## 7.线程概念及特点

### 7.1线程的概念

为了提高处理机的并行操作能力，提出了多线程的概念。线程是比进程更小的活动单位。

线程可以这样来描述：

- 线程是进程中的一条执行路径
- 它有自己私用的堆栈和处理机执行环境
- 它共享父进程的主存
- 它是单个进程所创建的许多个同时存在的线程中的一个

#### 进程和线程的区别和联系

**进程是任务调度的单位，也是系统资源的分配单位**；而**线程是进程中的一条执行路径**，当系统支持多线程处理时，线程是任务调度的单位，但不是系统资源的分配单位。线程完全继承父进程占用的资源，当它活动时，具有自己的运行现场。



### 7.2线程的特点与状态

1.线程的特点

​	相对于进程而言，线程的创建与管理的开销要小得多；在进程内创建多线程，可以提高系统的并行处理能力。

2.线程的状态变迁

​	和进程类似....

### 7.3操作系统的并发机制实例(UNIX)

#### 创建进程及应用实例

​	 一个进程调用fork（）函数后，系统先给新的进程分配资源，例如存储数据和代码的空间。然后把原来的进程的所有值都复制到新的新进程中，只有少数值与原来的进程的值不同。相当于克隆了一个自己。 

​	在fork函数执行完毕后，如果创建新进程成功，则出现两个进程，**一个是子进程**，**一个是父进程**。在**子进程中**，fork**函数返回0**，在**父进程中**，**fork返回新创建子进程的进程ID**。我们可以通过fork返回的值来判断当前进程是子进程还是父进程。 

#### 共享内存及应用实例

​	共享内存允许两个或更多进程访问同一块内存，就如同malloc()函数向不同进程返回了指向同一个物理内存区域的指针。



## 8 进程调度

进程调度时机可能有以下几种。

1. 进程完成其任务时
2. 在一次系统调用之后，该调用使当前进程暂时不能继续运行时
3. 在采用可剥夺调度方式的系统中，当具有更高优先级的进程要求处理机时。

#### 调度方式

- 非剥夺方式

  当有优先级更高的进程转变为就绪状态时，仍然让正在执行的进程继续执行，直到该进程完成或发生某件事而进入其他状态。

- 可剥夺方式

  当有优先级更高的进程转变为就绪状态时，便暂停正在执行的进程。缺点是系统的开销比采用非剥夺方式要大，因为，可能增加处理机切换的次数。

####进程优先数调度算法

定义：该算法预先确定各进程的优先数，系统将处理机的使用权赋予就绪队列中具备最高优先级的就绪进程。在可抢占情况下，无论何时，执行着的进程的优先级总要比就绪队列中的任何一进程优先级高。

#### 静态优先数和动态优先数

1. 静态优先数

   以静态方式指派给进程的优先级称为静态优先级，它一般在进程被创建时确定，且一经确定后在整个进程运行期间不在改变。

2. 动态优先数

   在创建一个进程时，根据系统资源的使用情况和进程的当前特点确定一个优先数，而在以后的任一时刻进行调整。

#### 循环轮转调度

常见的策略，除了优先调度策略外，还有一种策略是**先来先服务(FIFO)策略**，采用这种算法的问题是，一个进程在放弃对处理机的控制权之前可能执行很长时间，而使**其他进程的推进收到严重的影响**。为此，提出**循环轮转调度算法**：系统规定了一个时间片，每个进程被调度时分得一个时间片，当这**一时间片用完时，该进程转为就绪态并进入就绪队列末端**。

#####1.简单循环轮转调度

​	当CPU空闲时，选取就绪队列首元素，赋予时间片。当该进程时间片用完时，则释放CPU控制权，进入就绪队列的队尾，CPU控制权给下一个处于就绪队列首元素。

​	简单轮转法的优点虽比较简单，但由于采用固定时间片和仅有一个就绪队列，故服务质量是不够理想的。进一步改善轮转法的调度性能是沿着一下两个方向进行的：

- 将固定时间片改为可变时间片，这样可从固定时间片轮转法演变为可变时间片轮转法

- 将单就绪队列改为多就绪队列，从而形成多就绪队列轮转法

  

#####2.多级反馈队列调度 

1.一个优先级一个队列。不同优先级队列时间片大小不同，优先级低的队列时间片反而大。默认IO进程优先级>CPU进程。

2.进程用完时间片，降级进入下一级队列。

3.进程由运行态－>等待态，仍返回原队列。进程优先级被抢占，仍返回原队列。

4.创建一个新进程，就绪后进入第一级队列。

5.上一级队列为空时，CPU才开始调度下一级队列中的进程。

 

 

 

 

 











---
layout: post
title: 操作系统第八章 死锁
category: 学习
---

## 本章导学

### 本章学习目的与要求
理解“死锁”影响系统的可靠性。死锁的产生与进程对资源的需求、进程的执行速度、资源的分配策略有关。系统应采用一定的策略实现资源分配以保证系统的安全。
**`重点是死锁的防止和避免`**

![Alt text](http://7xqzc5.com1.z0.glb.clouddn.com/-1454214764099.png)

<!--more-->

## 第 一 节死锁的形成
1. **死锁概念：**

    若系统中存在一组进程（两个或多个），它们中每个进程都占用了某种资源，又都在等待已被该组进程中的其它进程占用的资源，如果这种等待永远不能结束，则说系统出现了死锁，或者说这组进程处于死锁状态。

2. **死锁的形成：**

    （1） 与进程对资源的需求有关；

    （2） 与进程的执行速度有关；

    （3） 与资源的分配策略有关。

3. **说明：**

    在以下范围内讨论死锁。

    （1）任何一个进程要求资源的最大数量不超过系统能提供的最大资源数量。

    （2）若任何一个进程在执行中所申请的资源能得到满足，那么它一定能在有限时间内执行结束，且归还它所占的金部资源。

    （3）一个进程只有在它所中请的资源得不到满足时，才处于等待资源状态。

## 第 二 节死锁的特征

### 一、死锁的必要条件：（即死锁产生一定会成立的条件）
1. **互斥地使用资源：** 每个资源每次只能给一个进程使用。
2. **占有且等待资源：** 一个进程占有了某些资源后又申请新资源而得不到满足时，处于等待资源的状态，且不释放已占资源。
3. **不可抢夺资源：** 任何一个进程不能抢夺另一个进程所占的资源。
4. **循环等待资源：** 相互等待已被其它进程占用的资源。

**-->必要条件说明：** 以上四个条件仅仅是必要条件而不是充分条件，即只要发生死锁，则这四个条件一定同时成立，如果其中的一个或几个条件不成立，则一定没有死锁。但反之不然，即若这四个条件同时成立，系统未必就有死锁存在。

### 二、资源分配图：（是判断死锁产生与否的方法）
**例：** 现假定某个系统有三类资源 R1 , R2 , R3 ，其中 R1 和 R3 都只有 1 个资源， R2 有 2 个资源，系统中有三个进程 P1 , P2 , P3 ，这些进程占用资源和等待资源的情况如下表所示。

| 进程 | 已占用的资源类 | 已占用的数量 | 等待的资源类 |
| :---:|:-----------:| :---------: |:---------:|
| P1   | R2          |  1          |R1
| P2   | R1,R2       |  1,1        |R3
| P3   | R3          |  1          |


![Alt text](http://7xqzc5.com1.z0.glb.clouddn.com/-1454216236434.png)

由图 1 可知：各进程占用和等待资源的情况没有形成环路，因而不存在循环等待，也就不会有死锁发生。但是，如果进程 P3 又提出申请一个 R2 类资源，那么由于当前 R2 中已没有可分配的资源，因而要增加一条有向边"P3 -->R2" （如图 2 ）。
这样就形成了下列两条环路：

P1 --> R1 --> P2 --> R3 --> P3 --> R2 --> P1

P2 --> R3 --> P3 --> R2 --> P2

这两条环路使进程 P1 , P2 , P3 等待资源的状态永远结束不了，陷入了死锁。

**-->** 结论：

（1）如果资源分配图中无环路，则系统一定没有死锁发生。

（2）如果资源分配图中有环路，且每个资源类中只有一个资源，则环路存在就意味着死锁的形成，环路中的进程就处于死锁状态。

（3）如果资源分配图中有环路，但涉及到的资源类中有多个资源，则环路的存在未必就形成死锁。

## 第 三 节死锁的防止

### 一、互斥条件

要使互斥使用资源的条件不成立，唯一的办法是允许进程共享资源。但部分的硬件设备的物理特性是改变不了的，所以要想破坏”互斥使用资源”这个条件经常是行不通的。

### 二、占有并等待条件
1. **静态分配资源：** 是指进程必须在开始执行前就申请自己所要的全部资源，仅当系统能满足进程的全部资源申请要求且把资源分配给进程后，该进程才开始执行。
2. **释放已占资源：** 仅当进程没有占用资源时，才允许它去申请资源。因此，如果进程已经占用了某些资源而又要再申请资源，那么按此策略的要求，它应先归还所占的资源，归还后才允许申请新资源。（如进程要占用磁带机，又要申请打印机，可以先启动磁带机工作，完成后先释放磁带机再中请打印机。由于申请者是在归还资源后才申请新资源，故不会出现占有了部分资源再等待其它资源的现象。）

### 三、不可抢夺条件
为了使这个条件不成立，我们可以约定如下：如果一个进程已占有了某些资源又要申请新资源 R ，而 R 已被另一进程 P 占用因而必须等待时，则系统可以抢夺进程 P 已占用的资源 R 。具体做法如下：

（1）若进程 A 申请的资源 R 尚未被占用，则系统可把资源 R 分配给进程 A 。

（2）若进程 A 申请的资源 R 已被进程 B 占用，则查看进程B的状态。如果进程B处于等待另一个资源的状态，那么就抢夺进程 B 已占的资源 R ，并把 R 分配给进程 A ，适当的时候再把 R 归还给进程 B 使用；否则让进程 A 处于等待资源 R 的状态。

（3）一个等待资源的进程只有在得到自己所申请的新资源和所有被其它进程抢夺去的老资源后，才能继续执行。

**-->注：** 这种可抢夺的资源分配策略不是对所有资源都适用的。例如，对打印机、磁带机等就不能采用这种抢夺的方式，否则会造成混乱。目前这种分配策略只适用于对处理器和主存资源的分配。

### 四、循环等待条件
对资源采用按序分配的策略可使循环等待资源的条件不成立。按序分配资源是指对系统中所有资源排一个顺序，对每一个资源给出一个确定的编号，规定任何一个进程申请两个以上资源时，总是先申请编号小的资源，再申请编号大的资源。
![Alt text](http://7xqzc5.com1.z0.glb.clouddn.com/-1454219319692.png)

## 第 四 节死锁的避免

### 一、安全状态
1. **安全状态概念：** 如果操作系统能保证所有的进程在有限的时间内得到需要的全部资源，则称系统处于安全状态，否则说系统是不安全的。显然，处于安全状态的系统不会发生死锁，而处于不安全状态的系统可能会发生死锁。

2. **资源分配算法：** 在分配资源时，只要系统能保持处于安全状态，就可避免死锁的发生。故每当有进程提出分配资源的请求时，系统应分析各进程已占资源数、尚需资源数和系统中可以分配的剩余资源数，确定是否处于安全状态。如果分配后系统仍然能维持安全状态，则可为该进程分配资源，否则就暂不为申请者分配资源，直到其它进程归还资源后再考虑它的分配问题。

3. **例子：** 现有 12 个同类资源供 3 个进程共享。进程 P1 总共需要 9 个资源，但第一次先申请 2 个资源。进程 P2 总共需要 10 个资源，第一次要求分配 5 个资源。进程 P3 总共需要 4 个资源，第一次请求 2 个资源。经第一轮的分配后，系统还有 3 个资源未被分配。

    | 进程 | 已占资源数 | 最大需求数 |
    | :---:|:-------:| :-------: |
    | P1   | 2       |  9        |
    | P2   | 5       |  10       |
    | P3   | 2       |  4        |

    这时系统处于安全状态。因为系统还剩余 3 个资源，可把其中的 2 个资源分配给进程 P3 ，这样 P3 就得到了所需的全部资源，能执行到结束。结束后便归还所占的全部 4 个资源。再加上原来剩余的 1 个资源，一共就有 5 个空闲资源，可分配给进程 P2 。同样地， P2 就得到了所需的全部资源，能执行到结束。结束后便归还所占的金部 10 个资源。最后 P1 便能得到尚需的 7 个资源而执行结束。结束后便归还所占的金部 9 个资源。这样三个进程都能在有限时间内得到各自所需的全部资源，执行结束后，系统可收回所有资源。

### 二、银行家算法
1. **银行家算法：**

    （1）当一个用户对资金的最大需求量不超过银行家现有的资金时，就可接纳该用户；

    （2）用户可以分期货款，但货款总数不能超过最大需求量；

    （3）当银行家现有的资金不能满足用户的尚需货款数时，可以推迟支付，但总能使用户在有限的时间里得到货款；

    （4）当用户得到所需的全部资金后，一定能在有限时间里归还所有的资金。

    **-->理解为资源分配算法：** 当进程首次申请资源时，要测试该进程对资源的最大需求量。如果系统现存的资源可以满足它的最大需求量，则按当前的申请量分配资源，否则就推迟分配。当进程在执行中继续申请资源时，先测试该进程已占用的资源数与本次申请的资源数之和是否超过该进程对资源的最大需求量。如果超过，则拒绝分配资源，否则再测试系统现存的资源能否满足该进程尚需的最大资源量。若能满足，则按当前的申请量分配资源，否则也要推迟分配。这样做，能保证在任何时刻至少有一个进程可以得到所需要的全部资源而执行到结束。

2. **例子**

    假定某系统有三类资源A , B , C 和 5 个进程 P1 , P2 , P3，P4，P5，资源类 A 共有 10 个资源，资源类 B 共有 5 个资源，资源类 C 共有 7 个资源，各进程对各类资源的最大申请量和第一次请求分配的资源量（注意， 此时系统尚未为进程分配资源）。如下表所示：
    ![Alt text](http://7xqzc5.com1.z0.glb.clouddn.com/-1454221629008.png)
    如果进程 P1 和 P2 首先提出分配请求，则系统为它们分配后剩余的资源量为 A 类8个，B类 4 个， C 类7 个。如接着是进程 P3 提出分配请求，则因这时 A 类的资源只剩 8 个，不能满足 P3 对 A 类的资源的最大需求量，故系统对 P3 的分配需推迟。但是对 P4 和 P5 向系统提出的请求可以接受，并为它们分配资源。

## 第 五 节死锁的检测

### 一、死锁的检测方法
1. **每类资源中只有一个资源：**
如果每类资源中只有一个资源，则可以设置两张表格来记录进程使用和等待资源的情况。一张为占用表，记录进程占用资源的情况。另一张为等待表，记录进程正在等待资源的情况。任一进程申请资源时，若该资源空闲，则把该资源分配给申请者，且在占用表中登记，否则把申请者登入等待表中。死锁检测程序定时地检测这两张表。如果发观有循环等待资源的进程，则表明有死锁出现。
例子：P1 **-->** R3 **-->** P2 **-->** R2 **-->** P3 **-->** R5 **-->** P1。

    （a）占用表

    | 资源 | 占用进程 |
    | :---:|:------:|
    | R1   | P1     |
    | R2   | P3     |
    | R3   | P2     |
    | R4   | P2     |
    | R5   | P1     |

    （b）等待表

    | 进程 | 等待资源 |
    | :---:|:------:|
    | P1   | R3     |
    | P2   | R2     |
    | P3   | R5     |

2. **资源类中含有若干个资源：**

    （1）初始检测：找出资源已经满足的进程，即不再申请资源的进程。若有这样的进程，则它们一定能在有限的时间内执行结束，且归还所占的资源。所以可以把它们所占的资源与系统中还剩余的资源加在一起作为可分配的资源，同时对这些进程置上标志。

    （2）循环检测：检测所有无标志的进程，找出一个尚需资源量不超过系统中的可分配资源量的进程。若能找到，则只要把资源分配给该进程，就一定能在有限时间内收回它所占的全部资源。故可把该进程已占的资源添加到可分配的资源中，同时为该进程置上一个标志。重复执行第二步，直到所有进程均有标志，或无标志的进程尚需资源量均超过可分配的资源量。

    （3）结束检测：若进程均有标志，表示当前不存在永远等待资源的进程，也即系统不处于死锁状态。若存在无标志的进程，表示系统当前已有死锁形成，这些无标志的进程就是一组处于死锁状态的进程。检测结束，解除检测时所设置的所有标志。

### 二、死锁的解除
1. **终止进程**

    （1）终止涉及死锁的所有进程

    （2）一次终止一个进程
2. **抢夺资源**

    从涉及死锁的一个或几个进程中抢夺资源，把夺得的资源再分配给卷入死锁的其他进程，直到死锁解除。应注意三个问题：

    （1）抢夺哪些进程的哪些资源

    （2）被抢夺者的恢复

    （3）进程的"饿死"  

    **-->**"死锁"和"饿死"的区别：死锁是指一组进程处于循环等待资源状态且永远不能结束等待。而饿死是指一个进程长期得不到资源而无法继续执行，它并没有卷入循环等待资源状态。

## 本章小结

1. **死锁的形成：与进程对资源的需求、进程的执行速度、资源的分配策略有关。**
2. **死锁的特征：互斥地使用资源、占有且等待资源、不可抢夺资源、循环等待资源。**
3. **死锁的防止：通过破坏死锁的四个必要条件之一。**
4. **死锁的避免：让系统处于安全状态。**
5. **死锁的检测 ：允许死锁发生，发生死锁后再解除死锁。**
**`重点是：死锁的防止和避免。（即第 3、4 节）`**

---
typora-root-url: ../study
typora-copy-images-to: ./memory_img
---

Go语言内存模型

参考OS虚拟内存 物理内存映射算法

跟Java不同，不用挪对象（Java会挪对象，由年轻代挪到老年代），这是Go的优点

什么时候开始GC？



# Go内存

## 简介

Go运行时的内存分配算法主要源自 Google 为 C 语言开发的<font color=red>TCMalloc算法</font>，全称<font color=red>Thread Caching Malloc</font>。核心思想就是把内存分为多级管理，从而降低锁的粒度。它将可用的堆内存采用二级分配的方式进行管理：每个线程都会自行维护一个独立的内存池，进行内存分配时优先从该内存池中分配，当内存池不足时才会向全局内存池申请，以避免不同线程对全局内存池的频繁竞争。所以Go在并发具有天生的优势。



go语言运行所需要的基础设施

1.协程调度，内存分配，GC

2.操作系统及CPU相关操作的封装（信号处理，系统调用，寄存器操作，原子操作等），CGO

3.pprof，trace，race检测的支持

4.map，channel，string等内置类型及反射的实现

![image-20210321180457491](/memory_img/image-20210321180457491.png)

1.与Java，Python不同，Go并没有虚拟机的概念，Runtime也直接被编译成native code

2.Go的Runtime与用户代码一起打包在一个可执行文件中

3.用户代码与Runtime代码在执行的时候并没有明显的界限，都是函数调用

4.Go对系统调用的指令进行了封装，可不依赖于glibc

5.一些Go的关键字被编译成Runtime包下的函数

![image-20210321190242215](/memory_img/image-20210321190242215.png)

## Runtime发展历程

![image-20210321190659317](/memory_img/image-20210321190659317.png)

## 内存管理组件

内存分配由内存分配器完成。分配器由3种组件构成：<font color=red>`mcach`， `mcentral`, `mheap`</font>。

![image-20210321215114648](/memory_img/image-20210321215114648.png)

### mcache



### mcentral



### mheap

## 分配流程

变量是在栈上分配还是在堆上分配，是由逃逸分析的结果决定的。通常情况下，编译器是倾向于将变量分配到栈上的，因为它的开销小，最极端的就是"zero garbage"，所有的变量都会在栈上分配，这样就不会存在内存碎片，垃圾回收之类的东西。

Go的内存分配器在分配对象时，根据对象的大小，分成三类：小对象（小于等于16B）、一般对象（大于16B，小于等于32KB）、大对象（大于32KB）。

大体上的分配流程：

- 32KB 的对象，直接从mheap上分配；
- <=16B 的对象使用mcache的tiny分配器分配；
- (16B,32KB] 的对象，首先计算对象的规格大小，然后使用mcache中相应规格大小的mspan分配；
- 如果mcache没有相应规格大小的mspan，则向mcentral申请；
- 如果mcentral没有相应规格大小的mspan，则向mheap申请；
- 如果mheap中也没有合适大小的mspan，则向操作系统申请；

## 总结

1.Go在程序启动时，会向操作系统申请一大块内存，之后自行管理。

2.Go内存管理的基本单元是mspan，它由若干个页组成，每种mspan可以分配特定大小的object。

3.mcache, mcentral, mheap是Go内存管理的三大组件，层层递进。mcache管理线程在本地缓存的mspan；mcentral管理全局的mspan供所有线程使用；mheap管理Go的所有动态分配内存。

4.极小对象会分配在一个object中，以节省资源，使用tiny分配器分配内存；一般小对象通过mspan分配内存；大对象则直接由mheap分配内存。

# Java内存

## 运行时数据区域

![image-20210321221249431](/memory_img/image-20210321221249431.png)

线程共享的：<font color=red>堆、方法区、直接内存</font>

线程私有的：<font color=red>程序计数器、虚拟机栈、本地方法栈</font>

## Java对象创建过程

![image-20210321222133194](/memory_img/image-20210321222133194.png)

## 内存分配的两种方式

![image-20210321222630274](/memory_img/image-20210321222630274.png)




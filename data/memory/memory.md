<font color=gree>聊聊Rust?</font>





跟Java不同，不用挪对象（Java会挪对象，由年轻代挪到老年代），这是Go的优点

CPU三级缓存，最初只有L1，后面增加到L3 

![image-20210401123109349](/memory_img/image-20210401123109349.png)

这里又涉及到<font color=red>伪共享</font>，说说伪共享。。。

<font color=red>内存访问流程</font>

![image-20210330110203813](/memory_img/image-20210330110203813.png)



# Go内存

## 简介

Go运行时的内存分配算法主要源自 Google 为 C 语言开发的<font color=red>TCMalloc算法</font>，全称<font color=red>Thread Caching Malloc</font>。核心思想就是把内存分为<font color=red>多级管理</font>，从而降低锁的粒度。它将可用的堆内存采用二级分配的方式进行管理：每个线程都会自行维护一个独立的<font color=red>内存池</font>，进行内存分配时优先从该内存池中分配，当内存池不足时才会向全局内存池申请，以避免不同线程对全局内存池的频繁竞争。所以Go在并发具有天生的优势。

![image-20210321180457491](/memory_img/image-20210321180457491.png)

1.与Java，Python不同，Go并没有虚拟机的概念，Runtime也直接被编译成native code

2.Go的Runtime与用户代码一起打包在一个可执行文件中，<font color=red>有点像小型虚拟机？</font>

3.用户代码与Runtime代码在执行的时候并没有明显的界限，都是函数调用

4.Go对系统调用的指令进行了封装，可不依赖于glibc

5.一些Go的关键字被编译成Runtime包下的函数

![image-20210321190242215](/memory_img/image-20210321190242215.png)

## Runtime发展历程

![image-20210321190659317](/memory_img/image-20210321190659317.png)

## 堆内存管理

​	![image-20210330115441018](/memory_img/image-20210330115441018.png)

## 栈内存

每个goroutine都有自己的栈，栈的初始大小是2KB，100万的goroutine会占用2G，但goroutine的栈会在2KB不够用时自动扩容，当扩容为4KB的时候，百万goroutine会占用4GB。

## go内存分配

![image-20210331193422810](/memory_img/image-20210331193422810.png)

小对象是在mcache中分配的，而大对象是直接从mheap分配的，从小对象的内存分配看起。

### 小对象分配

![image-20210331180034517](/memory_img/image-20210331180034517.png)

<font color=red>堆和栈形象比喻</font>

```go
通俗比喻的说，栈就如我们去饭馆吃饭，只需要点菜（发出申请）--》吃吃吃（使用内存）--》吃饱就跑剩下的交给饭馆（操作系统自动回收），而堆就如在家里做饭，大到家，小到买什么菜，每一个环节都需要自己来实现，但是自由度会大很多。
```



## 内存管理组件

内存分配由内存分配器完成。分配器由3种组件构成：<font color=red>`mcache`， `mcentral`, `mheap`</font>。

![image-20210321215114648](/memory_img/image-20210321215114648.png)

### <font color=red>mcache</font>

mcache与TCMalloc中的ThreadCache类似，<font color=red>**mcache保存的是各种大小的Span，并按Span class分类，小对象直接从mcache分配内存，它起到了缓存的作用，并且可以无锁访问**。</font>

但mcache与ThreadCache也有不同点，TCMalloc中是每个线程1个ThreadCache，Go中是<font color=red>**每个P拥有1个mcache**</font>，因为在Go程序中，当前最多有GOMAXPROCS个线程在运行，所以最多需要GOMAXPROCS个mcache就可以保证各线程对mcache的无锁访问，线程的运行又是与P绑定的，把mcache交给P刚刚好。

### <font color=red>mcentral</font>

mcentral与TCMalloc中的CentralCache类似，<font color=red>**是所有线程共享的缓存，需要加锁访问**</font>，它按Span class对Span分类，串联成链表，当mcache的某个级别Span的内存被分配光时，它会向mcentral申请1个当前级别的Span。

但mcentral与CentralCache也有不同点，CentralCache是每个级别的Span有1个链表，mcache是每个级别的Span有2个链表，这和mcache申请内存有关，稍后我们再解释。

### <font color=red>mheap</font>

mheap与TCMalloc中的PageHeap类似，<font color=red>**它是堆内存的抽象，把从OS申请出的内存页组织成Span，并保存起来**</font>。当mcentral的Span不够用时会向mheap申请，mheap的Span不够用时会向OS申请，向OS的内存申请是按页来的，然后把申请来的内存页生成Span组织起来，同样也是需要加锁访问的。

但mheap与PageHeap也有不同点：mheap把Span组织成了树结构，而不是链表，并且还是2棵树，然后把Span分配到heapArena进行管理，它包含地址映射和span是否包含指针等位图，这样做的主要原因是为了更高效的利用内存：分配、回收和再利用。





## 分配流程

变量是在栈上分配还是在堆上分配，是由<font color=red>逃逸分析</font>的结果决定的。Go是通过在编译器里做逃逸分析（escape analysis）来决定一个对象放栈上还是放堆上，不逃逸的对象放栈上，可能逃逸的放堆上；通常情况下，编译器是倾向于将变量分配到栈上的，因为它的开销小，最极端的就是"zero garbage"，所有的变量都会在栈上分配，这样就不会存在内存碎片，垃圾回收之类的东西。

<font color=red>逃逸分析</font>

```java
public void test(){
    List<Integer> a = new ArrayList<>();
    a.add(1); // a 未发生逃逸，因此在栈上分配
}

public List<Integer> test1(){
    List<Integer> a = new ArrayList<>();
    a.add(1);
    return a  // a 发生逃逸，因此分配在堆上
}

public void initResource(List<Resource> resources) {
    if (resource != null) {
        // resource对象通过传入的参数逃逸
        Resource resource = new Resource();
        resources.add(resource);
    }
}
```

首先在栈上分配，对于未发生逃逸的变量，则直接在栈上分配内存。因为栈上内存由在函数返回时自动回收，因此能减小gc压力。<font color=red>栈是线程私有的，栈上没有内存碎片</font>

不同于jvm的运行时逃逸分析，golang的逃逸分析是在编译期完成的。

<font color=red>逃逸分析总结：</font>

<font color=gree>不要盲目使用变量的指针作为函数参数，虽然它会减少复制操作。但其实当参数为变量自身的时候，复制是在栈上完成的操作，开销远比变量逃逸后动态地在堆上分配内存少的多。</font>





Go的内存分配器在分配对象时，根据对象的大小，分成三类：<font color=red>小对象（小于等于16B）、一般对象（大于16B，小于等于32KB）、大对象（大于32KB）。</font>

大体上的分配流程：

- <=16B 的对象使用mcache的tiny分配器分配；

- (16B,32KB] 的对象，首先计算对象的规格大小，然后使用mcache中相应规格大小的mspan分配；

- 32KB 的对象，直接从mheap上分配；

- 如果mcache没有相应规格大小的mspan，则向mcentral申请；

- 如果mcentral没有相应规格大小的mspan，则向mheap申请；

- 如果mheap中也没有合适大小的mspan，则向操作系统申请；


TCMalloc的定义：

1. 小对象大小：0~256KB
2. 中对象大小：257KB~1MB
3. 大对象大小：>1MB



## 总结

1.Go在程序启动时，会向操作系统申请一大块内存，之后自行管理。

2.Go内存管理的基本单元是mspan，它由<font color=red>若干个页</font>组成，每种mspan可以分配特定大小的object。

3.mcache, mcentral, mheap是Go内存管理的三大组件，层层递进。mcache管理线程在本地缓存的mspan；mcentral管理全局的mspan供所有线程使用；mheap管理Go的所有动态分配内存。

4.极小对象会分配在一个object中，以节省资源，使用<font color=red>tiny分配器</font>分配内存；一般小对象通过mspan分配内存；大对象则直接由mheap分配内存。

# 2. Java内存

## 2.1 运行时数据区域

![image-20210321221249431](/memory_img/image-20210321221249431.png)

线程共享的：<font color=red>堆、方法区、直接内存</font>

线程私有的：<font color=red>程序计数器、虚拟机栈、本地方法栈</font>



### 2.1.1 程序计数器

程序计数器是一块较小的内存空间，可以看作是<font color=red>当前线程所执行的字节码的行号指示器</font>。**字节码解释器工作时通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完。**

另外，**为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。**

**从上面的介绍中我们知道程序计数器主要有两个作用：**

1. 字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
2. 在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

**注意：程序计数器是唯一一个不会出现OutOfMemoryError的内存区域，它的生命周期随着线程的创建而创建，随着线程的结束而死亡。**



### 2.1.2 Java虚拟机栈

**与程序计数器一样，Java虚拟机栈也是线程私有的，它的生命周期和线程相同，<font color=red>描述的是 Java 方法执行的内存模型。</font>**

**Java 内存可以粗糙的区分为堆内存（Heap）和栈内存(Stack),其中栈就是现在说的虚拟机栈，或者说是虚拟机栈中局部变量表部分。** （实际上，Java虚拟机栈是由一个个栈帧组成，而每个栈帧中都拥有：局部变量表、操作数栈、动态链接、方法出口信息。）

**局部变量表主要存放了编译器可知的各种数据类型**（boolean、byte、char、short、int、float、long、double）、**对象引用**（reference类型，它不同于对象本身，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）。

**Java 虚拟机栈会出现两种异常：StackOverFlowError 和 OutOfMemoryError。**

- **StackOverFlowError：** 若Java虚拟机栈的内存大小不允许动态扩展，那么当线程请求栈的深度超过当前Java虚拟机栈的最大深度的时候，就抛出StackOverFlowError异常。
- **OutOfMemoryError：** 若 Java 虚拟机栈的内存大小允许动态扩展，且当线程请求栈时内存用完了，无法再动态扩展了，此时抛出OutOfMemoryError异常。

Java 虚拟机栈也是线程私有的，每个线程都有各自的Java虚拟机栈，而且随着线程的创建而创建，随着线程的死亡而死亡。



### 2.1.3 本地方法栈

和虚拟机栈所发挥的作用非常相似，区别是： **虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。** 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。

本地方法被执行的时候，在本地方法栈也会创建一个栈帧，用于存放该本地方法的局部变量表、操作数栈、动态链接、出口信息。

方法执行完毕后相应的栈帧也会出栈并释放内存空间，也会出现 StackOverFlowError 和 OutOfMemoryError 两种异常。



### 2.1.4 堆

Java 虚拟机所管理的内存中最大的一块，Java 堆是所有线程共享的一块内存区域，在虚拟机启动时创建。<font color=red>**此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。**</font>

Java 堆是垃圾收集器管理的主要区域，因此也被称作**GC堆（Garbage Collected Heap）**.从垃圾回收的角度，由于现在收集器基本都采用分代垃圾收集算法，所以Java堆还可以细分为：新生代和老年代：在细致一点有：Eden空间、From Survivor、To Survivor空间等。**进一步划分的目的是更好地回收内存，或者更快地分配内存。**

![image-20210322232205694](/memory_img/image-20210322232205694.png)

**在 JDK 1.8中移除整个永久代，取而代之的是一个叫元空间（Metaspace）的区域（永久代使用的是JVM的堆内存空间，而元空间使用的是物理内存，直接受到本机的物理内存限制）。**



### 2.1.5 方法区

**方法区与 Java 堆一样，是各个线程共享的内存区域，它用于<font color=red>存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据</font>。虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。**

HotSpot 虚拟机中方法区也常被称为 **“永久代”**，本质上两者并不等价。仅仅是因为 HotSpot 虚拟机设计团队用永久代来实现方法区而已，这样 HotSpot 虚拟机的垃圾收集器就可以像管理 Java 堆一样管理这部分内存了。但是这并不是一个好主意，因为这样更容易遇到内存溢出问题。

**相对而言，垃圾收集行为在这个区域是比较少出现的，但并非数据进入方法区后就“永久存在”了。**



### 2.1.6 运行时常量池

运行时常量池是方法区的一部分。Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有常量池信息（用于存放编译期生成的各种字面量和符号引用）

既然运行时常量池时方法区的一部分，自然受到方法区内存的限制，当常量池无法再申请到内存时会抛出 OutOfMemoryError 异常。

**JDK1.7及之后版本的 JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池。**



### 2.1.6 直接内存

直接内存并不是虚拟机运行时数据区的一部分，也不是虚拟机规范中定义的内存区域，但是这部分内存也被频繁地使用。而且也可能导致OutOfMemoryError异常出现。

JDK1.4中新加入的 **NIO(New Input/Output) 类**，引入了一种基于**通道（Channel）** 与**缓存区（Buffer）** 的 I/O 方式，它可以直接使用Native函数库直接分配堆外内存，然后通过一个存储在 Java 堆中的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样就能在一些场景中显著提高性能，因为**避免了在 Java 堆和 Native 堆之间来回复制数据**。

<font color=red>零拷贝就是使用的这个，下次讲讲各个框架零拷贝？</font>

本机直接内存的分配不会收到 Java 堆的限制，但是，既然是内存就会受到本机总内存大小以及处理器寻址空间的限制。



## 2.2 Java对象创建过程

![image-20210321222133194](/memory_img/image-20210321222133194.png)



## 2.3 内存分配的两种方式

![image-20210321222630274](/memory_img/image-20210321222630274.png)



## 2.4 对象的内存布局

在 Hotspot 虚拟机中，对象在内存中的布局可以分为3快区域：<font color=red>**对象头**、**实例数据**和**对齐填充**。</font>

**Hotspot虚拟机的对象头包括两部分信息**，**第一部分用于存储对象自身的自身运行时数据**（哈希吗、GC分代年龄、锁状态标志等等），**另一部分是类型指针**，即对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是那个类的实例。

**实例数据部分是对象真正存储的有效信息**，也是在程序中所定义的各种类型的字段内容。

**对齐填充部分不是必然存在的，也没有什么特别的含义，仅仅起占位作用。** 因为Hotspot虚拟机的自动内存管理系统要求对象起始地址必须是8字节的整数倍，换句话说就是对象的大小必须是8字节的整数倍。而对象头部分正好是8字节的倍数（1倍或2倍），因此，当对象实例数据部分没有对齐时，就需要通过对齐填充来补全。



## 2.5 对象的访问定位

建立对象就是为了使用对象，Java程序通过栈上的 reference 数据来操作堆上的具体对象。对象的访问方式有虚拟机实现而定，目前主流的访问方式有**①使用句柄**和**②直接指针**两种：

### 2.5.1 句柄

如果使用句柄的话，那么Java堆中将会划分出一块内存来作为句柄池，reference 中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自的具体地址信息；

![image-20210322222641098](/memory_img/image-20210322222641098.png)



### 2.5.2 直接指针

如果使用直接指针访问，那么 Java 堆对象的布局中就必须考虑如何防止访问类型数据的相关信息，reference 中存储的直接就是对象的地址。

![image-20210322224323277](/memory_img/image-20210322224323277.png)

<font color=red>**这两种对象访问方式各有优势。使用句柄来访问的最大好处是 reference 中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而 reference 本身不需要修改。使用直接指针访问方式最大的好处就是速度快，它节省了一次指针定位的时间开销。**</font>




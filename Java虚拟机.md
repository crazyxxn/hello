# JVM 概述

## JVM平台无关性

   Java是一个跨平台的语言，一次编译，到处运行。    
   	Java的跨平台，就是因为JVM的跨平台。其实JVM并不跨平台，大家在安装jdk,有Windows，Linux，mac,os版本。     
   一个程序在不同的机器上面执行，需要这么几个过程！     
   	Java源代码(.java) --> 编译(.class) --> 解释执行（机器码）     
   	C语言源代码(.c) ---> 编译(.h)（这个编译后的文件是 能够直接在计算机中执行）  

##  JVM简介
   
   JVM 全称 Java Virtual Machine(Java虚拟机)    
   	通过软件来模拟出来的具有完整的硬件系统功能的、运行在完全隔离的环境中的完整的计算机系统。    
   JVM世界观：java对象在jvm里生老病死。（ 从创建-->销毁 ）  
   作为java编译器和os之间的虚拟解释器，JVM根据不同的os，将java编译后的目标代码（字节码）解释成不同os可以运行的机器指令，所以说：有了JVM之后，java语言在不同平台上运行时不需要重新编译。一次编写，处处运行！  
   
## JVM 发展史  

  可以通过java -version来查看当前jdk中使用的jvm版本  
  	HotSpot VM:热代码追踪款    
  历史经典版本：classic VM(经典款）, extact VM（准确式内存管理款）--基于句柄的查找。  
   --句柄（https://www.cnblogs.com/mlgjb/p/8588028.html）
	
### hotspot	热代码探测技术

  01、从程序找出最具有编译价值的代码进行编译    
  02、通过编译器与解释器恰当地协同工作，可以在最优化的程序响应时间与最佳执行性能中取得平衡，即时编译的时间压力也相对减小，这样有助于引入更多的代码优化技术，输出质量更高的本地代码[机器执行码]。  

## JVM基本原理

  # JVM运行时内存区域
  
  JVM内存区域从线程安全的角度可以分为：共享区域和线程隔离区域。  
  JVM内存区域从具体的空间划分角度上可以分为：栈内存、堆内存，程序寄存器、方法区（在jdk1。8开始叫做Metaspace）,
其中栈内存可细分为：本地方法栈内存，虚拟机栈；堆内存可分为：新生代和老年代，其中新生代又分为Eden伊甸区，From Survivor,To Survivor幸存区。  
	共享区域：堆内存区域、方法区  
	线程隔离区域：栈内存、程序寄存器

### 程序计数器

   程序计数器（Program Counter Register）是当前线程所执行的字节码的行号指示器，字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。  
   为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各条线程之间计数器互不影响，独立存储，所以程序计数器这类内存区域为“线程私有”的内存。  	
	
   如果线程正在执行的是**Native**方法，这个计数器值则为**空**（Undefined）。  
   native方法 是与C++联合开发的时候用的！使用native关键字说明这个方法是原生函数，也就是这个方法是用C/C++语言实现的，并且被编译成了DLL，由java去调用。——JNI

### 栈内存

   所谓“栈”包括：java虚拟机栈、本地方法栈；他们作用相似，区别只是：虚拟机栈为虚拟机执行Java方法（也就是字节码）服务，而本地方法栈则为虚拟机使用到的Native方法服务。程序员人为的分为“堆栈”中的“栈”。  
   栈里存放了编译期可知的各种基本数据类型（boolean、byte、char、short、int、float、long、double）、对象引用和指向了一条字节码指令的地址。  
   每个方法在执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。    
   局部变量表所需的内存空间在编译期间完成分配，其中64位的long和double类型的数据会占2个局部变量空间，其余的数据类型只占用1个。当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。  
	操作数栈也要操作栈，主要是在方法计算时存放的栈。  
 	设置栈的大小的参数，-Xss

  	

    
### 堆内存

   Java堆（Java Heap）是Java虚拟机所管理的内存中最大的一块，此内存区域就是存放对象实例，几乎所有的对象实例都在这里分配内存。  
   Java堆是垃圾收集器管理的主要区域；内存回收的角度来看Java堆中还可以细分为：新生代和老年代；新生代细致一点的有Eden空间、From Survivor空间、To Survivor空间。  
	在实现时，既可以实现成固定大小的，也可以是可扩展的，不过当前主流的虚拟机都是按照可扩展来实现的（通过**-Xmx设置最大内存和-Xms设置初始内存**）  
   java  -Xms10m -Xmx10m Hello -->为当前Hello进程分配的初始化堆内存空间和最大堆内存空间是一致的，所以此堆内存空间是固定大小的。  
   java  -Xms10m -Xmx100m Hello -->为当前Hello进程分配的初始化堆内存空间和最大堆内存空间是不一致的，所以此堆内存空间是可扩展的。  

   需要注意的是，-Xmx的值 >= -Xms的值  

### 方法区

   方法区又叫静态区：用于存储已被虚拟机加载的类信息、常量池、静态变量、即时编译器编译后的代码等数据。虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做Non-Heap（非堆）；对于HotSpot虚拟机是使用永久代来实现方法区；  
   Java虚拟机规范对方法区的限制非常宽松，除了不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾收集。相对而言，垃圾收集行为在这个区域是比较少出现的，这区域的内存回收目标主要是针对常量池的回收和对类型的卸载，条件相当苛刻。  
   java中的常量池技术，是为了方便快捷地创建某些对象而出现的，当需要一个对象时，就可以从池中取一个出来（如果池中没有则创建一个），在需要重复创建相等变量时节省了很多时间。  

```
public class ConstantPool {
    public static void main(String[] args) {
        /*
            String str1=new String("hello");创建了几个对象？
            2
         */
            String str1 = new String("hello");

            String str2 = new String("hello");
            /**
             * ==的比较
             *  如果两侧是基本数据类型，比较的实际的值
             *  如果两侧是引用型数据类型，比较的是内存地址值
             * eqauls()，该方法是Object中的一个方法
             *  默认比较也是内存地址值
             *  在String类中equals比较的两个字符串的值是否相等
             *
             * str1.toString()==str2.toString() <=> str1 == str2
             * public String toString() {
             return this;
             }
             */
            System.out.println(str1==str2);//f
            System.out.println(str1.equals(str2));//t
            System.out.println(str1.toString()==str2.toString());//f
            String str3="hello";
            String str4="hello";

            System.out.println(str3 == str4);//t
        /*
            装箱和拆箱
            装箱，就是将java基本数据类型转化为对象的操作
            拆箱，就是将java对象转化为基本数据类型的操作
            一般情况下都是自动来完成，就是说不用创建对象的方式来完成
            Integer i = new Integer(3);//装箱或者创建对象
            Integer i = 3;//自动装箱
            int a = i.intValue();//拆箱
            int a = new Integer(3);//自动拆箱
         */
            System.out.println(new Integer(1) == new Integer(1));//f
            int a = new Integer(127);
            int b = new Integer(127);
            //此时a和b都是基本数据类型，比较的是值
            System.out.println("a==b?" + (a == b));//t
            /**
             * jvm有一个数字优化的技术，会将一个byte范围内[-128, 127]之间的数据的创建交给方法区
             */
            Integer c = 127;
            Integer d = 127;
            System.out.println("c==d?" + (c == d));//t
            Integer e = 128;//这里面有一个自动装箱new Integer(128)
            Integer f = 128;//这里面有一个自动装箱new Integer(128)
            System.out.println("e==f?" + (e == f));//f
    }
   
}
```

## JVM的异常

程序计数器
没有指定任何OutOfMemoryError情况
java虚拟机栈\本地方法栈区域
如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；如果扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常
堆
如果在堆中没有内存完成实例分配，并且堆也无法再扩展时，将会抛出OutOfMemoryError异常 
 报错后dump出信息： -XX:+HeapDumpOnOutOfMemoryError
方法区
当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常

# 对象的引用**  
   Java中的引用除了*强引用*（Person p = new Person())以外，还有其他三种引用，软引用，弱引用，虚（幽灵）引用。这四种引用的强弱关系，依次降低。  
强引用  
	在程序代码之中正常的类似于"Person p = new Person()"这类的引用；垃圾收集器不会回收掉被强引用的对象。
软引用   
	有用但非必须的对象，jdk中提供了SoftReference类来实现软引用；系统在发生内存溢出异常之前，会把只被软引用的对象进行回收。  
	用途？可以做缓存
虚引用  
对被引用对象的生存时间不影响；无法通过虚引用来取得一个对象实例；为一个对象设置虚引用关联的唯一目的就是能在这个对象被收集器回收时收到一个系统通知；jdk提供PhantomReference类来实现虚引用。

'''
public class ReferenceTest {
    public static void main(String[] args) {
        System.out.println("==========强引用===========");
        Person p = new Person();
        System.gc();//
        System.out.println(p);

        System.out.println("=====软引用=====");
        SoftReference<Person> sp = new SoftReference<Person>(new Person());
        System.gc();
        System.out.println(sp.get());

        System.out.println("=====弱引用======");
        WeakReference<Person> wp = new WeakReference<Person>(new Person());
        System.gc();
        System.out.println(wp.get());

        System.out.println("======虚引用======");
        ReferenceQueue<Person> referenceQueue = new ReferenceQueue<Person>();
        Person person = new Person();
        PhantomReference<Person> pp = new PhantomReference<Person>(person,referenceQueue);
        person = null;
        System.out.println(referenceQueue.poll());
        System.gc();
        System.out.println(pp.get());
        try {
            //gc后等1秒看结果
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(referenceQueue.poll());
        System.out.println("=============softReference对象的回收======================");
        try {
            List<HeapOOM.OOMObject> list;
            list = new ArrayList<HeapOOM.OOMObject>();

            while (true) {
                list.add(new HeapOOM.OOMObject());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println(sp.get());
        }

    }
}


class Person{
    String name = "张三" ;

    @Override
    public String toString(){
        return name;
    }

}'''

# 对象可达性分析***
   Java中用来判定对象是否被引用，就通过可达性分析来完成，早期最简单的方式就是通过引用计数器法来帮你判定对象是否被引用。  
   
## 引用计数法
   引用计数算法基本思想：给对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻计数器为0的对象就是不可能再被使用的。  
   
## 可达性分析
   通过一系列的称为"GC Roots"的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链(Reference Chain),当一个对象GC Roots没有任何引用链相连（即不可达）时，则证明此对象是不可用的。  
   
## GC Roots对象：
   虚拟机栈（栈帧中的本地变量表）中引用的对象。  
   方法区中类静态属性引用的对象。
   方法区中常量引用的对象。  
   本地方法栈中JNI(Java Native Interface即一般说的Native方法)引用的对象。  
   
# 对象的生死
 ## 判定对象的生死，需要进行两次的标记。
 
   当对象不可达的时候会被标记，此为第一次标记。如果我们对该对象复写了finalize()方法，并且没有被虚拟机调用过，会将该对象存放到一个叫F-Queue的队列中，稍后会由虚拟机启动一个低优先级的线程去处理这个队列中的对象。  
   如果在执行gc的时候，对象在该finalize()方法中重新和引用链上的其他对象建立起了引用，此时该对象就被标记为存活对象，不会被回收，否则执行第二次标记，执行完两次标记的对象将会被gc回收掉。







# 垃圾回收算法****
## 标记-清除
   最基础的收集算法是“标记-清除”（Mark-Sweep）算法，此方法分为两个阶段：标记，清除。  
   标记要清除的对象，统一清除；
   不足有两个：  
   一个是*效率问题*，标记和清除两个过程的效率都不高；  
   另一个是'空间问题'，标记清除之后会产生大量不连续的内存碎片，空间碎片太多可能会导致以后在程序运行过程中需要分配较大对象时，无法找到足够连续内存而不得不提前触发另一次垃圾收集动作。  
   由上述可推断出，该算法不适合在新生代中使用，相对比较适合于老年代。
   
 ## 标记-整理-清除  
 对标记清除算法的优化，就是标记整理清除。
 
 ## 复制算法  
   是新生代的算法  
   复制算法：它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。  
   优点：无内存碎片，只要移动堆顶指针，按顺序分配内存即可，实现简单，运行高效。缺点：实际可用内存缩小为原来的一半。  
   现在的商业虚拟机都采用这种收集算法来回收新生代。  
   1、将内存分为一块较大的Eden空间和两块较小的Survivor空间；  
   2、每次使用Eden和其中一块Survivor.  
   3、当回收时，将Eden和Survivor中还存活着的对象一次性地复制到另一块Survivor空间上，并清理掉Eden和刚才用过的Survivor空间。  
   HotSpot虚拟机默认Eden和Survivor的大小比例是8：1：1，浪费10%。  
   
## 垃圾回收方式 #####   
   也就是gc的方式。  
   Minor GC：清理年轻代的gc被称之为Minor GC ---> 采用的是复制算法  
   Major GC：清理老年代或者永久代的gc被称之为Major GC ---> 标记清除算法  
   Full GC：清理整个堆内存空间的gc被称为Full GC = MinorGC + MajorGC  
   
# JVM内存分配  
## 分配策略  
   对象在Eden分配：大多数情况下，对象在新生代Eden区中分配。当Eden区没有足够空间进行分配时，虚拟机将发起一次Minor GC，此时对象会进入Survivor区，当对象满足一些条件后会进入老年代。  
   ## 对象进入老年代的三种方式  
   1、big object直接进入老年代  
   虚拟机提供了一个-XX：PretenureSizeThreshold参数，令大于这个设置值的对象直接在老年代分配。这样做的目的时避免在Eden区及两个Survivor区之间发生大量的内存复制。  
   
   令体积大于3M的对象在老年代中创建，小于该阈值的对象在eden区中被创建。  
   2、当对象的age达到阈值的时候，直接进入老年代  
   jvm中的对象，自eden区被创建起，就有了年龄，当熬过一次minor gc没有被回收之后，年龄age+1.默认情况下经过15此gc之后，任然没有被垃圾回收掉的对象，也就是对象年龄>=15，晋升到老年代。  
   可以通过-xx:MaxTenuringThreshould参数来设置对象的进入老年代的年龄阈值。  
   
   
   3、当某一年龄的对象的总体积达到了survivor内存的一半的时候，大于等于该年龄的对象集体进入老年代。  
   无须等到MaxTenuringThreshould中要求的年龄。  
   # 空间分配担保  
   在发生Minor gc之前，虚拟机会先检查老年代最大可用的连续空间是否大于新生代的所有对象总空间，如果这个条件成立，那么Minor GC可以确保时安全的。  
   如果不成立，则虚拟机会查看-XX:HandlePromotionFailure设置值是否允许担保失败。  
   如果允许，那么会继续检查老年代最大可用的连续空间是否大于历次晋升到老年代对象的平均大小，如果大于，将尝试着进行一次Minor GC，尽管这次Minor GC是有风险的；如果小于，或者HandlePromotionFailure设置不允许冒险，那这时也要改为进行一次Full GC。
   
   # 垃圾收集器
   ## 并行和并发  
   并行（parallel）：指多条垃圾收集线程并行工作，但是此时：用户线程仍然处于线程等待状态。  
   并发（Concurrent）：指用户线程和垃圾收集线程同时执行（但不一定时并行的，可能会交替执行），用户程序在继续运行，而垃圾收集程序运行于另一个cpu上。
   ## 常见垃圾收集器  
   ### Serial  
   Serial收集器时最基础、历史最悠久的适合新生代的收集器。  
   特点：单线程、stop-the-world、复制算法  
   缺点：影响用户响应时间  
   优点：回收时简单高效、对于限定单个cpu环境下，serial收集器由于没有线程交互的开销，专心做垃圾收集，可以获得最高的单线程收集效率。  
   所以：serial收集器对于运行在client模式下的虚拟机来说，是一个很好的选择。  
   serialOld收集器时Serial的老年代收集器，采用“标记-整理”指定Serial的方式：  
   
   





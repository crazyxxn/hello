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
        System.out.println(str1==str2);//true --->周攀 
        System.out.println(str1.equals(str2));//false 俞剑波
        System.out.println(str1.toString()==str2.toString());//true 李红伟
        String str3="hello";
        String str4="hello";

        System.out.println(str3 == str4);
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
        System.out.println(new Integer(1) == new Integer(1));//true --俞剑波
        int a = new Integer(127);
        int b = new Integer(127);
        //此时a和b都是基本数据类型，比较的是值
        System.out.println("a==b?" + (a == b));//攀哥--true
        /**
         * jvm有一个数字优化的技术，会将一个byte范围内[-128, 127]之间的数据的创建交给方法区
         */
        Integer c = 127;
        Integer d = 127;
        System.out.println("c==d?" + (c == d));//false
        Integer e = 128;//这里面有一个自动装箱new Integer(128)
        Integer f = 128;//这里面有一个自动装箱new Integer(128)
        System.out.println("e==f?" + (e == f));//false
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

# JVM内存分配

# JVM分析工具


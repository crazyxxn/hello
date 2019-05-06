# JVM简介

   Java是一个跨平台的语言，一次编译，到处运行。
   Java的跨平台，就是因为JVM的跨平台。其实JVM并不跨平台，大家在安装jdk,有Windows，Linux，mac,os版本。
  
一个程序在不同的机器上面执行，需要这么几个过程！

Java源代码(.java) --> 编译(.class) --> 解释执行（机器码）

C语言源代码(.c) ---> 编译(.h)

##  JVM简介
   JVM Java Virtual Machine(Java虚拟机)
   通过软件来模拟出来的具有完整的硬件系统功能的、运行在完全隔离的环境中的完整的计算机系统。
   JVM世界观：java对象在jvm里生老病死。（ 从创建-->销毁 ）
    
## JVM 发展史 
  可以通过java -version来查看当前jdk中使用的jvm版本
  HotSpot VM:热代码追踪款
  历史经典版本：classic extact
	
classic 

extact
  基于句柄的查找，准确式内存管理
	
hotspot
  热代码探测技术
  01
  
  02

## JVM基本原理
  # JVM运行时内存区域
    JVM内存区域从线程安全的角度可以分为：共享区域和线程隔离区域。
    JVM内存区域从具体的空间划分角度上可以分为：栈内存、堆内存，程序寄存器、方法区（在jdk1。8开始叫做Metaspace）,
其中栈内存可细分为：本地方法栈内存，虚拟机栈；堆内存可分为：新生代和老年代，其中新生代又分为Eden伊甸区，From Survivor,To Survivor幸存区。
    
    #



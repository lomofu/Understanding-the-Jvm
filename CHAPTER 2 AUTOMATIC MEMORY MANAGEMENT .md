# CHAPTER 2 																AUTOMATIC MEMORY MANAGEMENT 			自动内存管理机制



### The Runtime Data Area  运行时数据区域



![运行时数据区域](C:\Users\FULO2\Downloads\Understanding the JVM\src\20141127164732211.png)



> #### Program Counter Register 程序计数器

The function of Program Counter Register is to save the row of .class which has run currently.

可以理解为 保存当前线程执行到的字节码的行号 比如63行...



Why the PCR is thread private?

==为什么PCR是线程私有的?==

*If you want to understand why the PCR is thread private, you should know the multi-thread of java. Honestly, the JVM implements the multi-thread according to switch the thread in a order and assign the time of executing of CPU(Central Processing Util).*

*此处需要结合java 的多线程去理解,java的多线程是通过线程之间的轮流切换和分配cpu的执行时间实现的*



So to keep the thread recovery correctly after switching the thread, wo need to make every thread have it private PCR to recored the .class which has run currently. In other words, every thread should not affect to each other. If the thread execute a java method, the PCR will recored the address of the order of the current executing .class. If the thread execute a Native method, the PCR will not recored anything, also the value of the PCR is null. This is the only memory area in the JVM which doesn't have any regualtion of the circumstance of 'Out of Memory Error'.

所以为了保证线程切换后能恢复到正确的执行顺序, (比如A线程执行到63行,切换到B线程,执行一段时间后又切换回A线程时,通过PCR记录的值来对应A线程当前执行的行数 也就是63行). 所以各个线程之间不能相互影响,也就是这块区域的数据是线程隔离的. 如果线程执行的是一个java方法 PCR会记录正在执行的虚拟机的字节码指令的地址. 如果线程执行的是一个Native方法,PCR 将不会记录,计数器的值是null. 这是唯一一块在java虚拟机规范中没有规定任何的Out of Memory Error 的情况.



> #### Java Virtual Machine Stacks 虚拟机栈

The JVMS describe the model of memory of execution of  java methods

Java 的虚拟机栈是描述java 方法执行的内存模型




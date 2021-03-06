# lec1: 操作系统概述

---

## **提前准备**

（请在上课前完成）

* 完成lec1的视频学习和提交对应的在线练习
* git pull ucore\_os\_lab, ucore\_os\_docs, os\_tutorial\_lab, os\_course\_exercises in github repos。这样可以在本机上完成课堂练习。
* 知道OS课程的入口网址，会使用在线视频平台，在线练习/实验平台，在线提问平台\(piazza\)
  * [http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring](http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring)


* 会使用linux shell命令，如ls, rm, mkdir, cat, less, more, gcc等，也会使用linux系统的基本操作。
* 在piazza上就学习中不理解问题进行提问。

> 已完成准备。

# 思考题

## 填空题

* 当前常见的操作系统主要用__C，汇编__编程语言编写。*少数为C++和其他语言*
* "Operating system"这个单词起源于__Operator__ 。
* 在计算机系统中，控制和管理__内存等硬件资源__ 、有效地组织__程序__运行的系统软件称作__操作系统__ 。*多道程序*
* 允许多用户将若干个作业提交给计算机系统集中处理的操作系统称为__批处理__操作系统
* 你了解的当前世界上使用最多的操作系统是__Linux__ 。
* 应用程序通过__系统调用__接口获得操作系统的服务。
* 现代操作系统的特征包括__并发__ ， __共享__ ， __虚拟__ ，__异步__ 。持久性
* 操作系统内核的架构包括__单内核结构__ ， __微内核结构__ ， __外核结构__ 。


## 问答题

- 请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。

  **并发性**：在多道操作系统出现之后，在一个工作时间段内，内存中并不只有一个作业，而是保持多个工作在内			  		存中并且在各工作间复用CPU。这就要求操作系统对同一时间段内运行的程序进行CPU资源的管理和调度，使得CPU资源能够更高地得到利用。

  **共享性**：内存中并发运行的程序有可能需要同时访问内存和其他硬件资源，这要求操作系统做到控制这些进程进行“互斥共享”。

  **虚拟性**：我们希望操作系统给我们带来更好的计算机使用体验，因此操作系统进行了很多方面的虚拟，如虚拟内存等等。通过多道技术，每一个进程都在cpu中以极小的时间片交替进行，这也让用户觉得自己占用了整个计算机的资源。

  **异步性**：异步性是指进程以不可预知的速度向前推进。内存中的每个进程何时执行,何时暂停,以怎样的速度向前推进,每道程序总共需要多少时间才能完成等,都是不可预知的。这是描述进程这种以不可预知的速度走走停停、何时开始何时暂停何时结束不可预知的性质。


- 为什么现在的操作系统基本上用C语言来实现？为什么没有人用python，java来实现操作系统？

  操作系统本质上是要控制硬件，因此汇编当然是最优的选择，但是汇编的实现难度较大，所以有了可以和汇编一一对应的C语言出现，用C语言本质上还是在使用汇编语言编写操作系统，只不过实现难度大大降低，更接近人的思维。相反，python和java这类高级语言会有很多你未知的底层操作，比如内存的分配和回收机制，我们并不能真实地接触到底层的硬件，因此实现操作系统是不妥的。

---

## 可选练习题

---

- 请分析并理解[v9\-computer](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)以及模拟v9\-computer的em.c。理解：在v9\-computer中如何实现时钟中断的；v9 computer的CPU指令，关键变量描述有误或不全的情况；在v9\-computer中的跳转相关操作是如何实现的；在v9\-computer中如何设计相应指令，可有效实现函数调用与返回；OS程序被加载到内存的哪个位置,其堆栈是如何设置的；在v9\-computer中如何完成一次内存地址的读写的；在v9\-computer中如何实现分页机制。

  实现时钟中断：

  描述有误和不全：



- 请编写一个小程序，在v9-cpu下，能够输出字符


- 输入的字符并输出你输入的字符


- 请编写一个小程序，在v9-cpu下，能够产生各种异常/中断


- 请编写一个小程序，在v9-cpu下，能够统计并显示内存大小



- 请分析并理解[RISC-V CPU](http://www.riscvbook.com/chinese/)以及会使用模拟RISC\-V(简称RV)的qemu工具。理解：RV的特权指令，CSR寄存器和在RV中如何实现时钟中断和IO操作；OS程序如何被加载运行的；在RV中如何实现分页机制。
  - 请编写一个小程序，在RV下，能够输出字符

  - 输入的字符并输出你输入的字符

  - 请编写一个小程序，在RV下，能够产生各种异常/中断

  - 请编写一个小程序，在RV下，能够统计并显示内存大小

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)

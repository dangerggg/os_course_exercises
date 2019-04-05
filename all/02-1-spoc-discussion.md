# lec 3 SPOC Discussion

## **提前准备**
（请在上课前完成）


 - 完成lec3的视频学习和提交对应的在线练习

 - git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises  　in github repos。这样可以在本机上完成课堂练习。

 - 仔细观察自己使用的计算机的启动过程和linux/ucore操作系统运行后的情况。搜索“80386　开机　启动”

   http://www.360doc.com/content/14/0911/15/480065_408677026.shtml

 - 了解控制流，异常控制流，函数调用,中断，异常(故障)，系统调用（陷阱）,切换，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在linux, ucore, v9-cpu中的os*.c中是如何具体体现的。

 - 思考为什么操作系统需要处理中断，异常，系统调用。这些是必须要有的吗？有哪些好处？有哪些不好的地方？

 - 了解在PC机上有啥中断和异常。搜索“80386　中断　异常”

   https://blog.csdn.net/vicrobert/article/details/41478453

 - 安装好ucore实验环境，能够编译运行lab8的answer

 - 了解Linux和ucore有哪些系统调用。搜索“linux 系统调用", 搜索lab8中的syscall关键字相关内容。在linux下执行命令: ```man syscalls```

 - 会使用linux中的命令:objdump，nm，file, strace，man, 了解这些命令的用途。

 - 了解如何OS是如何实现中断，异常，或系统调用的。会使用v9-cpu的dis,xc, xem命令（包括启动参数），分析v9-cpu中的os0.c, os2.c，了解与异常，中断，系统调用相关的os设计实现。阅读v9-cpu中的cpu.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。

 - 在piazza上就lec3学习中不理解问题进行提问。

> 已基本完成，v9-CPU仍在学习中

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
- 请描述在“计算机组成原理课”上，同学们做的MIPS CPU是从按复位键开始到可以接收按键输入之间的启动过程。

  MIPS CPU可以接收按键输入主要是运行了监控程序，实际上监控程序可以看作一个简单的操作系统，只不过没有我们说的操作系统那么复杂。我们可以事先将监控程序通过软件直接烧到RAM中或者通过flash进行自启动，事实上这也就是bootloader做的事。reset之后CPU从0地址开始执行程序，进行串口和SRAM的检查，并且在经过一系列的初始化操作之后通过串口发出ready的信号，我们便可以通过按键进行输入。

- x86中BIOS从磁盘读入的第一个扇区是是什么内容？为什么没有直接读入操作系统内核映像？

  MBR主引导记录。实际上硬盘的厂商有很多，BIOS并不知道他要找的OS存的磁盘用的是什么文件系统，所以要先从主引导记录中获取磁盘的格式信息。其中主引导扇区读过之后BIOS会进入当前唯一的活动分区读取活动分区引导记录，再从活动分区中读取操作系统。

- 比较UEFI和BIOS的区别。

  首先UEFI是一套新的规范，他是设计出来代替BIOS的借口。固件

  UEFI首先有一个verification的机制保证了一定的安全性，并且UEFI在独立分区中启动，有一定的隔离行。其次UEFI用C编写也实现了跨平台的可扩展特性。最后UEFI摆脱了BIOS设计上主引导记录的大小限制问题，使得bootloder的设计更加灵活，功能更加强大。

- 理解rcore中的Berkeley BootLoader (BBL)的功能。

  It is a supervisor execution environment for tethered RISC-V systems. It is designed to host the RISC-V Linux port.

  https://www.bsdcan.org/2016/schedule/attachments/385_riscv_bsdcan16.pdf

  1. Hardware initialization (e.g., DRAM, serial) 

  2. Passing boot parameters to kernel 
  3. Loading the kernel 

## 3.2 系统启动流程

- x86中分区引导扇区的结束标志是什么？

  `0x55` `0xAA`

- x86中在UEFI中的可信启动有什么作用？

  保证了磁盘的可靠性，尤其是在服务器的应用方面，如果随便插一个disk就能启动的话，结果会非常恐怖。

- RV中BBL的启动过程大致包括哪些内容？

  1. Hardware initialization (e.g., DRAM, serial) 
  2. Passing boot parameters to kernel 
  3. Loading the kernel 

## 3.3 中断、异常和系统调用比较
- 什么是中断、异常和系统调用？

  中断：外设向CPU提出请求

  异常：应用程序执行错误，包括非法地址和非法操作以及非法指令等

  系统调用：应用程序向操作系统提出服务请求

- 中断、异常和系统调用的处理流程有什么异同？

  相同点：应对中断、异常和系统调用，操作系统都会去查中断向量表，也就是我们可以把这三者都归为“中断”

  不同点：中断、异常和系统调用的触发源不同，中断是外设，异常是执行错误，系统调用是应用程序；这三者的处理方式不同，中断为异步（操作系统并不知道什么时候触发中断），异常为同步，系统调用既可异步也可同步；

- 以ucore/rcore lab8的answer为例，ucore的系统调用有哪些？大致的功能分类有哪些？

  大致有内存管理，进程切换和IO操作和文件操作四种系统调用，这也是操作系统基本的四种系统调用。

- EFLAGS寄存器的内容

## 3.4 linux系统调用分析
- 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(仅实践，不用回答)

- 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(仅实践，不用回答)

  > mooc上已经有了比较详细的说明，可以进一步阅读源码去获得更多的信息。


## 3.5 ucore/rcore系统调用分析 （扩展练习，可选）
- 基于实验八的代码分析ucore的系统调用实现，说明指定系统调用的参数和返回值的传递方式和存放位置信息，以及内核中的系统调用功能实现函数。

- 以ucore/rcore lab8的answer为例，分析ucore 应用的系统调用编写和含义。

- 以ucore/rcore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。

  > 暂时未完成相关的实验。


## 3.6 请分析函数调用和系统调用的区别
- 系统调用与函数调用的区别是什么？

  最大的区别是系统调用用了一套新的堆栈来保证操作系统的安全性。

  由于安全性的要求，系统调用比函数调用的效率要低很多。

- 通过分析x86中函数调用规范以及`int`、`iret`、`call`和`ret`的指令准确功能和调用代码，比较x86中函数调用与系统调用的堆栈操作有什么不同？

  函数调用使用`call`和`ret`，系统调用使用`int`和`iret`。

  call：将IP和CS压栈并进行转移

  ret：将IP弹出栈实现所谓的进转移

  int：首先系统调用需要CPU引发一个中断并对其进行处理，然后将标志寄存器入栈，接着将CS和IP入栈

  iret：通过栈中的内容设置IP，CS以及标志寄存器

  所以，系统调用的堆栈操作设计到的寄存器更多，操作也更加复杂

- 通过分析RV中函数调用规范以及`ecall`、`eret`、`jal`和`jalr`的指令准确功能和调用代码，比较x86？`riscv`

  中函数调用与系统调用的堆栈操作有什么不同？

  jal和jalr都是函数调用的功能，jal会将地址保存在$sa寄存器中以便执行完成后返回，jalr则可以自己选择保存的寄存器，整个调用过程不涉及返回地址的堆栈操作。

  ecall原本是scall，是超级用户模式下的系统调用，通过引发环境调用异常来请求执行环境。

  没有在risc-v中找到eret指令，是否是ebreak？


## 课堂实践 （在课堂上根据老师安排完成，课后不用做）
### 练习一
通过静态代码分析，举例描述ucore/rcore键盘输入中断的响应过程。

### 练习二
通过静态代码分析，举例描述ucore/rcore系统调用过程，及调用参数和返回值的传递方法。

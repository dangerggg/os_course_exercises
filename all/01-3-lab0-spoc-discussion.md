# lec2：lab0 SPOC思考题

## **提前准备**
（请在上课前完成，option）

- 完成lec2的视频学习
- git pull ucore_os_lab, os_tutorial_lab, os_course_exercises  in github repos。这样可以在本机上完成课堂练习。
- 了解代码段，数据段，执行文件，执行文件格式，堆，栈，控制流，函数调用,函数参数传递，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在不同操作系统（如linux, ucore,etc.)与不同硬件（如 x86, riscv, v9-cpu,etc.)中是如何相互配合来体现的。
- 安装好ucore实验环境，能够编译运行ucore labs中的源码。
- 会使用linux中的shell命令:objdump，nm，file, strace，gdb等，了解这些命令的用途。
- 会编译，运行，使用v9-cpu的dis,xc, xem命令（包括启动参数），阅读v9-cpu中的v9\-computer.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
- 了解基于v9-cpu的执行文件的格式和内容，以及它是如何加载到v9-cpu的内存中的。
- 在piazza上就学习中不理解问题进行提问。

> 已完成

---

## 思考题

- 你理解的对于类似ucore这样需要进程/虚存/文件系统的操作系统，在硬件设计上至少需要有哪些直接的支持？至少应该提供哪些功能的特权指令？

  支持进程的切换需要CPU提供时钟中断的功能，虚存需要CPU提供地址映射的功能，文件系统`需要硬件有稳定的存储介质来保证操作系统的持久性`。特权指令：中断使能，内存管理相关指令，IO操作权限等。

- 你理解的x86的实模式和保护模式有什么区别？你认为从实模式切换到保护模式需要注意那些方面？

  首先实模式和保护模式能访问的地址空间有区别，且实模式和保护模式的寻址方式不同。其次二者段的大小不同，实模式的段的大小是64k而保护模式的段大小则不确定。然而实模式直接访址给系统带来很多的危险，程序可以直接操作内存，因此保护模式提供了基址+偏移的寻址方式，程序不再能够直接操作内存。

- 物理地址、线性地址、逻辑地址的含义分别是什么？它们之间有什么联系？

  物理地址：通过数据总线直接访问内存的二进制地址

  线性地址：处理段内存机制的地址映射，通常是由操作系统虚拟出来的地址空间

  逻辑地址：程序提交给操作系统的访问地址

  关系为程序给出逻辑地址，操作系统将其转换为虚拟地址，如果有内存分页机制，虚拟地址经过转换可以得到物理地址，如果没有内存分页机制，得到的虚拟地址就是物理地址。

- 你理解的risc-v的特权模式有什么区别？不同 模式在地址访问方面有何特征？

  risc-v中有三种特权模式：用户模式，管理模式和机器模式。 机器模式是底层的物理设备访问。用户模式和管理员模式被分别用于传统应用程序和操作系统。

  在机器模式下访问的是物理地址，而用户模式和管理员模式访问的是虚拟地址。实现了 M 和 U 模式的处理器具有一个叫做物理内存保护（PMP，Physical Memory Protection）的功能，允许 M 模式指定 U 模式可以访问的内存地址。与 U 模式一样，S 模式下运行的软件不能使用 M 模式的 CSR 和指令，并且受到 PMP 的限制。

- 理解ucore中list_entry双向链表数据结构及其4个基本操作函数和ucore中一些基于它的代码实现（此题不用填写内容）

- 对于如下的代码段，请说明":"后面的数字是什么含义

  每个结构体中的域的位数。
```
 /* Gate descriptors for interrupts and traps */
 struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
 };
```

- 对于如下的代码段，

```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 1,2,3,0);
```
请问执行上述指令后， intr的值是多少？

值为`0x20003`

### 课堂实践练习

#### 练习一

1. 请在ucore中找一段你认为难度适当的AT&T格式X86汇编代码，尝试解释其含义。

   Init.c中：

   ```Asm
   mov %cs, %0;
   mov %ds, %1;
   mov %es, %2;
   mov %ss, %3;
   ```

   打印了系统在当前的状态信息。

2. (option)请在rcore中找一段你认为难度适当的RV汇编代码，尝试解释其含义。

#### 练习二

宏定义和引用在内核代码中很常用。请枚举ucore或rcore中宏定义的用途，并举例描述其含义。

le2page，to_struct，offsetof：将不同类型的域用双向链表联系起来

SetPageDirty等：常规的用宏来简化代码片段并且提高效率


## 问答题

#### 在配置实验环境时，你遇到了那些问题，是如何解决的。

首先ubuntu18上的qemu并不能很好的支持，最后找了一个github上的版本使用发现没有问题。

其次是IDE的选取上出了问题，我最初选用了Clion，但是Clion在嵌入式开发的调试方面并没有很好，然后赚到eclipse安装调试zylin时发现eclipse版本太高，zylin很久之前就没有更新，并不能用在很高的eclipse版本，最后做了eclipse降级才能使用。

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)

# lec6 SPOC思考题


NOTICE
- 有"w3l2"标记的题是助教要提交到学堂在线上的。
- 有"w3l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。

## 与视频相关思考题

### 6.1	非连续内存分配的需求背景
 1. 为什么要设计非连续内存分配机制？

    解决连续内存分配产生的碎片问题，提高内存分配的效率和内存利用率

    非连续内存可以实现内存的动态管理和分配，更加灵活


 1. 非连续内存分配中内存分块大小有哪些可能的选择？大小与大小是否可变?

    段式管理：内存分块较大，段大小灵活

    页式管理：内存分块较小，一般大小固定，常用4KB


 1. 为什么在大块时要设计大小可变，而在小块时要设计成固定大小？小块时的固定大小可以提供多种选择吗？

    一切都是契合非连续内存分配的灵活性，大小可变为了灵活，设计小块也是为了灵活。但是如果小块大小可变，控制难度就大大提升，所以说这是一个tradeoff。

    小块的固定大小可变，但一旦确定便不再变化。

### 6.2	段式存储管理
 1. 什么是段、段基址和段内偏移？

    段：段式内存管理将分配给进程的物理内存空间按照功能和特权级（访问方式和存储数据）分成一块块大小不同的空间。

    段基址：根据段基址找到对应的段

    段内偏移：找到段之后，通过段内偏移找到逻辑地址对应的物理地址


 1. 段式存储管理机制的地址转换流程是什么？为什么在段式存储管理中，各段的存储位置可以不连续？这种做法有什么好处和麻烦？

    用段号查段表得到段基址，段基址和段内偏移得到物理地址。

    段表支持段的存储位置不连续。

    内存分配高效，管理灵活，但地址映射复杂并且开销大。


### 6.3	页式存储管理
 1. 什么是页（page）、帧（frame）、页表（page table）、存储管理单元（MMU）、快表（TLB, Translation Lookaside Buffer）和高速缓存（cache）？

    页：逻辑地址的基本单位

    帧：物理地址的基本单位

    页表：将页转换为对应的帧的表	

    MMU：将逻辑地址转换为物理地址的计算单元

    TLB：页表的缓存

    cache：利用局部性原理，解决内存访问较慢的问题，将内存的一部分存在cpu中提高访问速度

1. 页式存储管理机制的地址转换流程是什么？为什么在页式存储管理中，各页的存储位置可以不连续？这种做法有什么好处和麻烦？

   用页号查页表得到帧号，帧号和帧内偏移得到物理地址。

   页表的存在使得，页式内存管理变得灵活。

   内存分配高效，管理灵活，但地址映射复杂并且开销大。


### 6.4	页表概述
 1. 每个页表项有些什么内容？有哪些标志位？它们起什么作用？

    标志位和帧号。

    存在位：对应的物理地址是否存在

    修改位：对应的物理地址是否被修改

    引用位：对应的物理地址是否被访问

    只读位：控制读写权限

    特权位：控制用户访问权限

 1. 页表大小受哪些因素影响？

    逻辑地址空间大小，页表的组织方式，进程数目


### 6.5	快表和多级页表
 1. 快表（TLB）与高速缓存（cache）有什么不同？

    TLB是页表的缓存，cache是内存的缓存

 1. 为什么快表中查找物理地址的速度非常快？它是如何实现的？为什么它的的容量很小？

    TLB在CPU中。使用关联存储方式实现。由于TLB在CPU中，因此他的容量收到CPU的功耗和面积限制。

 1. 什么是多级页表？多级页表中的地址转换流程是什么？多级页表有什么好处和麻烦？

    用树的结构组织页表，页表项中存储下一个页表的地址。

    通过多级页表逐级访问，最后加上偏移得到物理地址。

    好处是节省空间（通过标志位标志无效内存），坏处是应对大地址空间时显得比较繁琐。


### 6.6	反置页表
 1. 页寄存器机制的地址转换流程是什么？

    将逻辑地址哈希得到一个哈希值，用哈希值去查询相应的页寄存器，查看二者是否匹配。

 1. 反置页表机制的地址转换流程是什么？

    将进程号和逻辑地址哈希得到一个哈希值，用哈希值去查询页表，查看信息是否匹配。

 1. 反置页表项有些什么内容？

    进程号和页号以及标志位。

### 6.7	段页式存储管理
 1. 段页式存储管理机制的地址转换流程是什么？这种做法有什么好处和麻烦？

    通过段号查到页表，再通过页号查到对应的物理地址。

    可以利用二者的优势，实现灵活的内存管理和内存保护。但是实现起来更为复杂，更浪费资源。

 1. 如何实现基于段式存储管理的内存共享？

    修改段表，使得共享段的段基址相同，即指向同一块内存空间。

 1. 如何实现基于页式存储管理的内存共享？

    修改页表，使得共享的页对应的帧号相同，即指向同一块内存空间。

## 个人思考题
（1） (w3l2) 请简要分析64bit CPU体系结构下的分页机制是如何实现的

应该是采用多级分页方式（4级），维护全局描述符表寄存器（48位）。

详见https://www.cnblogs.com/jiurl/p/4925007.html

## 小组思考题
（1）(spoc) 某系统使用请求分页存储管理，若页在内存中，满足一个内存请求需要150ns (10^-9s)。若缺页率是10%，为使有效访问时间达到0.5us(10^-6s),求不在内存的页面的平均访问时间。请给出计算步骤。

0.9 * 150 + 0.1 * x = 500

（2）(spoc) 有一台假想的计算机，页大小（page size）为32 Bytes，支持32KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries
PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

PTE格式（8 bit） :
```
  VALID | PFN6 ... PFN0
```
PDE格式（8 bit） :
```
  VALID | PT6 ... PT0
```
其
```
VALID==1表示，表示映射存在；VALID==0表示，表示映射不存在。
PFN6..0:页帧号
PT6..0:页表的物理基址>>5
```
在[物理内存模拟数据文件](./03-2-spoc-testdata.md)中，给出了4KB物理内存空间的值，请回答下列虚地址是否有合法对应的物理内存，请给出对应的pde index, pde contents, pte index, pte contents。
```
1) Virtual Address 6c74
   Virtual Address 6b22
2) Virtual Address 03df
   Virtual Address 69dc
3) Virtual Address 317a
   Virtual Address 4546
4) Virtual Address 2c03
   Virtual Address 7fd7
5) Virtual Address 390e
   Virtual Address 748b
```

```
由于计算过程大致相似，选取第一个和最后一个计算：
Virtual Address 6c74:
  --> pde index:0x1b  pde contents:(valid 1, pt 0x20)
    --> pte index:0x3  pte contents:(valid 1, pfn 0x61)
      --> Translates to Physical Address 0xc34 --> Value: 0x6
      
Virtual Address 748b:
  --> pde index:0x1d  pde contents:(valid 1, pt 0x0)
    --> pte index:0x4  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)
```



比如答案可以如下表示： (注意：下面的结果是错的，你需要关注的是如何表示)

```
Virtual Address 7570:
  --> pde index:0x1d  pde contents:(valid 1, pfn 0x33)
    --> pte index:0xb  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)

Virtual Address 21e1:
  --> pde index:0x8  pde contents:(valid 0, pfn 0x7f)
      --> Fault (page directory entry not valid)

Virtual Address 7268:
  --> pde index:0x1c  pde contents:(valid 1, pfn 0x5e)
    --> pte index:0x13  pte contents:(valid 1, pfn 0x65)
      --> Translates to Physical Address 0xca8 --> Value: 16
```

[链接](https://piazza.com/class/i5j09fnsl7k5x0?cid=664)有上面链接的参考答案。请比较你的结果与参考答案是否一致。如果不一致，请说明原因。

（3）请基于你对原理课二级页表的理解，并参考Lab2建页表的过程，设计一个应用程序（可基于python、ruby、C、C++、LISP、JavaScript等）可模拟实现(2)题中描述的抽象OS，可正确完成二级页表转换。

[链接](https://piazza.com/class/i5j09fnsl7k5x0?cid=664)有上面链接的参考答案。请比较你的结果与参考答案是否一致。如果不一致，提交你的实现，并说明区别。

（4）假设你有一台支持[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)的机器，请问你如何设计操作系统支持这种类型计算机？请给出设计方案。

> 对于如何设计操作系统还没有一个很整体的概念，可能这个方案要满足反置页表所规定的内存分配方案，操作系统分配要能够快速的建立这样一个哈希表和反置页表。

 (5)[X86的页面结构](http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring/lecture06)
---

## 扩展思考题

阅读64bit IBM Powerpc CPU架构是如何实现[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)，给出分析报告。


## interactive　understand VM

[Virtual Memory with 256 Bytes of RAM](http://blog.robertelder.org/virtual-memory-with-256-bytes-of-ram/)：这是一个只有256字节内存的一个极小计算机系统。按作者的[特征描述](https://github.com/RobertElderSoftware/recc#what-can-this-project-do)，它具备如下的功能。

 - CPU的实现代码不多于500行；
 - 支持14条指令、进程切换、虚拟存储和中断；
 - 用C实现了一个小的操作系统微内核可以在这个CPU上正常运行；
 - 实现了一个ANSI C89编译器，可生成在该CPU上运行代码；
 - 该编译器支持链接功能；
 - 用C89, Python, Java, Javascript这4种语言实现了该CPU的模拟器；
 - 支持交叉编译；
 - 所有这些只依赖标准C库。

针对op-cpu的特征描述，请同学们通过代码阅读和执行对自己有兴趣的部分进行分析，给出你的分析结果和评价。

> 对于其支持交叉编译的分析。
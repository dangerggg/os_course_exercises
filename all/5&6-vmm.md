# Virtual Memory Management

## 单选题

---

(2012联考)下列关于虚拟存储器的叙述中，正确的是（  ）
- [ ] 虚拟存储只能基于连续分配技术
- [x] 虚拟存储只能基于非连续分配技术
- [ ] 虚拟存储容量只受外存容量的限制
- [ ] 虚拟存储容量只受内容容量的限制

> 采用连续分配方式的时候，会使得相当一部分内存空间都处于空闲状态，造成内存资源的严重浪费，无法从逻辑上扩大内存容量。

（2011年联考）在缺页处理过程中，操作系统执行的操作可能是（  ）
1)修改页表    2)磁盘I/O    3)分配页框
- [ ] 仅1、2
- [ ] 仅2、3
- [ ] 仅1、3
- [x] 1、2、3

> 如果还有可分配给程序的内存，那么会分配新的页框，修改页表，从磁盘读取内容放入到分配的页框中。

（2013计算机联考）若系统发生抖动(Thrashing)时，可用采取的有效措施是（  ）
1）撤销部分进程    2）增加磁盘交换区的容量    3）提高用户进程的优先级
- [x] 仅1
- [ ] 仅2
- [ ] 仅3
- [ ] 仅1、2

> 撤销部分进程可以增大可用内存，减少抖动。而磁盘交换区容量和进程优先级则跟抖动无关。

(南昌大学)一个虚拟存储器系统中，主存容量16MB，辅存容量1GB，地址寄存器位数32位。那么虚存最大容量为（  ）
- [ ] 1GB
- [ ] 16MB
- [ ] 1GB + 16MB
- [x] 4GB

> 虚拟存储器的最大容量跟虚拟地址空间有关，是2^32。

（上海交通大学）分页式虚拟存储管理系统中，分页是（  ）实现的
- [ ] 程序员
- [ ] 编译器
- [ ] 系统调用
- [x] 系统

> 分页是系统内部实现的，对用户透明。

为了使得内存需求较大的程序能够正常运行，常需要通过外存和内存的交换技术，这被叫做____技术
- [ ] 虚拟机
- [ ] 内存分配
- [ ] 进程调度
- [x] 虚拟存储

> 解释：虚拟机用于模拟真实物理机器，单独的内存分配技术可以不考虑使用外存，进程调度则用于管理进程的执行时间和次序等。虚拟存储是指当真实内存不能满足需求的时候，可以将程序需要的代码和数据放到内存中，暂时不需要的放到外存上；通过内存和外存的不断交换，来满足程序的运行需求。

虚拟内存是为了应对____的问题（）
- [ ] 内存访问速度过慢
- [ ] 内存管理困难
- [x] 内存容量不满足程序需求
- [ ] 磁盘访问过慢

> 解释：虚拟内存是应对内存容量不能满足程序需求的情况，并不能解决内存内存和外存访问速度的问题。

一般来讲，虚拟内存使得程序的运行速度____
- [ ] 加快
- [ ] 不变
- [x] 变慢
- [ ] 变得极不稳定

> 解释：由于虚拟内存有可能造成外存和内存的不断交换，虽然能够满足大程序的运行需求，但是程序的运行速度相比没有虚拟内存的情况下会变慢。

虚拟内存常用的页面淘汰技术，主要利用了程序的____特征
- [ ] 健壮性
- [ ] 完整性
- [x] 局部性
- [ ] 正确性

> 解释：程序的局部性是指程序呈现在某段时间内只访问程序的某一部分代码和数据的特性，而页面置换算法可以利用这一特性使常被访问的页面不被淘汰也就减少了缺页率。

在一个系统中，页面大小设定为4k，分配给每个进程的物理页面个数为1，在某应用程序中需要访问一个int[1024][1024]的数组（逐行访问），那么按行存储和按列存储的不同情况下，____
- [x] 按行存储时，执行效率高
- [ ] 按列存储时，执行效率高
- [ ] 执行效率相同
- [ ] 执行效率不确定

> 解释：按行存储的时候，每访问一行才会出现一次页面置换；而按列存储，则每访问一次就发生一次缺页。由于缺页的时候需要调用页面置换算法进行内外存交换，所以缺页率高的时候效率就低。

虚拟内存技术____
- [ ] 只能应用于分段系统
- [ ] 只能应用于分页系统
- [x] 可应用于分段系统、分页系统
- [ ] 只能应用于段页式系统

> 解释：虚拟内存技术是一种内外存交换的思想，可应用与分段系统、分页系统等。而目标系统是分段系统还是分页系统，影响的只是虚拟内存技术思想的具体实现。

在虚拟页式内存管理系统中，页表项中的‘访问位’给____提供参考价值。
- [ ] 分配页面
- [x] 页面置换算法
- [ ] 换出页面
- [ ] 程序访问

> 解释：页面置换算法可能需要根据不同页面是否被访问，访问时间和访问频率等进行淘汰页面的选择。

在虚拟页式内存管理系统中，页表项中的‘修改位’供____使用
- [ ] 分配页面
- [ ] 页面置换算法
- [x] 换出页面
- [ ] 程序访问

> 解释：页面换出的时候，需要判断外存上的相应页面是否需要重写。如果内存中该页面在使用期间发生了修改，则相应的修改位被设置，用于换出的时候通知操作系统进行外存相应页面的修改。

在虚拟页式内存管理系统中，页表项中的____供程序访问时使用
- [ ] 访问位
- [ ] 修改位
- [x] 状态位
- [ ] 保护位

> 解释：页表项的状态位用于指示该页是否已经调入内存，供程序访问时使用，如果发现该页未调入内存，则产生缺页中断，由操作系统进行相应处理。

在虚拟页式内存管理系统中，发生缺页的概率一般取决于____
- [ ] 内存分配算法
- [ ] 内存读取速度
- [ ] 内存写入速度
- [x] 页面置换算法

> 解释：缺页率的高低跟实际能分配的物理内存的大小，以及系统中的页面置换算法相关。差的页面置换算法可能造成需要访问的页面经常没有在内存中，而需要进行缺页中断处理。

页面置换算法的优劣，表现在____
- [ ] 程序在运行时能够分配到的页面数
- [ ] 单位时间内，程序在运行时得到的CPU执行时间
- [x] 程序在运行时产生的页面换入换出次数
- [ ] 程序本身的访存指令个数

> 解释：页面置换算法在满足程序运行需求的同时，应尽量降低页面的置换次数，从而降低运行开销。

选择在将来最久的时间内不会被访问的页面作为换出页面的算法叫做____
- [x] 最优页面置换算法
- [ ] LRU
- [ ] FIFO
- [ ] CLOCK

> 解释：LRU是换出在过去的时间里最久未被访问的页面；FIFO是换出最先被换入的页面；CLOCK类似于LRU，也是对FIFO的改进。但是以上三种算法都是根据过去一段时间内的页面访问规律进行换出页面的选择。而最优页面置换算法是指换出将来在最久的时间内不会被访问的页面，是一种理想情况也是不可能实现的。

Belady异常是指____
- [ ] 频繁的出页入页现象
- [x] 分配的物理页数变多，缺页中断的次数却增加
- [ ] 进程的内存需求过高，不能正常运行
- [ ] 进程访问内存的时间多于读取磁盘的时间

> 解释：一般情况下，分配的物理页数越多，缺页率会越低。但是某些页面置换算法如FIFO就可能造成相反的情况，也即分配的物理页数增多，缺页率却增高的情况。这种情况称为Belady异常。

在各种常见的页面置换算法中，____会出现Belady异常现象
- [x] FIFO
- [ ] LRU
- [ ] LFU
- [ ] CLOCK

> 解释：FIFO可能出现Belady异常，如访问顺序1,2,3,4,1,2,5,1,2,3,4,5，在最多分配3个物理块的情况下缺页9次，而在最多分配4个物理块的情况下缺页10次。

当进程访问的页面不存在，且系统不能继续给进程分配物理页面的时候，系统处理过程为____
- [ ] 确定换出页面->页面换出->页面换入->缺页中断
- [ ] 缺页中断->页面换入->确定换出页面->页面换出
- [ ] 缺页中断->确定换出页面->页面换入->页面换出
- [x] 缺页中断->确定换出页面->页面换出->页面换入

> 解释：首先在程序访问的时候发现页面不在内存中，从而发出缺页中断，进入页面置换的流程。需要确定换出页面才能执行页面交换，而页面换入之前要保证页面已经正确的换出，因为页面换出可能需要重写外存中相应的页面。

某进程的页面访问顺序为1、3、2、4、2、3、1、2，系统最多分配3个物理页面，那么采用LRU算法时，进程运行过程中会发生____缺页。
- [ ] 三次
- [ ] 四次
- [x] 五次
- [ ] 六次

> 解释：1（缺页） - 3（缺页） - 2（缺页） - 4（缺页，换出1） - 2 - 3 - 1（缺页，换出4） - 2

在现代提供虚拟内存的系统中，用户的逻辑地址空间____
- [ ] 不受限制
- [ ] 受物理内存空间限制
- [ ] 受页面大小限制
- [x] 受指令地址结构

> 解释：逻辑地址空间受到逻辑地址的结构限制，也即为指令地址的结构限制。

---

## 多选题

---

以下哪些页面置换算法是可以实现的____
- [ ] 最优页面置换算法
- [x] LRU
- [x] FIFO
- [x] CLOCK

> 解释：最优页面置换算法是根据将来的页面访问次序来选择应该换出的页面，因为在程序执行之前不可能已知将来的页面访问次序，所以不可能实现。而其它的页面置换算法则是根据已经发生的页面访问次序来决定换出的页面，都是可以实现的。

影响缺页率的因素有____
- [x] 页面置换算法
- [x] 分配给进程的物理页面数
- [x] 页面本身的大小
- [x] 程序本身的编写方法

> 解释：总体来讲，缺页率的主要影响因素的页面置换算法和分配给进程的物理页面数。但是页面本身的大小和程序本身的编写方法则涉及到页面访问次序的变化，对缺页率也会造成影响。

---

## 判断题

---

发生缺页的时候，一定会使用页面置换算法____
- [ ] 对
- [x] 错

> 解释：发生缺页的时候，如果分配给程序的物理页面数还有空闲，则直接换入新的页面，不需要使用页面置换算法来挑选需要换出的页面。

---
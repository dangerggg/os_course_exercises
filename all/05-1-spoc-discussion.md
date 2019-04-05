#lec11: 进程／线程概念spoc练习

## 视频相关思考题

### 11.1 进程的概念

1. 什么是程序？什么是进程？

   进程是指一个具有独立功能的**程序**在一个数据集上的一次动态执行过程。

   程序是我们编码出来的死的文件，位于外存中，操作系统把程序加载到内存中并分配一定的资源使其开始执行，这就是进程。


2. 进程有哪些组成部分？

   代码，数据，寄存器，堆栈，打开的文件资源，分配的内存资源


3. 请举例说明进程的独立性和制约性的含义。

   独立性：进程之间的资源相互独立，相互不可直接访问

   制约性：进程之间可以通过内核进行通信

   为了保护进程的数据安全，进程之间独立。但是某个进程可能需要其他进程产生的数据，这就涉及到了进程之间的通信。


4. 程序和进程联系和区别是什么？

   程序是组成进程的重要部分。

   进程是动态执行的程序且程序的每次执行对应不同的进程。


### 11.2 进程控制块

1. 进程控制块的功能是什么？

   记录进程运行的状态和信息，方便操作系统进行管理


2. 进程控制块中包括什么信息？

   进程标识符：PID，UID

   进程控制信息：调度优先级，打开的文件资源

   进程状态信息：SP，IP，内存资源


3. ucore的进展控制块数据结构定义中哪些字段？有什么作用？

   ```C
   struct proc_struct {
       enum proc_state state;        // Process state
       int pid;                      // Process ID
       int runs;                     // the running times of Proces
       uintptr_t kstack;             // Process kernel stack
       volatile bool need_resched;   // bool value: need to be rescheduled to release CPU?
       struct proc_struct *parent;   // the parent process
       struct mm_struct *mm;         // Process's memory management field
       struct context context;       // Switch here to run process
       struct trapframe *tf;         // Trap frame for current interrupt
       uintptr_t cr3;           // CR3 register: the base addr of Page Directroy Table(PDT)
       uint32_t flags;               // Process flag
       char name[PROC_NAME_LEN + 1]; // Process name
       list_entry_t list_link;       // Process link list 
       list_entry_t hash_link;       // Process hash list
   };
   ```
### 11.3 进程状态

1. 进程生命周期中的相关事件有些什么？它们对应的进程状态变化是什么？

   进程的创建，运行，等待，抢占，结束，唤醒；分别对应就绪态，就绪->运行，运行->等待，运行->就绪，等待->就绪


### 11.4 三状态进程模型

1. 运行、就绪和等待三种状态的含义？7个状态转换事件的触发条件是什么？

   运行：在处理机中运行

   就绪：只差处理机资源即可运行

   等待：需要某种资源或者事件才可以继续执行


### 11.5 挂起进程模型

1. 引入挂起状态的目的是什么？

   使得更多的内存空间被使用，可以支持更多的进程

2. 引入挂起状态后，状态转换事件和触发条件有什么变化？

   增加了等待挂起和就绪挂起状态，增加等待->等待挂起、就绪->就绪挂起、运行->就绪挂起三个转换

3. 内存中的什么内容放到外存中，就算是挂起状态？

   > 进程的内核栈？

### 11.6 线程的概念

1. 引入线程的目的是什么？

   提高程序的并行度，提高数据共享的效率，并且线程的创建和切换速度都要优于进程

2. 什么是线程？

   线程是进程的一部分，描述指令流执行状态。它是进程中的指令执行流的最小单元，是CPU调度的基本单位

3. 进程与线程的联系和区别是什么？

   进程是资源的分配单元，一个进程可以有多个线程，每个线程有自己独立的栈和寄存器，但共享进程的其他资源。

### 11.7 用户线程

1. 什么是用户线程？

   在用户态用函数库支持的多线程，对操作系统不可见

2. 用户线程的线程控制块保存在用户地址空间还是在内核地址空间？

   用户地址空间，操作系统不可见


### 11.8 内核线程

1. 用户线程与内核线程的区别是什么？

   线程的管理单元实现的位置。以及一系列由此造成的不同：开销，TCB管理等

2. 同一进程内的不同线程可以共用一个相同的内核栈吗？

   内核线程独立，但是用户线程可同时使用

3. 同一进程内的不同线程可以共用一个相同的用户栈吗？

   不行


## 选做题
1. 请尝试描述用户线程堆栈的可能维护方法。

## 小组思考题
(1) 熟悉和理解下面的简化进程管理系统中的进程状态变化情况。
 - [简化的三状态进程管理子系统使用帮助](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep7-process-run.md)
 - [简化的三状态进程管理子系统实现脚本](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep7-process-run.py)

(2) (spoc)设计一个简化的进程管理子系统，可以管理并调度如下简化进程。在理解[参考代码](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab4/process-concept-homework.py)的基础上，完成＂YOUR CODE"部分的内容。然后通过测试用例和比较自己的实现与往届同学的结果，评价自己的实现是否正确。可２个人一组。

### 进程的状态 

 - RUNNING - 进程正在使用CPU
 - READY   - 进程可使用CPU
 - DONE    - 进程结束

### 进程的行为
 - 使用CPU, 
 - 发出YIELD请求,放弃使用CPU


### 进程调度
 - 使用FIFO/FCFS：先来先服务,
   - 先查找位于proc_info队列的curr_proc元素(当前进程)之后的进程(curr_proc+1..end)是否处于READY态，
   - 再查找位于proc_info队列的curr_proc元素(当前进程)之前的进程(begin..curr_proc-1)是否处于READY态
   - 如都没有，继续执行curr_proc直到结束

### 关键模拟变量
 - 进程控制块
```
PROC_CODE = 'code_'
PROC_PC = 'pc_'
PROC_ID = 'pid_'
PROC_STATE = 'proc_state_'
```
 - 当前进程 curr_proc 
 - 进程列表：proc_info是就绪进程的队列（list），
 - 在命令行（如下所示）需要说明每进程的行为特征：（１）使用CPU ;(2)等待I/O
```
   -l PROCESS_LIST, --processlist= X1:Y1,X2:Y2,...
   X 是进程的执行指令数; 
   Ｙ是执行CPU的比例(0..100) ，如果是100，表示不会发出yield操作
```
 - 进程切换行为：系统决定何时(when)切换进程:进程结束或进程发出yield请求

### 进程执行
```
instruction_to_execute = self.proc_info[self.curr_proc][PROC_CODE].pop(0)
```

### 关键函数
 - 系统执行过程：run
 - 执行状态切换函数:　move_to_ready/running/done　
 - 调度函数：next_proc

### 执行实例

#### 例１
```
$./process-simulation.py -l 5:50
Process 0
  yld
  yld
  cpu
  cpu
  yld

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN YIELD
Time     PID: 0 
  1     RUN:yld 
  2     RUN:yld 
  3     RUN:cpu 
  4     RUN:cpu 
  5     RUN:yld 

```


#### 例２
```
$./process-simulation.py  -l 5:50,5:50
Produce a trace of what would happen when you run these processes:
Process 0
  yld
  yld
  cpu
  cpu
  yld

Process 1
  cpu
  yld
  cpu
  cpu
  yld

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN YIELD
Time     PID: 0     PID: 1 
  1     RUN:yld      READY 
  2       READY    RUN:cpu 
  3       READY    RUN:yld 
  4     RUN:yld      READY 
  5       READY    RUN:cpu 
  6       READY    RUN:cpu 
  7       READY    RUN:yld 
  8     RUN:cpu      READY 
  9     RUN:cpu      READY 
 10     RUN:yld      READY 
 11     RUNNING       DONE 
```

## 2.1 系统级架构总览

系统级架构由一系列寄存器（registers）、数据结构（data structure）、指令（instruction）组成，旨在支持基本的系统级操作，例如内存管理（memory management）、中断（interrupt）、作业管理（task management）还有多处理机控制（control of multiple processors）。

- IA-32 系统级寄存器及数据结构

- IA-32e 模式和 4 级分页的系统级寄存器和数据结构

### 2.1.1 全局和本地描述符表（GDT、LDT）

在使用保护模式的时候，所有内存访问都经过全局描述符表（global description table）或者可选的本地描述符表（local description table）。这些表包含叫段描述符（segment descriptors）的条目。段描述符包含段基地址、访问权限、类型以及使用情况信息。

每一个段描述符（segment descriptor）对应一个段选择子（segment selector）。段选择子为使用它的软件提供 GDT 或 LDT 的索引（其对应的段描述符的偏移量）和一个全局/局部的标志（决定段选择子指向的是 GDT 还是 LDT）、以及访问权限信息。

想要访问段中段一个字节，需要提供一个段选择子和一个偏移量（offset）。段选择子能够访问关于该段的段描述符。

- 处理器在线性地址空间为该段分配了一个基地址，并写在段描述符中。

- 偏移量提供了错所在字节对于基地址的偏移。

该机制可以用来访问任何有效段代码段（valid code）、数据段（data）或者栈段（stack segment），只要这些段能够在当前正在运行的处理机的 CPL （current privilege level） 中是可访问的。

CPL 用于定义当前执行代码段的保护级别。

GDT 的线性基地址存储在 GDT 寄存器（GTDR）中，LDT 的线性基地址存储在 LDT 寄存器（LDTR）中。

### 2.1.2 系统段、段描述符以及门（Gates）

除了代码段、数据段、栈段能够构成程序和过程的执行环境，本架构还定义了两个系统段：作业状态段（task-state segment TSS）和 LDT。GDT 不是一种段，因为它不是通过段选择子和段描述符进行访问的，而 TSS 和 LDT 有为它们定义的段描述符。

本架构还定义了一系列特殊的描述符，叫做门（gates）。有调用门（call gates）、中断门（interrupt gates）、Trap 门（trap gates）以及作业门（task gates）。它们为系统过程和处理程序提供了受保护的网关，这些过程和处理程序应该在不同的特权级别上运行，而不是应用程序和大多数过程。

### 2.1.3 作业状态段和作业门



### 2.1.4 中断及异常处理



### 2.1.5 内存管理



### 2.1.6 系统寄存器

为了协助初始化处理器和控制系统（controlling system operations）操作，本系统架构在 EFLAGS 寄存器和几个系统寄存器中提供系统标志（system flags）：

- EFLAGE 寄存器：
- 控制寄存器（control register）（CR0, CR2, CR3, CR4）
- 调试寄存器（debug register）
- GDTR、LDTR、IDTR 寄存器：包含其各自表的线性基地址及大小（上限）
- 作业寄存器（task registers）
- 模式寄存器（model-spec registers MSR）

### 2.1.7 其它系统资源

- 操作系统指令（operations system instructions）
- 性能监控计数器（performance-monitoring counters）
- 内部缓存（cache）及缓冲（buffer）

性能监控技术器是一个信号计数器，可以用来统计处理器信号，例如解码了多少条指令、收到了多少条中断或者读取了多少缓存。

处理器提供了数个内部缓存及缓冲。缓存用于存储数据和指令。缓冲区用于存储系统和应用程序段的解码地址，以及等待执行的写操作。



## 2.2 操作模式

- 保护模式
- 实模式
- 系统管理模式
- Virtual 8086 模式



处理器操作模式切换：

### 2.2.1 使能寄存器

IA32_EFER MSR 提供了一些关于 IA-32e 模式的激活和操作。也提供了一个关于页访问权限修改的字段。下面是 IA32_EFER MSR 的布局图：

## 2.3 EFLAG 寄存器中的系统标识和字段

EFLAGS 寄存器的 system flags 和 IOPL 字段、控制 I/O、可屏蔽硬件中断、调试、任务切换和 Virtual-8086 模式，只有特权代码才能够修改这些位。


**TF：**

Trap（第 8 位）—— 若为 1，则启用单步执行模式（single-step mode）用于调试；若为 0，则禁用单步执行模式。在单步执行模式时，处理器会在每个指令后生成一个调试异常（debug exception）。

若有一个应用程序使用 `POPF`、`POPFD`、`IRET` 指令将 TF 标志设为 1，则将会在  `POPF`、`POPFD`、`IRET` 之后的指令后面生成一个调试异常。

**IF:**

Interrupt enable（第 9 位）—— 控制处理器对可屏蔽硬件中断请求的响应。若该值被设定，可响应可屏蔽硬件中断；若该值被清除，禁止可屏蔽硬件中断。该标志对于异常生成或不可屏蔽中断（nonmaskable interrupts NMI）无效。

CPL、IOPL 和 VME（在 CR4 寄存器中）标志的状态决定了 IF 标志位是否可以被 `CLI`、`STI`、`POPF`、`POPFD` 和 `IRET` 指令修改。

**IOPL:**

I/O privilege level field（第 12、13 位）—— 表明当前运行程序或作业的 I/O 权限等级。要访问 I/O 地址空间，当前程序或作业的 CPL 必须小于等于 IOPL 的值。`POPF` 和 `IRET` 指令可以修改该字段的值（仅当 CPL 为 0）。

当虚扩展模式启用时，IOPL 也是控制 IF 标志的修改和 Virtual-8086 模式中断处理的机制之一。

**NT:**

Nested task（第 14 位）——

**RF:**



**VM:**



**AC:**



**VIP:**



**ID:**



## 2.4 内存管理寄存器

处理器提供了四个内存管理寄存器（GTDR、LDTR、IDTR、TR），这些寄存器指定了控制分段内存管理的数据结构的位置。一些特殊指令可用于装入和存储这些寄存器。

![截屏2021-06-19 下午4.50.21](/Users/harvey/Library/Application Support/typora-user-images/截屏2021-06-19 下午4.50.21.png)

### 2.4.1 全局描述符表寄存器（GTDR）



### 2.4.2 局部描述符表寄存器（LDTR）



### 2.4.3 IDTR 中断描述符表寄存器



### 2.4.4 作业寄存器（TR）





## 2.5 控制寄存器



## 2.6 扩展控制寄存器



## 2.8 系统指令概要
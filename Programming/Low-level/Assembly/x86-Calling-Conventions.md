# 两种函数调用处理方式
- [调用者清理](https://zh.wikipedia.org/wiki/X86%E8%B0%83%E7%94%A8%E7%BA%A6%E5%AE%9A#%E8%B0%83%E7%94%A8%E8%80%85%E6%B8%85%E7%90%86)
- [被调用者清理](https://zh.wikipedia.org/wiki/X86%E8%B0%83%E7%94%A8%E7%BA%A6%E5%AE%9A#%E8%A2%AB%E8%B0%83%E7%94%A8%E8%80%85%E6%B8%85%E7%90%86)

# 32 位调用规约

## cdecl

> cdecl（C declaration，即 C 声明）是源起 [C 语言](https://zh.wikipedia.org/wiki/C%E8%AF%AD%E8%A8%80 "C语言") 的一种调用约定，也是 C 语言的事实上的标准。

其内容包括：

1.  函数实参在线程栈上按照从右至左的顺序依次压栈；
2.  函数结果保存在寄存器 `EAX`/`AX`/`AL` 中；
3.  浮点型结果存放在寄存器 `ST0` 中；
4.  编译后的函数名前缀以一个下划线字符；
5.  调用者负责从线程栈中弹出实参（即清栈）；
6.  8bits 或者 16bits 长的整形实参提升为 32 bits；
7.  受到函数调用影响的寄存器（volatile registers）：`EAX`, `ECX`, `EDX`, `ST0` - `ST7`, `ES`, `GS`；
8.  不受函数调用影响的寄存器： `EBX`, `EBP`, `ESP`, `EDI`, `ESI`, `CS`, `DS`；
9.  `RET` 指令从函数被调用者返回到调用者（实质上是读取寄存器 `EBP` 所指的线程栈之处保存的函数返回地址并加载到 `IP` 寄存器）。


# 64 位调用规约

## Microsoft

*此约定主要在微软 OS 如 Windows 上使用。*

前四个整型参数存放在寄存器 `RCX`, `RDX`, `R8`, `R9` 上；同时寄存器 `XMM0` 到 `XMM3` 用来传递浮点参数。其余参数逆序压栈（从右往左），返回值放在 `RAX` 中。

## System V

*此约定主要在 Solaris，GNU/Linux，FreeBSD 和其他非微软 OS 上使用。*

前六个整型参数放在寄存器 `RDI`, `RSI`, `RDX`, `RCX`, `R8` 和 `R9` 上；同时 `XMM0` 到 `XMM7` 用来放置浮点参数。其余额外参数逆序入栈（从右往左），返回值同微软放在 `RAX` 中。

在系统调用中（详情：[[Syscall-Overview]]）使用寄存器 `R10` 替换 `RCX`。






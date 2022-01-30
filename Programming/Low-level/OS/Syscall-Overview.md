# 系统调用表

- [Chromium OS Docs - Linux System Call Table (googlesource.com)](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/constants/syscalls.md)
- [linux/syscall_64.tbl at master · torvalds/linux (github.com)](https://github.com/torvalds/linux/blob/master/arch/x86/entry/syscalls/syscall_64.tbl)
- [[Common-Syscall]] 常用系统调用

# 使用

## Assemble

```x86
mov 
```

## C/C++

```c
#include<unistd.h>
#include<sys/syscall.h>

// 方法签名
long syscall(long number, ...);
```

参数 `number` 为系统调用号，可以直接使用 `sys/syscall.h` 头文件内的宏常量。后面依次跟上系统调用所需的参数。

## Go

```go
import "syscall"

// 方法签名
func Syscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)  
func Syscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)  
func RawSyscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)  
func RawSyscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)
```

## Rust


## Python


# 寄存器

在 AT&T 格式中，寄存器名需要加上 `%` 前缀；而在 Intel 格式中，寄存器不需要添加前缀。

| AT&T         | Intel      |
| ------------ | ---------- |
| `pushl %eax` | `push eax` |

# 立即数

在 AT&T 格式中，立即数需要加上 `$` 前缀；而在 Intel 格式中，立即数的表示不需要添加前缀。

| AT&T       | Intel    |
| ---------- | -------- |
| `pushl $1` | `push 1` | 

# 操作数顺序

在 AT&T 格式中，目标操作数在源操作数的右边；而在 Intel 格式中，则目标操作数在源操作数左边。

| AT&T           | Intel        |
| -------------- | ------------ |
| `add $1, %eax` | `add eax, 1` |

# 操作数字长

在 AT&T 格式中，操作数的字长由指令最后一个字母决定，如 `b`、`w`、`l`，分别对应字节（byte, 8bits）、字（word, 16bits）、长字（long word, 32bits）；而在 Intel 格式中，操作数的字长是由 `byte ptr`、`word ptr` 等前缀表示的。

| AT&T            | Intel                  |
| --------------- | ---------------------- |
| `movb val, %al` | `mov al, byte ptr val` |

# 绝对转移和调用

在 AT&T 汇编格式中，绝对转移和调用指令（jump/call）的操作数前要加上 `*` 作为前缀，而在 Intel 格式中则不需要。

远程转移指令和远程子调用指令的操作码，在 AT&T 汇编格式中为 `ljump` 和 `lcall`，而在 Intel 汇编格式中则为 `jmp far` 和 `call far`，即：

| AT&T                      | Intel                     |
| ------------------------- | ------------------------- |
| `ljump $section, $offset` | `jmp far section:offset`  |
| `lcall $section, $offset` | `call far section:offset` |

与之相应的远程返回指令则为：

| AT&T                 | Intel                  |
| -------------------- | ---------------------- |
| `lret $stack_adjust` | `ret far stack_adjust` |

# 寻址格式

在 AT&T 汇编格式中，内存操作数的寻址方式是   

```gas
section:disp(base, index, scale)
```

而在 Intel 汇编格式中，内存操作数的寻址方式为：   

```nasm
section:[base + index*scale + disp]
```  

内存操作数相关的示例：

| AT&T                             | Intel                           |
| -------------------------------- | ------------------------------- |
| `movl -4(%ebp), %eax`            | `mov eax, [ebp - 4]`            |
| `movl array(, %eax, 4), %eax`    | `mov eax, [eax*4 + array]`      |
| `movw array(%ebx, %eax, 4), %cx` | `mov cx, [ebx + 4*eax + array]` |
| `movb $4, %fs:(%eax)`            | `mov fs:eax, 4`                 |


# 程序示例

```gas
# AT&T 格式
.data
    msg : .string "Hello, world!//n"
    len = . - msg

.text
.global _start

_start:
    # 打印 "Hello, world!"
    movl $len, %edx
    movl $msg, %ecx
    movl $1, %ebx
    movl $4, %eax
    int  $0x80

    # 退出程序
    movl $0,%ebx
    movl $1,%eax 
    int  $0x80
```

```nasm
; Intel 格式
section .data
    msg db "Hello, world!", 0xA
    len equ $ - msg

section .text
global _start

_start:
    ; 打印 "Hello, World!"
    mov edx, len
    mov ecx, msg
    mov ebx, 1
    mov eax, 4
    int 0x80

    ; 退出程序
    mov ebx, 0
    mov eax, 1  
    int 0x80
```
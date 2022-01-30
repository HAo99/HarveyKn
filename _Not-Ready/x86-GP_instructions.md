## 数据传送

- MOV/MOVBE/MOVSX/MOVZX/MOVD/MOVNTI：传送
- CMOV*cc*：条件传送
- POP/POPA/POPAD：出栈
- PUSH/PUSHA/PUSHAD：压栈
- ENTER：创建规程栈帧
- LEAVE：删除规程栈帧

## 地址读取

- LEA：获取有效地址

## 算术运算

- ADC/ADD：加法
    - ADC：带进位加法
    - ADD：带符号/无符号加法
- SBB/SUB：减法
    - SBB：带借位减法
    - SUB：带符号/无符号减法
- NEG：符号取反
- MUL/IMUL：乘法
    - MUL：无符号乘法
    - IMUL：带符号乘法
- DIV/IDIV：除法
    - DIV：无符号除法
    - IDIV：有符号除法
- INC：自增
- DEC：自减

## 移位运算

- SHL/SHR/SAL/SAR：按位移动
    - SHL：逻辑左移
    - SHR：逻辑右移
    - SAL：算术左移
    - SAR：算术右移
- RCL/RCR/ROL/ROR：按位旋转
    - RCL：带进位左旋转
    - RCR：带进位右旋转
    - ROL：左旋转
    - ROR：右旋转


## 比较和测试

- CMP：比较
- TEST：测试


## 逻辑运算

- AND：与运算
- OR：或运算
- XOR：异或运算
- NOT：非运算
- ANDN：与非运算

## 控制转移

- JMP：跳转
- J*cc*：条件跳转
- LOOP*cc*：条件循环
- CALL：规程调用
- RET：规程返回

## 中断和异常

- INT：触发软中断
- INTO
- IRET/IRETD/IRETQ

## 通用 IO

- IN：输入
- OUT：输出

## 信号量

- CMPXCHG：
- XADD：
- XCHG：

## 空操作

- NOP

## 系统调用

- SYSENTRY
- SYSEXIT
- SYSCALL
- SYSRET

## 缓存及内存管理

- LFENCE：读取屏障
- SFENCE：存储屏障
- MFENCE：内存屏障
- PREFETCH*level*
- PREFETCH
- PREFETCHW
- CLFLUSH：失效缓存行
- CLWB：缓存行写回



# File & I/O

- read(0)：从文件描述符中读取内容
- write(1)：从文件描述符中写入内容
- open(2)：打开文件
- close(3)：关闭文件
- lseek(8)：修改文件描述符读取的偏移量
- ioctl(16)：通用 IO 模型以外的操作
- pread64(17)：到指定偏移处进行内容读取，与当前偏移量无关，也不会改变当前偏移量
- pwrite64(18)：到指定偏移处进行内容写入，与当前偏移量无关，也不会改变当前偏移量
- readv(19)：将多个位置的内容读取集中成一次调用
- writev(20)：将多个位置的内容写入集中成一次调用
- access(21)：检查对文件的访问权限
- truncate(76)：文件截断，丢弃指定文件中超出 length 部分的内容
- ftruncate(77)：传入 fd 版本的 truncate
- getdents(78)：读取当前目录下的文件信息
- getcwd(79)：读取当前的工作目录路径
- chdir(80)：修改当前工作目录
- fchdir(81)：传入 fd 版本的 chdir
- rename(82)：重命名文件
- mkdir(83)：创建目录
- rmdir(84)：删除目录
- creat(85)：创建文件（可用 open 系统调用代替）
- link(86)：创建硬链接
- unlink(87)：删除硬连接，如果当前文件是最后一个节点，还会删除文件本身
- symlink(88)：创建符号链接
- readlink(89)：读取符号链接本身的内容，而不是所指向的文件
- chmod(90)：修改文件权限
- fchmod(91)：传入 fd 版本的 chmod
- chown(92)：修改文件属主
- fchown(93)：传入 fd 版本的 chmod
- lchown(94)： lchown()用途与 chown()相同，不同之处在于若参数 pathname 为符号链接，则将会改变链接文件本身的所有权，而与该链接所指代的文件无干。
- umask(95)：为进程的文件模式创建掩码
- utime(132)：修改文件的时间戳
- utimes(235)：用途与 utime 一致，但时间戳精度更高
- inotify_init(253)：创建一个 inotify 实例
- inotify_add_watch(254)：为 inotify 添加要监控的文件及其事件
- inotify_rm_watch(255)：删除 inotify 中的监控条目


# Memory

- mmap(9)：内存分配
- brk(12)：内存分配（调整堆的 Program Break）


# Process & Thread

- fork(57)：允许一（父）进程创建一个新的（子）进程
- execve(59)：加载一个新的程序
- exit(60)：终止进程
- wait4(61)：等待进程状态变更
- kill(62)：向进程发送信号

# Infomation

- uname(63)：查看当前系统架构

# IPC

## Message Queue

## Semaphore

## Shared Memory
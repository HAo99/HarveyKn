# 区块

区块（原文：Section），用于区分不同种类的手册页。手册页中的标题如 `read(2)`，其括号内的 2 即表示区块号，可用于区分同名的命令和函数。标准的手册一般分为 8 个区块：

| 区块 | 内容                 |
| ---- | -------------------- |
| 1    | 通用指令             |
| 2    | 系统调用             |
| 3    | C 标准库             |
| 4    | 特殊文件及驱动       |
| 5    | 文件格式和约定       |
| 6    | 游戏与屏幕保护       |
| 7    | 杂项                 |
| 8    | 系统管理员命令及服务 |

某些系统可能会包含额外区块：

| 区块 | 内容          |
| ---- | ------------- |
| 0    | C 库头文件    |
| 1    | 内核常规      |
| l    | LAPACK 库函数 |
| n    | Tcl/Tk 命令   |
| x    | X Window 相关 |

某些区块还会有特殊后缀进行区分（如 `setsockopt(3p)`）：

| 后缀 | 内容          |
| ---- | ------------- |
| p    | POSIX 规定    |
| x    | X Window 相关 |

# 布局

**NAME**

命令或函数的名称。

**SYNOPSIS**

- 若为命令，展示如何运行它以及它需要的命令行选项的正式描述；
- 若为函数，展示函数的签名及其所在的头文件。

**DESCRIPTION**

命令或函数的功能的文本描述。

**EXAMPLES**

一些常用的代码示例。

**SEE ALSO**

相关函数或命令的列表。

> 仍有其他部分未列出，因为这些部分并没有很好地被标准化。
> 例如：OPTIONS, EXIT STATUS, RETURN VALUE, ENVIRONMENT, BUGS, FILES, AUTHOR, REPORTING BUGS, HISTORY 以及 COPYRIGHT

# 命令用法

如果你要查阅一个命令的手册页，可以使用如下命令：

```bash
$ man <command_name>
# 示例
$ man echo
```

但该命令通常会显示有关于***命令***的手册页，而你有可能想查阅的是与这个命令重名的系统调用，可以使用区块号来进行约束：

```bash
$ man <section_number> <command_name/function_name>
# 示例
$ man 2 read
```

# 在线查阅

- [Linux man pages online (man7.org)](https://man7.org/linux/man-pages/index.html)
- [The Linux man-pages project (kernel.org)](https://www.kernel.org/doc/man-pages/)
- [Manpages of manpages-zh in Debian unstable — Debian Manpages](https://manpages.debian.org/unstable/manpages-zh/index.html)


# 参考

- [man page - Wikipedia](https://en.wikipedia.org/wiki/Man_page)
- [Understanding Man Pages ⋆ Mark McDonnell (integralist.co.uk)](https://www.integralist.co.uk/posts/understanding-man-pages/)
- [man-pages-zh/manpages-zh: Chinese Manual Pages (github.com)](https://github.com/man-pages-zh/manpages-zh)


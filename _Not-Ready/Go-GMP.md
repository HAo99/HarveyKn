- GMP 是 Go 语言的协程调度模型。
- G 代表 Goroutine，用于管理执行单元的上下文。
- M 代表 Machine，用于管理系统线程，与系统线程是 1:1 的关系。
- P 代表 Processor，可视作队列，用于管理 G 的执行逻辑。

Go 的 Goroutine 相比系统线程（OS Thread）的优劣
- 内存消耗：一个 Goroutine 在 AMD64 平台下只消耗 2 KB 的内存，并且其栈支持伸缩。而一个 OS Thread 默认会为栈分配 1～8 MB 的空间，而且为了线程之间防止栈越界，还会创建 Guard Page 来进行隔离。
- 创建/销毁：一个 OS Thread 的创建和销毁需要陷入（Trap）内核 ，而进入内核的开销是比较大的，涉及上下文的保存与恢复等。而一个 Goroutine 的创建与销毁都是在用户态进行的，开销较小。
- 调度切换：一个 OS Thread 的调度切换通常要花费 1000～1500ns（通常 1ns 可以执行12～18 条指令）。而一个 Goroutine 只需要花费 200ns。
    - 所以 OS 线程切换，执行指令的条数会减少 12000-18000。Goroutine 只会减少 2400-3600 条指令。Goroutine 的开销远小于 OS Thread。
- 复杂性：传统的 OS Thread 编程复杂，线程间通信困难（涉及大量 Callback，需要多路复用）。而 Goroutine 的创建具有 Built-in 的关键字支持，线程通信也有 Channel 机制，还有传统并发原语的支持。

- Go 的线程模型是 M:N，M 表示 OS Thread，N 表示 Goroutine，N 个 Goroutine 依附在 M 个 OS Thread 上执行。

- 可将任务调度视为生产者消费者模型，执行单元负责生产任务，GMP 调度器负责消费任务。
- 程序开始运行时，默认开启当前系统的逻辑核心数作为 P 的个数。该数值是以物理机为标准，并不是容器配置为标准。生产环境可能会导致 P 开得过多，而可用的 M 远小于 P。可用 Uber 出品的 [uber-go/automaxprocs](https://github.com/uber-go/automaxprocs) 库进行处理，它会根据容器的核心数进行配置，使用时只需引入即可 `import _ "go.uber.org/automaxprocs"`。

### 生产
- 任务队列有两个，一个是挂在 P 上的本地队列（p.runq），一个是全局队列（schedt.runq）。
    - 本地队列是有限空间的，是一个容量为 256 的指针数组
    - 全局队列是无限个数的，是一个链表
- 用户使用 `go` 关键字开启一个 Goroutine 时，Runtime 会包装为一个 G，并放入当前 Goroutine 所绑定的 P 的 runnext 字段中。
    - 如果当前 runnext 没有挂任何 G，则直接放入即可；
    - 如果当前 runnext 已有 G，则将其踢出并放入本地队列，新的 G 放入 runnext；
    - 如果当前 P 的任务队列已满，则将 P 队列中的后面一半元素和当前被踢出 runnext 的 G 一起放入到全局队列中去。
- P 要执行 G 必须要找一个 M 进行绑定，如果 P 没有找到在自旋（Spinning）等待的 M 的话，P 必须唤醒一个正在睡眠的 M，或者创建一个。
- M 的 OS Thread 被创建后，会执行一个特殊的 G 即 g0，它负责管理和调度 G。
    - 首先寻找一个空闲的 P 进行绑定，如果找不到则自旋；
    - 找到 P 绑定后，寻找 G 进行运行，如果找不到则自旋；
    - 为了避免因自旋过度浪费 CPU 资源，自旋的 M 最多只允许有 GOMAXPROCS 个，自旋一段时间后还找不到则进入睡眠状态。

### 消费
- M 在执行 G 时
    - 如果 runnext 有 G，则先执行该 G；
    - 如果 runnext 没有 G，则执行 P 本地队列中的 G；
    - 如果 P 的本地队列没有 G，则到全局队列中捞一半的 G 回来；
    - 如果全局队列也没有 G，则到别的 P 的队列中去捞，同样也是捞一半，该机制为 Work-Stealing。
- Work-Stealing 的机制是防止 P 产生饥饿。
- 假如 G 需要进行系统调用，Go 在标准库中的所有系统调用方法在 plan-9 汇编层都添加了一层包装。
    - 如果 G 执行完系统调用后，很快的速度就得到系统调用的返回，则什么也不会做
    - 如果 G 执行系统调用花费了很长的时间，由 sysmon 进行检查，则 P 会与 M 解绑
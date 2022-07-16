# Ch8 Exceptional Control Flow

系统必须对某些系统状态的变化做出反应，这些系统状态不是被内部程序捕获的，而且也不一定要和执行的程序相关，例如，硬件定时器定期产生的信号。对于这种突发的的情况，现代系统将这些突变称为异常控制流（Exceptional Control Flow，ECF），异常控制流通常发生在如下方面：

* 硬件层，硬件检测到的事件会触发异常处理程序；
* 操作系统层，上下文切换；
* 应用层，进程间的信号发送及信号处理程序

ECF 的重要性：

* ECF 是操作系统实现 I/O、进程及虚拟内存机的基本机制；
* 通过**陷入**和**系统调用**，实现应用程序与操作系统的交互；
* 操作系统为应用程序提供了 ECF 机制，用于创建新进程、等待进程终止、检测、相应和通知其它进程系统中的异常事件；
* ECF 是实现并发的基本机制；
* ECF 实现编程语言的异常机制，例如 `try`、`catch`、`throw`；软件异常允许程序进行非本地跳转（即违反通常的调用/返回栈规则的跳转）来响应错误情况，在 C 中使用 `setjmp` 和 `longjmp` 来实现的。
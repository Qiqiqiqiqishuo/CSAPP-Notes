# \##  线程内存模型

一组并发线程运行在一个进程的上下文中。 每个线程都有它自己独立的线程上下文， 包括线程ID、栈、栈指针、程序计数器、条件码和通用目的的寄存器值。 
每个线程和其他线程一起共享进程上下文的剩余部分。 这包括整个用户虚拟地址空间，
它是由只读文本（代码）、读/写数据、堆以及所有的共享库代码和数据区域组成的。 线程也共享相同的打开文件的集合。

线程的上下文是独立的，不能相互访问，包括线程id,栈，栈指针，程序计数器等。 每个线程和其他线程可以一起共享他们所属进程的上下文部分。


#### 10.4 读和写文件

应用程序通过调用`read`和`write`函数执行输入和输出。

```c
#include <unistd.h>
// fd 文件描述符       *buf 内存位置	 n 大小
ssize_t read(int fd, void *buf, size_t n);
// 返回： 若成功则为读的字节数，若 EOF(end of file) 则为0，若出错则为 -1.
ssize_t write(int fd, const void *buf, size_t n);
// 返回： 若成功则为写的字节数，若出错则为 -1.
```

##### `read`函数

含义：

+ 从描述符为`fd`的当前文件位置复制最多$n$个字节到内存位置 `buf`。

- 返回值
  - -1表示一个错误；
  - 0表示`EOF(end of file)；
  - 否则返回值表示的是实际传送的字节数量；

##### `write`函数

含义：从内存位置`buf`复制至多$n$个字节到描述符`fd`的当前文件位置。

以下程序使用`read`和`write`调用一次一个字节地从标准输入复制到标准输出。

```c
/* $begin cpstdin */
#include "csapp.h"

int main(void) 
{
    char c;

    while(Read(STDIN_FILENO, &c, 1) != 0) 
        Write(STDOUT_FILENO, &c, 1);
    exit(0);
}
/* $end cpstdin */
```

调用`lseek`函数，应用程序能够显式地修改当前文件的位置。

> ##### `ssize_t`和`size_t`的区别
>
> - read()函数`size_t`为参数，返回值为`ssize_t`。
> - x86-64 系统中，`size_t`定义为`unsigned long`，而`ssize_t` (**有符号的大小**) 被定义为`long`。
> - `read`函数返回一个有符号的大小，保证出错时能返回 **-1。**
> - 返回-1的可能性使得`read`的最大值减小了一半。

##### 不足值

背景：在某些情况下， read 和write传送的字节比应用程序要求的要少。这些不足值（ short count ）不表示有错误。

> **不足值是已经读到的文本**。

出现不足值的原因：

- 读时遇到 EOF
  - 准备读一个文件，该文件从当前文件位置开始只含有20个字节的。而文件以50个字节的片段进行读取。下一个`read`返回不足值为20，此后的`read`将通过返回不足值0发出EOF信号。
- 从终端读取文本行
  - 打开文件是与终端相关联的，每个`read`函数一次传送一个文本行，返回的不足值等于文本行的大小。
- **读和写网络套接字 socket**
  + 如果打开的文件对应于网络套接字，那么内部缓冲约束和较长的网络延迟会引起函数返回不足值。
  + 对Linux Pipe调用函数时，也有可能出现不足值。
- 读/写磁盘文件时，将会遇到不足值。
- 创建可靠诸如Web服务器的网络应用，须通过反复调用`read`和`write`处理不足值，直到所有需要字节传送完毕。

#### 10.5 用RIO包健壮地读写

RIO(Robust I/O) ：健壮的`I/O`包。

功能：

1. 自动处理上文中的不足值。
2. 在容易出现不足值的应用中，RIO包提供方便、健壮和高效的`I/O`。

RIO提供了两类函数：

- 无缓冲的输入输出函数
  - 函数直接在内存和文件中传送数据，没有应用级缓冲。
  - 对将**二进制数据**读写到网络和从网络读写**二进制数据**尤其有用。
- 带缓冲的输入函数
  - 高效地从文件中读取**文本行**和**二进制数据**。
  - 这些文件的内容缓存在**应用级缓冲区**中，类似于为 `printf` 这样的标准I/O函数提供的缓冲区。
  - 是**线程安全**的（见 Chapter 12.7.1 节）。
  - 在同一个描述符上可以被交错地调用。
  - 可以从一个描述符中读一些文本行，然后读取一些二进制数据，接着再多读取一些文本行。



##### 10.5.1 RIP的无缓冲的输入输出函数

调用`rio_readn`和`rio_writen`函数，应用程序可以在内存和文件之间直接传送数据。

```c
#include <unistd.h>
// fd 文件描述符       *usrbuf 内存位置	 n 大小
ssize_t rio_readn(int fd, void *usrbuf, size_t n);
ssize_t rio_writen(int fd, void *usrbuf, size_t n);
// 返回:若成功则为写传送的字节数,若EOF则为0(只对rio_readn而言)，若出错则为-1
```

`rio_readn`函数：

+ 从描述符`fd`的当前文件位置最多传送$n$个字节到内存位置`usrbuf`。
+ 遇到EOF时只能返回一个不足值。

`rio_writen`函数：

+ 从描述符`fd`的当前文件位置最多传送$n$个字节到描述符`fd`。
+ 不会返回不足值。
+ 对同一个描述符，可以任意交错地调用`rio_readn`和`rio_writen`。

`rio_readn`函数：

```c
/*
 * rio_readn - Robustly read n bytes (unbuffered)
 */
/* $begin rio_readn */
ssize_t rio_readn(int fd, void *usrbuf, size_t n) 
{
    size_t nleft = n;
    ssize_t nread;
    char *bufp = usrbuf;

    while (nleft > 0) {
		if ((nread = read(fd, bufp, nleft)) < 0) {
	    	if (errno == EINTR) /* Interrupted by sig handler return */
				nread = 0;      /* and call read() again */
	    	else
				return -1;      /* errno set by read() */ 
			} 
		else if (nread == 0)
	    	break;              /* EOF */
		nleft -= nread;
		bufp += nread;
    	}
    	return (n - nleft);         /* Return >= 0 */
}
/* $end rio_readn */
```

`rio_writen`函数：

```c
/*
 * rio_readn - Robustly read n bytes (unbuffered)
 */
/* $begin rio_readn */
ssize_t rio_readn(int fd, void *usrbuf, size_t n) 
{
    size_t nleft = n;
    ssize_t nread;
    char *bufp = usrbuf;

    while (nleft > 0) {
		if ((nread = read(fd, bufp, nleft)) < 0) {
	    	if (errno == EINTR) /* Interrupted by sig handler return */
				nread = 0;      /* and call read() again */
	    	else
				return -1;      /* errno set by read() */ 
			} 
		else if (nread == 0)
	   		break;              /* EOF */
		nleft -= nread;
		bufp += nread;
    	}
    	return (n - nleft);         /* Return >= 0 */
}
/* $end rio_readn */
```

> 如果`rio_readn`和`rio_writen`函数被一个从应用信号处理程序的返回中断，那么每个函数会手动地重启`read`或`write`。为了尽可能有较好的可移植性，我们允许被中断的系统调用，且在必要时重启它们。
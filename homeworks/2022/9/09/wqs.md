## P1-5

计算机系统是由硬件和系统软件组成的。

hello程序的生命周期：源程序（由0和1组成的位序列），转化为低级别的机器语言。按照一种称为可执行目标程序的格式打好包，以二进制磁盘文件形式存储。源文件到目标文件是由编译器驱动程序完成。

ASCII标准来表示文本字符，每个字符都有一个唯一的单字节大小的整数值。

只由ASCII字符构成的文件称为文本文件，其他文件称为二进制文件。

数字的机器表示方式与实际的整数和实数是不同的，是对真值的有限近似值。

C是作为用于Unix系统的程序语言开发出来的。大部分Unix内核（操作系统的核心）以及所有的支撑工具、函数库都是用C编写的。方便移植到其他机器上。C是系统级编程的首选，缺乏对抽象的显式支持，例如类、异常、对象。

编译系统：预处理器，编译器，汇编器和链接器。
预处理阶段：预处理器cpp读取头文件#include<> ，并直接插入程序文本中，得到了另一个c程序，以.i作为文件拓展名。
编译阶段：编译器ccl将文本文件hello.i翻译成文本文件hello.s，得到汇编语言程序。以文本格式描述低级机器语言指令。不同高级语言用了同一种汇编语言。
汇编阶段：汇编器as将hello.s翻译成机器语言指令，打包成可重定位目标格式，存在目标文件hello.o中，是一个二进制文件。
链接阶段：将库函数预编译好了的目标文件合并到hello.o中，得到hello文件，可执行目标文件，加载到内存中，由系统执行。

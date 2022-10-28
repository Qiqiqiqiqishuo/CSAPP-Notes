## P261-265
组合逻辑电路可以设计成在字级数据上执行许多不同类型的操作。算术/逻辑单元（ALU）是一种很重要的组合电路。三个输入：A、B数据输入，以及一个控制输入。

4.2.4 集合关系
在处理器设计中，很多时候都需要将一个信号与许多可能匹配的信号做比较，以检测正在处理的某个指令代码是否属于某一类指令代码。

两位的信号code可以控制对4个数据字A、B、C、D做选择。根据可能的code值，可以用相等测试来表示信号s1、s0的产生：
bool s1 = code == 2 || code == 3;
bool s0 = code == 1 || code == 3;
bool s1 = code in {2, 3};
bool s0 = code in {1, 3}

4.2.5 存储器和时钟
组合电路从本质上讲，不存储任何信息，只是简单的响应输入信号。时钟寄存器存储单个位或字，时钟信号控制寄存器加载输入值。随机访问存储器存储多个字，用地址来选择该读或该写哪个字（处理器的虚拟内存系统、寄存器文件）。
多端口随机访问存储器允许同时进行多个读和写操作。

4.3 Y86-64的顺序实现
4.3.1 将处理组织成阶段
fetch,decode,execute,memory,write back,PC update
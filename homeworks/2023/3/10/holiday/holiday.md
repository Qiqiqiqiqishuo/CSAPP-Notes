函数调用链

![image-20230311120757911](image/image-20230311120757911.png)

栈上用于特定call的每个内存块称为栈帧，我们为栈中每个被调用但是未返回的过程保留一个栈帧

递归有可能会爆栈

![image-20230311120817664](image/image-20230311120817664.png)

这里有一个`call_incr`的函数，他创建一个v1的值，并且生成一个指向该值的指针

红色代码会生成2条指令，其一是在栈中分配16字节的内存，其二是在离栈指针的偏移量为8的地方存储常数15213

我们在栈在获取空间的方式是递减堆栈指针
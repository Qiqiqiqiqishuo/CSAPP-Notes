%rsp：栈顶部

%rip：当前执行的指令地址

初始状态：一开始%rsp指向的是0x120

![image-20230304232418091](image/image-20230304232418091.png)



执行call会做3件事：

1.减少栈指针，从0x120编程0x118

2.将此调用之后的指令地址写入栈顶部

3.修改%rip为当前执行的指令地址

![image-20230304232436128](image/image-20230304232436128.png)

ret做了：

1.增加栈指针，回到原来的地方

2.修改%rip为新的指令地址

![image-20230304232452910](image/image-20230304232452910.png)
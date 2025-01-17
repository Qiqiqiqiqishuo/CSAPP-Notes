## 网络通信就是发出去和接收时候的复制

现代系统经常通过网络和其他系统连接到一起。从一个单独的系统来看，网络可视为一个 I/O 设备，如图 1-14 所示。 当系统从主存复制一串字节到网络适配器时，数据流经过网络到达另一台机器，而不是比如说到达本地磁盘驱动器。 相似地，系统可以读取从其他机器发送来的数据，并把数据复制到自己的主存。

![image-20230129012300605](image/image-20230129012300605.png)

随着 Internet 这样的全球网络的出现，从一台主机复制信息到另外一台主机已经成为计算机系统最重要的用途之一。比如，像电子邮件、即时通信、万维网、FTP 和 telnet 这样的应用都是基于网络复制信息的功能。 回到 hello 示例，我们可以使用熟悉的 telnet 应用在一个远程主机上运行 hello 程序。假设用本地主机上的 telnet 客户端连接远程主机上的 telnet 服务器。在我们登录到远程主机并运行 shell 后，远端的 shell 就在等待接收输入命令。此后在远端运行 hello 程序包括如图 1-15 所示的五个基本步骤。

![image-20230129012321111](image/image-20230129012321111.png)

当我们在 telnet 客户端键入 “hello” 字符串并敲下回车键后，客户端软件就会将这个字符串发送到 telnet 的服务器。telnet 服务器从网络上接收到这个字符串后，会把它传递给远端 shell 程序。接下来，远端 shell 运行 hello 程序，并将输出行返回给 telnet 服务器。最后，telnet 服务器通过网络把输出串转发给 telnet 客户端，客户端就将输出串输出到我们的本地终端上。 这种客户端和服务器之间交互的类型在所有的网络应用中是非常典型的。
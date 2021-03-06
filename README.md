在Linux下使用socket API编写一些网络应用程序，用以提高网络编程能力



## heartbeat

一个拥有心跳检测(基于TCP的带外数据机制实现)的长连接echo程序，可以指定每隔n秒轮询对端，如果连续轮询m次对端都未应答，则认为对端不再存活，会自动关闭连接。

**注意点**
- 当关闭一个select正在监听的文件描述符，select函数是否会返回属于未定义行为，一开始我在考虑如何处理该问题，但发现heartbeat程序刚好是在信号处理函数中执行关闭文件描述符操作，而信号处理会导致阻塞的系统调用返回一个EINTR错误，那么执行关闭文件描述符后，select返回就变成了确定的行为，免去了很多问题
- 当程序加上了处理OOB数据的功能后，如果直接kill程序，程序会给对端直接发送RST而不是FIN，有点奇怪，具体通过tcpdump抓取FIN和RST验证. `$ tcpdump -i <interface> "tcp[tcpflags] & (tcp-fin|tcp-rst) != 0"`



## httpdir

一个简单的HTTP服务器，启动命令：`$ ./a.out <listenip> <listenport> <dir>`，启动后可以通过浏览器请求`http://<listenip>:<listenport>`来访问服务器上`<dir>`路径下的文件和目录

注意点

- 程序只解析出HTTP请求的[Request Line](https://tools.ietf.org/html/rfc7230#section-3.1.1)，因为只需要拿到其中的request-target


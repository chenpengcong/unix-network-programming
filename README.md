在Linux下使用socket API编写一些网络应用程序，用以提高网络编程能力



- **heartbeat**

一个拥有心跳检测(基于TCP的带外数据机制实现)的长连接echo程序，可以指定每隔n秒轮询对端，如果连续轮询m次对端都未应答，则认为对端不再存活，会自动关闭连接。


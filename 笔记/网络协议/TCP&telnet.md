### TCP（Trajnsmission Control Potocol）传输控制协议

1. 面向连接（三次握手、四次挥手）
2. 可靠传输（序列号/确认号， 重传机制）
3. 流量控制（滑动窗口）
4. 多路复用

telnet远程登录

![1531666691866](C:\Users\brain\AppData\Local\Temp\1531666691866.png)

### 三次握手：

1. 客户端发送SYN
2. 服务端收到后，回发ACK+SYN
3. 客户端发送ACK

![1531666718468](C:\Users\brain\AppData\Local\Temp\1531666718468.png)

### 四次挥手：

1. 客户端发送FIN

2. 服务端发送ACK

   中间可以有数据传输

3. 服务端发送FIN

4. 客户端发送ACK
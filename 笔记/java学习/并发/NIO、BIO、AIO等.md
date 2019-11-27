---
typora-copy-images-to: ./
typora-root-url: ..\..\images
---

#### 区分网络IO与磁盘IO

在网络IO中，read、write等操作会因为数据没有就绪而进入“Blocking”，但是对于磁盘IO来说，不会进入Block。

所以现如今讨论的IO优化多是网络IO。

### BIO（Blocking IO）

![img](640.png)

在类似于网络中进行read, write, connect一类的系统调用时会被卡住。

举个例子，当用read去读取网络的数据时，是无法预知对方是否已经发送数据的。因此在收到数据之前，能做的只有等待，直到对方把数据发过来，或者等到网络超时。

对于单线程的网络服务，这样做就会有卡死的问题。因为当等待时，整个线程会被挂起，无法执行，也无法做其他的工作。

<!--此处的挂起指的是当前进程被挂起，不影响其他程序的运行-->



### NIO（No-Blocking IO）

在BIO中，调用read，如果发现没有数据到达，。就会Block；

在NIO中，如果没有数据到达，会立刻返回-1，并将errno设置EAGAIN/EWOULDBLOCK。表示等一下在进行尝试，加入轮询队列

##### NIO新问题

1. 在轮询中会频繁切换线程，导致Context Switch。每次切换都会在用户态和核心态切换一次。
2. 停顿时间设置，不合理的停顿时间会影响系统性能。

##### IO多路复用

![img](/Figure 1.2 Nonblocking IO.jpg)

多个socket共用线程，监听每个socket的事件，使用select进行选择

![img](165327_uH4K_2243330.png)

1. 得到Channel

2. 申请Buffer

3. 建立Channel和Buffer的读/写关系

4. 关闭

###### 核心组件

- Buffers（缓冲区）

- Selectors（选择器）
- Channels（通道）

一个Channel可以和文件或者网络Socket对应 。

### 实现

使用jdk实现IO

```
public class PlainOioServer {

    public void serve(int port) throws IOException {
    	//绑定端口到serversocket
        final ServerSocket socket = new ServerSocket(port);   
        try {
            for (;;) {
            	//阻塞操作：获得clientSocket，接受连接
                final Socket clientSocket = socket.accept();
                System.out.println("Accepted connection from " + clientSocket);
			//创建线程实现业务功能
                new Thread(new Runnable() {                        
                    @Override
                    public void run() {
                        OutputStream out;
                        try {
                            out = clientSocket.getOutputStream();
                            out.write("Hi!\r\n".getBytes(Charset.forName("UTF-8")));
                            out.flush();
                            //消息写入刷新就关闭连接
                            clientSocket.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                            try {
                                clientSocket.close();
                            } catch (IOException ex) {
                                // ignore on close
                            }
                        }
                    }
                }).start();                                        //6
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

使用JDK实现NIO

```
public class PlainNioServer {
    public void serve(int port) throws IOException {
    	//开启Channel，并进行配置为非阻塞
        ServerSocketChannel serverChannel = ServerSocketChannel.open();
        serverChannel.configureBlocking(false);
        // 设置ServerChannel 的ServerSocket, 绑定地址
        ServerSocket ss = serverChannel.socket();
        InetSocketAddress address = new InetSocketAddress(port);
        ss.bind(address);          
        //创建一个Selector处理channel
        Selector selector = Selector.open();                        
        //将channel注册到selector，并指定这是专门用来接受连接。
        serverChannel.register(selector, SelectionKey.OP_ACCEPT);
        final ByteBuffer msg = ByteBuffer.wrap("Hi!\r\n".getBytes());
        for (;;) {
            try {
            	//阻塞，直到传入一个连接事件
                selector.select();                                    
            } catch (IOException ex) {
                ex.printStackTrace();
                // handle exception
                break;
            }
            //获取接收到事件的selectionKeys
            Set<SelectionKey> readyKeys = selector.selectedKeys();   
            Iterator<SelectionKey> iterator = readyKeys.iterator();
            while (iterator.hasNext()) {
                SelectionKey key = iterator.next();
                iterator.remove();
                try {
                	//检查事件是不是可连接的
                    if (key.isAcceptable()) {             
                        ServerSocketChannel server =
                                (ServerSocketChannel)key.channel();
                        //处理连接事件，获得客户端连接
                        SocketChannel client = server.accept();
                        client.configureBlocking(false);
                        //接受客户端，注册到selector
                        client.register(selector, SelectionKey.OP_WRITE |
                                SelectionKey.OP_READ, msg.duplicate());    
                        System.out.println(
                                "Accepted connection from " + client);
                    }
                    //检查是否已经准备好写入数据
                    if (key.isWritable()) {                
                        SocketChannel client =
                                (SocketChannel)key.channel();
                        ByteBuffer buffer =
                                (ByteBuffer)key.attachment();
                        //循环写入数据
                        while (buffer.hasRemaining()) {
                            if (client.write(buffer) == 0) {        
                                break;
                            }
                        }
                        client.close();                    //10
                    }
                } catch (IOException ex) {
                    key.cancel();
                    try {
                        key.channel().close();
                    } catch (IOException cex) {
                        // 在关闭时忽略
                    }
                }
            }
        }
    }
}
```


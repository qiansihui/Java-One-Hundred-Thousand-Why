# Netty

## BIO、NIO和AIO

BIO:同步阻塞IO，在读取时若无数据，就会阻塞等待，效率很低
NIO：非阻塞IO，在读取时若无数据，就会立即返回。将很多个IO读写通过Channel注册到多路复用器Selector上，并监听想要处理的事件，Selector会轮询各个IO状态，返回准备就绪的SelectKey。
AIO：异步IO，告知内核启动某个操作，并让内核完成整个操作（数据从内核复制到用户缓冲区）是通过我们。

## Netty 的各大组件

1. Channel ,Netty网络操作的抽象类，bind/connect/read/write方法
2. EventLoop , Netty基于事件驱动模型，使用不同事件通知我们状态改变。EventLoop定义了整个连接的生命周期中当有事件发生时处理的核心抽象。
    EventLoop主要为Channel处理IO操作，两者配合。
3. ChannelFuture ,Netty为异步非阻塞的，通过Channel接口的addListener()方法注册 ChannelFutureListener ，当操作状态改变时触发调用。
4. ChannelHandler , 为Netty核心组件，充当所有处理入站和出站数据的应用程序逻辑的容器。
    ChannelHandler主要处理各种事件，如连接、数据接收、异常、数据转换等。
5. ChannelPipeline , 为 ChannelHandler链提供了容器，并定义了用于沿着链传播入站和出站事件流的API。

## Netty的线程模型

1. Reactor单线程模型。所有IO操作都在同一个NIO线程上完成，通过Acceptor接收客户端TCP连接请求，链路建成后，通过Dispatch将对应的ByteBuff派发到指定Handler上，进行消息编解码。
    用户线程消息编码后通过NIO线程将消息发送回客户端。
2. Reactor多线程模型。与Reactor单线程模型区别在于使用一组NIO线程来处理IO操作。若客户端-服务端握手需要安全认证是，Acceptor会有性能问题。
3. 主从Reactor多线程模型。服务端用于接收客户端连接的不再是一个NIO线程，而是一个独立的NIO线程池。
    Acceptor线程池仅仅用于客户端登录、握手和安全认证上，链路建立成功后，就将链路注册到后端subReactor线程池的IO线程上。

## TCP 粘包/拆包的原因及解决方法

TCP粘包拆包原因：

1. 应用程序write的字节大小大于套接口发送缓冲区大小
2. 进行MSS大小的TCP分段
3. 以太网帧的payload大于MTU的IP分片。

解决方法：

1. 消息定长，空格补缺。
2. 报文结尾以换行符分割。
3. 将消息分为消息头和消息体。
4. 更复杂的应用层协议。

## 了解哪几种序列化协议？包括使用场景和如何去选择

1. Java序列化协议，jvm语言使用，码流大，效率低。
2. Google的 ProtocolBuf , 结构化数据存储格式，高效的编解码性能，语言无关平台无关，支持java,c++,python，自动代码生成。
3. Facebook的 Thrift , 跨平台，支持多种语言，适用于搭建大型数据交换及存储通用工具。

## Netty的零拷贝实现

1. Netty的接收和发送使用Direct Buffers，使用堆外内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。
2. Netty提供了 Composite Buffers，通过聚合多个Buffer而不是创建新的，避免了内存拷贝
3. Netty文件传输采用了Channel.transferTo方法，可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环write导致的内存拷贝问题。

## Netty的高性能表现在哪些方面

1. 采用异步非阻塞的IO类库，基于Reactor模式，解决了传统同步阻塞模式IO下一个服务端无法平滑处理线性增长客户端问题。
2. TCP接收和发送缓冲区使用直接内存代替堆内存，避免了内存复制，提升了IO读取和写入性能。
3. 支持通过内存池循环利用ByteBuf，避免了频繁创建和销毁的开销。
4. 可配置的IO线程数、TCP参数等，为不同用户场景定制条有参数，满足不同性能场景。
5. 采用环形数组缓冲区实现无锁并发编程。
6. 合理使用线程安全容器、原子类，提升系统并发处理能力。
6. 关键资源的处理使用单线程串行化，避免多线程并发访问带来的锁竞争。
8. 通过引用计数及时释放不再使用的对象，内存管理降低GC频率。 


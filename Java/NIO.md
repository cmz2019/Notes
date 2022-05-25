# NIO

推荐阅读：

[Java NIO？看这一篇就够了！](https://blog.csdn.net/u011381576/article/details/79876754)

# 一、概述

Java NIO（New IO 或 Non Blocking IO）是从`Java 1.4` 版本开始引入的一个新的IO API，可以替代标准的 Java IO API。NIO 支持面向缓冲区的、基于通道的 IO 操作。NIO 将以更加高效的方式进行文件的读写操作。

## 1 阻塞IO

通常在进行同步 I/O 操作时，如果读取数据，代码会阻塞直至有可供读取的数据。同样，写入调用将会阻塞直至数据能够写入。传统的 Server/Client 模式会基于 TPR（Thread per Request），服务器会为每个客户端请求建立一个线程，由该线程单独负责处理一个客户请求。

这种模式带来的一个问题就是线程数量的剧增，大量的线程会增大服务器的开销。大多数的实现为了避免这个问题，都采用了线程池模型，并设置线程池线程的最大数量，这又带来了新的问题，如果线程池中有 100 个线程，而有 100 个用户都在进行大文件下载，会导致第 101 个用户的请求无法及时处理，即便第 101 个用户只想请求一个几 KB 大小的页面。

如果线程数量不够别的请求使用，那其他的请求就会被阻塞，等待拿到线程去处理。

## 2 非阻塞IO

通过Selector去实时监听通道中的事件

NIO 中非阻塞 I/O 采用了基于 Reactor 模式的工作方式，I/O 调用不会被阻塞，而是注册感兴趣的特定 I/O 事件，如可读数据到达，新的套接字连接等等，在发生特定事件时，系统再通知我们。NIO 中实现非阻塞 I/O 的核心对象就是 Selector，在 Selector 中注册各种 I/O 事件，当我们感兴趣的事件发生时，就是这个对象告诉我们所发生的事件，如下图所示：

<img src="https://strawberry-album.oss-cn-beijing.aliyuncs.com/image/image-20220304212916636.png" alt="image-20220304212916636" style="zoom:67%;" />

# 二、聊天室案例

代码如下：

[https://gitee.com/cmz2000/nio_chat/tree/master/src/main/java/com/strawberry](https://gitee.com/cmz2000/nio_chat/tree/master/src/main/java/com/strawberry)
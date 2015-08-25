
## 简介

Epoll是poll的改进版，更加高效，能同时处理大量文件描述符，跟高并发有关，Nginx就是充分利用了epoll的特性。讲这些没用，我们先了解poll是什么。

## Poll

Poll本质上是Linux系统调用，其接口为`int poll(struct pollfd *fds,nfds_t nfds, int timeout)`，作用是监控资源是否可用。

举个例子，一个Web服务器建了多个socket连接，它需要知道里面哪些连接传输发了请求需要处理，功能与`select`系统调用类似，不过`poll`不会清空文件描述符集合，因此检测大量socket时更加高效。

## Epoll

我们重点看看epoll，它大幅提升了高并发服务器的资源使用率，相比poll而言哦。前面提到poll会轮询整个文件描述符集合，而epoll可以做到只查询被内核IO事件唤醒的集合，当然它还提供边沿触发(Edge Triggered)等特性。

不知大家是否了解C10K问题，指的是服务器如何支持同时一万个连接的问题。如果是一万个连接就有至少一万个文件描述符，poll的效率也随文件描述符的更加而下降，epoll不存在这个问题是因为它仅关注活跃的socket。

## 实现

这是怎么做到的呢？简单来说epoll是基于文件描述符的callback函数来实现的，只有发生IO事件的socket会调用callback函数，然后加入epoll的Ready队列。更多实现细节可以参考Linux源码，

## Mmap

无论是select、poll还是epoll，他们都要把文件描述符的消息送到用户空间，这就存在内核空间和用户空间的内存拷贝。其中epoll使用mmap来共享内存，提高效率。

Mmap不是进程的概念，这里提一下是因为epoll使用了它，这是一种共享内存的方法，而Go语言的设计宗旨是"不要通过共享来通信，通过通信来共享"，所以我们也可以思考下进程的设计，是使用mmap还是Go提供的channel机制呢。

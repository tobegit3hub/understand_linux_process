## 系统调用sendfile

[Sendfile](http://man7.org/linux/man-pages/man2/sendfile.2.html)是Linux实现的系统调用，可以通过避免文件在内核态和用户态的拷贝来优化文件传输的效率。

其中大名鼎鼎的分布式消息队列服务Kafka就使用sendfile来优化效率，具体用法可参见其[官方文档](http://kafka.apache.org/documentation.html)。

## 优化策略

在普通进程中，要从磁盘拷贝数据到网络，其实是需要通过系统调用，进程也会反复在用户态和内核态切换，频繁的数据传输在此有效率问题。因此我们必须意识到Linux给我们提供了sendfile这样的系统调用，可以提高进程的数据传输效率。

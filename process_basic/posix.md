
## POSIX标准

POSIX是什么？也许你已经听过这个概念，却不能准确说出它的含义。POSIX是Portable Operating System Interface的缩写，表示可移植的操作系统接口，也就是Linux和其他Unix-like系统共同实现的API约定。

## POSIX进程

POSIX定义了进程间通信的接口，包括POSIX消息。

|接口名称|目的|
|-------|---|
|mq_open(3RT)|连接到以及（可选）创建命名消息队列|
|mq_close(3RT)|结束到开放式消息队列的连接|
|mq_unlink(3RT)|结束到开放式消息队列的连接，并在最后一个进程关闭此队列时将其删除|
|mq_send(3RT)|将消息放入队列|
|mq_receive(3RT)|在队列中接收（删除）最早且优先级最高的消息|
|mq_notify(3RT)|通知进程或线程消息已存在于队列中|
|mq_setattr(3RT)、mq_getattr(3RT)|设置或获取消息队列属性|

POSIX信号量。

|信号量|目的|
|-----|---|
|sem_open(3RT)|连接到以及（可选）创建命名信号量|
|sem_init(3RT)|初始化信号量结构（在调用程序内部，因此不是命名信号量）|
|sem_close(3RT)|结束到开放式信号量的连接|
|sem_unlink(3RT)|结束到开放式信号量的连接，并在最后一个进程关闭此信号量时将其删除|
|sem_destroy(3RT)|初始化信号量结构（在调用程序内部，因此不是命名信号量）|
|sem_getvalue(3RT)|将信号量的值复制到指定整数中|
|sem_wait(3RT)、sem_trywait(3RT)|当其他进程拥有信号量时进行阻塞，或者当其他进程拥有信号量时返回错误|
|sem_post(3RT)|递增信号量计数|

还有POSIX共享内存。

POSIX共享内存是通过映射内存的变体。二者的主要差异在于：

打开共享内存对象应使用`shm_open(3RT)`，而不是通过调用`open(2)`。

关闭和删除对象应使用`shm_unlink(3RT)`，而不是通过调用`close(2)`，此调用不删除对象。


## POSIX线程

POSIX也定义了线程的标准，包括创建和控制线程的API，在Pthreads库中实现，有关线程的知识有机会再深入学习。

POSIX概念比较多，我们来点实际操作，探讨如果创建和运行新的进程。


## Polling模型

传统的select/poll另一个致命弱点就是当你拥有一个很大的socket集合，不过由于网络延时，任一时间只有部分的socket是“活跃”的，但是select/poll每次调用都会线性扫描全部的集合，导致效率呈现线性下降。但是epoll不存在这个问题，它只会对“活跃”的socket进行操作---这是因为在内核实现中epoll是根据每个fd上面的callback函数实现的。那么，只有“活跃”的socket才会主动的去调用 callback函数，其他idle状态socket则不会，在这点上，epoll实现了一个“伪”AIO，因为这时候推动力在os内核。在一些 benchmark中，如果所有的socket基本上都是活跃的---比如一个高速LAN环境，epoll并不比select/poll有什么效率，相反，如果过多使用epoll_ctl,效率相比还有稍微的下降。但是一旦使用idle connections模拟WAN环境，epoll的效率就远在select/poll之上了。

## Epoll模型

epoll相关的系统调用有：epoll_create, epoll_ctl和epoll_wait。Linux-2.6.19又引入了可以屏蔽指定信号的epoll_wait: epoll_pwait。至此epoll家族已全。其中epoll_create用来创建一个epoll文件描述符，epoll_ctl用来添加/修改/删除需要侦听的文件描述符及其事件，epoll_wait/epoll_pwait接收发生在被侦听的描述符上的，用户感兴趣的IO事件。epoll文件描述符用完后，直接用close关闭即可，非常方便。事实上，任何被侦听的文件符只要其被关闭，那么它也会自动从被侦听的文件描述符集合中删除，很是智能。

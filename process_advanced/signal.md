
## 信号

我们知道信号是进程间通信的其中一种方法，当然也可以是内核给进程发送的消息，注意信号只是告诉进程发生了什么事件，而不会传递任何数据。

这是进程这个概念设计时就考虑到的了，因为我们希望控制进程，就像一个小孩我们想他按我们的想法做，前提就是他能够接受信号并且理解信号的含义。

## 信号种类

Linux中定义了很多信号，不同的Unix-like系统也不一样，我们可以通过下面的命令来查当前系统支持的种类。

```
➜ kill -l
HUP INT QUIT ILL TRAP ABRT EMT FPE KILL BUS SEGV SYS PIPE ALRM TERM URG STOP TSTP CONT CHLD TTIN TTOU IO XCPU XFSZ VTALRM PROF WINCH INFO USR1 USR2
```

其中1至31的信号为传统UNIX支持的信号，是不可靠信号(非实时的)，32到63的信号是后来扩充的，称做可靠信号(实时信号)。不可靠信号和可靠信号的区别在于前者不支持排队，可能会造成信号丢失，而后者不会。

简单介绍几个我们最常用的，在命令行中止一个程序我们一般摁Ctrl+c，这就是发送SIGINT信号，而使用kill命令呢？默认是SIGTERM，加上`-9`参数才是SIGKILL。

## 编程实例

```golang
import os/signal

siganl.Notify()
signal.Stop()
```

这是Go封装的信号接口，我们可以以此实现一个简单的信号发送和处理程序。

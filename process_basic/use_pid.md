
## 查看PID

首先我们想知道进程的PID，可以通过`top`或者`ps`命令还查看。

## Top

在命令行执行`top`后，得到类似下面的输出，可以看到目前有三个进程，PID分别是1、8和9。

```
top - 12:45:18 up 1 min,  0 users,  load average: 0.86, 0.51, 0.20
Tasks:   3 total,   1 running,   2 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056748 total,   301984 used,  1754764 free,    20984 buffers
KiB Swap:  1427664 total,        0 used,  1427664 free.   231376 cached Mem

PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
1 root      20   0    4312    692    612 S   0.0  0.0   0:00.23 sh
8 root      20   0   20232   3048   2756 S   0.0  0.1   0:00.03 bash
9 root      20   0   21904   2384   2060 R   0.0  0.1   0:00.00 top
```

## PS

执行`ps aux`后输出如下，其中`aux`参数让`ps`命令显示更详细的参数信息。前面PID为9的top进程已经退出了，取而代之的是PID为11的ps进程。

```
root@fa13d0439d7a:/go/src# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.2  0.0   4312   692 ?        Ss   12:45   0:00 /bin/sh -c /bin/bash
root         8  0.0  0.1  20232  3224 ?        S    12:45   0:00 /bin/bash
root        11  0.0  0.0  17484  2000 ?        R+   12:46   0:00 ps aux
```

## 使用PID

拿到PID后，我们就可以通过`kill`命令来结束进程了，也可以通过`kill -9`或其他数字向进程发送不同的信号。

信号是个很重要的概念，我们后面会详细介绍，那么有了进程ID，我们也可以看看进程名字。

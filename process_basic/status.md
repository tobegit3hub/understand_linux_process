
## 进程状态

根据进程的定义，我们知道进程是代码运行的实体，而进程有可能是正在运行的，也可能是已经停止的，这就是进程的状态。

网上有人总结进程一共5种状态，也有总结是8种，究竟应该怎么算呢，最好的方法还是看Linux源码。进程状态的定义在[fs/proc/array.c](https://github.com/torvalds/linux/blob/b6da0076bab5a12afb19312ffee41c95490af2a0/fs/proc/array.c)文件中。

```c
/*
* The task state array is a strange "bitmap" of
* reasons to sleep. Thus "running" is zero, and
* you can test for combinations of others with
* simple bit tests.
*/
static const char * const task_state_array[] = {
  "R (running)",		/*   0 */
  "S (sleeping)",		/*   1 */
  "D (disk sleep)",	/*   2 */
  "T (stopped)",		/*   4 */
  "t (tracing stop)",	/*   8 */
  "X (dead)",		/*  16 */
  "Z (zombie)",		/*  32 */
};
```

这真的是Linux的源码，可以看出进程一共7种状态，含义也比较清晰，注意其中D(disk sleep)称为不可中断睡眠状态(uninterruptible sleep)。

知道进程状态本身没什么

## 进程状态转换

![](image/status_transform.svg)


[使用Ptrace](https://idea.popcount.org/2012-12-11-linux-process-states/)

[include/linux/sched.h](https://github.com/torvalds/linux/blob/master/include%2Flinux%2Fsched.h)



```c
struct task_struct {
  volatile long state;	/* -1 unrunnable, 0 runnable, >0 stopped */
  void *stack;
  atomic_t usage;
  unsigned int flags;	/* per process flags, defined below */
  unsigned int ptrace;
```

## 查看状态

通过`ps aux`可以看到进程的状态。

O：进程正在处理器运行,这个状态从来没有见过.
S：休眠状态（sleeping）
R：等待运行（runable）R Running or runnable (on run queue) 进程处于运行或就绪状态
I：空闲状态（idle）
Z：僵尸状态（zombie）　　　
T：跟踪状态（Traced）
B：进程正在等待更多的内存页
D:不可中断的深度睡眠，一般由IO引起，同步IO在做读或写操作时，cpu不能做其它事情，只能等待，这时进程处于这种状态，如果程序采用异步IO，这种状态应该就很少见到了


其中就绪状态表示进程已经分配到除CPU以外的资源，等CPU调度它时就可以马上执行了。运行状态就是正在运行了，获得包括CPU在内的所有资源。等待状态表示因等待某个事件而没有被执行，这时候不耗CPU时间，而这个时间有可能是等待IO、申请不到足够的缓冲区或者在等待信号。

## 状态转换

进程的运行过程也就是进程状态转换的过程。

例如就绪状态的进程只要等到CPU调度它时就马上转为运行状态，一旦它需要的IO操作还没有返回时，进程状态也就转换成等待状态。

进程状态间转换还有很多，这里不一一细叙，马上去学习进程退出码吧。

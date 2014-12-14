
## 守护进程


## 实现守护进程


## 代码示例


## 运行结果


最终我还是用nohup加su -m来做 daemon的

nohup就是singnal(1, ignored)

终端关闭，就会像终端内的所有进程发送sighup信号，程序对该信号的默认处理就是退出。

如果之前没有用nohup，其实可以用disown -ah来处理的


事先的可以用nohup或setsid

没有选项时，每个  jobspec  被从正在运行的作业表中删除。如果给出了  -  选项，每个  jobspec  并不从表中删除，而是被标记，使得在  shell 接到 SIGHUP 信号时，不会向作业发出 SIGHUP 信号。如果没有给出 jobspec， 也没有给出 -a 或者 -r 选项，将使用当前作业 (current job)。如果没有给出 jobspec， 选项 -a 意味着删除或标记所有作业；选项 -r 不带 jobspec  参数时限制操作只对正在运行的作业进行。返回值是0，除非 jobspec 不指定有效的作业。

使得在  shell 接到 SIGHUP 信号时，不会向作业发出 SIGHUP 信号

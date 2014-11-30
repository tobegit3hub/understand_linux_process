
## 进程文件

在Linux中，“一切皆文件”，进程的一切运行信息(占用CPU、内存等)都可以在文件系统找到，例如看一下PID为1的进程信息。

```
root@87096bf68cb2:/go/src# ls /proc/1/
attr        cmdline          cwd      fdinfo   loginuid   mounts      numa_maps      pagemap      sessionid  status   wchan
auxv        comm             environ  gid_map  maps       mountstats  oom_adj        personality  smaps      syscall
cgroup      coredump_filter  exe      io       mem        net         oom_score      projid_map   stat       task
clear_refs  cpuset           fd       limits   mountinfo  ns          oom_score_adj  root         statm      uid_map
```

我们可以看一下它的运行状态，通过`cat /proc/1/status`即可。

```
root@87096bf68cb2:/go/src# cat /proc/1/status
Name:   bash
State:  S (sleeping)
Tgid:   1
Ngid:   0
Pid:    1
PPid:   0
TracerPid:      0
Uid:    0       0       0       0
Gid:    0       0       0       0
FDSize: 256
Groups:
VmPeak:    20300 kB
VmSize:    20300 kB
VmLck:         0 kB
VmPin:         0 kB
VmHWM:      3228 kB
VmRSS:      3228 kB
VmData:      408 kB
VmStk:       136 kB
VmExe:       968 kB
VmLib:      2292 kB
VmPTE:        60 kB
VmSwap:        0 kB
Threads:        1
SigQ:   0/3947
SigPnd: 0000000000000000
ShdPnd: 0000000000000000
SigBlk: 0000000000010000
SigIgn: 0000000000380004
SigCgt: 000000004b817efb
CapInh: 00000000a80425fb
CapPrm: 00000000a80425fb
CapEff: 00000000a80425fb
CapBnd: 00000000a80425fb
Seccomp:        0
Cpus_allowed:   1
Cpus_allowed_list:      0
Mems_allowed:   00000000,00000001
Mems_allowed_list:      0
voluntary_ctxt_switches:        684
nonvoluntary_ctxt_switches:     597
```

参考Linux手册可以看到更多信息，我们这不再深究，我们要知道的是`ps`命令获得的数据其实在文件系统也可以看到。

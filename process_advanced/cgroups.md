
## Cgroups

Cgroups全称Control Groups，是Linux内核用于资源隔离的技术。目前Cgroups可以控制CPU、内存、磁盘访问。

## 使用

Cgroups是在Linux 2.6.24合并到内核的，不过项目在不断完善，3.8内核加入了对内存的控制(kmemcg)。

要使用Cgroups非常简单，阅读前建议看sysadmincasts的视频，https://sysadmincasts.com/episodes/14-introduction-to-linux-control-groups-cgroups。

我们首先在文件系统创建Cgroups组，然后修改这个组的属性，启动进程时指定加入的Cgroups组，这样进程相当于在一个受限的资源内运行了。

## 实现

Cgroups的实现也不是特别复杂。有一个特殊的数据结构记录进程组的信息。

有人可能已经知道Cgroups是Docker容器技术的基础，另一项技术也是大名鼎鼎的Namespaces。

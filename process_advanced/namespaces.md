
## Namespaces简介

Linux Namespaces是资源隔离技术，在2.6.23合并到内核，而在3.12内核加入对用户空间的支持。

Namespaces是容器技术的基础，因为有了命名空间的隔离，才能限制容器之间的进程通信，像虚拟内存对于物理内存那样，开发者无需针对容器修改已有的代码。

## 使用Namespaces

阅读以下教程前建议看看，<https://blog.jtlebi.fr/2013/12/22/introduction-to-linux-namespaces-part-1-uts/>。

Linux内核提供了`clone`系统调用，创建进程时使用`clone`取代`fork`即可创建同一命名空间下的进程。

更多参数建议`man clone`来学习。

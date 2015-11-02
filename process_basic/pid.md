
## PID

首先我们来学习PID这个概念，PID全称Process ID，是标识和区分进程的ID。Linux系统保证不会同时存在两个进程拥有相同的PID，但在一个进程结束之后，其PID可能会再次被分配给新进程，参见StackOverflow上的[问题](http://unix.stackexchange.com/questions/26677/will-process-ids-be-recycled-what-if-you-reach-the-maximal-id)。

原来Hello World进程运行时也有一个PID，只是它运行结束后PID也释放了，我们可以通过print_pid.go程序显示当前进程的PID。

## 示例程序

程序print_pid.go的源码如下，通过`Getpid()`函数可以获得当前进程的PID。

```golang
package main

import (
  "fmt"
  "os"
)

func main() {
    fmt.Println(os.Getpid())
}
```

## 运行结果

```
root@87096bf68cb2:/go/src# go run print_pid.go
2922
root@87096bf68cb2:/go/src# go run print_pid.go
2932
```

可以看出，进程运行时PID是由操作系统随机分配的，同一个程序运行两次会产生两个进程，当然也就有两个不同的PID。

那PID究竟有什么用呢？我们稍后会讨论，现在先了解下PPID。

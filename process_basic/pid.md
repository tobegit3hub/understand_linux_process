
## 进程ID

PID全称Process ID，是标识和区分进程的ID，对我们而言它是全局唯一的正整数。

Hello World进程运行时也有一个PID，只是它运行结束后PID也释放了，我们可以通过print_pid.go程序显示当前进程的PID。

## 输出PID

程序print_pid.go的源码如下。

```
package main

import (
  "fmt"
  "os"
)

func main() {
    fmt.Println(os.Getpid())
}
```

两次的运行结果如下。

```
root@87096bf68cb2:/go/src# go run print_ppid.go
2922
root@87096bf68cb2:/go/src# go run print_ppid.go
2932
```

可以看出，进程运行时PID是随机分配的，同一个程序运行两个会产生两个进程，也就有两个不同的PID。PID的作用我们后面会深入地讨论，接下来探讨PPID。

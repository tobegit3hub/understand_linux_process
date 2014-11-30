
## 父进程ID

每个进程除了PID还有PPID，也就是父进程ID，通过PPID可以找到父进程的信息。

## 输出PPID

要想获得进程的PPID，可以通过以下print_ppid.go的程序来输出，代码如下。

```
package main

import (
  "fmt"
  "os"
  )

func main() {
    fmt.Println(os.Getppid())
}
```

执行print_ppid.go后得到以下的输出。

```
root@87096bf68cb2:/go/src# go run print_ppid.go
2892
root@87096bf68cb2:/go/src# go run print_ppid.go
2902
```

有趣的事情发生了，有没有发现每次运行的父进程ID都不一样，这不符合我们的预期啊，原来我们通过`go run`每次都会启动一个Go虚拟机来执行。

如果我们先生成二进制文件再执行结果会怎样呢？

```
root@87096bf68cb2:/go/src# ./print_ppid
1
root@87096bf68cb2:/go/src# ./print_ppid
1
root@87096bf68cb2:/go/src# ps aux |grep "1" |grep -v "ps" |grep -v "grep"
root         1  0.0  0.3  20228  3184 ?        Ss   07:25   0:00 /bin/bash
```

这次我们发现父进程ID都是一样的了，而且通过`ps`命令可以看到父进程就是`bash`，说明通过终端执行命令其实是从`bash`这个进程衍生出各种子进程。

接下来我们看看进程运行时接收的参数。

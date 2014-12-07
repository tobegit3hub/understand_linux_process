
## PPID

每个进程除了一定有PID还会有PPID，也就是父进程ID，通过PPID可以找到父进程的信息。

为什么进程都会有父进程ID呢？因为进程都是由父进程衍生出来的，后面会详细介绍几种衍生的方法。那么跟人类起源问题一样，父进程的父进程的父进程又是什么呢？实际上有一个PID为1的进程是由操作系统创建的，其他子进程都是由它衍生出来，所以前面的描述并不准确，进程号为1的进程并没有PPID。

## 示例程序

要想获得进程的PPID，可以通过以下`Getppid()`这个函数来获得，print_ppid.go程序的代码如下。

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

## 运行结果

```
root@87096bf68cb2:/go/src# go run print_ppid.go
2892
root@87096bf68cb2:/go/src# go run print_ppid.go
2902
```

有趣的事情发生了，有没有发现每次运行的父进程ID都不一样，这不符合我们的预期啊，原来我们通过`go run`每次都会启动一个新的Go虚拟机来执行进程。

## 编译后运行

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

拿到PID和PPID后有什么用呢？马上揭晓。

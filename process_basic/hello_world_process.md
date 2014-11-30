
## Bash进程

最简单的Bash程序只有一行代码，运行后输出“Hello World”就直接退出了。

```
root@87096bf68cb2:/go/src# echo Hello World
Hello World
```

这样我们就已经运行起最简单的进程了，当然我们也可以用Go重写Hello World程序。

## Go进程

所有Go(包括任何编程语言)写的程序都会以进程的形式运行，下面是最简单的Go应用hello_world.go。

```
package main

import (
  "fmt"
)

func main() {
    fmt.Println("Hello World")
}
```

我们运行后得到以下的输出。

```
root@87096bf68cb2:/go/src# go run hello_world.go
Hello World
```

Hello World进程运行时究竟发生了什么，我们马上讲解这部分内容。

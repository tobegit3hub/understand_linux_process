
## Hello World进程

Hello World程序是最简单的应用程序，不是进程哦，它在终端上输出“hello world”后就退出了。当我们运行Hello World程序的时候，就创建了Hello World进程。这也是最简单的进程，它只有正在运行和停止运行两个状态，后面也将会详细介绍进程状态的概念。

## Bash实现

用Bash实现Hello World程序只需要一行代码，运行后输出“Hello World”就直接退出了。

```
root@87096bf68cb2:/go/src# echo Hello World
Hello World
```

这样我们就已经运行起最简单的进程了，当然我们也可以用Go重写Hello World程序。

## Go实现

所有Go(包括任何编程语言)写的程序都会以进程的形式运行，下面是最简单的Go应用hello_world.go。

```golang
package main

import (
  "fmt"
)

func main() {
    fmt.Println("Hello World")
}
```

运行后得到以下的输出。

```
root@87096bf68cb2:/go/src# go run hello_world.go
Hello World
```

Hello World进程运行时究竟发生了什么，接下来我们将从各个方面介绍进程的概念。

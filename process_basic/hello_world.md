## Hello World进程

Hello World程序是每门编程语言的入门示例，注意这个程序还不是进程哦，它的作用是在终端输出“Hello World”然后直接退出。

当我们运行Hello World程序时，系统就创建一个Hello World进程。这也是最简单的进程了，没有系统调用、进程间通信等，输出字符串后就退出了。

## Bash实现

用Bash实现Hello World程序只需要一行代码，运行后新的进程也可以输出“Hello World”，然后就没有然后了。

```
root@87096bf68cb2:/go/src# echo Hello World
Hello World
```

稍微提一下`echo`是Linux自带的程序，可以接受一个或多个参数，反正就是如实地把它们输出到终端而已。

这样最简单的Linux进程就诞生了，当然我们也可以用Go重写Hello World程序。

## Go实现

Go实现的程序源码可参见hello_world.go。

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

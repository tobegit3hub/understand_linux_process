
## 进程名字

每个进程都一定有进程名字，例如我们运行`top`，进程名就是“top”，如果是自定义的程序呢？

其实进程名一般都是进程参数的第一个字符串，在Go中可以这样获得进程名。

```golang
package main

import (
  "fmt"
  "os"
)

func main() {
    processName := os.Args[0]

    fmt.Println(processName)
}
```

进程的输出结果如下。

```
root@87096bf68cb2:/go/src# go run process_name.go
/tmp/go-build650749614/command-line-arguments/_obj/exe/process_name
root@87096bf68cb2:/go/src# go build process_name.go
root@87096bf68cb2:/go/src# ./process_name
./process_name
```

是否稍稍有些意外，因为`go run`会启动进程重新编译、链接和运行程序，因此每次运行的进程名都不相同，而编译出来的程序有明确的名字，所以它的进程的名字都是一样的。

知道这些以后，我们可以开始接触接进程的运行参数。

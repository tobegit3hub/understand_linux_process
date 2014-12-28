
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

是否稍稍有些意外，因为我们直接使用`go run`，是跑在虚拟机上的，而编译出来的程序可以直接运行在操作系统上。

知道这些以后，我们可以开始接触接进程的运行参数。

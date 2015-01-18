
## 创建目录权限

如果你想创建一个目录并授予777权限，你需要怎么做？查看Go的API文档我们可以这样写。

源文件为mkdir.go。

```golang
package main

import (
  "fmt"
  "os"
)

func main() {
    err := os.MkdirAll("/tmp/gotest/", 0777)
    if err != nil {
      panic(err)
    }

    fmt.Println("Mkdir /tmp/gotest/")
}
```

## 运行结果

```
➜  understand_linux_process_examples git:(master) ✗ ll /tmp/
drwxr-xr-x   2 tobe  wheel    68B Dec 30 10:06 gotest
➜  understand_linux_process_examples git:(master) ✗ umask
022
```

## 正确做法

代码在mkdir_umask.go中。

```golang
package main

import (
  "fmt"
  "os"
  "syscall"
)

func main() {
    mask := syscall.Umask(0)
    defer syscall.Umask(mask)

    err := os.MkdirAll("/tmp/gotest/", 0777)
    if err != nil {
      panic(err)
    }

    fmt.Println("Mkdir /tmp/gotest/")
}
```

## 注意事项

这并不是Go的Bug，包括Linux系统调用都是这样的，创建目录除了给定的权限还要加上系统的Umask，Go也是如实遵循这种约定。

如果你想达到你的预期权限，知道Umask及其用法是必须的。


## 执行(Exec)外部程序

这是来自GoByExample的例子，代码在<https://gobyexample.com/execing-processes>。

把新程序加载到自己的内存。

与Spawn不同，执行外部程序并不会返回到原进程中，也就是让外部程序完全取代本进程。

## 代码实现

```golang
package main

import "syscall"
import "os"
import "os/exec"

func main() {
    binary, lookErr := exec.LookPath("ls")
    if lookErr != nil {
        panic(lookErr)
    }
    args := []string{"ls", "-a", "-l", "-h"}
    env := os.Environ()
    execErr := syscall.Exec(binary, args, env)
    if execErr != nil {
        panic(execErr)
    }
}
```

## 运行结果

```
$ go run execing-processes.go
total 16
drwxr-xr-x  4 mark 136B Oct 3 16:29 .
drwxr-xr-x 91 mark 3.0K Oct 3 12:50 ..
-rw-r--r--  1 mark 1.3K Oct 3 16:28 execing-processes.go
```

## 归纳总结

如果你的程序就是用来执行外部程序的，例如后面提到的项目实例Run，那使用`syscall.Exec`执行外部程序就最合适了。注意调用该函数后，本进程后面的代码将不可能再执行了。

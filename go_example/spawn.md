
## 衍生(Spawn)新进程

这是来自GoByExample的例子，代码在<https://gobyexample.com/spawning-processes>。

它能够执行任意Go或者非Go程序，并且等待放回结果，外部进程结束后继续执行本程序。

## 代码实现

```
package main

import "fmt"
import "io/ioutil"
import "os/exec"

func main() {
    dateCmd := exec.Command("date")
    dateOut, err := dateCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> date")
    fmt.Println(string(dateOut))

    grepCmd := exec.Command("grep", "hello")
    grepIn, _ := grepCmd.StdinPipe()
    grepOut, _ := grepCmd.StdoutPipe()
    grepCmd.Start()
    grepIn.Write([]byte("hello grep\ngoodbye grep"))
    grepIn.Close()
    grepBytes, _ := ioutil.ReadAll(grepOut)
    grepCmd.Wait()
    fmt.Println("> grep hello")
    fmt.Println(string(grepBytes))

    lsCmd := exec.Command("bash", "-c", "ls -a -l -h")
    lsOut, err := lsCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> ls -a -l -h")
    fmt.Println(string(lsOut))
}
```

## 运行结果

```
$ go run spawning-processes.go
> date
Wed Oct 10 09:53:11 PDT 2012
> grep hello
hello grep
> ls -a -l -h
drwxr-xr-x  4 mark 136B Oct 3 16:29 .
drwxr-xr-x 91 mark 3.0K Oct 3 12:50 ..
-rw-r--r--  1 mark 1.3K Oct 3 16:28 spawning-processes.go
```

## 归纳总结

因此如果你的程序需要执行外部命令，可以直接使用`exec.Command()`来Spawn进程，并且根据需要获得外部程序的返回值。

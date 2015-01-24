
## 退出码

任何进程退出时，都会留下退出码，操作系统根据退出码可以知道进程是否正常运行。

退出码是0到255的整数，通常0表示正常退出，其他数字表示不同的错误。

## 示例程序

```golang
package main

func main() {
  panic("Call panic()")
}
```

## 运行结果

```
root@fa13d0439d7a:/go/src# go run exit_code.go
panic: Call panic()

goroutine 16 [running]:
runtime.panic(0x425900, 0xc208000010)
/usr/src/go/src/pkg/runtime/panic.c:279 +0xf5
main.main()
/go/src/exit_code.go:4 +0x61

goroutine 17 [runnable]:
runtime.MHeap_Scavenger()
/usr/src/go/src/pkg/runtime/mheap.c:507
runtime.goexit()
/usr/src/go/src/pkg/runtime/proc.c:1445

goroutine 18 [runnable]:
bgsweep()
/usr/src/go/src/pkg/runtime/mgc0.c:1976
runtime.goexit()
/usr/src/go/src/pkg/runtime/proc.c:1445
exit status 2
```

我们可以看到最后一行输出了`exit status 2`，证明进程的退出码是2，也就是异常退出。相比之下，运行Hello World程序并没有输出退出码，也就是进程正常结束了。

## 使用退出码

不管是正常退出还是异常退出，进程都结束了这个退出码有意义吗？

当然有意义，我们在写Bash脚本时，可以根据前一个命令的退出码选择是否执行下一个命令。例如安装Run程序的命令`wget https://github.com/runscripts/run-release/blob/master/0.3.6/linux_amd64/run && sudo run --init`，只有下载脚本成功才会执行后面的安装命令。

[Travis CI](https://travis-ci.org/)是为开源项目提供持续集成的网站，因为测试脚本是由开发者写的，Travis只能通过测试脚本的返回值来判断这次测试是否正常通过。

Docker使用Dockerfile来构建镜像，这是类似Bash的领域定义语言(DSL)，每一行执行一个命令，如果命令的进程退出码不为0，构建镜像的流程就会中止，证明Dockerfile有异常，方便用户排查问题。

了解进程退出码后，我们去看更多的进程资源。


## 退出码介绍

理解Unix进程P86

所有的退出码如下：

* 0表示正常
* 非0表示异常

## 示例

```
package main

func main() {
  panic("Call panic()")
}
```

运行结果如下。

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


## 死锁概念

死锁(Deadlock)就是一个进程拿着资源A请求资源B，另一个进程拿着资源B请求资源A，双方都不释放自己的资源，导致两个进程都进行不下去。

## 示例程序

我们可以写代码模拟进程死锁的例子。

```golang
package main

func main() {
  ch := make(chan int)
  <-ch
}
```

## 运行结果

```
root@fa13d0439d7a:/go/src# go run deadlock.go
fatal error: all goroutines are asleep - deadlock!

goroutine 16 [chan receive]:
main.main()
/go/src/deadlock.go:5 +0x4f
exit status 2
```

这里Go虚拟机已经替我们检测出死锁的情况，因为所有Goroutine都阻塞住没有运行，关于Goroutine的概念有机会详细介绍一下。

我们可能很早就接触过死锁的概念，也很容易模拟出来，那么你是否知道活锁呢？

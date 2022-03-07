
## 捕获SIGKILL

SIGKILL是常见的Linux信号，我们使用`kill`命令杀掉进程也就是向进程发送SIGKILL信号。

和其他信号不同，[SIGKILL](https://en.wikipedia.org/wiki/Unix_signal#SIGKILL)和SIGSTOP是不可被Catch的，因此下面的代码是能编译通过但也是无效的，更多细节可以参考[golang/go#9463](https://github.com/golang/go/issues/9463).

```
c := make(chan os.Signal, 1)
signal.Notify(c, syscall.SIGKILL, syscall.SIGSTOP)
```

## 注意事项

这是Linux内核的限制，这种限制也是为了让操作系统有可能控制进程的生命周期，理解后我们也不应该去尝试捕获SIGKILL。

不过还是有人这样去做，最后结果也不符合预期，这需要我们对底层有足够的理解。

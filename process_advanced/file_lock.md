
## 进程锁

这里的进程锁与线程锁、互斥量、读写锁和自旋锁不同，它是通过记录一个PID文件，避免两个进程同时运行的文件锁。

进程锁的作用之一就是可以协调进程的运行，例如[crontab使用进程锁解决冲突](http://www.live-in.org/archives/1036.html)提到，使用crontab限定每一分钟执行一个任务，但这个进程运行时间可能超过一分钟，如果不用进程锁解决冲突的话两个进程一起执行就会有问题。后面提到的项目实例Run也有类似的问题，通过进程锁可以解决进程间同步的问题。

使用PID文件锁还有一个好处，方便进程向自己发停止或者重启信号。Nginx编译时可指定参数`--pid-path=/var/run/nginx.pid`，进程起来后就会把当前的PID写入这个文件，当然如果这个文件已经存在了，也就是前一个进程还没有退出，那么Nginx就不会重新启动。进程管理工具Supervisord也是通过记录进程的PID来停止或者拉起它监控的进程的。

## 使用进程锁

进程锁在特定场景是非常适用的，而操作系统默认不会为每个程序创建进程锁，那我们该如何使用呢？

其实要实现一个进程锁很简单，通过文件就可以实现了。例如程序开始运行时去检查一个PID文件，如果文件存在就直接退出，如果文件不存在就创建一个，并把当前进程的PID写入文件中。这样我们很容易可以实现读锁，但是所有流程都需要自己控制。

当然根据DRY(Don't Repeat Yourself)原则，Linux已经为我们提供了`flock`接口。

## 使用Flock

Flock提供的是advisory lock，也就是建议性的锁，其他进程实际上也可以读写这个锁文件。Linux上可以直接使用`flock`命令，使用C可以调用原生的`flock`接口，这里详细介绍Go 1.3引入的`FcntlFlock()`。

我们封装了简单的接口。

```
// Control the lock of file.
func fcntlFlock(lockType int16, path ...string) error {
  var err error
  if lockType != syscall.F_UNLCK {
    mode := syscall.O_CREAT | syscall.O_WRONLY
    lockFile, err = os.OpenFile(path[0], mode, 0666)
    if err != nil {
      return err
    }
  }

  lock := syscall.Flock_t{
    Start:  0,
    Len:    1,
    Type:   lockType,
    Whence: int16(os.SEEK_SET),
  }
  return syscall.FcntlFlock(lockFile.Fd(), syscall.F_SETLK, &lock)
}
```

这样对进程加锁。

```
// Lock the file.
func Flock(path string) error {
  return fcntlFlock(syscall.F_WRLCK, path)
}
```

这样对进程解锁。

```
// Unlock the file.
func Funlock(path string) error {
  err := fcntlFlock(syscall.F_UNLCK)
  if err != nil {
    return err
  } else {
    return lockFile.Close()
  }
}
```

学习完进程锁，我们开始了解各种进程，如孤儿进程、僵尸进程。

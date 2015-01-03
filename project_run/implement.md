## 实现Run


## 实现Flock

前面提到进程的文件锁，实际上Run也用到了，可以试想下以下的场景。

用户A执行`run pt-summary`，由于本地已经缓存了所以会直接运行本地的脚本。同时用户B执行`run -u pt-summary`，加上`-u`或者`--update`参数后Run会从远端下载并运行最新的脚本。如果不加文件锁的话，用户A的行为就不可预测了，而文件锁很好得解决了这个问题。

具体使用方法如下，我们封装了以下的接口。

```golang
var lockFile *os.File

// Lock the file.
func Flock(path string) error {
    return fcntlFlock(syscall.F_WRLCK, path)
}

// Unlock the file.
func Funlock(path string) error {
    err := fcntlFlock(syscall.F_UNLCK)
    if err != nil {
      return err
    } else {
        return lockFile.Close()
    }
}

// Control the lock of file.
func fcntlFlock(lockType int16, path ...string) error {
    var err error
    if lockType != syscall.F_UNLCK {
      mode := syscall.O_CREAT | syscall.O_WRONLY
      mask := syscall.Umask(0)
      lockFile, err = os.OpenFile(path[0], mode, 0666)
      syscall.Umask(mask)
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

在运行脚本前就调用锁进程的方法。

```
// Lock the script.
lockPath := cacheDir + ".lock"
err = flock.Flock(lockPath)
if err != nil {
  utils.LogError("%s: %v\n", lockPath, err)
  os.Exit(1)
}
```

## 实现HTTP请求

使用Run时它会自动从网上下载脚本，走的HTTP协议，具体实现方法如下。

```golang
// Retrieve a file via HTTP GET.
func Fetch(url string, path string) error {
  response, err := http.Get(url)
  if err != nil {
    return err
  }
  if response.StatusCode != 200 {
    return Errorf("%s: %s", response.Status, url)
  }

  defer response.Body.Close()
  body, err := ioutil.ReadAll(response.Body)
  if err != nil {
    return err
  }
  if strings.HasPrefix(url, MASTER_URL) {
    // When fetching run.conf, etc.
    return ioutil.WriteFile(path, body, 0644)
    } else {
      // When fetching scripts.
      return ioutil.WriteFile(path, body, 0777)
    }
}
````

Run的总体代码是很简单的，主要是通过解析run.conf下载相应的脚本并执行。

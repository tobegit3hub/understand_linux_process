## Nohup命令

每个开发者都会躺过这个坑，在命令行跑一个后台程序，关闭终端后发现进程也推出了，网上搜一下发现要用`nohup`，究竟什么原因呢？

原来普通进程运行时默认会绑定TTY(虚拟终端)，关闭终端后系统会给上面所有进程发送TERM信号，这时普通进程也就退出了。当然还有些进程不会退出，这就是后面将会提到的守护进程。

使用`nohup`的原因很简单，它能让进程脱离终端运行，关闭终端后也不会给进程发信号。

## 举个例子

我们用Go实现最简单的Web服务器，代码web_server.go如下。


```golang
package main

import (
  "fmt"
  "net/http"
)

  func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Println("Handle request")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8000", nil)
}
```

然后在终端上运行，并测试一下。

```
➜ go build web_server.go
➜ ./web_server &
[1] 25967
➜  wget 127.0.0.1:8000
--2014-12-28 22:24:07--  http://127.0.0.1:8003/
Connecting to 127.0.0.1:8003... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5 [text/plain]
Saving to: 'index.html.4'

100%[======================>] 5           --.-K/s   in 0s

2014-12-28 22:24:07 (543 KB/s) - 'index.html' saved [5/5]
```

如果关闭终端，`curl`命令就连不上我们的Web服务器了。如果使用`nohup`运行呢？

```
➜ go build web_server.go
➜ ./web_server &
[1] 25968
➜ exit
➜ wget 127.0.0.1:8003
--2014-12-28 22:24:11--  http://127.0.0.1:8003/
```

发现关闭终端对Web服务器进程没有任何影响，这正是我们预期的。这是运行守护进程最简单的方法，实际上标准的守护进程除了处理信号外，还要考虑这种因素，后面将会详述。

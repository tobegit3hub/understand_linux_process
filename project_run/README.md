


Run是开源的脚本管理工具，官方网站http://runscripts.org，项目地址https://github.com/runscripts/run。

Run可以执行任意的脚本，当然使用到Go库提供的系统调用程序。它的实现很简单，源码。

```
import os

func RunCommand(cmd string) {
     os.exec()
}

```

## 进程输入与输出

每个进程操作系统都会分配三个文件资源，分别是标准输入(STDIN)、标准输出(STDOUT)和错误输出(STDERR)。通过这些输入流，我们能够轻易得从键盘获得数据，然后在显示器输出数据。

## 标准输入

来自管道(Pipe)的数据也是标准输入的一种，我们写了以下的实例来输出标注输入的数据。

```golang
package main

import (
  "fmt"
  "io/ioutil"
  "os"
)

func main() {
  bytes, err := ioutil.ReadAll(os.Stdin)
  if err != nil {
    panic(err)
  }

  fmt.Println(string(bytes))
}
```

运行结果如下。

```
root@87096bf68cb2:/go/src# echo string_from_stdin | go run stdin.go
string_from_stdin
```

## 标准输出

通过`fmt.Println()`把数据输出到屏幕上，这就是标准输出了，这里不太演示了。

## 错误输出

程序的错误输出与标准输出类似，这里暂不演示。

了解完进程一些基础概念，我们马上要深入学习并发与并行的知识了。

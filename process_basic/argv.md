
## 进程参数

任何进程启动时都可以赋予一个字符串数组作为参数，一般名为ARGV或ARGS。

通过解析这些参数可以让你的程序更加通用，例如`cp`命令通过给定两个参数就可以复制任意的文件，当然如果需要的参数太多最好还是使用配置文件。

## 获得进程Argument

进程参数一般可分为两类，一是Argument，也就是作为进程运行的实体参数。例如`cp config.yml config.yml.bak`的这两个参数。

设计Go程序时可以轻易地获得这些参数，argument.go代码如下，代码来自<https://gobyexample.com/command-line-arguments>。

```
package main

import "os"
import "fmt"

func main() {
  argsWithProg := os.Args
  argsWithoutProg := os.Args[1:]

  arg := os.Args[3]
  fmt.Println(argsWithProg)
  fmt.Println(argsWithoutProg)
  fmt.Println(arg)
}
```

运行效果如下。

```
$ go build command-line-arguments.go
$ ./command-line-arguments a b c d
[./command-line-arguments a b c d]
[a b c d]
c
```

可以看出通过`os.Args`，不管是不是实体参数都可以获得，但是对于类似开关的辅助参数，Go提供了另一种更好的方法。

## 获得进程Flag

使用Flag可以更容易得将命令行参数转化成我们需要的数据类型，其中flag.go代码如下，代码来自<https://gobyexample.com/command-line-flags>。

```
package main

import "flag"
import "fmt"

func main() {
  wordPtr := flag.String("word", "foo", "a string")
  numbPtr := flag.Int("numb", 42, "an int")
  boolPtr := flag.Bool("fork", false, "a bool")
  var svar string
  flag.StringVar(&svar, "svar", "bar", "a string var")

  flag.Parse()

  fmt.Println("word:", *wordPtr)
  fmt.Println("numb:", *numbPtr)
  fmt.Println("fork:", *boolPtr)
  fmt.Println("svar:", svar)
  fmt.Println("tail:", flag.Args())
}
```

运行结果如下，相比直接使用`os.Args`代码也简洁了不少。

```
root@87096bf68cb2:/go/src# ./flag -word=opt -numb=7 -fork -svar=flag
word: opt
numb: 7
fork: true
svar: flag
tail: []
root@87096bf68cb2:/go/src# ./flag -h
Usage of ./flag:
  -fork=false: a bool
  -numb=42: an int
  -svar="bar": a string var
  -word="foo": a string
```

使用Go获取进程参数是很简单的，不过一旦参数太多，最佳实践还是使用配置文件。

进程参数只有在启动进程时才能赋值，如果需要在程序运行时进行交互，就需要了解进程的输入与输出了。

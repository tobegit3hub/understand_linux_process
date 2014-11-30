
## Docker简介

[Docker](https://www.docker.com/)是一个容器运行平台，你可以将程序及其依赖打包成容器，在不同机器上运行可得到一致的运行效果。因为不同的系统环境或Go版本可能影响程序的运行结果，为了得到可预测、可重复的实验环境，我们引入了Docker。

## Docker使用

我们不仅开源了示例代码，还创建了官方[Docker镜像](https://registry.hub.docker.com/u/tobegit3hub/understand_linux_process_examp/)。

只要运行Docker容器`docker run -i -t tobegit3hub/understand_linux_process_examp`，就可以马上创建本书的测试环境。

```
root@6a8e36a53495:/go/src# go run hello_world.go
Hello World
```

当然你也可以运行自己的Go示例，直接使用官方镜像`docker run -i -t golang:1.4 /bin/bash`。

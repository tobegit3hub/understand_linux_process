## Docker简介

![](image/docker.png)

[Docker](https://github.com/docker/docker)是一个容器运行平台，你可以将程序及其依赖打包成容器，在不同机器上运行可得到一致的运行效果。因为不同的系统环境或Go版本可能影响程序的运行结果，为了得到可预测、可重复的实验环境，我们引入了Docker容器技术。

## Docker使用

我们不仅开源了示例代码，还创建了官方[Docker镜像](https://registry.hub.docker.com/u/tobegit3hub/understand_linux_process_examp/)。

只要执行命令`docker run -i -t tobegit3hub/understand_linux_process_examp`，就可以马上创建本书的实验环境。进入容器后可以轻易地运行示例程序。

```
root@6a8e36a53495:/go/src# go run hello_world.go
Hello World
```

当然你也可以在本地运行自己的Go示例，或者使用官方Go镜像`docker run -i -t golang:1.4 /bin/bash`。

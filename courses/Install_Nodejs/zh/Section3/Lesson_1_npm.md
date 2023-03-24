# npm命令

## 什么是npm源

前面的课程我们说到，npm 就是个包管理系统，上面有无数的开源库可以通过 npm 命令下载，那么是从哪里下载的呢？就是这个 npm源。

我们在控制台执行下面命令可以查看当前配置的源：

```shell
npm config get registry
```

![screenshot](https://live.staticflickr.com/65535/52766683322_cd3914151e.jpg)

## 怎么切换

默认配置的源，国内使用可能存在安装慢的问题，我们可以使用以下命令切换源：

```shell
npm config set registry=https://xxx
```

这里推荐几个国内的镜像：

* https://registry.npmmirror.com 阿里云
* https://mirrors.cloud.tencent.com/npm 腾讯云

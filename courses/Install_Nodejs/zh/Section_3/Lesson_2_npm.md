# npm命令

我们在控制台执行 npm 命令可以查看当前配置的源：

```shell
npm config get registry
```

![screenshot](https://live.staticflickr.com/65535/52766683322_cd3914151e.jpg)

我们也可以使用 npm 命令切换源：

```shell
npm config set registry=https://xxx
```

这里推荐几个国内的镜像：

* https://registry.npmmirror.com 阿里云
* https://mirrors.cloud.tencent.com/npm 腾讯云

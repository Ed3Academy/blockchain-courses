# nrm

## 什么是 npm 源

前面的课程我们说到，npm 就是个包管理系统，上面有无数的开源库可以通过 npm 命令下载，那么是从哪里下载的呢？就是这个 npm 源。

默认配置的源，国内使用可能存在安装慢的问题，所以我们需要切换更快的源。

## 什么是 nrm

首先我们来介绍 nrm 这款工具。

[nrm](https://github.com/Pana/nrm) 是一款 npm 源切换管理工具 （NPM registry manager），通过命令快速切换 npm 源。

它也是一个 npm 包，首先来全局安装它：

```shell
npm install -g nrm open@8.4.2 --save
```

安装成功后，查看所有可选的源：

```shell
nrm ls
```

![screenshot](https://live.staticflickr.com/65535/52767547489_ed5e67d053_z.jpg)

使用某个源：

```shell
nrm use taobao
```

![screenshot](https://live.staticflickr.com/65535/52767709775_a5cb4069f8_z.jpg)

OK！大功告成！之后就愉快地安装 npm 包吧🥰

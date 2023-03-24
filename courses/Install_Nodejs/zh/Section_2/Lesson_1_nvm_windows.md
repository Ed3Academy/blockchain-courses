# nvm-windows

[nvm](https://github.com/nvm-sh/nvm) 即 Node 版本管理器（Node Version Manager），它能够在电脑中安装多个版本的 Node.js 。

由于它的安装有点复杂，我们首先介绍这款 nvm-windows ，傻瓜式安装。

访问 nvm-windows 的 [GitHub Releases](https://github.com/coreybutler/nvm-windows/releases) 页面，找到最新版本的 .exe 安装包，下载安装即可。

![screenshot](https://live.staticflickr.com/65535/52766646312_9a8ba12678_b.jpg)

之后就是傻瓜式安装了，安装成功后就能在控制台使用一系列 nvm 命令：

安装最新稳定版的 Node.js ：

```shell
nvm install lts
```

安装指定版本的 Node.js ：

```shell
nvm install 18.15.0
```

查看已安装的 Node.js 版本：

```shell
nvm list
```

使用指定版本的 Node.js ：

```shell
nvm use 18.15.0
```

这样便能在不同的项目中执行 `use` 使用不同的 Node.js 版本了。

![screenshot](https://live.staticflickr.com/65535/52767204351_958c681eba_z.jpg)

更多命令可以通过 `nvm` 查看😀

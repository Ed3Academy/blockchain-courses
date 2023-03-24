# 通过nvm安装

## nvm

上节说到对 Node.js 有多版本要求的问题，那么 nvm 应运而生。

![Node.js](https://live.staticflickr.com/65535/52767790633_4341c827d0_z.jpg)

[nvm](https://github.com/nvm-sh/nvm) 即Node版本管理器（Node Version Manager）。它支持通过命令行安装和切换不同版本的Node.js。

安装 nvm 首先需要创建以下文件中的一个：

`~/.bash_profile`，`~/.zshrc`，`~/.profile`，`~/.bashrc`

其次需要在 bash 中执行下面的命令：

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

或

```shell
curl -o- https://raw.gitmirror.com/nvm-sh/nvm/master/install.sh | bash
```

所以Windows用户需要安装 [Git](https://git-scm.com/downloads) 并使用 `Git Bash` 执行。

之后重启 bash 并执行 nvm ，如果能打印出以下内容说明安装成功了。

![screenshot](https://live.staticflickr.com/65535/52765833306_60ccb1ba07_b.jpg)

接下来就可以在命令行使用 nvm 安装 Node.js 了。

安装最新版本的 Node.js ：

```shell
nvm install node
```

安装指定版本的 Node.js ：

```shell
nvm install 18.15.0
```

查看已安装的 Node.js 版本：

```shell
nvm ls
```

使用最新版本的 Node.js ：

```shell
nvm use node
```

使用指定版本的 Node.js ：

```shell
nvm use 18.15.0
```

这样便能在不同的项目中执行 `use` 使用不同的 Node.js 版本了。

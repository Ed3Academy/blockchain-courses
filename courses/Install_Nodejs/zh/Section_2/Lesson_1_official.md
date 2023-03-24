# 官网下载安装

[Node.js官网](https://nodejs.org/zh-cn/download)下载安装是最靠谱的方式。

![download](https://live.staticflickr.com/65535/52765636216_ad181b666b_h.jpg)
根据你的系统，选择对应的安装包。以 Windows 系统为例，下载 .msi 安装更适合入门的同学，安装过程简单，也会帮我们配置好环境变量。

安装好之后，通过执行：

```shell
node -v
```

![screenshot](https://live.staticflickr.com/65535/52766628137_b9070cb112_w.jpg)

如果出现对应版本号，则说明我们安装成功了😀

但是这样安装也有弊端，我们的环境只有一个 Node.js 版本，如果遇到多个项目对 Node.js 版本要求不同的时候，就很棘手了🤯。

# 什么是npm和npx

## npm

npm 即 Node 包管理系统（Node Package Manager）。

如果安装了 Node.js，就会配套安装 npm，此时我们就可以全局执行 npm 命令了，例如最常用的 `npm install`。

数以万计的前端开源库，都会将代码发布到 [npm官网](https://www.npmjs.com/)，然后开发者就可以通过 npm 命令将这些库安装到自己项目中。

## npx

npm 从 5.2.0 版本开始，新增了 npx 命令。npx 解决了什么问题？我们来看一个场景。

假如我们的前端项目安装了 ESLint（代码检查工具），想要在控制台执行某些 ESLint 命令，只能在 `package.json` 中指定一个 `script`：

```json
{
  "scripts": {
    "eslint": "eslint -v"
  }
}
```

然后执行 `npm run eslint`才能执行该命令。

但是有了 npx 之后，我们可以直接在控制台执行 `npx eslint -v`。也就是说，npx 是一个执行命令，能够直接调用项目内部安装的模块。

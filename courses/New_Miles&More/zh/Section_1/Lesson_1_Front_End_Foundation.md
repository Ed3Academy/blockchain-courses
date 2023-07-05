# 📊搭建基础的前端项目

## **🚧 教学目标**

1. 了解DApp架构
2. 了解DApp前端技术栈 Next.js、wagmi、RainbowKit

## **💚 任务描述**

 Miles&More星享计划，需要搭建一个DApp前端项目，用于向客户展示机票及优惠券信息。

本次任务要求：

1. 完成前端运行环境及开发环境的安装
2. 完成 DApp 前端框架的搭建
3. 完成钱包插件添加，钱包账号的申请及前端与钱包的连接。

## **⚡ 相关知识**

 后面替换为AI课件，可以先参看AI脚本：
 
 <a href="https://docs.qq.com/sheet/DUUhnakNjSkZWWkt0?tab=ji2ydj" target="_blank">AI课件</a>

## **✨ 任务操作步骤**

1、完成前端运行环境及开发环境的安装

a）安装Node.js：下载并安装最新LTS版本的Node.js，这里不提供操作演示。

软件下载链接：[https://nodejs.cn/download/](https://nodejs.cn/download/)

b）安装VS Code:下载并安装最新LTS版本的VS Code，这里不提供操作演示。

软件下载连接：[https://code.visualstudio.com/](https://code.visualstudio.com/)

2、完成 DApp 前端框架的搭建a）安装Next.js.模板

启动VS Code，在控制台执行命令行：`npx create-next-app@latest`

备注：控制台如果找不到，在VSCode菜单项选择“View(视图)”，下拉选择“Terminal(终端)”

![final-web.png](https://i.postimg.cc/7LLF7bQP/t1-01.png)

b）安装依赖项(RainbowKit、ethers、wagmi)
   进入项目目录，执行命令行：

```
npm install "@rainbow-me/rainbowkit@0.12.14" "ethers@5.7.2" "wagmi@0.12.13"
```

![final-web.png](https://i.postimg.cc/nzVbqDvM/t1-02.png)

 c）导入wagmi和RainbowKit

用VSCode打开创建的前端项目，打开下方链接文件，复制文件代码并将其覆盖pages/_app.js文件：
 [https://gitee.com/ed3-academy/miles-more-web/blob/main/pages/_app.js](https://gitee.com/ed3-academy/miles-more-web/blob/main/pages/_app.js)

![final-web.png](https://i.postimg.cc/pX9MHSKd/t1-03.png)

d）在项目首页中添加钱包连接按钮

   打开下方链接文件，并将其覆盖pages/index.js文件：
[https://gitee.com/ed3-academy/miles-more-eb/blob/main/pages/index1.js](https://gitee.com/ed3-academy/miles-more-eb/blob/main/pages/index1.js)

![final-web.png](https://i.postimg.cc/263CMSwH/t1-04.png)

e）修改首页样式，美化首页

   打开下方链接文件，并将其覆盖pages/index.js文件：
[https://gitee.com/ed3-academy/miles-more-eb/blob/main/pages/index1.js](https://gitee.com/ed3-academy/miles-more-eb/blob/main/pages/index1.js)

![final-web.png](https://i.postimg.cc/WbV1bTb4/t1-05.png)

f）浏览前端页面

   1）控制台执行命令行：`npm run dev`

   2）打开浏览器访问http://localhost:3000

3、前端与钱包连接

a）添加钱包：在Edge浏览器上添加metamask钱包插件，搜索钱包扩展，并进行安装。

![final-web.png](https://i.postimg.cc/5NPnXwHj/t1-22.png)

b）申请钱包账号：在metamask钱包中创建区块链账号。

![final-web.png](https://i.postimg.cc/L2zJYP3C/t1-18.png)

c）完成钱包连接：在前端页面中点击钱包连接按钮根据步骤提示完成钱包连接。

![final-web.png](https://i.postimg.cc/SspwFPs3/t1-19.png)

![final-web.png](https://i.postimg.cc/qq6ns1cB/t1-20.png)

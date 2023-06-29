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

 后面替换为AI课件，可以先参看AI脚本：https://docs.qq.com/sheet/DUUhnakNjSkZWWkt0?tab=xib91a&_t=1683512609544

## **✨ 任务操作步骤**

1、完成前端运行环境及开发环境的安装

a）安装Node.js:下载并安装最新LTS版本的Node.js，这里不提供操作演示，可以参看：https://nodejs.cn/download/

b）安装VS Code:下载并安装最新LTS版本的VS Code，这里不提供操作演示，可以参看：https://code.visualstudio.com/

2、完成 DApp 前端框架的搭建
a）安装Next.js.模板
启动VS Code，在控制台执行npx create-next-app@latest

![final-web.png](https://i.postimg.cc/7LLF7bQP/t1-01.png)

b）安装依赖项(RainbowKit、ethers、wagmi)
   进行项目目录，执行命令npm install "@rainbow-me/
   rainbowkit@0.12.14" "ethers@5.7.2" "wagmi@0.12.13"

![final-web.png](https://i.postimg.cc/nzVbqDvM/t1-02.png)

   c）导入wagmi和RainbowKit：下载链接文件并覆盖pages/_app.js文件：
   https://gitee.com/ed3-academy/miles-more-web/blob/main/pages/_app.js
4. 完成钱包插件添加，钱包账号的申请及前端与钱包的连接。

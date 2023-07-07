# 📊创建拍卖合约

## **🚧 学习目标**

1. 了解solidity语言及solidity源文件结构
2. 了解和掌握solidity值类型
3. 了解和掌握变量的声明，变量的类型及变量可见性
4. 了解和掌握函数的声明及函数修饰符payable
5. 了解以太单位
6. 了解开发环境Remix如何开发，编译和部署合约

## **💚 任务描述**

我们准备开发一个盲拍系统，为了确保拍卖过程的公平、公正和公开，决定采用智能合约作为核心技术。智能合约能够提供自动化的拍卖流程、明确的拍卖规则、参与者的匿名性以及交易的透明性，从而确保每个参与者在公平的环境中进行竞拍，减少欺诈和操纵的风险，并保障每个参与者的权益。通过智能合约，可以确保拍卖过程的可信度和可验证性，增强参与者的信任。
 在盲拍系统中，首先需要建立一个竞拍合约，用于保存当前最高出价者与出价数值。

本务要求：

1.创建竞拍合约；

2.用户A投标

3.合约测试

## **⚡ 相关知识**

 后面替换为AI课件，可以先参看AI脚本：

<a href="https://docs.qq.com/sheet/DUUhnakNjSkZWWkt0?tab=ji2ydj" target="_blank">AI课件</a>

## **✨ 任务实现**

1、关键步骤说明

* 创建一个竞拍方法
* 接收用户传递进来的拍卖原生币；
* 记录拍卖最高价与出价人
* 使用remix测试

2、合约代码实现

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

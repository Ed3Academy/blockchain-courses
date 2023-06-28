# 本地合约环境搭建

## 教学目标

| 1、了解DApp中的测试环境      |
| :--------------------------- |
| 2、了解Hardhat开发环境是什么 |
| 3、掌握hardhat环境搭建和配置 |

## 任务描述

本地hardhat环境稳定、扩展性强，可用于自动化测试、部署和调试，适合作为项目的本地脚手架。“Miles&More星享计划”文件层级较多，需要使用自动化测试、部署和调试，因此选择hardhat作为本地IDE环境框架。
本次任务要求：
1、完成测试用原生币获取
2、完成本地脚手架搭建、测试

## 相关知识

### 一、DApp运行环境

1、公共主链

公共主网，也叫“主链”，是可公开验证的实际开源区块链本身，类比web2.0就是生产环境。在主网上每次进行的交易、每次更改以及每次启动项目时都需要支付区块链费用，所以在主网上进行测试需要花费昂贵的“测试成本”。

2、公共测试链

公共测试网，是一个测试区块链网络，用于在区块链或区块链项目正式部署前的测试。

3、本地测试网

本地测试网，是一个搭建在你本地机器上的区块链网络，它是私有的。并且它也是免费的，我们可以通过一些方式拿到无价值“代币”用于支付部署和操作的交易费用。

4、私有链

私有链 ，由集中管理者进行管理限制，只有内部少数人可以使用，信息不公开的区块链系统。

5、为什么需要领取原生币

要在公共测试网中部署智能合约或发送资金同样需要支付费用，这就需要测试网的代币来执行交易（无价值），那么你所做的就是打开水龙头获取一些交易代币。

### 二、Hardhat开发环境

1、Hardhat介绍

常用的主流框架：Remix，Truffle，Brownie，Hardhat，Foundry，其中Hardhat是目前最受开发者欢迎的开发框架。几种主流框架的对比详看下图：![final-web.png](https://i.postimg.cc/bY1mMVtT/t3-01.png)

2、Hardhat开发环境特点

* Hardhat 是一个基于以太坊的开发环境，用于编译、测试、部署和调试智能合约。
* Hardhat的设计理念是基于任务和插件，开发人员可以根据自己的需求选择合适的插件来扩展框架的功能。
* Hardhat内置了Hardhat Network，可以用于模拟以太坊网络的行为和功能，便于开发和测试智能合约。

## 操作步骤

**1、在Mumbai测试链水龙头领取测试用原生币Matic**

（1）Metamask钱包添加Mumbai测试链

a）登录https://chainlist.org/，该网站提供了一个链的列表，用于查找和探索不同的区块链网络。在 Chainlist 上，您可以查找各种区块链网络，包括公链、联盟链和私有链。

b）搜索Mumbai测试网，将其添加到钱包

![final-web.png](https://i.postimg.cc/TwvprK3r/t3-06.png)

![final-web.png](https://i.postimg.cc/dVMPQrHm/t3-07.png)

![final-web.png](https://i.postimg.cc/qRtHMwTw/t3-09.png)

![final-web.png](https://i.postimg.cc/FR25fSF4/t3-08.png)

![final-web.png](https://i.postimg.cc/KjBXtY4g/t3-10.png)

c）查看钱包是否已连接Mumbai

![final-web.png](https://i.postimg.cc/tRxGKBDC/t3-11.png)

（2）申请原生币

a）访问网址https://faucet.polygon.technology/，它是Polygon网络的一个水龙头（faucet）服务。在区块链领域，水龙头是一种提供免费加密货币的服务，用于帮助开发者获取测试网络上的代币。通过访问该网址，开发者可以申请一定数量的Polygon代币，以便在Polygon网络上进行测试和开发。

b）点击工具栏上的钱包扩展程序图标，在弹框中复制钱包的地址
c）将复制的地址黏贴至获取测试币相应的钱包地址文本框中，点击提交。

![final-web.png](https://i.postimg.cc/WzCGc27S/t3-03.png)

d）平台会弹出获取发放确认框，点击确认，弹出提示请求处理中。

![final-web.png](https://i.postimg.cc/FRqd7TJH/t3-04.png)

![final-web.png](https://i.postimg.cc/fRj24rVX/t3-05.png)

e） 打开钱包，查看原生币

![final-web.png](https://i.postimg.cc/K8HKYZ3y/t3-12.png)

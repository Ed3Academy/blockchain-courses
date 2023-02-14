# DApp前端
了解DApp前端的工具与框架

## 前置知识
传统前端开发知识，例如：React

## 建议了解的知识

- DApp前端开发框架，与传统Web类似。用的最多的是Next.js
- 前端区块链客户端类库。根本是web3.js或者ethers.js，建议使用基于ethers的封装类库wagmi
- 钱包连接插件：建议使用基于wagmi的Web3Modal/RainbowKit类库。（如果不是直接钱包，而是DID，半托管钱包等身份认证方式另说，例如Web3Auth和MagicLink）
- 其他辅助类库：SWR
- 其他前端样式组件库：tailwindcss、Chakra-UI等，传统的MUI，NextUI，antd等也算


## 实战教程

- 选择的前端开发框架教程，例如：create-react-APP、Vue、Next.js等
    - [Next.js](https://nextjs.org/)，建议使用脚手架起步
- 区块链辅助类库[wagmi](https://wagmi.sh/zh-CN)
- 钱包连接插件
  - [Web3Modal](https://github.com/WalletConnect/web3modal)
  - [RainbowKit](https://github.com/rainbow-me/rainbowkit)
- DApp入门教程：
  - Web3-react：<https://dev.to/yakult/tutorial-build-dapp-with-web3-react-and-swr-1fb0>
  - Web3Modal：<https://igor.technology/building-web3-frontend-for-your-dapp/>
  - Rainbowkit: <https://learnblockchain.cn/article/4624>

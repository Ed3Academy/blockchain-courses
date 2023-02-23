# 智能合约Solidity
智能合约Solidity的入门，实战和进阶

## 入门
> 了解智能合约后，建议solidity官方文档和案例

- 智能合约概念
- Solidity语法
- Remix体验


### 推荐学习资料
> 了解Solidity合约基础，可选remix在线体验合约交互

| 优先度 | 平台 | 类型 | 名称 | 推荐指数 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | 以太坊 | 在线文档 | [《智能合约简介》](https://ethereum.org/zh/developers/docs/smart-contracts/) -- 以太坊文档 | 7.5 | 以太坊官网智能合约介绍，延伸阅读部分比较好，算进阶了。|
| 1 | solidity | 在线文档 | [《solidity语言》](https://docs.soliditylang.org/zh/latest/) -- 官方文档 | 9 | 官方solidity教程，建议学完。同时建议查看[solidity博客](https://blog.soliditylang.org/)中每年的调查问卷，了解主流工具等使用情况。|
| 2 | bilibili | 在线视频课程 | [《智能合约编程》](https://www.bilibili.com/video/BV1yD4y1C7we?p=13&vd_source=2a4014dc01345e3daf837989466323d0) | 7.5 | 火大联合开课吧出品，智能合约快速实战，也可直接在这里学完入门和实战。 |
| 2 | solidity-by-example | 在线实例教程 | [《solidity-by-example》](https://solidity-by-example.org/) | 9 | solidity-by-example 跟着示例代码一步步学，除了基础，还有应用，hack，测试和DeFi。 |
| 2 | remix| 在线教程 | [《remix教程》](https://remix-ide.readthedocs.io/en/latest/index.html) | 8 | 在线的智能合约编辑器，英文版remix教程，如果不习惯，可以搜中文的remix教程。如果想直接在本地进行开发，可以跳过，本地目前建议使用hardhat开发框架 |

## 实战

> solidity实战，建议学习内容

- hardhat开发环境
- ethers.js区块链客户端类库
- OpenZeppelin 合约标准类库

| 优先度 | 平台 | 类型 | 名称 | 推荐指数 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | hardhat | 官方在线文档 | [《hardhat官方文档》](https://hardhat.org/docs) | 9 | 建议阅读官方hardhat文档，英文但简单，可以保证最新release版本。第二选择[登链社区的hardhat翻译文档](https://learnblockchain.cn/manuals)，但是可能旧。|
| 1 | ethers | 官方在线文档 | [《ethers官方文档》](https://docs.ethers.org/v5/) | 9 | 建议阅读官方文档，英文但简单，可以保证最新。第二选择[登链社区的ethers翻译文档](https://learnblockchain.cn/manuals)，但是可能旧。|
| 1 | OpenZeppelin | 官方在线文档 | [《OpenZeppelin官方文档》](https://docs.openzeppelin.com/) | 9 | 建议阅读官方文档，英文但简单，看Contracts部分。|
| 2 | github | hardhat脚手架模板 | [hardhat-template](https://github.com/paulrberg/hardhat-template) | 7.5 | hardhat脚手架推荐模板参考。|

### 建议完成作业

- 使用Hardhat、OpenZeppelin在以太坊测试网络，发行一个自己的代币
  - 使用OpenZeppelin编写一个ERC20代币，
    - 增加Mint方法，限制每个账户每天最多mint一次，每次mint数量，每次mint需要花费的以太币，管理员可配置
    - 增加管理员提取收到的以太币方法
  - 使用ethers.js 编写测试脚本，进行单元测试
  - 测试通过后，发布测试网络
  - 在测试网络，公开ERC20合约代码
  - 在测试网络，自己mint一次代币

## 进阶

- 合约升级
  - 合约升级改造：<https://docs.openzeppelin.com/contracts/4.x/upgradeable>
  - 合约升级插件使用：<https://docs.openzeppelin.com/upgrades-plugins/1.x/>
  - [打破区块链的不可篡改，代理模式如何以最佳方式实现智能合约升级？](https://weibo.com/ttarticle/p/show?id=2309404842207793512494)
  - [OpenZeppelin的三种代理模式](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/proxy)
- 事件监听：各客户端类库或SDK的官方文档
- 合约安全
  - [Solcurity: 合约代码安全建议](https://learnblockchain.cn/article/5329)
  - [solidity官方文档的安全考虑部分](https://docs.soliditylang.org/zh/latest/security-considerations.html)
  - [慢雾安全团队知识库](https://github.com/slowmist/Knowledge-Base)
  - [《区块链黑暗森林自救手册》](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook)
  - [consensys合约安全最佳实践](https://consensys.github.io/smart-contract-best-practices/)
  - [安全性建议和最佳做法合集](https://github.com/guylando/KnowledgeLists/blob/master/EthereumSmartContracts.md)
  - [Trail of Bits 构建安全的智能合约](https://github.com/crytic/building-secure-contracts)
  - [Ethernaut合约安全练习题](https://ethernaut.openzeppelin.com/)
- Gas优化
  - [学习10个专家solidity的gas优化技巧](https://www.alchemy.com/overviews/solidity-gas-optimization)
- 合约最佳实践
  - [智能合约开发的最佳实践](https://yos.io/2019/11/10/smart-contract-development-best-practices/)
  - [清洁合约 - 智能合约模式与实践指南](https://www.useweb3.xyz/guides/clean-contracts)
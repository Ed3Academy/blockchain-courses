# 部署到测试网

## 📤 设置部署到区块链上

接下来关闭你的本地区块链网络运行的终端，也就是你运行`npx hardhat node`的地方。我们不再需要它了。我主要是想告诉你在本地的部署是如何进行的。

现在我们要做的是真正的交易，部署到实际的区块链上。

继续并在 [此处](https://www.alchemy.com/) 创建一个帐户

很抱歉让你建立这么多账户，但是，这个生态系统很复杂，我们想利用现有的强大工具。Alchemy 所做的是为我们提供了一个简单的方法来部署到真正的以太坊区块链上。

## 💳 交易

因此，当我们想在以太坊区块链上执行一个动作时，我们称之为*交易*。例如，向某人发送Ethereum就是一个交易。做一些更新我们合约中的变量的事情，也被认为是一个交易。

因此，当我们调用`mint`，并且它做了`balances[account] += mintTokenNumber`，这就是一个交易！**部署智能合约也是一个交易**。部署一个智能合约也是一个交易。

记住，区块链没有主人。它只是世界上由**矿工**运行的一堆计算机，它们拥有区块链的副本。

当我们部署我们的合约时，我们需要告诉**所有这些**矿工，"嘿，这是一个新的智能合约，请把我的智能合约添加到区块链上，然后把它也告诉其他人"。

这就是 [Alchemy](https://www.alchemy.com/) 的用武之地。

Alchemy 本质上帮助我们广播我们的合约创建交易，以便矿工尽快获取它。 一旦交易被挖掘，它就会作为合法交易广播到区块链。 从那里，每个人都更新他们的区块链副本。

这是复杂的。 而且，如果您不完全理解它，请不要担心。 当您编写更多代码并实际构建此应用程序时，它自然会更有意义。

因此，[在此处](https://www.alchemy.com/) 创建一个帐户。

查看下面的视频，了解如何获取 API 密钥！
[Alchemy Quickstart Guide](https://docs.alchemy.com/docs/alchemy-quickstart-guide#1key-create-an-alchemy-key)

## 🕸️ 测试网

直到最后，我们才会部署到主网。为什么？因为它需要花费真金白银，这样不值得；我们将从 "testnet "开始，它是 "mainnet "的一个克隆，但它使用的是假的以太坊，所以我们可以尽情地测试东西。但是，重要的是要知道，测试网是由实际的矿工运行的，并模仿现实世界的场景。

这很了不起，因为我们可以在一个真实世界的场景中测试我们的应用程序，我们确实需要要这样做。

1. 广播我们的交易信息
2. 等待它被真正的矿工捡到
3. 等到它被开采出来
4. 等待它被广播回区块链，告诉所有其他矿工更新他们的副本。

因此，你将在接下来的几节课内完成这一切 :)。

## 🤑 获得一些假币

现在有几个测试网，我们将使用的测试网叫做 “Mumbai”，它是Polygon（一个以太坊的二层网络）的测试网。

为了部署到Mumbai，我们需要假的Matic。为什么？因为如果你要部署到真正的以太坊主网，你会使用真钱！所以，测试网复制了主网的工作方式，唯一的区别是不涉及真钱。

为了获得假Matic，我们必须向网络索取一些。 **这种假Matic只在这个特定的测试网络上工作。** 你可以通过水龙头为Mumbai获得一些假Matic。在使用龙头之前，请确保你的MetaMask钱包被设置为“Mumbai测试网络”。

您可以在[这个网站](https://calibration-faucet.filswan.com/#/dashboard)上找到可以领取假Matic的网址。

## 📈 部署到Mumbai测试网

我们需要更改我们的 `hardhat.config.js` 文件。 你可以在你的智能合约项目的根目录中找到它。

```javascript
require("@nomicfoundation/hardhat-toolbox");

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
    solidity: "0.8.17",
    networks: {
      mumbai: {
        url: "YOUR_ALCHEMY_API_URL",
        accounts: ["YOUR_PRIVATE_ACCOUNT_KEY"]
      },
    },
};
```

为了保证安全我们将使用 dotenv。 安装它：

```bash
npm install --save dotenv
```

现在我们可以更新 hardhat.config.js 来使用 dotenv 了：

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: "0.8.17",
  networks: {
    mumbai: {
      // This value will be replaced on runtime
      url: process.env.ALCHEMY_URL,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
};
```

在项目的根目录下创建一个`.env`的文件，并在里面填上你的私钥，他看起来是这样的（不要复制注释中的内容）：

```javascript
ALCHEMY_URL=YOUR_ALCHEMY_API_URL
PRIVATE_KEY=YOUR_PRIVATE_ACCOUNT_KEY
```

最后，将 .env 添加到你的 .gitignore 文件中，这样 Git 就会忽略它，你的秘密就不会离开你的机器！

接下来，从 Alchemy Dashboard获取您的 API URL 并将其粘贴进去。然后，您将需要您的 **私人** Mumbai 密钥（不是您的公共地址！），您可以从 metamask 获取它并将其也粘贴到那里。

**注意：访问您的私钥可以通过打开MetaMask来完成，将网络更改为“Mumbai Test Network”，然后单击三个点并选择“账户详情”>“导出私钥”**

为什么需要使用私钥？ 因为为了执行像部署合约这样的交易，你需要“登录”到区块链。 而且，您的用户名是您的公共地址，您的密码是您的私钥。 这有点像登录 AWS 或 GCP 进行部署。

一旦您完成了配置设置，我们就可以使用我们之前编写的部署脚本进行部署。

从`my-token`的根目录运行此命令。 请注意，我们所做的只是将其从`localhost`更改为`Mumbai`。

```bash
npx hardhat run scripts/deploy.js --network mumbai
```

## ❤️ 部署

这是我的输出:

```bash
Deploying contracts with account:  0x9f773d11C3eABb67Bd1827a983641b37c6C6B0a5
Account balance:  200000000000000000
MyToken address:  0xe7eF8d5C50fD89AaFF85384D50774aB15f0652FC
```
复制最后一行已部署合约的地址并将其保存在某处。 别弄丢了！ 稍后您将需要它作为前端:)。 你的会和我的不一样。

**你刚刚部署了你的合约。哇塞。**

实际上，您可以获取该地址，然后将其粘贴到 Polygonscan [此处](https://mumbai.polygonscan.com/)。 Polygonscan 是一个向我们展示区块链状态并帮助我们查看交易位置的地方。 你应该在这里看到你的交易:)。 可能需要一分钟才能出现！

例如，[这里是](https://mumbai.polygonscan.com/address/0xe7eF8d5C50fD89AaFF85384D50774aB15f0652FC) 我的！

<!-- 🚨 在你点击 "下一课 "之前 -->
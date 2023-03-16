# 搭建你的第一个示例项目

## 💻 准备开始在区块链上工作

现在，你所需要知道的是，智能合约是一段生活在区块链上的代码。区块链是一个公共场所，任何人都可以安全地读取和写入数据，并收取费用。想想看，它有点像AWS或Heroku，只是没有人真正拥有它。

所以在这种情况下，我们希望人们能够领取我们的代币。这里的大背景是：

1. **我们要写一个智能合约。** 该合约有所有围绕如何处理代币发放的逻辑。这就像你的服务器代码。
2. **我们的智能合约将被部署到区块链上。** 这样，世界上任何一个人都可以访问和运行我们的智能合约（如果我们允许他们这么做）。所以，很像一个服务器:)。
3. **我们要建立一个客户端网站**，让人们轻松地与我们在区块链上的智能合约互动。

我会根据需要深入解释某些事情（例如，挖矿是如何进行的，智能合约是如何编译和运行的，等等），*但现在我们只关注让本地Ethereum网络运行起来。这样我们才能编译和测试我们的智能合约代码! 你知道你需要启动一个本地环境来进行工作吗？这里也一样！*。

## ✨ Hardhat的魅力

1. 我们将经常使用一个叫Hardhat的工具。这将让我们轻松地启动一个本地以太坊网络，并给我们提供假的测试以太币（用于支付本地以太坊网络费用）和假的测试账户来工作。记住，这就像一个本地服务器。
2. 快速编译智能合约，并在我们的本地区块链上测试它们。

首先，您需要安装 Node/NPM。 如果没有，请前往 [此处](https://hardhat.org/tutorial/setting-up-the-environment.html) 。

我们建议使用当前的 LTS Node.js 版本运行 Hardhat，否则您可能会遇到一些问题！ 您可以在 [此处](https://nodejs.org/en/about/releases/) 找到当前版本。 **确保您的 NodeJs 版本正确，否则您会遇到问题！** 我们现在推荐版本 16。

接下来，让我们前往(CMD)终端（Git Bash 将无法运行）。 继续并 cd 到你想要工作的目录。而后在那里运行这些命令：

```bash
mkdir my-token
cd my-token
npm init -y
npm install --save-dev hardhat
```

## 🏨 创建示例项目

Cool! 现在我们应该有Hardhat了。让我们启动一个示例项目。

运行。

```bash
npx hardhat
```

*注意：如果你在安装 npm 的同时安装了 yarn，你可能会收到诸如 `npm ERR! could not determine executable to run`。 在这种情况下，您可以执行 `yarn add hardhat`。*

选择**Create a JavaScript project**的选项。对一切都选择yes。

![npx hardhat screenshot](https://live.staticflickr.com/65535/52750751265_28f743bb16_o.png)

这个示例项目会要求你安装 hardhat-waffle 和 hardhat-ethers 。这些是我们以后会用到的其他好东西:)。

继续安装这些其他依赖项，以防它没有自动完成。

```bash
npm install --save-dev chai @nomiclabs/hardhat-ethers ethers @nomicfoundation/hardhat-toolbox @nomicfoundation/hardhat-chai-matchers
```

你的`hardhat.config.js` 文件应该看起来是这样：

```javascript
require("@nomicfoundation/hardhat-toolbox");

// 您需要导出一个对象来设置您的配置
// 到 https://hardhat.org/config/ 学习更多内容

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
    solidity: "0.8.17",
};
```

## ✅ 运行它

为了确保一切正常，运行。

```bash
 npx hardhat compile
```

然后运行。

```bash
npx hardhat test
```

你应该看到像这样的东西。

![Lock.sol test result snapshot](https://live.staticflickr.com/65535/52749809467_4ec9f40d31_o.png)

让我们做一个小小的清理。现在在您最喜欢的代码编辑器中打开项目的代码。 我最喜欢 VSCode！ 我们想删除为我们生成的所有蹩脚的启动代码（我们不需要这些）。 我们是专业人士！

继续并删除 `test` 下的文件 `Lock.js`。 另外，删除 `scripts` 下的 `deploy.js`。 然后，删除 `contracts` 下的 `Lock.sol`文件。是删文件，不是删除文件夹！

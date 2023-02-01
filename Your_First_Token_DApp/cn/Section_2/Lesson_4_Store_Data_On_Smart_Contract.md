# 存储数据在智能合约中

## 📦 存储数据

从这里开始，让我们为我们的合约添加一些幻想。

我们希望能够让某人领取我们的代币。

所以，我们首先需要的是一个他们可以点击领取！

区块链 = 把它想象成一个云提供商，有点像 AWS，但它不归任何人所有。它由来自世界各地的矿机的计算能力运行。通常这些人被称为矿工，我们付钱给他们来运行我们的代码！

智能合约 = 有点像我们服务器的代码，具有人们可以点击的不同功能。

```solidity
// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.17;

contract MyToken {
    uint public mintTokenNumber;
    uint public totalSupply;
    mapping (address => uint) private balances;

    constructor(uint _mintTokenNumber) {
        mintTokenNumber = _mintTokenNumber;
    }

    function mint() external payable {
        balances[msg.sender] += mintTokenNumber;
        totalSupply += mintTokenNumber;
    }

    function balanceOf(address account) view public returns (uint) {
        return balances[account];
    }
}
```

搞定！

所以，这就是你在 Solidity 中编写函数的方式。而且，我们还添加了一个自动初始化为 0 的 `totalSupply` 变量。但是，这个变量很特别，因为它被称为“状态变量”，它很酷，因为它永久存储在合约存储中。

OK，这里最重要的是 `balances` 变量！你可以把代币理解为货币、股票等，所以我们需要记录每个人对应的余额。这里我们用一个 **用户到余额的映射** 来记录。

我们还在这里使用了一些魔法和 `msg.sender`。这是调用该函数的钱包地址。这太棒了！这就像内置身份验证。我们确切地知道是谁调用了这个函数，因为为了调用智能合约函数，你需要连接一个有效的钱包！

## ✅ 更新 测试脚本 来测试我们的函数

我们需要测试我们创建的函数是否正常工作。

基本上，当我们将合约部署到区块链时（我们在运行 `myTokenContractFactory.deploy()` 时会这样做），我们的函数就可以在区块链上被调用，因为我们在函数上使用了 **public/external** 关键字。

把它想象成一个公共 API 终端:)。

所以现在我们要专门测试这些功能！

```javascript
const { expect } = require("chai")

describe('MyToken', function () {
    it('Check balances', async function () {
        // 1. deploy
        const myTokenContractFactory = await hre.ethers.getContractFactory('MyToken')
        const mintTokenNumber = hre.ethers.BigNumber.from(10)
        const myTokenContract = await myTokenContractFactory.deploy(mintTokenNumber)
        await myTokenContract.deployed()

        // 2. connect wallet and mint
        const [_, otherAccount] = await hre.ethers.getSigners()
        await myTokenContract.connect(otherAccount).mint()

        // 3. check whether user balance change is right
        //    or not
        const balance = await myTokenContract.balanceOf(otherAccount.address)
        expect(balance).to.equal(mintTokenNumber)
    })
})
```

## 🤔效果如何？

```javascript
const [_, otherAccount] = await hre.ethers.getSigners()
```

为了将某些东西部署到区块链，我们需要有一个钱包地址！ Hardhat在后台神奇地为我们做了这件事，但在这里我忽略了第一个地址，它是合约所有者的钱包地址，我抓取了一个随机的钱包地址，并将其命名为“otherAccount”。这会更有意义。

我添加的最后一件事是：

```javascript
await myTokenContract.connect(otherAccount).mint()
const balance = await myTokenContract.balanceOf(otherAccount.address)
expect(balance).to.equal(mintTokenNumber)
```

基本上，我们需要手动调用我们的函数！ 就像我们使用任何普通 API 一样。 首先，我调用函数mint来获取我们的代币。 然后，我想确认持有的代币数量是否和我们设置的一样，所以我们通过 `expect` 来辅助我们判断。

像往常一样运行脚本：

```bash
npx hardhat test test/MyToken.test.js
```

这是我的输出：

![Simple Mint Test result screenshot](https://raw.githubusercontent.com/Ed3Academy/blockchain-courses/main/Your_First_Token_DApp/images/test-simple-mint-screenshot.png)

非常棒，嗯:)？

所以我们：

1. 创建了领取代币的 `mint` 函数。
2. 更改了状态变量。

这几乎是大多数智能合约的基础。 读取函数。 编写函数。 并更改状态变量。 我们现在拥有继续开发史诗般的 代币网站 所需的构建块。

很快，我们将能够从我们将要开发的 React 应用程序中调用这些函数 :)。

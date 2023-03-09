# 开始编写你的第一个合约

## 👨‍💻 让我们写一个智能合约

我们将直接开始我们的项目。

在 **`contracts`** 目录下创建一个文件并将其命名为 **`MyToken.sol`**. 当你在使用  Hardhat 时候，文件的架构至关重要，所以要特别小心！

我们将从每个智能合约开头都需要用到的架构开始。

```solidity
// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.17;

import "hardhat/console.sol";

contract MyToken {
    constructor() {
        console.log("Print anything you want!");
    }
}
```

注：如果你想方便语法高亮，可以在 [这里](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) 下载 VS Code 的 Solidity 插件.

```solidity
// SPDX-License-Identifier: UNLICENSED
```

它被称为 "SPDX license identifier" ，用来告诉大家代码的版权规范 ：）

```solidity
pragma solidity ^0.8.17;
```

这是我们希望合约使用的 Solidity 编译器的版本。基本意思是 “当运行合约的时候，我要使用 大于等0.8.17 版本小于0.9.0版本的 Solidity 编译器。” 注意，要确保编译器的版本在 `hardhat.config.js` 中是一样的。

```solidity
import "hardhat/console.sol";
```

Hardhat 魔法一般在我们的合约中做了一些控制台日志。实际上，调试智能合约是很有挑战性的，但这正是 Hardhat 带给我们的好东西之一，它使一切变得更加容易。

```solidity
contract MyToken {
    constructor() {
        console.log("Print anything you want!");
    }
}
```

所以，合约有点像其他语言中的 "类"，如果你见过他们的话！一旦我们第一次初始化这个合约，这个结构函数就会运行并打印出这一行，请在这一行写上你想要的任何内容！

## ✅ 编译一下，看看是否正确

我们可以通过编译来看看代码的语法是否有问题

```bash
npx hardhat compile
```

编译后，我们会发现项目根目录下多了 `artifacts` 文件夹。它存储了我们调用合约的必要文件。

在下一节课，我们将运行它，并看看我们能得到什么！

# Miles&More区块链应用开发入门

## Solidity 简介

![solidity.png](https://i.postimg.cc/WbnsTGGP/solidity.png)

Solidity 是一种面向对象的高级语言，用于实现智能合约。

Solidity 是一种结构化编程语言，运行在以太坊虚拟机（EVM）。它受到了C++、Python 和 JavaScript 等语言的影响。

## 开发工具 Remix

在我们课程的右侧，有一个可以运行 Solidity 的 IDE：Remix，我们可以在 IDE 里编写代码、编译和部署智能合约。

![remix-sidebar.png](https://i.postimg.cc/Gp0kyyBp/remix-sidebar.png)

## 第一个 Solidity 程序

我们通过一个名为 Counter 的智能合约来学习 Solidity：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Counter {
    int private count;

    function get() public view returns (int) {
        return count;
    }

    function increase() public {
        count += 1;
    }

    function decrease() public {
        count -= 1;
    }
}
```

我们来逐行学习 Solidity 的语法。

* 第1行是由“//”开头的注释（这和 JavaScript 一致），表示这个合约所用的软件许可是 MIT license。如果不编写许可，编译时会警告，但程序可以运行。

  ```solidity
  // SPDX-License-Identifier: MIT
  ```
* 第2行声明源文件所用的 Solidity 版本，因为不同版本语法有差别。`^0.8.17` 表示源文件必须要大于等于 `0.8.17` 且小于 `0.9.0` 版本的编译器编译。

  ```solidity
  pragma solidity ^0.8.17;
  ```

  这个语法和 `package.json` 中的版本号规则是一致的，即 `^` 要求第一个非零的版本号保持一致，且后面的补丁版本号不能小于当前版本。

  同时 Solidity 语句以分号 `;` 结尾。
* 接下来是用关键字 `contract` 声明一个智能合约 Counter。

  ```solidity
  contract Counter {

  }
  ```

  合约内部首先是声明了一个 `int` 类型的 `private` 状态变量 count（它会被默认初始化为 0）。

  ```solidity
  int private count;
  ```

  `private` 状态变量表示仅在合约内部可以访问。

  然后定义了一个类型为 `public`，`view` 的 get 函数，用于返回 count 的值。

  ```solidity
  function get() public view returns (int) {
      return count;
  }
  ```

  `public` 表示该函数在合约内外部均可调用，且合约部署后也能调用；view 表示该函数只会查看、不会修改状态变量。
  最后又定义了两个函数 increase 和 decrease，分别对 count 进行 `+1` ，`-1` 的操作。

  ```solidity
  function increase() public {
      count += 1;
  }

  function decrease() public {
      count -= 1;
  }
  ```

## 编译和部署

在编写代码的界面，有一个绿色的三角形按钮，点击即可编译（或者按 Ctrl + S）：

![compile-button.png](https://i.postimg.cc/GpTRqr0M/compile-button.png)

如果编译成功，左侧的图标会打勾：

![compile-success.png](https://i.postimg.cc/gcTDPP1y/compile-success.png)

现在我们点击左侧的部署菜单进入部署页面：

![deploy-page.png](https://i.postimg.cc/wT73S1m2/deploy-page.png)

Remix 模拟了一个 EVM 来运行智能合约，并且分配了测试账户（里面有100个 ETH），我们点击部署：

![deploy-contract.png](https://i.postimg.cc/kgmKxHqr/deploy-contract.png)

部署成功后，已部署的合约会增加一个 `COUNTER`  合约，包含3个 `public`  函数：decrease、increase、get。自己点一下试试我们写的函数吧😃

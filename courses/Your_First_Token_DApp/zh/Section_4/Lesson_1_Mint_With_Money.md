# 花钱 Mint

## 完善代币合约

以太坊官方提供了一个统一的标准帮助我们规范代币合约——ERC-20 标准！例如货币、股票份额等都可以基于它去实现！

我们可以使用 openzeppelin 对这个标准的基础实现来减少我们的工作量

首先，我们需要安装 openzeppelin

```bash
npm install @openzeppelin/contracts
```

我们的合约可以改成以下这样，`import` 是用来导入合约，我们通过 `is` 来继承已经实现的方法

```solidity
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
```

构造函数也需要改变一下，参数列表后的 `ERC20(...)` 用来调用父类构造函数

```solidity
    constructor(uint _mintTokenNumber) ERC20("MyToken", "MTK") {
        mintTokenNumber = _mintTokenNumber;
    }
```

这里我们可以简化 mint 操作

```solidity
    function mint(address _to) external payable {
        _mint(_to, mintTokenNumber);
    }
```

可以看到我们无需维护 `balances` 以及 `totalSupply` 两个变量，所以也可以删除

经过这样一番优化，是不是觉得代码更好阅读了呢？

## 增加限制

在第一章中我们提到过，我们希望有点赚头，所以我们需要指定一个 `mintPrice`

```solidity
    uint public mintPrice;

    constructor(uint _mintTokenNumber, uint _mintPrice) ERC20("MyToken", "MTK") {
        mintTokenNumber = _mintTokenNumber;
        mintPrice = _mintPrice;
    }
```

那么我们在 `mint` 方法中添加用户是否发送足额金额的校验

```solidity
    function mint(address _to) external payable {
        require(msg.value >= mintPrice, "Insufficient funds");

        _mint(_to, mintTokenNumber);
    }
```

其中 `msg.value` 是全局的变量代表发送给我们代币合约的 ETH 数量。而require是一个全局函数如果第一个表达式结果为false的话就会抛出错误。

## 取出资金

我们要能够取出存入合约的资金，需要一个 `withdraw` 方法帮助我们完成这件事对吧？

我们要保证这笔资金只能由我们取出，所以需要加上一个权限判断。幸运的是，openzeppelin 已经帮助我们做了一些实现，我们需要做的第一件事是继承它们的合约。

```solidity
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
```

接着我们来实现这个方法

```solidity
    function withdraw() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }
```

控制权限需要做的第二件事就是在函数参数列表后加上 `onlyOwner` ，这也叫做 `modifier` ，在 solidity 中它帮助我们做一些函数执行前后的校验

记住，转账给其他用户时需要将address用 `payable(...)` 进行转换哦~ 之后调用 `transfer`方法 即可

## 测测是否正确

和之前一样，我们需要重写我们的测试脚本来测一测我们的合约，以保证它能够正常运转

```javascript
const {
  loadFixture,
} = require("@nomicfoundation/hardhat-network-helpers");
const { expect } = require("chai");

describe("MyToken", function () {
  async function deployFixture() {
    let mintTokenNumber = 10;
    let mintPrice = 1000;

    // Contracts are deployed using the first signer/account by default
    const [owner, otherAccount] = await ethers.getSigners();

    const MyToken = await ethers.getContractFactory("MyToken");
    const myToken = await MyToken.deploy(mintTokenNumber, mintPrice);

    return { myToken, mintPrice, mintTokenNumber, owner, otherAccount };
  }

  describe("Deployment", function () {
    it("Should set the right owner", async function () {
      const { myToken, owner } = await loadFixture(deployFixture);

      expect(await myToken.owner()).to.equal(owner.address);
    });

    it("Should set the right mintPrice", async function () {
      const { myToken, mintPrice } = await loadFixture(deployFixture);

      expect(await myToken.mintPrice()).to.equal(mintPrice);
    });
  });

  describe("Mint", function () {
    it("Should mint the right token number to mint account", async function () {
      const { myToken, mintPrice, mintTokenNumber, owner, otherAccount } =
        await loadFixture(deployFixture);

      await expect(
        myToken
          .connect(otherAccount)
          .mint(otherAccount.address, { value: mintPrice })
      )
        .to.changeEtherBalance(myToken.address, mintPrice)
        .to.changeEtherBalance(otherAccount.address, -mintPrice)
        .to.changeTokenBalance(myToken, otherAccount.address, mintTokenNumber);
    });
  });
});
```

我们可能要测试部署、领取等很多的测试场景，所以这里我们利用 hardhat 提供的 `loadFixture` 函数保证我们每一个场景跑完后，我们的链都是“干净”的。

别忘了重新部署我们的合约更新到 Mumbai 测试网上 😀

执行部署前，请仔细观察 👀 我们的测试脚本！构造函数新增了一个参数，测试脚本中多了一个传参。同样地，部署合约前，我们需要更新一下 `deploy.js`：

```js
const main = async () => {
  const [deployer] = await hre.ethers.getSigners();
  const accountBalance = await deployer.getBalance();

  console.log("Deploying contracts with account: ", deployer.address);
  console.log("Account balance: ", accountBalance.toString());

  let mintTokenNumber = 10;
  let mintPrice = 1000;

  const myTokenContractFactory = await hre.ethers.getContractFactory("MyToken");
  const myTokenContract = await myTokenContractFactory.deploy(
    mintTokenNumber,
    mintPrice
  );
  await myTokenContract.deployed();

  console.log("MyToken address: ", myTokenContract.address);
};

const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.log(error);
    process.exit(1);
  }
};

runMain();
```

OK！我们再试试之前的部署命令吧！

```bash
npx hardhat run scripts/deploy.js --network mumbai
```

```none
Deploying contracts with account:  0x9f773d11C3eABb67Bd1827a983641b37c6C6B0a5
Account balance:  199530244995302450
MyToken address:  0x6494e0E82BB0D3aB1B11e80b3Bc2A4fC1dE247d7
```

## ⏳Ed3LoyaltyPoints智能合约开发

## 📒合约代码

Ed3LoyaltyPoints是积分合约🥰，我们选用ERC20标准来实现它。合约的主要内容包括：

- 继承ERC20Capped，限制积分供应量上限；ERC20Capped 是 ERC20 代币标准的一个扩展，它增加了一个代币总供应量的上限限制，同时在发行新代币时检查供应量是否已经达到了上限。
- 将积分精度设置为1

您可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3LoyaltyPoints.sol)找到这份合约，关键代码如下：

```solidity
/*
 * 合约积分
 * ERC20 是以太坊上最常用的代币标准之一，规定了代币的基本功能，包括转账、余额查询、授权转移等。
 * ERC20Capped 是 ERC20 代币标准的一个扩展，它增加了一个代币总供应量的上限限制，同时在发行新代币时检查供应量是否已经达到了上限。
 * ERC20Burnable 是 ERC20 代币标准的一个扩展，它增加了代币的销毁功能，允许代币持有者销毁自己的代币。
 * Ownable 是一个基础合约，它提供了一个拥有者（owner）的概念，允许合约的拥有者执行一些关键操作，例如更改合约状态、转移合约所有权等。
 */
contract Ed3LoyaltyPoints is ERC20, ERC20Capped, ERC20Burnable, Ownable {
    // Ed3积分合约构造函数，在部署脚本中传入name、symbol和发行上限
    constructor(string memory name, string memory symbol, uint256 cap) ERC20(name, symbol) ERC20Capped(cap) {}

    function mint(address _to, uint256 _mintTokenNumber) external onlyOwner {
        _mint(_to, _mintTokenNumber);
    }

    // 重写父类的_mint方法，指明使用的mint方法是 ERC20Capped提供的
    function _mint(address account, uint256 amount) internal virtual override(ERC20, ERC20Capped) {
        ERC20Capped._mint(account, amount);
    }
    // 设定代币精度为1
    function decimals() public view virtual override returns (uint8) {
        return 1;
    }
}

```

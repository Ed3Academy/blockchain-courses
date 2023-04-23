# 🚪Ed3AirlineGate智能合约开发

## 📒合约代码

Ed3AirlineGate是我们的服务窗口，这里是买机票以及发放积分的统一入口☺️。合约的主要内容包括：

- 在构造函数中需要指明积分合约地址、机票地址以及购买一张机票可以获取多少积分；
- mint()函数中需要使用原生币购买机票，需要使用 `payable` 修饰，同时返回积分给用户；
- 集成了两个第三方合约Ed3AirTicketNFT和Ed3LoyaltyPoints，我们可以先声明这些接口有哪些方法，然后通过接口的方式直接调用它们。

```solidity
interface IEd3AirTicketNFT {
    function mint(address _to) external payable;
    function maxSupply() external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function mintPrice() external view returns (uint256);
}

interface IEd3LoyaltyPoints {
    function mint(address _to, uint256 _mintTokenNumber) external;
}
```

你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3AirlineGate.sol)找到这份合约，关键代码如下：

```solidity
// @title Ed3航空公司服务窗口，用于用于购买机票并发放积分，同时提供接口让管理员可以转移购买机票的资金。
contract Ed3AirlineGate {
    address payable public ed3TicketNFTAddress;
    address public ed3LoyaltyPointsAddress;
    uint256 public immutable POINTS_PER_TICKET;

    /**
     * @notice 航空公司服务窗口构造函数
     * @param _ed3LoyaltyPointsAddress 积分地址
     * @param _ed3TicketNFTAddress NFT机票地址
     * @param _pointsPerTicket 设置积分兑换优惠券比例
     */
    constructor(address _ed3LoyaltyPointsAddress, address payable _ed3TicketNFTAddress, uint256 _pointsPerTicket) {
        ed3LoyaltyPointsAddress = _ed3LoyaltyPointsAddress;
        ed3TicketNFTAddress = _ed3TicketNFTAddress;
        POINTS_PER_TICKET = _pointsPerTicket;
    }

    /**
     * @notice 购买机票、获取积分的函数
     * @param _to 获得机票和积分的地址
     */
    function mint(address _to) external payable {
        uint256 mintPrice = IEd3AirTicketNFT(ed3TicketNFTAddress).mintPrice();
        require(msg.value >= mintPrice, "Insufficient funds");
        uint256 maxSupply = IEd3AirTicketNFT(ed3TicketNFTAddress).maxSupply();
        uint256 totalSupply = IEd3AirTicketNFT(ed3TicketNFTAddress).totalSupply();
        require(maxSupply > totalSupply, "air ticket sold out");
        // 购买机票NFT
        IEd3AirTicketNFT(ed3TicketNFTAddress).mint{ value: msg.value }(_to);
        // 每次购买机票后可以得到 POINTS_PER_TICKET 积分
        IEd3LoyaltyPoints(ed3LoyaltyPointsAddress).mint(_to, POINTS_PER_TICKET);
    }
}

```

# 🎟️Ed3Coupon智能合约开发

Ed3Coupon是我们的优惠券合约，它基本和机票合约大同小异：Ed3Airticket机票是使用原生币来购买，而Ed3Coupon优惠券是需要消耗Ed3LoyaltyPoints来兑换。

## 🖼️生成NFT元数据

可以参见Ed3AirTicket部分是如何生成NFT元数据的，只是脚本中需要更改Ed3Coupon图片路径😝。这是我们选用的Coupon图片：

![coupon](https://live.staticflickr.com/65535/52833694844_978c3904f9_b.jpg)

## 📒合约代码

与Ed3Airticket不同的是，兑换处Ed3Coupon需要的是Ed3LoyaltyPoints，mint()方法中校验的就是Ed3LoyaltyPoints余额是否足够而非原生币☺️。合约的主要内容包括：

- 在构造函数中需要指明积分合约地址用来校验用户提供的积分是否足够；
- mint()函数中需要使用积分兑换优惠券；

您可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3Coupon.sol)找到这份合约，关键代码如下：

```solidity

    // 在构造函数中需要指明积分合约地址用来校验用户提供的积分是否足够；
    constructor(
        address _tokenAddress,
        string memory _name,
        string memory _symbol,
        string memory _baseUri,
        uint256 _mintPrice,
        uint256 _maxSupply,
        uint256 _launchDate,
        address _paymentAddress
    ) ERC721(_name, _symbol) {
        baseUri = _baseUri;
        mintPrice = _mintPrice;
        maxSupply = _maxSupply;
        launchDate = _launchDate;
        paymentAddress = payable(_paymentAddress);
        tokenAddress = _tokenAddress;
    }

    // 将优惠券mint给指定用户，这是使用指定的token才能兑换优惠券，以物易物。指定的token以及兑换数值比例在构造函数中做了指定
    function mint(address _to) public {
        require(block.timestamp >= launchDate, "minting not enabled yet, please wait");
        require(tokenIdCounter.current() < maxSupply, "Maximum supply reached");
        // 校验是否有足够的积分用于兑换
        require(ERC20(tokenAddress).balanceOf(msg.sender) >= mintPrice, "Insufficient funds");
        ERC20(tokenAddress).transferFrom(msg.sender, address(this), mintPrice);
        uint256 tokenId = tokenIdCounter.current();
        _mint(_to, tokenId);
        tokenIdCounter.increment();
    }
```

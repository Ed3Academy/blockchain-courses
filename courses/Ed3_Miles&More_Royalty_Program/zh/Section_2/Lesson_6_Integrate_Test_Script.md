# 📑集成测试脚本

当完成上述几个合约的编写后，接下来我们需要对这些合约做一次集成测试🥳！测试主要内容包括：

- 前置内容包括
  - 部署机票
  - 部署积分
  - 部署服务窗口
  - 部署优惠券
- 用户携带ticketMintPrice资金通过服务窗口购买机票
- 授权积分合约给优惠券合约
- 完成积分的兑换
- 校验优惠券的数量为1

您可以通过命令 `npx hardhat test ./test/testDeployLoyaltyProgram.js`完成测试，从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/test/testDeployLoyaltyProgram.js)找到这份测试脚本，关键代码如下：

```javascript
// npx hardhat test ./test/testDeployLoyaltyProgram.js
describe("Ed3Coupon mint test", function () {
  async function deployFixture() {
    const [owner] = await ethers.getSigners();
    // 部署机票
    const ticketNFTName = "Ed3AirTicket";
    const ticketNFTSymbol = "Ed3AirTicket";
    const ticketMintPrice = 10 ** 14;
    const [deployer] = await ethers.getSigners();
    const ticketMetadata = ticketNFTLocation.metadata;
    const ticketCount = ticketNFTLocation.count;
    // 获取合约对象
    const Ed3AirTicketNFT = await ethers.getContractFactory("Ed3AirTicketNFT");
    // 设置ERC721开始发售时间
    const ticketLaunchDate = moment("2023-03-12 00:00");
    // 部署合约
    const ed3AirTicketNFT = await Ed3AirTicketNFT.deploy(
      ticketNFTName,
      ticketNFTSymbol,
      `ipfs://${ticketMetadata}/`,
      ticketMintPrice,
      ticketCount,
      Math.round(ticketLaunchDate.valueOf() / 1000),
      deployer.address,
    );

    // 部署积分
    const pointTotalSupply = 10000;
    const Ed3LoyaltyPoints = await ethers.getContractFactory("Ed3LoyaltyPoints");
    const ed3LoyaltyPoints = await Ed3LoyaltyPoints.deploy("Ed3LoyaltyPoints", "ELP", pointTotalSupply);

    // 部署服务窗口 GateV2
    const pointsPerTicket = 1000;
    const Ed3AirlineGate = await ethers.getContractFactory("Ed3AirlineGate");
    const ed3AirlineGate = await Ed3AirlineGate.deploy(
      ed3LoyaltyPoints.address,
      ed3AirTicketNFT.address,
      pointsPerTicket,
    );
    await ed3LoyaltyPoints.transferOwnership(ed3AirlineGate.address);

    // 部署优惠券 Coupon
    const couponName = "Ed3Coupon";
    const couponSymbol = "Ed3Coupon";
    const couponMetadata = couponNFTLocation.metadata;
    const couponMintPrice = 1000;
    const couponCount = couponNFTLocation.count;
    const Ed3Coupon = await ethers.getContractFactory("Ed3Coupon");
    const couponLaunchDate = moment("2023-03-12 00:00");
    const ed3Coupon = await Ed3Coupon.deploy(
      ed3LoyaltyPoints.address,
      couponName,
      couponSymbol,
      `ipfs://${couponMetadata}/`,
      couponMintPrice,
      couponCount,
      Math.round(couponLaunchDate.valueOf() / 1000),
      owner.address,
    );
  }
  describe("Mint", function () {
    describe("exchange coupon", function () {
      it("Should mint the NFT to mint account", async function () {
        const {
          ticketMintPrice,
          ed3AirTicketNFT,
          ed3AirlineGate,
          ed3LoyaltyPoints,
          ed3Coupon,
          owner,
        } = await loadFixture(deployFixture);
        // 用户通过服务窗口购买机票，带上的资金是ticketMintPrice
        await ed3AirlineGate.connect(owner).mint(owner.address, { value: ticketMintPrice });
        const ed3AirlineTicketBalance = await ed3AirTicketNFT.balanceOf(owner.address);
        const ed3LoyaltyPointsBalance = await ed3LoyaltyPoints.balanceOf(owner.address);
        console.log("ed3AirlineGate ticket balance:", ed3AirlineTicketBalance);
        console.log("ed3LoyaltyPoints balance:", ed3LoyaltyPointsBalance);
        console.log("ed3LoyaltyPoints approve address:", ed3Coupon.address);
        // 授权积分合约给优惠券合约，如此做才能让优惠券合约收走对应的积分完成兑换
        await ed3LoyaltyPoints.connect(owner).approve(ed3Coupon.address, 0);
        await ed3LoyaltyPoints.connect(owner).approve(ed3Coupon.address, ed3LoyaltyPointsBalance);
        console.log("ed3Coupon balance before:", await ed3Coupon.balanceOf(owner.address));
        // 完成积分的兑换
        await ed3Coupon.connect(owner).mint(owner.address);
        // 校验优惠券的数量为1
        expect(await ed3Coupon.balanceOf(owner.address)).to.equal(1);
        console.log("ed3Coupon token balance after:", await ed3Coupon.balanceOf(owner.address));
      });
    });
  });
});
```

如果看到下方截图，那么恭喜您，测试通过🉑~

![test_result](https://live.staticflickr.com/65535/52833446366_b44b325618_b.jpg)

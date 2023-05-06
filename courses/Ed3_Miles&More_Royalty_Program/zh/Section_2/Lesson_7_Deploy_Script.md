# ♾️集成部署脚本

经过以上代码开发和集成测试，恭喜您到最后一步，我们可以发布上链了！部署脚本和集成测试脚本大同小异。关于机票，您可以使用自己部署的机票或者使用Ed3提供的机票，这在脚本中选择屏蔽即可😁！

您可以通过命令 `npx hardhat node` 在本地起fork测试链，然后通过命令 `npx hardhat run ./scripts/deployLoyaltyProgram.js --network localhost` 在本地进行合约部署😊。

最终我们指定网络地址为PolygonMumbai，可通过 `npx hardhat run ./scripts/deployLoyaltyProgram.js --network PolygonMumbai`命令发布公共测试链😄！

从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/scripts/deployLoyaltyProgram.js)找到这份集成部署脚本，关键代码如下：

```JavaScript
// npx hardhat run ./scripts/deployLoyaltyProgram.js --network PolygonMumbai
// npx hardhat run ./scripts/deployLoyaltyProgram.js --network localhost
async function main() {
  const [owner] = await ethers.getSigners();

  // 部署机票
  // const ticketNFTName = "Ed3AirTicket";
  // const ticketNFTSymbol = "Ed3AirTicket";
  // const ticketMintPrice = 10 ** 14;
  // const [deployer] = await ethers.getSigners();
  // const ticketMetadata = ticketNFTLocation.metadata;
  // const ticketCount = ticketNFTLocation.count;
  // const Ed3AirTicketNFT = await ethers.getContractFactory("Ed3AirTicketNFT");
  // const ticketLaunchDate = moment("2023-03-12 00:00");
  // const ed3AirTicketNFT = await Ed3AirTicketNFT.deploy(ticketNFTName, ticketNFTSymbol, `ipfs://${ticketMetadata}/`, ticketMintPrice, ticketCount, Math.round(ticketLaunchDate.valueOf () / 1000), deployer.address);
  // console.log( `npx hardhat verify --network PolygonMumbai "${ed3AirTicketNFT.address}" ${ticketNFTName} ${ticketNFTSymbol} ipfs://${ticketMetadata}/ ${ticketMintPrice} ${ticketCount} ${Math.round(ticketLaunchDate.valueOf() / 1000)} ${deployer.address}`);
  const ed3AirTicketNFT = "0x8B939b4469BC384e841afE4d809D95F1373e81cF";

  // 部署积分
  const pointTotalSupply = 10000;
  const pointName = "Ed3LoyaltyPoints";
  const pointSymbol = "ELP";
  const Ed3LoyaltyPoints = await ethers.getContractFactory("Ed3LoyaltyPoints");
  const ed3LoyaltyPoints = await Ed3LoyaltyPoints.deploy(pointName, pointSymbol, pointTotalSupply);

  // 部署服务窗口 GateV2
  const pointsPerTicket = 1000;
  const Ed3AirlineGate = await ethers.getContractFactory("Ed3AirlineGate");
  const ed3AirlineGate = await Ed3AirlineGate.deploy(ed3LoyaltyPoints.address, ed3AirTicketNFT, pointsPerTicket);
  await ed3LoyaltyPoints.transferOwnership(ed3AirlineGate.address);

  // 部署优惠券 Coupon
  const couponName = "Ed3Coupon";
  const couponSymbol = "Ed3Coupon";
  const couponMetadata = couponNFTLocation.metadata;
  const couponMintPrice = 1000;
  const couponCount = 1000;
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

```

当成功部署合约之后，您应该看到以下截图输出😇：

![deploy_result](https://i.postimg.cc/6Q0LPjhv/deploy-result.png)

接下来我们可以对合约进行开源，这样所有人都可以在网络上看到合约的代码！我们复制对应verify语句在命令行执行后应该看到如下截图😚：

![verify_result](https://i.postimg.cc/Zqf8xfZK/verify-result.png)

访问 [https://mumbai.polygonscan.com](https://mumbai.polygonscan.com/address/0x56676b6D007Acb62b59C19Fe53d7d94Ed9A23ae1#code) 可以看到，示例中的Ed3Coupon合约已经成功上链并完成开源😝！

![verify_scan](https://i.postimg.cc/kqvhh748/ether-scan.png)

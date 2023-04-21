
# ⏯️案例流程

![ed3_miles&more_flow_chart](https://live.staticflickr.com/65535/52831401433_c11d1cfd9b_b.jpg)

1. 用户 User 通过 Ed3AirlineGate 服务窗口购买机票 Ed3AirTicketNFT；
2. Ed3AirlineGate 服务窗口将机票 Ed3AirTicketNFT 和积分 Ed3LoyaltyPoints 发放给 User；
3. User 使用 Ed3LoyaltyPoints 积分兑换 Ed3Coupon 优惠券；

通过流程图可以看到，我们需要开发以下智能合约：Ed3AirlineGate、Ed3AirTicketNFT、Ed3LoyaltyPoints、Ed3Coupon（NFT）。

合约部分的仓库可以从[这里](git@github.com:Ed3Academy/ed3-hardhat-template.git)找到。您需要复制.env.example 并改名为.env，填充.env 中账号信息。

账号信息需要填充以下内容

```javascript
// rpc提供商的apiKey，从这里申请获取：https://app.infura.io/login，需要科学上网，或者可以使用这个账号，不过可能会有限速问题：56375da21c3b4229b525bb8b0d0dfd57
INFURA_API_KEY="zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
// 钱包的私钥，获取钱包可以参照教程 https://ed3academy.xyz/course/63fdc8a6220f4ff4b42ced94 
PRIVATE_KEY="zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
// NMUMBAI网络的apiKey，可以从这里申请获取：https://polygonscan.com/myapikey
POLYGONMUMBAI_SCAN_API_KEY="zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
// NFT_STORAGE的apiKey，可以从这里申请获取：https://nft.storage/，需要科学上网，或者可以使用这个账号，不过可能会有限速问题：eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJkaWQ6ZXRocjoweDU0N0FERDFFZmFGMzU2YTFCMDk2NzU4YjAwZDAyNjUzZGY0OGEwRjUiLCJpc3MiOiJuZnQtc3RvcmFnZSIsImlhdCI6MTY3Njg3NjE5NjcwNCwibmFtZSI6Ind0Zi1uZnQifQ.kI8fwg9Ulm3OgdAp3RrNJtGclpCqXGdntReUp-ZDZFI
NFT_STORAGE_API_KEY="zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
```

然后通过以下命令安装项目依赖。

```powershell
npm install
```

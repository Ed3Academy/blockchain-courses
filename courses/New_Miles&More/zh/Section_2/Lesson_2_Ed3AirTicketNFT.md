# 🎫Ed3AirTicketNFT智能合约开发

我们先说说依赖的Ed3AirTicketNFT。这是一个机票合约，我们采用ERC721标准实现🥰。而发布ERC721代币，我们还需要有NFT的元数据，让我们先上传一张喜欢的图片！😘

## 🖼️生成NFT元数据

选择一张您喜欢的图片作为NFT的image，这里我们选择的是机票，需要放在nfts/images/ticket路径下，或者配置到你您喜欢的路径😄，如果这么做，您也需要在代码中更改对应的路径。

![airticket](https://i.postimg.cc/t47ckDg4/airticket.jpg)

您可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/scripts/upload-nfts.js)找到这份生成NFT元数据脚本，执行 `npx hardhat run ./scripts/upload-nfts.js` 就可以生成NFT元数据了。脚本主要过程为：

- 将图片上传到ipfs上
- 将NFT元数据上传到ipfs上
- 将NFT元数据保存到本地，供后续部署合约时初始化使用

关键代码如下：

```javascript
// npx hardhat run ./scripts/upload-nfts.js
async function main() {
  // ticket和coupon都是使用相同的上传脚本，我们在这里切换想要上传的NFT图片是ticket还是coupon
  const pathName = "ticket";
  // const pathName = "coupon";
  const images = readDirectory(path.join(__dirname, "../nfts/images/" + pathName));
  // 将图片上传到ipfs上
  const imageMetadata = await client.storeDirectory(images);
  console.log("Images uploaded to: ", imageMetadata);
  const nftFiles = readDirectory(path.join(__dirname, "../nfts/metadata/" + pathName));
  const nftFilesModified = await Promise.all(
    nftFiles.map(async (file) => {
      const obj = JSON.parse(await file.text());
      obj;
      obj.image = `ipfs://${imageMetadata.toString()}/${obj.image}`;
      return new File([JSON.stringify(obj)], file.name);
    }),
  );
  // 将元数据上传到ipfs上
  const fileMetadata = await client.storeDirectory(nftFilesModified);
  console.log("Metadata uploaded to: https://ipfs.io/ipfs/", fileMetadata);
  // 将元数据保存到本地
  const locationPath = path.join(__dirname, "../nfts/location/" + pathName + "/location.json");
  fs.writeFileSync(
    locationPath,
    JSON.stringify(
      {
        images: imageMetadata.toString(),
        metadata: fileMetadata.toString(),
        count: nftFiles.length,
      },
      null,
      2,
    ),
  );
}
```

发布成功后，我们可以在ipfs上查看我们的元数据！您可以从[这里](https://ipfs.io/ipfs/bafybeibuvuunohdpdknchojvwdbcfcdgjwlt6qznacjyxpfqficab24ng4/0.json)访问到元数据。

```json
{
  "name": "Ed3Coupon",
  "description": "Ed3Coupon",
  "image": "ipfs://bafybeifajmlobagdmv3dwa3dy4vn3w5jp55vzg346jsuaepd5w6pnid4x4/0.jpg"
}
```

## 📒合约代码

ERC721是以太坊上用于实现非同质化代币（Non-Fungible Tokens，NFTs）的一种标准。ERC721代币每个代币都是独一无二的，每个代币都有自己的唯一标识符（Token ID）。

合约主要的mint()方法需要完成购买资金校验、将机票发放到购买者账户里，同时保证下一张机票id唯一。您可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3AirTicketNFT.sol)找到这份合约，关键代码如下：

```solidity

    // 将机票mint给指定用户
    function mint(address _to) external payable {
        // 校验当前区块时间是否在首发时间之后
        require(block.timestamp >= launchDate, "minting not enabled yet, please wait");
        // 校验当前代币供应量是否达到上限
        require(tokenIdCounter.current() < maxSupply, "Maximum supply reached");
        // 校验用户用于mint的资金是否足够
        require(msg.value >= mintPrice, "Insufficient funds");
        // 获取代币ID
        uint256 tokenId = tokenIdCounter.current();
        // 将指定tokenid的代币mint给用户
        _mint(_to, tokenId);
        // 代币id自增
        tokenIdCounter.increment();
    }
```

## 📜合约部署脚本

当然，在部署到polygonMumbai网络上，需要有polygonMumbai上的原生代币，您可以从[这里](https://faucet.polygon.technology/)获取。

您可以通过 `npx hardhat run ./scripts/deployTicket.js  --network PolygonMumbai`完成部署。脚本的主要内容包括：

- 获取机票合约对象；
- 指定元数据信息完成部署动作；
- 别忘记最后将合约开源

您可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/scripts/deployEd3AirTicket.js)找到这份脚本，关键代码如下：

```javascript
// npx hardhat run ./scripts/deployTicket.js  --network PolygonMumbai
const { ethers } = require("hardhat");
const moment = require("moment");
// 在这里获取元数据地址
const NFTLocation = require("../nfts/location/ticket/location.json");
// 设置机票名称
const NFTName = "Ed3AirTicket";
// 设置机票标识符
const NFTSymbol = "Ed3AirTicket";
// 设置机票发行上限 10w张
const count = 100000;
const mintPrice = 10 ** 14;
async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying contracts with the account: " + deployer.address);
  const { metadata } = NFTLocation;
  const Ed3AirTicketNFT = await ethers.getContractFactory("Ed3AirTicketNFT");
  const launchDate = moment("2023-03-12 00:00");
  const ed3AirTicketNFT = await Ed3AirTicketNFT.deploy(
    NFTName,
    NFTSymbol,
    `ipfs://${metadata}/`,
    mintPrice,
    count,
    Math.round(launchDate.valueOf() / 1000),
    deployer.address,
  );
}

```

😆 到这里机票终于发布成功了！恭喜您！㊗️。

或者您也可以直接用Ed3提供的[机票合约😀](https://mumbai.polygonscan.com/address/0x0305462813Bf77F0F87A09036879c808d03846ED#readContract)，限量10w张，童叟无欺，先到先得。

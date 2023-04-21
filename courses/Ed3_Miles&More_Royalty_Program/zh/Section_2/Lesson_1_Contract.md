# 📑智能合约开发

![ed3_miles&more_flow_chart](https://live.staticflickr.com/65535/52831401433_c11d1cfd9b_b.jpg)

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

## 🎫Ed3Airticket

我们先说说依赖的Ed3AirTicket。这是一个机票合约，我们采用ERC721标准实现🥰。而发布ERC721代币，我们还需要有NFT的元数据，让我们先上传一张喜欢的图片！😘

### 🖼️生成NFT元数据

选择一张你喜欢的图片作为NFT的image，这里我们选择的是机票，需要放在nfts/images/ticket路径下，或者配置到你喜欢的路径😄，如果这么做，你也需要在代码中更改对应的路径。

![airticket](https://live.staticflickr.com/65535/52831366119_3cb5727f5a_b.jpg)

你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/scripts/upload-nfts.js)找到这份生成NFT元数据脚本，执行 `npx hardhat run ./scripts/upload-nfts.js` 就可以生成NFT元数据了。脚本主要过程为：

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

发布成功后，我们可以在ipfs上查看我们的元数据！你可以从[这里](https://ipfs.io/ipfs/bafybeibuvuunohdpdknchojvwdbcfcdgjwlt6qznacjyxpfqficab24ng4/0.json)访问到元数据。

```json
{
  "name": "Ed3Coupon",
  "description": "Ed3Coupon",
  "image": "ipfs://bafybeifajmlobagdmv3dwa3dy4vn3w5jp55vzg346jsuaepd5w6pnid4x4/0.jpg"
}
```



### 📒合约代码

ERC721是以太坊上用于实现非同质化代币（Non-Fungible Tokens，NFTs）的一种标准。ERC721代币每个代币都是独一无二的，每个代币都有自己的唯一标识符（Token ID）。

合约主要的mint()方法需要完成购买资金校验、将机票发放到购买者账户里，同时保证下一张机票id唯一。你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3AirTicketNFT.sol)找到这份合约，关键代码如下：

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

### 📜合约部署脚本

当然，在部署到polygonMumbai网络上，需要有polygonMumbai上的原生代币，您可以从[这里](https://faucet.polygon.technology/)获取。

您可以通过 `npx hardhat run ./scripts/deployTicket.js  --network PolygonMumbai`完成部署。脚本的主要内容包括：

- 获取机票合约对象；
- 指定元数据信息完成部署动作；
- 别忘记最后将合约开源

你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/scripts/deployTicket.js)找到这份脚本，关键代码如下：

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

😆 到这里机票终于发布成功了！恭喜你㊗️。

或者你也可以直接用Ed3提供的[机票合约😀](https://mumbai.polygonscan.com/address/0x0305462813Bf77F0F87A09036879c808d03846ED#readContract)，限量10w张，童叟无欺，先到先得。

## ⏳Ed3LoyaltyPoints

Ed3LoyaltyPoints是积分合约🥰，我们选用ERC20标准来实现它。合约的主要内容包括：

- 继承ERC20Capped，限制积分供应量上限；ERC20Capped 是 ERC20 代币标准的一个扩展，它增加了一个代币总供应量的上限限制，同时在发行新代币时检查供应量是否已经达到了上限。
- 将积分精度设置为1

你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3LoyaltyPoints.sol)找到这份合约，关键代码如下：

### 📒合约代码

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

## 🚪Ed3AirlineGate

Ed3AirlineGate是我们的服务窗口，这里是买机票以及发放积分的统一入口☺️。合约的主要内容包括：

- 在构造函数中需要指明积分合约地址、机票地址以及购买一张机票可以获取多少积分；
- mint()函数中需要使用原生币购买机票，同时返回积分给用户；
- 这里我们集成了两个第三方合约Ed3AirTicketNFT和Ed3LoyaltyPoints，我们可以先声明这些接口有哪些方法，然后通过接口的方式直接调用它们。

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

### 📒合约代码

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

## 🎟️Ed3Coupon

Ed3Coupon是我们的优惠券合约，它基本和机票合约大同小异：Ed3Airticket机票是使用原生币来购买，而Ed3Coupon优惠券是需要消耗Ed3LoyaltyPoints来兑换。

### 🖼️生成NFT元数据

可以参见Ed3AirTicket部分是如何生成NFT元数据的，只是脚本中需要更改Ed3Coupon图片路径😝。这是我们选用的Coupon图片

![coupon](https://live.staticflickr.com/65535/52833694844_978c3904f9_b.jpg)

### 📒合约代码

与Ed3Airticket不同的是，兑换处Ed3Coupon需要的是Ed3LoyaltyPoints，mint()方法中校验的就是Ed3LoyaltyPoints余额是否足够而非原生币☺️。合约的主要内容包括：

- 在构造函数中需要指明积分合约地址用来校验用户提供的积分是否足够；
- mint()函数中需要使用积分兑换优惠券；

你可以从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/contracts/Ed3Coupon.sol)找到这份合约，关键代码如下：

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

# 🔭集成测试脚本

当完成上述几个合约的编写后，接下来我们需要对这些合约做一次集成测试🥳！主要内容包括：

- 前置内容包括
  - 部署机票
  - 部署积分
  - 部署服务窗口
  - 部署优惠券
- 用户携带ticketMintPrice资金通过服务窗口购买机票
- 授权积分合约给优惠券合约
- 完成积分的兑换
- 校验优惠券的数量为1

你可以通过命令 `npx hardhat test ./test/testDeployLoyaltyProgram.js`完成测试，从[这里](https://github.com/Ed3Academy/ed3-hardhat-template/blob/main/test/testDeployLoyaltyProgram.js)找到这份测试脚本，关键代码如下：

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

如果看到下方截图，那么恭喜你，测试通过🉑~

![test_result](https://live.staticflickr.com/65535/52833446366_b44b325618_b.jpg)

# ♾️集成部署脚本

经过以上代码开发和集成测试，恭喜你到最后一步，我们可以发布上链了！部署脚本和集成测试脚本大同小异。关于机票，你可以使用自己部署的机票或者使用Ed3提供的机票，这在脚本中选择屏蔽即可！

你可以通过命令 `npx hardhat node` 在本地起fork测试链，然后通过命令 `npx hardhat run ./scripts/deployLoyaltyProgram.js --network localhost` 在本地进行合约部署。

最终我们指定网络地址为PolygonMumbai，即可上公共测试链了。 `npx hardhat run ./scripts/deployLoyaltyProgram.js --network PolygonMumbai`

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

当成功部署合约之后，你应该可以看到以下截图输出：

![deploy_result](https://live.staticflickr.com/65535/52833919323_ae561f629f_b.jpg)

接下来我们可以对合约进行开源，这样所有人都可以在网络上看到合约的代码！我们复制对应verify语句在命令行执行后应该看到如下截图：

![verify_result](https://live.staticflickr.com/65535/52832907237_a6a387f775_b.jpg)

访问 [https://mumbai.polygonscan.com](https://mumbai.polygonscan.com/address/0x56676b6D007Acb62b59C19Fe53d7d94Ed9A23ae1#code) 可以看到，示例中的Ed3Coupon合约已经成功上链并完成开源！

![verify_scan](https://live.staticflickr.com/65535/52833926068_9df57f7909_b.jpg)

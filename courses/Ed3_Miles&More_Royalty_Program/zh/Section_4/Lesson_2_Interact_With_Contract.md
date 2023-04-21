# 和你的合约交互

## 📒 获取合约 ABI

就像我们在传统前端开发中，需要知道后端 API 的 url、参数等信息一样，我们与区块链交互也需要这样的信息！

还记得我们的合约编译后生成的 `artifacts` 文件夹吗？它就蕴含着我们所需要的文件，这类文件也叫作 ABI。

4个合约的 ABI 文件对应如下：

![abi-structure.png](https://i.postimg.cc/Zqgk1N65/abi-structure.png)

我们在前端项目根目录下新建一个 `abi` 文件夹，然后我们将合约项目中的4个 json 文件拷贝至 `abi` 文件夹中：

![abi.png](https://i.postimg.cc/BvdL9CsW/abi.png)

然后我们在 `index.jsx` 中导入 abi 文件：

```jsx
// ...
import { Card, Row, Col, Button, Typography } from "antd";
import ABI_GATE from "../abi/AirlineGate.json";
import ABI_TICKET from "../abi/TicketNft.json";
import ABI_POINT from "../abi/LoyaltyPoints.json";
import ABI_COUPON from "../abi/Coupon.json";

const { Title } = Typography;
// ...
```

## 🏠 获取合约地址

还记得你在 Mumbai Testnet 上部署的合约吗？那次部署的输出包括你的智能合约地址，看起来应该是这样的。

```none
Deploying contracts with account:  0x9f773d11C3eABb67Bd1827a983641b37c6C6B0a5
Account balance:  200000000000000000
MyToken address:  0xe7eF8d5C50fD89AaFF85384D50774aB15f0652FC
```

前端项目中需要知道合约地址，才能与合约进行交互。那么我们在 `index.jsx `中声明几个常量：

```jsx
// ...
import ABI_POINT from "../abi/LoyaltyPoints.json";
import ABI_COUPON from "../abi/Coupon.json";

const { Title } = Typography;
const GATE_ADDRESS = "0xBa147139813736Db98FBBb0567fF2A80D093D3e9";
const TICKET_ADDRESS = "0x8B939b4469BC384e841afE4d809D95F1373e81cF";
const POINT_ADDRESS = "0x0F4638eC264C25d6fBe59e66A9F3Cf0672B6DABF";
const COUPON_ADDRESS = "0x5bb8CcE2A46ab36970d10cD4087B7D172337FbFa";
// ...
```

## 🍬 一些准备工作

在我们开始实现与合约交互之前，我们需要添加一个 `useState` 和一个 `useEffect`：

```jsx
// ...
import { useState, useEffect } from "react";
// ...
const Home = () => {
  const [mounted, setMounted] = useState(false);
  // ...
  useEffect(() => {
    setMounted(true);
  }, []);

  if (!mounted) {
    return;
  }
  // return ...
}
```

为什么要这么做呢？

这是由于 Next.js 默认开启了服务端渲染，一旦前端界面的数据是异步获取的，那么就会产生 UI 渲染前后端不一致的[问题](https://nextjs.org/docs/messages/react-hydration-error)，导致页面报这样的错：

![hydration-failed.png](https://i.postimg.cc/mrjHyQ1h/hydration-failed.png)

而添加了 `mounted` 这个 state 之后，因为默认值是 `false`，无论是前后端，初始渲染的时候只会返回空，保证了渲染的一致性；待浏览器完成渲染之后，则会执行 `setMounted(true)`，正常渲染我们的界面。

## 📒 读取数据

有了 ABI 和合约地址，我们可以通过 wagmi 来获取合约数据了。

[wagmi](https://wagmi.sh/) 是一个 React Hooks 的集合，它提供了很多与钱包、账户、合约交互的能力。

首先我们来读取机票价格，这是被定义在 `Ed3AirTicketNFT` 合约的 `mintPrice` 方法，那么我们要用到 wagmi 的 `useContractRead`：

```jsx
import { useContractRead } from "wagmi";

const Home = () => {
  // ...
  const { data: mintPrice, isLoading: isLoadingMintPrice } = useContractRead({
    address: TICKET_ADDRESS,
    abi: ABI_TICKET,
    functionName: "mintPrice",
  });
  // ...
              <Card title="机票价格" loading={isLoadingMintPrice}>
                {mintPrice && <p>{mintPrice.toString()} MATIC</p>}
              </Card>
  // ...
}
```

这样一来我们的页面就显示出了机票价格：

![screenshot](https://i.postimg.cc/wMG1fmJz/read-mint-price.png)

正好对应部署脚本设置的 `10 ** 14` Wei，那么我们再添加一个格式化方法，将其转化为 MATIC。

首先引入一个新的库 bignumber.js：

在根目录新建 `util.js`：

## 📝 写入数据

向我们的合约写数据的代码与读数据没有超级大的区别。主要的区别是，当我们想向合约写入新的数据时，我们需要通知矿工，以便交易可以被开采。当我们读取数据时，我们不需要这样做。读取是 "免费 "的，因为我们所做的只是从区块链上读取，**我们没有改变它。**

下面是 mint 的代码：

我们在 `mounted` 完成前添加以下代码

```tsx
const { config } = usePrepareContractWrite({
  address: contractAddress,
  abi,
  functionName: "mint",
});
const { data, write: mint } = useContractWrite(config);
const { isError, isLoading, isSuccess } = useWaitForTransaction({
  hash: data?.hash,
});

useEffect(() => {
  setMounted(true);
}, []);
```

首先，我们需要通过 `usePrepareContractWrite`函数 来获取write函数所需要的配置参数。这里我们指定了合约地址、ABI以及要调用的方法名。

接着，我们将配置传入 `useContractWrite` 就可以获得区块链可识别的交易数据 `data`。

再通过 `useWaitForTransaction`函数 即可完成调用。

我们还要在 mint 卡片中添加按钮以便完成 mint 调用

```tsx
<div className={styles.card}>
  <div>Mint</div>
  <div>
    <button
      onClick={() => {
        mint?.();
      }}
    >
      Confirm
    </button>
  </div>
  {isSuccess && <div>Mint success</div>}
  {isError && <div>Mint error</div>}
</div>
```

很简单，对吧 :)?

这里最棒的是，当交易被挖出时，你实际上可以打印出交易哈希值，将其复制/粘贴到 [Mumbai Polygonscan](https://mumbai.polygonscan.com/), 并看到它正在被实时处理 :)。

当我们运行这个时，你会看到 Metamask 弹出并要求我们支付 "Gas"，我们用我们的假美元支付。 有一篇关于它的很好的文章[这里](https://ethereum.org/en/developers/docs/gas/). 试着弄清楚什么是 Gas:)。

我们刷新页面会发现 balance 和 supply 也增加了！

## 🎉 成功

真是一个好东西！我们现在有一个实际的客户端，可以读取和写入区块链的数据。从这里，你可以做任何你想做的事情。你已经有了基本的东西了。你可以建立一个去中心化的 Twitter 版本。你可以建立一种方式，让人们发布他们最喜欢的备忘录，并允许人们用 ETH 给发布最佳备忘录的人 "打赏"。你可以建立一个去中心化的投票系统，一个国家可以用它来投票选举一个政治家，在那里一切都公开而清晰。

可能性确实是无穷无尽的。

# 一些准备工作

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

还记得你在 Mumbai Testnet 上部署的合约吗？那次部署的输出包括你的智能合约地址。

前端项目中需要知道合约地址，才能与合约进行交互。那么我们在 `index.jsx `中声明几个合约地址常量：

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

## ⚒ 解决异步渲染问题

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

# 向合约写入数据

## 🎫 购买机票

向我们的合约写数据的代码与读数据没有太大的区别。读取是免费的，因为我们所做的只是从区块链上读取，**我们没有改变它。** 但是当我们向合约写入新的数据时，我们需要通知矿工，以便交易可以被开采，需要支付相应的 Gas fee。

所以记得先到 [Polygon Faucet](https://faucet.polygon.technology/) 领取一些测试用的 MATIC😀

💪让我们正式开始

购买机票需要调用 `Ed3AirlineGate` 的 `mint` 方法，我们这里会用到 wagmi 的 `usePrepareContractWrite`、`useContractWrite`、`useWaitForTransaction` 这些 hooks：

```jsx
import {
  useContractRead,
  useAccount,
  useBalance,
  usePrepareContractWrite,
  useContractWrite,
  useWaitForTransaction,
} from "wagmi";
// ...
const Home = () => {
  // ...
  // 购买机票，mint 积分
  const { config: configMint } = usePrepareContractWrite({
    address: GATE_ADDRESS,
    abi: ABI_GATE,
    functionName: "mint",
    args: [address],
    overrides: {
      value: mintPrice,
    },
  });
  const {
    data: dataMint,
    isLoading: isLoadingMintStart,
    write: mint,
  } = useContractWrite(configMint);
  const { isLoading: isLoadingMint } = useWaitForTransaction({
    hash: dataMint?.hash,
  });
  // ...
                <Col span={24}>
                  <Button
                    type="primary"
                    block
                    loading={isLoadingMintStart || isLoadingMint}
                    onClick={() => mint?.()}
                  >
                    购买机票
                  </Button>
                </Col>
  // ...
}
```

首先，我们需要通过 `usePrepareContractWrite` 配置合约地址、ABI、方法名、目标地址、支付金额等参数。

接着，我们将配置传入 `useContractWrite` 就可以获得交易数据 `data` 和写合约方法 `mint`。

然后将交易 hash 传递给 `useWaitForTransaction`，从而得知交易的状态。

写合约的 `mint` 方法传递给购买按钮的 `onClick` 事件，点击按钮执行购买机票的操作：

![buy-ticket.png](https://i.postimg.cc/1RGywkmv/buy-ticket.png)

在钱包中确认交易后，机票就购买成功了！刷新页面后，账户余额、机票、积分都会相应变化。

这里最棒的是，当交易被挖出时，你实际上可以打印出交易哈希值，将其复制/粘贴到 [Mumbai Polygonscan](https://mumbai.polygonscan.com/)，并看到它正在被实时处理 :)

## 🎟 兑换优惠券

类似的，我们来实现兑换优惠券的功能。

兑换优惠券稍有不同，从流程上说，首先有一个批准的操作，我们需要先批准积分的额度，才能消耗积分去兑换优惠券。

因此在我们的前端代码中，需要先调用 `Ed3LoyaltyPoints` 的 `approve` 方法，再调用 `Ed3Coupon` 的 `mint` 方法。

首先引入 @wagmi/core：

```shell
npm install @wagmi/core
```

@wagmi/core 是 wagmi 的核心模块，支持在 js 中直接与区块链交互（而不是通过 hook 的方式）。

然后修改 `index.jsx`：

```jsx
// ...
import { Card, Row, Col, Button, Typography, message } from "antd";
import {
  prepareWriteContract,
  writeContract,
  waitForTransaction,
} from "@wagmi/core";
// ...
const Home = () => {
  // ...
  const [isExchanging, setIsExchanging] = useState(false);
  // ...
  // 兑换优惠券
  const exchangeCoupon = async () => {
    setIsExchanging(true);
    const config = await prepareWriteContract({
      address: POINT_ADDRESS,
      abi: ABI_POINT,
      functionName: "approve",
      args: [COUPON_ADDRESS, 0],
    });
    const { hash } = await writeContract(config);
    await waitForTransaction({
      hash,
    });
    const config1 = await prepareWriteContract({
      address: POINT_ADDRESS,
      abi: ABI_POINT,
      functionName: "approve",
      args: [COUPON_ADDRESS, balancePoint.value],
    });
    const { hash: hash1 } = await writeContract(config1);
    await waitForTransaction({
      hash: hash1,
    });
    const config2 = await prepareWriteContract({
      address: COUPON_ADDRESS,
      abi: ABI_COUPON,
      functionName: "mint",
      args: [address],
    });
    const { hash: hash2 } = await writeContract(config2);
    await waitForTransaction({
      hash: hash2,
    });
    setIsExchanging(false);
    message.success("兑换成功");
    setTimeout(() => {
      window.location.reload();
    }, 500);
  };
  // ...
                <Col span={24}>
                  <Button block loading={isExchanging} onClick={exchangeCoupon}>
                    兑换优惠券
                  </Button>
                </Col>
  // ...
}
```

点击兑换优惠券，不出意外的话，你的钱包会弹窗3次，第一次是批准积分的额度为0：

![approve-point-0.png](https://i.postimg.cc/rm4kTC1p/approve-point-0.png)

第二次是批准积分最大额度，第三次是真正的兑换操作。

Cooool🎊我们实现了兑换优惠券的操作。学到这里，恭喜你基本掌握了如何使用 wagmi 完成与区块链交互😍

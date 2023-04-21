# 从合约读取数据

## 💰 获取机票价格

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

```shell
npm install bignumber.js
```

在根目录新建 `util.js`：

```javascript
import BigNumber from "bignumber.js";

const DECIMAL = String(1e18);

export const formatBigNumber = (bn, decimals = DECIMAL) => {
  return new BigNumber(bn.toString()).div(decimals).toString();
};
```

该方法将传入的 Bignumber 除以精度，这样便会得到格式化后的数字了，那么我们在 `index.jsx` 引入它：

```jsx
// ...
import { formatBigNumber } from "../util";
// ...
              <Card title="机票价格" loading={isLoadingMintPrice}>
                {mintPrice && <p>{formatBigNumber(mintPrice)} MATIC</p>}
              </Card>
// ...
```

![format-price.png](https://i.postimg.cc/PJzcbrzh/format-price.png)

看，机票价格变成了 0.0001 MATIC😃

## 📄 获取其余数据

那么我们照瓢画葫芦，完成后面4个卡片的数据读取。

* 已购机票对应 `Ed3AirTicketNFT` 的 `balanceOf` 方法
* 剩余机票对应 `Ed3AirTicketNFT` 的 `maxSupply` 减去 `totalSupply`
* 我的积分对应 `Ed3LoyaltyPoints` 的 `balanceOf` 方法
* 我的优惠券对应 `Ed3Coupon` 的 `balanceOf` 方法

示例代码如下：

```jsx
// ...
import { useContractRead, useAccount, useBalance } from "wagmi";
// ...
const Home = () => {
  // ...
  const { address } = useAccount();
  // 已购机票
  const { data: balanceTickets, isLoading: isLoadingBalanceTickets } =
    useContractRead({
      address: TICKET_ADDRESS,
      abi: ABI_TICKET,
      functionName: "balanceOf",
      args: [address],
    });
  // 机票总数
  const { data: maxSupply, isLoading: isLoadingMaxSupply } = useContractRead({
    address: TICKET_ADDRESS,
    abi: ABI_TICKET,
    functionName: "maxSupply",
  });
  // 已售机票
  const { data: totalSupply, isLoading: isLoadingTotalSupply } =
    useContractRead({
      address: TICKET_ADDRESS,
      abi: ABI_TICKET,
      functionName: "totalSupply",
    });
  // 我的积分
  const { data: balancePoint, isLoading: isLoadingBalancePoint } = useBalance({
    address,
    token: POINT_ADDRESS,
  });
  // 我的优惠券
  const { data: balanceCoupon, isLoading: isLoadingBalanceCoupon } = useContractRead({
    address: COUPON_ADDRESS,
    abi: ABI_COUPON,
    functionName: "balanceOf",
    args: [address],
  });
  // ...
            <Col span={12}>
              <Card title="已购机票" loading={isLoadingBalanceTickets}>
                {balanceTickets && <p>{balanceTickets.toString()} 张</p>}
              </Card>
            </Col>
            <Col span={12}>
              <Card
                title="剩余机票"
                loading={isLoadingMaxSupply || isLoadingTotalSupply}
              >
                {maxSupply && totalSupply && (
                  <p>{maxSupply.sub(totalSupply).toString()} 张</p>
                )}
              </Card>
            </Col>
            <Col span={12}>
              <Card title="我的积分" loading={isLoadingBalancePoint}>
                {balancePoint && <p>{balancePoint.value.toString()}</p>}
              </Card>
            </Col>
            <Col span={12}>
              <Card title="我的优惠券" loading={isLoadingBalanceCoupon}>
                {balanceCoupon && <p>{balanceCoupon.toString()} 张</p>}
              </Card>
            </Col>
  // ...
}
```

![read-contract.png](https://i.postimg.cc/W1z2nm0f/read-contract.png)

🎉就是这样了！我们成功实现了读取合约数据的功能🥰

可能有同学注意到，在获取“我的积分”时，我们使用了 `useBalance` 这个 hook，这是因为 `Ed3LoyaltyPoints` 合约是一个 ERC-20 标准合约，wagmi 对此提供了 `useBalance` 这个 hook。当然你也可以使用 `useContractRead`。

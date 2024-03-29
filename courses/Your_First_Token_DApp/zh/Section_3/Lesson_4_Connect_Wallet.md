# 连接钱包

## 🌈 使用 rainbowkit

因此，为了让我们的网站与区块链对话，我们需要以某种方式将我们的钱包连接到它。一旦我们将钱包连接到我们的网站，我们的网站将有权限代表我们调用智能合约。记住，这就像在网站上进行身份验证一样。

Rainbowkit 是一个很便捷的工具，它封装了 metamask、coinbase 等钱包的交互，帮助我们一次接入多种钱包！

我们先来安装一下 rainbowkit 以及相关依赖

```bash
npm install @rainbow-me/rainbowkit ethers@5.7.2
```

## ⚙️ 完成相关配置

在连接钱包之前，我们需要在 `pages/_app.tsx` 中添加一些配置

```tsx
import "@/styles/globals.css";
import "@rainbow-me/rainbowkit/styles.css";
import type { AppProps } from "next/app";
import { RainbowKitProvider, getDefaultWallets } from "@rainbow-me/rainbowkit";
import { configureChains, createClient, WagmiConfig } from "wagmi";
import {
  mainnet,
  polygon,
  optimism,
  arbitrum,
  goerli,
  polygonMumbai,
} from "wagmi/chains";
import { publicProvider } from "wagmi/providers/public";

const { chains, provider, webSocketProvider } = configureChains(
  [
    mainnet,
    polygon,
    optimism,
    arbitrum,
    polygonMumbai,
    ...(process.env.NEXT_PUBLIC_ENABLE_TESTNETS === "true" ? [goerli] : []),
  ],
  [
    publicProvider(),
  ]
);

const { connectors } = getDefaultWallets({
  appName: "RainbowKit App",
  chains,
});

const wagmiClient = createClient({
  autoConnect: true,
  connectors,
  provider,
  webSocketProvider,
});

export default function MyApp({ Component, pageProps }: AppProps) {
  return (
    <WagmiConfig client={wagmiClient}>
      <RainbowKitProvider chains={chains}>
        <Component {...pageProps} />
      </RainbowKitProvider>
    </WagmiConfig>
  );
}
```

除了rainbowkit，这里还有许多名字中带有“wagmi”的变量或者组件。[wagmi](https://wagmi.sh/)是rainbowkit的底层组件库，关于它更多有意思的功能，我们在下一章就能体会到！🤭

在这里我们都配置了哪些东西呢？我们将介绍 `configureChains` 函数所配置的

1. 用户可以选择的链
2. RPC服务的提供商，这里我们使用的是公共的，当然也可以添加我们部署合约时使用的 `alchemyProvider`

## 💰 建立一个连接钱包的按钮

在 Web3 的世界里，连接你的钱包实际上就是你的用户的一个“登录”按钮，所以我们在这里需要创建它。在一般情况下，创建这样的按钮是很复杂的，但好在我们有rainbowkit。修改`pages/index.tsx`一探究竟：

```tsx
import { ConnectButton } from "@rainbow-me/rainbowkit";
...
      <main className={styles.main}>
        <ConnectButton />
        <h2>My Token</h2>
        <div className={styles.card}>
          <div>Balance</div>
        </div>
        ...
      </main>
...
```

我们的修改非常简单。只是添加了一个 rainbowkit 的 `ConnectButton` 组件就完成了连接钱包按钮的创建。

![screenshot](https://live.staticflickr.com/65535/52752639984_350c350742.jpg)

连接钱包后就能看到我们的账户信息

![screenshot](https://live.staticflickr.com/65535/52752637909_4161b4e526.jpg)

是不是很简单呢？
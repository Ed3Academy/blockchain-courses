# 升级你的门户网站

## 告诉用户领取花费

Cool！我们完成了合约的部署，但千万不要忘记更新我们portal中合约的地址 `contractAddress`

我们可以添加一个卡片用来展示我们领取的花费

```tsx
          {totalSupply && <div>{totalSupply.toString()}</div>}
        </div>
        <div className={styles.card}>
          <div>mintPrice</div>
          {mintPrice && <div>{mintPrice.toString()}</div>}
        </div>
        <div className={styles.card}>
          <div>Mint</div>
```

别忘了从合约上读取花费数据！

```tsx
    functionName: "totalSupply",
  });

  const { data: mintPrice } = useContractRead<
    typeof abi,
    "mintPrice",
    number | BigNumber | undefined
  >({
    address: contractAddress,
    abi,
    functionName: "mintPrice",
  });

  const { config } = usePrepareContractWrite({
```

## 完成支付

现在我们需要在调用 `mint` 时，需要用户指定Token发放到哪个地址，所以我们需要在mint卡片中加一个输入框

```tsx
        </div>
        <div className={styles.card}>
          <div>Mint</div>
          <div>
            <input
	      value={mintTo}
              onChange={(e) => setMintTo(e.target.value)}
            />
          </div>
          <div>
```

同时声明一个state

```tsx
  const [mintTo, setMintTo] = useState("");
```

此外，我们需要在调用 `mint` 方法时 发送一些 Ether 到合约

```tsx
  const { config } = usePrepareContractWrite({
    address: contractAddress,
    abi,
    functionName: "mint",
    args: [mintTo],
    overrides: {
      value: mintPrice,
    },
  });
  const { data, write: mint } = useContractWrite(config);
  const { isError, isLoading, isSuccess } = useWaitForTransaction({
    hash: data?.hash,
  });
```

OK！大功告成，可以邀请你的好友来领取你的专属代币啦！

# 显示机票和优惠券

目前我们已经完成了这个 DApp 的大部分功能，还差一小部分：查看我们购买的机票🎫和优惠券🎟

## 🗄️ IPFS

机票和优惠券都是 ERC-721 合约，也就是 NFT 的一种，它的元数据是存储在 [IPFS](https://ipfs.tech/) 上的，前端需要对 IPFS 上的元数据做一些简单处理。

我们在项目根目录新建 `ipfs.js` 文件：

```javascript
// 替换前缀，使得浏览器能够访问IPFS路径
export const formatIpfsUrl = ipfsUrl => {
  if (ipfsUrl?.indexOf('ipfs://') === 0) {
    return ipfsUrl.replace('ipfs://', 'https://ipfs.io/ipfs/')
  }
  return ipfsUrl
}

// 获取IPFS元数据
export const getIpfs = async ipfsUrl => {
  const uri = formatIpfsUrl(ipfsUrl)
  const data = await fetch(uri)
  return data.json()
}

```

## 💻 搭建 UI 交互

首先还是在 `index.jsx` 实现我们的 UI 交互，已购机票右侧添加“详情”按钮，点击弹窗显示机票列表（包含图片、名称等信息）：

```jsx
// ...
import { Card, Row, Col, Button, Typography, Modal, Spin, message } from "antd";
import {
  prepareWriteContract,
  writeContract,
  waitForTransaction,
  readContract,
} from "@wagmi/core";
import { getIpfs, formatIpfsUrl } from "../ipfs";
// ...
const Home = () => {
  // ...
  const [isTicketModalOpen, setIsTicketModalOpen] = useState(false);
  const [isLoadingTicketNft, setIsLoadingTicketNft] = useState(false);
  const [ticketNftList, setTicketNftList] = useState([]);
  // ...
  const openTicketDetail = async () => {
    setIsTicketModalOpen(true);
    setIsLoadingTicketNft(true);
    const ticketNum = Number(balanceTickets.toString());
    const readTokenId = [];
    // 获取NFT对应的tokenId
    for (let i = 0; i < ticketNum; i++) {
      readTokenId.push(
        readContract({
          address: TICKET_ADDRESS,
          abi: ABI_TICKET,
          functionName: "tokenOfOwnerByIndex",
          args: [address, i],
        })
      );
    }
    const tokenIds = await Promise.all(readTokenId);
    // 获取NFT元数据IPFS路径
    const ipfs = await readContract({
      address: TICKET_ADDRESS,
      abi: ABI_TICKET,
      functionName: "tokenURI",
      args: [0],
    });
    // 获取NFT元数据
    const metadata = await getIpfs(ipfs);
    // 拼装NFT列表数据
    const metadatas = tokenIds.map((el) => ({
      tokenId: el,
      name: metadata.name,
      description: metadata.description,
      image: metadata.image,
    }));
    setTicketNftList(metadatas);
    setIsLoadingTicketNft(false);
  };
  // ...
      <main>
  // ...
                  <Card
                    title="已购机票"
                    loading={isLoadingBalanceTickets}
                    extra={
                      <span
                        className={styles.detail}
                        onClick={openTicketDetail}
                      >
                        详情
                      </span>
                    }
                  >
                    {userTickets && <p>{userTickets.toString()} 张</p>}
                  </Card>
  // ...
          <Modal
            title="我的机票"
            footer={null}
            open={isTicketModalOpen}
            onCancel={() => setIsTicketModalOpen(false)}
          >
            {isLoadingTicketNft && (
              <div className={styles.spin}>
                <Spin size="large" />
              </div>
            )}
            {!isLoadingTicketNft && (
              <Row gutter={[20, 20]}>
                {ticketNftList.map((nft) => (
                  <Col key={nft.tokenId} span={12}>
                    <Card
                      hoverable
                      cover={
                        <img
                          className={styles.cover}
                          src={formatIpfsUrl(nft.image)}
                          alt="NFT"
                        />
                      }
                    >
                      <Card.Meta
                        title={`${nft.name}#${nft.tokenId}`}
                        description={nft.description}
                      />
                    </Card>
                  </Col>
                ))}
              </Row>
            )}
          </Modal>
      </main>
  // ...
}
```

OK！点击详情看看我们的机票列表吧：

![my-tickets.png](https://i.postimg.cc/QdbFF1Rd/my-tickets.png)

Done！弹窗中显示了机票的图片、名称等信息，这些都是一个个的 NFT😎

查询优惠券的代码和机票几乎是一样的，我们在这里展示一下效果图：

![my-coupon.png](https://i.postimg.cc/sXV1PcZL/my-coupon.png)

大家快去尝试一下实现这个功能吧🧐

## 🎉 大功告成

至此，我们就完成了本次实践案例，希望能对你进入 Web3 领域有所启发！

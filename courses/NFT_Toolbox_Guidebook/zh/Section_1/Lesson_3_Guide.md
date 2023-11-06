# Q1:没有翻墙VPN怎么办？

没有翻墙VPN则使用Edge浏览器或者Firefox浏览器 ，请参照以下步骤进行钱包安装和使用

**步骤一：使用对应浏览器获取MetaMask钱包插件**

![get_wallet.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E8%8E%B7%E5%8F%96%E9%92%B1%E5%8C%85.png)

![get_wallet2.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E8%8E%B7%E5%8F%96%E9%92%B1%E5%8C%852.png)

**步骤二：添加MetaMask插件**

![Edge4.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/Edge%E6%B7%BB%E5%8A%A0%E6%8F%92%E4%BB%B6.png)

![Edge5.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/Firefox%E6%B7%BB%E5%8A%A0%E6%8F%92%E4%BB%B6.png)

**步骤三：MetaMask下载并安装完成，手动切换网络继续进行钱包创建**

扩展下载完成后，会默认连接 Ethereum 主网，因VPN限制可能存在连接超时，当Ethereum 主网连接超时弹出 无法连接的弹窗时，进行以下操作：

1. 通过 “切换网络” 连接到 Polygon 主网(NFT Toolbox部署在Polygon主网)

![change_network.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%88%87%E6%8D%A2%E7%BD%91%E7%BB%9C.png)

2. 点击添加网络

![add_network.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E6%B7%BB%E5%8A%A0%E7%BD%91%E7%BB%9C.png)

3. 手动输入网络信息并保存
   
网络名称：Mumbai

新的RPC URL：https://rpc-mumbai.maticvigil.com

链ID：80001

货币符号：MATIC

区块链浏览器URL（可选）：https://mumbai.polygonscan.com

![add_mumbai.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%88%87%E6%8D%A2%E7%BD%91%E7%BB%9C%E5%B9%B6%E4%BF%9D%E5%AD%98.png)

**步骤四：进行钱包创建**

参考 “前置准备” 章节的视频教程

![create_wallet.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%88%9B%E5%BB%BA%E9%92%B1%E5%8C%85.png)

**步骤五：钱包创建成功**

1.请将小狐狸固定到工具栏！！

![Edge6.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%9B%BA%E5%AE%9A%E5%B7%A5%E5%85%B7%E6%A0%8F.png)

![Firefox3.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/Firefox%E5%9B%BA%E5%AE%9A%E5%B7%A5%E5%85%B7%E6%A0%8F.png)

2.跟随 MetaMask 提示 点击“切换至 Mumbai 网络”

![change_mumbai_net.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%88%87%E6%8D%A2%E5%88%B0Mumbai.png)

3.回到 NFT Toolbox首页，点击刷新，点击左侧栏的“MetaMask”进行钱包连接，在小狐狸弹出框中点击确认，登录成功就可以开始体验 NFT Toolbox 啦 ~

![login_MM.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E8%BF%9E%E6%8E%A5MetaMask.png)


# Q2:小狐狸MeteMask插件安装后报错

![change_polygon_net.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/MetaMask%E6%8A%A5%E9%94%99.png)

在安装完浏览器插件后，偶发MetaMask会弹出报错信息，此时点击“重新启动MetaMask”，然后在浏览器扩展中找到“MetaMask”并将其固定到工具栏

此时可点击MetaMask图标打开页面，继续完成钱包创建步骤~

# Q2:为什么点击按钮后没有反应

首先检查一下小狐狸 MetaMask 是不是有未处理交易角标

（如果没有把小狐狸固定到工具栏的建议把小狐狸固定到浏览器工具栏）

![subscript.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%B0%8F%E7%8B%90%E7%8B%B8%E8%A7%92%E6%A0%87.png)

如果有角标则表示需要用户进行相关操作，点击小狐狸图标，弹出操作界面拉到底部进行操作

说明：

在 NFT Toolbox 平台，以下3个操作将会触发小狐狸MetaMask钱包弹出交互弹窗

1. 未登录情况下，点击 “MetaMask” ，此时要进行签名授权
2. 填完NFT信息后，点击“创建NFT”，此时要提交链上交易
3. 填完 赠送者的钱包地址，点击“赠送NFT”，此时要提交链上交易


# Q3:我的钱包地址在哪里获取

当要进行NFT赠送时，则需要输入接收者的钱包地址，有两个方式获取自己的钱包地址

**方式一：首页右上角下拉菜单 -> 复制地址**

![copy.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%A4%8D%E5%88%B6%E5%9C%B0%E5%9D%80.png)

**方式二：小狐狸MetaMask钱包中复制**

![copy1.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E9%92%B1%E5%8C%85%E5%A4%8D%E5%88%B6%E5%9C%B0%E5%9D%80.png)

# Q4：小狐狸添加网络失败怎么办
1.当你在进行添加测试网络的操作时，提示【连接到自定义网络时出错。】

2.当你在切换到测试网络时，小狐狸一直在loading状态，并且提示【我们无法连接到 Mumbai Testnet】

当你出现以上问题，就说明测试链的RPC地址失效了，这个时候你需要手动添加新的测试网络
![error1.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E8%BF%9E%E6%8E%A5%E9%94%99%E8%AF%AF.png)
![error2.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E4%B8%80%E7%9B%B4loading.png)


**步骤：添加可用的RPC地址**
1.访问https://chainlist.org/chain/80001

这个地址提供了Polygon Mumbai所有的PRC节点，以及节点的网络状态
![rpcpage.png](https://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E8%8E%B7%E5%8F%96RPC%E8%8A%82%E7%82%B9%E5%9C%B0%E5%9D%80.png)

2.在Mumbai RPC URL List列表中，选择Score和Privacy均为绿色打钩状态的RPC Server Address
![selectrpc.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E9%80%89%E6%8B%A9%E5%8F%AF%E7%94%A8RPC%E5%9C%B0%E5%9D%80.png)

3.点击地址后面的【Add to Metamask】按钮
![addrpc.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E7%82%B9%E5%87%BB%E6%B7%BB%E5%8A%A0%E5%9C%B0%E5%9D%80.png)

4.此时小狐狸会弹出的网络添加请求，在请求弹窗中，点击【批准】
![comfirmaddrpc.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E6%B7%BB%E5%8A%A0%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E7%AA%97%E5%8F%A3.png)

5.网络添加成功后，小狐狸弹出切换网络请求窗口，点击【切换网络】
![switchrpc.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E5%88%87%E6%8D%A2%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E7%AA%97%E5%8F%A3.png)

6.点击浏览器的小狐狸图标，小狐狸可正常显示账户余额，即表示切换网络成功

注意：若切换rpc后小狐狸依然连接不上测试网，则需要再重新选择一个可用的RPC地址
![success.png](http://gcdncs.101.com/v0.1/static/nft_toolbox_service/tutorial/%E6%AD%A3%E5%B8%B8%E7%8A%B6%E6%80%81.png?serviceName=nft_toolbox_service&attachment=true)

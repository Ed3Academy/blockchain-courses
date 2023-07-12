# 📊完善拍卖出价方法

## **🚧 教学目标**

1. 了解和掌握solidity引用类型
2. 了解和掌握映射类型
3. 了解和掌握条件判断
4. 了解和掌握异常处理

## **💚 任务描述**

在盲拍系统中，可以多个用户参与竞拍，也可以一个用户多次参与竞拍，此时需要系统判断和记录最高竞拍者信息，同时也要保存上一次竞拍的最高者信息，因此合约实现实现以下任务：    
1. 判断当前用户竞拍的价格是否高于最高竞拍价
2. 记录最高竞拍信息
3. 保存上一轮的最高竞拍信息到相应的账户，已备后续结束竞拍将钱退还给用户。

**本任务要求：**

1. 判断和记录最高竞拍信息
2. 保存上一次最高竞拍信息 

## **⚡ 相关知识**
知识点简述，（后续会替换AI课件，可以先参看AI脚本：）  
[AI课件](https://docs.qq.com/sheet/DSmdHWWNoT25LTENl?tab=e6n1tf)  
   

## **✨ 任务关键步骤分析**

1. 声明一个映射类型变量用于存放上一次最高竞拍者信息
2. 判断当前竞拍价是否高于最高竞拍价  
3. 如果当前竞拍价低于最高价，触发异常并回滚交易
4. 保存上一轮的最高竞拍者信息到映射变量  

## **✨ 任务实现**
1. **完善合约**  
    根据步骤提示，完善右侧合约文件中begin...end之间的代码。
3. **合约测试**  
   a. 编译合约：点击编译按钮

   ![compile_contract.png](https://ed3academy.xyz/github/courses/Bid_Master/compile_contract.png)

   b. 部署合约：选择部署选项卡，确认合约是否为待部署的合约，点击Deploy按钮，发布合约

   ![deploy.png](https://ed3academy.xyz/github/courses/Bid_Master/deploy.png)

   c. 选择测试账户：下拉列表为Remix提供的区块链上虚拟用户，选择一个区块链账户

   ![test_account.png](https://ed3academy.xyz/github/courses/Bid_Master/test_account.png)

   d. 设置竞拍价格、测试竞拍方法、查看相应的状态变量

   ![call_function.png](https://ed3academy.xyz/github/courses/Bid_Master/call_function.png)

其中，address显示的是最高竞拍者的地址，通过Remix完成合约的部署和合约方法的测试。

## **🌸 知识测试**  
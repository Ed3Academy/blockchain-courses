# 📊创建拍卖合约

## **🚧 学习目标**

1. 了解solidity语言及solidity源文件结构
2. 了解和掌握solidity值类型
3. 了解和掌握变量的声明、变量的类型及变量可见性
4. 了解和掌握函数的声明及函数修饰符payable
5. 了解以太单位
6. 了解Remix IDE如何开发，编译和部署合约

## **💚 任务描述**

在盲拍小程序中，首先需要建立一个竞拍合约，用于保存当前最高出价者与出价数值。
本任务要求：

1. 创建竞拍合约
2. 用户A发起竞标
3. 合约测试

## **⚡ 相关知识**

 后面替换为AI课件，可以先参看AI脚本：

[AI课件](https://docs.qq.com/sheet/DUUhnakNjSkZWWkt0?tab=ji2ydj)

## **✨ 任务实现**

1. 关键步骤说明

a）声明两个变量分别用于保存最高拍卖者的地址和最高竞拍价格

b）创建一个竞拍方法，允许接收用户的拍卖原生币

c）记录拍卖最高价与出价人

d）使用remix测试

2. 合约代码实现

   将下方代码，复制到右侧的合约文件中，根据提示步骤，填充begin..end之间的代码，完善合约。

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.17;
contract BlindAuction_1 {   

    /****************  begin *****************/
    //步骤1：声明两个变量用于存放最高竞拍者的地址和最高竞拍价格，命名为
    //highestBidder，highestBid（提示：正确使用数据类型、设置可见性）
   

     //步骤2：设置竞拍的方法可见性为外部，同时该函数可以接受原生币  
    function bid() {

     //步骤3：记录竞拍最高价的地址为当前交易的地址，竞拍最高金额为当前
     //接收的原生币数值（提示：使用全局变量）


    }

    /****************  end *****************/
} 
```

3. 合约测试

1）编译合约

![final-web.png](https://i.postimg.cc/QxzD4kDb/1.png)

2）发布合约

![final-web.png](https://i.postimg.cc/TYb6LvVj/2.png)

3）调试合约

a、选择一个区块链账户

![final-web.png](https://i.postimg.cc/8C6KwTCw/3.png)

b、设置竞拍价格、测试竞拍方法、查看相应的状态变量

![final-web.png](https://i.postimg.cc/8PfPLPfs/4.png)

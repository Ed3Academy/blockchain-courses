# 📊创建拍卖合约

## **🚧 教学目标**

1. 了解solidity语言及solidity源文件结构
2. 了解Remix IDE如何开发，编译和部署合约
3. 了解以太单位
4. 了解和掌握solidity值类型
5. 了解和掌握变量的声明、变量的类型及变量可见性
6. 了解和掌握函数的声明及函数修饰符payable

## **💚 任务描述**

在盲拍系统中，首先需要建立一个竞拍合约，用于保存当前最高出价者与最高出价数值。

**本任务要求：**

1. 创建竞拍合约
2. 保存最高竞拍信息
3. 测试合约

## **⚡ 相关知识**  
1. **以太单位**    
    在以太坊中，以太单位用于衡量货币数量、交易费用和合约中的数值。以下是一些常见的以太单位：  

    |  单位  | 说明 |
    | --- | --- |
    | Ether | Ether是以太币的基本单位，通常用于表示较大的货币值和交易金额  |
    | Wei  | Wei是以太币的最小单位，1 Ether等于10^18 Wei |
    | Gwei  | Gwei是以太币的下一级单位，1 Gwei等于10^9 Wei  |   

2. **变量的声明语法**       
    <数据类型> <可见性修饰符> <变量名>;

3. **变量的可见性**        
    |  可见性  | 说明 |
    | --- | --- |
    | public | 公开的变量可以被合约内外的所有人访问和调用  |
    | internal  | 合约内部及派生合约可访问，外部合约不可访问 |
    | private  | 只有合约自身可访问  |  

4. **函数的声明**  

    ```solidity
    function myFunction(uint256 myParam) public payable returns (uint256) {
        // 函数逻辑
           return  myParam * 2;      
    }
    ``` 
    其中：  
    a. function:函数关键字   
    b. myFunction：函数名   
    c. uint256 myParam：函数参数    
    d. public：函数公开性
    e. **payable: 声明该函数允许接收原生币和转账**  
    e. returns (uint256)：返回值类型声明  
    f. return  myParam * 2:  函数体    
    g. 函数说明：返回myParam*2  
 
     
后面替换为AI课件，可以先参看AI脚本：

[AI课件](https://docs.qq.com/sheet/DSmdHWWNoT25LTENl?tab=BB08J2)

## **✨ 任务实现**

1. **关键步骤说明**    
    a. 声明两个变量分别用于保存最高拍卖者的地址和最高竞拍价格  
    b. 创建一个竞拍方法，允许接收用户的拍卖原生币  
    c. 记录拍卖最高价与出价人  
    d.使用remix测试  
2. **完善合约**  
    根据步骤提示，完善右侧合约文件中begin...end之间的代码。
3. **合约测试**  
   a. 编译合约：点击编译按钮

   ![final-web.png](https://i.postimg.cc/QxzD4kDb/1.png)

   b. 部署合约：选择部署选项卡，确认合约是否为待部署的合约，点击Deploy按钮，发布合约

   ![final-web.png](https://i.postimg.cc/sgPpJ3Hv/deploy.png)

   c. 选择测试账户：下拉列表为Remix提供的区块链上虚拟用户，选择一个区块链账户

   ![final-web.png](https://i.postimg.cc/8C6KwTCw/3.png)

   d. 设置竞拍价格、测试竞拍方法、查看相应的状态变量

   ![final-web.png](https://i.postimg.cc/8PfPLPfs/4.png)

其中，address显示的是最高竞拍者的地址，通过Remix完成合约的部署和合约方法的测试。

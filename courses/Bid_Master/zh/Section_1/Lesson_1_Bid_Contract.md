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

本任务要求：

1. 创建竞拍合约
2. 保存最高竞拍信息
3. 测试合约

## **⚡ 相关知识**

 后面替换为AI课件，可以先参看AI脚本：

[AI课件](https://docs.qq.com/sheet/DSmdHWWNoT25LTENl?tab=BB08J2)

<a href="https://docs.qq.com/sheet/DSmdHWWNoT25LTENl?tab=BB08J2" target="_blank">ai</a>

## **✨ 任务实现**

1. 关键步骤说明  
    a. 声明两个变量分别用于保存最高拍卖者的地址和最高竞拍价格  
    b. 创建一个竞拍方法，允许接收用户的拍卖原生币  
    c. 记录拍卖最高价与出价人  
    d.使用remix测试  
   
2. 完善合约  
根据步骤提示，完善右侧合约文件中begin...end之间的代码。  
  
3. 合约测试  
    a. 编译合约     
![final-web.png](https://i.postimg.cc/QxzD4kDb/1.png)  

    b. 发布合约     
![final-web.png](https://i.postimg.cc/TYb6LvVj/2.png)  

    c. 调试合约  
下拉列表为Remix提供在区块链上在虚拟用户，选择一个区块链账户  
![final-web.png](https://i.postimg.cc/8C6KwTCw/3.png)  

    d. 设置竞拍价格、测试竞拍方法、查看相应的状态变量    
![final-web.png](https://i.postimg.cc/8PfPLPfs/4.png)
其中，address显示的是最高竞拍者在地址。

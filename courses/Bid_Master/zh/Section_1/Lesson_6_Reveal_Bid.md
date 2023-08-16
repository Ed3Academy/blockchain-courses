# 📊披露盲拍出价

## **🚧 教学目标**

1. 了解变量的存储位置
2. 了解如何输出调试信息
3. 了解和掌握循环结构(for,while,do...while)
4. 了解和掌握continue关键字
5. 了解和掌握事件Event

## **💚 任务描述**

竞拍者采用盲拍的方式参与竞拍，将出价哈希携带预存的以太币一起出价。当竞拍结束后，竞拍者披露在盲拍阶段提交的出价，披露的原则如下图所示：  

 ![call_function.png](https://i.postimg.cc/m2qrbhdV/11.png)

具体说明如下：  
s1. 用户A出了3次盲拍，分别为(1eth,true,abc) 订金1eth,(2eth,true,abc) 订金3the,(3eth,true,abc) 订金4eth     
s2. 用户传入所有的历史竞拍数据  
s3. 取一组历史竞拍数据计算哈希与用户当次的盲拍的出价哈希进行比较(第一组历史数据比对第一次盲拍，第二组比对第二次，以此类推)    
s4. 如果比对成功，执行s5，如果不对成功，回到s3，取下一组数据。    
s5. 将该次盲拍的预存金额累加到返回资金    
s6. 判断该次竞拍是否真拍且是否是最高竞拍出价，如果符合，执行S7，如果不符合执行S8    
s7. 从返回资金中扣除该次竞拍出价    
s8. 将该次的盲拍出价哈希设置为0，避免再次认领     

本任务目标：
1. 竞拍者一一披露自己的竞拍
2. 判断竞拍者披露的竞拍价与之前的出价是否一致
3. 判断竞拍者真拍的竞拍价是否是最高竞拍
4. 将最高的竞拍价转给受益人
 

## **⚡ 相关知识**
知识点简述，（后续会替换AI课件，可以先参看AI脚本：）  
[AI课件](https://docs.qq.com/sheet/DSmdHWWNoT25LTENl?tab=z32x08)  
   

## **✨ 任务关键步骤分析**  
1、相关流程图示    
1.1 各个合约方法调用时间节点参看下图：  

![call_function.png](https://i.postimg.cc/zXMP0t08/12.png)  

1.2 判断是否最高竞拍出价流程参看下图：  

![call_function.png](https://i.postimg.cc/wxnGk4WH/14.png)  

1.3 披露盲拍的流程图参看下图：    

![call_function.png](https://i.postimg.cc/1zYH342P/13.png)     

2、关键步骤分析  
2.1. 修改构造函数，新增披露时长。其中披露结束时间等于竞拍结束时间加披露时长  
2.2. 新增披露函数，披露函数在盲拍结束时间之后，披露结束时间之前执行 
2.3. 披露函数接受三个数组参数：values、fakes和secrets，分别存储用户在盲拍阶段提交的出价数值、是否为无效出价的标志，以及用于披露的密钥信息。
2.4. 披露函数检查传入的参数数组长度是否与用户在盲拍阶段提交的出价数量相等。  
2.5. 披露函数遍历用户在盲拍阶段提交的每个出价，对每个出价进行披露检查。  
2.6. 对于正确披露的出价，会将对应的订金退还给用户，并处理真正用于招标的金额。对于未正确披露的出价，可以下次继续披露，如果忘记相关历史出价信息，会导致披露失败。  
2.7. 如果本次竞拍为真拍且预存订金大于等于竞拍价，认定该次竞拍为用户真正用于招标的金额，判断该金额是不是竞拍最高价，如果是则从返回金额中把竞拍出价扣除  
2.8. 设置盲拍信息中当次的竞拍出价为0，避免再次认领同一笔定金  
2.9. 将定金返回给竞拍者  
2.10. 向区块链发出通知有用户进行了披露操作

## **✨ 任务疑难点分析**
1. 如何对竞拍者披露的招标历史计算哈希？   
解析：使用solidity内置的哈希函数keccak256
```Solidity
    keccak256(abi.code(value,fake,secte)); 
```   
示例中，通过调用keccak256函数，其中三个参数分别为：  
a. 竞拍出价  
b. 是否为无效出价，true：无效出价，false：真实出价  
c. 密码字符串   

2. 如何将历史竞拍数据和盲拍信息进行一一比较？    
解析：分别定义三个数组，一个用于存储历史出价values[]，一个用于存储是否伪拍fakes[]，一个用于存储密钥secrets[]
而竞拍数据在映射变量bids[msg.sender]数据中,  其中每个竞拍者对应的竞拍信息是一个结构体数组，结构体成员为竞拍价blindedBid和预存订金deposit，将历史出价信息进行哈希计算，将得出的哈希值和盲拍的竞拍哈希值进行比较。
```Solidity
//示例：将用户传入的历史竞拍数据和他的盲拍数据进行验证。
   bidHash = bids[msg.sender][i].blindedBid；
    //如果验证成功，披露成功
   if(bidHash == keccak256(abi.code(value[i],fake[i],secte[i]));)
   {
      ...
   } 
```  
3. 如何向区块链发出通知告知有用户进行了披露操作？    
解析：如果要将相关信息传递给区块链上的监听器，可以先声明event，再通过emit触发事件，发出通知。
```Solidity
// 声明一个竞拍结束时间  
event AutionEnded( )    
触发事件  
emit  AutionEnded()      
```
4. 如何将外部传入的出价哈希值转换为以太币单位？   
解析：  
4.1 ethers.utils.parseEther 是以太坊开发中的一个函数，用于将以太（Ether）的数值表示转换为对应的 Wei 单位表示。在以太坊中，Wei 是最小的货币单位，而以太是较大的货币单位。
4.2 访问https://playground.ethers.org/ ,这是一个在线的Ethereum Playground，提供了一个交互式的环境，让用户可以在浏览器中直接运行和测试以太坊智能合约和Ethers.js代码。
```JavaScript
    ethers.utils.keccak256(
    ethers.utils.defaultAbiCoder.
    encode(["uint256", "bool", "string"],
     [ethers.utils.parseEther("4.0").toString(), false, "101"]),
  );
``` 

## **✨ 任务实现**
1. **完善合约**  
    根据步骤提示，完善右侧合约文件中begin...end之间的代码。  

3. **合约测试**  
   a. 编译和部署任务6合约，设置盲拍时长为120s，设置披露时长为120s，选择一个受益人 ，点击transact执行部署（时长可以根据情况自行选择）  

     ![call_function.png](https://i.postimg.cc/jSvFVsrD/2.png)
   
   b.用户A出价了3次分别为(1eth,true,abc) 订金1eth,(2eth,true,abc) 订金3the,(3eth,true,abc) 订金4eth,访问https://playground.ethers.org/ ,生成相应的哈希值，已备测试调试使用。  


   c. 用户B出价了2次分别为(2eth,true,cde) 订金2th,(4eth,false,cde) 订金5eth，访问哈希值生成链接，生成相应的哈希值，已备测试使用。  


   d.用数组方式整理历史竞拍数据。如图所示。  

   ![call_function.png](https://i.postimg.cc/GpCNKPW5/hisBid.png)

   e. 选择账户A出价3次，将3次生成的哈希值，在调用盲拍函数时传入。执行盲拍方法，进行三次盲拍。
   
   ![call_function.png](https://i.postimg.cc/q76ycT4t/3.png)  
    

    c. 选择账户B出价2次，将2次生成的哈希值，在调用盲拍函数时传入。执行盲拍方法，进行两次盲拍。</span>  

   d. 用户A披露出价[1000000000000000000,2000000000000000000,3000000000000000000]，[true,true,false]，["abc","abc","abc"]。（注意：切换用户A），将相应的参数值分别传入给出价数组，是否无效出价数组，密码串数组
   
    ![call_function.png](https://i.postimg.cc/MZMCvWHC/4.png) 
     
    当披露方法调用成功，将在调试终点显示出设置的调试信息，如下图所示  
    ![call_function.png](https://i.postimg.cc/KzvgJqkT/5.png)  

   e. 用户B披露出价[2000000000000000000,4000000000000000000],[true,false],["cde","cde"]。<span color="red">（注意：切换用户B）</span>，操作方式与上面步骤一致。

      当披露方法调用成功，将在调试终点显示出设置的调试信息，如下图所示  
      ![call_function.png](https://i.postimg.cc/8kb6fr53/6.png) 

   f. 查看最高竞拍者  
   ![call_function.png](https://i.postimg.cc/9MV6Dk8v/7.png) 

   g. 结束竞拍，查看受益人账户是否发生改变  

     ![call_function.png](https://i.postimg.cc/R0GknRkh/8.png) 


   h. 用户A主动赎回竞拍出价，查看用户A账户是否发生改变  

     ![call_function.png](https://i.postimg.cc/FKZYdMFS/9.png)  
 
   
至此，完成了任务43合约的调试，部署和测试。
## **🌸 知识测试**  
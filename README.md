# 虚拟币分析模型(BitcoinModel）
[TOC]

## 项目依赖

1. python3的环境
2. requests==2.23.0
3. numpy==1.16.2
5. pandas==0.25.3
6. beautifulsoup4==4.9.3
7. 外部网络
8. tokenview.com提供的API接口（详见[tokenview.com的API说明文档](https://documenter.getpostman.com/view/5728777/RzZ6HfX2?version=latest#1ff89c3f-e8dc-45c6-af53-4a7a84bace9e)）
## 模型原理
### 模型整体架构
![d2ac150aa50a0f11dbaa0d548796a99e.png](en-resource://database/597:1)
### 数据爬取模块
![4b8d87b4e9a82c93b8e1fce97b648ae9.png](en-resource://database/599:1)
**更多细节请参考《江西709专案分析报告》**
## 快速使用
```
#建立一个列表存储钱包地址，注意有些钱包地址不区分大小写，需要传入小写的地址，如ETH钱包地址。
address1 = '0xEcF09966a1A83FA30a201D23FF139784Dd980738'
address2 = '0xE95B045ff8a0E65d65a31E7390F9fBC49eeE52AB'
address3 = '0x69444Cf008401fCC18986B008dDDBD172d000e01'
roots = [address1.lower(), address2.lower(), address3.lower()]
#设定一个工作目录
work_path = r'E:\ETH_FIND'
#传入时间段信息
stime = '2020-05-01 00:00:00'
etime = '2020-10-01 00:00:00'
#指定公链
public_chain = 'eth'
#指定代币
symbol='USDT'
#创建一个虚拟币模型类
cm = BitcoinModel(roots, work_path, stime, etime, public_chain=public_chain, symbol=symbol, headers=headers, backward_epoch=-1,is_crawle=True)
#执行虚拟币分析模型
cm.coin_analysis()
#对模型结果绘制关系网络图
cm.DrawNetwork()
```
## 模型参数详细介绍
### 创建虚拟币模型类的参数
```
cm=BitcoinModel(roots, work_path, stime, etime, forward_epoch=2, backward_epoch=2, backward_base=1, headers=None, public_chain='eth', symbol='USDT', model_epoch=2, is_crawle=True)
```

#### 输入参数


* roots为列表，传入多个初始的钱包地址

*  work_path为字符串，传入结果存储的工作路径

* stime为字符串，起始时间，必须按照时间格式传入参数“YYYY-mm-dd HH:mm:ss”

* etime为字符串，终止时间，必须按照时间格式传入参数“YYYY-mm-dd HH:mm:ss”

* forward_epoch为整数，默认为2，指定初始化爬取数据基于roots向下游爬取两层

* backward_epoch为整数，默认为2，基于backward_base层地址向上爬取两层

* headers需要传入字典，默认为None, 需要传入浏览器的headers字典以伪装浏览器请求

* public_chain为字符串，默认为'eth'，需传入小写的公链简称

* symbol为字符串，默认为'USDT'，代币符号

* model_epoch为整数，默认为2，指定虚拟币分析模块中资金分析模型执行的次数

* is_crawle为逻辑型，默认为True，需要爬取交易数据；False仅依靠本地数据进行资金分析，不爬取数据
### 执行虚拟币分析模块
```
cm.coin_analysis()
```
#### 输出结果

* 网络请求错误及其他错误的日志文件log.txt
* 存储每个钱包地址明细数据的文件夹eth_tests
* 虚拟币关系网络中的钱包地址节点评估表'节点分析.xlsx'
* 存储关系网络中每个节点周期分析结果的明细文件夹'周期明细'
* 可用于I2软件和networkx模块绘图的数据'绘图数据.xlsx'
* 代币混入分析的数据表'混入分析.xlsx'
* 钱包地址唯一标识的编码表'钱包地址索引.xlsx'
### 执行关系网络绘图模块
```
cm.DrawNetwork(ncolor=200, ecolor=200, nsize=1000, esize=50)
```
#### 输入参数

* ncolor为整数，调整节点颜色缩放比例的参数
* ecolor为整数，调整边颜色缩放比例的参数
* nsize为整数，调整节点大小缩放比例的参数
* esize为整数，调整边粗细缩放比例的参数
#### 输出结果

* 梳理的虚拟币关系网络图'关系网络图.png'
![a74dcc1ae4cd5d71cdca668058607418.png](en-resource://database/601:1)
## 模型特性
### 优势

* 虚拟币分析模型实际完成了爬取、分析和绘图等工作，调用简单方便，非常灵活；
* 数据爬取通过tokenview.com的API接口，爬取数据比较稳定，不需要访问国外的网站；
* 支持了多个智能合约公链如ETH、ETC、TRX、NEO、ONT、EOS等的分析工作；
* 支持了多种代币的梳理分析工作。

### 劣势

* 模型依赖于tokenview.com的API接口；
* tokenview.com的API接口有访问频率的限制，每1分钟才能请求一次；
* tokenview.com的API有访问数据量的限制，每个钱包地址最多访问最近发生的2500条交易数据。
## 未来期望
实现neo4j版本的图数据库存储，以满足用户的虚拟币关系查询需求。

# InvoryRaptor
InvoryRaptor项目介绍

系统为全球首个物联网神经网络平台（IOTNN），使接入设备按照神经网络模式进行工作。物联网神经网络是一种模拟人脑的神经网络以期能够实现类人操控处理的能力。
人脑中的神经网络是一个非常复杂的组织。成人的大脑中估计有1000亿个神经元之多。
<div align=center>
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/dn.jpg" alt="system" title="system" width="265" height="187" />
</div>
一个神经元通常具有多个树突，主要用来接受传入信息；而轴突只有一条，轴突尾端有许多轴突末梢可以给其他多个神经元传递信息。
轴突末梢跟其他神经元的树突产生连接，从而传递信号。这个连接的位置在生物学上叫做“突触”。
人脑对于事务的处理往往交给不同的神经元
<div align=center>
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/sjy.jpg" alt="system" title="system" width="398" height="237" />
</div>
从物联网角度来说，每个物品会发送不同类型的消息，而这些消息会被不同的"神经元"进行处理，处理的结果会传导到另一些"神经元中"
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/nn.png" alt="system" title="system" width="367" height="278" />

在本平台中的神经元叫做Angler，每个Angler负责处理某一类传导过来的信息，这些信息可能来自于输入层，也可能来自于其他Angler处理后的消息。
## 1、逻辑组成部分
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/system.jpeg" alt="system" title="system" width="856" height="718" />

### 1.1、Angler
为平台中最小的逻辑调度的单元，angler在平台中的位置类似于操作系统中的线程概念。一般为一个docker里封装的一个应用，运行在kubernetes pod中的一个执行某个微服务。
Angler为自己所在的matrix提供某种微服务。
Angler向外提供服务的方式为两种：
1.	通过tcp 8080端口，向外提供http服务接口
2.	订阅kafka的消息，并对应进行处理。订阅topic的名称格式为：
	matrix名称_angler名称
一般情况下相同的angler在matrix可运行多个副本，提供负载均衡能力。几个副本的angler中有一个为主angler，
某些特殊任务（如微信更换token，由主angler完成），其他angler为辅angler。当主angler关闭或出现异常时，副angler中将自动选举出一个为新的主master。

### 1.2、Matrix
Matrix是平台中通道的配置，对应一种类型的设备。例如移动端、PC端等由于提供通讯协议及逻辑可能并不相同，因此二者使用不同的matrix进行通道拆分。
每个matrix在kubernetes中拥有同名的namespace进行隔离，matrix中的angler的pod及server运行在该namespace中。

### 1.3、Project
Project是一个应用单元，承担某个完整的应用场景。是平台中资源分配单元，一个Project中的所有angler程序共享存储、参数、状态等信息，类似于操作系统中的进程的概念。

## 2、项目组成
## 2.1、PostOffice项目
[PostOffice项目地址](https://github.com/IvoryRaptor/postoffice)


## 3、其他约定及标准
### 3.1、Zookeeper存储结构
[Zookeeper存储结构](https://github.com/IvoryRaptor/InvoryRaptor/tree/master/zookeeper)

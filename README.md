# InvoryRaptor
InvoryRaptor项目介绍

系统为物联网神经网络平台，使接入设备按照神经网络模式进行工作。
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/nn.png" alt="system" title="system" width="856" height="718" />

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

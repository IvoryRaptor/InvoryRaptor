# Zookeeper存储结构
>整个平台中，各应用模块同步状态、服务发现及分布式锁等功能，采用中间件Zookeeper。系统规定了以下几个节点及节点结构。

整个平台中平台中

## 1 project中共享参数

[project中共享参数](https://github.com/IvoryRaptor/InvoryRaptor/blob/master/zookeeper/PROJECT.md)

## 2 angler运行状态
[angler运行状态](https://github.com/IvoryRaptor/InvoryRaptor/blob/master/zookeeper/ANGLER.md)

## 3 IOTNN转发规则结构
>> PostOffice通过Zookeeper配置。决定消息来到时转发到哪个TOPIC

[Zookeeper中Postoffice转发规则结构](https://github.com/IvoryRaptor/InvoryRaptor/blob/master/zookeeper/POSTOFFICE.md)

>> Dance发送消息时，除指定的Topic以外，还需要转发到哪些TOPIC。

[Zookeeper中Dance转发规则结构](https://github.com/IvoryRaptor/InvoryRaptor/blob/master/zookeeper/DANCE.md)

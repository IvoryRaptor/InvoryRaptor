# Zookeeper中Postoffice转发规则结构
>当Angler部署到集群中，PostOffice将自动支持其消息转发。当Angler从集群中移除时，PostOffice将自动关闭该对用的消息支持。
## 转发规则结构
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/zk-postoffice.png" alt="zk-postoffice" title="zk-postoffice" width="294" height="404" />

postoffice监视消息转发规则结构如下，postoffice节点下每个matrix拥有一个自己名称命名的节点，节点下为消息resource与action的结合，中间用”.”分割，节点下为目标kafka的topic名称。所有节点为固定节点，除非管理后台删除否则节点一旦创建不会被系统自动删除。
angler启动后，会对自己订阅的kafka的topic进行确认，如果不存在则添加对应子节点。
# project中共享参数
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/zk-project.png" alt="zk-project" title="zk-project" width="294" height="404" />

project中共享参数存储在zookeeper的/project子节点下，每个project拥有以自己名称为路径的子节点。
子节点下为该project存储共享参数的键值对节点，节点名称为参数名，节点值为参数的值。
所有project上的节点都为固定节点，除非应用程序主动删除，否则永久保持。以避免由于模块升级时，导致某些参数丢失而影响其他模块。如果某应用模块对其他某模块强依赖，则应检测angler运行状态。

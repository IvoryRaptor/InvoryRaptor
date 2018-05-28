# Angler设计

为平台中最小的逻辑调度的单元，angler在平台中的位置类似于操作系统中的线程概念。一般为一个docker里封装的一个应用，运行在kubernetes pod中的一个执行某个微服务。
Angler为自己所在的matrix提供某种微服务。
Angler向外提供服务的方式为两种：
1.	通过tcp 8080端口，向外提供http服务接口
2.	订阅kafka的消息，并对应进行处理。订阅topic的名称格式为：
	matrix名称_angler名称
一般情况下相同的angler在matrix可运行多个副本，提供负载均衡能力。几个副本的angler中有一个为主angler，
某些特殊任务（如微信更换token，由主angler完成），其他angler为辅angler。当主angler关闭或出现异常时，副angler中将自动选举出一个为新的主master。


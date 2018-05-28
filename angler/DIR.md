# Angler应用程序目录结构
## 启动代码
一般情况下，angler应用目录下，具备几个启动程序，分别是：

dev.py      开发调试用的代码，代码中会配置main.py文件中启动程序所需要的变量值，并调用main.py中的启动代码

main.py     程序启动代码，主要包含根据传入参数修改配置文件，并启动程序angler应用

release.py  docker启动代码，将环境变量传入main.py的启动函数中，并启动应用程序。

## resources 资源
配置resources响应服务，每个文件包含一个resource服务，文件名与其中的类相对应：

文件名即resources名称，全为小写，单词间用_隔开。

类名称用驼峰编码法，各单词首字母大写，其余字母小写。类要求直接或间接从angler.MQHandler继承
其中实现的函数即与消息中的action相同。

例如：需要接收及相应admin.login这个消息，那么需要建立admin.py文件

然后书写如下代码：

```
class AdminMQ(MQHandler):
    def login(self):
        pass
```
当有admin.login消息到达时，该函数将响应该消息。
获取消息的其他内容，下表如下：

名称 | 类型 | 描述
---- | --- | ---
self.angler | Angler | 系统中的angler对象
self.packet | MQMessage | 当前处理的数据包
self.source | Address | 触发处理的消息源
self.destination | Address | 需要处理及响应的设备
self.matrix | string | matrix名称
self.device | string | 设备名称
self.resource | string | 资源名称
self.action | string | 操作名称
self.payload | byte[] | 附加数据

以上变量都是由MQHandler所提供的，在系统中还有其他一些MQHandler会重载或扩展其他的类型。
其中还包括一些基本功能函数。


### resource访问装饰器定义
系统中定义了几种针对resource权限访问的装饰器

#### 基于session的权限控制
在设备对应的session中，加入设备可调用的resource及对应的action。
在设备的session的permissions键上存储对应的支持的动作列表：admin.login等
有两种修饰方法，可以修饰在resource对应的函数上，也可以修饰在resource的类上，
分包修饰符为session_permissions_class和session_permissions_func
```
@session_permissions_class
class MenuMQ(MQJsonHandler):
    def all(self):
    permissions = self.get_session('permissions')
    self.reply({
        "data": mongo.find(
            MONGODB_COLLECTION,
            {
                'permission': {'$in': permissions}
            },
            {'order': 1}
        )
    })
    
class MenuMQ(MQJsonHandler):
    @session_permissions_func
    def all(self):
        permissions = self.get_session('permissions')
        self.reply({
            "data": mongo.find(
                MONGODB_COLLECTION,
                {
                    'permission': {'$in': permissions}
                },
                {'order': 1}
            )
        })
```

#### 基于契约的权限控制
分别为mongo_contract_class及分别为mongo_contract_func。


 
## services 服务
该目录中包含包文件__init__.py
该文件中，包含resources中应用的外部资源，如mongodb数据库、关系型数据库等资源。

## config 配置文件
应用中运行的配置文件
注意，不同类型的配置文件放置在不同的目录中，该目录下不放置任何配置文件。这是为了当kubernetes中对configmap文件包组合时，相互的影响。
默认包含的配置文件：
### 1.1.1 angler配置文件
位置：/config/angler/config.yaml配置

配置文件结构如下：
```
angler: test
matrix: a1A325fYEJX
project: iot
services:
  mongo:
    database: iot
    host: mongodb.default
    port: 30707
session:
  host: redis.default
  port: 6379
source:
  group: test
  host: kafka.default
  port: 9092
  query: a1A325fYEJX_test
sync:
  host: zookeeper.default
  port: 2181
  type: zookeeper
```

### 1.1.2 日志配置文件
位置/config/log/logging.conf
```
[loggers]
keys=root,angler

[handlers]
keys=consoleHandler

[formatters]
keys=simpleFormatter

[logger_root]
level = DEBUG
handlers=consoleHandler

[logger_angler]
level = INFO
handlers = consoleHandler
qualname = angler
propagate = 0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[formatter_simpleFormatter]
format=[%(levelname)s] %(asctime)s %(name)s %(message)s
datefmt=

```
## 
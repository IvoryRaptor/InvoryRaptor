# Angler配置文件

> 应用中运行的配置文件
注意，不同类型的配置文件放置在不同的目录中，该目录下不放置任何配置文件。这是为了当kubernetes中对configmap文件包组合时，相互的影响。
默认包含的配置文件：

## 1、angler运行配置文件

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

angler      表示Angler名称，该应用将监听Topic为matrix+angler，之间用_分割
matrix      表示该运行angler所在的matrix。matrix在kubernetes中运行在不同的namespace中
project     表示应用所在的项目集群，多个matrix共享project的资源
services    存储不同service的配置，子节点名称与service中的__init__.py中定义的对象名称相同。
            （注意：就算不需要配置文件，此处也需要有对应节点，否则该服务不会被加载到系统中，不会被执行）
session     系统使用的session的配置，目前session只支持redis


## 2、日志配置文件
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
Angler各模块拥有自己的日志输出命名空间，在此处配置可以将各模块区分开，
## 3、其他配置文件
其他配置文件，需放置在应用的config目录中。以服务名称对应目录名
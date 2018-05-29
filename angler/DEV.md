# Angler应用开发
## 1、概述
在本平台中由POSTOFFICE将信息根据设备类型及信息类型进行分类，传递给不同的"神经元"组中进行处理（平台中的神经元叫做Angler）。

## 2、添加Resource
例如：定义Resource admin类

1、那么需要建立admin.py文件。文件名全小写，如果是两个单词组成，则之间用_分割。

2、然后书写如下代码：
```
from angler.handlers import MQHandler


class AdminMQ(MQHandler):
    pass
```
类名称用驼峰编码法，各单词首字母大写，其余字母小写。类要求直接或间接从angler.MQHandler继承。


## 3、添加Action
定义与action同名的函数，以处理该类型的消息。
```
from angler.handlers import MQHandler


class AdminMQ(MQHandler):
    def login():
        pass
```

### 3.1、 处理代码中，可以使用如下方式，获取消息内容：

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

### 3.2、使用session
> Session 在平台中为每个设备提供状态保持的存储。每台设备在在线的全生命周期中，不同Angler种共享Session

#### 3.2.1、获取Session
函数名：*get_session*

参数：

名称 | 类型 | 描述
---- | --- | ---
key | string | session中的变量名
default | any | 默认值，当变量不存在时，返回该变量

#### 3.2.2、存储Session
函数名：set_session

参数：

名称 | 类型 | 描述
---- | --- | ---
key | string | 存储在session中的变量名
value | any | 变量对应的值

#### 3.2.3、清除Session
函数名：*clear_session*

*注意：程序需要手工清除session，这是因为当设备离线时，Angler可以收到离线消息(device.offline)，处理离线后的事情，
如果session自动被清理，则会引发程序无法获取设备运行时的状态等内容。*


### 3.3、发送消息
>将消息发送给其他Angler，或者发送给设备端
#### 3.3.1、回复消息

#### 3.3.2、发送
函数名：reply

参数：

名称 | 类型 | 默认值 |描述  
---- | --- | --- | ---
resource | string | 处理消息的resource | 消息的resource
action | string | _ + 处理消息的action | 消息的action
payload | byte[] |  | 附加数据


## 4、添加访问限制
>系统中以装饰器限制resource及其action是否可访问的控制，

### 4.1 基于session的权限控制
在设备对应的session中，加入设备可调用的resource及对应的action。

在设备的session的permissions键上存储对应的支持的动作列表：admin.login等

有两种修饰方法，可以修饰在resource对应的函数上，也可以修饰在resource的类上，

分包修饰符为session_permissions_class
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
```

session_permissions_func
```
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

### 4.2 基于契约的权限控制
>用于控制设备A是否可以调用设备B的Resource的Action，分别为mongo_contract_class及分别为mongo_contract_func。


## 5 service
>service为各resource提供统一的外部服务，如数据库、zookeeper、redis等

### 5.1 使用service
> 系统中所有resource调用的外部资源都应在service的包文件(\_\_init\_\_)中定义


### 5.2 自定义service
[详见](https://github.com/IvoryRaptor/InvoryRaptor/blob/master/angler/SERVICE.md)

## 6 获取设备连接的POSTOFFICE地址
函数名：*find_postoffice*

名称 | 类型 | 描述
---- | --- | ---
matrix | string | matrix名称
device | string | 设备名称

返回POSTOFFICE服务编号，如果设备不在线，返回None
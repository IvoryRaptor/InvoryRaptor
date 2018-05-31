# 其他MQHandler
## 1、Payload为Json格式（MQJsonHandler）
这里主要用于处理Web应用，通讯使用JSON格式。
```
from angler.handlers import MQJsonHandler


class AdminMQ(MQJsonHandler):
    def list(self):
        pass
```
### 1.1、回复(reply)
函数：reply

参数：

名称 | 类型 | 默认值 |描述  
---- | --- | --- | ---
payload | dict | {} | 附加数据
resource | string | 处理消息的resource | 消息的resource
action | string | _处理消息的action | 消息的action

### 1.2、
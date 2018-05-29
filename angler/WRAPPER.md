# Angler Resource访问控制
>系统中定义了几种针对resource权限访问的装饰器，可以在Resource类上做访问限制，也可以在Action操作上做访问限制

## 1、基于session的权限控制
系统可以在设备上线后，在其session中添加键值为permissions，值为Resource.Action字符串数组。
例如：
```
[
    'document.all',
    'document.get',
    ...
]
```
如果的Resource.Action存在于该数组中，则允许访问，否则不允许访问。

在平台中实现以上功能，Angler提供了两个装饰器。session_permissions_class和session_permissions_func

### 1.1、对函数的权限控制
在函数上设置权限控制，例如menu.all函数做权限限制。在该函数上增加session_permissions_func装饰器

例如：
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
### 1.2、对类所有函数加控制
在类上增加权限控制，例如menu所有消息都进行权限限制。在该类上增加session_permissions_class装饰器

例如：
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

## 2、基于MongoDB契约的权限控制

分别为mongo_contract_class及分别为mongo_contract_func。
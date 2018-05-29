# Angler自定义服务
> 自定义service同样存储在services目录中，每个类，必须显性或隐性的继承自angler.IService
继承自angler.IService的类必须实现以下4个函数：

## 1、 构造函数
```
def __init__(self):
    IService.__init__(self, name)
```
需要调用IService的构造函数，需要传入服务名称。方便模块日志输出，模块日志命名空间为angler.模块名

## 2 配置函数
函数名：*config*
参数：conf，Angler配置文件中对应节点的内容
用于初始化部分需要准备的内容，该函数执行在start之前。这么设计的目的在于，
避免由于配置文件错误导致某些模块被初始化导致系统不稳定。

## 3 启动服务函数
函数名：*start*
参数：angler，Angler系统对象，系统对象集合。
启动服务，服务启动完成后，服务应该可以被resource各部分调用。

#### 4 停止服务函数
函数名：*stop*
参数：无
系统停止运行时会被调用，做一下析构操作。

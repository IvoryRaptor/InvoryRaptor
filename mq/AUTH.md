# 验证规则
>MQTT连接报文CONNECT，使用MQTT协议标准的Username及Password进行验证。为了避免密码泄露引起安全问题，因此密码为秘钥加密结果。

## 客户端标识符 Client Identifier

服务端使用客户端标识符 (ClientId) 识别客户端。连接服务端的每个客户端都有唯一的客户端标识符（ClientId）。
客户端和服务端都必须使用ClientId识别两者之间的MQTT会话相关的状态。
客户端标识符 (ClientId) 必须存在而且必须是CONNECT报文有效载荷的第一个字段

**建议：使用唯一编号如设备编号或Mac地址作为该信息，因为如果两个设备连接的ClientId相同，则后面的链接会顶替掉前面的链接，
前面的链接将自动关闭**

由于鉴权需求，传递clientId后需要添加以下信息：
"|securemode=3,signmethod=hmacsha1,timestamp=132323232|"

* securemode 验证模式

* securemode 加密模式
hmacsha1或hmacMD5，目前仅支持这两种

* timestamp 时间戳

## Username
服务端可以将它用于身份验证和授权，在系统中固定为
**deviceName+"&"+productKey**
* deviceName 设备名称
* productKey 设备类型Key，也就是系统中的Matrix

## Password
服务端可以将它用于身份验证和授权，此处为使用加密算法进行加密的结果。加密方法如下：
**sign_hmac(deviceSecret,content)**
* sign_hmac 加密函数，由前面clientId传递的signmethod决定。目前仅支持hmacsha1或hmacMD5
* deviceSecret 设备秘钥，一机一密
* content 加密内容，提交给服务器的参数（productKey,deviceName,timestamp,clientId）, 按照字母顺序排序, 然后将参数值依次拼接

----

## 示例：
### 参数为：
变量名 | 值
---- | ---
clientId | 12345
deviceName |  device
productKey | pk
timestamp | 789
signmethod | hmacsha1
deviceSecret | secret

### MQTT Connection Message为：
变量名 | 值
---- | ---
mqttclientId | 12345|securemode=3,signmethod=hmacsha1,timestamp=789|
username | device&pk
password | hmacsha1("secret","clientId12345deviceNamedeviceproductKeypktimestamp789").toHexString();  

//最后是二进制转16制字符串，大小写不敏感。 这个例子结果为 FAFD82A3D602B37FB0FA8B7892F24A477F851A14

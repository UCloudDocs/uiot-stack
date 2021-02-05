
## rrpc简介
rrpc是基于MQTT协议制定的一套请求和响应的同步机制，物联网平台提供API给服务端，设备端只需按照固定的格式恢复消息，服务端使用API即可同步获取设备端的响应结果

## topic格式
* rrpc发布topic：/$system/${productSN}/${deviceSN}/rrpc/request/${requestID}
* rrpc订阅topic：/$system/${productSN}/${deviceSN}/rrpc/response/${requestID}
其中productSN是产品序列号，deviceSN是设备序列号,requestID是云端生成的唯一的RRPC消息ID

## 子设备驱动rrpc使用方法
子设备驱动（modbus dlt645等）都已支持rrpc功能，无需增加代码逻辑和驱动配置

## 以modbus子设备驱动为例

* 安装部署完modbus子设备驱动

* 添加云端和子设备上行和下行的rrpc消息路由

### 增加云平台rrpc消息发送到子设备的消息路由

![down.png](https://i.loli.net/2021/02/04/bpkw3JeT7gXdYqE.png)

### 增加子设备rrpc消息到云平台的消息路由

![up.png](https://i.loli.net/2021/02/04/GM3RuvBPUxCq8HE.png)

* 通过设备调试发送操作到modbus驱动

![test.png](https://i.loli.net/2021/02/04/nf7khzxu4sVy9qA.png)

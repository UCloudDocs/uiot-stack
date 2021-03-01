
# RRPC简介
RRPC是基于MQTT协议制定的一套请求和响应的同步机制，IoT平台提供API给服务端，设备端只需按照固定的格式回复消息，服务端使用API即可同步获取设备端的响应结果



## Topic格式
* RRPC发布topic：/$system/${productSN}/${deviceSN}/rrpc/request/${requestID}

* RRPC订阅topic：/$system/${productSN}/${deviceSN}/rrpc/response/${requestID}

其中productSN是产品序列号，deviceSN是设备序列号,requestID是云端生成的唯一的RRPC消息ID



## 子设备驱动RRPC使用方法

子设备驱动（Modbus DLT645等）都已支持RRPC功能，无需增加代码逻辑和驱动配置。



## 以Modbus子设备驱动为例

使用流程

1. 安装部署完Modbus子设备驱动；
2. 添加云端和子设备上行和下行的RRPC消息路由；
3. 通过设备调试发送操作到Modbus驱动；

**增加云平台RRPC消息发送到子设备的消息路由**

![图片](../../images/rrpc-11.png)

**增加子设备RRPC消息到云平台的消息路由**

![图片](../../images/rrpc-22.png)

**通过设备调试发送操作到modbus驱动**

![图片](../../images/rrpc-33.png)



## 应用使用RRPC方法

应用可直接调用RRPC API（/api/v1/mqtt/rrpc）向设备发送Topic消息并同步获得执行结果。

API调用方法[查看链接](uiot-stack/IoT平台开发指南/消息通信?id=RRPC消息)
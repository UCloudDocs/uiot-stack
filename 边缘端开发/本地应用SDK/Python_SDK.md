# Python SDK参考文档


## from iotedgeapplicationlinksdk
* **[getLogger()](#getLogger)**

---
## from iotedgeapplicationlinksdk.client
* **[get_application_config()](#get_application_config)**
* **[get_application_name()](#get_application_name)**
* **[get_gateway_product_sn()](#get_gateway_product_sn)**
* **[get_gateway_device_sn()](#get_gateway_device_sn)**
* **[publish()](#publish)**
* **[register_callback()](#register_callback)**

---
<a name="getLogger"></a>
### getLogger()
返回应用内置logger。

---
<a name="get_application_config"></a>
### get_application_config()
返回应用配置。

---
<a name="get_application_name"></a>
### get_application_name()
返回应用名称。

---
<a name="get_gateway_product_sn"></a>
### get_gateway_product_sn()
返回应用网关ProductSN。

---
<a name="get_gateway_device_sn"></a>
### get_gateway_device_sn()
返回应用网关DeviceSN。

---
<a name="publish"></a>
### publish()
上报消息到Link IoT Edge。参数(topic: str, payload: bytes)

* topic`str`: 上报消息到Link IoT Edge的mqtt topic。
* payload`bytes`: 上报消息到Link IoT Edge的消息内容

---
<a name="register_callback"></a>
### register_callback
设置应用接收消息的回调函数，参数(cb, rrpc_cb)

* cb`func`: 应用消息回调，例如：` def callback(topic:str, msg:bytes): print(str(msg,'utf-8')`
* rrpc_cb`func`: 应用RRPC消息回调，例如：` def callback(topic:str, msg:bytes): print(str(msg,'utf-8')`

## Demo
```
import json
import logging
import time

from iotedgeapplicationlinksdk import getLogger
from iotedgeapplicationlinksdk.client import get_application_config, get_application_name, get_gateway_product_sn, get_gateway_device_sn, publish, register_callback

# 配置log
log = getLogger()
log.setLevel(logging.DEBUG)

# 主函数
if __name__ == "__main__":
    try:
        # 获取应用配置信息
        appConfig = get_application_config()
        log.info('app config:{}'.format(appConfig))

        # 从应用配置获取设备数据上报周期
        uploadPeriod = 5
        if "period" in appConfig.keys() and isinstance(appConfig['period'], int):
            uploadPeriod = int(appConfig['period'])

        appName = get_application_name()
        productSN = get_gateway_product_sn()
        deviceSN = get_gateway_device_sn()
        log.info('app info:{}, {}, {}'.format(appName, productSN, deviceSN))

        # 设置应用上报topic
        topic = '/{}/{}/upload'.format(productSN, deviceSN)

        # 设置应用消息回调
        def callback(topic: str, payload: bytes):
            log.info("recv message from {} : {}".format(topic, str(payload)))

        # 设置应用RRPC消息回调
        def rrpc_callback(topic: str, payload: bytes):
            log.info("recv rrpc message from {} : {}".format(
                topic, str(payload)))

        register_callback(callback, rrpc_callback)
        i = 0
        # 周期上报消息
        while True:
            relayStatus = ("on", "off")[i % 2 == 0]
            payload = {
                "timestamp": time.time(),
                'RelayStatus': relayStatus
            }

            byts = json.dumps(payload).encode('utf-8')

            publish(topic, byts)

            log.info("upload {} : {}".format(topic, str(byts)))

            time.sleep(uploadPeriod)
            i = i+1
            if (i > 1000):
                i = 1

    except Exception as e:
        log.error('load app config error: {}'.format(str(e)))
        exit(1)

```
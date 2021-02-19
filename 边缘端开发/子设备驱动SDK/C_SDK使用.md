# C SDK使用

本文介绍子设备驱动C SDK的使用（模拟继电器开关状态的变化上报），用户可以根据提供的API接口以及子设备的协议编写子设备数据上行及下行逻辑。

## 使用说明

- 编译使用`cmake 2.8`、`make`
- 编译最后生成可执行文件命名为`main`，且打包为zip包，zip包文件名没有要求
- 编译如果依赖动态链接库，需要一并打包到zip包，并确保`main`能正确找到该`*.so`路径
- 本文档所有路径基于git仓库根目录

## 驱动SDK使用流程

**下载C SDK**

```bash
git clone https://github.com/ucloud/iotstack-driver-sdk-c.git
```

**编写`main.c`文件**

本例给出子设备驱动实现基本功能，省略错误处理，详细用户可以参考`./samples/iotstack_driver_test.c`。

子设备驱动的基本功能，包括4部分：

- 初始化子设备驱动
- 解析驱动配置信息，及子设备配置信息
- 创建子设备对象，并配置下行消息回调
- 子设备上线`login`
- 发布消息

```c
/* 注：本部分代码，只做核心代码展示，省略错误处理、内存释放等代码，
   用户使用，请参考文件 ./samples/iotstack_driver_test.c */

// 定义下行消息callback接口
static void edge_normal_msg_handler_user(char *topic, char *payload)
{
    log_write(LOG_INFO, "topic:%s payload:%s", topic, payload);
    /*
    增加逻辑，处理云端到子设备驱动端的下行消息
    */

}

//rrpc消息处理接口
static void edge_rrpc_msg_handler_user(char *topic, char *payload)
{
    log_write(LOG_INFO, "rrpc topic:%s payload:%s", topic, payload);
    /*
    增加逻辑，处理云端到子设备驱动端的rrpc消息
    */
	
    //将处理完成的payload填入edge_rrpc_response的第二个入参，作为rrpc的执行结果返回云端
    edge_rrpc_response(topic, "rrpc response message!");
    return;
}

int main(int argc, char **argv)
{
    edge_status         status     = EDGE_OK;
    subdev_client *     subdevClient = NULL;
    cJSON *dirver_info_cfg = NULL;
    cJSON *device_list_cfg = NULL;
    int upload_period = 5;
    char *topic_str = NULL;
    struct timeval stamp;
    char *time_stamp = NULL;
    const char *switch_str[2] = {"on", "off"};
    int loop = 0;

    topic_str = (char *)malloc(512);
    time_stamp = (char *)malloc(512);

    // 1. SDK资源初始化
    status = edge_common_init();

    // 2.1 获取并解析驱动配置
    /*
    {
        "period": 10,
        "msg_config": {
            "topic": "/%s/%s/upload",
            "param_name": "relay_status"
        }
    }
    */
    dirver_info_cfg = cJSON_Parse(edge_get_driver_info()); 

    // 解析出配置文件中的配置信息
    cJSON *period = cJSON_GetObjectItem(dirver_info_cfg, "period");
    cJSON *config_item = cJSON_GetObjectItem(dirver_info_cfg, "msg_config");
    cJSON *topic_format_json = cJSON_GetObjectItem(config_item, "topic");
    cJSON *topic_param_name_json = cJSON_GetObjectItem(config_item, "param_name");

    // 2.2 获取子设备列表及配置
    device_list_cfg = cJSON_Parse(edge_get_device_info());

    // 判断是否绑定子设备，并获取设备列表的productsn和devicesn，将设备上线
    if(cJSON_GetArraySize(device_list_cfg) > 0)
    {
        // 解析出子设备列表信息
        cJSON *arrary_item = cJSON_GetArrayItem(device_list_cfg, 0);
        cJSON *arrary_productsn = cJSON_GetObjectItem(arrary_item, "productSN");
        cJSON *arrary_devicesn = cJSON_GetObjectItem(arrary_item, "deviceSN");

        // 根据配置文件，拼装发布消息Topic
        memset(topic_str, 0, 512);
        snprintf(topic_str, 512, "/%s/%s/upload", arrary_productsn->valuestring, arrary_devicesn->valuestring);

        // 3. 初始化一个子设备，并传入回调函数接口
        subdevClient = edge_subdev_construct(arrary_productsn->valuestring, arrary_devicesn->valuestring, edge_normal_msg_handler_user, edge_rrpc_msg_handler_user);

        // 4. 子设备登录
        status = edge_subdev_login_async(subdevClient);
    }

    while(1)
    {
        // 维持心跳,并周期发送消息
        sleep(upload_period);

        gettimeofday(&stamp, NULL);
        memset(time_stamp, 0, 512);
        snprintf(time_stamp, 512, "{\"timestamp\": \"%ld\", \"%s\": \"%s\"}", stamp.tv_sec, topic_param_name_json->valuestring, switch_str[loop++ % 2]);

        // 5. 发布消息
        /*
        {
            "timestamp": 1589463296,
            "relay_status": "on"/"off
        }
        */
        status = edge_publish(topic_str, time_stamp);
    }
}
```

**编译**

```bash
// 更改路径到SDK根目录
make
// 生成可执行文件
ls samples/iotstack_driver_test
```

**打包驱动**

```bash
#打包驱动SDK，如有动态链接库，需要一起打包
mv iotstack_driver_test main
zip -r driver.zip main
```

**上传驱动zip压缩包到驱动管理**

**分配驱动到网关设备**

**进行测试**

配置路由后，通过日志模块查看上下行消息内容。

## 驱动API介绍

### edge_common_init

```c
edge_status edge_common_init(void)
```

C SDK驱动初始化，该函数必须要调用。

**输入参数**

无

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_get_driver_info

```c
char * edge_get_driver_info(void)
```

获取驱动配置信息。

**输入参数**

无

**返回值**

返回驱动配置json字符串。

### edge_get_device_info

```c
char * edge_get_device_info(void)
```

获取子设备列表及子设备配置。

**输入参数**

无

**返回值**

返回子设备列表及子设备配置json字符串，字符串示例如下：

```json
{
    "deviceList": [{
        "productSN": "xxxxxx",
        "deviceSN": "yyyyy",
        "config": {}
    }]
}
```

### edge_get_product_sn

```c
char * edge_get_product_sn(void)
```

获取网关的产品序列号。

**输入参数**

无

**返回值**

返回产品序列号字符串

### edge_get_device_sn

```c
char * edge_get_device_sn(void)
```

获取网关的设备序列号。

**输入参数**

无

**返回值**

返回设备序列号字符串

### edge_subdev_construct

```c
subdev_client * edge_subdev_construct(const char *product_sn, const char *device_sn, edge_normal_msg_handler normal_msg_handle, edge_rrpc_msg_handler rrpc_msg_handle)
```

创建一个子设备句柄。

**输入参数**

| Parameter name    | Type                    | Description                |
| ----------------- | ----------------------- | -------------------------- |
| product_sn        | const char *            | 子设备产品序列号           |
| device_sn         | const char *            | 子设备设备序列号           |
| normal_msg_handle | edge_normal_msg_handler | 子设备的接收消息的回调函数 |
| rrpc_msg_handle   | edge_rrpc_msg_handler   | 子设备接收的rrpc消息的回调函数 |
**返回值**

- 成功 - 返回subdev_client指针
- 失败 - 返回NULL

### edge_publish

```c
edge_status edge_publish(const char *topic, const char *str)
```

向指定topic发布消息。

**输入参数**

| Parameter name | Type         | Description       |
| -------------- | ------------ | ----------------- |
| topic          | const char * | 发送消息的Topic   |
| str            | const char * | 发送消息的Payload |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_subdev_dynamic_auth

```c
edge_status edge_subdev_dynamic_auth(subdev_client *pst_subdev_client, const char *product_secret, uint32_t time_out_ms)
```

动态注册子设备，动态注册原理参考动态注册。

**输入参数**

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| product_secret    | const char *  | 子设备产品密钥                      |
| time_out_ms       | uint32_t      | 等待注册成功reply超时时间，单位毫秒 |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_subdev_login_sync

```c
edge_status edge_subdev_login_sync(subdev_client *pst_subdev_client, uint32_t time_out_ms)
```

子设备上线操作，同步接口。

**输入参数**

| Parameter name    | Type          | Description                        |
| ----------------- | ------------- | ---------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                         |
| time_out_ms       | uint32_t      | 同步时使用，等待超时时间，单位毫秒 |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_subdev_logout_sync

```c
edge_status edge_subdev_logout_sync(subdev_client *pst_subdev_client, uint32_t time_out_ms)
```

子设备下线操作，同步接口。

**输入参数**

| Parameter name    | Type          | Description                        |
| ----------------- | ------------- | ---------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                         |
| time_out_ms       | uint32_t      | 同步时使用，等待超时时间，单位毫秒 |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_subdev_login_async

```c
edge_status edge_subdev_login_async(subdev_client *pst_subdev_client)
```

子设备上线操作，异步接口，不关心成功与否。

**输入参数**

| Parameter name    | Type          | Description |
| ----------------- | ------------- | ----------- |
| pst_subdev_client | subdev_client | 子设备句柄  |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_subdev_logout_async

```c
edge_status edge_subdev_logout_async(subdev_client *pst_subdev_client)
```

子设备下线操作，异步接口，不关心成功与否。

**输入参数**

| Parameter name    | Type          | Description |
| ----------------- | ------------- | ----------- |
| pst_subdev_client | subdev_client | 子设备句柄  |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_set_topo_notify_handle

```c
void edge_set_topo_notify_handle(edge_topo_notify_handler topo_notify_handle)
```

设置子设备绑定关系发生改变后的回调函数。

**输入参数**

| Parameter name     | Type                     | Description              |
| ------------------ | ------------------------ | ------------------------ |
| topo_notify_handle | edge_topo_notify_handler | 绑定关系发生改变回调函数 |

**回调函数：**

```c
void (*edge_topo_notify_handler)(topo_operation opera, char *payload)
```

**返回值**

无

### edge_set_subdev_status_handle

```c
void edge_set_subdev_status_handle(edge_subdev_status_handler subdev_status_handle)
```

设置子设备启用/禁用的回调函数。

**输入参数**

| Parameter name       | Type                       | Description              |
| -------------------- | -------------------------- | ------------------------ |
| subdev_status_handle | edge_subdev_status_handler | 绑定关系发生改变回调函数 |

回调函数：

```c
void (*edge_subdev_status_handler)(subdev_able opera,char *payload)
```

**返回值**

无

### edge_get_online_status

```c
bool edge_get_online_status(void)
```

获取当前边缘网关的在线状态。

**输入参数**

 无

**返回值**

 布尔值

### edge_get_topo

```c
char *edge_get_topo(uint32_t time_out_ms)
```

获取当前网关下的拓扑绑定关系。

**输入参数**

| Parameter name | Type     | Description                            |
| -------------- | -------- | -------------------------------------- |
| time_out_ms    | uint32_t | 等待获取拓扑绑定关系超时时间，单位毫秒 |

**返回值**

返回topo关系json字符串，格式如下：

```json
{
    "RequestID": "123",
    "RetCode": 0,
    "Data": [{
        "ProductSN": "product1234",
        "DeviceSN": "device1234"
    }]
}
```

### edge_add_topo

```c
edge_status edge_add_topo(subdev_client *pst_subdev_client, uint32_t time_out_ms)
```

添加拓扑绑定关系。

**输入参数**

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| time_out_ms       | uint32_t      | 等待添加成功reply超时时间，单位毫秒 |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### edge_delete_topo

```c
edge_status edge_delete_topo(subdev_client *pst_subdev_client, uint32_t time_out_ms)
```

删除拓扑绑定关系。

**输入参数**

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| time_out_ms       | uint32_t      | 等待添加成功reply超时时间，单位毫秒 |

**返回值**

- 成功 - 返回EDGE_OK
- 失败 - 返回类型参考：edge_status枚举

### log_write

```c
void log_write(log_level level, const char *format,...)
```

记录日志。

**输入参数**

| Parameter name | Type         | Description                                         |
| -------------- | ------------ | --------------------------------------------------- |
| level          | log_level    | 日志等级分为：DEBUG、INFO、WARNING、ERROR、CRITICAL |
| format         | const char * | 日志记录格式                                        |

**返回值**

- 无


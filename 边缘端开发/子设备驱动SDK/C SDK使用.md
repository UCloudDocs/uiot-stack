# C SDK使用

本文介绍子设备驱动C SDK的使用（模拟继电器开关状态的变化上报），用户可以根据提供的API接口以及子设备的协议编写子设备数据上行及下行逻辑。

## 使用说明

- 编译使用`cmake 2.8`、`make`
- 编译最后生成可执行文件命名为`main`，且打包为zip包，zip包文件名没有要求
- 编译如果依赖动态链接库，需要一并打包到zip包，并确保`main`能正确找到该`*.so`路径
- 本文档所有路径基于git仓库根目录

## 驱动SDK使用流程

1. 下载C SDK

```bash
git clone https://github.com/ucloud/uiotedge-driver-sdk-c.gitCopyErrorSuccess
```

1. 编写`main.c`文件

本例给出子设备驱动实现基本功能，省略错误处理，详细用户可以参考`./samples/uiot_edge_test.c`。

子设备驱动的基本功能，包括4部分：

- 初始化子设备驱动
- 解析驱动配置信息，及子设备配置信息
- 创建子设备对象，并配置下行消息回调
- 子设备上线`login`
- 发布消息

```c
/* 注：本部分代码，只做核心代码展示，省略错误处理、内存释放等代码，
   用户使用，请参考文件 ./samples/uiot_edge_test.c */

// 定义下行消息callback接口
static void edge_normal_msg_handler_user(char *topic, char *payload)
{
    log_write(LOG_INFO, "topic:%s payload:%s", topic, payload);
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

    // 1. SDK初始化
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
        subdevClient = edge_subdev_construct(arrary_productsn->valuestring, arrary_devicesn->valuestring, edge_normal_msg_handler_user);

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
}CopyErrorSuccess
```

1. 编译

```bash
// 更改路径到SDK根目录
make
// 生成可执行文件
ls samples/uiot_edge_testCopyErrorSuccess
```

1. 打包驱动

   ```bash
   #打包驱动SDK，如有动态链接库，需要一起打包
   mv uiot_edge_test main
   zip -r driver.zip mainCopyErrorSuccess
   ```

2. 上传驱动zip压缩包到驱动管理

3. 分配驱动到网关设备

4. 进行测试

   配置路由后，通过日志模块查看上下行消息内容。

## 驱动API介绍

### edge_common_init

```c
edge_status edge_common_init(void)CopyErrorSuccess
```

C SDK驱动初始化，该函数必须要调用。

- 输入参数

  无

- 返回值

  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_get_driver_info

```c
char * edge_get_driver_info(void)CopyErrorSuccess
```

获取驱动配置信息。

- 输入参数

  无

- 返回值

  返回驱动配置json字符串。

### edge_get_device_info

```c
char * edge_get_device_info(void)CopyErrorSuccess
```

获取子设备列表及子设备配置。

- 输入参数

  无

- 返回值

  返回子设备列表及子设备配置json字符串，字符串示例如下：

  ```json
  {
      "deviceList": [{
          "productSN": "xxxxxx",
          "deviceSN": "yyyyy",
          "config": {}
      }]
  }CopyErrorSuccess
  ```

### edge_get_product_sn

```c
char * edge_get_product_sn(void)CopyErrorSuccess
```

获取网关的产品序列号。

- 输入参数

  无

- 返回值

  返回产品序列号字符串

### edge_get_device_sn

```c
char * edge_get_device_sn(void)CopyErrorSuccess
```

获取网关的设备序列号。

- 输入参数

  无

- 返回值

  返回设备序列号字符串

### edge_subdev_construct

```c
subdev_client * edge_subdev_construct(const char *product_sn, const char *device_sn, edge_normal_msg_handler normal_msg_handle)CopyErrorSuccess
```

创建一个子设备句柄。

- 输入参数

| Parameter name    | Type                    | Description                |
| ----------------- | ----------------------- | -------------------------- |
| product_sn        | const char *            | 子设备产品序列号           |
| device_sn         | const char *            | 子设备设备序列号           |
| normal_msg_handle | edge_normal_msg_handler | 子设备的接收消息的回调函数 |

- 返回值
  - 成功 - 返回subdev_client指针
  - 失败 - 返回NULL

### edge_publish

```c
edge_status edge_publish(const char *topic, const char *str)CopyErrorSuccess
```

向指定topic发布消息。

- 输入参数

| Parameter name | Type         | Description       |
| -------------- | ------------ | ----------------- |
| topic          | const char * | 发送消息的Topic   |
| str            | const char * | 发送消息的Payload |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_subdev_dynamic_auth

```c
edge_status edge_subdev_dynamic_auth(subdev_client *pst_subdev_client, const char *product_secret, uint32_t time_out_ms)CopyErrorSuccess
```

动态注册子设备，动态注册原理参考动态注册。

- 输入参数

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| product_secret    | const char *  | 子设备产品密钥                      |
| time_out_ms       | uint32_t      | 等待注册成功reply超时时间，单位毫秒 |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_subdev_login_sync

```c
edge_status edge_subdev_login_sync(subdev_client *pst_subdev_client, uint32_t time_out_ms)CopyErrorSuccess
```

子设备上线操作，同步接口。

- 输入参数

| Parameter name    | Type          | Description                        |
| ----------------- | ------------- | ---------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                         |
| time_out_ms       | uint32_t      | 同步时使用，等待超时时间，单位毫秒 |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_subdev_logout_sync

```c
edge_status edge_subdev_logout_sync(subdev_client *pst_subdev_client, uint32_t time_out_ms)CopyErrorSuccess
```

子设备下线操作，同步接口。

- 输入参数

| Parameter name    | Type          | Description                        |
| ----------------- | ------------- | ---------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                         |
| time_out_ms       | uint32_t      | 同步时使用，等待超时时间，单位毫秒 |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_subdev_login_async

```c
edge_status edge_subdev_login_async(subdev_client *pst_subdev_client)CopyErrorSuccess
```

子设备上线操作，异步接口，不关心成功与否。

- 输入参数

| Parameter name    | Type          | Description |
| ----------------- | ------------- | ----------- |
| pst_subdev_client | subdev_client | 子设备句柄  |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_subdev_logout_async

```c
edge_status edge_subdev_logout_async(subdev_client *pst_subdev_client)CopyErrorSuccess
```

子设备下线操作，异步接口，不关心成功与否。

- 输入参数

| Parameter name    | Type          | Description |
| ----------------- | ------------- | ----------- |
| pst_subdev_client | subdev_client | 子设备句柄  |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_set_topo_notify_handle

```c
void edge_set_topo_notify_handle(edge_topo_notify_handler topo_notify_handle)CopyErrorSuccess
```

设置子设备绑定关系发生改变后的回调函数。

- 输入参数

| Parameter name     | Type                     | Description              |
| ------------------ | ------------------------ | ------------------------ |
| topo_notify_handle | edge_topo_notify_handler | 绑定关系发生改变回调函数 |

回调函数：

```c
void (*edge_topo_notify_handler)(topo_operation opera, char *payload)CopyErrorSuccess
```

- 返回值
  - 无

### edge_set_subdev_status_handle

```c
void edge_set_subdev_status_handle(edge_subdev_status_handler subdev_status_handle)CopyErrorSuccess
```

设置子设备启用/禁用的回调函数。

- 输入参数

| Parameter name       | Type                       | Description              |
| -------------------- | -------------------------- | ------------------------ |
| subdev_status_handle | edge_subdev_status_handler | 绑定关系发生改变回调函数 |

回调函数：

```c
void (*edge_subdev_status_handler)(subdev_able opera,char *payload)CopyErrorSuccess
```

- 返回值
  - 无

### edge_get_online_status

```c
bool edge_get_online_status(void)CopyErrorSuccess
```

获取当前边缘网关的在线状态。

- 输入参数 无
- 返回值 布尔值。

### edge_get_topo

```c
char *edge_get_topo(uint32_t time_out_ms)CopyErrorSuccess
```

获取当前网关下的拓扑绑定关系。

- 输入参数

| Parameter name | Type     | Description                            |
| -------------- | -------- | -------------------------------------- |
| time_out_ms    | uint32_t | 等待获取拓扑绑定关系超时时间，单位毫秒 |

- 返回值 返回topo关系json字符串，格式如下：

  ```json
  {
      "RequestID": "123",
      "RetCode": 0,
      "Data": [{
          "ProductSN": "product1234",
          "DeviceSN": "device1234"
      }]
  }CopyErrorSuccess
  ```

### edge_add_topo

```c
edge_status edge_add_topo(subdev_client *pst_subdev_client, uint32_t time_out_ms)CopyErrorSuccess
```

添加拓扑绑定关系。

- 输入参数

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| time_out_ms       | uint32_t      | 等待添加成功reply超时时间，单位毫秒 |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_delete_topo

```c
edge_status edge_delete_topo(subdev_client *pst_subdev_client, uint32_t time_out_ms)CopyErrorSuccess
```

删除拓扑绑定关系。

- 输入参数

| Parameter name    | Type          | Description                         |
| ----------------- | ------------- | ----------------------------------- |
| pst_subdev_client | subdev_client | 子设备句柄                          |
| time_out_ms       | uint32_t      | 等待添加成功reply超时时间，单位毫秒 |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### edge_set_log

```c
edge_status edge_set_log(log_level level, uint32_t file_size_mb, uint32_t file_number)CopyErrorSuccess
```

设置日志记录等级，日志文件大小和文件个数（一天一个日志文件）。

- 输入参数

| Parameter name | Type      | Description                                         |
| -------------- | --------- | --------------------------------------------------- |
| level          | log_level | 日志等级分为：DEBUG、INFO、WARNING、ERROR、CRITICAL |
| file_size_mb   | uint32_t  | 单个日志文件大小                                    |
| file_number    | uint32_t  | 日志文件个数                                        |

- 返回值
  - 成功 - 返回EDGE_OK
  - 失败 - 返回类型参考：edge_status枚举

### log_write

```c
void log_write(log_level level, const char *format,...)CopyErrorSuccess
```

记录日志。

- 输入参数

| Parameter name | Type         | Description                                         |
| -------------- | ------------ | --------------------------------------------------- |
| level          | log_level    | 日志等级分为：DEBUG、INFO、WARNING、ERROR、CRITICAL |
| format         | const char * | 日志记录格式                                        |

- 返回值
  - 无


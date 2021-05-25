# 设备固件升级（OTA）开发

IoT 平台支持设备通过 OTA(Over-the-Air Technology) 进行固件升级。本章描述如何利用设备端 C-SDK 提供的API进行设备端 OTA 功能开发。下面的讲解对应于 C-SDK 示例代码 samples/ota/ota_sample.c 。

## 功能说明

- C-SDK 提供支持固件下载及校验的 API，但固件存储以及烧录需要用户在应用程序中实现。

- 建议在 OTA 功能设计之初预留充足的存储容量以便存放固件，同时充分考虑到烧录的风险，如有必要提前设计烧录失败的回退逻辑。

## 开发步骤

**准备**

1. 在控制台创建一个产品和一个设备，替换 ota_sample.c 中的设备四元组信息。

2. 上传一个固件（模块信息版本号任选，与示例代码中的 "1.0.0" 不同即可）。

3. 在示例代码启动并上报版本之后，在控制台发起设备升级操作，将设备升级到上传的固件版本。

**初始化**

在使用 OTA 功能之前，首先需要进行初始化，包括 MQTT 客户端的创建、OTA 的初始化以及版本上报。可参照 ota_sample.c 中 main 函数的初始化部分代码。

```
//1. 首先创建MQTT客户端，并与云端建立MQTT连接
void *client = IOT_MQTT_Construct(&init_params);
if (client != NULL) {
    LOG_INFO("MQTT Construct Success");
} else {
    LOG_ERROR("MQTT Construct Failed");
    return FAILURE;
}

//2. OTA 初始化，获取句柄
void *h_ota = IOT_OTA_Init(UIOT_MY_PRODUCT_SN, UIOT_MY_DEVICE_SN, client);
if (NULL == h_ota) {
    LOG_ERROR("init OTA failed");
    return FAILURE;
}

//3. 上报初始版本，上报时需上报固件模块、版本号；未上报版本的设备无法升级
if (IOT_OTA_ReportVersion(h_ota,"default","1.0.0") < 0) {
    LOG_ERROR("report OTA version failed");
    return FAILURE;
}
```

**下载及升级流程**

流程包括固件的下载、校验、存储和烧录等步骤。由于硬件平台等差异，用户可根据实际情况仿照示例代码实现整个流程。

```
    //初始化下载分区
    download_handle = HAL_Download_Init(h_ota->download_name);
    if(download_handle == NULL)
    {
        ret = FAILURE_RET;
        goto __exit;
    }
	
    buffer_read = (char *)HAL_Malloc(HTTP_OTA_BUFF_LEN);
    if (buffer_read == NULL)
    {
        LOG_ERROR("No memory for http ota!");
        ret = FAILURE_RET;
        goto __exit;
    }
    memset(buffer_read, 0x00, HTTP_OTA_BUFF_LEN);
	
    //向云端上报开始下载
	IOT_OTA_ReportUpgrading(h_ota);

    LOG_INFO("OTA file size is (%d)", file_size);
    do
    {
        //获取升级文件内容，并写入下载分区
        length = IOT_OTA_FetchYield(h_ota, buffer_read, HTTP_OTA_BUFF_LEN, HTTP_OTA_RANGE_LEN, 10);
        if (length > 0)
        {
            /* Write the data to the corresponding partition address */
            if(HAL_Download_Write(download_handle, total_length, buffer_read, length) == FAILURE_RET){
                ret = FAILURE_RET;
                goto __exit;
            }

            total_length += length;
            //wait cancel cmd
            IOT_OTA_Yield(handle, 100);
        }
        else
        {
            LOG_ERROR("Exit: server return err (%d)!", length);
            ret = ERR_OTA_FETCH_FAILED;                
            goto __exit;
        }
    } while (!IOT_OTA_IsFetchFinish(h_ota));

    //下载完成，向云端上报下载成功
    if (total_length == file_size)
    {    
        ret = SUCCESS_RET;
        IOT_OTA_Ioctl(h_ota, OTA_IOCTL_CHECK_FIRMWARE, &firmware_valid, 4);
        if (0 == firmware_valid) {
            LOG_ERROR("The firmware is invalid"); 
            ret = IOT_OTA_GetLastError(h_ota);
            goto __exit;
        } else {
            LOG_INFO("The firmware is valid");            
            IOT_OTA_ReportSuccess(h_ota);
        }
        
        if(HAL_Download_End(download_handle))
            ret = FAILURE_RET;

        LOG_INFO("Download firmware to flash success.");
    }
```

**资源释放**

固件升级完成后，需要释放 OTA 过程中使用的资源。

```
//关闭文件
fclose(fp);
//释放OTA资源
IOT_OTA_Destroy(h_ota);
//（可选）释放 MQTT Client 资源
IOT_MQTT_Destroy(&client);
```

## API 列表

### IOT_OTA_Init

初始化 OTA 模块

```
void *IOT_OTA_Init(const char *product_sn, const char *device_sn, void *ch_signal);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| product_sn | const char * | 输入 | 指向产品序列号的指针 |
| device_sn | const char * | 输入 | 指向设备序列号的指针 |
| ch_signal | void * | 输入 | 指定的信号通道，目前为 IOT_MQTT_Construct 返回的句柄 |
| ret | void * | 返回值 | 初始化成功，返回句柄；初始化失败，返回 NULL |

### IOT_OTA_Destroy

释放 OTA 相关的资源

```
int IOT_OTA_Destroy(void *handle);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| ret | int | 返回值 | <0: 失败  =0: 成功 |

### IOT_OTA_ReportVersion

向 OTA 服务器报告固件版本信息

```
int IOT_OTA_ReportVersion(void *handle, const char *module, const char *version);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| module | const char * | 输入 | 以字符串格式指定模块名 |
| version | const char * | 输入 | 以字符串格式指定固件版本 |
| ret | int | 返回值 | <0: 上报失败  >0: 成功，返回对应 publish 的 packet id |

### IOT_OTA_ReportSuccess

向 OTA 服务器上报升级成功

```
int IOT_OTA_ReportSuccess(void *handle);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| ret | int | 返回值 | <0: 上报失败  >0: 成功，返回对应 publish 的 packet id |

### IOT_OTA_ReportFail

向 OTA 服务器上报失败信息

```
int IOT_OTA_ReportFail(void *handle, IOT_OTA_ReportErrCode err_code);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| err_code | IOT_OTA_ReportErrCode | 输入 | 错误码 |
| ret | int | 返回值 | <0: 上报失败  >0: 成功，返回对应 publish 的 packet id |

### IOT_OTA_IsFetching

检查是否处于下载固件的状态

```
int IOT_OTA_IsFetching(void *handle);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| ret | int | 返回值 | 0: No  1: Yes |

### IOT_OTA_IsFetchFinish

检查固件是否已经下载完成

```
int IOT_OTA_IsFetchFinish(void *handle);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| ret | int | 返回值 | 0: No  1: Yes |

### IOT_OTA_FetchYield

从远程服务器获取固件

```
int IOT_OTA_FetchYield(void *handle, char *buf, size_t buf_len, uint32_t timeout_s);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| buf | char * | 输出 | 指定存储固件数据的缓冲区 |
| buf_len | size_t | 输入 | buf 的长度 |
| timeout_s | uint32_t | 输入 | 超时时间 |
| ret | int | 返回值 | <0: 对应的错误码  0: 在 timeout_s 超时期间内没有任何数据被下载 (0, len] : 在 timeout_s 超时时间内下载数据的长度 |

### IOT_OTA_Ioctl

获取指定的 OTA 信息

```
int IOT_OTA_Ioctl(void *handle, IOT_OTA_CmdType type, void *buf, size_t buf_len);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| type | IOT_OTA_CmdType | 输入 | 指定想要的信息 |
| buf | void * | 输出 | 为数据交换指定缓冲区 |
| buf_len | size_t | 输入 | buf 的长度 |
| ret | int | 返回值 | <0: 失败  0: 成功 |

**说明**

* 如果 type==OTA_IOCTL_FETCHED_SIZE, 'buf' 需要传入 uint32_t 类型指针, 'buf_len' 需指定为 4

* 如果 type==OTA_IOCTL_FILE_SIZE, 'buf' 需要传入 uint32_t 类型指针, 'buf_len' 需指定为 4

* 如果 type==OTA_IOCTL_MD5SUM, 'buf' 需要传入 buffer, 'buf_len' 需指定为 33

* 如果 type==OTA_IOCTL_VERSION, 'buf' 需要传入 buffer, 'buf_len' 需指定为 OTA_VERSION_LEN_MAX

* 如果 type==OTA_IOCTL_CHECK_FIRMWARE, 'buf' 需要传入 uint32_t 类型指针, 'buf_len'需指定为 4 。返回：0, 固件MD5校验不通过, 固件无效; 1, 固件有效

### IOT_OTA_GetLastError

获取最后一个错误码

```
int IOT_OTA_GetLastError(void *handle);
```

**参数列表**

| 参数 | 数据类型 | 参数类型 | 说明 |
| --- | --- | --- | --- |
| handle | void * | 输入 | IOT_OTA_Init 返回的句柄 |
| ret | int | 返回值 | 对应错误的错误码 |


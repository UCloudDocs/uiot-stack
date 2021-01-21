# 本地应用C-SDK使用

本文介绍应用SDK的使用，用户可以根据提供的API接口编写数据上行及下行逻辑。

## 使用说明

* 编译使用cmake 2.8、make
* 编译最后生成可执行文件命名为main，且打包为zip包，zip包文件名没有要求
* 编译如果依赖动态链接库，需要一并打包到zip包，并确保main能正确找到该*.so路径

本文档所有路径基于git仓库根目录
## 驱动SDK使用流程

**下载应用C SDK**
```
git clone https://github.com/ucloud/application-sdk-c.git
```
**编写main.c文件**

本例给出实现基本功能，省略错误处理，详细用户可以参考samples/samples.c。

应用的基本功能，包括4部分：

* 初始化应用
* 解析驱动配置信息，及应用配置信息
* 创建应用，并配置下行消息处理回调函数
* 发布消息
```
/* 注：本部分代码，只做核心代码展示，省略错误处理、内存释放等代码，
   用户使用，请参考文件 samples/samples.c */

// 定义下行消息callback接口
void recvmsg_handler(char *topic, char *payload)
{
    //打印接收到的下次消息内容
    log_write(LOG_INFO, "receive topic:%s",topic);
    log_write(LOG_INFO, "receive payload:%s",payload);
    /*
    	在此处编写下行消息处理逻辑
    */
    return;
}

int main(int argc, char **argv)
{
    app_status status = APP_OK;    
    char topic_str[100] = {0};    
    struct timeval stamp;
    char time_stamp[100] = {0};
    cJSON *app_info = NULL;
    //初始化，获取产品、设备SN号信息
    status = app_common_init();
    if(APP_OK != status)
    {
        log_write(LOG_ERROR, "app_common_init fail");
        goto end;
    }

    log_write(LOG_INFO, "productSN:%s, deviceSN:%s",app_get_productSN(),app_get_deviceSN());
    log_write(LOG_INFO, "app info:%s",app_get_info());

    //注册回调函数，回调函数处理接收到的下行消息
    status = app_register_cb(recvmsg_handler);
    if(APP_OK != status)
    {
        log_write(LOG_ERROR, "app_register_cb fail");
        goto end;
    }

    //获取应用配置信息，此处解析出上报消息的topic名称
    app_info = cJSON_Parse(app_get_info());
    if (!app_info) 
    {
        log_write(LOG_ERROR, "parse app info fail");
        goto end;
    }
    /*
        "topic":"/%s/%s/upload"
    */
    cJSON *topic_format = cJSON_GetObjectItem(app_info, "topic");

    snprintf(topic_str, 100, topic_format->valuestring, app_get_productSN(), app_get_deviceSN());
    while(1)
    {    
        //每隔5秒上报当前时间
        sleep(5);
        gettimeofday(&stamp, NULL);
        memset(time_stamp, 0, 100);
        snprintf(time_stamp, 100, "{\"timestamp\": \"%ld\"}", stamp.tv_sec);
        log_write(LOG_INFO, "send message[%s]", time_stamp);
        
        status = app_publish(topic_str, time_stamp);
        if(APP_OK!= status)
        {
            log_write(LOG_ERROR, "app_publish fail");
            goto end;
        }
    }

end:    
    cJSON_Delete(app_info);
    return status;
}
```

### 编译
// 更改路径到SDK根目录
```
make
```
// 生成可执行文件
**打包应用SDK，如有动态链接库，需要一起打包**
```
mv samples main
zip -r driver.zip main
```

**上传应用zip压缩包到驱动管理**

**分配应用到网关设备**

**进行测试**

**配置路由后，通过日志模块查看上下行消息内容**


## 应用SDK API
**应用sdk相关资源初始化**
```
app_status app_common_init(void)
```
入参：无
出参：执行结果，成功返回APP_OK

**获取应用名称**
```
char *app_get_name(void)
```
入参：无
出参：应用名称的字符串

**获取产品SN号（网关）**
```
char *app_get_productSN(void)
```
入参：无
出参：返回产品SN的字符串

**获取设备SN号（网关）**
```
char *app_get_deviceSN(void)
```
入参：无
出参：返回设备SN号的字符串

**获取应用信息**
```
char *app_get_info(void)
```
入参：无
出参：返回应用信息的json字符串

**注册云平台到应用的下行消息处理回调函数**
```
app_status edge_status app_register_cb(msg_handler handle)
```
入参：消息处理函数指针（void (*msg_handler)(char *topic, char *payload)）
出参：执行结果，成功返回APP_OK
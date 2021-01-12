# Topic管理

### 创建Topic

```http
POST /api/v1/topic/create HTTP/1.1
Host: localhost:8080
```

###### 参数说明

|参数|说明|
|---|---|
|Method|请求方法。只支持 POST 方法。|
|URL|`/api/v1/topic/create`，URL 地址|
|Context-Type|application/json|

Post Json 参数

|参数名称|必选|类型|描述|
|---|---|---|---|
|DeviceIdentifier|是|string|Topic中设备身份标识(${DeviceSN},$broadcast)|
|TopicSuffix|是|string|Topic后缀|
|Description|是|sting|Topic描述|
|Permission|是|string|Topic权限|
|ProductSN|是|string|ProductSN|


###### 返回

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|0|
|Data|是|struct|创建成功消息|

###### Data详情

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|uint64|TopicID|
###### 响应示例
```json
{
    "Code": 0,
	"Data": {
		"TopicID": 110
    }
}
```

---

### 获取Topic列表

```http
POST /api/v1/topic/list HTTP/1.1
Host: localhost:8080
```

###### 参数说明

|参数|说明|
|---|---|
|Method|请求方法。只支持 POST 方法。|
|URL|`/api/v1/topic/list`，URL 地址|

###### POST 参数

|参数名称|必选|类型|描述|
|---|---|---|---|
|ProductSN|是|string|产品序列号|
|TopicType|否|string|Topic类型（sys,user）|
|Offset|否|int|偏移量|
|Limit|否|int|分页限制|


###### 返回

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|0|
|Data|是|struct|Topic信息列表|

###### Data详情

|参数名称|必选|类型|描述|
|---|---|---|---|
|Count|是|int|Topic总数|
|TopicList|是|array|Topic列表|


###### TopicList详情

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|uint64|TopicID|
|Topic|是|string|Topic|
|Permission|是|string|规则引擎权限 pub, sub或者pubsub|
|TopicType|是|string|Topic类型(sys,user)|
|TopicSuffix|是|string|Topic后缀|
|RuleEnginePermission|是|string|RuleEngine操作权限 pub, sub或者pubsub|
|DeviceIdentifier|是|string|Topic中设备身份标识(${DeviceSN},$broadcast)|
###### 响应示例
```json
{
    "Code": 0,
    "Data": {
        "Count": 5,
        "TopicList": [
            {
                "TopicID": 0,
                "Topic": "/$system/sc0t2mdmls6mj8ud/${DeviceSN}/device/status",
                "Permission": "-",
                "Description": "()",
                "TopicType": "sys",
                "RuleEnginePermission": "sub",
                "TopicSuffix": "device/status",
                "DeviceIdentifier": "${DeviceSN}"
            },
            {
                "TopicID": 0,
                "Topic": "/$system/sc0t2mdmls6mj8ud/${DeviceSN}/rrpc/request",
                "Permission": "sub",
                "Description": "RRPC Request Topic",
                "TopicType": "sys",
                "RuleEnginePermission": "sub",
                "TopicSuffix": "rrpc/request",
                "DeviceIdentifier": "${DeviceSN}"
            }
        ]
    }
}
```

---

### 修改Topic

```http
POST /api/v1/topic/update HTTP/1.1
Host: localhost:8080
```

###### 参数说明

|参数|说明|
|---|---|
|Method|请求方法。只支持 POST 方法。|
|URL|`/api/v1/topic/update`，URL 地址|
|Context-Type|application/json|

Post Json 参数

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|uint|TopicID|
|DeviceIdentifier|是|string|Topic中设备身份标识(${DeviceSN},$broadcast)|
|TopicSuffix|是|string|Topic后缀|
|Description|否|sting|Topic描述|
|Permission|是|string|Topic权限|


###### 返回

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|0|
###### 响应示例
```json
{
    "Code": 0
}
```
---

### 删除Topic

```http
Post /api/v1/topic/delete HTTP/1.1
Host: localhost:8080
```

###### 参数说明

|参数|说明|
|---|---|
|Method|请求方法。只支持 Post 方法。|
|URL|`/api/v1/topic/delete`，URL 地址|
|Context-Type|application/json|

Post 参数

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|[]uint64|TopicID数组|
|ProductionID|是|uint64|产品ID|


###### 返回

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|0|
###### 响应示例
```json
{
    "Code": 0,
}
```
---

### 获取Topic详情

```http
GET /api/v1/topic/info HTTP/1.1
Host: localhost:8080
```

###### 参数说明

|参数|说明|
|---|---|
|Method|请求方法。只支持 GET 方法。|
|URL|`/api/v1/topic/info`，URL 地址|
|Context-Type|application/json|

Query参数

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|uint64|TopicID|


###### 返回

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|0|
|Data|是|struct|Topic信息|

###### Data详情

|参数名称|必选|类型|描述|
|---|---|---|---|
|TopicID|是|uint64|TopicID|
|ProductID|是|uint64|ProductID|
|Topic|是|string|Topic|
|Permission|是|string|规则引擎权限 pub, sub或者pubsub|
|TopicSuffix|是|string|Topic后缀|
|DeviceIdentifier|是|string|Topic中设备身份标识(${DeviceSN},$broadcast)|
###### 响应示例
```json
{
    "Code": 0,
    "Data": {
        "TopicID": 1,
        "ProductID": 2,
        "DeviceIdentifier": "${DeviceSN}",
        "TopicSuffix": "upload111",
        "Topic": "/8522fvnipy8tic5z/${DeviceSN}/upload111",
        "Permission": "pub"
    }
}
```



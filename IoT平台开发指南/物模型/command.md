## Command API Reference

#### 1, Command Create

```http
POST /api/v1/tmodel/command/create HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                      |
| ------------ | ----------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。              |
| URL          | `/api/v1/tmodel/command/create`，URL 地址 |
| Context-Type | application/json                          |

Post Json 参数

| 参数名称    | 必选 | 类型         | 描述       |
| ----------- | ---- | ------------ | ---------- |
| ProductSN   | 是   | string       | 产品序列号 |
| Name        | 是   | sting        | 名称       |
| Identifier  | 是   | sting        | 标识符     |
| Description | 否   | sting        | 描述       |
| Input       | 否   | []DataStruct | Input      |
| Output      | 否   | []DataStruct | Output     |

###### DataStruct

| 参数名称   | 必选 | 类型               | 描述                                                     |
| ---------- | ---- | ------------------ | -------------------------------------------------------- |
| Name       | 是   | sting              | 名称                                                     |
| Identifier | 是   | sting              | 标识符                                                   |
| DataType   | 是   | sting              | 数据类型（int,float,string,bool,enum,array,struct,date） |
| UnitName   | 否   | sting              | 单位                                                     |
| ParamType  | 否   | sting              | Array 参数类型，当 DataType=array 时必填                 |
| Params     | 否   | []StructDataStruct | Struct 参数列表，当 DataType=struct 时必填               |
| Enums      | 否   | []EnumDataStruct   | Enum 参数列表，当 DataType=enum 时必填                   |
| Bool       | 否   | BoolDataStruct     | Bool 参数列表，当 DataType=bool 时必填                   |

###### 返回

| 参数名称 | 必选 | 类型   | 描述 |
| -------- | ---- | ------ | ---- |
| Code     | 是   | int    | 0    |
| Data     | 是   | struct | 信息 |

###### Data 详情

| 参数名称   | 必选 | 类型  | 描述   |
| ---------- | ---- | ----- | ------ |
| Identifier | 是   | sting | 标识符 |

```json
{
  "Code": 0,
  "Data": {
    "Identifier": "a0006"
  }
}
```

---

#### 2, Command List

```http
POST /api/v1/tmodel/command/list HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                    |
| ------------ | --------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。            |
| URL          | `/api/v1/tmodel/command/list`，URL 地址 |
| Context-Type | application/json                        |

Post Json 参数

| 参数名称   | 必选 | 类型   | 描述       |
| ---------- | ---- | ------ | ---------- |
| ProductSN  | 是   | string | 产品序列号 |
| Name       | 是   | sting  | 名称       |
| Identifier | 是   | sting  | 标识符     |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |
| Data     | 是   | Data | 信息 |

###### Data 详情

| 参数名称 | 必选 | 类型      | 描述 |
| -------- | ---- | --------- | ---- |
| Count    | 是   | int       | 数量 |
| List     | 是   | []command | 列表 |

###### command

| 参数名称    | 必选 | 类型         | 描述   |
| ----------- | ---- | ------------ | ------ |
| Name        | 是   | sting        | 名称   |
| Identifier  | 是   | sting        | 标识符 |
| Description | 否   | sting        | 描述   |
| Output      | 否   | []DataStruct | Output |
| Input       | 否   | []DataStruct | Input  |

```json
{
  "Code": 0,
  "Data": {
    "Count": 1,
    "List": [
      {
        "Identifier": "e0003",
        "Name": "e0003",
        "Description": "desc",
        "Type": "error",
        "Input": [
          {
            "Identifier": "a00001",
            "Name": "a",
            "DataType": "int"
          }
        ],
        "Output": [
          {
            "Identifier": "a00001",
            "Name": "a",
            "DataType": "int"
          }
        ]
      }
    ]
  }
}
```

---

#### 3, Command Update

```http
POST /api/v1/tmodel/command/update HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                      |
| ------------ | ----------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。              |
| URL          | `/api/v1/tmodel/command/update`，URL 地址 |
| Context-Type | application/json                          |

Post Json 参数

| 参数名称    | 必选 | 类型         | 描述       |
| ----------- | ---- | ------------ | ---------- |
| ProductSN   | 是   | string       | 产品序列号 |
| Name        | 是   | sting        | 名称       |
| Identifier  | 是   | sting        | 标识符     |
| Description | 否   | sting        | 描述       |
| Input       | 否   | []DataStruct | Input      |
| Output      | 否   | []DataStruct | Output     |

###### 返回

| 参数名称 | 必选 | 类型   | 描述 |
| -------- | ---- | ------ | ---- |
| Code     | 是   | int    | 0    |
| Data     | 是   | struct | 信息 |

###### Data 详情

| 参数名称   | 必选 | 类型  | 描述   |
| ---------- | ---- | ----- | ------ |
| Identifier | 是   | sting | 标识符 |

```json
{
  "Code": 0,
  "Data": {
    "Identifier": "a0006"
  }
}
```

---

#### 4, Command Delete

```http
POST /api/v1/tmodel/command/delete HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                      |
| ------------ | ----------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。              |
| URL          | `/api/v1/tmodel/command/delete`，URL 地址 |
| Context-Type | application/json                          |

Post Json 参数

| 参数名称   | 必选 | 类型    | 描述       |
| ---------- | ---- | ------- | ---------- |
| ProductSN  | 是   | string  | 产品序列号 |
| Identifier | 是   | []sting | 标识符     |

###### 返回

| 参数名称 | 必选 | 类型   | 描述     |
| -------- | ---- | ------ | -------- |
| Code     | 是   | int    | 0        |
| Data     | 是   | struct | 用户信息 |

```json
{
  "Code": 0
}
```

---

#### 5, Command Data

```http
POST /api/v1/tmodel/command/data HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                    |
| ------------ | --------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。            |
| URL          | `/api/v1/tmodel/command/data`，URL 地址 |
| Context-Type | application/json                        |

Post Json 参数

| 参数名称  | 必选 | 类型   | 描述       |
| --------- | ---- | ------ | ---------- |
| ProductSN | 是   | string | 产品序列号 |
| DeviceSN  | 是   | string | 设备序列号 |
| KeyWord   | 否   | sting  | KeyWord    |
| StartTime | 否   | int64  | 开始时间   |
| EndTime   | 否   | int64  | 截止时间   |
| Offset    | 否   | int    | Offset     |
| Limit     | 否   | int    | Limit      |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |
| Data     | 是   | List | 信息 |

###### Data

| 参数名称    | 必选 | 类型   | 描述   |
| ----------- | ---- | ------ | ------ |
| Name        | 是   | sting  | 名称   |
| Identifier  | 是   | sting  | 标识符 |
| Description | 否   | sting  | 描述   |
| Timestamp   | 是   | int    | 时间戳 |
| Output      | 是   | map    | 输出   |
| Input       | 是   | map    | 输入   |
| Status      | 是   | string | 状态   |

```json
{
  "Code": 0,
  "Data": {
    "Count": 1,
    "List": [
      {
        "Identifier": "a0006",
        "Name": "a0006",
        "Description": "desc",
        "Status": "success",
        "Output": {
          "Value": 1
        },
        "Timestamp": 102568844440
      }
    ]
  }
}
```

---

#### 6, Command Send

```http
POST /api/v1/tmodel/command/send HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                    |
| ------------ | --------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。            |
| URL          | `/api/v1/tmodel/command/send`，URL 地址 |
| Context-Type | application/json                        |

Post Json 参数

| 参数名称   | 必选 | 类型           | 描述         |
| ---------- | ---- | -------------- | ------------ |
| ProductSN  | 是   | string         | 产品序列号   |
| DeviceSN   | 是   | string         | 设备序列号   |
| Identifier | 是   | sting          | 标识符       |
| Input      | 是   | map[string]any | Input        |
| Sync       | 否   | bool           | Sync         |
| Timeout    | 是   | int            | 同步超时时间 |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |

```json
{
  "Code": 0
}
```

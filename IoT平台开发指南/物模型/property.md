## Property API Reference

#### 1, Property Create

```http
POST /api/v1/tmodel/property/create HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 POST 方法。               |
| URL          | `/api/v1/tmodel/property/create`，URL 地址 |
| Context-Type | application/json                           |

Post Json 参数

| 参数名称    | 必选 | 类型               | 描述                                                     |
| ----------- | ---- | ------------------ | -------------------------------------------------------- |
| ProductSN   | 是   | string             | 产品序列号                                               |
| Name        | 是   | sting              | 名称                                                     |
| Identifier  | 是   | sting              | 标识符                                                   |
| Description | 否   | sting              | 描述                                                     |
| DataType    | 是   | sting              | 数据类型（int,float,string,bool,enum,array,struct,date） |
| UnitName    | 否   | sting              | 单位                                                     |
| ParamType   | 否   | sting              | Array 参数类型，当 DataType=array 时必填                 |
| Params      | 否   | []StructDataStruct | Struct 参数列表，当 DataType=struct 时必填               |
| Enums       | 否   | []EnumDataStruct   | Enum 参数列表，当 DataType=enum 时必填                   |
| Bool        | 否   | BoolDataStruct     | Bool 参数列表，当 DataType=bool 时必填                   |

###### StructDataStruct

| 参数名称   | 必选 | 类型             | 描述                                   |
| ---------- | ---- | ---------------- | -------------------------------------- |
| Name       | 是   | sting            | 名称                                   |
| Identifier | 是   | sting            | 标识符                                 |
| DataType   | 是   | sting            | 数据类型（int,float,string,struct      |
| UnitName   | 否   | sting            | 单位                                   |
| Enums      | 否   | []EnumDataStruct | Enum 参数列表，当 DataType=enum 时必填 |
| Bool       | 否   | BoolDataStruct   | Bool 参数列表，当 DataType=bool 时必填 |

###### BoolDataStruct

| 参数名称 | 必选 | 类型  | 描述 |
| -------- | ---- | ----- | ---- |
| True     | 是   | sting | 值   |
| False    | 是   | sting | 值   |

###### EnumDataStruct

| 参数名称    | 必选 | 类型  | 描述   |
| ----------- | ---- | ----- | ------ |
| Value       | 是   | int   | 值     |
| Description | 是   | sting | 值描述 |

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

#### 2, Property List

```http
POST /api/v1/tmodel/property/list HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                     |
| ------------ | ---------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。             |
| URL          | `/api/v1/tmodel/property/list`，URL 地址 |
| Context-Type | application/json                         |

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

| 参数名称 | 必选 | 类型       | 描述 |
| -------- | ---- | ---------- | ---- |
| Count    | 是   | int        | 数量 |
| List     | 是   | []Property | 列表 |

###### Property

| 参数名称    | 必选 | 类型               | 描述                                                     |
| ----------- | ---- | ------------------ | -------------------------------------------------------- |
| Name        | 是   | sting              | 名称                                                     |
| Identifier  | 是   | sting              | 标识符                                                   |
| Description | 否   | sting              | 描述                                                     |
| DataType    | 是   | sting              | 数据类型（int,float,string,bool,enum,array,struct,date） |
| UnitName    | 否   | sting              | 单位                                                     |
| ParamType   | 否   | sting              | Array 参数类型，当 DataType=array 时必填                 |
| Params      | 否   | []StructDataStruct | Struct 参数列表，当 DataType=struct 时必填               |
| Enums       | 否   | []EnumDataStruct   | Enum 参数列表，当 DataType=enum 时必填                   |
| Bool        | 否   | BoolDataStruct     | Bool 参数列表，当 DataType=bool 时必填                   |

```json
{
  "Code": 0,
  "Data": {
    "Count": 3,
    "List": [
      {
        "Identifier": "a0003",
        "Name": "a0003",
        "Description": "desc",
        "DataType": "struct",
        "Params": [
          {
            "Identifier": "a00001",
            "Name": "a",
            "DataType": "int"
          }
        ]
      },
      {
        "Identifier": "a0001",
        "Name": "a0001",
        "Description": "desc",
        "DataType": "bool",
        "Bool": {
          "False": "a00001",
          "True": "a"
        }
      },
      {
        "Identifier": "a0004",
        "Name": "a0004",
        "Description": "desc",
        "DataType": "array",
        "ParamType": "struct",
        "Params": [
          {
            "Identifier": "a00001",
            "Name": "a",
            "DataType": "int"
          },
          {
            "Identifier": "a00002",
            "Name": "b",
            "DataType": "string"
          }
        ]
      },
      {
        "Identifier": "a0006",
        "Name": "a0006",
        "Description": "desc",
        "DataType": "array",
        "ParamType": "struct",
        "Params": [
          {
            "Identifier": "a00001",
            "Name": "a",
            "DataType": "int"
          },
          {
            "Identifier": "a00002",
            "Name": "b",
            "DataType": "string"
          }
        ]
      }
    ]
  }
}
```

---

#### 3, Property Update

```http
POST /api/v1/tmodel/property/update HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 POST 方法。               |
| URL          | `/api/v1/tmodel/property/update`，URL 地址 |
| Context-Type | application/json                           |

Post Json 参数

| 参数名称    | 必选 | 类型               | 描述                                                     |
| ----------- | ---- | ------------------ | -------------------------------------------------------- |
| ProductSN   | 是   | string             | 产品序列号                                               |
| Name        | 是   | sting              | 名称                                                     |
| Identifier  | 是   | sting              | 标识符                                                   |
| Description | 否   | sting              | 描述                                                     |
| DataType    | 是   | sting              | 数据类型（int,float,string,bool,enum,array,struct,date） |
| UnitName    | 否   | sting              | 单位                                                     |
| ParamType   | 否   | sting              | Array 参数类型，当 DataType=array 时必填                 |
| Params      | 否   | []StructDataStruct | Struct 参数列表，当 DataType=struct 时必填               |
| Enums       | 否   | []EnumDataStruct   | Enum 参数列表，当 DataType=enum 时必填                   |
| Bool        | 否   | BoolDataStruct     | Bool 参数列表，当 DataType=bool 时必填                   |

###### StructDataStruct

| 参数名称   | 必选 | 类型             | 描述                                   |
| ---------- | ---- | ---------------- | -------------------------------------- |
| Name       | 是   | sting            | 名称                                   |
| Identifier | 是   | sting            | 标识符                                 |
| DataType   | 是   | sting            | 数据类型（int,float,string,struct      |
| UnitName   | 否   | sting            | 单位                                   |
| Enums      | 否   | []EnumDataStruct | Enum 参数列表，当 DataType=enum 时必填 |
| Bool       | 否   | BoolDataStruct   | Bool 参数列表，当 DataType=bool 时必填 |

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

#### 4, Property Delete

```http
POST /api/v1/tmodel/property/delete HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 POST 方法。               |
| URL          | `/api/v1/tmodel/property/delete`，URL 地址 |
| Context-Type | application/json                           |

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

#### 5, Property Desire

```http
POST /api/v1/tmodel/property/desire HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 POST 方法。               |
| URL          | `/api/v1/tmodel/property/desire`，URL 地址 |
| Context-Type | application/json                           |

Post Json 参数

| 参数名称   | 必选 | 类型   | 描述       |
| ---------- | ---- | ------ | ---------- |
| ProductSN  | 是   | string | 产品序列号 |
| DeviceSN   | 是   | string | 设备序列号 |
| Identifier | 是   | sting  | 标识符     |
| Value      | 是   | any    | 数据       |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |

```json
{
  "Code": 0
}
```

---

#### 6, Property Data

```http
POST /api/v1/tmodel/property/data HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                     |
| ------------ | ---------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。             |
| URL          | `/api/v1/tmodel/property/data`，URL 地址 |
| Context-Type | application/json                         |

Post Json 参数

| 参数名称  | 必选 | 类型   | 描述       |
| --------- | ---- | ------ | ---------- |
| ProductSN | 是   | string | 产品序列号 |
| DeviceSN  | 是   | string | 设备序列号 |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |
| Data     | 是   | List | 信息 |

###### Data

| 参数名称    | 必选 | 类型          | 描述   |
| ----------- | ---- | ------------- | ------ |
| Name        | 是   | sting         | 名称   |
| Identifier  | 是   | sting         | 标识符 |
| Description | 否   | sting         | 描述   |
| Current     | 否   | PropertyValue | 当前值 |
| Desired     | 否   | any           | 期望值 |

###### PropertyValue

| 参数名称  | 必选 | 类型 | 描述       |
| --------- | ---- | ---- | ---------- |
| Timestamp | 是   | int  | 时间戳毫秒 |
| Value     | 是   | any  | 数组       |

```json
{
  "Code": 0,
  "Data": {
    "List": [
      {
        "Identifier": "a0006",
        "Name": "a0006",
        "Description": "desc",
        "Current": {
          "Value": 1,
          "Timestamp": 102568844440
        },
        "Desired": 1
      }
    ]
  }
}
```

---

#### 7, Set Device Property Data

```http
POST /api/v1/tmodel/property/set HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                    |
| ------------ | --------------------------------------- |
| Method       | 请求方法。只支持 POST 方法。            |
| URL          | `/api/v1/tmodel/property/set`，URL 地址 |
| Context-Type | application/json                        |

Post Json 参数

| 参数名称  | 必选 | 类型   | 描述                        |
| --------- | ---- | ------ | --------------------------- |
| ProductSN | 是   | string | 产品序列号                  |
| DeviceSN  | 是   | string | 设备序列号                  |
| Data      | 是   | json   | 属性数据{Identifier: Value} |

###### 返回

| 参数名称 | 必选 | 类型 | 描述 |
| -------- | ---- | ---- | ---- |
| Code     | 是   | int  | 0    |

```json
{
  "Code": 0
}
```

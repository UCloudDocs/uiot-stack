## Template API Reference

#### 1, Template Import

```http
POST /api/v1/tmodel/template/import HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 POST 方法。               |
| URL          | `/api/v1/tmodel/template/import`，URL 地址 |
| Content-Type | multipart/form-data                        |

Post Form 参数

| 参数名称  | 必选 | 类型   | 描述       |
| --------- | ---- | ------ | ---------- |
| ProductSN | 是   | string | 产品序列号 |
| File      | 是   | file   | 模板文件   |

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

#### 2, Template Export

```http
GET /api/v1/tmodel/template/export HTTP/1.1
Host: localhost:8080
```

###### 参数说明

| 参数         | 说明                                       |
| ------------ | ------------------------------------------ |
| Method       | 请求方法。只支持 GET 方法。                |
| URL          | `/api/v1/tmodel/template/export`，URL 地址 |
| Content-Type | multipart/form-data                        |

Query Form 参数

| 参数名称  | 必选 | 类型   | 描述       |
| --------- | ---- | ------ | ---------- |
| ProductSN | 是   | string | 产品序列号 |

###### 返回

| 参数                | 说明                               |
| ------------------- | ---------------------------------- |
| Content-Disposition | attachment; filename=template.json |

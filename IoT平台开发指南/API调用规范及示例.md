# API调用规范及示例

IoT平台使支持API调用以满足客户的业务需求。



## API调用规范



### HTTP请求头信息（HTTP Request Header）

|参数名称|必选|类型|描述|
|---|---|---|---|
|Authorization|否|string|JWT Token,需访问认证的API时必填|
|ProjectName|否|string|ProjectName,认证的除用户和项目API时必填|
|Context-Type|否|application/json|Method:Post时必填，body必填|




### HTTP响应头信息（HTTP Response Header）

|参数名称|必选|类型|描述|
|---|---|---|---|
|Context-Type|是|application/json|返回的 Context Type类型|




### HTTP响应消息体（HTTP Response Body）

|参数名称|必选|类型|描述|
|---|---|---|---|
|Code|是|int|返回码（0，400，401，402, 500）|
|Message|否|sting|返回错误信息|
|Data|是|Any|响应数据|

**HTTP响应码描述（HTTP Response Code）**

|参数名称|描述|
|---|---|
|0||成功|
|400|参数错误|
|401|身份认证失败（Token错误，需返回登陆页面）|
|402|权限认证失败（没有日志（或者相关模块的权限））|
|500|内部服务器错误|







## API调用示例

本节通过一个具体的示例（创建产品）介绍如何实现API调用。用户可将其中的参数换成自己的实际参数进行测试。API仅支持HTTPS调用。

1. 概述   

   接口的调用使用HTTPS GET或POST调用。调用的参数包括接口的参数+公共参数两部分。
   - 域名：可在IoT控制台平台参数中获取
   - 接口参数：某个具体接口需要的参数；
   - 公共参数：每个接口都需要用到的项目名（ProjectName）、Authorization（登录时获取的Token）
   
2. 查看接口文档获取接口名及参数

名称| 内容
---|---
接口名| /api/v1/product/create- 创建产品 
接口参数|**ProductName**：产品名称<br>**Description**：描述<br>**ProductType**：产品类型

3. 获取公共参数，即每次请求都需要的参数

名称|内容
---|---
ProjectName|需操作的资源所属的项目
Authorization|用户登录时获取的Token


4. 获取Token  



5. 请求接口获取响应

   通过HTTPS GET或POST得到响应结果

   1）**通过GET方式**  
   ① 当参数中存在特殊字符时需要进行编码，编码的规则为：  
      - 字符 `A~Z`、`a~z`、`0~9`不编码；
      - 字符`-`、`_`、`.`、`~`不编码；
      - 其它字符编码成 `%XY` 的格式，其中`XY`是字符对应ASCII码的16进制表示。比如：英文的双引号`”`对应的编码为`%22`；
      - 对于扩展的UTF-8字符，编码成 `%XY%ZA…` 的格式；
      - 英文空格` `要编码成`%20`，而不是加号`+`。

   ② 将请求参数转换成URL Params，拼接上Base URL进行GET请求：
     ```
GET /api/v1/product/info?ProductSN=yihyugtyfytft HTTP/1.1
   Host: ${域名或IP}:8080
   Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VJRCI6MSwiUm9sZUlEIjoxLCJVc2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjAzODkzMzQ1LCJpc3MiOiJ1aW90IHN0YWNrLXVzZXIifQ.8ACQkiM6YSB4VgHJySGAlCQF_lTMsZ7dPo7F9xMwaWI
   ProjectName: default
     ```
   ③ 返回响应结果为：
   ```
   {
       "Code": 0,
       "Data": {
           "ActiveCount": 0,
           "CreatedTime": 1604300597762,
           "Description": "dwaasddd",
           "DeviceCount": 0,
           "OnlineCount": 0,
           "ProductID": 1,
           "ProductName": "bobo_test1",
           "ProductPassword": "jglscoc53ceg5mve",
           "ProductPublished": false,
           "ProductSN": "omrb4q753x9sk904",
           "ProductType": "device"
    }
   }
   
   ```
   
   2）**通过POST方式**  
   ① POST通过JSON格式请求：
   
   ```
   POST /api/v1/product/create HTTP/1.1
   Host: ${域名或IP}:8080
   Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VJRCI6MSwiVXNlcm5hbWUiOiJhZG1pbiIsImV4cCI6MTYwNDU5Nzc2MiwiaXNzIjoidWlvdCBzdGFjay11c2VyIn0.zyiQuYn-IxN4B7x0mhk5uj1aGzFLnL85wwx2ecchxG8
   ProjectName: default
   Content-Type: application/json
   Content-Length: 186
   
   {
       "ProductName":"bobo_test3",
       "ProductType":"device",
       "Description":"dwaasddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd波波"
   }
   ```
   ②返回响应结果为：
   ```
   {
       "Code": 0,
       "Data": {
           "ProductID": 110
       }
   }
   ```


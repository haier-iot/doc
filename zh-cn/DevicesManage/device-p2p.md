
!>  **更新时间**：{docsify-updated}  




### 音视频服务接口

#### 开通视频云存服务接口

> 提供智家APP调用，开通视频设备的腾讯云存服务。

##### 1、接口定义

?> **接入地 址：**  `/rcs/avm/tencent/device/create-storage `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| apiVersion     | String | header| 是|Api版本号(默认v1)|
| deviceId     | String | Body| 是|海尔定义的设备唯一标识|
| orderSn     | String | Body| 是|海尔商城订单号|

**输出参数**  

|   名称      |     类型      | 位置  |示例 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String  |   Body   | 00000| 返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）  |
|  retInfo  |  String  |   Body   | 成功| 用于调试的返回信息，不支持国际化，也不能直接显示在UI  |
|  payload  |  CreateStorageResultDto  |   &nbsp;   | 是| 返回数据  |


**CreateStorageResultDto**

|   字段名      |    类型       |说明|
| ------------- |:----------:|:---------:|
|  status   | Integer| 云存服务状态(1-正常使用中，2-待续费，3-已过期)  |
|  startTime   | Long| 云存订单开始时间 UTC时间 单位秒  |
|  endTime   | Long| 云存订单结束时间 UTC时间 单位秒  |



##### 2、请求样例  

**用户请求**
```  
POST data:


Request Headers:

Connection: keep-alive

appId: ************

appVersion: 01.00.00.00000

clientId: 111111111111

accessToken: ************

sign: ************

timestamp: 1585577435707

Content-Encoding: utf-8

Content-type: application/json

Post：

{

  "deviceId": "1221211121",

  "orderSn":"ssdw23233"

}
```  

**请求应答**

```
{

  "payload": {

    "status": 1,

    "startTime": 1602664844,

    "endTime": 1607935244

  },

  "retCode": "00000",

  "retInfo": "成功"

}


```

#### 查看视频云存订单及状态接口

> 提供智家APP调用，查看视频云存订单及状态信息

###### 1、接口定义

?> **接入地 址：**  `/rcs/avm/tencent/device/find-storage-state  `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| apiVersion     | String | header| 是|Api版本号(默认v1)|
| deviceId     | String | Body| 是|海尔定义的设备唯一标识|


**输出参数**  

|   名称      |     类型      | 位置  |示例 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String  |   Body   | 00000| 返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）  |
|  retInfo  |  String  |   Body   | 成功| 用于调试的返回信息，不支持国际化，也不能直接显示在UI  |
|  payload  |  StorageStateResultDto  |   &nbsp;   | 是| 返回数据  |


**StorageStateResultDto**

|   字段名      |    类型       |说明|
| ------------- |:----------:|:---------:|
|  activatedStorage   | Integer| 当前生效的视频云存规格(0,7,30)  |
|  remainderDays   | Long| 当前生效的视频云存剩余天数  |
|  expireTime   | Long| 当前生效的视频云存失效时间 UTC时间 单位秒  |


##### 2、请求样例  

**用户请求**
```
POST data:

[no cookies]

Request Headers:

Connection: keep-alive

appId: ****************

appVersion: ****************

clientId: ****************

accessToken: ****************

sign: ****************

timestamp: 1585577435707

Content-Encoding: utf-8

Content-type: application/json

Post：

{

  "deviceId": "1221211121"

}


```  

**请求应答**

```
{

  "payload": {

    "activatedStorage": 30,

    "remainderDays": 20,

    "expireTime": 1607935244

  },

  "retCode": "00000",

  "retInfo": "成功"

}

```


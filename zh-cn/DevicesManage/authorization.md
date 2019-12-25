
!> **更新时间**：{docsify-updated}  


## 简介

设备管理服务为开发者提供对智能终端设备信息的管控的基础能力服务。开发者通过集成此服务模块，可提供用户与设备绑定、展现设备状态、位置等信息以及设备部分属性的基础维护能力，实现智能设备相关的基础能力集成的快速开发。

Haier U+云平台提供的对智能互联设备的操作设备进行命令下发、读写等相关设备的操作能力。


### 基础信息管理服务流程
基础信息管理服务主要为开发的应用程序实现用户与设备绑定、解绑设备、获取用户设备列表等与智能互联设备相关的基础管理服务。
![设备管理场景流程][DevicesStandard_flow]

### 设备控制服务流程

设备管理服务基于用户拥有对应的设备权限，即设备管理员（绑定用户）或权限用户。

设备操作命令可通过业务服务上报到平台，平台将控制命令解析并下发到智能设备进行控制

![设备管理企业版场景流程][DeviceE_flow]



## 接口公共部分说明
### AuthInfo
权限内容，其中至少一项为ture

参数名|类型|说明|备注
:-|:-:|:-:|:-
view|Boolean|是否有查看权限|
set|Boolean|是否有配置权限|
control|Boolean|是否有控制权限|

### Permission
权限信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
auth|AuthInfo|权限内容|
authType|String|权限类型|home：家庭分享</br>share：个人分享</br>owener：设备主人</br>server：给appserver的权限

### DeviceBriefInfo

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备WiFitype|
deviceType|String|设备类别|
online|Boolean|是否在线|



### DeviceInfo

绑定设备信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifi|
deviceType|String|设备类型|
totalPermission|AuthInfo|权限和，权限信息的综合|
permissions|Permission[]|权限信息|
online|Boolean|是否在线|

### BaseProperty
基础属性

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|设备品牌|
model|String|设备型号|
others|Map<String,Strng>|其他属性|

### Deviceversion
设备详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|String|设备ID|
modules|Set<Module>|模块信息|
wifiType|String|wifi类型|
deviceType|String|设备类型|
baseProperty|BaseProperty|品牌信息|
location|Location|位置信息|

### Module
模块信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
moduleId|String|模块ID|
moduleType|String|模块类型|
ModuleInfos|Map<String,String>|模块其他信息|

### Location
定位信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
longitude|Double|经度|
latitude|Double|维度|
cityCode|String|城市编码|

### DeviceNetQualityDto
设备信号强度

参数名|类型|说明|备注
:-|:-:|:-:|:-
softwareType|String|软件类型，平台信息|
hardware|String|硬件版本类型|
hardwareVers|String|硬件版本号|
softdwareVers|String|软件版本号|
netType|String|网络类型|可取值：</br>unknown,位置网络或设备不支持挽留过质量上报；</br>Wifi：WIFI网络
strength|String|信号强度|

### DeviceStatus
设备状态

参数名|类型|说明|备注
:-|:-:|:-:|:-
timestamp|long|时间戳|
deviceId|String|设备Id|
statuses|Map<String,String>|设备状态|

### RoomInfoLocation
房间位置

参数名|类型|说明|备注
:-|:-:|:-:|:-
userId|String|用户ID|
deviceId|String|设备Id|
room|String|设备房间位置信息|

### DeviceRoomInfoDto
设备房间位置信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifitype|
deviceType|String|设备类别|
room|String|设备房间位置信息|
permission|Permission[]|权限信息|
online|Boolean|是否在线|

### BrandInfo
品牌/型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|品牌|
model|String|型号|
deviceId|String|设备ID|  


### DevFWVersion  
设备整机固件版本信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
model|String|U+产品整机型号编码|
vers|String|设备当前固件版本号。字符串格式，不做其余格式校验|
firmware|Firmware|设备可升级的新版本固件信息|如果设备不支持升级或者当前没有更新的固件，则不存在firmware字段    


### Firmware
设备可升级的新版本固件信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
vers|String|新版本固件版本号。字符串格式，不做其余格式校验|
desc|String|新版本固件描述信息|
timeout|Int|升级超时时间，单位：秒，取值范围：10~604800（7天）|升级超时时间平台仅用于记录，并且在升级进度及结果中返回给用户，具体超时判断由App进行  
digestAlg|String|设备固件文件摘要算法，可取值：sha256 -- SHA256计算摘要|   
digest|String|设备固件文件摘要信息，格式：全小写十六进制字符串|
url|String|设备固件文件URL地址|
keyAlg|String|设备固件文件加密算法。如果设备固件未加密，则值为空字符串。可取值：aes256 -- AES256加密|  
key|String|设备固件文件加密密钥，格式：全小写十六进制字符串。如果设备固件未加密，则值为空字符串|  
size|int|原始升级包大小|


### User
用户信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
loginId|String|用户名（邮箱）|
userId|String|用户Id|
userProflie|Map|用户拓展属性|

### OpPropertyValue
属性操作

参数名|类型|说明|备注
:-|:-:|:-:|:-
name|String|属性|
value|String|值|

### OpResult 
操作结果

参数名|类型|说明|备注
:-|:-:|:-:|:-
usn|String|操作序列号|
deviceId|String|操作设备ID
result|String|操作应答结果|是一个base64码，标准模型设备解密后的结果为：`{"extData":{},"args":[]}`，其中[]中的数据为多个由name,value组成的键值对；</br>非标准模型设备解密后的结果为:`{"extData":{},"statuses":[]}`，其中[]中的数据为多个由name,value组成的键值对

### DeviceBaseInfo

设备基本信息

参数名|类型|说明|备注
:-|:-:|:-:|:- 
deviceId|String|设备Id|
sern|String|机器编码|
productCodeT|String|产品型号编码|
productNameT|String|产品型号名称|
connectionStatus|String|设备连接状态|online：在线；offline：离线

### ProductInfo
产品型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
ProdcutCodeT|String|产品型号编码|
ProcutNameT|String|产品型号名称|



### 绑定与解绑

#### 绑定
> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=300个，绑定时设备必须平台上线。）</font>


##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID
name|String|body|必填|设备名称
data|String|body|必填|绑定加密数据

**输出参数:** 输出标准应答参数

##### 2、请求样例
**请求样例**
```
请求地址：/uds/v1/protected/bindDevice
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body
{
    "deviceId": "************",
    "name": "test002",
    "data": "5f10bf4f5af08db934c8165c32140227"
}
```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}
```
##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006、G20904、G20908、G20910



#### 解绑设备
> 用户解绑设备，同时解除设备的相关分享

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/unbindDevice` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/B00000000002/unbindDevice
Header：
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功！"
}
```
##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 设备管理员查询设备管理员分享给个人的所有设备
> 查询设备管理员分享给个人的所有设备  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ person/shareDevices?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|String	|pageNumber|	url|	否	|当前访问信息的起始页，从1开始|
|String	|pageSize	|url|	否	|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  |共享设备信息 |
|String|	totalCount|	Body|	必填	|总数|
|String|	pageSize|	Body|	必填	|当前返回页实际数量，不超过规定的最大数据|
|String|	pageNumber|	Body|	必填	|当前页，从1开始|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"shareDevs":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},

"devName":"测试1",
"devShareUser":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}
},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试2",
"devShareUser":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}
}
],
"retCode":"00000",
"retInfo":"成功"

}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

#### 普通用户查询分享给我的个人分享设备
> 查询分享给我的所有个人分享设备   

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/shareDevices2me?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|String	|pageNumber|	url|	否	|当前访问信息的起始页，从1开始|
|String	|pageSize	|url|	否	|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |
|String|	totalCount|	Body|	必填	|总数|
|String|	pageSize|	Body|	必填	|当前返回页实际数量，不超过规定的最大数据|
|String|	pageNumber|	Body|	必填	|当前页，从1开始|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
shareDevs ":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

}
],
	"totalCount": "20",
	"pageSize": "3",
	2.7.1.10	UserBriefInfo"pageNumber": "1",
    "retCode": "00000", 
    "retInfo": "成功", 
   
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008     

#### 设备管理员查询自有设备列表
> 查询用户自有设备列表    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/devices`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| DeviceBriefInfo [] |  devices  |   Body  |  必填  |  &emsp;|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"devices ":[{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
}, 
{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
}], 
"retCode":"00000","retInfo":"成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    


#### 设备管理员分享设备给个人
> 设备管理员分享设备给个人，发送分享个人设备消息给目标用户，记录消息发送结果到日志，支持targetId为临时的userid

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{targetId}/shareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice  | shareDev   | Body |必填|设备的分享信息 |  
| String  | targetId   | url |必填|分享设备的目标用户 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"shareDev":{
"devInfo":{           
"deviceId": "100823",
},
"devName":"test",
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

}
}


```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，F31225   
  

#### 设备管理员取消设备分享
> 设备管理员取消用户分享，发送取消个人分享设备消息给目标用户，记录消息发送结果到日志, 支持targetId为临时的userid

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/ {targetId}/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | targetId   | url |必填|设备分享者 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| -------------|:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

#### 用户删除分享给自己的个人设备
> 用户取消设备管理员分享给自己的设备

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{devId}/shareDeviceToMe`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| -------------|:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   





[^-^]:常用图片注释
[DevicesStandard_flow]:../_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:../_media/_DevicesEnterprise/DeviceE_flow.png

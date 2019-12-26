
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



### 设备操作高级动能


#### 设备指令执行操作接口（经过逻辑运算）

> 领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

统一接收标准模型的命令，命令经过逻辑运算、转换、补偿后下发到设备


##### 1、接口定义
?> **接入地址：** `stdudse/v1/modfier/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|上下文|必填|用户token
deviceId|String|Body|必填|设备ID
cmdName|String|Body|否|组命令id（1、若该操作为组命令操作，则该值必填。2、若该操作为单命令操作，则该值不需要传递）
cmdArgs|Map<String,String>|Body|必填|一组命令,即属性集合（key-value）。（若该操作为单命令操作，则该值必须只有一对key-value。）
callbackUrl|String|Body|非必填|操作应答回调地址，只支持http协议


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|非必填|序列号sn

##### 2、请求示例

**请求样例**


```
Header：
appId:MB-*****-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
sign:bd4495183b97e8133aeab2f1916fed41
timestamp: 1436236880183
accessToken:TGT37FAT5QBI2UNO2TFWT4AASDKAF0
language:zh-cn
timezone:8
Content-type: application/json

Body
{
"deviceId": "********",
"cmdName": "grSetDAC",
"cmdArgs": {"pmvStatus":"true","cleaningTimeStatus":"false","cloudFilterChangeFlag":"false","electricHeatingStatus":"true","onOffStatus":"true","operationMode":"4"},
"callbackUrl": "http://www.uhome.haier.net/callback.html"
}

```

**请求应答**

```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```

**操作应答**

```
{
  "deviceId": "0007A893C119",
  "resCode": "0",
  "result": "eyJhcmdzIjpbeyJuYW1lIjoidGFyZ2V0VGVtcGVyYXR1cmUiLCJ2YWx1ZSI6IjE2LjAwIn0seyJuYW1lIjoid2luZERpcmVjdGlvblZlcnRpY2FsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoib3BlcmF0aW9uTW9kZSIsInZhbHVlIjoiNCJ9LHsibmFtZSI6InNwZWNpYWxNb2RlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoid2luZFNwZWVkIiwidmFsdWUiOiIxIn0seyJuYW1lIjoidGVtcFVuaXQiLCJ2YWx1ZSI6IjEifSx7Im5hbWUiOiJwbXZTdGF0dXMiLCJ2YWx1ZSI6InRydWUifSx7Im5hbWUiOiJpbnRlbGxpZ2VuY2VTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoiaGFsZkRlZ3JlZVNldHRpbmdTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoic2NyZWVuRGlzcGxheVN0YXR1cyIsInZhbHVlIjoidHJ1ZSJ9LHsibmFtZSI6IjEwZGVncmVlSGVhdGluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJlY2hvU3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6ImxvY2tTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoic2lsZW50U2xlZXBTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoibXV0ZVN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJyYXBpZE1vZGUiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoiZWxlY3RyaWNIZWF0aW5nU3RhdHVzIiwidmFsdWUiOiJ0cnVlIn0seyJuYW1lIjoiaGVhbHRoTW9kZSIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJvbk9mZlN0YXR1cyIsInZhbHVlIjoidHJ1ZSJ9LHsibmFtZSI6InRhcmdldEh1bWlkaXR5IiwidmFsdWUiOiIzMCJ9LHsibmFtZSI6Imh1bWFuU2Vuc2luZ1N0YXR1cyIsInZhbHVlIjoiMCJ9LHsibmFtZSI6IndpbmREaXJlY3Rpb25Ib3Jpem9udGFsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoibG9jYWxGaWx0ZXJDaGFuZ2VGbGFnIiwidmFsdWUiOiJ0cnVlIn0seyJuYW1lIjoiZW5lcmd5U2F2aW5nU3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6ImxpZ2h0U3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6InNlbGZDbGVhbmluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJjaDJvQ2xlYW5pbmdTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoicG0ycDVDbGVhbmluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJodW1pZGlmaWNhdGlvblN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJmcmVzaEFpclN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJpbmRvb3JUZW1wZXJhdHVyZSIsInZhbHVlIjoiMTguNTAifSx7Im5hbWUiOiJpbmRvb3JIdW1pZGl0eSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6Im91dGRvb3JUZW1wZXJhdHVyZSIsInZhbHVlIjoiLTY0LjAwIn0seyJuYW1lIjoiYWNUeXBlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoic2Vuc2luZ1Jlc3VsdCIsInZhbHVlIjoiMCJ9LHsibmFtZSI6ImFpclF1YWxpdHkiLCJ2YWx1ZSI6IjAifSx7Im5hbWUiOiJwbTJwNUxldmVsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoiZXJyQ29kZSIsInZhbHVlIjoiNiJ9LHsibmFtZSI6IkVyckFja0ZsYWciLCJ2YWx1ZSI6InRydWUifSx7Im5hbWUiOiJvcFNyYyIsInZhbHVlIjoiMyJ9LHsibmFtZSI6InRvdGFsQ2xlYW5pbmdUaW1lIiwidmFsdWUiOiIyNjY0In0seyJuYW1lIjoiaW5kb29yUE0ycDVWYWx1ZSIsInZhbHVlIjoiNTMifSx7Im5hbWUiOiJvdXRkb29yUE0ycDVWYWx1ZSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6ImNoMm9WYWx1ZSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6InZvY1ZhbHVlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoiY28yVmFsdWUiLCJ2YWx1ZSI6IjAifV19",
  "retCode": "00000",
  "retInfo": "成功",
  "usn": "600ce95da3e14fc7a68f483dd14db864"
}

```

##### 3、错误码

> G03002、B00001、G20202、B00004、A00001、000001、A00006、A00005、A00007、A00008、A00009


### 设备控制类接口

设备控制类接口通过APPSERVER访问

若APPSERVER部署在外网，则通过https://uws.haier.net/udse访问；
若APPSERVER部署在内网，则通过https://internal.uws.haier.net/udse访问；


**权限申请**

设备控制类接口服务授权，使用服务IP白名单策略，需要是用此服务请联系IOT平台技术支持开通系统IP白名单

**公共头部分**

Header 中appid 字段填写内容为系统ID，即systemid。 此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”



#### 用户设备操作-控制通道-非标准模型
> 用户设备操作-控制通道，支持非标准模型（6位码设备）设备的操作

##### 1、接口定义
?> **接入地址：** `/udse/v1/devicesOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
category|String|Body|必填|操作的分类</br>单命令："AttrOp";组命令："GroupOp"
name|String|Body|必填|操作名称
operateCodes|String|Body|必填|操作命令Base64加密值
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/devicesOperate
Header：
	appId:SV-****-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH",
	"category": "AttrOp",
	"name": "221001",
	"operateCodes": "eyJ2YWx1ZSI6IjIyMTAwMSJ9",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJzdGF0dXNlcyI6IFsKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfSwKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfQogICAgXQp9"，
	"resCode":0
}
```

##### 3、接口错误码
> B00001、B00004、A00001、D00006、G20202、G03002




#### 用户读属性-异步接口-标准模型

> 用户读属性-异步接口，支持标准模型的



##### 1、接口定义
?> **接入地址：** `/udse/v1/propertyRead`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
property|String|Body|必填|设备读属性的属性名
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/propertyRead
Header：
	appId:SV-****-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0="
}
```


##### 3、请求错误码
> B00001、B00004、A00001、D00006、G20202、G03002


#### 用户写属性-异步接口-标准模型
> 用户写属性-异步接口，支持标准模型的属性写

##### 1、接口定义
?> **接入地址：** `/udse/v1/propertyWrite`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
property|String|Body|必填|设备写属性的属性名
value|String|Body|必填|设备写属性的属性名
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/propertyWrite
Header：
	appId:SV-****-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"value": "value",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```
**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```
##### 3、接口错误码
> B00001、G20202、B00004、A00001、D00006、G03002



#### 用户设备操作-异步接口-标准模型
> 用户设备操作-异步接口，支持标准模型设备操作

##### 1、接口定义
?> **接入地址：** `/udse/v1/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|设备操作请求序列号
operationName|String|Body|必填|操作名称
operationValue|List<OpPropertyValue>|Body|必填|属性值的列表，由模型文档决定是否必填及如何填
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/operate
Header：
	appId:SV-****-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"operationName": "operationName",
	"operationValue": 
	[
	    {"name": "name1","value": "value1"},
	    {"name": "name2","value": "value2"}
	],
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```


**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```
**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```
##### 3、接口错误码
> B00001、B00004、A00001、D00006、G20202、G03002





## 关于设备控制接口参数填写与返回值

### 非标准设备单命令参数

```
category: 'AttrOp',
name: '20g10o',
operateCodes:base64({"value":"30g106"})
```
### 非标准设备组命令重要参数
```
category: 'GroupOp',
name: '000001',
operateCodes:base64({"value":[{"name":"20g10o","value":"30g106"},{"name":"20g10m","value":"12"},{"name":"20g10d","value":"30g101"}]})
```
>ID文档中，name有可能写成0x0001，请传递参数时，使用000001



[^-^]:常用图片注释
[DevicesStandard_flow]:../_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:../_media/_DevicesEnterprise/DeviceE_flow.png

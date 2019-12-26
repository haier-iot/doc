
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

### DeviceVersion
设备详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|String|设备ID|
modules|Set<Module>|模块信息|
wifiType|String|wifi类型|
deviceType|String|设备类型|
baseProperty|BaseProperty|品牌信息|
location|Location|位置信息|
productNameT|	String|	产品型号名称	
productCodeT|	String|	产品型号编码	
deviceRole|	String|	设备角色：1 普通设备;2 网关设备;3 附件设备;4 子设备;仅返回数字，角色信息不返回	
parentDeviceId|	String|	主设备Id：若该设备本身为主设备，该字段为null；若该设备为从设备，该字段为主设备Id	
deviceRoleType|	String|	设备类别：三种：主设备；从设备；无	
slaveDeviceIds|	List<String>|	所属从设备Id的集合：若无从设备该字段为空	

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

### DeviceProductInfo
参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|	String|	设备Id	
sern|	String|	机器编码	
productCodeT|	String|	产品型号编码	
productNameT|	String|	产品型号名称	
timestamp|	Long|	时间戳|	修改时间
factoryNum|	String|	工厂编码	



### 设备信息管理接口

#### 获取设备别名
> 获取设备aliasName（别名）

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
alisaName|String|body|必填|设备别名
deviceId|String|body|必填|设备ID

##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/aliasName
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
    "aliasName": "test002",
    "deviceId": "DC330D01FBF1",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 获取设备位置信息
> 获取设备的loaction（位置）内容

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
loc|Lociation|body|必填|设备位置信息


##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/location
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
    "loc": 
    {
        "cityCode": "101031900",
        "latitude": 0,
        "longitude": 0
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006




#### 获取设备详细信息
> 拥有设备查看权限的用户，查询设备的详细信息

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceInfo`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceVersion|DeviceVersion|body|必填|设备信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/deviceInfo
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
    "deviceVersion": {
        "baseProperty": {
            "model": "33"
        },
        "deviceId": "DC330D01FBF1",
        "deviceType": "03001001",
        "location": {
            "cityCode": "101031900",
            "latitude": 0,
            "longitude": 0
        },
        "modules": [
            {
                "moduleId": "",
                "moduleInfos": {
                    "hardwareVers": "1.0.00",
                    "softwareVers": "2.3.01",
                    "hardwareType": "R",
                    "softwareType": "eSDK_WIFI_AC",
                    "supportUpgrade": "1"
                },
                "moduleType": "basemodule"
            },
            {
                "moduleId": "DC330D01FBF1",
                "moduleInfos": {
                    "hardwareVers": "R_1.0.00",
                    "platform": "eSDK_WIFI_AC",
                    "protocolVers": "1.00",
                    "configVers": "0.0.0",
                    "softwareVers": "e_2.3.01",
                    "ip": "42.123.65.3"
                },
                "moduleType": "wifimodule"
            }
        ],
        "wifiType": "00000000000000008080000000041410",
        “productNameT”:”AB8Y7400Y”,
        “productCodeT”:” KFR-72LW/08DBA22A(香槟金)套机”
    },
    "retCode": "00000",
    "retInfo": "成功!"
}



```

##### 3、接口错误码

> A00001、B00001、G20202、A00004、B00001、D00006

#### 根据设备列表查询设备型号
> 根据设备列表查询设备型号

##### 1、接口定义

?> **接入地址：** `/udse/v1/devices/getProductInfos`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
|List<String>|Body|必填|设备id集合

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceProductInfos|Map<String,DeviceProductInfo>|body|必填|设备型号信息集合

##### 2、请求样例

**请求样例**

```
请求地址：/udse/v1/devices/getProductInfos
Header：
appId: SV-*****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
sign: 0e120bba*************0549e6bf3eb77f9d
timestamp: 1491014237857 
language: zh-cn
timezone: +8
appKey: 2**************a3a
Content-Encoding: utf-8
Content-type: application/json

[
    "DC****799325","DC****992B1"
]

```

**请求应答**

```
{
  "deviceProductInfos": {
    "DC****992B1": {
      "deviceId": "DC3****992B1",
      "productCodeT": "GD0QZ6005",
      "productNameT": "JSQ38-20CR7NPU1",
      "sern": "2",
      "sourceFrom": 2,
      "timestamp": 1564642994000
    },
    "DC****99325": {
      "deviceId": "DC33****9325",
      "productCodeT": "GA0SNC01C",
      "productNameT": "ES85H-PLUS9(U1)",
      "sern": "2",
      "sourceFrom": 2,
      "timestamp": 1563862732000
    }
  },
  "retCode": "00000",
  "retInfo": "成功!"
}


```

##### 3、接口错误码

> A00001、B00001、20903


#### 更新位置信息
> 有配置权限的用户更改设备的位置信息

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** PUT

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID
loc|Location|body|必填|位置信息

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/location
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
body
{
    "loc":
		{"cityCode":"101010200",
		 "latitude":39.975675,
         "longitude":116.322712
		}
}
```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 更新设备别名
> 有配置权限的用户更改设备别名

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** PUT

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID
aliasName|String|body|必填|设备新别名

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/aliasName

PUT data:
{"aliasName":"拨测的设备"}

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
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码


> A00001、B00001、G20202、A00004、B00001、D00006



#### 我的设备列表
> 查询我所有的设备,包含我的设备,个人分享给我的设备,家庭分享给我的设备

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/deviceinfos`</br>
**HTTP Method：** GET

**输入参数：** 无

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceinfos|DeviceInfo|body|必填|设备信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/deviceinfos
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
    "deviceinfos": [
        {
            "deviceId": "0007A8C17DBD",
            "deviceName": "空气净化器1数",
            "deviceType": "21001001",
            "online": false,
            "permissions": [
                {
                    "auth": {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "111c12002400081021010000005a4e4b32303134303931313031000000000000"，
            “productNameT”:” KFR-50LW/08GDB23A(茉莉白)套机”,
            “productCodeT”:” AB8XC706N”
        },
        {
            "deviceId": "C8934640B1DE",
            "deviceName": "空气盒子1",
            "deviceType": "14001001",
            "online": false,
            "permissions": [
                {
                    "auth": {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "101c120024000810140d00118003940000000000000000000000000000000000",
            “productNameT”:” KFR-72LW/08DBA22A(香槟金)套机”,
            “productCodeT”:” AB8Y7400Y”
        },
        {
            "deviceId": "DC330D01FBF1",
            "deviceName": "test002",
            "deviceType": "03001001",
            "online": true,
            "permissions": [
                {
                    "auth": {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "00000000000000008080000000041410",
            “productNameT”:” KFR-72LW/08DBA22A(香槟金)套机”,
            “productCodeT”:” AB8Y7400Y”
        }
    ],
    "retCode": "00000",
    "retInfo": "成功!"
}


```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006





#### 添加设备品牌信息
> 为设备添加品牌信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/saveDeviceBrand`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|Header|必填|用户token
brantInfo|BrandInfo|body|必填|设备品牌信息

**输出参数：** 输出标准应答参数 

##### 2、请求样例

**请求样例**
```
请求地址：/uds/v1/protected/deviceAndRoom
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
POST data
{   
	"brandInfo":{
		"brand":"品牌",
		"model":"型号",
		"deviceId":"0007A8947C62"
	}
}

```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、20903、D00006



#### 查询设备品牌信息
> 查询设备的品牌信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceBrand`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|上下文|必填|用户token
deviceId|String|url|必填|设备ID

**输出参数：** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
brantInfo|BrandInfo|body|必填|设备品牌信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/0007A8947C62/deviceBrand
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
    "brandInfo":
    {
        "brand":"品牌",
        "deviceId":"0007A8947C62",
        "model":"型号"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、20903、D00006、G24001  


#### 检测设备整机固件版本信息  
> 检测设备整机固件版本信息  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/fota/checkDevFMVersion`</br>
**HTTP Method：** POST  
**前置条件：** 用户和设备有绑定关系   

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|Header|必填|用户token
deviceId|String|Body|必填|设备ID

**输出参数：** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
devFWVersionDto|DevFWVersionDto|body|必填|设备整机固件版本信息  

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/fota/checkDevFMVersion  
POST data:
{   
	"deviceId":"0007A8947C62"
}

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
   "devFWVersionDto":
   {
      "firmware":
      {
         "desc":"2222222",
         "digest":"4387f38fa0ac5e911dd02c32b2beeba6",
         "digestAlg":"",
         "key":" ",
         "keyAlg":" ",
         "timeout":10,
      "url":"http://uhome.haier.net:80/resource/enabling/operation/mgmtUpgradeFileUpload/1558496591300.bin",
         "vers":"1.2.00",
         “size”:111
      },
      "model":"1a2b3c4d5e6f7g8h",
      "vers":"1.0.00"
   },
   "retCode":"00000",
   "retInfo":"成功"
}

```

##### 3、接口错误码
> A00001、B00001、G20202、G00003  


#### 获取设备型号信息

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

##### 1、接口定义
?> **接入地址：** `/stdudse/v1/protected/getBaseInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息

##### 2、请求示例

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/getBaseInfo
Body：
{
  “deviceId”: “DC330DC4B6EE”
}

```

**请求应答**
```
{
  "deviceBaseInfo": {
"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码

> G10001

#### 修改设备型号信息

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

修改设备型号信息，产品编码补习已在海极网注册


##### 1、接口定义
?> **接入地址：** `/stdudse/v1/protected/modifyModelInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id
codeType|Integer|Body|是|1、机器编码；2、产品型号编码
code|String|Body|是|具体编码
modifyType|String|Body|否|修改方式：1、APP扫码更新；2、APP用户选择；3、其他，请描述

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息
possibleList|List<ProductInfo>|Body|否|设备可能的其他型号

##### 2、请求示例

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/modifyModelInfo
Body：
{
  "deviceId": "DC330DC4B6EE",
  "codeType": 1,
  "code": "AURGH4JIOJAVU"
}

```

**请求应答**
```
{
  "deviceBaseInfo": {
"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
"possibleList": [
  ],
  "retCode": "00000",
  "retInfo": "成功"
}

```


##### 3、错误码

> C00003


[^-^]:常用图片注释
[DevicesStandard_flow]:../_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:../_media/_DevicesEnterprise/DeviceE_flow.png

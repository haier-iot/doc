
!> **更新时间**：{docsify-updated}  




### 设备信息管理接口

#### 获取设备别名
> 获取设备aliasName（别名）

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
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
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
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



#### 查询设备是否在线
> 用户可以查看设备是否在线

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/isOnline`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
isOnline|String|Body|必填|状态信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/isOnline
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
    "isonline": "true",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 查询设备信号强度
> 获取设备的信号强度

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceNetQuality`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceNetQuality|DeviceNetQualityDto|Body|必填|设备ID

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/deviceNetQuality
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
    "deviceNetQualityDto":
    {
        "hardwareType":"R",
        "hardwareVers":"1.0.00",
        "softwareType":"UDISCOVERY_UWT",
        "softwareVers":"2.3.12",
        "netType":"Wifi",
        "strength":"100"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006



#### 获取设备最新状态
> 获取到用户最新上报的状态信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/lastReportStatus`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
DeviceStatus|DeviceStatus|Body|必填|设备状态

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/lastReportStatus
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
    "devicestatus":
    {
        "timestamp":"1502854990142",
        "deviceId":"DC330D003121",
        "statuses":
        [
            {"name":"2000ZX","value":""},
            {"name":"623006","value":"323000"}
        ]
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006




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
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|Body|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|Body|是|设备id 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

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
deviceId|String|Body|是|设备id 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
codeType|Integer|Body|是|1、机器编码；2、产品型号编码
code|String|Body|是|具体编码
modifyType|String|Body|否|修改方式：1、规则修改；2、直接修改；无输入或输入为空时默认采用【直接修改】逻辑

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



#### 根据typeId、应用分类、大中类获取符合条件的成品编码列表（所有）

根据typeId、应用分类、大中类获取型号信息，获取完整的型号信息数据


##### 1、接口定义
?> **接入地址：** `/dcs/device-service-2c/get/netDevice/productCodes`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|是|接口版本，此版本为v1.
language|String|Header|是|国际化标识，代表客户端使用的语言。具体标识代码见附录。默认请传 “zh-cn“，代表中文</br>（此条只是为了日后接入国际化标识做准备，当传入的code不支持时一律认为是中文，不传也默认中文）
appTypeCode|String[]|Body|否|成品编码
midCode|String[]|Body|否|中类编码
bigCode|String[]|Body|否|大类编码
typeId|String[]|Body|否|typeId

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|String[]|Body|是|返回数据

##### 2、请求示例

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/netDevice/productCodes
POST
 
data:
{
    "midCode":"02012"
}
[no cookies]
Request Headers:Connection:
keep-alive
appId: ************
appVersion: v1
clientId: test123456
sequenceId: ************
accessToken: ************
sign: ************
timestamp: 1590053835901
language: zh-cn
timezone: +8
Content-Encoding: utf-8
Content-type: application/json


```

**请求应答**
```

{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":[
        "AA9C3Y015",
        "AA9C3Y115",
        "yn",
        "TEST0335",
        "TEST9999",
        "TEST9998",
        "Ytest22",
        "Ytest11中文",
         ...
        "u3x9Haxii"
    ]
}
```



#### 根据成品编码获取型号信息

根据成品编码，获取完整的型号信息数据


##### 1、接口定义
?> **接入地址：** `/dcs/device-service-2c/get/netDevice/info`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|是|接口版本，此版本为v1.
language|String|Header|是|国际化标识，代表客户端使用的语言。具体标识代码见附录。默认请传 “zh-cn“，代表中文</br>（此条只是为了日后接入国际化标识做准备，当传入的code不支持时一律认为是中文，不传也默认中文）
productCodes|String[]|Body|是|成品编码

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|`List<ServiceDeviceInfoResultDto>`|Body|是|返回数据

##### 2、请求示例

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/netDevice/info
POST
 
data:
{
    "productCodes":["21389***1231"]
}
[no cookies]
Request Headers:Connection:
keep-alive
appId: *************
appVersion: v1
clientId: test123456
sequenceId: 20161020153428000015
accessToken: **************
sign: 3**************
timestamp: 1590053835901
language: zh-cn
timezone: +8
Content-Encoding: utf-8
Content-type: application/json


```

**请求应答**
```

{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
        "deviceInfos":[
            {
                "productCode": "21389ade1231",   
                "modelCode": "xxx",
                "typeId": "xxx",
                "productImg1": "https://pic/test/1.png",
                "brandCode":"B01",
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003",
                "brandName":"海尔6666",
                "videoFlag":1
            }
        ],...
}

```


#### 配网信息查询

根据自发现信息或mac查询设备的型号、图标、配置引导信息


##### 1、接口定义
?> **接入地址：** `/dcs/netdeviceinfo/find/deviceInfomation`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
productCode|String|Body|非必填|产品编码
selfDiscoveryInformationCode|String|Body|非必填|1~14位字符串，海极网按型号配置的热点自发现信息</br>（是热点中的NNN那一段，不是完整热点）
mac|String|Body|非必填|设备的mac（不是deviceId，此时设备还未注册）

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|`DeviceInfomation`|Body|是|返回数据





##### 2、请求示例

**请求样例**
```
POST data:

[no cookies]

Request Headers:

Connection: keep-alive

appId: ********************

appVersion: 01.00.00.00000

clientId: ********************

accessToken: ********************

sign: ********************

timestamp: 1585577435707

Content-Encoding: utf-8

Content-type: application/json

Post：

{

  "mac": "string",

  "selfDiscoveryInformationCode": "string",

  "productCode":"String"

}

```

**请求应答**
```

{

  "payload": {

    "apptypeCode": "string",

    "apptypeName": "string",

    "bindFailed": "string",

    "bindFailedImg": "string",

    "bindSuccess": "string",

    "bindSuccessImg": "string",

    "brandCode": "string",

    "configMode": "string",

    "deviceRole": "string",

    "hotspotName": "string",

    "modelCode": "string",

    "productCode": "string",

    "productDesc": "string",

    "productImg1": "string",

    "productImg2": "string",

    "customHotspotCode ": "string",

"customHotspotName": "string",

"state": "string",

    "step1": "string",

    "step1Img": "string",

    "step2": "string",

    "step2Img": "string",

    "step3": "string",

    "step3Img": "string",

    "step4": "string",

    "step4Img": "string",

    "step5": "string",

    "step5Img": "string",

    "typeId": "string"

  },

  "retCode": "string",

  "retInfo": "string"

}

```



#### 根据DeviceId查询设备绑定状态

根据DeviceId和token查询设备绑定状态，如果用户和设备有绑定关系则返回绑定时间，反之返回错误码(1900001: 当前用户与该设备不匹配);


##### 1、接口定义
?> **接入地址：** `/dcs/device-service-2c/get/device/user/binding/status`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备身份标识
accessToken|String|Header|是|用户token信息

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|ServiceDeviceBindStatusResultDto|Body|是|1.用户与设备有绑定关系, 返回bts 2.用户与设备没有绑定关系，返回错误码1900001


##### 2、请求示例

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/device/user/binding/status
data:
{
    "deviceId":"DC330D861B7C"
}
[no cookies]
Request Headers:Connection:
keep-alive
appId: *********
apiVersion: v1
clientId: test123456
sequenceId: 20161020153428000015
accessToken: ***************
sign: 314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
timestamp: 1590053835901
language: zh-cn
timezone: +8
Content-Encoding: utf-8
Content-type: application/json




```

**请求应答**
```
{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
        "bts": 1604570483000
    }
}


```



#### 查询设备网络质量

根据DeviceId和token查询设备网络质量;


##### 1、接口定义
?> **接入地址：** `/dcs/device-service-2c/get/device/net/quality`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备身份标识
accessToken|String|Header|是|用户token信息
apiVersion|String|Header|是|接口版本，此版本为v1.

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|DevNetQuality|Body|是|返回数据


##### 2、请求示例

**请求样例**
```
POST URL:  https://uws.haier.net/dcs/device-service-2c/get/device/net/quality

POST data:

{
    "deviceId":"DC330D861B7C"
}

[no cookies]



Request Headers:

Connection: keep-alive

appId: **********

appVersion: v1

clientId: test123456

sequenceId: *******

accessToken: **********

sign: 314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4

timestamp: 1590053835901

language: zh-cn

timezone: +8

Content-Encoding: utf-8

Content-type: application/json


```

**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":{
          "machineId":"DC330D861B7C",

	  "isOnLine":true,
	
	  "netType":"Wifi",
	  "statusLastChangeTime ":159343991566,
	
	  "moduleVersion ":"2.3.30/eSDK_WIFI_AC/3.0.00/R",
	
	  "ssid":"UHOME-TEST-WIFI",
	
	  "rssi":-38,
	
	   "pssi":72,
	
	  "signalLevel ":2,
	
	  "ilostRatio":20,
	
	  "its":8

}

}



```



#### 根据DeviceId查询拓扑关系

根据DeviceId和token查询设备拓扑关系，如果用户对该设备有权限继续查询，反之返回错误码(1200001: 当前用户与该设备不匹配);


##### 1、接口定义
?> **接入地址：** `/dcs/device-service-2c/get/device/topological/relation`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备身份标识
accessToken|String|Header|是|用户token信息
apiVersion|String|Header|是|接口版本，此版本为v1.

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Map<String,List<ServiceDeviceTopologicalResultDto>|Body|是|用户对该设备没有权限，返回错误码1200001<br/>查询的设备不在线，返回错误码1200002<br/>查询到的拓扑关系列表中，(1)如果设备离线则该条记录不返回; (2)如果用户对设备无操作权限，只返回desc,其中desc值为"未知设备",其他字段不返回。<br/>具体返回结构见5.1 请求示例返回返回结构中，key:  father 表示该设备的父设备  child表示该设备的子设备


##### 2、请求示例

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/device/topological/relation
data:
{
    "deviceId":"DC330D861B7C"

}
[no cookies]
Request Headers:Connection:
keep-alive
appId: ************************
apiVersion: v1
clientId: test123456
sequenceId: 20161020153428000015
accessToken: ************************
sign: 314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
timestamp: 1590053835901
language: zh-cn
timezone: +8
Content-Encoding: utf-8
Content-type: application/json

```

**请求应答**
```
{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
    "father": [
      {
        "deviceId": "deviceId1",
        "familyName": "familyName1",
        "productCode": "productCode",
        "typeId": "typeId",
        "productImg": "productImg"
      },
     {
       "desc": "未知设备"
     }
  ],
  "child": [
    {
      "deviceId": "deviceId1",
      "familyName": "familyName1",
      "productCode": "productCode",
      "typeId": "typeId",
      "productImg": "productImg"
    },
    {
      "desc": "未知设备"
    }
  ]
 }

}


```



[^-^]:常用图片注释
[DevicesStandard_flow]:../_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:../_media/_DevicesEnterprise/DeviceE_flow.png

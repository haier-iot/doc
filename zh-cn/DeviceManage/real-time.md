
!> **更新时间**：{docsify-updated}  



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
[DevicesStandard_flow]:_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:_media/_DevicesEnterprise/DeviceE_flow.png

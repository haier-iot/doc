
!> **更新时间**：{docsify-updated}  


## 简介

家庭模型服务实现的是一种以“家庭”为单位的智能互联设备共享组，使用此服务可以建立“家庭”组，在组下管理家庭成员及相关消息。

服务提供了创建/解散家庭、添加/删除家庭成员、邀请加入家庭及相关查询功能。可以在家庭组中进行设备共享与场景公用等操作。家庭成员（包括家庭创建者）可将自有设备分享到家庭，分享到家庭的设备，家庭成员自动具有控制权限，包含查看权，配置权，控制权等。

### 家庭管理流程


![家庭模型流程][family_flow]


### 规则与约束

1、支持用户建立/解散一个或多个家庭，同时支持用户不建立家庭。<br/>
2、支持且仅支持管理员邀请/删除家庭成员。<br/>
3、支持成员加入/主动退出家庭，成员加入家庭需要家庭管理员确认。<br/>
4、不存在设备与家庭的关系，只存在设备与人的关系，人可分享设备的相关权限至家庭和个人。<br/>
5、用户的设备只可以在一个家庭中共享，不可以在多个家庭中共享，用户可选择要共享的家庭。<br/>
6、平台支持用户权限分级别：设备绑定权，设备控制权，设备信息查看权，为便携设备的绑定留足扩展空间，各APP可自行决定是否呈现给用户。<br/>
7、当用户各自带着设备进入一个家庭时，设备的相关权限可以共享，当用户离开的时候，各自带着各自设备离开，分享的相关权限解除。<br/>
8、基于扩展性考虑，平台家庭模型不做过多限制，但APP呈现端可做多种限制 。<br/>
9、用户角色：(1).用户绑定设备，成为"设备管理员" (2).自己创建家庭为"家庭管理员"(3).加入到他人家庭为"家庭成员"
10、创建家庭数限制：一个用户最多是20个家庭的管理员（ps：默认创建家庭用户成为家庭管理员，当管理员移交角色后，可重新创建家庭。）<br/>
11、加入家庭数限制：一个用户最多可加入50个家庭，即最多是50个家庭的成员（不含管理员）。<br/>
12、一个用户最多在70个家庭中。<br/>
13、家庭资源数限制：<br/>
	（1）设备数：一个家庭下最多加入500个设备。<br/>
	（2）成员数：一个家庭下最多50个成员，含管理员用户。<br/>
	（3）虚拟成员数：一个家庭下最多20个虚拟成员。


## 接口公共部分

### AuthInfo  
描述权限内容信息结构。   
  
| **名称** | 权限内容信息，至少一项为true |&emsp;| AuthInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|view| Boolean | 是否有查看权 ||  
|set| Boolean | 是否有配置权限 ||  
|control| Boolean | 是否有控制权 |&emsp;|    

### QueryInfo  
描述查询条件的接口。  
 
| **名称** | 查询条件对象 | &emsp; | QueryInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|queryKey|String|查询关键字|mobile：手机，email：邮箱|  
|queryValue|String|关键字值|根据queryKey确定值|  
|accType|String|账号类型|&emsp;|  

### QueryUserInfoResult   
查询用户信息结果。  
     
| **名称** | 查询用户信息结果对象 |&emsp;| QueryUserInfoResult |
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|mobile|String|用户手机号|可能为空，由用户信息是否完整决定|  
|email|String|电子邮件|可能为空，由用户信息是否完整决定|  
|userId|String|临时分配用户userid|不为空|  
|loginName|String|用户登录名（ids注册的登录名）|不为空|  

### Permission  
描述权限信息结构。  

| **名称** | 权限信息 |&emsp;| Permission |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|  
|auth|AuthInfo|权限内容||  
|authType|String|权限类型|home:家庭分享，share：个人分享，owner：设备主人，server：给appserver的权限|  


### ShareDevice   
描述设备的共享信息内容,包含设备分享到家庭的id,设备的属主,设备名字,分享权限集。 
  
| **名称** | 设备共享信息 | &emsp;|ShareDevice |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|devInfo|DeviceBriefInfo|设备简明信息||  
|devName|String|设备名称||  
|devShareUser|UserBriefInfo|分享用户简明信息||  
|devFamilyId|String|设备所属家庭Id||  
|devOwner|UserBriefInfo|设备管理员简明信息||  
|permission|Permision|权限|&emsp;| 
|devRoomId|String|设备所属房间ID||  
|devRoomName|String|设备房间名称|&emsp;|  

### FamilyInfo    
描述家庭信息,包含家庭主人,家庭创建时间,家庭名称。     
  
| **名称** | 家庭信息 | &emsp;|FamilyInfo |    
| ------------- |:-------------:|:-----:|:-------------:|    
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型|长度19，添加请求时不填|      
|familyName|String|家庭名称| 不超过64位|    
|familyOwner|UserBriefInfo|家庭管理员用户简明信息|添加请求时不填| 
|ownerName|String|家庭管理员用户简明信息|最长不超过32|  
|familyLable|String|家庭标签|APP定义，如父母等|  
|familyDesc|String|家庭描述| 不超过1K|  
|appId|String|应用Id| |  
|createtime|date|家庭建时间|&emsp;|  
|familyLogo|String|家庭logo|默认值为平台内置家庭logo url，不超过1K|  
|familyPicture|String|家庭图片|默认值为平台内置家庭图片 url，不超过1K|  
|familyLocation|Location|家庭位置信息|家庭位置信息|  
|familyPosition|String|家庭位置|小区等信息|   
|familyExternData|String|扩展信息|IOT平台可定义，json|  
|familyLastUpdater|String|家庭最后修改人|添加请求时不填|  
|LastUpdateTime|date|家庭最后修改时间|精确到秒，含年月日信息，，添加操作请求时不填|  
|securityLevel|int|安全级别|添加操作请求时不填|  
|deviceCount|int|设备数量|添加操作请求时不填|    
|memberCount|int|成员数量|添加操作请求时不填|    

### FamilyInfoOwnerName   
描述家庭信息,包含家庭主人,家庭创建时间,家庭名称。  
  
| **名称** | 家庭信息,包含管理员的昵称 | &emsp;|FamilyInfoOwnerName |  
| ------------- |:-------------:|:-----:|:-------------:|    
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型|长度19|      
|familyName|String|家庭名称||    
|familyOwner|UserBriefInfo|家庭管理员用户简明信息||    
|ownerName|String|家庭管理员用户简明信息||  
|appId|String|应用Id|&emsp;|  
|createtime|date|家庭创建时间|&emsp;|  

### RoomInfo  
描述房间信息   
 
|**名称**|描述房间信息 |&emsp;|RoomInfo|      
| ------------- |:-------------:|:-----:|:-------------:|
|**字段名**|**类型**|**说明**|**备注**| 
|roomName|String|房间名称||
|roomId|String|房间ID||
|familyId|String|房间所属家庭ID||
|roomClass|String|房间类型||
|roomLabel|String|房间标签||
|roomCreater|UserBriefInfo|房间添加人||
|roomLogo|String|房间logo url||
|roomPicture|String|房间图片 url||
|roomExternData|String|扩展信息，使用json格式||
|roomCreateTime|Date|房间创建时间||
|fromAppid|String|来源APPID或者systemId||
|lastUpdateTime|Date|房间最后修改时间||
|lastUpdateUser|UserBriefInfo|房间最后编辑用户信息|&emsp;|

### FamilyMemberInfo    
描述家庭成员信息,包含家庭成员id,成员名称,所属家庭id。  
  
| **名称** |家庭信息 | &emsp;|FamilyMemberInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|memberInfo|UserBriefInfo|用户简明信息||    
|memberName|String|用户在家庭中的昵称||    
|familyId|String|家庭id||    
|joinTime|String|加入家庭时间|格式：`YYYY-MM-DD HH:mm:ss`|   

### DeviceBriefInfo    
| **名称** | 设备简明信息 | &emsp;|DeviceBriefInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|deviceName|String|设备名称，等同于别名|&emsp;|  
|deviceId|String|设备ID|&emsp;|  
|wifiType|String|设备wifitype|&emsp;|  
|deviceType|String|设备类别|&emsp;| 
|online|Boolean|是否在线|&emsp;|     
|productName|String|设备型号名称|&emsp;|  
|productCode|String|设备型号代码|&emsp;|  


### UserBriefInfo    
Map<String,String> 用户属性值key/value  
下表为存在的key,其他key属性需要忽略

| **名称** | 用户简明信息 | &emsp;|UserBriefInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|name|	String|	昵称|&emsp;|	
|email|	String|	电子邮件|&emsp;|	
|userId|	String|	用户id|	不为空|
|mobile|	String|	手机号|	&emsp;|
|avatarUrl|	String|	头像url|	用户中心提供的头像地址|
|isVirtualUser|	String|	是否为虚拟用户|	true，虚拟用户，false，实体用户|
|hostUserId|	String|	2.7.1.6	FamilyInfo宿主用户的IOT平台userid|	&emsp;|
|ucUserId|	String|	用户中心userId|&emsp;|	



### Location 
|**名称**	|模块信息 |&emsp;|Location|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|longitude|Double|经度||  
|latitude|Double|维度||  
|cityCode|String|城市编码|&emsp;|       


### QueryClause 
|**名称**	|查询条件 |&emsp;|QueryClause|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|queryInfo |Map<String,String>|查询条件信息|&emsp;|   



### FamilyShareDevice

描述设备诶的共享信息内容，包含设备分享到家庭的id、设备主人、设备名称、  
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
deviceId|String|设备ID|
devName|String|设备名称|
devOwner|String|设备主人|
devFamilyId|String|设备所属家庭Id|
permission|Permission|权限|   




### DeviceRoomInfoDto  

  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifitype|
deviceType|String|设备类别|
room|String|设备房间位置信息|   
permission|Permission[]|权限信息|   
online|Boolean|是否在线|
productNameT|String|设备型号名称| 
productCodeT|String|设备型号代码|    



### FamilyQRCodeInfo    

家庭模型快速响应码  
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
shareUrl|String|分享url|
event|String|快速响应码要处理的事件|
familyId|String|家庭ID|
timeout|int|按秒计算，为二维码有效期|
params|String|附加参数，数字字母最长64位|   


## 家庭管理服务接口清单  


#### 家庭管理员创建家庭
> 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/family `  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|

**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

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
{"familyInfo": {"familyName": "科技"}}


```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "科技",
        "familyOwner": {
            "userId": "100013957366154693",
            "mobile": "136****8934"
        },
        "appId": "MB-***-0009",
        "createTime": "2019-02-26 11:27:42",
        "familyLocation": {},
        "securityLevel": "0",
        "deviceCount": "0",
        "memberCount": "0"
    }
}



```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员创建家庭带管理员昵称
> 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/familyWithOwnerName `  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名   | 位置  | 必填|说明|
| ---- |:-------:|:-----:|:------:|:------:|
|  FamilyInfoWithOwnerName   | familyInfo | Body| 必填|家庭信息|

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfoWithOwnerName |  familyInfo  |   Body  |  必填  | 家庭信息 |

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
"familyInfo":{
"familyName":"我家"，
"ownerName":"admin100013957366154693"
	}
}



```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功","familyInfo":{"familyId":"566070358640000000","familyName":"我的家庭1","familyOwner":{"userId":"100013957366154693","mobile":"136****8934"},"ownerName":"admin100013957366154693","appId":"MB-UBOT-0009","createTime":"2018-01-03 16:17:20"}}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员修改家庭名称
> 用户修改拥有的家庭的名称，发送修改家庭信息消息给家庭成员,记录发送结果到日志    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family `  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:--------:|:--------:|
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|  
|  String    | familyId | url| 必填|家庭id|  

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
"familyInfo":{
"familyName":"你家"
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108   

#### 家庭管理员或家庭成员查询家庭信息
> 家庭管理员或家庭成员查询家庭信息    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family `  
 **HTTP Method：** GET

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | url| 必填|家庭id|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

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
"familyInfo":{
"familyName":"我家",
"familyId":"10086",
"familyOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
},
"appId":"MB-PORTAL-0000"
}
   "retCode": "00000",
   "retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109   

#### 家庭管理员查询创建家庭信息列表
> 验证用户token,确认用户是否为家庭主人,查询家庭信息,并返回    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/createdFamilies `  
 **HTTP Method：** GET

**输入参数**  

| 类型  | 参数名 | 位置  | 必填|说明|
| ---- |:------:|:-----:|:-------:|:-------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo[] |  families  |   Body  |  必填  | 家庭信息列表 |

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
"families":[{
"familyName":"我家",
"familyId":"10005618",
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"
},
{
"familyName":"你家",
"familyId":"1000569",
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"
}]
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

#### 家庭成员查询加入的家庭信息列表
> 验证用户token,确认用户是否为家庭成员,查询加入的家庭信息,并返回    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/families?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型      | 参数名   | 位置  | 必填|说明|
| -------- |:--------:|:-----:|:---------:|:---------:|  
|String	|pageNumber|	url|	否	|当前访问信息的起始页，从1开始|
|String	|pageSize	|url|	否	|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理| 

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo[] |  families  |   Body  |  必填  | 家庭信息列表 |
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
    "retCode": "00000",
    "retInfo": "成功",
    "families": [
        {
            "familyId": "755111162274000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-15 17:17:55",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "1",
            "memberCount": "2"
        },
        {
            "familyId": "880111442716000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-18 11:51:57",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "0",
            "memberCount": "2"
        },
        {
            "familyId": "115111466664000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-18 18:31:05",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "0",
            "memberCount": "2"
        }
]，
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员删除家庭
> 家庭管理员删除家庭,收回用户分享给家庭的设备权限,收回家庭成员的家庭分享设备权限，向删除前家庭成员发送删除家庭消息，记录消息推送结果到日志   
/ufm/v1/protected/shareDeviceService/ person/shareDevices
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名    | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url | 必填 |家庭id |  

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
```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108    

#### 家庭管理员添加家庭成员
> 家庭管理员添加家庭成员,分享家庭设备权限给成员，发送家庭成员添加家庭成员消息,支持memberId为临时的userid  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | memberId   | Body |必填|家庭成员id |  
| String  | memberName   | Body |必填|家庭成员名称 |  
| String  | familyId   | Body |必填|家庭id |  
  

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
"familyId":"105876",
"memberId":"10008573",
"memberName":"老大"
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31105、E31108  

#### 家庭管理员邀请用户加入家庭
> 家庭管理员邀请用户加入家庭，并向用户下发推送邀请码, 支持targetId为临时的userid,邀请生成正确,推送失败时,返回给发起方家庭ID和邀请码。

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/{familyId}/{targetId}/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | targetId   | url |必填|被邀请用户id |  
| String  | familyId   | url |必填|家庭id |  
| String  | familyId  | Body |必填|家庭成员在家庭中名称 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| String |  familyId  |   body |  必填  | 家庭id|
| String |  inviteCode  |   body |  必填  | 用户邀请码|  

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
"memberName": "大家长"
}

```  

**请求应答**

```java
{
"familyId": "11389",
"inviteCode": "123034",
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、H00001  
    
#### 家庭成员回复邀请加入家庭
> 家庭成员接受家庭管理员邀请，加入家庭，,分享家庭设备权限给成员，并发送添加家庭成员消息给家庭成员；如果拒绝，本次邀请结束，发送用户拒绝加入家庭消息到家庭管理员，记录消息推送结果到日志  


##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/agreeInvitation/{familyId}/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId   | url |必填|家庭id|  
| String  | inviteCode   | body |必填|邀请码 |  
| String  | memberName  | Body |必填|家庭成员在家庭中名称 | 
| Boolean  | agree  | Body |必填|true，同意，false不同意 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


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
"inviteCode": "123034",
"agree": true
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、E31131、E31132、E31133    

#### 家庭管理员或家庭成员修改家庭成员名称
> 家庭管理员修改家庭成员信息，其中包含家庭管理员在家庭中的昵称，家庭成员可以修改自己的信息，发送修改家庭成员信息消息给家庭成员，记录消息推送结果到日志  
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/{memberId}/familyMember`  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| FamilyMemberInfo  | memberInfo   | Body |必填|家庭成员信息|  
| String  | familyId   | url |必填|家庭id |  
| String  | memberId  | url |必填|家庭成员id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


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
"memberInfo":
{
"memberName":"老大"
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109  

#### 家庭管理员删除家庭成员
> 家庭主人删除家庭成员,并解除成员在家庭中分享的设备关系，并收回成员分享给家庭的设备，发送删除家庭成员消息给家庭全体成员，记录消息推送结果到日志 
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/{memberId}/familyMember`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | memberId   | url |必填|用户id |  
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31107、E31108  


#### 家庭成员主动退出家庭
> 家庭成员主动退出家庭,撤销家庭成员的家庭设备分享,撤销其他家庭成员对本家庭成员设备的分享权限，向家庭其他成员发送删除家庭成员消息  
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/familyMember`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108   

#### 家庭管理员或家庭成员查询家庭成员
>家庭管理员或家庭成员查询家庭成员  
 /ufm/v1/protected/familyService/families
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/familyMembers?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
|String	|pageNumber|	url|	否	|当前访问信息的起始页，从1开始|
|String	|pageSize	|url|	否	|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|   

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |
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

"familyMembers":[
{"familyId":"105876",
"memberInfo":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"memberName":"测试1","joinTime ":"2016-11-01 00:00:00"},
{"familyId":"105876",
"memberInfo":{
"userId: "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
"userSex": "male"
},
"memberName":"测试2""joinTime ":"2016-11-01 00:00:00"},
{"familyId":"105876",
"memberInfo":{
"userId: "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
"userSex": "male"
},
"memberName":"测试3""joinTime ":"2016-11-01 00:00:00"}
],
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
"retCode":"00000",
"retInfo":"成功"

}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109     

#### 家庭成员或家庭管理员查询家庭成员所有成员（包含管理员）
>家庭管理员或家庭成员查询家庭成员 
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/allFamilyMembers?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
|String	|pageNumber|	url|	否	|当前访问信息的起始页，从1开始|
|String	|pageSize	|url|	否	|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|   

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |
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

{"retCode":"00000","retInfo":"成功","familyMembers":[{"memberInfo":{"userId":"100013957366154693","mobile":"136****8934"},"memberName":"admin100013957366154693","familyId":"566070358640000000","joinTime":"2018-01-03 16:17:20"}]}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109      

#### 根据关键字精确检索好友信息
>精确查找用户信息，用于执行需要用户ID的场景，本次用户id有时效性，临时分配，有效期为1天，支持其他接口使用，在相关接口中有说明,同时屏蔽用户敏感信息,包含手机号,邮箱,登录名。
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/userInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| QueryInfo  | queryInfo  | body |必填|查询条件 | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| QueryUserInfoResult |  qUserR   |   Body  |  必填   | 用户信息 |


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
"queryInfo":{
"queryKey":"mobile",
"queryValue":"13800000000"，
"accType":"0"，
}
}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功","qUserInfo":{"userId":"1566070351894000000","mobile":"13621108934","email":"chenj@haierubic.com","loginName":"H642R1510027712631"}}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31301


#### 家庭管理员变更
>家庭管理员可以主动移交管理员角色，变更时，只能变更给当前家庭下其他家庭成员；变更完成时，家庭管理员变为家庭普通成员
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/changeAdmin`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| familyId  | String  | body |必填|家庭id  | 
| newFamilyOwner  | String  | body |必填|新家庭管理员的userId  |   

**输出参数**  

标准输出


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
{"familyId":"647111777513000000","newFamilyOwner":"100013957366152179"}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、F31214

#### 查询家庭下的房间列表
>查询家庭下的房间列表信息
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/roomInfoList`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| QueryClause  | queryInfo  | body |必填|查询条件  | 

**输入参数对象说明**  

|**名称**	|查询条件 |&emsp;|QueryClause|  
| ------ |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|familyId|String|家庭id |&emsp;|  
 

**输出参数**  


| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| RoomInfo[]  | roomInfos  | body |必填|用户信息  | 



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
"queryInfo":{
"familyId":"11111111"
}
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"roomInfos": [{
		"roomName": "testroorm1",
		"roomId": "150111135560000000",
		"familyId": "878111118405000000",
		"roomCreateTime": "2019-02-15 09:52:40"
	}]
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31301


#### 获取家庭二维码参数

> 家庭管理员或家庭成员获取家庭二维码参数，获取加入家庭的二维码参数

##### 1、接口定义

?> **接入地址：** `/ufm/v1/protected/familyService/family/QRCode`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
FamilyQRCodeInfo|QRCode|body|是|

**家庭模型快速响应码**

字段名|类型|说明|备注
:-:|:-:|:-:|:-
shareUrl|String|分享url|非必填，不传或者传空传空字符，返回默认url，默认值为`http://uplusapp.cn/U/0005H`
event|String|快速响应码要处理的事件|非必填，目前只支持`1uplus://joinFamily`其他事件返回参数错误，可不填，当event为空或者空字符时使用默认值`uplus://joinFamily`
familyId|String|家庭ID|必填
timeout|int|按秒计算，为二维码有效期|必填
params|String|附加参数，数字字母最长64位|非必填

**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
String|familyId|body|必填|家庭id
String|qrcode|body|必填|二位码url


##### 2、请求样例
**用户请求**
```
{ 
	"familyId":"647112241261000000",
	"timeout":30
}

```

**请求应答**
```
  {
	"retCode": "00000",
	"retInfo": "成功",
	"familyId": "647112241261000000",
	"qrcode": "http://uplusapp.cn/U/0005H?token=748dae0417fe4361b7ff882785b4f021&content=uplus://joinFamily/748dae0417fe4361b7ff882785b4f021",
	"expiresTime": "2019-06-01 11:47:11"
}

```

##### 3、错误码

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31137



#### 使用二维码加入家庭

> 用户使用App扫描二维码后由app处理参数后，本接口执行加入家庭，如果用户已经是家庭成员或者家庭管理员，返回E31105，提示用户已经是家庭成员。

##### 1、接口定义

?> **接入地址：** `/ufm/v1/protected/familyService/family/QRCode/param`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String|familyQRCode|Body|必填|用户通过接口“获取家庭二维码参数”获取的qrcode中，token的值
userFamilyName|用户在家庭中的名称|Body|必填|用户加入家庭附属条件

**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
String|familyId|body|必填|家庭id


##### 2、请求样例
**用户请求**
```
{
	"familyQRCode":"748dae0417fe4361b7ff882785b4f021",
	"userFamilyName":"cindy"
}

```

**输出参数**

```
{
   "familyId": "647112241261000000",
    "retCode": "00000",
	"retInfo": "成功"
}
```

##### 3、错误码

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138



#### 虚拟用户加入家庭

> 实体账户作为家庭成员添加虚拟用户进入家庭，实体账户必须为虚拟账户的宿主账户

##### 1、接口定义

?> **接入地址：** `/ufm/v1/protected/familyService/joinFamily/virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String|	virtualUCId|	Body|	必填	虚拟用户用户中心ID
String| 	familyId|	Body|	必填	要加入的家庭ID
String|	userFamilyName|	Body|	必填	用户加入家庭附属参数,为用户在家庭中昵称


**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
String|	virtualUserId|	Body|	必填|	用户在iot平台分配的userId


##### 2、请求样例
**用户请求**
```
{
	"familyId":"444130753006000000",
	"userFamilyName":"test",
	"virtualUCId":"237"
}



```

**输出参数**

```
{
"retCode":"00000",
"retInfo":"成功",
"familyId":"164131078929000000",
"virtualUserId":"100013957366167109"
}


```

##### 3、错误码

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408、E31409


#### 虚拟用户退出家庭

> 实体账户必须为虚拟账户的宿主账户，实体账户协助虚拟账户退出家庭

##### 1、接口定义

?> **接入地址：** `/ufm/v1/protected/familyService/leaveFamily/virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String|	virtualUserId|	Body|	必填|	虚拟用户在IOT平台的用户ID
String| 	familyId|	Body|	必填	|要加入的家庭ID



**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-



##### 2、请求样例
**用户请求**
```
{ 
"familyId":"164131078929000000",
"virtualUserId":"100013957366167109"
}	




```

**输出参数**

```
{
    "retCode": "00000",
	"retInfo": "成功"
}


```

##### 3、错误码

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408


#### 查询虚拟成员所在家庭

> 实体账户查询虚拟用户所在的家庭

##### 1、接口定义

?> **接入地址：** `/ufm/v1/protected/familyService/queryFamily /virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String|	virtualUserId|	Body|	必填	|虚拟用户在IOT平台的用户ID



**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
FamilyInfo[]|	families|	Body|	必填	|家庭信息


##### 2、请求样例
**用户请求**
```
{
"virtualUserId":"1111111111111"
}


```

**输出参数**

```
{
	"retCode": "00000",
	"retInfo": "成功",
	"families": [{
		"familyId": "693130509064000000",
		"familyName": "Aalitest",
		"familyOwner": {
			"isVirtualUser": "false",
			"email": "",
			"name": "187****6123",
			"userId": "100013957366158663",
			"ucUserId": "2005021119",
			"avatar": "https://account.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
			"mobile": "18730000000"
		},
		"appId": "MB-UBOT-0009",
		"createTime": "2019-08-28 02:31:04",
		"familyLocation": {},
		"securityLevel": "0",
		"deviceCount": "0",
		"memberCount": "3"
	}, {
		"familyId": "693130508700000000",
		"familyName": "我的家庭",
		"familyOwner": {
			"isVirtualUser": "false",
			"email": "",
			"name": "187****6123",
			"userId": "100013957366158663",
			"ucUserId": "2005021119",
			"avatar": "https://account.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
			"mobile": "1873000000"
		},
		"appId": "MB-UBOT-0009",
		"createTime": "2019-08-28 02:25:01",
		"familyLocation": {},
		"securityLevel": "0",
		"deviceCount": "0",
		"memberCount": "4"
	}
],
	"totalCount": "2"
}


```

##### 3、错误码

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408


















[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
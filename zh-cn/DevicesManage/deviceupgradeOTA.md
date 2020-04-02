
!>  **更新时间**：{docsify-updated}  

## 设备升级操作手册



**设备升级流程**  

![设备升级流程图][pic1]

## 1.注册账号

1)	注册海极网个人账号，地址: 
	https://www.haigeek.com/developercenter/static/develop.html#/system/reg/
2)	开通海极网权限，登录BPM系统： 
	https://ssosci.haier.net:444/authority/home.jsp，搜索“海极网”，填写申请信息，直线经理审批


## 2.加入企业

1)	联系产线管理员，加入企业

2)	设置权限
由管理员根据操作权限分配角色

![流程][pic2]

权限详情请参考如下表格：

![权限详情][pic3]


## 3.创建产品

在第1步创建功能集，并在第3步进行型号适配


## 4.开通服务

在第5步“拓展功能”中开通设备升级服务，并启用具体型号

![开通服务1][pic4]
![开通服务2][pic5]


## 5.固件管理

创建固件，输入固件相关信息，上传固件包
<font color='red'> 注：只有在“拓展功能”中开通了设备升级服务的型号，才能创建固件 </font>

![固件管理][pic6]

## 6.创建升级任务

创建任务，填写任务相关信息
<font color='red'>
注：
1.	只有创建了固件的型号，才能创建升级任务
2.	仅升级适用版本：仅升级和当前版本不相同的版本；仅升级适用版本的设备ID：仅升级适用版本下指定的设备ID 
3.	通知接收人邮箱：升级结果将通过邮件通知给接收人
 </font>

![创建升级任务1][pic7]
![创建升级任务2][pic8]

## 7.灰度发布

升级任务创建完成后，到第2步“灰度发布”，输入发布范围，进行小范围灰度发布。
点击“灰度发布”，显示灰度发布结果。根据结果选择“返回修改”或“发布完成”。
点击“发布完成”，上传见证性资料，提交审核
<font color='red'>
注：灰度发布的发布范围仅支持按适用版本的设备ID升级
 </font>

![灰度发布1][pic9]
![灰度发布2][pic10]


## 8.升级策略

提交审核后，由“运营者”角色录入APP升级文案，APP升级注意事项

![升级策略][pic11]


## 9.任务发布
由管理员进行任务审核，确认信息无误后，选择“通过”，点击“确认并启动升级”
如果信息有误，选择“不通过”，点击“确认”，返回任务列表

![任务发布1][pic12]
![任务发布2][pic13]



> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 预约定时新增     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量新增  |适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 预约定时删除     | 将指定任务ID的预约定时设置为删除状态 | 是| 无|
| 预约定时修改     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量修改 | 适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 根据userid查询预约| 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约） | 是| 无|
| 根据deviceid查询预约| 根据deviceid查询预约详情 | 是| 无|
| 根据taskId查询预约| 根据taskid查询预约详情 | 是| 无|
| 预约定时执行日志查询| 预约定时执行日志查询 | 是| 无|
| 预约任务执行预览| 任务立即下发一次 | 是| 无|



### 预约定时服务接口

#### 预约定时新增

> 预约定时新增，适用于单设备单任务场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/add   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| taskInfo     | TaskInfo | Body| 必填|任务信息|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  detailInfo  |  &emsp;  |   Body   | 必填| 返回任务id(taskId) |

**输入参数TaskInfo对象说明:**

| 参数名        | 类型          | 必填|说明|
| ------------- |:-------------:|:-------------:|:-----:|
|deviceId|	String varchar(50)|	必填	|设备id| 
|schedulerType|	int|	必填	|预约类型（1=设备），此版本仅支持设备预约|
|typeId	|String varchar(100)|	必填|	设备typeId|
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo|	必填	|多套指令集；当前版本只支持一套|
|cron	|Cron[]|	选填|	任务执行表达式；cron和intervals必填其一|
|intervals	|int|	选填|	任务执行距当前的间隔时间，以分钟为单位，限制一天以（0-1440），如为0需要立即执行；cron和intervals必填其一。|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskSeq	|int	|选填|	子任务序号id；默认值为1|

##### 2、请求样例  

**用户请求**
```java  
Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: ****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: *********
sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
timestamp: 1491013448628 
language: zh-cn
timezone: +8
appKey: **********
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "status": 0,
  "deviceId": "DC330D000023",
  "schedulerType": 1,
  "typeId": "edd032ff-7113-4970-a780-392d44c423b3",
  "cron": [
    {
	  "seconds ": "*",
      "minutes": "0 / 5",
      "hours": "*",
      "day": " * ",
      "month": "*",
      "week": "?",
      "year": ""
    }
  ],
  "endTime": "2018 - 05 - 21 12: 00: 00",
  "argsInfo": {
    "cmdMsgList": [
      {
        "index": 0,
        "delaySeconds": 0,
        "name": "onOffStatus",
        "value": "true"
      }
    ],
    "backUrl": "http://*********"
  },
  "taskName": "空调开机",
  "taskDesc": "空调开机",
  "taskSeq": "1"
}


```  

**请求应答**

```java
{
    "retCode": "00000",
"retInfo": "预约定时创建成功",
"detailInfo": {
     “taskId”: “878442f1550c4c189c5307873ca9b1dd”
}
}

```

#### 预约定时批量新增

> 预约定时批量新增，适应于单设备批量定时和批量设备单定时场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

###### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/group/add   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| taskInfos     | List<TaskInfo> | Body| 必填|批量任务添加；list长度上限为50，超出则分批次调用；|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  detailInfo  |  &emsp;  |   Body   | 必填| 返回任务id(taskId) |

**输入参数TaskInfo对象说明:**

| 参数名        | 类型          | 必填|说明|
| ------------- |:-------------:|:-------------:|:-----:|
|deviceId|	String varchar(50)|	必填	|设备id| 
|schedulerType|	int|	必填	|预约类型（1=设备），此版本仅支持设备预约|
|typeId	|String varchar(100)|	必填|	设备typeId|
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo|	必填	|多套指令集；当前版本只支持一套|
|cron	|Cron[]|	选填|	任务执行表达式；cron和intervals必填其一|
|intervals	|int|	选填|	任务执行距当前的间隔时间，以分钟为单位，限制一天以（0-1440），如为0需要立即执行；cron和intervals必填其一。|
|endTime	|dateTime|	选填|	任务终止时间；不填默认值三个月有效期；如果有值，按照此值的有效期|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskId	|String varchar(50)	|选填|	任务id;在分次批量添加任务时，第二次此字段为必填，表示和上批次对应|
|taskSeq|	int|	必填	|子任务序号id；每组中的此字段不能重复；从1开始的正整数；|

##### 2、请求样例  

**用户请求**
```java  
Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: ****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: **************
sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
timestamp: 1491013448628 
language: zh-cn
timezone: +8
appKey: ***********
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "taskInfos": [
    {
      "status": 0,
      "deviceId": "DC330D003121",
      "schedulerType": 1,
      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
      "intervals": 60,
      "endTime": "2019-07-31 16:00:00",
      "argsInfo": {
        "backUrl": "http://*********",
        "cmdMsgList": [
          {
            "index": 0,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          },
          {
            "index": 1,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          }
        ]
      },
      "taskName": "批量关机",
      "taskDesc": "批量关机",
      "taskSeq": "1"
    },
    {
      "status": 0,
      "deviceId": "DC330D003121",
      "schedulerType": 1,
      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
      "intervals": 60,
      "endTime": "2019-07-31 13:00:00",
      "argsInfo": {
        "backUrl": "http://*********",
        "cmdMsgList": [
          {
            "index": 0,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          },
          {
            "index": 1,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          }
        ]
      },
      "taskName": "批量关机2",
      "taskDesc": "批量关机2",
      "taskSeq": "2"
    }
  ]
}

```  

**请求应答**

```java
{
    "retCode": "00000",
"retInfo": "预约定时创建成功",
"detailInfo": {
     “taskId”: “878442f1550c4c189c5307873ca9b1dd”
}
}

```

#### 预约定时删除
> 将指定任务ID的预约定时设置为删除状态

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/delete   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String |	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	选填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: *****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: *******
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: ********
Content-Encoding: utf-8
Content-type: application/json
Body:
{
  “taskId”: “878442f1550c4c189c5307873ca9b1dd”，
   “taskSeq”:1
}



```

**请求应答**
```java
{
	"retCode": "00000",
	"retInfo": "成功!"
}


```

#### 预约定时修改
> 预约定时修改，适用于单设备单任务场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/modify   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:---------:|
|taskInfo|	TaskInfo|	Body|	必填	|任务信息|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |

>输入参数TaskInfo对象说明：

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId	|String varchar(50)|	Body|	必填|	预约定时任务id|
|taskSeq	|int|	Body|	必填	|子任务序号id|
|taskName	|String varchar(50)	|Body|	选填	|任务名称|
|taskDesc	|String varchar(100)	|Body|	选填	|任务描述|
|status	|int|	Body|	选填	|定时预约状态 0 启用；2 暂停；|
|endTime	|dateTime	|Body	|选填	|如果有值，按照此值的有效期|
|cron	|cron[]|	Body	|选填	|任务执行表达式；|
|intervals	|int|	Body	|选填	|任务执行距当前的间隔时间，以分钟为单位，限制一天以内（0-1440），如为0需要立即执行；|
|argsInfo	|ArgsInfo|	Body|	选填|	多套指令集；当前版本只支持一套|




##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-*****-0000
appVersion: *****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: ***********
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: **************
Content-Encoding: utf-8
Content-type: application/json
Body:
{
  "taskId": "a2edc5e0b9ba485f9e63abae35e49983",
  "taskSeq": 1,
  "status": 0,
  "intervals": 60,
  "endTime": "2019-07-31 13:00:00",
  "argsInfo": {
    "backUrl": "http://******",
    "cmdMsgList": [
      {
        "index": 0,
        "delaySeconds": 0,
        "name": "onOffStatus",
        "value": "true"
      },
      {
        "index": 1,
        "delaySeconds": 0,
        "name": "onOffStatus",
        "value": "true"
      }
    ]
  },
  "taskName": "空调关机-修改",
  "taskDesc": "空调开机-修改"
}


```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```



#### 预约定时批量修改
> 预约定时批量修改，适应于单设备批量定时和批量设备单定时场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/group/modify   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskInfos|	List<TaskInfo>|	Body|	必填	|批量任务的修改；list长度上限为50，超出则分批次调用；|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |  

>输入参数TaskInfo对象说明：

|参数名	|类型	|位置|	必填|	说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|taskId	|String varchar(50)|	Body|	必填|	预约定时任务id|
|taskSeq|	int|	Body|	必填|	子任务序号id；每组中的此字段不能重复|
|taskName|	String varchar(50)|	Body|	选填|	任务名称|
|taskDesc|	String varchar(100)|	Body|	选填|	任务描述|
|status|	Int|	Body|	选填	|定时预约状态 0 启用；2 暂停；|
|endTime	|dateTime|	Body|	选填	|如果有值，按照此值的有效期|
|cron	|cron[]|	Body	|选填	|任务执行表达式；|
|intervals	|int|	Body	|选填	|任务执行距当前的间隔时间，以分钟为单位，限制一天以内（0-1440），如为0需要立即执行；|
|argsInfo|	ArgsInfo|	Body|	选填|	多套指令集；当前版本只支持一套|



##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: *****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: **********
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: **********
Content-Encoding: utf-8
Content-type: application/json
Body:
{
  "taskInfos": [
    {
      "status": 2,
      "deviceId": "DC330D003121",
      "schedulerType": 1,
      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
      "intervals": 60,
      "endTime": "2019-07-30 14:00:00",
      "argsInfo": {
        "backUrl": "http://***********",
        "cmdMsgList": [
          {
            "index": 0,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          },
          {
            "index": 1,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          }
        ]
      },
      "taskName": "批量关机a111",
      "taskDesc": "批量关机a111",
      "taskId": "bad2adb0d756469b8ae0e292c81240ab",
      "taskSeq": 1
    },
    {
      "status": 2,
      "deviceId": "DC330D003121",
      "schedulerType": 1,
      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
      "intervals": 60,
      "endTime": "2019-07-30 15:00:00",
      "argsInfo": {
        "backUrl": "http://*******",
        "cmdMsgList": [
          {
            "index": 0,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          },
          {
            "index": 1,
            "delaySeconds": 0,
            "name": "onOffStatus",
            "value": "true"
          }
        ]
      },
      "taskName": "批量关机a112",
      "taskDesc": "批量关机a112",
      "taskId": "bad2adb0d756469b8ae0e292c81240ab",
      "taskSeq": "2"
    }
  ]
}


```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```



#### 根据userid查询预约
> 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约）

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/userid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	选填	|设备id；如果不为空，根据userId和deviceId查询，为空根据userId查询|
|startNumber|	int	|Body 	|选填	|分页参数，起始值；如果不填写，默认值为1|
|length	|int|	Body| 	选填	|分页参数，长度；如果不填写，默认值为100|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: ****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: *******
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: ************
Content-Encoding: utf-8
Content-type: application/json
Body
{
  “deviceId”:” DC330D01FBF1”,
  “startNumber”:1,
“length”:10
}



```

**请求应答**
```java
{
  "retCode": "00000",
"retInfo": "查询成功",
“detailInfo “:{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		“taskId”: “bad2adb0d756469b8ae0e292c81240ab”，
		“taskSeq”: 1，
		“taskName”: “空调开机”，
		“deviceId”: “DC330D01FBF1”，
		“scheduleType”：1，
		“wifiType”:” 111c120024000810040100318000614300000001000000000000000000000000”,
		“appInfo”:[
			{
			“appId”:”1111111”,
			“appVersion”:””,
			“appName”:””,
			“appLogo”:””
			}
			]， 
		“createUserInfo”: [
			{
			“userId”:”111111”,
			“userName”:"",
			“headImg”:””,
			“phone”:”139****1234”
			}
		]，
		“createTime”: “2018-04-19 17:00:00”，
		“modifyUserInfo”: “[
			{
			“userId”:”111111”,
			“userName”:”test”,
			“headImg”:”b.jpg”,
			“phone”:”139****1234”
			}
		]，
		“modifyTime”: “2018-04-19 17:00:00”，
		“cron”: [
			{
			“seconds”:”0”,
			“minutes”:”0/5”,
			“hours”:”*”,
			“day”:”*”,
			“monty”:”*”,
			“week”:”?”,
			“year”:””
			}
		],
		“endTime”:”2018-05-21 12:00:00”,
		“status”: 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		“taskDesc”:”空调开机”
		}
	]
    }
}


```


#### 根据deviceid查询预约
>根据deviceid查询预约详情

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/deviceid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	必填|	设备id|
|startNumber|	int|	Body|	选填	|分页参数，起始值；如果不填写，默认值为1|
|length|	int|	Body|	选填	|分页参数，长度；如果不填写，默认值为100|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
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
Body
{
	"deviceId": "DC330D32C193",
 	"startNumber":1,
	"length":10
}


```

**请求应答**
```java
{
  "retCode": "00000",
"retInfo": "查询成功",
“detailInfo”:{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		“taskId”: “bad2adb0d756469b8ae0e292c81240ab”，
		“taskSeq”: 1，
		“taskName”: “空调开机”，
		“deviceId”: “DC330D01FBF1”，
		“scheduleType”：1，
		“wifiType”:” 111c120024000810040100318000614300000001000000000000000000000000”,
		“appInfo”:[
			{
			“appId”:”1111111”,
			“appVersion”:””,
			“appName”:””,
			“appLogo”:””
			}
			]， 
		“createUserInfo”: [
			{
			“userId”:”111111”,
			“userName”:"",
			“headImg”:””,
			“phone”:”139****1234”
			}
		]，
		“createTime”: “2018-04-19 17:00:00”，
		“modifyUserInfo”: “[
			{
			“userId”:”111111”,
			“userName”:”test”,
			“headImg”:”b.jpg”,
			“phone”:”139****1234”
			}
		]，
		“modifyTime”: “2018-04-19 17:00:00”，
		“cron”: [
			{
			“seconds”:”0”,
			“minutes”:”0/5”,
			“hours”:”*”,
			“day”:”*”,
			“monty”:”*”,
			“week”:”?”,
			“year”:””
			}
		],
		“endTime”:”2018-05-21 12:00:00”,
		“status”: 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		“taskDesc”:”空调开机”
		}
	]
    }
}


```



#### 根据taskId查询预约
>根据taskid查询预约详情

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/taskid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String varchar(50)|	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	选填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-*****-0000
appVersion: *****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: *************
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: **************
Content-Encoding: utf-8
Content-type: application/json
Body
{
  “taskId”: “bad2adb0d756469b8ae0e292c81240ab”
}


```

**请求应答**
```java
{
"retCode": "00000",
"retInfo": "查询成功",
“detailInfo “:{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		“taskId”: “bad2adb0d756469b8ae0e292c81240ab”，
		“taskSeq”: 1，
		“taskName”: “空调开机”，
		“deviceId”: “DC330D01FBF1”，
		“scheduleType”：1，
		“wifiType”:” 111c120024000810040100318000614300000001000000000000000000000000”,
		“appInfo”:[
			{
			“appId”:”1111111”,
			“appVersion”:””,
			“appName”:””,
			“appLogo”:””
			}
			]， 
		“createUserInfo”: [
			{
			“userId”:”111111”,
			“userName”:"",
			“headImg”:””,
			“phone”:”139****1234”
			}
		]，
		“createTime”: “2018-04-19 17:00:00”，
		“modifyUserInfo”: “[
			{
			“userId”:”111111”,
			“userName”:”test”,
			“headImg”:”b.jpg”,
			“phone”:”139****1234”
			}
		]，
		“modifyTime”: “2018-04-19 17:00:00”，
		“cron”: [
			{
			“seconds”:”0”,
			“minutes”:”0/5”,
			“hours”:”*”,
			“day”:”*”,
			“monty”:”*”,
			“week”:”?”,
			“year”:””
			}
		],
		“endTime”:”2018-05-21 12:00:00”,
		“status”: 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		“taskDesc”:”空调开机”
		}
	]
    }
}


```



#### 预约定时执行日志查询
>预约定时执行日志查询

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/execlog   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId	|String 	|URL|	可选	|任务id|
|taskSeq|	int|	Body|	必填|	子任务序号id|




**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskRestLogDataInfo>|	Body|	必填	|返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-*****-0000
appVersion: ****.99990
clientId: 123
sequenceId: 2014022801010
accessToken: *******
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: *****
Content-Encoding: utf-8
Content-type: application/json
Body
{
  “taskId”:“bad2adb0d756469b8ae0e292c81240ab”,
    “taskSeq”:  “1”
 }


```

**请求应答**
```java
{
    "batchResult": true, 
    "cmdSn": "181206135609768fa163e26700a3593", 
    "detailInfo": [
        {
            "cmdSn": "181206135609768fa163e26700a3593", 
            "cmdSubSn": "181206135609768fa163e26700a3593:_:000", 
            "deviceId": "DC330D861BB6", 
            "execResult": true, 
            "execResultCode": "00000", 
            "execResultInfo": "成功", 
            "execStep": 2, 
            "oid": 14839, 
            "resultTime": 1544075772000
        }
    ], 
    "retCode": "00000", 
    "retInfo": "成功"
}


```


#### 预约任务执行预览
>预约任务执行预览，任务立即下发一次

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/preview   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String varchar(50)|	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	必填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-********-0000
appVersion: *******.99990
clientId: 123
sequenceId: 2014022801010
accessToken: ************
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: ************
Content-Encoding: utf-8
Content-type: application/json
Body:
{
  “taskId”: “bad2adb0d756469b8ae0e292c81240ab”
    “taskSeq”:1
}

```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```






[^-^]:常用图片注释
[pic1]:../_media/_scheduler/pic1.png
[pic2]:../_media/_scheduler/pic2.png
[pic3]:../_media/_scheduler/pic3.png
[pic4]:../_media/_scheduler/pic4.png
[pic5]:../_media/_scheduler/pic5.png
[pic6]:../_media/_scheduler/pic6.png
[pic7]:../_media/_scheduler/pic7.png
[pic8]:../_media/_scheduler/pic8.png
[pic9]:../_media/_scheduler/pic9.png


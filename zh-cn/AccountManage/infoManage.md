!> **更新时间**：{docsify-updated}  

## 简介
	
>  海尔账号的信息管理维护服务由海尔集团690用户中心提供。	


## 注册成为开发者  

>  请联系用户中心相关人员，获取开发者权限。

## 申请应用

>  联系相关人员，申请应用通过后，获得测试环境和正式环境的client_id与client_secret，前者为应用Id，后者为应用密钥。测试环境和正式环境的client_id与client_secret不会相同。  

## 前提

>  用户中心的userid长度最长是20位，全部由数字组成。各业务系统在使用该userid时，请确保数据库字段长度兼容到20位 （varchar或bigint），如果是java工程，请确保接收或存储用户userid时使用string或者 long类型。


## 接口列表

### 获取用户基本信息            

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

示例返回值为本接口最多能返回的用户信息字段，通常情况下，用户个人信息不会那么全面，那么如果值为空的字段，也不会在接口中返回。例如用户没有维护生日，那么返回值json将不会有birthdate字段。

请接入方在解析返回值过程中注意。

返回参数说明

| Parameter     | Desc       | type  | Remark  | 
| ------------- |:-------------:|:----------|:----------|
|id	|用户中心唯一用户ID|	long|	用户中心唯一用户ID|
|username|	用户名，可用于登录	|string|	可能为空|
|email|	邮箱，可用于登录	|string	|手机邮箱必定返回一个|
|email_verified|	邮箱是否验证|	boolean	|可能为空|
|phone_number|	手机号	|string|	手机邮箱必定返回一个|
|phone_number_verified|	手机是否验证|	boolean|	可能为空|
|nickname	|昵称，不可用于登录	|string	|可能为空|
|gender|	性别，男male，女female	|string	|可能为空|
|address|	家庭住址|	object	可能为空|
|given_name|	姓名，不可用于登录	|string	|可能为空|
|avatar_url|	头像|	string	|可能为空|
|birthdate|	生日，2001-01-01	|string	|可能为空|
|created_at|	创建时间	|date	|不为空|
|updated_at|	更新时间	|date	|不为空|


Request:
```
GET /userinfo HTTP/1.1    
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx

```  
 

Response:
```
{
    "nickname": "本帅是滑到",
    "username": "H817R158***",
    "email": "875458",
    "gender": "",
    "address": {
        "province": "浙江",
        "province_id": "33",
        "city": "杭州",
        "city_id": "22",
        "district": "西湖区",
        "district_id": "6",
        "line1": "三墩",
        "postcode": "310013"
    },
    "user_id": 20***,
    "given_name": "222",
    "avatar_url": "https://accountstatic.haier.com/avatartest/2****.jpg",
    "email_verified": true,
    "birthdate": "",
    "phone_number": "*****",
    "phone_number_verified": true,
    "created_at": 1586853023000,
    "updated_at": 1596518714000
}

```  

Error:   
 
错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token|401|access_token非法或失效|  
|insufficient_scope| 403 |权限不足，未获得接口所需的scope|  


### 修改密码              

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    


Request:
```
PUT /v1/users/change-password HTTP/1.1 
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx
Content-Type: application/json

{
"old_password": "123456",
"new_password": "654321"
}
```  


| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|old_password| 旧密码 |Y|
|new_password|新密码 |Y|  


Response:
```
{
"success": true
}
```  

Error:  

```
{
"error": "invalid_request"
} 
```  
 
错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token|401|access_token非法或失效|  
|insufficient_scope| 403 |权限不足，未获得接口所需的scope|  
 

### 找回密码              

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    


Request:
```
PUT /v2/users/getback-sms HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx
Content-Type: application/json

{
"sms_answer": "435928",
"mobile": "13111111111",
"new_password": "Haier123",
"client_ip":"192.169.1.1",
"user_agent":"Mozilla/5.0"
}
```  


| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|sms_answer| 短信随机码 |Y|
|mobile|手机号 |Y|  
|new_password|新密码 |Y|  
|client_ip|用户真实ip |Y|  
|user_agent|客户端userAgent信息 |Y|  


Response:
```
{
"success": true //成功
}
```  

Error:  

```
{
"error": "invalid_request"
} 
```  
 
错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token|401|access_token非法或失效|  
|insufficient_scope| 403 |权限不足，未获得接口所需的scope|   


业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|verification_code_not_match| 400 |短信验证码不正确|  
|cannot_getback_more| 400 |找回密码次数超限|  
|invalid_password| 400 |密码格式非法|  
|phone_number_not_exist| 400 |手机号不存在|  
|cannot_getback_more| 400|找回密码次数超限|
|verification_code_expired|400|验证码过期|    


    



### 更新用户基本信息                

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

请接入方在解析返回值过程中注意。

Request:
```
POST /haier/v1/users/me HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx
{
"nickname": "",
"gender": "female",
"avatar_url": "http://example.com/a.jpg",
"birthdate": "1991-01-01",
"address": {
"province_id": 0,
"province": "",
"city_id": 0,
"city": "",
"district_id": 0,
"district": "",
"town_id": 0,
"town": "",
"line1": "",
"line2": "",
"postcode": "0"
}
} 
```  


| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|nickname| 昵称|N|
|gender|性别 |female/male|  
|avatar_url|头像地址 |N|  
|birthdate|生日 |N|  
|address|居住地 |N|  


Response:  

```
"success": true  
```  

Error:  
 
错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token|401|access_token非法或失效|  
|insufficient_scope| 403 |权限不足，未获得接口所需的scope|  



### 退出接口              

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

请接入方在解析返回值过程中注意。

Request:
```
POST /v2/haier/signout HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer xxxxx
```  



Response:  

```
true
```  

Error:  

```
{
"error": "invalid_token",
"error_description": "Invalid access token: 04c9bebc-3037-43e0-b977-193f7b4ccc59"
} 
```  
 
错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token|401|access_token非法或失效|  
|insufficient_scope| 403 |权限不足，未获得接口所需的scope| 



###  注销接口

#### 发送短信验证码

     参考接口"发送短信验证码"，scenario传**logout**


#### 短信账号校验

注: 此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token。

接口中参数code由接口2参数scenario值为logout时发送到用户手机上。

**Request:**

```
POST /v2/haier/user/cancel/check/sms HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
Content-Type:application/x-www-form-urlencoded
```

| Parameter | Desc                                             | Required |
| --------- | ------------------------------------------------ | -------- |
| code      | 注销验证码，由接口1 参数scenario值为logout时获取 | Y        |

**Response:**

```
{
  "success": true
}
```

**错误码表:**

接口调用错误

| Error Code         | HTTP Status Code | Description                      |
| ------------------ | ---------------- | -------------------------------- |
| invalid_request    | 400              | 参数非法或缺失必填参数           |
| unauthorized       | 401              | 未授权接口 (未传 access_token)   |
| invalid_token      | 401              | access_token 非法或失效          |
| insufficient_scope | 403              | 权限不足, 未获得接口所需的 scope |

业务相关错误

| Error Code                  | HTTP Status Code | Description                 |
| --------------------------- | ---------------- | --------------------------- |
| social_bind                 | 401              | 账号已绑定第三方登录        |
| verification_code_not_match | 400              | 短信验证码不匹配 (不需重发) |
| verification_code_expired   | 400              | 短信验证码过期 (需重发)     |

#### 发送邮箱验证码

注: 此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖应用级token(接口〇获取)

**Request:**

```
POST /v2/email-verification-code/send HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
Content-Type: application/json
{
  "email": "xxxx@xxx.xxx",
  "scenario": "logout"
}
```

| Parameter | Desc                           | Required |
| --------- | ------------------------------ | -------- |
| email     | 邮箱地址                       | Y        |
| scenario  | 固定值: 传 **logout** 表示注销 | Y        |

**Response:**

```
{
  "success": true,   //发送成功
  "delay": 60        //下次发邮件延迟, 单位秒, 例: 60 秒后才能再次发送
}
```

**错误码表:**

接口调用错误

| Error Code         | HTTP Status Code | Description                      |
| ------------------ | ---------------- | -------------------------------- |
| invalid_request    | 400              | 参数非法或缺失必填参数           |
| unauthorized       | 401              | 未授权接口 (未传 access_token)   |
| invalid_token      | 401              | access_token 非法或失效          |
| insufficient_scope | 403              | 权限不足, 未获得接口所需的 scope |

业务相关错误

| Error Code             | HTTP Status Code | Description                                               |
| ---------------------- | ---------------- | --------------------------------------------------------- |
| invalid_email          | 400              | 邮箱格式非法                                              |
| invalid_scenario       | 400              | scenario 格式错误                                         |
| email_limit_send_today | 400              | 已超出当天发送次数                                        |
| too_often              | 400              | 发送过于频繁, 同时会增加 delay 字段指示需要等待的剩余秒数 |

####  邮箱账号校验

注: 此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token

**Request:**

```
POST /v2/haier/user/cancel/check/email HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
Content-Type: application/x-www-form-urlencoded
```

| Parameter | Desc                                             | Required |
| --------- | ------------------------------------------------ | -------- |
| code      | 注销验证码，由接口3 参数scenario值为logout时获取 | Y        |

**Success Response:**

```
{
  "success": "true"
}
```

**Error:**

**错误码表:**

接口调用错误

| Error Code         | HTTP Status Code | Description                      |
| ------------------ | ---------------- | -------------------------------- |
| invalid_request    | 400              | 参数非法或缺失必填参数           |
| unauthorized       | 401              | 未授权接口 (未传 access_token)   |
| invalid_token      | 401              | access_token 非法或失效          |
| insufficient_scope | 403              | 权限不足, 未获得接口所需的 scope |

业务相关错误

| Error Code                  | HTTP Status Code | Description              |
| --------------------------- | ---------------- | ------------------------ |
| verification_code_expired   | 400              | 验证码不存在或者已经过期 |
| verification_code_not_match | 400              | 验证码不匹配，绑定失败   |
| social_bind                 | 400              | 账号已绑定第三方社交登录 |

#### 注销账号

注: 此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token

**Request:**

```
POST /v2/haier/user/cancel/delete HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
```

**Success Response:**

```
{
  "success": "true"
}
```


**Error:**

**错误码表:**

接口调用错误

| Error Code         | HTTP Status Code | Description                      |
| ------------------ | ---------------- | -------------------------------- |
| invalid_request    | 400              | 参数非法或缺失必填参数           |
| unauthorized       | 401              | 未授权接口 (未传 access_token)   |
| invalid_token      | 401              | access_token 非法或失效          |
| insufficient_scope | 403              | 权限不足, 未获得接口所需的 scope |

#### 订阅注销消息

注: 消息订阅采用RocketMQ 版，需要引入sdk

```
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.7.0</version>
</dependency>
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-acl</artifactId>
    <version>4.7.0</version>
</dependency>
```

**Request:**

| Parameter   | Desc                                                    | Required |
| ----------- | ------------------------------------------------------- | -------- |
| accessKey   | 固定值：用户中心下发 （生产测试不一致）                 | Y        |
| secretKey   | 固定值：用户中心下发（生产测试不一致）                  | Y        |
| Topic       | 固定值：TOPIC_LOGOUT_USERINFO                           | Y        |
| group_id    | 消息组:用户中心下发 （生产测试不一致）                  | Y        |
| namesrvAddr | 消息集群地址(内网)环境：用户中心下发 （生产测试不一致） | Y        |

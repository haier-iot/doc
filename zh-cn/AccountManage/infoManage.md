!> **更新时间**：{docsify-updated}  

## 简介
	
>  海尔账号的信息管理维护服务由海尔集团690用户中心提供。	


## 注册成为开发者  

>  请联系用户中心相关人员，获取开发者权限。

## 申请应用

>  联系相关人员，申请应用通过后，获得测试环境和正式环境的client_id与client_secret，前者为应用Id，后者为应用密钥。测试环境和正式环境的client_id与client_secret不会相同。  



## 前提

1.接口中需要的client_id和client_secret由用户中心授权下发，根据接入应用的不同进行不同的授权。<br/>

2.文档提供的接口为海尔品牌接入接口，卡萨帝接口及GE品牌接口，分别随品牌不同而区分不同调用域名。<br/>

3.文档提供的接口大多受到access_token保护，接口分为应用级token保护和个人级token保护：应用级
保护大多发生于登录动作之前，token由以下获取应用级保护的access_token接口获取；个人级token保护大多发生于登录动作之后，token
由登录等动作接口返回。受保护的接口在被调用时，调用方需要在HTTP请求的Header中新增Authorization,
值为Bearer[access_token]，后面接口将直接备注受应用级还是设备级保护，请调用方自行甄别。<br/>

4.本接口的鉴权文档，需要透传Uhome参数，注意，接口中提到的client_id为用户中心下发的应用标识，uhome_client_id为Uhome要求的客户端标识，请注意区分，不要混淆。<br/>

5.鉴权通过后，接口返回的access_token字段为用户中心下发的用户访问令牌，uhome_access_token
为U+云平台下发的设备访问令牌，请注意区分，不要混淆。<br/>

6.关于透传的uhome三个参数，uhome_app_id、uhome_client_id、uhome_sign，做出以下说明：uhome_app_id是由U+云平台(海极网)颁发的应用ID，40位以内字符，Haier U+云平台全局唯一;uhome_client_id是客户端ID，主要用途为唯一标识客户端(例如,手机)。可调用U+云平台usdk得到客户端ID的值。<br/>
uhome_sign是U+云平台要求的安全验证签名，需要加密的字段只需要Uhome的appId+appKey+clientid(顺序不可变化)，不用加密原来文档里的url和body字符串等，具体请参考U+云平台的签名认证章节。<br/>

7.以下所有接口的正常Response下的HTTP Status Code为200，后无特殊情况不再说明。


## 接口列表

### 获取用户基本信息            

注: 此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

示例返回值为本接口最多能返回的用户信息字段，通常情况下，用户个人信息不会那么全面，那么如果值为空的字段，也不会在接口中返回。例如用户没有维护生日，那么返回值json将不会有birthdate字段。

请接入方在解析返回值过程中注意。  


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
"name": "H405R1450621708059R28", //默认字段
"username": "testGGG", //用户名(可用于登录)
"email": "123@123.com", //邮箱(可用于登录)
"gender": "female", //性别
"user_id": 3323432, //user_id，即为统一的用户中心/官网user_id
"email_verified": true, //邮箱验证
"birthdate": "2016-12-28", //生日
"phone_number": "18560630620", //手机号码 (可用于登录)
"avatar_url": "http://example.com/a.jpg", //头像
"phone_number_verified":true, //手机验证
"address": { //居住地
"province_id": 0, //省id (取自hp系统pro_code字段)
"province": "", //省
"city_id": 0, //市id (取自hp系统city_code字段)
"city": "", //市
"district_id": 0, //区id (取自hp系统area_code字段)
"district": "", //区
"line1": "", //详细地址
"postcode": "0" //HP邮编
}
"updated_at": 1483596113000 //默认时间戳
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
|bad_credentials| 400 |短信验证码不正确|  
|cannot_getback_more| 400|找回密码次数超限|
|invalid_password|400|密码格式非法|    


    



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
POST /uhome/signout HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx
Content-Type: application/x-www-form-urlencoded
uhome_client_id=123456&uhome_app_id=dddd&uhome_sign=76db3251a2237edbee21943&uhome_to
ken=aadfadf  
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
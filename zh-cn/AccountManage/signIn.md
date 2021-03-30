!> **更新时间**：{docsify-updated}  

## 简介
	
>  海尔账号的注册登录服务由海尔集团690用户中心提供。	


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

4.以下所有接口的正常Response下的HTTP Status Code为200，后无特殊情况不再说明。


## 接口列表

### 获取应用级保护的access_token接口

Request:

```
POST /oauth/token  HTTP /1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
	  https://account-api.haier.net [海尔品牌正式环境]
Content-Type:application/x-www-form-urlencoded

client_id=wodeyingyong&cliend_secret=secret&grant_type=client_credentials

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret| 用户中心下发的client_secret |Y|
|grant_type |固定值， client_credentials |Y|

Response:

```
{
"access_token": "yyyyy", //应用级access_token
"expires_in": 43199, //有效期，单位秒(默认有效期10天)
"token_type": "bearer" //token类型，调用时形如Bearer yyyy
}

```  

错误码表：

| Error Code     | HTTP Status Code | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|
|invalid_client| 401|无权调用，或所传 client_id / client_secret非法|
|invalid_grant| 400|授权异常或失败|
|unauthorized_client |400|应用未被授权此 grant_type|
|unsupported_grant_type| 400|系统不支持此 grant_type|
|invalid_scope| 400| scope 非法|


### 判断账号是否可用

注: 此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token

Request:

```
GET /v1/users/identifier-available?identifier=18888888888 HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy

```

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|identifier|手机号/用户名/邮箱|Y|


Response:
```
{
"available": true //true表示手机未被注册，可以继续注册
//false表示手机已被注册，不可以继续注册
}
```

错误码表：

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token| 401|access_token 非法或无效|
|insufficient_scope |403|权限不足，未获得接口所需的scope|


### 获取(刷新)图形验证码

注: 此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token

Request:
```
POST /v1/captcha HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy
```

Response:  
```
{
"captcha_token": "abc",
"captcha_image": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAgAAAQABAAD/..." //base64
的图片码
}
```
错误码表：

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token| 401|access_token 非法或无效|
|insufficient_scope |403|权限不足，未获得接口所需的scope|


### 发送短信验证码  

注: 此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token  

特别说明，本接口的captcha_token和captcha_answer虽然是选填项，但是原则上要求调用本接口时都要求用户输入，防止短信验证码接口被劫持为短信轰炸机的素材。有特殊业务要求的接入方，不能要求用户输入图形验证码的，请向用户中心说明情况再自行调用接口。  


Request:  

```
POST /v2/sms-verification-code/send HTTP/1.1  
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy  
Content-Type: application/json
{
"phone_number": "18888888888"，
"scenario": "registration"，
"captcha_token":"8d505749-3c26-49a1-b344-6def60164948"，
"captcha_answer":"3p5wg"
}

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|phone_number| 手机号 |Y|
|scenario|固定值：传registration表示注册 或 传login表示登录 或 传getback表示找回密码 |Y|
|captcha_token |由获取图形验证码接口获取的图形随机码token |N|  
|captcha_answer |由获取图形验证码接口获取的图形随机码图片上展示的答案 |N|


Response:
```
{
"success": true, //发送成功
"delay": 60 //下次发短信延迟，单位秒，例:60秒后才能再次发送
}
```  

Error:  
```
{
"error": "phone_number_occupied"
}
```
当错误为：`too_often`时，会同时返回`delay`指示剩余需要等待的秒数  
```  
{
"error": "too.often",
"delay": 59
}  
```  

错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token| 401|access_token 非法或无效|
|insufficient_scope |403|权限不足，未获得接口所需的scope|  

业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_phone_number| 400 |手机号格式非法|
|phone_number_occupied| 400|手机号被占用|
|captcha_required| 400|失败次数过多，须输入验证码|
|too_often |400|发送过于频繁，同时会增加delay字段指示需要等待的剩余秒数|
|sms_limit_send_today |400|单天单人请求短信次数超限|    
|mobile_temporarily_locked |400|手机号被暂时锁定|        

### 注册接口    

注: 此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token  

本接口提供的注册功能不适合手机号注册即登录。如果有此类需求，请直接调用短信随机码快速登录接口。接口中参数verification_code由发送短信验证码接口参数scenario值为registration时发送到用户手机上。  


Request:  

```
POST /v1/signup HTTP/1.1  
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy  
Content-Type: application/json
{
"phone_number": "18888888888"，
"verification_code": "353532"，
"password": "123456"
}

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|phone_number| 手机号 |Y|
|verification_code|注册验证码，由发送短信验证码接口参数scenario值为registration时获取 |Y|  
|password |密码 |Y|  


Response:
```
{
"success": true
}
```  

Error:   
   
```  
{
"error": "phone_number_occupied"
}
```
 

错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|
|unauthorized| 401|未授权接口（未传access_token）|
|invalid_token| 401|access_token 非法或无效|
|insufficient_scope |403|权限不足，未获得接口所需的scope|  

业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_phone_number| 400 |手机号格式非法|
|phone_number_occupied| 400|手机号被占用|  
|invalid_password| 400|密码格式非法|
|verification_code_not_match| 400|短信验证码不匹配（不需重发）|
|verification_code_expired |400|短信验证码过期（需重发）|   



### 短信随机码快速登录     

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口同样适合快速注册，适合不需要用户输入密码的短信随机码快速注册即登录的场景。接口中参数verification_code由发送短信验证码接口参数scenario值为login时发送到用户手机上。  

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:  

```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=sms&username=18888888888&password=553412

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定 password |Y|  
|connection |固定 sms |Y|  
|username |手机号 |Y|  
|password |短信验证码 |Y|  
|client_ip |用户真实IP |N|  
|longitude |经度 |N|  
|latitude |纬度  |N|  
|multiportflag |端的唯一标识 |N|  

Response:
```
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",   //即为所需的 访问令牌,
  "expires_in":3600,                       //access_token过期时间(单位秒，默认有效期10天)
  "scope": "openid profile email",           //默认授权范围，可忽略
  "token_type":"bearer",                     //access_token类型，受个人保护接口可使用
  "refresh_token":"2YotnFZFEjr1zCsicMWpAA"   //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
}

```  

Error:  
  
```  
{
"error": "bad_credentials"
}
```
 

错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|invalid_client| 401 |无权调用，或所传client_id / client_secret非法|
|invalid_grant| 400|授权异常或失败（如密码或短信验证码错误）|
|unauthorized_client| 400|应用未被授权此grant_type|
|unsupported_grant_type |400|系统不支持此grant_type|  
|invalid_scope| 400 |scope非法|  


业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|not_verified| 403 |手机/邮箱未激活|
|account_locked| 403|账户被冻结|  
|bad_credentials| 400|密码或短信验证码不正确|


### 账号密码登录      

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口为了防止CC攻击或者DDos撞库，当接口调用验证账号密码错误超过5次以后，5分钟内将在返回值返回图形验证码信息，第6次需要强制输入图形验证码信息，此后的图形验证码由接口2刷新后传入仍然有效。

当密码错误超过10次后，用户中心将锁定账号，1小时后自动解锁。

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:
```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=basic_password&username=admin&password=123456

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 password |Y|  
|connection |固定值传 basic_password |Y|  
|username |用户名/邮箱/手机号 |Y|  
|password |密码  |Y|  
|captcha_token |&nbsp; |N|  
|captcha_answer |&nbsp; |N|  
|client_ip |用户真实IP  |N|  
|longitude |经度 |N|   
|latitude |纬度 |N|
|multiportflag |端的唯一标识 |N|   

Response:
```
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",   //即为所需的 访问令牌,
  "expires_in":3600,                       //access_token过期时间(单位秒，默认有效期10天)
  "scope": "openid profile email",           //默认授权范围，可忽略
  "token_type":"bearer",                     //access_token类型，受个人保护接口可使用
  "refresh_token":"2YotnFZFEjr1zCsicMWpAA"   //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
}

```  

Error:   
 
```  
{  
"error": "username_not_found"
}
```  
当错误为：`captcha_required`时，会同时返回`captcha_token`与`captcha_image`，后者为base64图片数据，客户端遇到此错需要将图片展示给用户，提示用户输入验证码，待用户输入后重新提交登录请求，此时`captcha_token`与`captcha_answer`必填。  

```  
{
"error": "captcha_required",
"captcha_token": "abc",
"captcha_image": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAgAAAQABAAD/..."
}  
```  


错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|invalid_client| 401 |无权调用，或所传client_id / client_secret非法|
|invalid_grant| 400|授权异常或失败（如密码或短信验证码错误）|
|unauthorized_client| 400|应用未被授权此grant_type|
|unsupported_grant_type |400|系统不支持此grant_type|  
|invalid_scope| 400 |scope非法|  


业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|username_not_found| 400 |用户名/手机/邮箱不存在|
|not_verified| 403|手机/邮箱未激活|  
|account_locked| 403|账户被冻结|
|bad_credentials |400|密码或短信验证码不正确|  
|captcha_required |400|失败次数过多，须输入验证码；或者验证码不正确|
|uhome_token_request_error |400|获取Uhome设备令牌失败|   

### 刷新普通登录        

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口使用的refresh_token仰赖短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token。  

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:
```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=refresh_token&refresh_token=XXXXXXX

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 refresh_token |Y|  
|refresh_token |短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token |Y|  
|client_ip |用户真实IP |N|  
|longitude |经度 |N|  
|latitude	 |纬度 |N|  
  

Response:
```
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",   //即为所需的 访问令牌,
  "expires_in":3600,                       //access_token过期时间(单位秒，默认有效期10天)
  "scope": "openid profile email",           //默认授权范围，可忽略
  "token_type":"bearer",                     //access_token类型，受个人保护接口可使用
  "refresh_token":"2YotnFZFEjr1zCsicMWpAA"   //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
}

```  

Error:   
 
```  
{
"error": "invalid_client",
"error_description": "Bad client credentials"
}
```  

错误码表：  

接口调用错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|invalid_request| 400 |参数非法或缺失必填参数|  
|invalid_client| 401 |无权调用，或所传client_id / client_secret非法|
|unauthorized_client| 400|应用未被授权此grant_type|
|unsupported_grant_type |400|系统不支持此grant_type|  
|invalid_grant| 400 |refresh_token非法|  





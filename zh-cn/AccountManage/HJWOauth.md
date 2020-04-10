
!>  **更新时间**：{docsify-updated}  



## 1.介绍

为第三方应用提供海尔账号的Oauth授权服务，允许用户使用海尔帐号登录第三方应用，访问和控制自己的海尔设备。

接入流程

![接入流程][pic1]

## 2.注册成为开发者

访问[海级网-开发者中心](https://www.haigeek.com/developercenter/static/develop.html#/system/login)
选择我是企业用户表单，填写和上转相关信息，完成注册


## 3.创建云应用

### 3.1	创建云应用

经过审核后获得systemId和systemKey：	

| 名称      | 说明         | 
| ------------- |:-------------:|
|systemId| 云应用ID |
|systemKey| 云应用ID的秘钥 |


### 3.2	添加服务

云应用 -> 服务信息->添加服务->海尔帐号控制设备

![海尔账号控制设备][pic2]

接入信息

![接入信息][pic3]

## 4.接入流程的开发

### 4.1授权请求地址

```
http://resource.haigeek.com/download/resource/selfService/haigeek/mobile-page/Oauth/login.html?client_id={systemId}&state={state}&response_type=code&redirect_url={redirect_url}

```
| 参数名称      | 必填         | 说明         |
| ------------- |:-------------:|:-------------:|
|response_type| 是 |授权码模式 response_type=code |
|client_id| 是 |云应用systemId |
|redirect_uri| 是 |授权回调地址，务必和授权表单里填写的一致|
|scope| 否 |权限范围，用于对客户端的权限进行控制，如果客户没有传递该参数，那么服务器则以该应用的所有权限代替 |
|state| 否 |用于维持请求和回调过程中的状态，防止CSRF攻击，服务器不对该参数做任何处理，如果客户端携带了该参数，则服务器在响应时原封不动的返回 |


输入上面的授权请求地址后，出现如下界面：

![开通服务1][pic8]

1、	输入海尔账号登录名（支持手机号和用户名）和密码，点击授权并登录；如未注册国海尔账号，请点击右上方的app下载注册链接，注册海尔账号；如图1所示。</br>
2、	支持使用手机号直接登录，手机接收验证码短信，输入短信验证码即可完成授权，如图2所示。</br>
3、	用户需要勾选权限和登录协议才能点击授权并登录按钮。</br>
4、	如输入密码错误次数过多，会提示图形验证码界面，强制用户输入验证才能完成授权，当用户输入密码错误次数达到限制，系统会锁定该用户帐号，5小时后自动解锁。


### 4.2	授权成功返回
```
Location: {redirect_uri}?code=SplxlOBeZQQYbYS6WxSbIA&state={state}
```
|       |          |          |
| ------------- |:-------------:|:-------------:|
|redirect_uri| 授权回调地址，务必和授权表单里填写的一致|
|code| 用户授权给应用的授权码 |
|state| 应用生成的随机字符,藉此判断此次回跳是否被伪造 |
				
### 4.3	获取授权token 

?> **请求地址：** `https://uws.haier.net/oauth2/v1/token`</br>
**HTTP Method：** POST </br>
**Content-Type：** application/x-www-form-urlencoded


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
client_id|String|Param|是|开发者在海极网申请的云应用systemId
client_secret|String|Param|是|开发者在海极网申请的云应用systemKey
redirect_uri|String|Param|否|回调地址, 必须和申请应用是填写的一致(参数部分可不一致)
grant_type|String|Param|是|务必为authorization_code
code|String|Param|否|如grant_type为authorization_code时必须传入code字段，使用授权码换取token，code有效期为 10 分钟，只能使用1次
refresh_token|String|Param|否|如grant_type为refresh_token时必须传入refresh_token字段，用于刷新token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
access_token|String|json|是|访问令牌
refresh_token|String|json|是|访问令牌生命周期（单位：秒）
expires_in|String|json|是|刷新令牌
scope|String|json|是|访问令牌实际权限范围


```
POST /token HTTP/1.1
/oauth/v1/token?
	grant_type=authorization_code&
	code=ANXxSNjwQDugOnqeikRMu2bKaXCdlLxn&
	client_id=SV-CLOUDAPP-0000&
	client_secret=0rDSjzQ20XUj5itV7WRtznPQSzr5pVw2&
	redirect_uri=http%3A%2F%2Fwww.example.com%2Foauth_redirect

```

### 4.4	刷新授权token 


?> **请求地址：** `https://uws.haier.net/oauth2/v1/token`</br>
**HTTP Method：** POST </br>
**Content-Type：** application/x-www-form-urlencoded


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
client_id|String|Param|是|开发者在海极网申请的云应用systemId
client_secret|String|Param|是|开发者在海极网申请的云应用systemKey
redirect_uri|String|Param|否|回调地址, 必须和申请应用是填写的一致(参数部分可不一致)
grant_type|String|Param|是|务必为refresh_token
refresh_token|String|Param|否|如grant_type为refresh_token时必须传入refresh_token字段，用于刷新token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
access_token|String|json|是|访问令牌
refresh_token|String|json|是|访问令牌生命周期（单位：秒）
expires_in|String|json|是|刷新令牌
scope|String|json|是|访问令牌实际权限范围


```
/v1/token?
    grant_type=refresh_token&
    refresh_token=TGT8U7349535U3453475983453U37&
    client_id= SV-CLOUDAPP-0000&
    client_secret= 0rDSjzQ20XUj5itV7WRtznPQSzr5pVw2&
    scope=mobile


```


## 5.解除授权

### 5.1	用户在智家app中取消授权

1、下载智家app，我的->第三方平台授权->取消授权

![授权][pic6]

2、第三方开发者提供取消授权回调地址，用户在智家app端主动取消授权时，第三方可通过回调地址同步删除缓存在本地的授权token。如下图所示，第三方开发者需在海极网填写取消授权回调地址。

![取消授权][pic7]

3、回调地址的定义

?> **接口名称：**  第三方提供取消授权回调地址 </br>
**请求地址：** `第三方自定义，例：https://www.abc.com/oauth/cancel/callback?token=xyz`</br>
**HTTP Method：** GET


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|Param|是|授权token

**输出参数**

由第三方自定义

### 5.2	第三方应用端实现取消授权

#### 5.2.1取消授权接口


?> **接口名称：** 在第三方应用端取消授权</br>
**请求地址：** `https://uws.haier.net/uaccount/v1/oauth/thirdpart/cancel`</br>
**HTTP Method：** POST


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|是|云应用ID；
clientId|String|Header|是|云应用授权时的终端ID；用于标识授权终端
accessToken|String|Header|是|授权token
sign|String|Header|是|对请求进行签名运算产生的签名，sha256算法签名；见签名算法章节
timestamp|String|Header|是|Unix时间戳，精确到毫秒。
Content-Type|String|Header|是|application/json;charset=UTF-8。
systemId|String|Body|是|云应用ID
clientId|String|Body|是|云应用授权时的终端ID；用于标识授权终端

**输出参数**

标准输入，成功返回00000




#### 5.2.2 获取云应用授权时的终端ID


?> **接口名称：** 获取云应用授权时的终端ID </br>
**请求地址：** `https://uws.haier.net/oauth/2.0/tokeninfo`</br>
**HTTP Method：** POST


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Param|是|授权token；

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
user_id|String|Body|是|海尔账号ID
iss|String|Body|是|授权颁发者的字符串
exp|String|Body|是|有效期 单位秒
app_id|String|Body|是|云应用ID
iat|String|Body|是|创建时间，时间戳
client_id|String|Body|是|云应用授权时的终端ID
error_description|String|Body|否|授权token无效
error|String|Body|否|授权token失效时返回D00006错误码









[^-^]:常用图片注释
[pic1]:../_media/_HJWOauth/pic1.jpg
[pic2]:../_media/_HJWOauth/pic2.png
[pic3]:../_media/_HJWOauth/pic3.png
[pic4]:../_media/_HJWOauth/pic4.png
[pic5]:../_media/_HJWOauth/pic5.png
[pic6]:../_media/_HJWOauth/pic6.png
[pic7]:../_media/_HJWOauth/pic7.png
[pic8]:../_media/_HJWOauth/pic8.jpg

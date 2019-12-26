!> **更新时间**：{docsify-updated}  


## 简介
	
>  海尔账号的Oauth授权服务由海尔集团690用户中心提供。	



## 注册成为开发者  

>  请联系用户中心相关人员，获取开发者权限。

## 申请应用

>  联系相关人员，申请应用通过后，获得测试环境和正式环境的client_id与client_secret，前者为应用Id，后者为应用密钥。测试环境和正式环境的client_id与client_secret不会相同。  

##  使用的技术

>  本平台的单点登录采用Oauth2协议，遵循OpenId Connect规范。
一般情况下，采用Oauth2的authorization_code流程接入。

##  接入流程

  由于应用的多样化，可分为以下几种情况：


1.后端渲染的Web应用（Rails，Spring MVC...），传统的B/S架构 

2.单页应用（React，AngularJS...）,又分：a.后端提供API接口 ；b.无后端，前端直接对接本平台获得用户。

3.原生APP应用（IOS，Android...）

4.纯后端应用（REST，dubbo...）,指仅提供服务的应用

本文档（单点登录）支持前三者，不支持第四钟纯后端应用，请阅读者悉知。


特别的,有一种特殊场景:由本平台APP通过webview 打开的应用,此时单点登录的状态直接由接入
本平台的APP保持;不过由于从接入流程的角度看并无二致,本文档后续不会再关注此场景.


1)下文将依据不同的应用种类,进行流程的描述,先给出一些约定:

下文描述本平台的服务地址均使用https://idp.com和com.idp://,其中前者为web服务域名地址,后者为原生 APP应用自定义scheme地址;真实对接的域名如下,请开发者注意替换使用


o测试环境:http://taccount.haier.com

o正式环境:http://account.haier.com 和http://account-api.haier.net(只有/userinfo接口使用)

2)相同的,我们擅自的将应用的回跳域名描述成https://rp.com和com.rp://,请替换成确切
的域名。此域名需要区分测试环境和正式环境,提供给用户中心配置回调白名单。


3)我们假设为应用创建了client_id和client_secret分别为 rptest和rpsecret


分步描述:
1.用户访问到应用 需要登录的页面,例:https://rp.com/user/me

2.此时应用要生成授权URL,如下
https://idp.com/oauth/authorize?
client_id=rptest&response_type=code&state=xyz&redirect_uri=https://rp.com/login_callback


需要特别注意的是,若采用webview打开HTTP 授权URL,则无法享受单点登录特性,APP内
嵌的H5应用单点登录方案请另外索取。

其中必传参数:

client_id为应用ID,我们使用例子中的rptest

response_type为授权方式,这里固定为code

redirect_uri指定回跳地址,这里为https://rp.com/login_callback,应用通过这个地址接
收用户的授权码;此参数必传


非必传参数:

state为应用生成的随机字符,在用户授权回调时会原样返回给应用,藉此可以判断来自本
平台的回跳是否被伪造;此参数非必传,但推荐传送以增强安全性

display 告知本平台以何种登录界面展示给用户,如设定为qr时,本平台仅展示只有二维码
的页面(意在让用户扫码登录),同时此页面可作为iframe 嵌入应用当前页面;若未指定,则默认
展示登录界面

3.用户进入2.中应用生成的授权URL,登录并完成对应用的授权(抑或不授权);此步骤系本
平台内部完成,应用方无需关注


4.用户授权之后,本平台将重定向用户至回跳页:
https://rp.com/login_callback/?code=ksdfj&state=xyz,若是原生APP则为
com.rp://login_callback/?code=ksdfj&state=xyz

其中:

code即是用户授权给应用的授权码

state接2.为应用生成的随机字符,藉此判断此次回跳是否被伪造


5.用户(往往是浏览器,原生APP则可认为是系统)接收到重定向后,访问重定向的URL(4.中的
URL),即是将授权码给予了应用


6.应用(的后端)取得授权码后(指通过访问4.中URL 使得后端程序得到code),在后端发起
HTTPPOST请求向本平台交换访问令牌,其中,参数uhome_client_id,uhome_app_id,uhome_sign,type_uhome 为设备token鉴权必须,如果
无须设备鉴权,则无须传入此四个参数;uhome_sign 的获取方法请参考附录一。 如下例:


**需要设备鉴权:**
```
POST /oauth/token HTTP/1.1
Host:idp.com[测试环境http://taccount.haier.com 正式环境
http://account.haier.com]
Authorization:Basic cnBOZXNOOnJwc2VjcmVO
Content-Type:application/x-www-form-urlencoded
grant_type=authorization_code&code=ksdfj&redirect_uri=https%3A%2F%2Frp.com%2F1
ogin_callback&uhome_client_id=123456&uhome_app_id=MB-RSQCSAPP-
0000&uhome_sign=76dfe3686b3251a223e458db5445711447e967b48504f87c09c4b12dbee219
43&type_uhome=type_uhome_common_token
或者
POST /oauth/token HTTP/1.1
Host:idp.com[测试环境http://taccount.haier.com 正式环境
http://account.haier.com]
Content-Type:application/x-www-form-urlencoded
client_id=rptest&client_secret=rpsecret&grant_type=authorization_code&code=ksd
fj&redirect_uri=https%3A%2F%2Frp.com%2Flogin_callback&uhome_client_id=123456&u
home_app_id=MB-RSQCSAPP-
0000&uhome_sign=76dfe3686b3251a223e458db5445711447e967b48504f87c09c4b12dbee219
43&type_uhome=type_uhome_common_token
```

**无须设备鉴权:**
```
POST /oauth/token HTTP/1.1
Host:idp.com[测试环境http://taccount.haier.com 正式环境
http://account.haier.com]
Authorization:Basic cnBOZXNOOnJwc2Vjcmvo
Content-Type:application/x-www-form-urlencoded
grant_type=authorization_code&code=ksdfj&redirect_uri=https%3A%2F%2Frp.com%2F1
ogin_callback
或者
POST/oauth/token HTTP/1.1
Host:idp.com[测试环境http://taccount.haier.com 正式环境
http://account.haier.com]
Content-Type:application/x-www-form-urlencoded
client_id=rptest&client_secret=rpsecret&grant_type=authorization_code&code=ksd
fj&redirect_uri=https%3A%2F%2Frp.com%2Flogin_callback
```


两种方式皆可,前者将client_id和client_secret通过HTTP的Basic Authorization
头进行传送,其中cnBOZXNOOnJwc2Vjcmv为base64(<client_id>+":"+
《client_secret》);后者通过表单直接传送;推荐使用前者.


必传参数:

grant_type:这里固定传 authorization_code

code:为4.和5.中获得的授权码

redirect_uri:必须与2.中的redirect_uri]相同

uhome_client_id:客户端ID,主要用途为唯一标识客户端(例如,手机)。可调用U+云平台usdk
得到客户端ID的值

uhome_app_id:由U+云平台(海集网)办法的应用ID,40位以内字符,Haier U+云平台全局唯
一;

uhome_sign:是U+云平台要求的安全验证签名,需要加密的字段只需要Uhome的

appld+appkey+clientid(顺序不可变化),具体请参考附录1

type_uhome:这里固定传type_uhome_common_token


7.接6.,成功返回如下例:

```
HTTP/1.1 200 OK
Content-Type:application/json;charset=UTF-8
Cache-Control:no-store
Pragma:no-cache
{
"access_token":"63341c4d-5399-42a8-bb31-
ca008e569a67&&TGT1UA9G6BABCBD52NZCRQR2KONSTO&&100013957366165837",
"token_type":"bearer",
"refresh_token":"7d0122b3-90e9-477e-8f9a-4c981bdc40a5",
"expires_in":863999,
"scope":"equipments.admin users.admin openid clients.admin",
"source":"ehaier"
}
```

access_token区分步骤6是否区分设备鉴权来解析:


**有设备鉴权的情况**,access_token即为token信息的组合,

由三部分组成,即<access_token>&&<uhome_access_token>&&<uhome_user_id>,字符串由两
个&&符号分隔为三部分,第一部分为用户中心访问令牌access_token,第二部分为云平台
token,第三部分为云平台user_id


**无须设备鉴权的情况**,access_token 即为用户中心访问令牌。

refresh_token即为刷新令牌,可调用接口刷新access_token

expires_in 即为 access_token过期时间(单位秒),access_token 默认有效期为10天,

source为单点登录的被登录方可接收到的源登录方信息client_id信息。



8.应用拿到用户中心访问令牌access_token后,即以用户的身份向本平台发起请求获得用户信
息:
```
GET/userinfo HTTP/1.1
Host:idp.com[测试环境http://taccount.haier.com 正式环境http://account-
api.haier.net]
Authorization:Bearer 2YotnFZFEjr1zCsicMWpAA

```
其中2YotnFZFEjr1zCsicMWpAA即为7.中拿到的访问令牌



9.接8.,成功返回如下例:

```
HTTP/1.1 200 OK
Content-Type:application/json;charset=UTF-8
"name":"H405R1450621708059R28",//默认字段
"username":"testGGG",
//用户名(可用于登录)
"email":"123@123.com",
//邮箱(可用于登录)
"gender":"female",
//性别
"user_id":3323432,
//user_id,即为统一的用户中心/官网user_id
"email_verified":true,
//邮箱验证
"birthdate":"2016-12-28",
//生日
"phone_number":"18560630620",
//手机号码(可用于登录)
"phone_number_verified":true,
//手机验证
"updated_at":1483596113000
//默认时间戳
}

```

10.此时,应用即可将用户信息写入应用自己的session,至此用户成功"登录"至应用


11.应用应当在1.中记住用户本要访问的URL,(本例中为https://rp.com/user/me),本文档
推荐应用在session中保存此URL;在"登录"成功后,引导用户返回
https://rp.com/user/me; 而原生APP往往不会这么做,直接返回首页即可.



注册

场景:用户点击委托方注册链接

第一步:委托方引导用户进入提供方注册地址,附加上委托方标识(client_id)和回跳地址(redirect_uri)

http://idp.com/register?client_id=rptest&redirect_uri=https://client.com

第二步:用户在提供方完成了注册,提供方依照委托方指定回跳至委托方地址https://client.com

注意:

此处的redirect_uri没有白名单限制

idp.com 需替换为测试环境:http://taccount.haier.com 正式环境:http://account.haier.com



登出
应用前端跳转(href)

http://idp.com/logout?
client_id=rptest&post_logout_redirect_uri=http://client.com/api/user/logout

其中参数
client_id为应用ID,我们使用例子中的 rptest

post_logout_redirect_uri为回跳退出链接,用户回跳退出各应用,http://client.com/logout为退出范例

注意:

此处post_logout_redirect_uri没有白名单限制

idp.com 需替换为测试环境:http://taccount.haier.com 正式环境:http://account.haier.com


附录1

1.uhome签名认证说明

应用需要对发送到openapi的请求进行签名,执行签名计算的签名值需要赋值到Header头中的
sign属性(见公共部分说明),以便服务端进行签名验证。

注:本文档接口中需要的uhome_sign不是通过Header传输,请参考具体接口调用形式。


2.uhome签名认证参数介绍

待签名字符串为:appld+appkey+clientid;其中:

appld,uhome应用ID,40位以内字符,Haier U+云平台全局唯一;

appKey,uhome平台分配给应用的appKey,不能明文发送;

clientld,客户端ID,主要用途为唯一标识客户端(例如,手机)。可调用usdk得到客户端ID的
值。;


3.uhome签名认证算法

签名算法就是对待签名字符串计算64位小写sha256值.

Java程序示例:

```
String getsign(String appId,String appKey,String timestamp,String body){
appKey=appKey.trim();
appKey=appKey.replaceA11("\"","");
StringBuffer sb=new StringBuffer();
sb.append(appId).append(appKey).append(clientId);
MessageDigest md=nul1;
byte[]bytes=null;
try {
md=MessageDigest.getInstance("SHA-256");
bytes=md.digest(sb.tostring().getBytes("utf-8"));
}catch(Exception e){
e.printStackTrace();
return BinaryToHexString(bytes);
}
String BinaryToHexString(byte[]bytes){
StringBuilder hex=new StringBuilder();
String hexStr="0123456789abcdef";
for(int i=0;i< bytes.length;i++){
hex.append(String.valueOf(hexStr.charAt((bytes[i]&OxFO)>>4)));
hex.append(String.valueOf(hexStr.charAt(bytes[i]&Ox0F)));
}
return hex.tostring();
}

```

4.uhome签名认证示例

appKey:e8a1e19058c0928d7690cfd59c6b062d

appld:MB-ABC-0000

clientld:357070051953752-88329BFE7949

待签名字符串:(注意必须appld+appKey+clientld, **顺序不能变!**)

MB-ABC-0000e8a1e19058c0928d7690cfd59c6b062d357070051953752-88329BFE7949

经过计算,签名字符串为:
09868caa8236d713460667abdc85155edbc084b3b397cc577fc37be72df84484







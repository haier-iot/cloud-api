
!> **更新时间**：{docsify-updated}  


## 简介
	
>  账号服务由海尔集团690用户中心提供。	



## 注册成为开发者  

>  请联系用户中心相关人员，陈子琦（chenziqi@haier.com）获取开发者权限。

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
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=sms&username=18888888888&password=553412&type_uhome=type_uhome_common_token&uhome_client_id=123456&uhome_app_id=MB-RSQCSAPP-0000&uhome_sign=76dfe3686b3251a223e458db5445711447edbee21943

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定 password |Y|  
|connection |固定 sms |Y|  
|username |手机号 |Y|  
|password |短信验证码 |Y|  
|type_uhome |固定 type_uhome_common_token |Y|  
|uhome_client_id |参考前提，透传UHome参数 |Y|  
|uhome_app_id |参考前提，透传UHome参数  |Y|  
|uhome_sign |参考前提，透传UHome参数 |Y|  

Response:
```
{
"access_token":"2YotnFZFEjr1zCsicMWpAA", //即为所需的访问令牌,
"expires_in":3600, //access_token过期时间(单位秒，默认有效期10天)
"scope": "openid profile email", //默认授权范围，可忽略
"token_type":"bearer", //access_token类型，受个人保护接口可使用
"refresh_token":"2YotnFZFEjr1zCsicMWpAA", //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
"uhome_access_token": "TGT91JFPNKDSYT22QTGFGOMZ85U900", //即为所需的 uhome设备令牌
"uhome_user_id"：13123123 //透传回UHome返回的uhome userid
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
|uhome_token_request_error |400|获取Uhome设备令牌失败|  

### 账号密码登录      

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口为了防止CC攻击或是DDos撞库，当接口调用验证账号密码错误超过3次以后，将在返回值返回图形验证码信息，第4次需要强制输入图形验证码信息，此后的图形验证码由获取图形验证码接口刷新后传入任然有效。  

当密码错误超过5次后，用户中心将锁定账号，24小时后自动解锁。

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:
```
POST /oauth/token HTTP/1.1    
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Content-Type: application/x-www-form-urlencoded  
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=sms&username=18888888888&password=553412&type_uhome=type_uhome_common_token&uhome_client_id=123456&uhome_app_id=MB-RSQCSAPP-0000&uhome_sign=76dfe3686b3251a223e458db5445711447edbee21943

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 password |Y|  
|connection |固定值传 basic_password |Y|  
|username |用户名/邮箱/手机号 |Y|  
|password |密码  |Y|  
|type_uhome |固定 type_uhome_common_token |Y|  
|uhome_client_id |参考前提，透传UHome参数 |Y|  
|uhome_app_id |参考前提，透传UHome参数  |Y|  
|uhome_sign |参考前提，透传UHome参数 |Y|   
|captcha_token | |N|
|captcha_answe | |N|   

Response:
```
{
"access_token":"2YotnFZFEjr1zCsicMWpAA", //即为所需的访问令牌,
"expires_in":3600, //access_token过期时间(单位秒，默认有效期10天)
"scope": "openid profile email", //默认授权范围，可忽略
"token_type":"bearer", //access_token类型，受个人保护接口可使用
"refresh_token":"2YotnFZFEjr1zCsicMWpAA", //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
"uhome_access_token": "TGT91JFPNKDSYT22QTGFGOMZ85U900", //即为所需的 uhome设备令牌
"uhome_user_id"：13123123 //透传回UHome返回的uhome userid
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
client_id=wodeyingyong&client_secret=secret&grant_type=refresh_token&refresh_token=XXXXXXX&type_uhome=type_uhome_common_token&uhome_client_id=123456&uhome_app_id=MBRSQCSAPP-0000&uhome_sign=76dfe3686b3251a223e458db5445711447edbee21943

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 password |Y|  
|refresh_token |短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token |Y|  
|type_uhome |固定值传 type_uhome_common_token |Y|  
|uhome_client_id |参考前提，透传UHome参数 |Y|  
|uhome_app_id |参考前提，透传UHome参数  |Y|  
|uhome_sign |参考前提，透传UHome参数 |Y|    

Response:
```
{
"access_token":"2YotnFZFEjr1zCsicMWpAA", //即为所需的访问令牌,
"expires_in":3600, //access_token过期时间(单位秒，默认有效期10天)
"scope": "openid profile email", //默认授权范围，可忽略
"token_type":"bearer", //access_token类型，受个人保护接口可使用
"refresh_token":"2YotnFZFEjr1zCsicMWpAA", //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
"uhome_access_token": "TGT91JFPNKDSYT22QTGFGOMZ85U900", //即为所需的 uhome设备令牌
"uhome_user_id"：13123123 //透传回UHome返回的uhome userid
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
|uhome_token_request_error| 400 |获取Uhome设备令牌失败|   

### 社交登录后绑定手机          

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口中参数verification_code由发送短信随机码接口参数scenario值为login时发送到用户手机上。    

本接口social_type严格必填，如果不填，则为普通登录接口。  

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:
```
POST /oauth/token HTTP/1.1    
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Content-Type: application/x-www-form-urlencoded  
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=sms&username=18888888888&password=553412&social_type=wechat&social_open_id:o6h-4w4p6FhAnJkowD_9HpQTDnE0&social_access_token=Cmt4XaSExdtLqNxOxkZGtZ0eZhD4al_2KSX&social_extra=adfadf&type_uhome=type_uhome_social_token&uhome_client_id=123456&uhome_app_id=MB-RSQCSAPP-0000&uhome_sign=76dfe3686b3251a223e458db5445711447edbee21943

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 password |Y|   
|connection |固定值传 sms |Y|  
|username |手机号 |Y|  
|password | 短信验证码，由发送短信随机码接口参数scenario值为login获取 |Y|   
|social_type |固定值，微信传wechat；微信公众号传wechat-public；微博传weibo；QQ传qq；网易传netease；豆瓣传douban |Y|  
|social_open_id |三方社交返回的唯一userid |Y|  
|social_access_token |三方社交返回的唯一登录token |Y|  
|social_extra |三方云端校验token需要的额外授权信息，social_type为qq时，需要必填qq开放者平台申请的秘钥 |N|  
|type_uhome |固定值传 type_uhome_social_token|Y|  
|uhome_client_id |参考前提，透传UHome参数 |Y|  
|uhome_app_id |参考前提，透传UHome参数  |Y|  
|uhome_sign |参考前提，透传UHome参数 |Y|    

Response:
```
{
"access_token":"2YotnFZFEjr1zCsicMWpAA", //即为所需的访问令牌,
"expires_in":3600, //access_token过期时间(单位秒，默认有效期10天)
"scope": "openid profile email", //默认授权范围，可忽略
"token_type":"bearer", //access_token类型，受个人保护接口可使用
"refresh_token":"2YotnFZFEjr1zCsicMWpAA", //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
"uhome_access_token": "TGT91JFPNKDSYT22QTGFGOMZ85U900", //即为所需的 uhome设备令牌
"uhome_user_id"：13123123 //透传回UHome返回的uhome userid
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
|invalid_grant| 400 |授权异常或失败（如密码或短信验证码错误）|
|unauthorized_client| 400|应用未被授权此grant_type|
|unsupported_grant_type |400|系统不支持此grant_type|  
|invalid_scope| 400 |scope非法|  
     

业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|not_verified| 403 |手机/邮箱未激活|  
|account_locked| 403 |账户被冻结|  
|bad_credentials| 400 |密码或短信验证码错误）|
|social_open_id_error| 400|social_open_id不能为空或错误|
|social_access_token_error |400|social_access_token不能为空或错误|  
|social_type_error| 400 |social_type错误|  
|social_extra_error| 400 |social_extra错误|  
|uhome_token_request_error| 400 |获取Uhome设备令牌失败|  


### 刷新社交登录            

注: 此接口不受个人或者应用token保护，由特殊参数保护。    

本接口使用的refresh_token仰赖短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token，需缓存至客户端。 

一般情况下，本接口依赖的refresh_token从客户端缓存获取，此时refresh_token字段直接传入；在遭遇用户清理数据或者其他突发情况拿不到refresh_token时，此时refresh_token字段传入social_type值:social_open_id值，由用户中心向三方云端校验真伪后，再获取唯一用户信息返回。  

如果手机更换或者缓存更新等，客户端不知道该社交账号是否绑定过手机，可以通过本接口的报错获悉三方社交账号是否绑定过账号。如果报错􁲙social_connection_null，则未绑定过手机，请调用社交登录后绑定手机接口。  

通常情况下，请客户端返回正常缓存的refresh_token。

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  


Request:
```
POST /oauth/token HTTP/1.1    
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Content-Type: application/x-www-form-urlencoded  
client_id=wodeyingyong&client_secret=secret&grant_type=refresh_token&refresh_token=XXXXXXX&social_type=wechat&social_open_id:o6h-4w4p6FhAnJkowD_9HpQTDnE0&social_access_token=Cmt4XaSExdtLqNxOxkZGtZ0eZhD4al_2KSX&social_extra=adfadf&type_uhome=type_uhome_social_token&uhome_client_id=123456&uhome_app_id=MB-RSQCSAPP-0000&uhome_sign=76dfe3686b3251a223e458db5445711447edbee21943

```  

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 refresh_token |Y|   
|refresh_token |短信随机码快速登录接口或者账号密码登录接口返回的refresh_token；如果refresh_token丢失，那么传social_type
值:social_open_id值 |Y|  
|social_type |固定值，微信传wechat；微信公众号传wechat-public；微博传weibo；QQ传qq；网易传netease；豆瓣传douban |Y|   
|social_open_id |三方社交返回的唯一userid |Y|  
|social_access_token |三方社交返回的唯一登录token |Y|  
|social_extra |三方云端校验token需要的额外授权信息，social_type为qq时，需要必填qq开放者平台申请的秘钥 |N|  
|type_uhome |固定值传 type_uhome_social_token|Y|  
|uhome_client_id |参考前提，透传UHome参数 |Y|  
|uhome_app_id |参考前提，透传UHome参数  |Y|  
|uhome_sign |参考前提，透传UHome参数 |Y|    

Response:
```
{
"access_token":"2YotnFZFEjr1zCsicMWpAA", //即为所需的访问令牌,
"expires_in":3600, //access_token过期时间(单位秒，默认有效期10天)
"scope": "openid profile email", //默认授权范围，可忽略
"token_type":"bearer", //access_token类型，受个人保护接口可使用
"refresh_token":"2YotnFZFEjr1zCsicMWpAA", //刷新token，可调用刷新token接口刷新出新的access_token(默认有效期1年)
"uhome_access_token": "TGT91JFPNKDSYT22QTGFGOMZ85U900", //即为所需的 uhome设备令牌
"uhome_user_id"：13123123 //透传回UHome返回的uhome userid
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
|invalid_grant| 400 |授权异常或失败（如密码或短信验证码错误）|
|unauthorized_client| 400|应用未被授权此grant_type|
|unsupported_grant_type |400|系统不支持此grant_type|  
|invalid_grant| 400 |refresh_token非法|  
     

业务相关错误  

| Error Code     | HTTP Status Code       | Description  | 
| ------------- |:-------------:|:----------|
|social_access_token_error |400|social_access_token不能为空或错误|  
|social_refresh_token_error| 400 |refresh_token不能为空或者错误|  
|social_type_error| 400 |social_type错误|  
|social_extra_error| 400 |social_extra错误|  
|uhome_token_request_error| 400 |获取Uhome设备令牌失败|  
|social_connection_null| 400 |社交账号并没有绑定过手机，刷新失效|  


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




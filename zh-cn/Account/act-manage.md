#  用户管理

## 获取应用级保护的access_token接口

**使用说明**

接口中需要的client_id和client_secret由用户中心授权下发，根据接入应用的不同进行不同的授权。

**接口描述**

Request:

```
POST /oauth/token  HTTP /1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
	  https://account-api.haier.net [海尔品牌正式环境]
Content-Type:application/x-www-form-urlencoded

client_id=wodeyingyong&cliend_secret=secret&grant_type=client_credentials

```  

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret| 用户中心下发的client_secret |Y|
|grant_type |固定值， client_credentials |Y|


**输出参数**

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


## 判断账号是否可用


**使用说明**

此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token

**接口描述**

Request:

```
GET /v1/users/identifier-available?identifier=18888888888 HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy

```

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|identifier|手机号/用户名/邮箱|Y|


**输出参数**

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


## 获取(刷新)图形验证码

**使用说明**

此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token


**接口描述**

Request:
```
POST /v1/captcha HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer yyyyy
```

**输入参数**



**输出参数**

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


## 发送短信验证码  

**使用说明** 

此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token  

特别说明，本接口的captcha_token和captcha_answer虽然是选填项，但是原则上要求调用本接口时都要求用户输入，防止短信验证码接口被劫持为短信轰炸机的素材。有特殊业务要求的接入方，不能要求用户输入图形验证码的，请向用户中心说明情况再自行调用接口。  


**接口描述**

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

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|phone_number| 手机号 |Y|
|scenario|固定值：传registration表示注册 或 传login表示登录 或 传getback表示找回密码 |Y|
|captcha_token |由获取图形验证码接口获取的图形随机码token |N|  
|captcha_answer |由获取图形验证码接口获取的图形随机码图片上展示的答案 |N|


**输出参数**

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

## 注册接口    

**使用说明**

此接口使用的 access_token ( Authorization: Bearer yyyyy ) 依赖应用级token  

本接口提供的注册功能不适合手机号注册即登录。如果有此类需求，请直接调用短信随机码快速登录接口。接口中参数verification_code由发送短信验证码接口参数scenario值为registration时发送到用户手机上。  


**接口描述**

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

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|phone_number| 手机号 |Y|
|verification_code|注册验证码，由发送短信验证码接口参数scenario值为registration时获取 |Y|  
|password |密码 |Y|  

**输出参数**

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



## 短信随机码快速登录     


**使用说明**

此接口不受个人或者应用token保护，由特殊参数保护。    

本接口同样适合快速注册，适合不需要用户输入密码的短信随机码快速注册即登录的场景。接口中参数verification_code由发送短信验证码接口参数scenario值为login时发送到用户手机上。  

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  

**接口描述**

Request:  

```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=sms&username=18888888888&password=553412

```  

**输入说明**

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


**输出说明**

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


## 账号密码登录      

**使用说明**

此接口不受个人或者应用token保护，由特殊参数保护。    

本接口为了防止CC攻击或者DDos撞库，当接口调用验证账号密码错误超过5次以后，5分钟内将在返回值返回图形验证码信息，第6次需要强制输入图形验证码信息，此后的图形验证码由接口2刷新后传入仍然有效。

当密码错误超过10次后，用户中心将锁定账号，1小时后自动解锁。

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  

**接口描述**

Request:
```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=password&connection=basic_password&username=admin&password=123456

```
  
**输入参数**

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


**输出参数**

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


## 刷新普通登录        

**使用说明**

此接口不受个人或者应用token保护，由特殊参数保护。    

本接口使用的refresh_token仰赖短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token。  

本接口为POST请求，`Content-Type为application/x-www-form-urlencoded`，不是`application/json`，参数也不是json形式，请接入方注意。  

**接口描述**

Request:
```
POST /oauth/token HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Content-Type: application/x-www-form-urlencoded
client_id=wodeyingyong&client_secret=secret&grant_type=refresh_token&refresh_token=XXXXXXX

```  

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|client_id| 用户中心下发的client_id |Y|
|client_secret|用户中心下发的client_secret |Y|  
|grant_type |固定值传 refresh_token |Y|  
|refresh_token |短信随机码快速登录接口或者账号密码登录接口或者本接口获取的refresh_token |Y|  
|client_ip |用户真实IP |N|  
|longitude |经度 |N|  
|latitude	 |纬度 |N|  
  
**输出参数**

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




## 获取用户基本信息            


**使用说明**

此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

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

**接口描述**


Request:
```
GET /userinfo HTTP/1.1    
Host: https://taccount.haier.com [海尔品牌测试环境]
https://account-api.haier.net [海尔品牌正式环境]
Authorization: Bearer xxxxx

```  

**输入参数** 


**输出参数**

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


## 修改密码              

**使用说明**

此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    


**接口描述**

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

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|old_password| 旧密码 |Y|
|new_password|新密码 |Y|  


**输出参数**

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
 

## 找回密码              

**使用说明**

此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    


**接口描述**

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

**输入参数**


| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|sms_answer| 短信随机码 |Y|
|mobile|手机号 |Y|  
|new_password|新密码 |Y|  
|client_ip|用户真实ip |Y|  
|user_agent|客户端userAgent信息 |Y|  


**输出参数**

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


    



## 更新用户基本信息                

**使用说明**

此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

请接入方在解析返回值过程中注意。

**接口描述**

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

**输入参数**

| Parameter      | Desc         | Required  | 
| ------------- |:-------------:|:----------|
|nickname| 昵称|N|
|gender|性别 |female/male|  
|avatar_url|头像地址 |N|  
|birthdate|生日 |N|  
|address|居住地 |N|  


**输出参数**

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



## 退出接口              

**使用说明**

此接口使用`access_token ( Authorization: Bearer yyyyy )`仰赖个人级token。    

请接入方在解析返回值过程中注意。


**接口描述**

Request:
```
POST /v2/haier/signout HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer xxxxx
```  

**输入参数**


**输出参数**

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



##  注销接口

### 发送短信验证码

     参考接口"发送短信验证码"，scenario传**logout**


### 短信账号校验

**使用说明**

此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token。

接口中参数code由接口2参数scenario值为logout时发送到用户手机上。

**接口描述**

Request:

```
POST /v2/haier/user/cancel/check/sms HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
Content-Type:application/x-www-form-urlencoded

```

**输入参数**

| Parameter | Desc                                             | Required |
| --------- | ------------------------------------------------ | -------- |
| code      | 注销验证码，由接口1 参数scenario值为logout时获取 | Y        |


**输出参数**

Response:

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

### 发送邮箱验证码


**使用说明**

此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖应用级token(接口〇获取)


**接口描述**

Request:

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

**输入参数**


| Parameter | Desc                           | Required |
| --------- | ------------------------------ | -------- |
| email     | 邮箱地址                       | Y        |
| scenario  | 固定值: 传 **logout** 表示注销 | Y        |



**输出参数**

Response:

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

###  邮箱账号校验

**使用说明**

此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token


**接口描述**

Request:

```
POST /v2/haier/user/cancel/check/email HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
Content-Type: application/x-www-form-urlencoded
```

**输入参数**

| Parameter | Desc                                             | Required |
| --------- | ------------------------------------------------ | -------- |
| code      | 注销验证码，由接口3 参数scenario值为logout时获取 | Y        |


**输出参数**

Success Response:

```
{
  "success": "true"
}
```

Error:

错误码表:

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

### 注销账号

**使用说明**

此接口使用的 access_token (`Authorization: Bearer yyyyy`) 仰赖个人级token


**接口描述**

Request:

```
POST /v2/haier/user/cancel/delete HTTP/1.1
Host: https://taccount.haier.com [海尔品牌测试环境]
      https://account-api.haier.net [海尔品牌正式环境] 
Authorization: Bearer yyyyy
```

**输入参数**


**输出参数**

Success Response:

```
{
  "success": "true"
}
```


Error:

错误码表:

接口调用错误

| Error Code         | HTTP Status Code | Description                      |
| ------------------ | ---------------- | -------------------------------- |
| invalid_request    | 400              | 参数非法或缺失必填参数           |
| unauthorized       | 401              | 未授权接口 (未传 access_token)   |
| invalid_token      | 401              | access_token 非法或失效          |
| insufficient_scope | 403              | 权限不足, 未获得接口所需的 scope |

### 订阅注销消息

**使用说明**

消息订阅采用RocketMQ 版，需要引入sdk

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



消息订阅请参考以下文档：https://help.aliyun.com/document_detail/150025.html?spm=a2c4g.11186623.6.646.70d45101Z0LQhG
#  用户授权

## 海尔账号授权
	
### 介绍

为第三方应用提供海尔账号的Oauth授权服务，允许用户使用海尔帐号登录第三方应用，访问和控制自己的海尔设备。

接入流程

![接入流程][pic1]

### 注册成为开发者

访问[海级网-开发者中心](https://www.haigeek.com/developercenter/static/develop.html#/system/login)
选择我是企业用户表单，填写和上转相关信息，完成注册


### 创建云应用

**1	创建云应用**

经过审核后获得systemId和systemKey：	

| 名称      | 说明         | 
| ------------- |:-------------:|
|systemId| 云应用ID |
|systemKey| 云应用ID的秘钥 |


**2	添加服务**

云应用 -> 服务信息->添加服务->海尔帐号控制设备

![海尔账号控制设备][pic2]

接入信息

![接入信息][pic3]

### 接入流程的开发

**1. 海尔授权登录H5页链接**

第三方App通过发现技能或者绑定第三方账号的方式打开海尔账号Oauth授权登录H5页，此时第三方App要生成授权 URL（标准的OAuth 授权码模式的认证的URI）。用户进入授权URL，登录并完成对应用的授权，用户中心将重定向 用户至第三方App回跳页，并带上code和state。


```
 测试环境：https://taccount.haier.com/oauth/authorize?client_id=rptest&amp;response_type=code&amp;state=xyz&amp;redirect_uri=https://r p.com/login_callback&amp;multiportflag=123  

 生产环境：https://account.haier.com/oauth/authorize?client_id=rptest&amp;response_type=code&amp;state=xyz&amp;redirect_uri=https://r p.com/login_callbac&amp;multiportflag=123

```

|Parameter | Desc| Require| 
| ------------- |:-------------:|:-------------:|
|client_id |为海极网分配的systemid, 我们使用例子中的 rptest| Y|
|response_type| 为授权方式, 这里固定为 code| Y|
|redirect_uri |指定回跳地址, 这里为 https://rp.com/login_callback| Y|
|multiportflag|请求终端唯一标识，随机字符，最大长度不超过32位。在访问IoT设备中心绑定控制设备接口时强制校验终端标识ID是否和token匹配（注明：设备中心接口参数定义为 clientId）|Y|
|state|为应用生成的随机字符, 在用户授权 回调时会原样返回给应用,藉此可以判断来自本平台的回跳是否被伪造; 此参数非必传,但推荐传送以增强安全性|N|
|display|告知本平台以何种登录界面展示给用户, 如设定为qr时, 本平台仅展示只有二维码的页面(意在让用户 扫码登录), 同时此页面可作为iframe嵌入应用当前页面; 若未指定, 则默认展示登录界面|N|



输入上面的授权请求地址后，出现如下界面：

![开通服务1][pic8]

1、	输入海尔账号登录名（支持手机号和用户名）和密码，点击授权并登录；如未注册国海尔账号，请点击右上方的app下载注册链接，注册海尔账号；如图1所示。</br>
2、	支持使用手机号直接登录，手机接收验证码短信，输入短信验证码即可完成授权，如图2所示。</br>
3、	用户需要勾选权限和登录协议才能点击授权并登录按钮。</br>
4、	如输入密码错误次数过多，会提示图形验证码界面，强制用户输入验证才能完成授权，当用户输入密码错误次数达到限制，系统会锁定该用户帐号，5小时后自动解锁。


**2. 授权成功返回**
```
Location: {redirect_uri}?code=SplxlOBeZQQYbYS6WxSbIA&state={state}
```
| 参数 |    说明      |     备注     |
| ------------- |:-------------:|:-------------:|
|redirect_uri| 授权回调地址，务必和授权表单里填写的一致|
|code| 用户授权给应用的授权码 |
|state| 应用生成的随机字符,藉此判断此次回跳是否被伪造 |
				
**3. 获取授权token**

**使用说明**

通过oauth登录获取的token获取海尔token，其中：  
1. Oauth登录返回的code，使用授权码换取token，code有效期为10分钟，只能使用1次；  
2. refresh token默认有效期为1年，失效过后，需要海尔账号重新授权；
3. 在RefreshToken的有效期内，使用接口“刷新海尔token，/ucs/uia/refresh/token”，获取到的RefreshToken有效期不会延长，只会重新刷新10天accessToken的有效期；

**接口描述**

?> **请求地址：** `https://uws.haier.net/ucs/uia/get/token-code `</br>
**HTTP Method：** POST </br>



**输入参数**

参数名|类型|位置|是否必填|说明
:-|:-:|:-:|:-:|:-
code|String|body|是|Oauth登录返回的code ，使用授权码换取token，code有效期为 10 分钟，只能使用1次
redirectUrl |String|body|是|和调用登录h5 时使用的回调地址一致 
systemId|String|Header|是|海极网申请的systemId 
sequenceId |String|Header |否|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用日期+顺序编号的方式。 
apiVersion |String|Header |是|此处默认填v1 
sign |String|Header |是|签名 
timestamp |long |Header |是|Unix时间戳，精确到毫秒。 
Content-Type  |String |Header |是|application/json;charset=UTF-8  

**输出参数**

参数名|类型|位置|说明
:-|:-:|:-:|:-
retCode| String| body| 错误码 
retInfo |String| body |错误详细信息 
payload |Object  |Body|  
accessToken|String| payload| 授权凭证 
refreshToken |String |payload |刷新凭证 
uhomeClientId| String| payload| 访问Iot设备中心绑定控制设备接口，头信息必填参数（注明：设备中心接口参数定义为 clientId） 
source |String| payload| 来源 
scope |String |payload| 权限范围 
expiresIn |Long| payload| accessToken的有效时间（以秒为单位） 


**请求示例** 

```
POST data: 
{ 
 "code": "abcopdfoiekdsm", 
 "redirectUrl": "https://www.haigeek.com" 
} 
```

**请求应答**

```
{ 
 "retCode": "00000", 
 "retInfo": "成功", 
 "payload": { 
  "accessToken": "wfhwdh", 
  "expiresIn": 300, 
  "refreshToken": "wfwidoijwdmpocop", 
  "scope": "write,read", 
  "source": "haier" 
 } 
} 
```

**4. 刷新授权token**

**使用说明**

使用刷新refreshToken刷新出新的accessToken和refreshToken

**接口描述**

?> **请求地址：** `https://uws.haier.net/ucs/uia/refresh/token `</br>
**HTTP Method：** POST </br>


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
refreshToken  |String|body|是|刷新凭证 
systemId|String|Header|是|海极网申请的systemId 
sequenceId |String|Header |否|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用日期+顺序编号的方式。 
apiVersion |String|Header |是|此处默认填v1 
sign |String|Header |是|签名 
timestamp |long |Header |是|Unix时间戳，精确到毫秒。 
Content-Type  |String |Header |是|application/json;charset=UTF-8  

**输出参数**

参数名|类型|位置|说明
:-|:-:|:-:|:-
retCode| String| body| 错误码 
retInfo |String| body |错误详细信息 
payload |Object  |Body|  
accessToken|String| payload| 授权凭证 
refreshToken |String |payload |刷新凭证 
uhomeClientId| String| payload| 访问Iot设备中心绑定控制设备接口，头信息必填参数（注明：设备中心接口参数定义为 clientId） 
source |String| payload| 来源 
scope |String |payload| 权限范围 
expiresIn |Long| payload| accessToken的有效时间（以秒为单位） 


**请求示例**

```
{ 
 "refreshToken": "refreshToken" 
} 
```

**请求应答**
```
{ 
 "retCode": "00000", 
 "retInfo": "成功", 
 "payload": { 
  "accessToken": "wfhwdh", 
  "expiresIn": 300, 
  "refreshToken": "wfwidoijwdmpocop", 
  "scope": "write,read", 
  "source": "haier" 
 }
} 
```

## 账号隐式授权

### 应用场景

App内业务授权,例如：App中的Html5详情页的高级功能，需App用户选择开启功能后，对应功能后端的应用服务才获得授权，按照功能业务处理逻辑主动给用户名下的设备进行逻辑计算及操作控制设备。


### 服务说明
	
为开发者提供登录应用App1用户的会话授权分享给应用App2使用，以及相关会话管理服务内容。


1）请求需提供要分享到的AppId、ClientId，和当前终端的AccessToken，对AccessToken进行校验后，生成授权码，此授权码只能用于指定的AppId、ClientId和账户。授权码的有效期为10分钟（有效期可配置）。<br/>

2）用户收到授权码后，通过安全渠道传输给需要分享到的App，App通过提交自己的AppId、ClientId和授权码，系统对授权码与AppId和ClientId的对应关系和授权码的有效期进行校验，校验合格则为对应的账户生成新的RefreshToken和AccessToken返回给用户；校验失败，则返回授权码校验失败的错误。<br/>

3）refreshToken 刷新令牌 用于会话accessToken延期，延期会话会生成新的refreshToken和accessToken ；refreshToken只能用一次刷新，刷一次后失效，未使用刷新前长期有效。AccessToken的有效期默认25天。<br/>


4）仅通过登录或RefreshToken获取的AccessToken，方可获取授权码，通过授权码获取到的AccessToken及由RefreshToken再次获取的AccessToken不可用来申请授权码。<br/>


详细的业务流程参考下图：

![业务流程][session1]

### 接口列表

**1. 获取授权Code**

**使用说明**

通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程。  



**接口描述**

?> **接入地址：** `/uaccount/v2/auth/shareCode`</br>
**HTTP Method：** POST  
**token验证：** 是    

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareAppId|String|Body|必填|请求分享会话的appId  
shareClientId|String|Body|必填|请求分享会话的clientId  
accessToken|String|Body|必填|用于分享会话的accessToken    

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
code|String|Body|必填|会话分享验证码 


**请求示例**

```java 
POST https://uws.haier.net/uaccount/v2/auth/shareCode

POST data:
{"accessToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000","shareClientId":"MB-T**2-0000","shareClientId":"456FEW334DD" }

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

``` java
{
	"retCode":"00000",
	"retInfo":"成功",
	"code":"72f7b235dd3afee2c77907d160c66539850b3224da60cb6e6638809005f48ec5"
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|    
D00004|accessToken不存在或已过期|用于分享会话的accessToken过期或不存在    
D00026|禁止分享此会话|分享会话的accessToken不能分享给其他终端    

**2. 获取Access Token**


**使用说明**

通过Authorization Code，获取用户授权accessToken及refreshToken。  

**接口描述**

?> **接入地址：** `/uaccount/v2/auth/shareToken`</br>
**HTTP Method：** POST  
**token验证：** 否      

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
code|String|Body|必填|会话分享验证码    
  

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Body|必填|安全令牌  
refreshToken|String|Body|必填|刷新令牌  
scope|String|Body|必填|访问资源的范围  
expire|String|Body|必填|有效期 ，单位秒  



**请求示例**

```java
POST https://uws.haier.net/uaccount/v2/auth/shareToken

POST data: 
{"code":" da48b7de0a9bd0639b43fc40948176821784d3c01276870cceccf0b6564624e7 " }

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST2-0000
appVersion: 2.4.0
clientId: 456FEW334DD
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":"TGTV5FR3XH20S0B2E7G56V1CMQ4T67,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|    
00001|登录成功但未接受最新版本隐私协议|老账号未接受隐私条款的情况，仅存在于海外环境    
B00001|缺少必填参数|code不存在   
B00002|参数类型错误|数据格式错误   

**3. AccessToken刷新**

**接口描述**

accessToken过期后，可以使用对应的refreshToken获取新的accessToken。  


**接口描述**

?> **接入地址：** `/uaccount/v2/auth/token`</br>
**HTTP Method：** POST  
**token验证：** 否      

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
refreshToken|String|Body|必填|app端持有refreshToken，用于会话accessToken延期，延期会话会生成新的refreshToken和accessToken     
grantType|String|Body|必填|授权方式 ，当前默认为 refresh_token  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Body|必填|安全令牌  
refreshToken|String|Body|必填|刷新令牌  
scope|String|Body|必填|访问资源的范围  
expire|String|Body|必填|有效期 ，单位秒  

**请求示例**


```java
POST https://uws.haier.net/uaccount/v2/auth/token

POST data:
{"refreshToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000","grantType":"refresh_token"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":"TGTV5FR3XH20S0B2E7G56V1CMQ4T67,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
00001|登录成功但未接受最新版本隐私协议|老账号未接受隐私条款的情况，仅存在于海外环境    
D00025|refreshToken不存在或已过期|     
D00005|Token不是由此应用创建，未通过token验证|refreshToken不是同一个设备创建   



**4. 应用授权状态查询**


**使用说明**

通过accessToken，查询此用户进行会话分享的客户端列表。  

**接口描述**

?> **接入地址：** `/uaccount/v2/auth/queryShareList`</br>
**HTTP Method：** POST  
**token验证：** 是      

**输入参数**

输入标准输入参数  
  

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareTokenInfoList|List|Body|必填|客户端列表信息</br>shareAppId 客户端appId</br> shareClientId 客户端clientId</br> state  0 授权，（已获取授权码，授权码未过期，未获得token）</br> 1 登录（已获得分享token）  



**请求示例**

```java

POST data: 

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"shareTokenInfoList":
	[
		{
			"shareAppId":"MB-HEYJOZB-0001",
			"shareClientId":"sh0326",
			"state":"1"
		}
	]
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|参数不符合规则要求     
D00008|用户不合法|Token已过期或不存在       
D00030|未授权|分享获取的token不能取消授权     


**5. 用户授权取消** 

**使用说明**

用户发起，取消对特定应用的授权后，该应用获取的RefreshToken和accessToken及会话延期的Token全部失效。  


**接口描述**

?> **接入地址：** `/uaccount/v2/auth/cancelShare`</br>
**HTTP Method：** POST  
**token验证：** 否      

**输入参数**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareAppId|String|Body|必填|会话分享的appId  
shareClientId|String|Body|必填|会话分享的clientId     

  

**输出参数**

输出标准输出参数    



**请求示例**

```java

POST data: 
{"shareAppId":"MB-HEYJOZB-0001","shareClientId":"123456789"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|参数不符合规则要求     
B00001|缺少必填参数|缺少必填参数         
D00008|用户不合法|Token已过期或不存在       
D00030|未授权|分享获取的token不能取消授权        



**6. accessToken登出** 


**使用说明**

账号退出登录，会话accessToken失效。  

**接口描述**

?> **接入地址：** `/uaccount/v1/security/logout`</br>
**HTTP Method：** POST  
**token验证：** 是         

**输入参数**  

输入标准输入参数   

  

**输出参数**

输出标准输出参数    




**请求示例**

```java

Header：
appId:MB-TEST-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
accessToken: TGT1OY0RUUAH5D242SB68E9WX0W930
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
language:zh-cn
timezone:8
Content-type: application/json


```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
D00005|Token不是由此应用创建，未通过token验证|操作成功       
D00016|你已退出或尚未登录|           



## 设备授权
	
### 介绍

语音WIFI模块设备（如遥控器），需要用户绑定设备时，自动获取用户授权，以便用户可对遥控器对话，控制用户名下其他设备；若用户不进行授权，则无法使用遥控器语音设备进行控制其他设备；


>说明：
>
>1、被授权方应是一个应用（海极网），可是海极网的服务提供者，或应用开发者；<br/>
2、用户绑定设备，自动认为用户已同意授权应用方获取用户授权信息；<br/>
3、应用通过deviceId获取用户授权信息 前提：1）设备商定义开放应用访问设备授权（typeId+systemId） 2）用户已经绑定具体deviceId设备；<br/>
4、用户绑定设备若不同意授权，则应用使用deviceid获取用户授权时，反馈授权失败信息；<br/>
5、用户解绑设备，设备厂商+用户授权 自动解除授权；




### 授权服务接口

**1. 获取授权token**

> 通过授权码获取设备授权token请求头携带AI云server的 systemId，clientId（设备的deviceId），签名，body中携带授权码；获取设备授权token</br>
> 前置条件：AI云server接收IOT用户系统的授权码

**接口描述**

?> **接入地 址：**  `/uaccount/v3/auth/deviceAuthToken `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| code     | String | Body| 是|会话分享验证码|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  accessToken  |  String  |   Body   | 是| 安全令牌  |
|  refreshToken  |  String  |   Body   | 是| 刷新令牌  |
|  scope  |  String  |   Body   | 是| 访问资源的范围，默认auth_default  |
|  expire  |  String  |   Body   | 是| 有效期 ，单位秒  |



**请求示例**  


```  
POST https://uws.haier.net/uaccount/v3/auth/deviceAuthToken

POST data: 
{"code":"783uy5758345353595tyttwtyh934753753" }

[no cookies]

Request Headers:
Connection: keep-alive
systemId: SV-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```  

**请求应答**

```
{"retCode":"00000","retInfo":"成功
","refreshToken": TGTV5FR3XH20S0B2E7G56V1CMQ4T67,"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00","scope":"all","expire":"2160000"}


```

**2. 会话刷新**

> accessToken过期后，可以使用对应的refreshToken获取新的accessToken</br>
> 前置条件：获取有效的授权，包括accessToken和refreshToken

**接口描述**

?> **接入地 址：**  `/uaccount/v3/auth/token  `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| refreshToken     | String| Body| 是|用于会话accessToken延期，延期会话会生成新的refreshToken和accessToken|
| grantType     | String| Body| 是|授权方式 ，当前默认为refresh_token|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  accessToken  |  String  |   Body   | 是| 安全令牌  |
|  refreshToken  |  String  |   Body   | 是| 刷新令牌  |
|  scope  |  String  |   Body   | 是| 访问资源的范围，默认auth_default  |
|  expire  |  String  |   Body   | 是| 有效期 ，单位秒  |



**请求示例**  


```
POST https://uws.haier.net/uaccount/v3/auth/token

POST data:
{"refreshToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000","grantType":"refresh_token"}

[no cookies]

Request Headers:
Connection: keep-alive
systemId: SV-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


```  

**请求应答**

```
{"retCode":"00000","retInfo":"成功
","refreshToken": TGTV5FR3XH20S0B2E7G56V1CMQ4T67,"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00","scope":"auth_app","expire":"2160000"}

```
**3. 回调服务接口**

?> 接收授权码回调接口服务，由需要开通设备授权的业务开发方按接口定义要求开发服务，部署服务并提供外网访问服务URL地址给到IoT平台进行配置授权。


**接口描述**

?> **接入地 址：**  `由设备开发者定义  `  
 **HTTP Method：** GET

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|timestamp|	long |	Header|	是	|Unix时间戳，精确到毫秒。|
|sign|	String	|Header|	是	|对请求进行签名运算产生的签名，签名规则详见下sign签名算法。|
|code|	String |	param|	是	|授权码|
|systemId|	String |	param|	是	|回调云server 的systemId|
|deviceId|	String |	param|	是	|设备ID|
|typeId|	String |	param|	是	|设备类型ID|

**输出参数**  


|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String  |   Body   | 是| 返回码（其中00000代表请求成功）  |
|  retInfo  |  String  |   Body   | 是| 用于调试的返回信息，不支持国际化，也不能直接显示在UI上  |
|  sn  |  String  |   Body   | 是| 请求唯一标识码，由云端生成  |





**请求示例**
```
Header：
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
GET
http://********/oauth/iot/callback?code=**&systemId=**& deviceId=***&typeId=***


```

**请求应答**
```
{
  "retCode": "00000",
  "retInfo": "操作成功",
"sn":"123456789",
  "data": null
}


```



[^-^]:常用图片注释
[pic1]:../Account/_media/_account/pic1.jpg
[pic2]:../Account/_media/_account/pic2.png
[pic3]:../Account/_media/_account/pic3.png
[pic4]:../Account/_media/_account/pic4.png
[pic5]:../Account/_media/_account/pic5.png
[pic6]:../Account/_media/_account/pic6.png
[pic7]:../Account/_media/_account/pic7.png
[pic8]:../Account/_media/_account/pic8.jpg

[session1]:../Account/_media/_account/session1.png
[session2]:../Account/_media/_account/session2.png

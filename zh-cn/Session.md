
!> **更新时间**：{docsify-updated}  


## 简介
	
>  会话分享服务为开发者提供登录应用App1用户的会话授权分享给应用App2使用，以及相关会话管理服务内容。

详细的业务流程参考下图：

![业务流程][session1]

1）请求需提供要分享到的AppId、ClientId，和当前终端的AccessToken，对AccessToken进行校验后，生成授权码，此授权码只能用于指定的AppId、ClientId和账户。授权码的有效期为10分钟（有效期可配置）。<br/>

2）用户收到授权码后，通过安全渠道传输给需要分享到的App，App通过提交自己的AppId、ClientId和授权码，系统对授权码与AppId和ClientId的对应关系和授权码的有效期进行校验，校验合格则为对应的账户生成新的RefreshToken和AccessToken返回给用户；校验失败，则返回授权码校验失败的错误。<br/>

3）RefreshToken的有效期默认永久有效，AccessToken的有效期默认25天。<br/>


4）仅通过登录或RefreshToken获取的AccessToken，方可获取授权码，通过授权码获取到的AccessToken及由RefreshToken再次获取的AccessToken不可用来申请授权码。<br/>



## 应用场景
>  关于面向普通用户的控制设备场景，可通过使用App登录用户会话分享给应用服务，获得用户授权应用的token，再通过调用平台的token版设备控制服务来进行设备控制。

![应用场景][session2]

## 接口列表

### 获取会话分享验证码  

> 通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程。  



##### 1、接口定义
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

##### 2、请求样例

**请求样例**

```
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

```
{
	"retCode":"00000",
	"retInfo":"成功",
	"code":"72f7b235dd3afee2c77907d160c66539850b3224da60cb6e6638809005f48ec5"
}

```

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|    
D00004|accessToken不存在或已过期|用于分享会话的accessToken过期或不存在    
D00026|禁止分享此会话|分享会话的accessToken不能分享给其他终端    

### 登录会话分享  

> 通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程。  

##### 1、接口定义
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

##### 2、请求样例

**请求样例**

```
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

```
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":"TGTV5FR3XH20S0B2E7G56V1CMQ4T67,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}

```

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|    
00001|登录成功但未接受最新版本隐私协议|老账号未接受隐私条款的情况，仅存在于海外环境    
B00001|缺少必填参数|code不存在   
B00002|参数类型错误|数据格式错误   

### 登录会话刷新  

> accessToken过期后，可以使用对应的refreshToken获取新的accessToken。  

##### 1、接口定义
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

##### 2、请求样例

**请求样例**

```
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

```
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":"TGTV5FR3XH20S0B2E7G56V1CMQ4T67,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}

```

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
00001|登录成功但未接受最新版本隐私协议|老账号未接受隐私条款的情况，仅存在于海外环境    
D00025|refreshToken不存在或已过期|     
D00005|Token不是由此应用创建，未通过token验证|refreshToken不是同一个设备创建   


### 查询会话分享列表  

> 通过accessToken，查询此用户进行会话分享的客户端列表。  

##### 1、接口定义
?> **接入地址：** `/uaccount/v2/auth/queryShareList`</br>
**HTTP Method：** POST  
**token验证：** 是      

**输入参数**

输入标准输入参数  
  

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareTokenInfoList|List|Body|必填|客户端列表信息</br>shareAppId 客户端appId</br> shareClientId 客户端clientId</br> state  0 授权，（已获取授权码，授权码未过期，未获得token）</br> 1 登录（已获得分享token）  


##### 2、请求样例

**请求样例**

```

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

```
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

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|参数不符合规则要求     
D00008|用户不合法|Token已过期或不存在       
D00030|未授权|分享获取的token不能取消授权     



### 取消会话分享  

> 用户提交登录获取的Token（或登录进行会话延期获取的Token）和会话分享的appId和clientId，Token校验成功且会话分享存在的情况下，将通过会话分享获取的RefreshToken和accessToken及会话延期的Token全部设置为失效。  

##### 1、接口定义
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


##### 2、请求样例

**请求样例**

```

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

```
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
B00004|参数不符合规则要求|参数不符合规则要求     
B00001|缺少必填参数|缺少必填参数         
D00008|用户不合法|Token已过期或不存在       
D00030|未授权|分享获取的token不能取消授权        



### 退出登录  

> 账号退出登录，会话accessToken失效。  

##### 1、接口定义
?> **接入地址：** `/uaccount/v1/security/logout`</br>
**HTTP Method：** POST  
**token验证：** 是         

**输入参数**  

输入标准输入参数   

  

**输出参数**

输出标准输出参数    


##### 2、请求样例

**请求样例**

```

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

```
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

##### 3、接口错误码

错误码|描述|情景 
:-|:-:|:-
D00005|Token不是由此应用创建，未通过token验证|操作成功       
D00016|你已退出或尚未登录|           









[^-^]:常用图片注释
[session1]:_media/_session/session1.png
[session2]:_media/_session/session2.png

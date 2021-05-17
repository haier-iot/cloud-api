#  介绍

## 应用场景
>  云云对接开发接口文档，为第三方应用程序登录海尔优家IOT平台，获取IOT平台安全令牌token提供指导。


## 服务说明
	
> 第三方应用程序登录海尔优家IOT平台时，须提供第三方云秘钥clientKey、第三方云秘钥clientSecret、以及海尔Iot云平台回调第三方接口验证授权码的URL地址。同时需要第三方云开发者在海尔开放平台[海极网][haigeek]创建自己的移动应用，并告知应用的AppId信息。



验证第三方平台用户的身份合法性。流程如下图：

![业务流程][thirdpartUserCheck]

## 接口列表

### 第三方云登录获取iot平台token

> 1. 头信息（header）为应用信息，请求体（body）包括业务信息
2.	Header中的签名是对应用信息进行身份鉴别




**接口描述**

?> **接入地址：** `/uaccount/v1/security/thirdpartUserLogin`</br>
**HTTP Method：** POST  


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
code|String|Body|必填|第三方云颁发的鉴权账号身份的随机数  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Body|必填|U+ iot平台安全令牌 
scope|String|Body|必填|授权范围 
expire|String|Body|必填|有效期时长 单位秒



**请求示例**

```java 
POST https://uws.haier.net/uaccount/v1/security/thirdpartUserLogin

POST data:
{"code":"8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"}

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
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

``` java
{
	"retCode":"00000",
	"retInfo":"成功",
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}


```

**接口错误码**

错误码|描述|情景 
:-|:-:|:-
D00013|登录失败|回调第三方接口时失败    
A00001|服务不可用|appId或接口未通过授权



### 第三方云验证授权码

> Iot云平台回调第三方接口验证授权码，由第三方云提供。


**接口描述**

?> **接入地址：** `由第三方云提供`</br>
**HTTP Method：** POST  
    

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
code|String|Body|必填|第三方云颁发的鉴权账号身份的随机数
clientKey|String|Body|必填|第三方云秘钥id
clientSecret|String|Body|必填|第三方云秘钥secret

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
openId|String|Body|必填|第三方云账号id
retCode|String|Body|必填|返回码
retInfo|String|Body|必填|返回码描述



**请求示例**

```java
url：

POST data:
{"code":"8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"," clientKey":"iot-client-project"," clientSecret ":"E3er4J88P=vb9"}

[no cookies]

Request Headers:
Connection: keep-alive
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 27


```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"openId":"34509854534"
}

```




**接口错误码**

错误码|描述|情景 
:-|:-:|:-
00000|成功|授权码验证成功  









[^-^]:常用图片注释
[haigeek]:(http://www.haigeek.com)
[thirdpartUserCheck]:../Account/_media/_account/thirdpartUserCheck.png

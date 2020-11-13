
!>  **更新时间**：{docsify-updated}  




### 授权服务接口

#### 获取授权token

> 通过授权码获取设备授权token请求头携带AI云server的 systemId，clientId（设备的deviceId），签名，body中携带授权码；获取设备授权token</br>
> 前置条件：AI云server接收IOT用户系统的授权码

##### 1、接口定义

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



##### 2、请求样例  

**用户请求**
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

#### 会话刷新

> accessToken过期后，可以使用对应的refreshToken获取新的accessToken</br>
> 前置条件：获取有效的授权，包括accessToken和refreshToken

###### 1、接口定义

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



##### 2、请求样例  

**用户请求**
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
### 回调服务接口

#### 接收授权码回调接口
> 接收授权码回调接口，并从海极网添加设备授权时录入</br>
> 前置条件：添加设备授权，并录入回调地址，海极网提供查询接口

##### 1、接口定义

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




##### 2、请求样例  

**用户请求**
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

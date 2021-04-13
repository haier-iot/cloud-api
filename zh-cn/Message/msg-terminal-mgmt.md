#  终端管理

?>  使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>
访问地址：`https://uws.haier.net/ums/v3`



## 终端注册

> 通过该接口，在系统中注册接收消息的终端，建立appId、userId、clientId、pushId之间的关系；该接口是使用ums和umse的先决条件。</br>
> 在注册时，若需要注册用户信息userId，则需要在header中传递accessToken


### 1、接口定义
?> **接入地址：** `/account/register`</br>
**HTTP Method：** POST

**输入参数：**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
channel|Integer|body|是|通道类型。</br>0代表极光，</br>1代表m2m通道，</br>2代表fcm通道，</br>3代表邮件，</br>4代表不使用或无通道。
pushId|String|body|否|终端的通道推动标识。</br>当channel为4时可以为空，其它情况不能为空，fcm通道时长度应该为152。
devAlias|String|body|否|设备别名
msgVersion|String|body|是|消息模型版本，对应消息模型中的version
mfrsChan|Integer|body|否|厂商通道标识 华为手机：0，华为FA：1，小米手机：2，Oppo手机：3，VIVO手机：4，苹果手机：5，FCM：6
mfrsRegId|String|body|否|厂商通道注册时的pushtoken
mfrsMsgOpen|Integer|body|否|厂商消息权限是否打开0：未打开1：打开


**输出参数:** 标准输出参数

### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/account/register

POST data:
{
	"channel":2,
	"pushId":"fbIyFJWV_M4:APA91bFYu308MAM5PyJxvUMiJKHT6yJl_O4z3HTyjr",
	"devAlias":"ios of yy",
	"msgVersion":"v3"
}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1545817794954 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************

```

**输出参数**
```
{
	"retCode":"00000",
	"retInfo":"success"
}
```

## 终端注销
> 注销已注册的终端信息</br>
> 一般在用户账号注销或APP卸载时用该接口


### 1、接口定义
?> **接入地址：** `/account/logout`</br>
**HTTP Method：** POST

**输入参数：** 无参数输入

**输出参数：** 标准输出参数

### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/account/logout

POST data:


[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1545817872035 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************
Content-Length: 0
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```
**输出参数**

```
{
	"retCode":"00000",
	"retInfo":"success"
}
```



## 获取用户终端列表
> 查询该用户下所有处于激活状态的终端</br>
> 根据userId查询，该userId注册的所有激活状态的终端信息

### 1、接口定义
?> **接入地址：** `/account/getTerminals`</br>
**HTTP Method：** POST

**输入参数：**  无输入参数

**输出参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<TerminalDto>|Body|是|终端信息列表


### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/account/getTerminals

POST data:


[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1566542876463 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 1234
accessToken: ************************
Content-Length: 0
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


```
**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":[{"userId":"4340515081329747","clientId":"1234","devAlias":"BLA-AL00-YY","appId":"MB-****-0000"}]}
```








[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:../Message/_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:../Message/_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:../Message/_media/_MessagePush/MessagePush_flow.png

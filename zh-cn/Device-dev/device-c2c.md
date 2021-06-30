# 云云对接类

## 简介  

### 目的
作为第三方云服务设备接入的依据和输入，为第三方云服务设备接入提供基础和指导。


### 使用场景
第三方生态平台及设备接入U+平台，且在设备底板与模块都无需改动的情况下，设备云端按U+云设备网关API的定义调用，最终可实现设备APP控制、设备语音控制、场景（远程）等功能。


## 解决方案

### 前置条件
设备已接入其他云，且设备配置绑定使用自有APP完成：

**1.	设备侧云端接入U+ IoT云云授权；**

**2.	设备侧云端按U+ IoT云设备接入API实现设备接入；**


### 方案介绍

![解决方案图][solution]  

方案说明：

  **1.	绑定设备**

第三方APP进行配置绑定设备，使其设备接入第三方平台，可通过自身平台进行连接和控制设备。

  **2.	授权**

通过OAuth获取第三方云用户设备控制权限，智家APP可以完成设备控制、场景控制，音箱可进行语音控制。

  **3.	云云对接（HTTPS）**

通过HTTPS双向认证，最终实现第三方设备接入U+平台，进而完成云云对接。


### 典型案例

  **1.	360平台对接**

接入360摄像头、可视门铃等设备到U+云平台，从而实现智家APP对其设备的查看及控制，支持语音控制、场景联动。

  **2.	涂鸦平台对接**

接入插座、灯光等设备到U+云平台，从而实现智家APP对其设备的状态查看及控制，支持语音控制、场景联动。


## 接入流程

设备通过第三方云服务接入，首先需要通过以下流程对第三方云服务进行注册或配置：  
  **1.	开发者（企业）登录海极网开发者中心，选择进入“平台授权信息”；**
  
![接入流程图][flow1] 

  **2.	平台信息维护，点击“编辑”添加平台基本信息，保存下一步，进行授权信息；**

![接入流程图][flow2]    

  **3.	按指引，编辑维护好Oauth授权配置、设备通讯配置，提交审核；**

![接入流程图][flow3]   

  **4.	完成上面的“平台授权信息”审核通过后，即可进入“我的产品”，按功能集创建产品功能集，接入方式选择“云云接入”，第三方平台选择“平台授权信息”的信息，按开发流程完成设备接入开发；**

![接入流程图][flow4]   


## 接口说明


### 公共说明

#### 第三方云服务实现的回调接口

第三方云服务需要实现并线下注册的回调接口如下表：

<table>
<tr>
    <td bgcolor="#3399FF"><b>回调类型</b></td>
    <td bgcolor="#3399FF"><b>必须实现</b></td>
	<td bgcolor="#3399FF"><b>说明</b></td>
</tr>
<tr>
    <td>获取用户设备列表</td>
	<td>是</td>
	<td>IoT平台主动获取用户授权的三方设备列表</td>
</tr>
<tr>
    <td>设备新增结果通知</td>
	<td>否</td>
	<td>IoT平台接收到三方设备新增推送后，将处理结果异步推送到三方指定接口(设备模型由三方适配时,会用到此接口)</td>
</tr>
<tr>
    <td>设备信息查询</td>
	<td>是</td>
	<td>IoT平台通过本接口来主动向第三方云服务定期同步设备信息</td>
</tr>
<tr>
    <td>设备属性读</td>
	<td>否</td>
	<td rowspan="3">IoT平台通过本接口将APP或APPServer对设备的控制指令发送给第三方云服务。如果不实现本接口，则对所有第三方云服务发送的远程控制请求会全部失败。
	
<font color="#FF0000">特别注意：设备标准模型文档中定义的组操作（getAllProperty，getAllAlarm）必须实现。</font></td>
</tr>
<tr>
    <td>设备属性写</td>
	<td>否</td>
</tr>
<tr>
    <td>设备操作</td>
	<td>是</td>
</tr>
<table>

IoT平台回调第三方云服务接口时，第三方云服务可通过以下两种方式对IoT平台身份进行验证：  
  **1.	将IoT平台访问IP配置为白名单；**  
  **2.	通过HTTPS双向认证，验证IoT平台证书；**  


#### HTTP接入地址定义


本文档提供的所有接口仅支持https协议请求，接入地址如下：  
`https://uws.haier.net:443/cloudgw/接口地址`   
访问非生产环境时，需要配置第三方云服务DNS，以便是域名指向对应的环境。  


#### HTTP Header参数定义


第三方云服务调用IoT平台HTTPS接口时，需要携带以下HTTP Header参数：  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|是|系统ID，40位以内字符，系统全局唯一标识  
sequenceId|String|Header|是|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用不带中横线的UUID，例如：e3480efdef6a410f9f60801707671bcc  
sign|String|Header|是|对请求进行签名运算产生的签名
timestamp|String|Header|是|Unix时间戳，当前UTC标准时间，精确到毫秒，例如：1557900146503    
Content-Type|String|Header|是|本接口Payload内容仅支持UTF-8编码的Json格式数据，值必须为application/json;charset=UTF-8

请求头信息样例（其中#为占位符，不代表具体含义）：  

```
	Connection: keep-alive  
	systemId: ######    
	deviceId: ######  
	sequenceId: e3480efdef6a410f9f60801707671bcc  
	sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015 （此字段需计算）  
	timestamp: 1557900146503  
	Content-Type: application/json;charset=UTF-8

```


### 云云账号互通

#### 第三方Oauth登录链接

通过智家App第三方设备入口，点击绑定第三方时，加载第三方Oauth 2.0 H5登录授权页（第三方提供），支持HTTPS

**HTTP Method：** GET

**请求参数** 

参数名|描述|必填|
:-|:-:|:-:|
client_id|为第三方给海尔颁发的应用标识|是|
response_type|为授权方式,这里固定为 code|是|
redirect_uri|海尔用户中心回调地址https://account-api.haier.net/api/user/third/callback/bind|是|
state|回调code时，原样返回|是|


#### 第三方根据code换token接口规范

支持HTTPS

**HTTP Method：** POST
**Content-Type：**  application/x-www-form-urlencoded

**请求参数** 

参数名|描述|必填|
:-|:-:|:-:|
grant_type|这里固定传authorization_code|是|
code|授权码|是|
redirect_uri|回调地址|是|

```

response：
	{
		"access_token": "63341c4d-5399-42a8-bb31-65837", 
		"token_type": "bearer", 
		"refresh_token": "7d0122b3-90e9-477e-8f9a-4c981bdc40a5",
		"expires_in": 863999, 
		"scope": "equipments.admin users.admin openid clients.admin",
		"user_id": 2005046073
	}

```


#### 第三方刷新登录接口规范

支持HTTPS

**HTTP Method：** POST
**Content-Type：**  application/x-www-form-urlencoded

**请求参数** 

参数名|描述|必填|
:-|:-:|:-:|
client_id|第三方下发的client_id|是|
client_secret|第三方下发的client_secret|是|
grant_type|固定值，传refresh_token|是|
refresh_token|接口3或者本接口返回的refresh_token|是|

```

response：
	{
		"access_token":"2YotnFZFEjr1zCsicMWpAA", 
		"expires_in":3600, 
		"scope": "openid profile email", 
		"token_type":"bearer", 
		"refresh_token":"2YotnFZFEjr1zCsicMWpAA", 
		"user_id": 2005046073
	}

```


#### 第三方取消授权回调接口规范

支持HTTPS

**HTTP Method：** POST
**Content-Type：**  application/x-www-form-urlencoded

**请求参数** 

参数名|描述|必填|
:-|:-:|:-:|
uId|海尔用户Id|是|
t|13位时间戳|是|
sign|签名，算法：sign=MD5(systemId+systemKey+t+uId).toUpperCase()，systemId、systemKey为接入海极网时分配的systemId、systemKey值|是|


### 设备授权接入

账号授权/解除授权及设备列表同步示意图：
![授权接入流程][access] 

#### 获取用户设备列表（IoT平台回调）

**使用说明**

海尔IoT平台主动获取用户授权三方设备列表

**接口描述**  

?> **接入地址：** `三方定义` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Body|是|三方用户token


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。
sequenceId|String|Body|是|请求唯一标识
openId|String|Body|是|三方用户Id
devices|Array|Body|是|设备列表


**devices定义:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
productId|String|Body|是|三方产品编号
machineId|String|Body|是|注册的设备的物理唯一标识，该标识在同一systemId下需唯一
machineName|String|Body|是|设备名称
online|int|Body|是|在线：1，离线：0


**示例**

**请求样例**

```
请求地址：三方提供
Body：
{
     "accessToken" : "***"
}

```

**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
	"sequenceId":"***",
	"openId"："***"，
	"devices"[{
			"productId":"***",
			"machineId":"***",
			"machineName":"***",
			"online" : "***"
			},
			{
			"productId":"***",
			"machineId":"***",
			"machineName":"***",
			"oneline":"***"
			}]
}

```


#### 设备新增结果通知（IoT平台回调）

**使用说明**

海尔IoT平台接收到三方设备新增推送后，将处理结果异步推送到三方指定接口

**接口描述**  

?> **接入地址：** `三方定义` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|海尔生成的设备唯一标识
uPlusCode|String|Body|是|成品编码，海尔型号的唯一标识
machineId|String|Body|是|注册的设备的物理唯一标识，该标识在同一systemId下需唯一
productId|String|Body|是|三方产品的标识
accessToken|String|Body|是|三方用户token
state|String|Body|是|新增注册状态（失败：0，成功：1）


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**示例**

**请求样例**

```
请求地址：三方提供
Body：
{
	"deviceId":"***",
	"uPlusCode":"***",
	"machineId":"***",
	"productId":"***",
	"accessToken":"***",
	"state":"***"
}

```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
}

```


#### 设备新增推送（调用IoT平台）

**使用说明**

用户在三方平台新增设备时，将已授权给海尔优家平台控制的类型设备通知到此接口

**接口描述**  

?> **接入地址：** `/v1/dev/add` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
openId|String|Body|是|OAuth获取到的三方用户的身份标识
accessToken|String|Body|是|OAuth获取到的三方用户token
productId|String|Body|是|三方产品编号
machineId|String|Body|是|注册的设备的物理唯一标识，该标识在同一systemId下需唯一
machineName|String|Body|是|设备名称
uPlusID|String|Body|是|海尔设备typeId
uPlusCode|String|Body|是|海尔设备产品编号


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**示例**

**请求样例**

```
请求地址：https://api.haigeek.com/customgateway/v1/dev/add
Body：
{
	"openId":"***",
	"productId":"binding",
	"machineId":"***",
	"machineName":"***",
	"uPlusID":"***",
	"uPlusCode":"***"
}

```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
}

```

#### 设备移除推送（调用IoT平台）

**使用说明**

用户在三方平台移除设备时，将已授权给海尔优家平台控制的类型设备通知到此接口

**接口描述**  

?> **接入地址：** `/v1/dev/remove` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
openId|String|Body|是|OAuth获取到的三方用户的身份标识
accessToken|String|Body|是|OAuth获取到的三方用户token
productId|String|Body|是|三方产品型号
machineId|String|Body|是|注册的设备的物理唯一标识，该标识在同一systemId下需唯一


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**示例**

**请求样例**

```
请求地址：https://api.haigeek.com/customgateway/v1/dev/remove
Body：
{
	"openId":"***",
	"productId":"binding",
	"machineId":"***"
}

```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
}

```


### 设备上报及控制

控制下发及状态上报示意图：
![上报及控制流程][control] 

从示意图可以看出此部分分为主动上报、状态查询、设备控制，接口的介绍按照此顺序进行介绍：


#### 设备属性状态上报

**使用说明**

设备注册后，即可在适合的时机上报设备的属性状态，属性上报会实时通过uSDK发送到App上用来展示。
为了保证设备属性变化展示的实时性，设备属性状态一旦发生变化，应该立即上报。设备每次都需要上报当前全部属性状态，以便使用方可以实时得知设备当前最新的全部属性状态。


**接口描述**  

?> **接入地址：** `/v1/dev/property` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID,字符串长度范围：1~32
propertys|Property数组|Body|是|设备属性列表,数组长度范围：1~256


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**Propertys定义:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|属性名，字符串长度范围：1~64
value|String|Body|是|属性值，字符串长度范围：0~64

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/property
Body：
{
	 "deviceId":"***",
     "propertys":[
        { "name":"***","value":"***"},
        { "name":"***","value":"***"},
        ……
    ]

}

```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
}

```


#### 设备报警状态上报

**使用说明**

设备注册后，即可在适合的时机上报设备的报警状态，设备报警上报会实时通过uSDK发送到App上用来展示。
为了保证设备报警变化展示的实时性，设备报警状态一旦发生变化，应该立即上报。设备每次都需要上报当前正在发生的全部报警，以便使用方可以实时得知设备当前最新的全部报警状态。


**接口描述**  

?> **接入地址：** `/v1/dev/alarm` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID,字符串长度范围：1~32
alarms|alarm数组|Body|是|设备报警列表,数组长度范围：1~256


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**Alarms定义:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|报警名，字符串长度范围：1~64
value|String|Body|是|报警值，某些报警仅有报警名时，报警值填空字符串，字符串长度范围：0~64

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/alarm
Body：
{
	 "deviceId":"***",
     "alarms":[
        {"name":"***","value":"***"},
        {"name":"***","value":"***"},
        ……
    ]

}

```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"***"
}

```


#### 设备上线（调用IoT平台）

**使用说明**

IoT平台会维护设备的在线状态，设备注册到IoT平台时，会携带设备初始在线状态，随后可以通过设备上线与设备下线更新设备在线状态。IoT平台仅被动维护设备在线状态，任何设备在线状态变化，都需要第三方云服务主动维护。本接口会直接更新设备在IoT平台的在线状态，即不论设备当前是否在线，均会直接更新为在线。

**接口描述**  

?> **接入地址：** `/v1/dev/online` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID，字符串长度范围：1~32


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/online
Body：
{
     "deviceId" : "***"
}


```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}
```


#### 设备下线（调用IoT平台）

**使用说明**

IoT平台会维护设备的在线状态，设备注册到Iot平台时，会携带设备初始在线状态，随后可以通过设备上线与设备下线更新设备在线状态。IoT平台仅被动维护设备在线状态，任何设备在线状态变化，都需要第三方云服务主动维护。本接口会直接更新设备在IoT平台的在线状态，即不论设备当前是否离线，均会直接更新为离线。

**接口描述**  

?> **接入地址：** `/v1/dev/offline` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID，字符串长度范围：1~32


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/offline
Body：
{
     "deviceId" : "***"
}


```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}
```


#### 设备信息查询（IoT平台回调）

**使用说明**

IoT平台通过本接口来主动向第三方云服务定期同步设备信息，以便应对由于服务或网络中断导致的设备信息或在线状态不同步问题。

**接口描述**  

?> **接入地址：** `***` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|Body|是|回调类型，值固定为：devInfoQuery
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
machineId|String|Body|是|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
rptProperty|Boolean|Body|是|是否立即上报属性状态：如果值为true，则第三方云服务在应答本接口后，应立即调用“设备属性状态上报”接口上报设备当前全部属性状态；如果值为false，则忽略
rptAlarm|Boolean|Body|是|是否立即上报报警状态：如果值为true，则第三方云服务在应答本接口后，应立即调用“设备报警状态上报”接口上报设备当前全部报警状态；如果值为false，则忽略


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W20010
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。
uPlusID|String|Body|是|U+产品编号，接入U+产品的唯一标识，由U+统一定义，字符串长度范围：64
online|Boolean|Body|是|设备当前在线状态：true为在线；false为离线

**示例**

**请求样例**

```
请求地址：https://***
Body：
{
    "callbackType":"devInfoQuery",
    "deviceId":"***",
    "rptProperty":true | false,
    "rptAlarm":true | false
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***",
    "uPlusID":"***",
    "online":true | false

}

```


#### 设备控制请求--属性读（IoT平台回调）

**使用说明**

设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。
对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。


**接口描述**  

?> **接入地址：** `***` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|Body|是|回调类型，值固定为：propertyRead
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
machineId|String|Body|是|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|Body|否|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
propertyName|String|Body|是|要读取的设备属性名，字符串长度范围：无限制


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W20010
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://***
Body：
{
	"callbackType":"propertyRead",
	"deviceId":"***",
	"machineId":"***",
	"token":"***",
	"sn":"***",
	"propertyName":"***"
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


#### 设备控制应答--属性读（调用IoT平台）

**使用说明**

设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。
如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**  

?> **接入地址：** `/v1/dev/opack/propertyRead` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|Body|是|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理，可返回错误码见“第三方云设备控制异步应答错误码”定义
propertyName|String|Body|否|要读取的设备属性名，字符串长度范围：1~64，仅errNo不为0时（表示控制失败），本字段可以不存在
propertyValue|String|Body|否|读取到的设备属性值，字符串长度范围：0~64，仅errNo不为0时（表示控制失败），本字段可以不存在


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/propertyRead
Body：
{
    "deviceId":"***",
    "sn":"***",
    "errNo":"***",
    "propertyName":"***",
    "propertyValue":"***"
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


#### 设备控制请求--属性写（IoT平台回调）

**使用说明**

设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。
对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。


**接口描述**  

?> **接入地址：** `***` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|Body|是|回调类型，值固定为：propertyWrite
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
machineId|String|Body|是|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|Body|否|要设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
propertyName|String|Body|是|要写入的设备属性名，字符串长度范围：无限制
propertyValue|String|Body|是|要写入的设备属性值，字符串长度范围：无限制


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W20010
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://***
Body：
{
	"callbackType":"propertyWrite",
	"deviceId":"***",
	"machineId":"***",
    "token":"***",
    "sn":"***",
    "propertyName":"***",
    "propertyValue":"***"
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


#### 设备控制应答--属性写（调用IoT平台）

**使用说明**

设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。
如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**  

?> **接入地址：** `/v1/dev/opack/propertyWrite` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|Body|是|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理，可返回错误码见“第三方云设备控制异步应答错误码”定义


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/propertyWrite
Body：
{
    "deviceId":"***",
    "sn":"***",
    "errNo":"***",
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


#### 设备控制请求--操作（IoT平台回调）

**使用说明**

设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。
对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。

<font color="#FF0000">特别注意：设备标准模型文档中定义的组操作（getAllProperty，getAllAlarm）必须实现。</font></td>


**接口描述**  

?> **接入地址：** `***` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|Body|是|回调类型，值固定为：operation
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
machineId|String|Body|是|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|Body|否|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
operationName|String|Body|是|设备操作名，字符串长度范围：无限制
parameters|Parameter数组|Body|是|设备操作请求参数列表。如当前操作请求无参数，则值为空数组[]，数组长度范围：无限制


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W20010
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**Parameter定义:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|参数名，字符串长度范围：无限制
value|String|Body|是|参数值，字符串长度范围：无限制

**示例**

**请求样例**

```
请求地址：https://***
Body：
{
    "callbackType":"operation",
	"deviceId":"***",
	"machineId":"***",
	"token":"***",
    "sn":"***",
    "operationName":"***",
    "parameters":[
        {"name":"***","value":"***"},
        {"name":"***","value":"***"},
        ……
    ]

}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


#### 设备控制应答--操作（调用IoT平台）

**使用说明**

设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。
如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**  

?> **接入地址：** `/v1/dev/opack/operation` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备ID，字符串长度范围：1~32
sn|String|Body|是|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|Body|是|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理，可返回错误码见“第三方云设备控制异步应答错误码”定义
parameters|Parameter数组|Body|否|设备操作应答参数列表，如当前操作应答无参数，则值为空数组[]，数组长度范围：0~256，仅errNo不为0时（表示控制失败），本字段可以不存在；


**输出参数:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误，见“错误码定义”章节。可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上。字符串长度范围：0~256。详细错误情况以retCode在本接口文档中对应描述为准。


**Parameter定义:**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|参数名，字符串长度范围：1~64
value|String|Body|是|参数值，字符串长度范围：0~64

**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/operation
Body：
{
    "deviceId":"***",
    "sn":"***",
    "errNo":"***",
    "parameters":[
        {"name":"***", "value":"***"},
        {"name":"***", "value":"***"},
        ……
    ]

}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"***"
}

```


## 错误码定义
### 公共错误码  

错误码W00001~W10000及部分其他错误码，为公共错误码，及IoT平台的接口与第三方云服务实现的回调接口都会出现的错误码，定义如下：  

| **错误码** | **描述** |
| :-------: |:-----:|
|00000|成功|
|W00001|通用错误，表示暂时未定义或无法区分的错误。|
|W00002|内部错误，表示系统内部发生且不对外开放的错误。正常业务下不会返回该错误。|
|W00003|格式错误，即Payload不是UTF-8编码的Json格式|
|W00004|参数错误，即接口Payload中某必填字段不存在、某字段值不合法等|
|W01001|sequenceId错误，即HTTP Header中的sequenceId字段为空或者超长|
|W01002|时间戳错误，即时间戳与当前时间相差较大|
|W01003|接口无权限访问，例如部分需要访问秘钥的接口秘钥错误|
|C00001|systemId错误，即HTTP Header中的systemId字段为空或在IoT平台不存在|
|C00002|无访问授权，指设备云接入网关未在UWS接口注册|
|D00001|HTTP Header签名校验错误|

### IoT平台接口错误码  

错误码W10000~W20000，为仅IoT平台的接口出现的错误码，定义如下：  

| **错误码** | **描述** |
| :-------: |:-----:|
|W10001|设备未注册|
|W10002|设备注册时，U+产品编号不存在|
|W10003|设备注册时，设备产品信息签名错误，通常为deviceKey错误导致|
|W10004|设备绑定时，用户token无效|
|W10005|设备绑定时，用户绑定时间窗未开启|
|W10006|设备绑定时，用户已达到绑定设备数量上限|
|W10007|设备控制应答sn错误|
|W10008|设备注册时，产品型号编号不存在|

### 第三方云服务回调接口错误码 

错误码W20000~W30000，为仅第三方云服务实现的回调接口出现的错误码，定义如下：  


| **错误码** | **描述** |
| :-------: |:-----:|
|W20010|设备不存在|

### <span id="jump1">第三方云设备控制异步应答错误码</span>   
<!--  <h5 id="1">第三方云设备控制异步应答错误码</h5>--> 

设备控制异步应答错误码为整数类型，该错误码会直接返回给App处理，定义如下：  

| **错误码** | **描述** |
| :-------: |:-----:|
|0 |成功。|
|1 |通用错误码，表示暂时未定义或无法区分的错误。|
|3 |格式错误，表示PayLoad消息体格式错误，例如不是合法的Json格式。|
|4 |缺少参数，表示PayLoad消息体中缺少部分必填字段。|
|11|控制指令执行超时。|
|12|无效命令。主要指命令执行逻辑错误。例如关机状态下再关机等。|
|13|非法值。主要指设备操作的操作值非法。例如温度设置的温度值越界。|
|16|设备不在线。主要指向设备下发控制指令时，设备实际已经离线，则需在异步应答中通知平台设备不在线，平台收到该错误码后会将设备置为不在线状态，不会再向设备下发控制命令，直到设备主动上线，或者设备信息查询回调查到设备在线。|  
|17|不支持的命令。主要指读属性或写属性请求时，设备无此属性，或者操作时，设备不支持此操作。|




[^-^]:常用图片注释
[solution]:../Device-dev/_media/_device-c2c/solution.png
[flow1]:../Device-dev/_media/_device-c2c/flow1.png
[flow2]:../Device-dev/_media/_device-c2c/flow2.png
[flow3]:../Device-dev/_media/_device-c2c/flow3.png
[flow4]:../Device-dev/_media/_device-c2c/flow4.png
[access]:../Device-dev/_media/_device-c2c/access.png
[control]:../Device-dev/_media/_device-c2c/control.png

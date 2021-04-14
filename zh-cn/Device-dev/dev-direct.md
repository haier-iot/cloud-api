# 云设备类


## 简介

### 目的

> 作为第三方云服务设备接入的依据和输入，为第三方云服务设备接入提供基础和指导。

### 使用场景

> 第三方品牌设备接入，设备底板无需改动，模块集成配网SDK，设备云端按U+云设备网关API的定义调用


## 解决方案

### 方案说明

![解决方案图][solution]

**适用设备**  
	1、设备已接入自有云（Server）；  
	2、需使用U+APP配置/绑定设备；  
	3、使用U+APP控制设备；

### 绑定方式

**方式1**

![绑定方式1][bind1]

**步骤**  
1、APP需要连上目标wifi  
2、设备开启蓝牙等待APP发现设备  
3、APP发现设备输入账号密码  
4、摄像头扫描APP生成的二维码获取到用户信息  
5、摄像头连上平台，调用 注册等接口，预绑定完成以后通知用户   
6、用户点击确定按钮确认绑定 ，通知云设备server并发送相关参数信息  
7、云设备server去调iot绑定接口进行绑定    
8、将绑定成功结果通知给用户


**方式2**

![绑定方式2][bind2]

**步骤**  
1、App（集成第三方SDK）发现第三方设备  
2、填写目标wifi账号密码使第三方设备设备连上第三方平台  
3、三方设备连上平台之后会通知第三方SDK ，三方SDK通知app  
4、APP发起绑定，云设备server调用设备注册 设备开窗 用户绑定接口  
5、将绑定成功结果通知用户


## 接入流程

**开发者（企业）登录海极网开发者中心，按功能集创建 ，接入方式选择“云设备”**

![接入流程][flow]


## 设备接入相关接口

### 设备注册


**使用说明**

> 设备接入IoT平台，需要先在IoT平台注册，否则后续对该设备的所有动作均会失败。设备可以重复注册，machineId是设备物理唯一标识，对于同一个systemId的同一个machineId，重复注册时返回的deviceId始终是相同的，设备信息（uPlusID、online）也会直接覆盖该设备的原有信息。


**接口描述**

?> **接入地址：** `/v1/dev/reg`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
machineId|String|body|必填|要注册的设备的物理唯一标识，该标识在同一systemId下需唯一，字符串长度范围：1~32，（字符集可取值[0-9][A-Z] [a-z]）：对于WIFI类设备，建议使用MAC地址，格式为全大写无连接符十六进制字符串，例如CC1122334455；对于2G或NB类设备，建议使用IMEI地址，格式为全数据字符串，例如860123456789876
uPlusID|String|body|必填|U+产品编号，接入U+产品的唯一标识，由U+统一定义，字符串长度范围：64
devSign|String|body|必填|设备产品信息签名，用于设备身份认证，全小写十六进制字符串，字符串长度范围：64，例如：996a4a51cfc228f7d5044319767d6f792f106460e86a9050b79dcee0a6dd31d9。算法如下（+表示字符串拼接）：SHA256（machineId+uPlusID+deviceKey+ timestamp），其中：deviceKey为海极网创建uPlusID时分配；timestamp为HTTP Header中的时间戳字段；
online|Boolean|body|必填|设备当前在线状态：true为在线；false为离线；
uPlusCodeT|String|body|非必填|U+产品整机型号编码（即成品编码）。设备上报的型号编码必须是在海极网注册或录入的，否则设备注册会失败；如果该设备型号编码未被用户修改过，则设备上报型号信息后，会直接覆盖平台该设备现有型号信息；如果该设备型号编码当前已由用户修改，则本次注册时将不会覆盖原值，注册仍然返回成功。字符串长度范围：9~11。注：该字段为非必填，请求body中可以不含该字段或设置该字段值为null，但不可设置为长度为0的字符串""


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10002、W10003、W10008
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
deviceId|String|Body|是|注册成功后Iot平台分配的设备ID，字符串长度范围：1~32


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/reg
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	    "deviceMac" : "***",
	    "uPlusID" : "***",
	    "devSign" : "***",
		"online" : true | false,
	    "uPlusCodeT" : "***"
	
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***",
    "deviceId" : "***"
}

```


### 设备绑定

**使用说明**

> 备在IoT平台注册，即可将该设备绑定在一个优家用户下。调用本接口绑定设备时，设备绑定必须先由uSDK对要绑定的用户开启绑定时间窗（每次开启，20分钟内有效，且只能绑定一次），否则即绑定失败。如果绑定成功，本设备与之前其他用户的绑定关系会被自动解除。一个优家用户，最多可以绑定300个设备。如果存在alias字段，设备绑定成功后，会设置设备的别名，如果绑定失败，则不会设置。


**接口描述**

?> **接入地址：** `/v1/dev/bind`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
token|String|body|必填|设备要绑定的用户Token，字符串长度范围：1~32
deviceName|String|body|非必填|设备名称。如果不传该字段则默认生成“应用分类+deviceId后四位”，字符串长度范围：1~32。注：该字段为非必填，请求body中可以不含该字段或设置该字段值为null，但不可设置为长度为0的字符串""


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001、W10004、W10005、W10006
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/bind
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
		"deviceId" : "***",
		"token" : "***"
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备上线

**使用说明**

> IoT平台会维护设备的在线状态，设备注册到IoT平台时，会携带设备初始在线状态，随后可以通过设备上线与设备下线更新设备在线状态。IoT平台仅被动维护设备在线状态，任何设备在线状态变化，都需要第三方云服务主动维护。本接口会直接更新设备在IoT平台的在线状态，即不论设备当前是否在线，均会直接更新为在线。


**接口描述**

?> **接入地址：** `/v1/dev/online`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/online
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
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

### 设备下线

**使用说明**

> IoT平台会维护设备的在线状态，设备注册到IoT平台时，会携带设备初始在线状态，随后可以通过设备上线与设备下线更新设备在线状态。IoT平台仅被动维护设备在线状态，任何设备在线状态变化，都需要第三方云服务主动维护。本接口会直接更新设备在IoT平台的在线状态，即不论设备当前是否离线，均会直接更新为离线。


**接口描述**

?> **接入地址：** `/v1/dev/offline`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/offline
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
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


### 开启设备绑定时间窗

**使用说明**

> 设备也可以通过优家APP直接进行绑定，但需要在优家APP绑定之前，必须调用本接口对设备开启绑定时间窗，否则即会绑定失败；对设备开启绑定时间窗后，20分钟内有效，如果调用本接口时当前设备的绑定时间窗已开启，则重置时间窗有效期为20分钟；


**接口描述**

?> **接入地址：** `/v1/dev/setBindTimeWindow`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/setBindTimeWindow
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
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


### 设备信息查询

**使用说明**

> IoT平台通过本接口来主动向第三方云服务定期同步设备信息，以便应对由于服务或网络中断导致的设备信息或在线状态不同步问题。


**接口描述**

?> **接入地址：** `第三方提供`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|body|必填|回调类型，值固定为：devInfoQuery
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
token|String|body|非必填|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段。
machineId|String|body|必填|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
rptProperty|Boolean|body|必填|是否立即上报属性状态：如果值为true，则第三方云服务在应答本接口后，应立即调用“设备属性状态上报”接口上报设备当前全部属性状态；如果值为false，则忽略
rptAlarm|Boolean|body|必填|是否立即上报报警状态：如果值为true，则第三方云服务在应答本接口后，应立即调用“设备报警状态上报”接口上报设备当前全部报警状态，如果值为false，则忽略


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W20010
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
uPlusID|String|body|必填|U+产品编号，接入U+产品的唯一标识，由U+统一定义。若第三方没返回该字段,则平台不处理，字符串长度范围：64。
online|Boolean|body|必填|设备当前在线状态：true为在线；false为离线；若第三方没返回该字段,则平台不处理


**示例**

**请求样例**

```
请求地址：https://***
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	"callbackType" : "devInfoQuery",
	"deviceId" : "***",
	"token" : "***",
    "rptProperty" : true | false,
    "rptAlarm" : true | false

	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***",
    "uPlusID" : "***",
    "online" : true | false
}

```


### 设备注册（基于模块）

**使用说明**

> 设备接入IoT平台，需要先在IoT平台注册，否则后续对该设备的所有动作均会失败，设备可以重复注册。


**接口描述**

?> **接入地址：** `/v1/dev/regWithModule`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
moduleType|String|body|必填|设备模块类型，目前仅支持nbiotmodule和4gmodule
moduleId|String|body|必填|设备模块id，目前仅支持IMEI号，字符串长度范围：15~17，（字符集可取值[0-9]）
subId|String|body|非必填|子设备id，非必填，即设备硬件ID下子设备唯一标识，字符串长度范围：1~16，（字符集可取值[0-9][A-Z] [a-z]）。注：该字段为非必填，请求body中可以不含该字段或设置该字段值为null，但不可设置为长度为0的字符串""
uPlusID|String|body|必填|U+产品编号，接入U+产品的唯一标识，由U+统一定义，字符串长度范围：64
devSign|String|body|必填|设备产品信息签名，用于设备身份认证，全小写十六进制字符串，字符串长度范围：64。算法如下（+表示字符串拼接）：SHA256（moduleType + moduleId + subId +uPlusID+deviceKey+ timestamp）其中：deviceKey为海极网创建uPlusID时分配；timestamp为HTTP Header中的时间戳字段；
online|Boolean|body|必填|设备当前在线状态：true为在线；false为离线；
uPlusCodeT|String|body|非必填|U+产品整机型号编码（即成品编码），设备上报的型号编码必须是在海极网注册或录入的，否则设备注册会失败。如果该设备型号编码未被用户修改过，则设备上报型号信息后，会直接覆盖平台该设备现有型号信息。如果该设备型号编码当前已由用户修改，则本次注册时将不会覆盖原值，注册仍然返回成功。字符串长度范围：9~11。注：该字段为非必填，请求body中可以不含该字段或设置该字段值为null，但不可设置为长度为0的字符串""


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10002、W10003、W10008
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
deviceId|String|body|必填|注册成功后Iot平台分配的设备ID，字符串长度范围：1~32


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/regWithModule
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	"moduleType" : "4gmodule",
	"moduleId" : "123456789012345",
	"subId" : "sub1",
    "uPlusID" : "***",
    "devSign" : "***",
	"online" : true | false,
    "uPlusCodeT" : "***"
	}

```
**请求应答**
```
{
	"retCode" : "00000",
    "retInfo" : "***",
    "deviceId" : "***"
}

```


### 开启设备绑定时间窗（基于模块）

**使用说明**

> 支持第三方服务开启设备绑定时间窗（基于模块），指定一个模块类别及模块id即可开启关联的所有设备的绑定时间窗。
对设备开启绑定时间窗后，20分钟内有效，如果调用本接口时当前设备的绑定时间窗已开启，则重置时间窗有效期为20分钟。


**接口描述**

?> **接入地址：** `/v1/dev/setBindTimeWindowWithModule`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
moduleType|String|body|必填|设备模块类型，目前仅支持nbiotmodule与4gmodule两个值
moduleId|String|body|必填|设备模块id，目前仅支持IMEI号，字符串长度范围：15~17，（字符集可取值[0-9]）


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10021，W10022
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
resultDetail|Object|Body|必填|resultDetail为与入参模块关联的所有已注册设备开启时间窗结果：当retCode值为00000，表示所有设备绑定时间窗开启成功，resultDetail示例：{"devId1":true, "devId2":true}；当retCode值为W10021，表示部分设备绑定时间窗开启失败，resultDetail示例：{"devId1":true, "devId2":false}，true表示devId1开启时间窗成功，false表示devId2开启时间窗失败；当retCode值为W10022，表示根据入参模块信息未查询到任何关联设备，未开启任何设备时间窗，resultDetail示例：{}


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/setBindTimeWindowWithModule
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	"moduleType" : "4gmodule",
	"moduleId" : "123456789012345"
	}

```
**请求应答**
```
{
    "retCode" : "W10021",
    "retInfo" : "开启设备绑定时间窗（基于模块）时，部分开启失败",
    "resultDetail" : {"devId1": true, "devId2": false}
}

```


## 设备上报相关接口

### 设备属性状态上报

**使用说明**

> 设备注册后，即可在适合的时机上报设备的属性状态，属性上报会实时通过uSDK发送到App上用来展示。为了保证设备属性变化展示的实时性，设备属性状态一旦发生变化，应该立即上报。设备每次都需要上报当前全部属性状态，以便使用方可以实时得知设备当前最新的全部属性状态。


**接口描述**

?> **接入地址：** `/v1/dev/property`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
propertys|Property数组|body|必填|设备属性列表，Property类型定义见下面章节，数组长度范围：1~256


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**Property类型定义**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|必填|属性名，字符串长度范围：1~64
value|String|Body|必填|属性值，字符串长度范围：0~64


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/property
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	"deviceId" : "***",
    "propertys" : [
        { "name" : "***", "value" : "***" },
        { "name" : "***", "value" : "***" },
		……
		]
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备报警状态上报

**使用说明**

> 设备注册后，即可在适合的时机上报设备的报警状态，设备报警上报会实时通过uSDK发送到App上用来展示。为了保证设备报警变化展示的实时性，设备报警状态一旦发生变化，应该立即上报。设备每次都需要上报当前正在发生的全部报警，以便使用方可以实时得知设备当前最新的全部报警状态。


**接口描述**

?> **接入地址：** `/v1/dev/alarm`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
alarms|Alarm数组|body|必填|设备报警列表，Alarm类型定义见下面章节，数组长度范围：1~256


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**Alarm类型定义**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|必填|报警名，字符串长度范围：1~64
value|String|Body|必填|报警值，字符串长度范围：0~64


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/alarm
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "deviceId" : "***",
    "alarms" : [
        { "name" : "***", "value" : "***" },
        { "name" : "***", "value" : "***" },
        ……
    	]
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 新增设备主从关系

**使用说明**

> 注册两个设备后，可以使用本接口新增这两个设备之间的主从关系到云平台，以供上层业务使用。


**接口描述**

?> **接入地址：** `/v1/dev/saveRelationship`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
mDevId|String|body|必填|主设备ID，字符串长度范围：1~32
mRole|Int|body|必填|主设备角色，可取值：1.普通设备；2.网关设备；4.子设备；
sDevId|String|body|必填|从设备ID，字符串长度范围：1~32
sRole|Int|body|必填|从设备角色，可取值：3.附件设备；4.子设备；
bind|Int|body|必填|是否由平台代绑定从设备，可取值：0.不代绑定；1.代绑定；值为1时，平台是否确实进行代绑定行为，还要取决于从设备的角色定义，例如附件设备平台会进行代绑定，而普通设备则不会（实际逻辑以业务中台定义为准）


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10009~ W10019
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/saveRelationship
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "mDevId" : "***",
    "mRole" : 2, 
    "sDevId" : "***",
    "sRole" : 4, 
	"bind" : 1
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 删除设备主从关系

**使用说明**

> 新增设备主从关系后，如果主从关系不再存在，可以通过本接口删除该主从关系。


**接口描述**

?> **接入地址：** `/v1/dev/deleteRelationship`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
mDevId|String|body|必填|主设备ID，字符串长度范围：1~32
sDevId|String|body|必填|从设备ID，字符串长度范围：1~32
unbind|Int|body|必填|是否由平台代解绑从设备，可取值：0.不代解绑；1.代解绑；值为1时，平台是否确实进行代解绑行为，还要取决于从设备的角色定义，例如附件设备平台会进行代解绑，而普通设备则不会（实际逻辑以业务中台定义为准）


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10009~ W10019
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/deleteRelationship
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "mDevId" : "***",
	"sDevId" : "***",
    "unbind" : 1
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 删除主设备下所有主从关系

**使用说明**

> 给定主设备id，可以通过本接口删除该主设备下所有的主从关系。


**接口描述**

?> **接入地址：** `/v1/dev/deleteAllRelationship`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
mDevId|String|body|必填|主设备ID，字符串长度范围：1~32


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10023~ W10025
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/deleteAllRelationship
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
	"mDevId" : "***"
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


## 设备控制相关接口

### 设备控制请求--属性读

**使用说明**

> 设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。


**接口描述**

?> **接入地址：** `***`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|body|必填|回调类型，值固定为：propertyRead	
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
machineId|String|body|必填|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|body|非必填|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
propertyName|String|body|必填|要读取的设备属性名，字符串长度范围：无限制


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W20010
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://***
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "callbackType" : "propertyRead",
	"deviceId" : "***",
	"machineId" : "***",
	"token" : "***",
    "sn" : "***",
    "propertyName" : "***"
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备控制应答--属性读

**使用说明**

> 设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**

?> **接入地址：** `/v1/dev/opack/propertyRead`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|body|必填|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理，可返回错误码见“第三方云设备控制异步应答错误码”定义
propertyName|String|body|非必填|要读取的设备属性名，字符串长度范围：1~64。仅errNo不为0时（表示控制失败），本字段可以不存在；注：该字段为非必填，请求body中可以不含该字段或设置该字段值为null，但不可设置为长度为0的字符串""
propertyValue|String|body|非必填|读取到的设备属性值，字符串长度范围：0~64，仅errNo不为0时（表示控制失败），本字段可以不存在


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/propertyRead
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "deviceId" : "***",
    "sn" : "***",
    "errNo" : ***,
    "propertyName" : "***"
    "propertyValue" : "***"
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备控制请求--属性写

**使用说明**

> 设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。


**接口描述**

?> **接入地址：** `***`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
callbackType|String|body|必填|回调类型，值固定为：propertyWrite	
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
machineId|String|body|必填|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|body|非必填|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
propertyName|String|body|必填|要写入的设备属性名，字符串长度范围：无限制
propertyValue|String|body|非必填|要写入的设备属性值，字符串长度范围：无限制


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W20010
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://***
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "callbackType" : "propertyWrite",
	"deviceId" : "***",
	"machineId" : "***",
	"token" : "***",
    "sn" : "***",
    "propertyName" : "***",
    "propertyValue" : "***"
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备控制应答--属性写

**使用说明**

> 设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**

?> **接入地址：** `/v1/dev/opack/propertyWrite`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-	
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|body|必填|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理。可返回错误码见“第三方云设备控制异步应答错误码”定义。


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/propertyWrite
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "deviceId" : "***",
    "sn" : "***",
    "errNo" : ***
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备控制请求--操作

**使用说明**

> 设备注册到IoT平台并且当前在线，App或AppServer控制设备时，即会使用本回调接口通过第三方云服务发送给设备执行。对设备控制是一个异步请求，第三方云服务收到本请求时需要立即返回，即接口返回时仅代表第三方云服务收到本控制请求，而实际向设备下发及设备执行结果，第三方云服务需要通过主动调用“设备控制应答”接口发送至IoT平台。特别注意：设备标准模型文档中定义的组操作（getAllProperty，getAllAlarm）必须实现。


**接口描述**

?> **接入地址：** `***`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-	
callbackType|String|body|必填|回调类型，值固定为：operation	
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
machineId|String|body|必填|设备注册时使用的设备物理唯一标识，字符串长度范围：1~32
token|String|body|非必填|设备绑定的第三方用户token，字符串长度范围：1~32，如当前设备没有授权的第三方用户，则无此字段
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
propertyName|String|body|必填|设备操作名，字符串长度范围：无限制
parameters|Parameter数组|body|必填|设备操作请求参数列表，如当前操作请求无参数，则值为空数组[]，字符串长度范围：无限制


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W20010
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**Parameter类型定义**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|必填|参数名，字符串长度范围：1~64
value|String|Body|必填|参数值，字符串长度范围：0~64


**示例**

**请求样例**

```
请求地址：https://***
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "callbackType" : "operation",
	"deviceId" : "***",
	"machineId" : "***",
	"token" : "***",
    "sn" : "***",
    "operationName" : "***",
    "parameters" : [
        { "name" : "***", "value" : "***" },
        { "name" : "***", "value" : "***" },
        ……
    ]
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
}

```


### 设备控制应答--操作

**使用说明**

> 设备向第三方云服务发送，或者第三方云服务从设备取到设备控制应答后，需要调用本接口，将控制结果发送到IoT平台。如果控制序列号与请求不同，本接口也会返回成功，但会直接丢弃本应答消息。


**接口描述**

?> **接入地址：** `/v1/dev/opack/operation`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-	
deviceId|String|body|必填|设备ID，字符串长度范围：1~32
sn|String|body|必填|控制序列号，对于一次控制，控制应答的序列号需要同控制请求的序列号相同，字符串长度范围：1~64
errNo|Integer|body|必填|设备控制应答错误码，表示本次设备控制结果，该错误码会直接返回至App处理。可返回错误码见“第三方云设备控制异步应答错误码”定义。
parameters|Parameter数组|body|非必填|设备操作应答参数列表，如当前操作应答无参数，则值为空数组[]，数组长度范围：0~256，仅errNo不为0时（表示控制失败），本字段可以不存在


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码，00000代表请求成功，其它值代表错误，可返回错误：公共错误码、W10001，W10007
retInfo|String|Body|必填|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256

**Parameter类型定义**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|必填|参数名，字符串长度范围：1~64
value|String|Body|必填|参数值，字符串长度范围：0~64


**示例**

**请求样例**

```
请求地址：https://uws.haier.net/cloudgw/v1/dev/opack/operation
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
	{
    "deviceId" : "***",
    "sn" : "***",
    "errNo" : ***,
    "parameters" : [
        { "name" : "***", "value" : "***" },
        { "name" : "***", "value" : "***" },
        ……
    ]
	}

```
**请求应答**
```
{
    "retCode" : "00000",
    "retInfo" : "***"
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
|W10009|新增设备主从关系时，设备角色在平台中不存在|
|W10010|新增设备主从关系时，保存设备关系失败|
|W10011|新增设备主从关系时，代绑定失败或回滚解绑从设备失败|
|W10012|新增设备主从关系时，主设备绑定的用户数量大于1，无法选择代绑定的从设备目标用户；删除设备主从关系时，从设备绑定的用户数量大于1，无法选择需要删除代绑定的从设备目标用户|
|W10013|新增设备主从关系时，主设备未与任何用户绑定，无法代绑定从设备；删除设备主从关系时，从设备未与任何用户绑定，无法代解绑从设备|
|W10014|新增设备主从关系时，到达分享上限|
|W10015|更新设备主从关系时，分享失败或回滚失败|
|W10016|新增设备主从关系时，超过用户允许的主设备允许存在的最大从设备数量|
|W10017|删除设备主从关系时，删除设备关系失败|
|W10018|删除设备主从关系时，主从关系不存在|
|W10019|新增设备主从关系时，设备角色不允许发起主动绑定|
|W10020|设备注册（基于模块）时，保存设备模块信息失败|
|W10021|开启设备绑定时间窗（基于模块）时，部分开启失败|
|W10022|开启设备绑定时间窗（基于模块）时，未查到任何关联的设备|
|W10023|删除主设备下所有主从关系时，中台服务无应答|
|W10024|删除主设备下所有主从关系时，当前用户存在绑定关系，无法删除主从信息|
|W10025|删除主设备下所有主从关系时，中台请求主数据失败|

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
[solution]:../Device-dev/_media/_device-direct/solution.png
[bind1]:../Device-dev/_media/_device-direct/bind1.png
[bind2]:../Device-dev/_media/_device-direct/bind2.png
[flow]:../Device-dev/_media/_device-direct/flow.png
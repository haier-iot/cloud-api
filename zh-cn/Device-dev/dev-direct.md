# 运设备类


## 简介




## 解决方案




## 接入流程



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



### 设备报警状态上报



### 新增设备主从关系

### 删除设备主从关系

### 删除主设备下所有主从关系




## 设备控制相关接口


### 设备控制请求--属性读



### 设备控制应答--属性读



### 设备控制请求--属性写



### 设备控制应答--属性写


### 设备控制请求--操作


### 设备控制应答--操作



# 设备分享

## 设备管理员查询设备管理员分享给个人的所有设备

**使用说明**

> 查询设备管理员分享给个人的所有设备

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/shareDevices?pageNumber={curpage}&pageSize={pageSize}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
pageNumber|String|url|否	|当前访问信息的起始页，从1开始
pageSize|String|url|否|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareDevs|ShareDevice[]|Body|是|共享设备信息
totalCount|String|Body|是|总数
pageSize|String|Body|是|当前返回页实际数量，不超过规定的最大数据
pageNumber|String|Body|是|当前页，从1开始


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json

```
**请求应答**
```java
{
	"shareDevs":[
		{
		"devInfo":{
			"deviceName": "测试",
			"deviceId": "100823",
			"wifiType": "0000000000000000000000000",
			"deviceType": "00000",
			"online":false
		},
		
		"devName":"测试1",
		"devShareUser":{
			"userId":  "10811563273",
			"userNickName": "xiaoyi",
			"userHeadImg": "https://uhome.haier.com/resource/headimg",
			"userAge": "10811563273",
			"userAddr": "10811563273",
			“userSex”: “male”
		}
		},
		"permission":{
			"authType":"share",
			"auth":{"view":true,"set":false,"control":false}
		}
		},
		{
		"devInfo":{
			"deviceName": "测试",
			"deviceId": "100823",
			"wifiType": "0000000000000000000000000",
			"deviceType": "00000",
			"online":false
		},
		"devName":"测试2",
		"devShareUser":{
			"userId": "10811563273",
			"userNickName": "xiaoyi",
			"userHeadImg": "https://uhome.haier.com/resource/headimg",
			"userAge": "10811563273",
			"userAddr": "10811563273",
			“userSex”: “male”
		}
		},
		"permission":{
			"authType":"share",
			"auth":{"view":true,"set":false,"control":false}
		}
		}
	],
	"totalCount": "20",
	"pageSize": "3",
	"pageNumber": "1",
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008

</div>


## 普通用户查询分享给我的个人分享设备

**使用说明**

> 查询分享给我的所有个人分享设备

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/shareDevices2me?pageNumber={curpage}&pageSize={pageSize}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
pageNumber|String|url|否	|当前访问信息的起始页，从1开始
pageSize|String|url|否|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareDevs|ShareDevice[]|Body|是|共享设备信息
totalCount|String|Body|是|总数
pageSize|String|Body|是|当前返回页实际数量，不超过规定的最大数据
pageNumber|String|Body|是|当前页，从1开始


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json

```
**请求应答**
```java
{
	"shareDevs":[
		{
		"devInfo":{
			"deviceName": "测试",
			"deviceId": "100823",
			"wifiType": "0000000000000000000000000",
			"deviceType": "00000",
			"online":false
		},
		
		"devName":"测试1",
		"devShareUser":{
			"userId":  "10811563273",
			"userNickName": "xiaoyi",
			"userHeadImg": "https://uhome.haier.com/resource/headimg",
			"userAge": "10811563273",
			"userAddr": "10811563273",
			“userSex”: “male”
		}
		},
		"permission":{
			"authType":"share",
			"auth":{"view":true,"set":false,"control":false}
		}
		},
		{
		"devInfo":{
			"deviceName": "测试",
			"deviceId": "100823",
			"wifiType": "0000000000000000000000000",
			"deviceType": "00000",
			"online":false
		},
		"devName":"测试2",
		"devShareUser":{
			"userId": "10811563273",
			"userNickName": "xiaoyi",
			"userHeadImg": "https://uhome.haier.com/resource/headimg",
			"userAge": "10811563273",
			"userAddr": "10811563273",
			“userSex”: “male”
		}
		},
		"permission":{
			"authType":"share",
			"auth":{"view":true,"set":false,"control":false}
		}
		}
	],
	"totalCount": "20",
	"pageSize": "3",
	"pageNumber": "1",
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008


## 设备管理员查询自有设备列表

**使用说明**

> 查询用户自有设备列表

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/devices`</br>
**HTTP Method：** GET

**输入参数**


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
devices|DeviceBriefInfo []|Body|是|


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json

```
**请求应答**
```java
{
	"devices ":[{
		"deviceName": "测试",
		"deviceId": "100823",
		"wifiType": "0000000000000000000000000",
		"deviceType": "00000",
		"online":false
		}, 
		{
		"deviceName": "测试",
		"deviceId": "100823",
		"wifiType": "0000000000000000000000000",
		"deviceType": "00000",
		"online":false
		}], 
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008


## 设备管理员分享设备给个人

**使用说明**

> 设备管理员分享设备给个人，发送分享个人设备消息给目标用户，记录消息发送结果到日志，支持targetId为临时的userid

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/{targetId}/shareDevice`</br>
**HTTP Method：** POST

**输入参数**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
shareDev|ShareDevice|Body|是|设备的分享信息 
targetId|String|url|是|分享设备的目标用户


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	    "shareDev": {
	        "devInfo": {
	            "deviceId": "100823"
	        }, 
	        "devName": "test", 
	        "permission": {
	            "authType": "share", 
	            "auth": {
	                "view": true, 
	                "set": false, 
	                "control": false
	            }
	        }
	    }
	}

```
**请求应答**
```java
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，F31225


## 设备管理员取消设备分享

**使用说明**

> 设备管理员取消用户分享，发送取消个人分享设备消息给目标用户，记录消息发送结果到日志, 支持targetId为临时的userid

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/ {targetId}/{devId}/shareDevice`</br>
**HTTP Method：** DELETE

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
devId|String|url|是|设备id 
targetId|String|url|是|设备分享者


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008


## 用户删除分享给自己的个人设备

**使用说明**

> 用户取消设备管理员分享给自己的设备

**接口描述**

?> **接入地址：** `/ufm/v1/protected/shareDeviceService/person/{devId}/shareDeviceToMe`</br>
**HTTP Method：** DELETE

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
devId|String|url|是|设备id 


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```java  
Header：
	appId: MB-****-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
	timestamp: 1491014447260 
	language: zh-cn
	timezone: +8
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	    "shareDev": {
	        "devInfo": {
	            "deviceId": "100823"
	        }, 
	        "devName": "test", 
	        "permission": {
	            "authType": "share", 
	            "auth": {
	                "view": true, 
	                "set": false, 
	                "control": false
	            }
	        }
	    }
	}

```
**请求应答**
```java
{
	"retCode":"00000",
	"retInfo":"成功"
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008
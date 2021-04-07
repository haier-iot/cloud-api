#  设备音视频服务

## 开通视频云存服务接口

**使用说明**

> 提供智家APP调用，开通视频设备的腾讯云存服务。

**接口描述**

?> **接入地址：** `/rcs/avm/tencent/device/create-storage`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|header|是|Api版本号(默认v1)|
deviceId|String|Body|是|海尔定义的设备唯一标识|
orderSn|String|Body|是|海尔商城订单号|


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
payload| CreateStorageResultDto|Body|是|返回数据|


**示例**

**请求样例**

```
Request Headers:
	Connection: keep-alive
	appId: ************
	appVersion: 01.00.00.00000
	clientId: 111111111111
	accessToken: ************
	sign: ************
	timestamp: 1585577435707
	Content-Encoding: utf-8
	Content-type: application/json
Post：
	{
	  "deviceId": "1221211121",
	  "orderSn":"ssdw23233"
	}

```
**请求应答**
```
{
	"payload": {
	    "status": 1,
	    "startTime": 1602664844,
	    "endTime": 1607935244
		},
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}

```


## 查看视频云存订单及状态接口

**使用说明**

> 提供智家APP调用，查看视频云存订单及状态信息

**接口描述**

?> **接入地址：** `/rcs/avm/tencent/device/find-storage-state`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|header|是|Api版本号(默认v1)
deviceId|String|body|是|海尔定义的设备唯一标识


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
payload|StorageStateResultDto|Body|是|返回数据|

**StorageStateResultDto**

|   字段名      |    类型       |说明|
| ------------- |:----------:|:---------:|
|  activatedStorage   | Integer| 当前生效的视频云存规格(0,7,30)  |
|  remainderDays   | Long| 当前生效的视频云存剩余天数  |
|  expireTime   | Long| 当前生效的视频云存失效时间 UTC时间 单位秒  |


**示例**

**请求样例**

```
POST data:
	[no cookies]
Request Headers:
	Connection: keep-alive
	appId: ****************
	appVersion: ****************
	clientId: ****************
	accessToken: ****************
	sign: ****************
	timestamp: 1585577435707
	Content-Encoding: utf-8
	Content-type: application/json
Post：
	{
		"deviceId": "1221211121"
	}

```
**请求应答**
```
{
	"payload": {
	    "activatedStorage": 30,
	    "remainderDays": 20,
	    "expireTime": 1607935244
		},
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}

```

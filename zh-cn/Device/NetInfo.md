# 设备网络信息

## 查询设备信号强度

**使用说明**

> 获取设备的信号强度

**接口描述**

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceNetQuality`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceNetQuality|DeviceNetQualityDto|Body|必填|设备ID

**示例**

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/deviceNetQuality
Header：
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```

**请求应答**

```
{
    "deviceNetQualityDto":
    {
        "hardwareType":"R",
        "hardwareVers":"1.0.00",
        "softwareType":"UDISCOVERY_UWT",
        "softwareVers":"2.3.12",
        "netType":"Wifi",
        "strength":"100"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

**接口错误码**
> A00001、B00001、G20202、A00004、D00006


## 查询设备网络质量

**使用说明**

根据DeviceId和token查询设备网络质量;


**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/device/net/quality`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备身份标识
accessToken|String|Header|是|用户token信息
apiVersion|String|Header|是|接口版本，此版本为v1.

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|DevNetQuality|Body|是|返回数据

**示例**

**请求样例**
```
POST URL:  https://uws.haier.net/dcs/device-service-2c/get/device/net/quality

POST data:
	{
	    "deviceId":"DC330D861B7C"
	}
	[no cookies]

Request Headers:
	Connection: keep-alive
	appId: **********
	appVersion: v1
	clientId: test123456
	sequenceId: *******
	accessToken: **********
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	Content-Encoding: utf-8
	Content-type: application/json

```

**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":{
      "machineId":"DC330D861B7C",
	  "isOnLine":true,
	  "netType":"Wifi",
	  "statusLastChangeTime ":159343991566,
	  "moduleVersion ":"2.3.30/eSDK_WIFI_AC/3.0.00/R",
	  "ssid":"UHOME-TEST-WIFI",
	  "rssi":-38,
	  "pssi":72,
	  "signalLevel ":2,
	  "ilostRatio":20,
	  "its":8
		}
}


```
# 设备型号信息


## 获取设备型号信息

**使用说明**

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

**接口描述**  

?> **接入地址：** `/stdudse/v1/protected/getBaseInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id 长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息

**示例**

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/getBaseInfo
Body：
	{
	  “deviceId”: “DC330DC4B6EE”
	}

```

**请求应答**
```
{
  "deviceBaseInfo": {
	"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
  "retCode": "00000",
  "retInfo": "成功"
}

```

**错误码**  

> G10001

## 修改设备型号信息

**使用说明**

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

修改设备型号信息，产品编码补习已在海极网注册


**接口描述**  

?> **接入地址：** `/stdudse/v1/protected/modifyModelInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
codeType|Integer|Body|是|1、机器编码；2、产品型号编码
code|String|Body|是|具体编码
modifyType|String|Body|否|修改方式：1、规则修改；2、直接修改；无输入或输入为空时默认采用【直接修改】逻辑

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息
possibleList|List<ProductInfo>|Body|否|设备可能的其他型号

**示例**

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/modifyModelInfo
Body：
{
  "deviceId": "DC330DC4B6EE",
  "codeType": 1,
  "code": "AURGH4JIOJAVU"
}

```

**请求应答**
```
{
  "deviceBaseInfo": {
	"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
"possibleList": [
  ],
  "retCode": "00000",
  "retInfo": "成功"
}

```


**错误码**
> C00003


## 根据成品编码获取型号信息

**使用说明**

根据成品编码，获取完整的型号信息数据

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/netDevice/info`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|是|接口版本，此版本为v1.
language|String|Header|是|国际化标识，代表客户端使用的语言。具体标识代码见附录。默认请传 “zh-cn“，代表中文</br>（此条只是为了日后接入国际化标识做准备，当传入的code不支持时一律认为是中文，不传也默认中文）
productCodes|String[]|Body|是|成品编码

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|`List<ServiceDeviceInfoResultDto>`|Body|是|返回数据

**示例**

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/netDevice/info
POST data:
	{
	    "productCodes":["21389***1231"]
	}
[no cookies]
Request Headers:
	Connection:keep-alive
	appId: *************
	appVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: **************
	sign: 3**************
	timestamp: 1590053835901
	language: zh-cn
	timezone: Asia/Shanghai
	Content-Encoding: utf-8
	Content-type: application/json

```

**请求应答**
```

{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
        "deviceInfos":[
            {
                "productCode": "21389ade1231",   
                "modelCode": "xxx",
                "typeId": "xxx",
                "productImg1": "https://pic/test/1.png",
                "brandCode":"B01",
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003",
                "brandName":"海尔6666",
                "videoFlag":1
            }
        ],...
}

```




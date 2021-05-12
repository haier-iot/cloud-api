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


## 根据typeId、应用分类、大中类获取符合条件的成品编码列表（所有）

**使用说明**

根据typeId、应用分类、大中类获取型号信息，获取完整的型号信息数据


**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/netDevice/productCodes`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|是|接口版本，此版本为v1.
language|String|Header|是|国际化标识，代表客户端使用的语言。具体标识代码见附录。默认请传 “zh-cn“，代表中文</br>（此条只是为了日后接入国际化标识做准备，当传入的code不支持时一律认为是中文，不传也默认中文）
appTypeCode|String[]|Body|否|成品编码
midCode|String[]|Body|否|中类编码
bigCode|String[]|Body|否|大类编码
typeId|String[]|Body|否|typeId

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|String[]|Body|是|返回数据
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/netDevice/productCodes
POST data:
	{
	    "midCode":"02012"
	}
[no cookies]
Request Headers:
	Connection:keep-alive
	appId: ************
	appVersion: v1
	clientId: test123456
	sequenceId: ************
	accessToken: ************
	sign: ************
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
    "payload":[
        "AA9C3Y015",
        "AA9C3Y115",
        "yn",
        "TEST0335",
        "TEST9999",
        "TEST9998",
        "Ytest22",
        "Ytest11中文",
         ...
        "u3x9Haxii"
    ]
}
```


## 根据typeId、应用分类、大中类分页获取型号信息

**使用说明**

根据typeId、应用分类、大中类获取型号信息，获取完整的型号信息数据


**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/netDevice/infos/page`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|是|接口版本，此版本为v1.
language|String|Header|是|国际化标识，代表客户端使用的语言。具体标识代码见附录。默认请传 “zh-cn“，代表中文</br>（此条只是为了日后接入国际化标识做准备，当传入的code不支持时一律认为是中文，不传也默认中文）
pager|RequestPager|Body|是|
entity|ServiceDeviceInfosRequest|Body|是|


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|ServiceDeviceInfosResult|Body|是|返回数据
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**RequestPager**  

| **名称** | 分页信息 |&emsp;| RequestPager |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|pageIndex| long | 页码 |默认1，不能为0|  
|pageSize| long | 每页数量限制|默认50，不能为0|  


**ServiceDeviceInfosRequestDto**  

| **名称** | 分页请求主体 |&emsp;| ServiceDeviceInfosRequestDto |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|appTypeCode| String | Body |成品编码|  
|midCode| String | Body|中类编码|  
|bigCode| String | Body|大类编码| 
|typeId| String | Body|typeId| 


**ServiceDeviceInfoResult**  

| **名称** | 分页详情 |&emsp;| ServiceDeviceInfosResult |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|content| List<ServiceDeviceInfoResultDto> | 返回信息 | |  
|pageIndex| long | 每页数量限制 | |  
|pageSize| long |  | | 
|total| long |  | |  
|totalPages| long |  | | 
|numberOfElements| long |  | | 
|first| boolean |  | | 
|last| boolean |  | | 


**ServiceDeviceInfoResultDto**  

| **名称** | 设备详细信息 |&emsp;| ServiceDeviceInfoResultDto |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|productCode| String | 成品编码 | |  
|modelCode| String | 型号名称 | |  
|productImg1| String | 型号图片url | 本图片为图片1 | 
|typeId| String | typeId |  |  
|brandName| String | 品牌名称 | | 
|brandCode| String | 品牌编码 | | 
|videoFlag| Integer | 音视频标识 | | 
|screenFlag| String | 屏幕标识 | 暂无同步 | 
|bigCode| String | 大类 | | 
|midCode| String | 中类 | | 
|appTypeName| String | 应用类型名称 | | 
|appTypeCode| String | 应用类型编码 | | 


**示例**

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/netDevice/infos/page
POST

Body:
	{
	    "pager":{
	        "pageSize":10,
	        "pageIndex":1
	    },
	    "entity":{
	        "bigCode":"02"
	    }
	}
	[no cookies]

Request Headers:
	Connection: keep-alive
	appId: SV-*********-0000
	appVersion: v1
	clientId: te*********56
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign: 314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a*********
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**
```

{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":{
        "content":[
            {
                "productCode":"AA9C3Y015",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.haigeek.com:443/download/resource/selfService//hardware/modelimg/b844513bb41e4dc1998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandName":"海尔6666",
                "brandCode":"B01",
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"AA9C3Y115",
                "modelCode":"KFR-32GW/06NGA23A套机",
                "typeId":"00000000000000008080000000041410",
                "brandName":"海尔6666",
                "brandCode":"B01",
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"yn",
                "modelCode":"yn",
                "productImg1":"yn",
                "typeId":"yn",
                "brandName":"yn",
                "brandCode":"A001",
                "videoFlag":1,
                "screenFlag":"0",
                "bigCode":"02",
                "midCode":"02021",
                "appTypeName":"yn",
                "appTypeCode":"yn"
            },
            {
                "productCode":"TEST0335",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.haigeek.com:443/download/resource/selfService//hardware/modelimg/b844513bb41e4dc1998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"TEST9999",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.haigeek.com:443/download/resource/selfService//hardware/modelimg/b844513bb41e4dc1998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"TEST9998",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.haigeek.com:443/download/resource/selfService//hardware/modelimg/b844513bb41e4dc1998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"Ytest22",
                "modelCode":"套机",
                "productImg1":"https://resource.ha998cc35f2a6e2a0c.png",
                "typeId":"test-2020202020202020202020202020",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"Ytest11中文",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.ha998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"@#$%&*-_|/.",
                "modelCode":"5KFR-32GW/06NGA23A套机",
                "productImg1":"https://resource.ha998cc35f2a6e2a0c.png",
                "typeId":"500000000000000008080000000041410",
                "brandCode":"B01",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012",
                "appTypeName":"测试003",
                "appTypeCode":"Test003"
            },
            {
                "productCode":"Ytest1002",
                "modelCode":"5KFR-32GW/06NGA23A套机-update",
                "typeId":"TTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT",
                "videoFlag":2,
                "bigCode":"02",
                "midCode":"02012"
            }
        ],
        "pageIndex":1,
        "pageSize":10,
        "total":2592,
        "totalPages":260,
        "numberOfElements":10,
        "first":false,
        "last":false
    }
}

```
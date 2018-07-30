!> **当前版本：** [设备管理服务企业版v1.5.1][DevicesEnterprise_document_url]</br>
**日期：** 2018-07-19 

## 简介
设备管理企业版为Haier U+云平台提供的对智能互联设备的操作设备进行命令下发、读写等相关设备的操作能力，可以通过应用服务器直接对设备进行操作。
![设备管理企业版图片][DevicesStandard_type]

### 名词解释

-  **标准设备**
>  标准设备是指

-  **非标准设备**
-  **单命令**
-  **组命令**

### 功能介绍
**厂商授权设备控制**
经过厂商授权的第三方应用，可通过设备类型（typeid）来获取设备信息，并通过设备的MAC地址来对设备下发控制命令。

能力|能力简述|
:-:|:-
查询设备信息|通过设备MAC查询与设备绑定的用户信息；查询设备的最新状态（标准/非标模型）
设备属性操作|读取标准模型设备属性；写入标准模型设备属性
非标准设备命令下发|向非标准设备下发命令，包括单命令或组命令
操作标准模型设备|通过设备操作符操作标准设备模型，并可获取设备的应答回调

**用户授权设备控制**
经过用户授权的应用，可通过用户授权来对设备进行控制操作。

能力|能力描述
:-:|:-
非标准设备操作|用户对绑定的非标准化设备(设备6位码)进行管理与操作
标准设备属性读写|用户对已绑设备属性信息的读取与写入
标准设备操作|用户以下发命令的方式对设备进行操作

## 公共结构说明
**User：** 用户信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
loginId|String|用户名（邮箱）|
userId|String|用户Id|
userProflie|Map|用户拓展属性

**OpPropertyValue：** 属性操作

参数名|类型|说明|备注
:-|:-:|:-:|:-
name|String|属性|
value|String|值|

**OpResult：** 操作结果

参数名|类型|说明|备注
:-|:-:|:-:|:-
usn|String|操作序列号|
deviceId|String|操作设备ID
result|String|操作应答结果|是一个base64码，</br>标准模型设备解密后的结果为：</br>{"extData":{},"args":[]}，其中[]中的数据为多个由name,value组成的键值对；</br>非标准模型设备解密后的结果为:</br> {"extData":{},"statuses":[]}，其中[]中的数据为多个由name,value组成的键值对


## 接口清单
### 厂商授权设备控制

#### 查询设备绑定的用户

> 根据MAC查询绑定的用户信息

##### 1、接口定义
？> **接入地址：** `/udse/v1/devBindUsers`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备的id


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
users|Users[]|Body|是|用户列表

##### 2、请求样例
**请求样例**
```
请求地址：/udse/v1/devBindUsers
Header：
	appId: SV-GEHWHKQ-0000
	appVersion: 99.99.99.99990
	clientId: 123
	sequenceId: 20161020153428000015
	accessToken: TGT3RFHEN0534U172OOYRA0GKCHKI0
	sign: 139854d169436e6d91c7b11701b0e2a4bd9152c2005a1fab95dcd60639c3c17d
	timestamp: 1490253051551 
	language: zh-cn
	timezone: +8
	appKey: 961c447171c19efd78beaef9abc72e7d
	Content-Encoding: utf-8
	Content-type: application/json 
Body
{
	"deviceId":"0007A8947D05"
}
```
**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"users":
	[
		{
			"loginId": "mnxyxxxh2@163.com","userId": "838670064340172800"
		},
        {
			"loginId": "mnxyxxxh1@163.com","userId": "844794014807883776"
			"userProfile":{"country": "italy","nickName": "名字","avatar": "123"}   
		}
    ]
}
```
##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202



#### 非标准设备命令下发
> 厂商服务端下发非标准设备命令（单命令、组命令），不需要使用用户token

##### 1、接口定义
？> **接入地址：** `/udse/v1/devOp`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备id
sn|String|Body|必填|操作流水号
category|String|Body|必填|操作的分类</br>单命令："AttrOp"；组命令:"GroupOp"
name|String|Body|必填|操作的名称
operateCodes|String|Body|必填|操作命令Base64加密值
callbackUrl|String|Body|非必填|操作应答回调地址，只支持http协议

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求样例

**请求样例**
```
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH",
	"category": "AttrOp",
	"name": "221001",
	"operateCodes": "eyJ2YWx1ZSI6IjIyMTAwMSJ9",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```
**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJzdGF0dXNlcyI6IFsKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfSwKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfQogICAgXQp9"，
	"resCode":0
}
```
##### 3、接口错误码
> C00002、C00004、C00006、D00008、G20202、G03002



#### 查询设备最新状态
> 支持标准模型和非标准模型设备

##### 1、接口定义
?> **接入地址：** `/udse/v1/devOpStatus`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID

**输出参数:** 


参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
Statuses|Map<String,String>|Body|必填|设备状态信息，具体信息是由模块上传给云平台，云平台存储并对外提供。
timestamp|Long|Body|必填|操作时间戳

##### 2、请求样例
**请求样例**
```
请求地址：/udse/v1/devOpStatus
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119"
}
```
**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"statuses": 
	{
		"60200a": "302000","202008": "NULL",
		"202009": "202009","202006": "NULL",
		"202007": "202007","202004": "NULL",
		"20200m": "NULL","20200j": "20200j",
		"20200k": "NULL"
	},
	"timestamp": "1490250511728"
}

```

##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202



#### 读取标准模型设备属性
> 读取标准模型设备的属性

##### 1、接口定义
？> **接入地址：** `/udse/v1/stdDevPropertyRead`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
property|String|Body|必填|设备读属性的属性名
sn|String|Body|必填|设备读属性的请求序列号
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列

##### 2、请求样例

**请求样例**
```
请求地址：/udse/v1/stdDevPropertyRead
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```


**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答数据**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0="
}

```

##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202



#### 写入标准模型设备属性
> 写入标准模型设备属性

##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevPropertyWrite`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
property|String|Body|必填|设备写属性的属性名
value|String|Body|必填|设备写属性的属性值
sn|String|Body|必填|设备读属性的请求序列号
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列

##### 2、请求样例
**请求样例**

```
请求地址：/udse/v1/stdDevPropertyWrite
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"value": "value",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**

```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```

##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202



#### 操作标准模型设备
> 操作标注模型设备

##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备id
operationName|String|Body|必填|操作名称
operationValue|List<OpPropertyValue>|Body|必填|属性值的列表，由模型文档决定是否必填及如何填
sn|String|Body|必填|设备操作请求序列号
callbackUrl|String|body|非必填|操作应答回调地址,只支持http协议

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/stdDevOperate
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"operationName": "operationName",
	"operationValue": 
	[
    	{"name": "name1","value": "value1"},
    	{"name": "name2","value": "value2"}
	],
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**

```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```
##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202

### 用户授权设备控制

#### 用户设备操作-控制通道-非标准模型
> 用户设备操作-控制通道，支持非标准模型（6位码设备）设备的操作

##### 1、接口定义
?> **接入地址：** `/udse/v1/devicesOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
category|String|Body|必填|操作的分类</br>单命令："AttrOp";组命令："GroupOp"
name|String|Body|必填|操作名称
operateCodes|String|Body|必填|操作命令Base64加密值
callbackUrl|String|Body|必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/devicesOperate
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH",
	"category": "AttrOp",
	"name": "221001",
	"operateCodes": "eyJ2YWx1ZSI6IjIyMTAwMSJ9",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJzdGF0dXNlcyI6IFsKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfSwKICAgICAgICB7CiAgICAgICAgICAgICJuYW1lIjogIioqKiIsCiAgICAgICAgICAgICJ2YWx1ZSI6ICIqKioiCiAgICAgICAgfQogICAgXQp9"，
	"resCode":0
}
```

##### 2、接口错误码
> B00001、B00004、A00001、D00006、G20202、G03002




#### 用户读属性-异步接口-标准模型
> 用户读属性-异步接口，支持标准模型的属性读

##### 1、接口定义
?> **接入地址：** `/udse/v1/propertyRead`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
property|String|Body|必填|设备读属性的属性名
callbackUrl|String|Body|必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/propertyRead
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```

**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0="
}
```


##### 3、请求错误码
> B00001、B00004、A00001、D00006、G20202、G03002


#### 用户写属性-异步接口-标准模型
> 用户写属性-异步接口，支持标准模型的属性写

##### 1、接口定义
?> **接入地址：** `/udse/v1/propertyWrite`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
property|String|Body|必填|设备写属性的属性名
value|String|Body|必填|设备写属性的属性名
callbackUrl|String|Body|必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/propertyWrite
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"property": "propertyName",
	"value": "value",
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```
**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```
##### 3、接口错误码
> B00001、G20202、B00004、A00001、D00006、G03002



#### 用户设备操作-异步接口-标准模型
> 用户设备操作-异步接口，支持标准模型设备操作

##### 1、接口定义
?> **接入地址：** `/udse/v1/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|设备操作请求序列号
operationName|String|Body|必填|操作名称
operationValue|List<OpPropertyValue>|Body|必填|属性值的列表，由模型文档决定是否必填及如何填
callbackUrl|String|Body|必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/operate
Header：
	appId:MB-ABC-0000
	appVersion:2015110401
	clientId:356877020056553-08002700DC94
	sequenceId:08002700DC94-15110519074300001
	accessToken: TGTFUNXMDK4AQIN2I9SJ8M9MGV1D00
	sign:bd4495183b97e8133aeab2f1916fed41
	timestamp: 1436236880183
	language:zh-cn
	timezone:8
	Content-type: application/json
Body
{
	"deviceId": "0007A893C119",
	"operationName": "operationName",
	"operationValue": 
	[
	    {"name": "name1","value": "value1"},
	    {"name": "name2","value": "value2"}
	],
	"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
	"callbackUrl": "https://www.uhome.haier.net/callback.html"
}
```


**请求应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```
**操作应答**
```
{
	"retCode": "00000",
	"retInfo": "成功!",
	"usn": "600ce95da3e14fc7a68f483dd14db864",
	"deviceId": "0007A893C119",
	"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0=",
	"resCode":0
}
```

##### 3、接口错误码
> B00001、B00004、A00001、D00006、G20202、G03002

## 使用方式

### 开通流程
![开通流程][DevicesStandard_liucheng]

### 应用场景
设备管理服务（标准版）主要为开发的应用程序实现用户与设备绑定、解绑设备、获取用户设备列表等与智能互联设备相关的基础管理服务。

## 文档资料
[设备管理服务标准版v2.0.1][DevicesEnterprise_document_url]


[^-^]:文本连接注释
[DevicesEnterprise_document_url]:#

[^-^]:常用图片注释
[DevicesStandard_type]:_media/_DevicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_DevicesEnterprise/DevicesEnterprise_liucheng.png




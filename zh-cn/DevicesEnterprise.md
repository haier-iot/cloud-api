
> **当前版本：** [UWS 设备管理服务企业版 V1.5.2](zh-cn/ChangeLog/DevicesEnterprise)  
**更新时间**：{docsify-updated} 

## 企业版注意事项

### 权限申请

企业版服务授权，使用服务IP白名单策略，需要是用此服务请联系IOT平台技术支持开通系统IP白名单

### 公共头部分

Header 中appid 字段填写内容为系统ID，即systemid。 此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”

## 简介
设备管理企业版为Haier U+云平台提供的对智能互联设备的操作设备进行命令下发、读写等相关设备的操作能力，可以通过应用服务器直接对设备进行操作。
![设备管理企业版图片][DevicesStandard_type]

**用户Token授权设备控制**  

通过用户Token授权来对设备进行远程控制操作。

能力|能力描述
:-:|:-
非标准设备操作|用户对绑定的非标准化设备(设备6位码)进行管理与操作
标准设备属性读写|用户对已绑设备属性信息的读取与写入
标准设备操作|用户以下发命令的方式对设备进行操作

**高级授权设备控制(厂商授权设备管理服务)**  

厂商授权设备管理服务，是U+云平台专门为企业用户提供的管理、控制设备的服务，只需获取U+云平台颁发的授权码，无需实现token授权，快速实现设备控制管理功能。详细服务了解和授权开通联系[海尔优家商务BD][Business]。

<!--  注释开始

能力|能力描述
:-:|:-
查询设备相关信息|查询设备绑定的用户；查询设备最新状态（支持标准模型和非标准模型）
非标准设备操作|非标准设备命令（单命令、组命令）下发
标准设备属性读写|用户对已绑设备属性信息的读取与写入
标准设备操作|用户以下发命令的方式对设备进行操作

如有业务需要，可联系[海尔优家商务BD][Business]沟通商定更高级的授权设备控制方案。
注释结束
-->


## 规则与约束
1、设备管理企业服务是针对服务器端进行授权，需提供部署服务器的外网IP地址，在云平台配置IP白名单才可以访问。</br>
2、公共header请求头中的appId传值需是申请企业级应用systemId，签名算法使用对应的systemKey。</br>
3、deviceId 设备Id为发起请求的accessToken安全令牌 token所有拥有控制权限的设备。</br>




## 应用场景
设备管理服务基于用户拥有对应的设备权限，即设备管理员（绑定用户）或权限用户。

设备操作命令可通过业务服务上报到平台，平台将控制命令解析并下发到只能设备进行控制

![设备管理企业版场景流程][DeviceE_flow]







## 公共结构说明
### User
用户信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
loginId|String|用户名（邮箱）|
userId|String|用户Id|
userProflie|Map|用户拓展属性|

### OpPropertyValue
属性操作

参数名|类型|说明|备注
:-|:-:|:-:|:-
name|String|属性|
value|String|值|

### OpResult 
操作结果

参数名|类型|说明|备注
:-|:-:|:-:|:-
usn|String|操作序列号|
deviceId|String|操作设备ID
result|String|操作应答结果|是一个base64码，标准模型设备解密后的结果为：`{"extData":{},"args":[]}`，其中[]中的数据为多个由name,value组成的键值对；</br>非标准模型设备解密后的结果为:`{"extData":{},"statuses":[]}`，其中[]中的数据为多个由name,value组成的键值对


## 接口清单

### 用户Token授权设备控制
> API接口总览  

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| 用户设备操作-控制通道-非标准模型| 用户设备操作-控制通道，支持非标准模型（6位码设备）设备的操作| 是| 无| 
| 用户读属性-异步接口-标准模型  | 用户读属性-异步接口，支持标准模型的属性读| 是| 无| 
| 用户写属性-异步接口-标准模型 | 用户写属性-异步接口，支持标准模型的属性写| 是| 无| 
|用户设备操作-异步接口-标准模型 | 用户设备操作-异步接口，支持标准模型设备操作| 是| 无| 

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
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
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
	appId:SV-****-0000
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

##### 3、接口错误码
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
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
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
	appId:SV-****-0000
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
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
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
	appId:SV-****-0000
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
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
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
	appId:SV-****-0000
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


### 高级授权设备控制(厂商授权设备管理服务)

> API接口总览   
 
具体接口服务了解和授权开通请联系[海尔优家商务BD][Business]。

<!--   注释开始

> API接口总览  

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
|查询设备绑定的用户| 根据设备MAC查询绑定的用户信息| 是| 无| 
|非标准设备命令下发| 非标准设备命令（单命令、组命令）下发| 是| 无| 
|查询设备最新状态| 支持标准模型和非标准模型设备| 是| 无| 
|读取标准模型设备属性|读取标准模型设备属性| 是| 无| 
|写入标准模型设备属性|写入标准模型设备属性| 是| 无| 
|操作标准模型设备|操作标准模型设备| 是| 无| 

#### 查询设备绑定的用户
> 根据设备MAC查询绑定的用户信息

##### 1、接口定义
?> **接入地址：** `/udse/v1/devBindUsers`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
users|User[]|Body|必填|用户列表

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/devBindUsers
Header：
appId: SV-****-0000
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
"users": [
        {
"loginId": "mnxyxxxh2@163.com",
"userId": "838670064340172800"
        },
        {
"loginId": "mnxyxxxh1@163.com",
"userId": "844794014807883776"
"userProfile": {
"country": "italy",
"nickName": "名字",
"avatar": "123"
}   
     }
    ]
}

```


##### 3、接口错误码
> B00001、C00002、C00006、D00001、G20202


#### 非标准设备命令下发
> 非标准设备命令（单命令、组命令）下发

##### 1、接口定义
?> **接入地址：** `/udse/v1/devOp`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
sn|String|Body|必填|操作流水号，必须唯一
category|String|Body|必填|操作的分类。</br>单命令："AttrOp"</br>组命令："GroupOp"
name|String|Body|必填|操作的名称
operateCodes|String|Body|必填|操作命令Base64加密值  
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/devOp
Header：
appId:SV-****-0000
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


##### 3、请求错误码
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


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
statuses|Map<String,String>|Body|必填|设备状态信息，具体信息是由模块上传给云平台，云平台存储并对外提供。  
timestamp|long|Body|必填|操作时间戳


##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/devOpStatus
Header：
appId:SV-****-0000
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
"statuses": {
"60200a": "302000",
"202008": "NULL",
"202009": "202009",
"202006": "NULL",
"202007": "202007",
"202004": "NULL",
"20200m": "NULL",
"20200j": "20200j",
"20200k": "NULL"
  },
"timestamp": "1490250511728"
}


```

##### 3、请求错误码
> B00001、C00002、C00006、D00001、G20202 


#### 读取标准模型设备属性  

##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevPropertyRead`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID  
property|String|Body|必填|设备读属性的属性名  
sn|String|Body|必填|设备读属性请求序列号 
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号  

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/stdDevPropertyRead
Header：
appId:SV-****-0000
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
> B00001、C00002、C00006、D00001、G20202 



#### 写入标准模型设备属性  

##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevPropertyWrite`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID  
property|String|Body|必填|设备写属性的属性名 
value|String|Body|必填|设备写属性的属性值 
sn|String|Body|必填|设备写属性请求序列号 
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号  

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/stdDevPropertyWrite
Header：
appId:SV-****-0000
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

##### 3、请求错误码
> B00001、C00002、C00006、D00001、G20202 



#### 操作标准模型设备  

##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID  
operationName|String|Body|必填|操作名称 
operationValue|List<OpPropertyValue>|Body|必填|属性值的列表，由模型文档决定是否必填及如何填。  
sn|String|Body|必填|设备操作请求序列号   
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号  

##### 2、请求示例

**请求示例**
```
请求地址：/udse/v1/stdDevOperate
Header：
appId:SV-****-0000
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
"operationValue": [
    {
"name": "name1",
"value": "value1"
    },
    {
"name": "name2",
"value": "value2"
    }
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
##### 3、请求错误码
> B00001、C00002、C00006、D00001、G20202 
注释结束
-->

## 关于参数填写与返回值

### 非标准设备单命令参数

```
category: 'AttrOp',
name: '20g10o',
operateCodes:base64({"value":"30g106"})
```
### 非标准设备组命令重要参数
```
category: 'GroupOp',
name: '000001',
operateCodes:base64({"value":[{"name":"20g10o","value":"30g106"},{"name":"20g10m","value":"12"},{"name":"20g10d","value":"30g101"}]})
```
>ID文档中，name有可能写成0x0001，请传递参数时，使用000001

### sn与usn

sn由调用方生成，指请求的流水号。云平台应答该请求时，会返回usn，如果涉及到回调，回调中的usn也表示是对请求sn的回答。</br>
即，完整会话为：</br>
请求：</br>
sn，callback  ——>对应示例中的“用户请求” </br>
云平台响应请求：usn ——>对应示例中的“请求应答” </br>
调用方callback接收：usn ——>对应示例中的“操作应答数据” </br>
> sn与usn一一对应，请保证sn的唯一。

### retCode与resCode
retCode为云平台对请求的执行应答，见各接口的返回值及公共返回值</br>
resCode为设备对命令的执行情况的应答，在回调中出现，当为0时，表示设备成功执行，其他值均为错误，具体错误信息需查看错误码列表或查询ID文档或咨询设备开发工程师。

### 控制resCode错误码列表

resCode值|说明|备注
:-:|:-|:-:
0|成功|
1|通用错误码，表示暂时未定义或无法区分的错误|
2|内部错误，表示设备内部发生且不对外开发的错误|
11|命令执行超时|
12|无效命令，主主要指命令执行逻辑错误。例如关机状态下再关机等|
13|非法值，主要指设备操作的操作值非法。例如温度设置的温度值越界|
14|无效token，即用户token在云平台不存在|
15|用户与设备无绑定关系|
16|设备不在线|
17|不支持的命令，主要指读属性或写属性请求时，设备无此属性，或者操作时，设备不支持次操作|
107|无效命令，下发的命令设备底板不支持|E++类设备特有，即E++协议的无效命令帧
200|云平台内部错误，表示云平台内部发生且不对外开放的错误|云平台直接返回，且设备发出

>注意：以上错误码，除200以外，其他错误码均为设备直接返回，而具体返回该错误码的处理逻辑请直接咨询设备方。







[^-^]:常用图片注释
[DevicesStandard_type]:_media/_DevicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_DevicesEnterprise/DevicesEnterprise_liucheng.png
[DeviceE_flow]:_media/_DevicesEnterprise/DeviceE_flow.png
[Business]:/zh-cn/Business



> **当前版本：：** [UWS 设备管理服务企业版-欧洲环境 V1.5.2](en-us/ChangeLog/DevicesStandard)  
**更新时间：** {docsify-updated} 


## 简介  

设备管理企业版为Haier U+云平台提供的对智能互联设备的操作设备进行命令下发、读写等相关设备的操作能力，可以通过应用服务器直接对设备进行操作。  

能力|能力描述
:-:|:-
非标准设备操作|用户对绑定的非标准化设备(设备6位码)进行管理与操作
标准设备属性读写|用户对已绑设备属性信息的读取与写入
标准设备操作|用户以下发命令的方式对设备进行操作  



**高级授权设备控制(厂商授权设备管理服务)**  

厂商授权设备管理服务，是U+云平台专门为企业用户提供的管理、控制设备的服务，只需获取U+云平台颁发的授权码，无需实现token授权，快速实现设备控制管理功能。详细服务了解和授权开通联系[海尔优家商务BD][Business]。



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

|   错误码      |     描述      | 情景  |  
| ------------- |:----------:|:-----:|   
| B00001   |  缺少必填参数| 参数填写不全  |   
| B00004   | 参数不符合规则要求 | 参数格式错误   |   
| A00001   |  服务不可用 | 服务不可用  |   
| D00006   |会话失效 | 用户会话状态不存在 |    
| G03002  | 设备不在线 | 设备不在线无法下发命令 |      
| G20202  | 当前用户与该设备不匹配 |  当前用户与该设备不匹配    |  
 



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


##### 3、接口错误码

|   错误码      |     描述      | 情景  |  
| ------------- |:----------:|:-----:|   
| B00001   | 缺少必填参数| 参数填写不全   |   
| B00004   | 参数不符合规则要求 |参数格式错误 |   
| A00001   | 服务不可用 | 服务不可用  |   
| D00006   | 会话失效|  用户会话状态不存在 |    
| G03002  | 设备不在线 | 设备不在线无法下发命令|      
| G20202  |当前用户与该设备不匹配 | 当前用户与该设备不匹配   |  
 



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

|   错误码      |     描述      | 情景  |  
| ------------- |:----------:|:-----:|   
| B00001   |  缺少必填参数|appId为空  |   
| B00004   | 参数不符合规则要求 | 回调地址格式不正确   |   
| A00001   | 服务不可用 | 服务不可用  |   
| D00006   |会话失效 | 用户会话状态不存在 |    
| G03002  | 设备不在线| 设备不在线，不能发送命令 |      
| G20202  | 当前用户与该设备不匹配|  当前用户与该设备不匹配 |  
 


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


|   错误码      |     描述      | 情景  |  
| ------------- |:----------:|:-----:|   
| B00001   |  缺少必填参数|appId为空  |   
| B00004   | 参数不符合规则要求 | 回调地址格式不正确   |   
| A00001   | 服务不可用 | 服务不可用  |   
| D00006   |会话失效 | 用户会话状态不存在 |    
| G03002  | 设备不在线| 设备不在线，不能发送命令 |      
| G20202  | 当前用户与该设备不匹配|  当前用户与该设备不匹配 |  
 
### 高级授权设备控制(厂商授权设备管理服务)   


> API接口总览

详细服务了解和授权开通请联系[海尔优家商务BD][Business]。


<!--  注释开始
|Query the device bound user|Query the bound user information based on the device MAC |yes | no |  
|Non-standard equipment orders issued|Non-standard equipment command (single command, group command) issued |yes | no |  
|Check the latest status of the device|Support for standard and non-standard model devices |yes | no |  
|Read the standard model device attributes|Read the standard model device attributes |yes | no |  
|Write standard model device attributes|Write standard model device attributes |yes | no |  
|Operate standard model equipment|Operate standard model equipment |yes | no |  

####  Query the device bound user  
> Query the bound user information based on the device MAC   

##### 1、Interface definition    

?> **Access address：** `/udse/v1/devBindUsers`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|users|User[]|Body|yes|List of users|

##### 2、Request sample
**User request**
```
Header：
appId: SV-GEHWHKQ-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 20161020153428000015
accessToken: 
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
**Request response**
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

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| B00001   |   Lack of required parameters | AppId is empty   |    
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00001  |  Sign error | Digital signature error  |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   | 
 

####  Non-standard equipment orders issued    
> Non-standard equipment command (single command, group command) issued    

##### 1、Interface definition    

?> **Access address：** `/udse/v1/devOp`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  
|sn|String|Body|yes|Operate the serial number. Must be the only|  
|category|String|Body|yes|Classification of operations.</br>Single order: "AttrOp"</br>Group order: GroupOp"|  
|name|String|Body|yes|Operation name|  
|operateCodes|String|Body|yes|Operation command Base64 encryption value|   
|callbackUrl|String|Body|no|Operation reply callback address, support only HTTP protocol|   

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|usn|String|Body|yes|Operation sequence number|

##### 2、Request sample
**User request**
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
**Request response**
```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```  
**Operational response**  

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

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00004   |  Insufficient operation permission |Size format error   | 
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00008  |  Illegal user | AccessToken error  |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
| G03002  | Equipment offline | The device cannot issue commands without being online  |    


####  Check the latest status of the device  
> Support for standard and non-standard model devices  

##### 1、Interface definition    

?> **Access address：** `/udse/v1/devOpStatus`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  


**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|statuses|Map<String,String>|Body|yes|Device status information, the specific information is transferred from the module to the cloud platform, which is stored and provided by the cloud platform.|  
|timestamp|long|Body|yes|Operation time stamp|


##### 2、Request sample
**User request**
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
"deviceId": "0007A893C119"
}

```
**Request response**
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

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00001  | Sign error | Digital signature error |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  



####  Read the standard model device attributes    
> Read the standard model device attributes       

##### 1、Interface definition    

?> **Access address：** `/udse/v1/stdDevPropertyRead`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  
|property|String|Body|yes|The device reads the property name of the property|  
|sn|String|Body|yes|Operate the serial number. Must be the only|  
|callbackUrl|String|Body|no|Operation reply callback address, support only HTTP protocol|   

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|usn|String|Body|yes|Operation sequence number|

##### 2、Request sample
**User request**
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
"property": "propertyName",
"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
"callbackUrl": "https://www.uhome.haier.net/callback.html"
}


```
**Request response**
```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```  
**Operational response**  

```  
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864",
"deviceId": "0007A893C119",
"result": "ewogICAgImV4dERhdGEiOiB7fSwKICAgICJhcmdzIjogWwogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9LAogICAgICAgIHsKICAgICAgICAgICAgIm5hbWUiOiAiKioqIiwKICAgICAgICAgICAgInZhbHVlIjogIioqKiIKICAgICAgICB9CiAgICBdCn0="
}


```

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00001  | Sign error | Digital signature error |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  


####  Write standard model device attributes      
> Write standard model device attributes       

##### 1、Interface definition    

?> **Access address：** `/udse/v1/stdDevPropertyWrite`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  
|property|String|Body|yes|The device writes the property name of the property|  
|value|String|Body|yes|The device writes the attribute value of the attribute|    
|sn|String|Body|yes|Operate the serial number. Must be the only|  
|callbackUrl|String|Body|no|Operation reply callback address, support only HTTP protocol|   

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|usn|String|Body|yes|Operation sequence number|

##### 2、Request sample
**User request**
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
"property": "propertyName",
"value": "value",
"sn": "FJIJ2L3-FSFRFGRTWT-HYRH"",
"callbackUrl": "https://www.uhome.haier.net/callback.html"
}


```  

**Request response**
```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}
```

**Operational response**  

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

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00001  | Sign error | Digital signature error |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  



####  Operate standard model equipment      
> Operate standard model equipment      

##### 1、Interface definition    

?> **Access address：** `/udse/v1/stdDevOperate`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|  
|operationName|String|Body|yes|The operation name|  
|operationValue|List<OpPropertyValue>|Body|yes|A list of property values, which are required by the model document and how.|    
|sn|String|Body|yes|Operate the serial number. Must be the only|  
|callbackUrl|String|Body|no|Operation reply callback address, support only HTTP protocol|   

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|usn|String|Body|yes|Operation sequence number|

##### 2、Request sample
**User request**
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

**Request response**
```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```

**Operational response**  

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

##### 3、Interface error code  

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| C00002   |  Appserver has no access authorization | &emsp;   |   
| C00006   | Product configuration information is empty |  &emsp;   |    
| D00001  | Sign error | Digital signature error |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
注释结束
-->



[^-^]:文本连接注释
[DevicesEnterprise_document_url]:_document/_devicesEnterprise/EquipmentManagementServiceEnterpriseEditionV1.5.2.docx  

[^-^]:常用图片注释
[^-^]:[DevicesStandard_type]:_media/_devicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_devicesEnterprise/DevicesEnterprise_liucheng.png
[Business]:/en-us/Business
[DeviceE_flow]:_media/_devicesEnterprise/DeviceE_flow.png




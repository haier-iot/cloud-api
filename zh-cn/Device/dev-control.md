# 设备控制 


## 设备操作高级动能

### 批量命令操作（经过逻辑运算）

**使用说明**

> 领域模型接口使用Https协议，使用`https://uws.haier.net+接口地址`进行访问

统一接收标准模型的命令，命令经过逻辑运算、转换、补偿后下发到设备

**接口描述**  

?> **接入地址：** `/stdudse/v1/sendbatchCmd/{sn}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|上下文|必填|用户token  
sn|String|url|必填|批量命令总sn
cmdMsgList|List<CmdItem>|Body|必填|批量命令（详情见公共结构）
backUrl|String|Body|非必填|操作应答回调地址，只支持http协议


**输出参数**

标准输出参数

**公共结构**  


| **名称** | 单个指令对象 |&emsp;| CmdItem |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|sn| String | 批量指令的总sn |用户必填|  
|index| int | 在批量指令中的顺序号，从0开始，步长为1，不能重复、不能缺号；下发指令时将，按这个序号，逐个指令下发。（编号错误，将导致指令下发错误）|用户必填|  
|delaySeconds| int | 执行这个指令前需要延时（等待）的秒数 |<font color="red">该参数已失效</font>|    
|subSn| string | 本条指令的sn，发送到领域模型。组成为：总sn+“：”+指令序号 |由批量指令顺序执行控制器填写，在下发指令前填写完成|  
|deviceId| string | 设备id|用户必填|  
|name| string | 单个属性设置时，属性名 |用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一| 
|value| string | 单个属性设置时，属性值|用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|
|cmdName| string | 组命令时，操作名|用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|
|cmdArgs| Map<String,String> | 组命令时，属性集合|用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一| 


**示例**

**请求样例**


```
Header：
	Connection: keep-alive
	appId: MB-*****-0000
	appVersion: 10.01.11.00025
	clientId: lipeizhen
	sequenceId: 20150812102234777777
	accessToken: TGT36YE3RVND80GY1ZGCIHB6LR2MW
	sign:ab9cdf9c9e7e857677fce6fe2ff8624f162791653c8b21e2c1f4e2ce3aa3b4d8
	timestamp: 1604653214611
	language: cn
	timezone: +8
	Content-Type: application/json;charset=UTF-8
	appKey: f50c76fbc8271d361e1f6b5973f54585
	Content-Length: 207
	Host: 192.168.140.16:6320
	User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

Body:
{
	"sn": " lpz4130527016",
	"backUrl": "https://uws.haier.net/scheduler/v2/device/callback",
	"cmdMsgList": [{
		"delaySeconds": 0,
		"deviceId": "0007A8B7014E",
		"index": 0,
		"name": "onOffLight",
		"sn": " lpz4130527016",
		"subSn": " lpz4130527016:0",
		"value": "true"
	}]
}

```

**请求应答**

```
{
	"retCode": "00000",
	"retInfo": "成功!"
}

```

**操作应答(最终返回给调用方的应答)**

```
{
	"cmdSn": "lpz4130527016",
	"batchResult": false,
	"batchResultInfo": "",
	"detailInfo": [{
		"execResultInfo": "设备执行命令错误",
		"execStep": 2,
		"execResult": false,
		"invalidCode": "1"
        "productCodeT": "ABCDEFG123"
		"cmdSn": "lpz4130527016",
		"execResultCode": "S00004",
		"oid": 3959165,
		"cmdSubSn": "lpz4130527016:_:000",
		"deviceId": "0007A8B7014E",
		"resultTime": 1588070070000
	}]
}

```  

备注：detailInfo参数定义请查看（查询批量命令执行结果明细接口）
注意：在给调用方的应答异常【设备执行命令错误】和控制异常【未知异常】中会携带invalidCode，但并不是一定会携带invalidCode
比如：设备应答12和13时也会返回【设备执行命令错误】，但不携带invalidCode,控制下发时若遇到别的异常也会返回【未知异常】，但不携带invalidCode

**错误码**

> G03002、B00001、G20202、B00004、A00001、000001、A00006、A00005、A00007、A00008、A00009


### 查询批量命令执行结果明细

**使用说明**

> 查询批量命令执行结果明细


**接口描述**  

?> **接入地址：** `/stdudse/v1/queryResult/{sn}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|上下文|必填|用户token  
sn|String|url|必填|批量命令总sn


**输出参数**  

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
detailInfo|List<BatchResultDto>|body|必填|查询到的结果  


**公共结构**  


| **名称** | 批量命令执行结果明细 |&emsp;| BatchResultDto |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|oid| long | 数据代理主键 |只读|  
|cmdSn| string |批量指令的总sn|只读|  
|cmdSubSn| string | 本条指令的sn，发送到领域模型。组成为：总sn+“：”+指令序号 |只读|    
|deviceId| string | 设备id|只读|  
|execStep| int | 本条指令的执行步骤 |只读| 
|execResult| boolean |本条指令本步骤的执行结果；true为成功、false为失败：如超时、异常等|只读|
|execResultCode| string | 本条指令本步骤的执行结果说明，错误码|只读|
|execResultInfo| string | 本条指令本步骤的执行结果说明，错误信息|只读| 
|invalidCode| string | 本条指令设备应答或控制下发的无效命令编码（十进制整数）|只读| 
|productCodeT| string | 产品型号编码|只读| 
|resultTime| long | 本条指令本步骤的反馈时间|只读| 


**示例**

**请求样例**


```
Headers:
	Connection: keep-alive
	appId: MB-*****-0000
	appVersion: XX.XX.XX.XXXXX
	clientId: 1234
	sequenceId: sdfsadf
	sign:d5f5f22caf9809990b5854fe4077834fa34ebf1274bda0ebdbd15821b0a9bee4
	timestamp: 1604653710256
	language: zh-cn
	timezone: +8
	appKey: f50c76fbc8271d361e1f6b5973f54585
	Content-Encoding: utf-8
	Content-type: application/json
	accessToken: TGT3S0MI00DBFHGD1ZWV12Z5JEQ680
	Host: 192.168.140.16:6320
	User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```

**请求应答**

```
{
	"batchResult": true,
	"cmdSn": "181206135609768fa163e26700a3593",
	"detailInfo": [{
		"cmdSn": "181206135609768fa163e26700a3593",
		"cmdSubSn": "181206135609768fa163e26700a3593:_:000",
		"deviceId": "DC330D861BB6",
		"execResult": true,
		"execResultCode": "00000",
		"execResultInfo": "成功",
		"execStep": 2,
		"oid": 14839,
		"resultTime": 1544075772000
	}],
	"retCode": "00000",
	"retInfo": "成功"
}


```

**错误码**

|**执行步骤说明（execStep）**|**错误码**|**描述**|
|:----------:|:-----:|:--------:|    
|execStep=1下发失败；execStep=2设备有应答，应答失败，不是0、1、2、11、12、13、107| S00001 | 未知异常 |
|设备未响应| S00002 |设备未响应|
|设备有应答，应答是11| S00003 | 设备执行超时 |
|设备有应答，应答是12、13| S00004 | 设备执行命令错误|
|设备有应答，应答是17| S00005 | 设备执行不支持该命令 |
|设备有应答，应答是1、2| S00006 |设备未知错误|
|命令队列已满，无法执行| S00007 | 当前设备繁忙请稍后再试|
|下发失败，设备不在线| S03002 | 设备离线|
|execStep=1下发成功/控制请求与当前状态一致；execStep=2设备有应答，应答是0| A00000 | 成功|   



<!--   注释掉了toC领域模型单控接口  推荐使用toC领域模型批控接口
#### 设备指令执行操作接口（经过逻辑运算）

> 领域模型接口使用Https协议，使用`https://uws.haier.net+接口地址`进行访问

统一接收标准模型的命令，命令经过逻辑运算、转换、补偿后下发到设备


##### 1、接口定义
?> **接入地址：** `/stdudse/v1/modfier/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|上下文|必填|用户token
deviceId|String|Body|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
cmdName|String|Body|否|组命令id（1、若该操作为组命令操作，则该值必填。2、若该操作为单命令操作，则该值不需要传递）
cmdArgs|Map<String,String>|Body|必填|一组命令,即属性集合（key-value）。（若该操作为单命令操作，则该值必须只有一对key-value。）
callbackUrl|String|Body|非必填|操作应答回调地址，只支持http协议


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|非必填|序列号sn

##### 2、请求示例

**请求样例**


```
Header：
appId:MB-*****-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
sign:bd4495183b97e8133aeab2f1916fed41
timestamp: 1436236880183
accessToken:TGT37FAT5QBI2UNO2TFWT4AASDKAF0
language:zh-cn
timezone:8
Content-type: application/json

Body
{
"deviceId": "********",
"cmdName": "grSetDAC",
"cmdArgs": {"pmvStatus":"true","cleaningTimeStatus":"false","cloudFilterChangeFlag":"false","electricHeatingStatus":"true","onOffStatus":"true","operationMode":"4"},
"callbackUrl": "http://www.uhome.haier.net/callback.html"
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
  "deviceId": "0007A893C119",
  "resCode": "0",
  "result": "eyJhcmdzIjpbeyJuYW1lIjoidGFyZ2V0VGVtcGVyYXR1cmUiLCJ2YWx1ZSI6IjE2LjAwIn0seyJuYW1lIjoid2luZERpcmVjdGlvblZlcnRpY2FsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoib3BlcmF0aW9uTW9kZSIsInZhbHVlIjoiNCJ9LHsibmFtZSI6InNwZWNpYWxNb2RlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoid2luZFNwZWVkIiwidmFsdWUiOiIxIn0seyJuYW1lIjoidGVtcFVuaXQiLCJ2YWx1ZSI6IjEifSx7Im5hbWUiOiJwbXZTdGF0dXMiLCJ2YWx1ZSI6InRydWUifSx7Im5hbWUiOiJpbnRlbGxpZ2VuY2VTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoiaGFsZkRlZ3JlZVNldHRpbmdTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoic2NyZWVuRGlzcGxheVN0YXR1cyIsInZhbHVlIjoidHJ1ZSJ9LHsibmFtZSI6IjEwZGVncmVlSGVhdGluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJlY2hvU3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6ImxvY2tTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoic2lsZW50U2xlZXBTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoibXV0ZVN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJyYXBpZE1vZGUiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoiZWxlY3RyaWNIZWF0aW5nU3RhdHVzIiwidmFsdWUiOiJ0cnVlIn0seyJuYW1lIjoiaGVhbHRoTW9kZSIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJvbk9mZlN0YXR1cyIsInZhbHVlIjoidHJ1ZSJ9LHsibmFtZSI6InRhcmdldEh1bWlkaXR5IiwidmFsdWUiOiIzMCJ9LHsibmFtZSI6Imh1bWFuU2Vuc2luZ1N0YXR1cyIsInZhbHVlIjoiMCJ9LHsibmFtZSI6IndpbmREaXJlY3Rpb25Ib3Jpem9udGFsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoibG9jYWxGaWx0ZXJDaGFuZ2VGbGFnIiwidmFsdWUiOiJ0cnVlIn0seyJuYW1lIjoiZW5lcmd5U2F2aW5nU3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6ImxpZ2h0U3RhdHVzIiwidmFsdWUiOiJmYWxzZSJ9LHsibmFtZSI6InNlbGZDbGVhbmluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJjaDJvQ2xlYW5pbmdTdGF0dXMiLCJ2YWx1ZSI6ImZhbHNlIn0seyJuYW1lIjoicG0ycDVDbGVhbmluZ1N0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJodW1pZGlmaWNhdGlvblN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJmcmVzaEFpclN0YXR1cyIsInZhbHVlIjoiZmFsc2UifSx7Im5hbWUiOiJpbmRvb3JUZW1wZXJhdHVyZSIsInZhbHVlIjoiMTguNTAifSx7Im5hbWUiOiJpbmRvb3JIdW1pZGl0eSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6Im91dGRvb3JUZW1wZXJhdHVyZSIsInZhbHVlIjoiLTY0LjAwIn0seyJuYW1lIjoiYWNUeXBlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoic2Vuc2luZ1Jlc3VsdCIsInZhbHVlIjoiMCJ9LHsibmFtZSI6ImFpclF1YWxpdHkiLCJ2YWx1ZSI6IjAifSx7Im5hbWUiOiJwbTJwNUxldmVsIiwidmFsdWUiOiIwIn0seyJuYW1lIjoiZXJyQ29kZSIsInZhbHVlIjoiNiJ9LHsibmFtZSI6IkVyckFja0ZsYWciLCJ2YWx1ZSI6InRydWUifSx7Im5hbWUiOiJvcFNyYyIsInZhbHVlIjoiMyJ9LHsibmFtZSI6InRvdGFsQ2xlYW5pbmdUaW1lIiwidmFsdWUiOiIyNjY0In0seyJuYW1lIjoiaW5kb29yUE0ycDVWYWx1ZSIsInZhbHVlIjoiNTMifSx7Im5hbWUiOiJvdXRkb29yUE0ycDVWYWx1ZSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6ImNoMm9WYWx1ZSIsInZhbHVlIjoiMCJ9LHsibmFtZSI6InZvY1ZhbHVlIiwidmFsdWUiOiIwIn0seyJuYW1lIjoiY28yVmFsdWUiLCJ2YWx1ZSI6IjAifV19",
  "retCode": "00000",
  "retInfo": "成功",
  "usn": "600ce95da3e14fc7a68f483dd14db864"
}

```

##### 3、错误码

> G03002、B00001、G20202、B00004、A00001、000001、A00006、A00005、A00007、A00008、A00009

-->

## 设备控制类接口

设备控制类接口通过APPSERVER访问：  
若APPSERVER部署在外网，则通过https://uws.haier.net/udse访问；  
若APPSERVER部署在内网，则通过https://internal.uws.haier.net/udse访问；


**权限申请**

设备控制类接口服务授权，使用服务IP白名单策略，需要是用此服务请联系IOT平台技术支持开通系统IP白名单

**公共头部分**

Header 中appid 字段填写内容为系统ID，即systemid。此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”



### 用户设备操作-（非标准模型的单命令/组命令操作）

**使用说明**

> 用户设备操作-控制通道，支持非标准模型（6位码设备）设备的操作

**接口描述**  

?> **接入地址：** `/udse/v1/devicesOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
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

**示例**

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
Body:
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

**接口错误码**  

> B00001、B00004、A00001、D00006、G20202、G03002



### 用户写属性-异步接口-(标准模型-单命令写操作）

**使用说明**

> 用户写属性-异步接口，支持标准模型的属性写

**接口描述**  

?> **接入地址：** `/udse/v1/propertyWrite`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
sn|String|Body|必填|操作流水号，必须唯一
property|String|Body|必填|设备写属性的属性名
value|String|Body|必填|设备写属性的属性名
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号

**示例**

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
Body:
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
**接口错误码**  

> B00001、G20202、B00004、A00001、D00006、G03002



### 用户设备操作-异步接口-（标准模型-组命令操作）

**使用说明**

> 用户设备操作-异步接口，支持标准模型设备操作

**接口描述**  

?> **接入地址：** `/udse/v1/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
sn|String|Body|必填|设备操作请求序列号
operationName|String|Body|必填|操作名称
operationValue|List<OpPropertyValue>|Body|必填|属性值的列表，由模型文档决定是否必填及如何填
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议
accessToken|String|Header|必填|用户token

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|必填|操作序列号


**示例**

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
Body:
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
**接口错误码**  

> B00001、B00004、A00001、D00006、G20202、G03002


### 重要说明<sup style="color:red">(注意)<sup>

**非标准设备单命令参数**

```
category: 'AttrOp',
name: '20g10o',
operateCodes:base64({"value":"30g106"})
```
**非标准设备组命令重要参数**  

```
category: 'GroupOp',
name: '000001',
operateCodes:base64({"value":[{"name":"20g10o","value":"30g106"},{"name":"20g10m","value":"12"},{"name":"20g10d","value":"30g101"}]})
```
>ID文档中，name有可能写成0x0001，请传递参数时，使用000001

**sn与usn**

sn由调用方生成，指请求的流水号。云平台应答该请求时，会返回usn，如果涉及到回调，回调中的usn也表示是对请求sn的回答。</br>
即，完整会话为：</br>
请求：</br>
sn，callback  ——>对应示例中的“用户请求” </br>
云平台响应请求：usn ——>对应示例中的“请求应答” </br>
调用方callback接收：usn ——>对应示例中的“操作应答数据” </br>
> sn与usn一一对应，请保证sn的唯一。

**retCode与resCode**  

retCode为云平台对请求的执行应答，见各接口的返回值及公共返回值</br>
resCode为设备对命令的执行情况的应答，在回调中出现，当为0时，表示设备成功执行，其他值均为错误，具体错误信息需查看错误码列表或查询ID文档或咨询设备开发工程师。

**控制resCode错误码列表**

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

**业务通道下行args说明**  

业务通道（下行）中args：
Base64加密值：e++协议中，8D下行透传数据内容，源数据Base64编码后的字符串

例如要发给底板0x010203040506， 
cSrc = "010203040506"; 
byte[] b = hexStringToBytes(cSrc); 
args = byteToBase64(b); 
args就是base64编码后的内容

用户设备操作-控制通道-非标准模型中operateCode：
json对象base64之后的值，例：
String dataString={"data": args}// args就是base64编码后的内容
operateCodes=Base64Base64.encodeBase64String(dataString.getBytes())

十六进制转byte数组的代码：
```
public static byte[] hexStringToBytes(String hexString) {
  if (hexString == null || hexString.equals("")) {
      return null;
  }
  hexString = hexString.toUpperCase();
  int length = hexString.length() / 2;
  char[] hexChars = hexString.toCharArray();
  byte[] d = new byte[length];
  for (int i = 0; i < length; i++) {
      int pos = i * 2;
      d[i] = (byte) (charToByte(hexChars[pos]) << 4
              | charToByte(hexChars[pos + 1]));
  }
  return d;
}
```
byte数组base64的代码：
```
public static String byteToBase64(byte[] buffer) {
  if (buffer == null || buffer.length == 0) {
      return null;
  }

  String enStr = DatatypeConverter.printBase64Binary(buffer)
          .replaceAll("\n", "").replaceAll("\r", "");

  return enStr;
}
```





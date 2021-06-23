#  设备权限

## 服务器端配网信息查询（2B）

**使用说明**

> 根据型号信息查询设备的型号、图标、配置引导信息 

**接口描述**

?> **接入地址：** `https://internal.uws.haier.net/dcs/device-service-2b/find/device/binding/netinfo`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|必填|应用ID，40位以内字符,Haier U+ 云平台全局唯一，或者是云平台ID 
appVersion|String|Header|必填|应用版本最多32 位字符,应用版本标识
apiVersion|String|Header|非必填|接口版本，格式为：x.y.z，如”2.0.0”，不填时，按最新版本处理
sequenceId|String|Header|必填|报文流水(客户端唯一)客户端交易流水号，6-32 位，由客户端自行定义，自行生成，建议使用日期+顺序编号的方式
sign|String|Header|必填|对请求进行签名运算产生的签名，签名算法见签名机制
timestamp|long|Header|必填|Unix时间戳，精确到毫秒
language|String|Header|必填|国际化标识，代表客户端使用的语言
timezone|String|Header|必填|代表客户端使用的时区，uws暂不支持国际化，填写8即可
Content-Type|String|Header|必填|必须为application/json;charset=UTF-8
productCode|String|body|必填|产品编码


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256
payload|DeviceInfomation|Body|否|返回数据


**示例**

**请求样例**

```
POST data:
Request Headers:
	Connection: keep-alive	
	appId: MB-UZHSH-0000	
	appVersion: 01.00.00.00000	
	clientId: 111111111111	
	accessToken: TGT3EM9DNXY65GYY20VNX6077ISHG0	
	sign: 37b1b9e18c9b12481e6ebcdec9c4e5f3b8da80fbc52930ae13efcdb57f159e
	timestamp: 1585577435707
	Content-Encoding: utf-8	
	Content-type: application/json
Post：
	{
	  "mac": "string",
	  "selfDiscoveryInformationCode": "string",
	  "productCode":"String"
	}

```
**请求应答**
```
{
  "payload": {
    "apptypeCode": "string",
    "apptypeName": "string",
    "bindFailed": "string",
    "bindFailedImg": "string",
    "bindSuccess": "string",
    "bindSuccessImg": "string",
    "brandCode": "string",
    "configMode": "string",
    "deviceRole": "string",
    "hotspotName": "string",
    "modelCode": "string",
    "productCode": "string",
    "productDesc": "string",
    "productImg1": "string",
    "productImg2": "string",
    "customHotspotCode ": "string",
	"customHotspotName": "string",
	"state": "string",
    "step1": "string",
    "step1Img": "string",
    "step2": "string",
    "step2Img": "string",
    "step3": "string",
    "step3Img": "string",
    "step4": "string",
    "step4Img": "string",
    "step5": "string",
    "step5Img": "string",
    "typeId": "string"
  },
  "retCode": "string",
  "retInfo": "string"
}

```

**接口错误码说明**

| **错误码** | **描述** |
| :-------: |:-----:|
|A00001|服务不可用|
|A00002|网络异常|
|A00003|访问或操作超时|
|A00004|系统内部错误|
|A00005|数据库访问异常|
|A00006|未知异常|
|B00001|缺少必填参数|
|B00002|参数类型错误|
|B00003|参数数值超出值域或不是枚举值|
|B00004|参数不符合规则要求|
|B00006|参数长度错误|
|B00007|参数与接口定义不匹配|
|C00001|appId与appKey验证失败|
|C00002|appServer无访问授权|
|C00003|访问权限不足|
|C00004|操作权限不足|
|C00005|重复请求|
|C00006|未知设备类型|
|C00007|appId配置信息为空|
|C00008|appKey为空|
|D00001|sign签名错误|
|D00002|账号或密码错误|
|1100001|设备中心网器信息接口内部错误|
|1100002|参数不可为空|
|1100005|通过code查不到，并且mac信息为空|
|1100006|产品型号为空|
|1100008|自发现热点信息不存在|
|1100007|调用外部服务异常|
|1100009|自发现热点信息和mac都为null|
|00001|主数据查询型号信息异常|
|00002|设备查询无型号信息|



## 绑定设备

**使用说明**

> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=300个，绑定时设备必须平台上线。）</font>

**接口描述**

?> **接入地址：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID 
name|String|body|必填|设备名称
data|String|body|必填|绑定加密数据


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：/uds/v1/protected/bindDevice
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: ************
    sign: ************
    timestamp: 1491013448628 
    language: zh-cn
    timezone: Asia/Shanghai
    appKey: ************
    Content-Encoding: utf-8
    Content-type: application/json
Body
{
    "deviceId": "************",
    "name": "test002",
    "data": "5f10bf4f5af08db934c8165c32140227"
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}

```

**接口错误码**
> A00001、B00001、G20202、A00004、B00001、D00006、G20904、G20908、G20910、G20914

</div>


## 解绑设备

**使用说明**

> 用户解绑设备，同时解除设备的相关分享

**接口描述**

?> **接入地址：** `/uds/v1/protected/{deviceId}/unbindDevice`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID 


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：/uds/v1/protected/{deviceId}/unbindDevice
Header：
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: ************
    sign: ************
    timestamp: 1491014596343 
    language: zh-cn
    timezone: Asia/Shanghai
    appKey: ************
    Content-Encoding: utf-8
    Content-type: application/json

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}

```

**接口错误码**
> A00001、B00001、G20202、A00004、B00001、D00006、G20915、G20916

</div>



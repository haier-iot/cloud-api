
>  **当前版本**：[UWS 应用版本更新 V1.0.0]()  
 **更新时间**：{docsify-updated} 

### 简介

> 此接口可用于应用版本的更新。首先开发者需要提交App相关信息并提供后续维护(可联系海尔优家IOT运维支持)，信息包括:版本号、版本名称、描述信息、资源编号(资源存储的url)、app状态、是否强制更新等信息。其中app状态通常默认为1，资源编号可以是资源存储的url或者带有下载二维码标识的图片，开发者视情况自定义即可。  


应用版本管理服务,信息维护与版本更新

版本信息维护：应用版本的维护通过后台流程审核维护到云平台，新版app经运维审核通过后在平台委会最新应用版本

版本更新：使用app版本查询接口检测APP版本状态，更具返回检测结果（详见接口说明），根据业务需求进行维护，推荐更新或强制更新

![app版本管理流程][version_flow]


<!-- 注释开始
## 接口清单  

> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| app版本查询    | 获取应用版本信息 | 是| 无| 
| 上传资源文件    | 上传资源文件到服务器| 是| 无|   

注释结束 -->


### 接口服务  

#### app版本查询 
> 获取应用版本信息，内容包括安卓、IOS的下载地址。


##### 1、接口定义
?> **接入地 址：**  `/uas/v1/appVersion/getLatest`  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  appId    | String | Header| 必填|应用的appId|

**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| version |  String  |   Body  |  必填  | 版本号（格式2015110501） |
| versionName |  String  |   Body  |  必填  |版本名称，可返回为空字符串 |
| description |  String  |   Body  |  必填  | 描述，可返回为空字符串） |
| resId |  String  |   Body  |  必填  | 资源编号或资源存储的url，可返回为空字符串 |
| status |  Integer  |   Body  |  必填  | app状态 |
| force |  String  |   Body  |  必填  | 是否强制|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 01.00.00.00000
clientId: 8F5BD72F-EAC5-4A5F-9093-77328C81E1AE
sequenceId: 123456
accessToken: TGT2HZDYSL2E17ML1YQHYJLBRZS
sign:b8aebde2498d0e5274d729c2494dd53267e44e53ae0dd7d3e
timestamp: 1547194823847 
language: zh-cn
timezone: +8
Content-Encoding: utf-8
Content-type: application/json


```  

**请求应答**

```java
{
   "data":
   {
      "description":"App新版本升级内容简述",
      "force":false,
      "resId":"资源地址url",
      "status":1,
      "version":"2018010901",
      "versionName":"3.1.1"
   },
   "retCode":"00000",
   "retInfo":"成功"
}

```

##### 3、错误码  
> B00001、C00002、C00006、C00007、D00001  




<!-- 注释开始
#### 上传资源文件 
> 上传资源文件到服务器。（注意：使用该接口需先联系能力管理员，对该APPId进行上传授权，以及配置上传资源的文件格式、大小，否则返回文件配置不存在错误）。


##### 1、接口定义
?> **接入地 址：**  `/uas/v1/resource/uploadFile`  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  file    | multipart/form-data | Body| 必填|上传的文件|  
|  description    | String| Body| 必填|文件描述，255个字符以内|  
|  ownerType    | Integer | Body| 必填|拥有者类型：0：用户，1：设备,9:其它|


**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| resourceInfo |  ResourceInfo  |   Body  |  必填  | 上传的资源信息 |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId:MB-ABC-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
accessToken: TGT1OY0RUUAH5D242SB68E9WX0W930
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
language:zh-cn
timezone:8
Content-type:multipart/form-data

Body
{
"description":"测试文件上传新接口",
"ownerType":0
}


```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"，
"resourceInfo":{
		"id":30121,
"createTime":"2016-09-22 16:09:14",
"description":"测试文件上传新接口",
"fileType":"png",
"ownerType":0,
"fileName":"table.png",
"systemId":"SV-UZHSH-0000",
"url":"/uam/v1/resource/enabling/uzhsh/100013957366155388.jpg",
"creator":"100013957366155388"
}
}

```

##### 3、错误码  
> C00002、C00004、C00006、C00007、D00008  

注释结束 -->



[version_flow]:_media/_versionmanage/version_flow.png


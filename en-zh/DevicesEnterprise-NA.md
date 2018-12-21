
> **current version：** [UWS EquipmentManagementServiceEnterpriseEdition V1.5.2](en-us/ChangeLog/DevicesStandard)  
**Update time：** {docsify-updated} 

## Introduction
The enterprise version of the service can be applied to both the application-side and the enterprise-level device management from the case where the application server (ie, the app server) is directly connected to the platform. The application server uniformly obtains device information and device control capabilities.  

**User Token authorized device control**  
The device is remotely controlled by the user token authorization.

ability|Capability brief|
:-:|:-
Non-standard device operation|User manages and operates the bound non-standardized device (device 6-bit code)
Standard device attribute read and write|User reads and writes the attribute information of the tied device   
Standard equipment operation|The user operates the device by issuing the following command  


<!-- 注释开始
**Advanced authorization device control(Manufacturer authorized device management services)**   

The device management service authorized by the manufacturer is the management and control device service specially provided by U+ cloud platform for enterprise users. It only needs to obtain the authorization code issued by U+ cloud platform, and it does not need to realize token authorization to quickly realize the device control and management function. Detailed service understanding and authorization to connect with  [Haier U+ Business BD][Business].
注释结束  -->


### Application scenario
The enterprise version service is applicable to the case where the application server (ie, the app server) is directly connected to the platform, and the enterprise-level device management is performed. The application server uniformly obtains device information and device control.


## Public structure description
### User
User Info

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
loginId|String|user name (mailbox)|
userId|String|User ID|
userProflie|Map|User extended attribute|

### OpPropertyValue
Attribute operation

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
name|String|Attributes|
value|String|value|

### OpResult 
Operation result

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
usn|String|Operation serial number|
deviceId|String|Operating device ID
result|String|Operation response result|Is a base64 code, </br> the result of decrypting the standard model device is:</br>`{"extData":{},"args":[]}`, where the data in [] is multiple by name, The key-value pair consisting of value;</br> The result of decrypting the non-standard model device is:</br> `{"extData":{},"statuses":[]}`, where the data in [] is multiple a key-value pair consisting of name, value  


## Interface list
### User Token authorized device control    


> API interface overview  


| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| User Equipment Operation - Control Channel - Non-Standard Model   |User equipment operation - control channel, support for operation of non-standard model (6-bit code device) devices| yes|  no |  
|User Read Attribute - Asynchronous Interface - Standard Model | User read attribute-asynchronous interface, support attribute reading of standard model| yes|  no |  
| User write attribute - asynchronous interface - standard model | User write attribute - asynchronous interface, support attribute writing of standard model| yes|  no |  
| User Equipment Operation - Asynchronous Interface - Standard Model | User device operation - asynchronous interface, supports standard model device operation| yes|  no |  

####  User Equipment Operation - Control Channel - Non-Standard Model 
> ser equipment operation - control channel, support for operation of non-standard model (6-bit code device) devices  

##### 1、Interface definition    

?> **Access address：** `/udse/v1/devicesOperate`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device ID|
|sn|String|Body|yes|Operation serial number, must be unique|
|category|String|Body|yes|Classification of operations</br> single command: "AttrOp"; group command: "GroupOp"|
|name|String|Body|yes|Operation name|
|operateCodes|String|Body|yes|Operation command Base64 encrypted value|
|callbackUrl|String|Body|yes|Operation response callback address, only supports http protocol|
|accessToken|String|Header|yes|User token|   


**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|usn|String|Body|yes|Operation serial number|

##### 2、Request sample
**User request**
```
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
> B00001、B00004、A00001、D00006、G20202、G03002



#### User Read Attribute - Asynchronous Interface - Standard Model
> User read attribute-asynchronous interface, support attribute reading of standard model   


##### 1、Interface definition    

?> **Access address：** `/udse/v1/propertyRead`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device id|
|sn|String|Body|yes|Operation serial number. Must be unique|
|property|String|Body|yes|The attribute name of the device read attribute|   
|callbackUrl|String|Body|yes|Operation response callback address, only supports http protocol|
|accessToken|String|Header|yes|User token |  


**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|usn|String|Body|yes|Operation serial number|  


##### 2、Request sample

**User request**
```
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
> B00001、B00004、A00001、D00006、G20202、G03002 



#### User write attribute - asynchronous interface - standard model  
> User write attribute - asynchronous interface, support attribute writing of standard model   

##### 1、Interface definition  

?> **Access address：** `/udse/v1/propertyWrite`</br>
**HTTP Method：** POST

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|   
|deviceId|String|Body|yes|Device id|
|sn|String|Body|yes|Operation serial number. Must be unique|
|property|String|Body|yes|The attribute name of the device write attribute|  
|value|String|Body|yes|The attribute name of the device write attribute|   
|callbackUrl|String|Body|yes|Operation response callback address, only supports http protocol|
|accessToken|String|Header|yes|User token |  

**Output parameters** 


|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|usn|String|Body|yes|Operation serial number|  

##### 2、Request sample
**User request**
```
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
> B00001、G20202、B00004、A00001、D00006、G03002  


#### User Equipment Operation - Asynchronous Interface - Standard Model  
> User device operation - asynchronous interface, supports standard model device operation  

##### 1、Interface definition
?> **Access address：** `/udse/v1/operate`</br>
**HTTP Method：** POST

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|   
|deviceId|String|Body|yes|Device id|
|sn|String|Body|yes|Operation serial number. Must be unique|
|operationName|String|Body|yes|Operation name|  
|operationValue|List<OpPropertyValue>|Body|yes|A list of attribute values, determined by the model documentation, and required|  
|callbackUrl|String|Body|yes|Operation response callback address, only supports http protocol|
|accessToken|String|Header|yes|User token | 

**Output parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|usn|String|Body|yes|Operation serial number|  



##### 2、Request example

**User request**
```
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
> B00001、B00004、A00001、D00006、G20202、G03002




[^-^]:文本连接注释
[DevicesEnterprise_document_url]:_document/_devicesEnterprise/EquipmentManagementServiceEnterpriseEditionV1.5.2.docx  

[^-^]:常用图片注释
[^-^]:[DevicesStandard_type]:_media/_devicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_devicesEnterprise/DevicesEnterprise_liucheng.png
[Business]:/en-us/Business




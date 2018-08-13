!> **current version：** [EquipmentManagementServiceEnterpriseEditionv1.5.1][DevicesEnterprise_document_url]</br>
**date：** 2018-07-19 

## Introduction
The device management enterprise version provides the operation capability of the related devices such as command delivery, reading and writing, etc. for the operation device of the intelligent interconnection device provided by the Haier U+ cloud platform, and can directly operate the device through the application server.  


### Noun explanation


-  **Non-standard equipment**
-  **Single command**
-  **Group command**

### Function introduced
**Vendor authorized device control**  

A device-authorized third-party application can obtain device information by using the device type (typeid) and send control commands to the device through the MAC address of the device.  

ability|Capability brief|
:-:|:-
Query device information|Query the user information bound to the device through the device MAC; query the latest status of the device (standard/non-standard model)  
Non-standard equipment orders issued|Send commands to non-standard devices, including single commands or group commands  

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
result|String|Operation response result|Is a base64 code, </br> the result of decrypting the standard model device is:</br>{"extData":{},"args":[]}, where the data in [] is multiple by name, The key-value pair consisting of value;</br> The result of decrypting the non-standard model device is:</br> {"extData":{},"statuses":[]}, where the data in [] is multiple a key-value pair consisting of name, value  


## Interface list
### Vendor authorized device control


> API interface overview  


| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| Query the user bound to the device   |User information bound according to device MAC query| yes|  no |  
|Non-standard device commands are issued | Non-standard device commands (single command, group command) are issued| yes|  no |  
| Query the latest status of the device | Support for standard models and non-standard model devices| yes|  no |  

#### Query the user bound to the device
> User information bound according to device MAC query

##### 1、Interface definition  

？> **Access address：** `/udse/v1/devBindUsers`</br>
**HTTP Method：** PUT

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device id|  


**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|users|Users[]|Body|yes|user list|

##### 2、Request sample
**User request**
```
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
"nickName": "name",
"avatar": "123"
}   
     }
    ]
}


```
##### 3、Interface error code
> B00001、C00002、C00006、D00001、G20202



#### Non-standard device commands are issued  
> Non-standard device commands (single command, group command) are issued  


##### 1、Interface definition  

？> **Access address：** `/udse/v1/devOp`</br>
**HTTP Method：** PUT

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|Body|yes|Device id|
|sn|String|Body|yes|Operation serial number. Must be unique|
|category|String|Body|yes|Classification of operations</br> single command: "AttrOp"; group command: "GroupOp"|   
|name|String|Body|yes|The name of the operation|
|operateCodes|String|Body|yes|Operation command Base64 encrypted value|   


**Output parameters**  

Output standard response parameters  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
| | | | |&emsp;|


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
"operateCodes": "eyJ2YWx1ZSI6IjIyMTAwMSJ9"
}

```

**Request response**
```
{
	"retCode": "00000",
	"retInfo": "成功!"
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
> C00002、C00004、C00006、D00008、G20202、G03002  



#### Query the latest status of the device
> Support for standard models and non-standard model devices  

##### 1、Interface definition  

?> **Access address：**  `/udse/v1/devOpStatus`</br>
**HTTP Method：** PUT

**Input parameters**

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|   
|deviceId|String|Body|yes|Device ID|

**Output parameters** 


|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|Statuses|Map<String,String>|Body|yes|Device status information, which is uploaded by the module to the cloud platform, and the cloud platform is stored and provided externally.|
|timestamp|Long|Body|yes|Operation timestamp|  

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
> B00001、C00002、C00006、D00001、G20202  


## Way of use

### Opening process
![开通流程][DevicesStandard_liucheng]

### Application scenario
The enterprise version service is applicable to the case where the application server (ie, the app server) is directly connected to the platform, and the enterprise-level device management is performed. The application server uniformly obtains device information and device control.  

## Documentation
[EquipmentManagementServiceEnterpriseEdition][DevicesEnterprise_document_url]


[^-^]:文本连接注释
[DevicesEnterprise_document_url]:_document/_devicesEnterprise/EquipmentManagementServiceEnterpriseEditionV1.5.2.docx  

[^-^]:常用图片注释
[^-^]:[DevicesStandard_type]:_media/_devicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_devicesEnterprise/DevicesEnterprise_liucheng.png




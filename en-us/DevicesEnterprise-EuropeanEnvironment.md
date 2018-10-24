!> **current version：** EquipmentManagementServiceEnterpriseEditionv1.5.2  
**date：** {docsify-updated} 

## Introduction
The enterprise version of the service can be applied to both the application-side and the enterprise-level device management from the case where the application server (ie, the app server) is directly connected to the platform. The application server uniformly obtains device information and device control capabilities.  

## Noun explanation


-  **Non-standard equipment**
-  **Single command**
-  **Group command**

## Function introduced  

**User Token authorized device control**  
The device is remotely controlled by the user token authorization.

ability|Capability brief|
:-:|:-
Non-standard device operation|User manages and operates the bound non-standardized device (device 6-bit code)
Standard device attribute read and write|User reads and writes the attribute information of the tied device   
Standard equipment operation|The user operates the device by issuing the following command  


**More advanced authorized device control**  
If you have business needs, you can contact [Haier U+ Business BD][Business] to communicate and agree on a more advanced authorized equipment control solution.
  

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
|Query the device bound user|Query the bound user information based on the device MAC |yes | no |  
|Non-standard equipment orders issued|Non-standard equipment command (single command, group command) issued |yes | no |  
|Check the latest status of the device|Support for standard and non-standard model devices |yes | no |  
|Read the standard model device attributes|Read the standard model device attributes |yes | no |  
|Write standard model device attributes|Write standard model device attributes |yes | no |  
|Operate standard model equipment|Operate standard model equipment |yes | no |  
| User Equipment Operation - Control Channel - Non-Standard Model   |User equipment operation - control channel, support for operation of non-standard model (6-bit code device) devices| yes|  no |  
|User Read Attribute - Asynchronous Interface - Standard Model | User read attribute-asynchronous interface, support attribute reading of standard model| yes|  no |  
| User write attribute - asynchronous interface - standard model | User write attribute - asynchronous interface, support attribute writing of standard model| yes|  no |  
| User Equipment Operation - Asynchronous Interface - Standard Model | User device operation - asynchronous interface, supports standard model device operation| yes|  no |  


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

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| B00004   | The parameters do not conform to the rules | Parameter format error   |   
| A00001   |  Service unavailable | Service unavailable  |   
| D00006   | Session invalidation |  User session state does not exist  |    
| G03002  | Equipment offline | The device cannot issue commands without being online |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
 



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

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| B00004   | The parameters do not conform to the rules | Parameter format error   |   
| A00001   |  Service unavailable | Service unavailable  |   
| D00006   | Session invalidation |  User session state does not exist  |    
| G03002  | Equipment offline | The device cannot issue commands without being online |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
 



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

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| B00004   | The parameters do not conform to the rules | Parameter format error   |   
| A00001   |  Service unavailable | Service unavailable  |   
| D00006   | Session invalidation |  User session state does not exist  |    
| G03002  | Equipment offline | The device cannot issue commands without being online |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
 


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

|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| B00001   |  Lack of required parameters| AppId is empty   |   
| B00004   | The parameters do not conform to the rules | Parameter format error   |   
| A00001   |  Service unavailable | Service unavailable  |   
| D00006   | Session invalidation |  User session state does not exist  |    
| G03002  | Equipment offline | The device cannot issue commands without being online |      
| G20202  | The current user does not match the device |  The current system does not match the device</br>The current system does not have permission to operate the device   |  
 

## Way of use

### Opening process
![开通流程][DevicesStandard_liucheng]

### Application scenario
The enterprise version service is applicable to the case where the application server (ie, the app server) is directly connected to the platform, and the enterprise-level device management is performed. The application server uniformly obtains device information and device control.  



[^-^]:文本连接注释
[DevicesEnterprise_document_url]:_document/_devicesEnterprise/EquipmentManagementServiceEnterpriseEditionV1.5.2.docx  

[^-^]:常用图片注释
[^-^]:[DevicesStandard_type]:_media/_devicesEnterprise/DevicesEnterprise_type.png
[DevicesStandard_liucheng]:_media/_devicesEnterprise/DevicesEnterprise_liucheng.png
[Business]:/en-us/Business




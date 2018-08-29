!> **current version：** [DeviceManagementServiceStandardEditionV2.0.1][DevicesStandard_document_url]</br>
**date：** 2018-07-19 

## Introduction  
The Device Management Service Standard Edition provides developers with a basic capability service for the management of intelligent terminal device information. By integrating this service module, the developer can provide the basic maintenance capability of binding the user to the device, presenting the device status, location, and other attributes of the device, and realize rapid development of the basic capability integration related to the smart device.  
![设备管理标准版图片][DevicesStandard_type]

### Noun explanation

-  **noun**

### Function Introduction

**Binding and unbinding**</br>
1、Bind device: The user associates with the device to form a binding relationship. The binding user becomes the device administrator.  
2、Unbind the device: The user unbinds the device and the related sharing of the device is also released.</br>
3、My device list: Used to query all smart connected devices that are bound to a single user.</br>

**Query and obtain device information**</br>
1、Information inquiry: including the ability to query whether the device is online, query the brand information of the device, and query the location of the room where the device is located.</br>
2、Obtain device attribute information: including obtaining device alias, acquiring device location new information, device detail information, device signal strength, and other related information for application presentation or other service operations based on device information.</br>

**Device information modification**</br>
1、The device management service standard version can be used to modify device-related attributes and information, including updating location information, adding device brand information, and updating device aliases.</br>

## Public structure description  

### DeviceInfo  
Bind device information  

Field name|Types|Description|Remarks
:-|:-:|:-:|:-
deviceName|String|Device name, equivalent to an alias|
deviceId|String|Device ID|
wifiType|String|Device wifitype| typeId
deviceType|String|Equipment type|8-digit code consisting of large, medium and small categories  
totalPermission|AuthInfo|Integration of permissions and permissions information|
permissions|String|Permission information|
online|String|Whether online|  
deviceVersion|DeviceVersion|Device version information|    

### AuthInfo
Permission content, at least one of which is ture

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
view|Boolean|Is there a right to view?|
set|Boolean|Is there a configuration permission?|
control|Boolean|Is there control?|

### Permission
Permission information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
auth|AuthInfo|Permission content|
authType|String|Permission type|Home: family sharing</br>share: personal sharing</br>owener: device owner</br>server: permissions to appserver  

### Location
Location information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
longitude|Double|longitude|
latitude|Double|Dimension|
cityCode|String|City code|

### BaseProperty
Basic attribute  

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
brand|String|Equipment brand|
model|String|Equipment model|
others|Map<String,Strng>|Other attributes|

### Deviceversion
Device details  

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
deviceId|String|Device ID|
modules|Set<Module>|Module information|
wifiType|String|Wifi type|
deviceType|String|Equipment type| 8-bit code, typeId  
baseProperty|BaseProperty|Brand information|
location|Location|location information|

### Module
Module information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
moduleId|String|Module ID|
moduleType|String|Module type|
ModuleInfos|Map<String,String>|Module other information|Collection of attributes and values  


## Interface list

### Device class interface

> API interface overview


| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| Query user device list   | Get a list of devices that users bind themselves to and share with others| yes|  no |  
|Binding device   | Establish a binding relationship between the user and the device| yes|  no |  
| Unbind device  | Unbind the user from the device| yes|  no |  
| Modify device alias   | Update device alias| yes|  no |  
|Edit device location information  | Modify device latitude and longitude, city code information| yes|  no |  
| Get device alias   | Query device alias| yes|  no |  
| Get device location information  | Query device location information| yes|  no |  
| Get device details  | Query device details| yes|  no |  
| Query whether the device is online  | Users can check if the device is online| yes|  no |  


#### Query user device list 
> Get a list of devices that users bind themselves to and share with others. You can get your own bound device by authType=owner filtering.  

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/deviceinfos`</br>
**HTTP Method：** GET

**Input parameters**  
No input parameters  
   
|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
| | | | |&emsp;|  


**Output parameters:**  
  
|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceinfos| DeviceInfo[]| Body|yes | Device information list  |  


##### 2、Request sample
**User request**  
```
Header：
    Connection: keep-alive
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```
**Request response**  
```
{
  "deviceinfos": [
    {
      "deviceName": "测试",
      "deviceId": "100823",
      "wifiType": "0000000000000000000000000",
      "deviceType": "00000",
      " totalPermission": {
        "view": true,
        "set": true,
        "control": true
      }，"permissions": [
        {
          "authType": "home",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "share",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "owner",
          "auth": {
            "view": true,
            "set": true,
            "control": true
          }
        }
      ]，"online": false,
"deviceVersion": {
         "deviceId ": "100823",
       "modules": [
           {
             "moduleId": "1",
            "moduleType": "1",
         "moduleInfos": [
             {
               "key1": "value1"
             },
             {
               "key2": "value2"
             }
           ]
         },
         {
           "moduleId": "2",
           "moduleType": "1",
           "moduleInfos": [
             {
               "key1": "value1"
             },
             {
               "key2": "value2"
             }
           ]
         },
         {
           "moduleId": "1",
           "moduleType": "1",
           "moduleInfos": [
             {
               "key1": "value1"
             },
             {
               "key2": "value2"
             }
           ]
         }
       ],
       "wifiType": "00000",
       "deviceType": "00000",
       "baseProperty": {
         "brand": "1",
         "model": "1",
         "others": [
           {
             "key1": "value1"
           },
           {
             "key1": "value1"
           }
         ]
       }，"location": {
         "longitude": "120",
         "latitude": "45",
         "cityCode ": "010"
       }
     },
    },
    {
      "deviceName": "测试",
      "deviceId": "100823",
      "wifiType": "0000000000000000000000000",
      "deviceType": "00000",
      " totalPermission": {
        "view": true,
        "set": true,
        "control": true
      }，"permissions": [
        {
          "authType": "home",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "share",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "owner",
          "auth": {
            "view": true,
            "set": true,
            "control": true
          }
        }
      ],
      "online": false,
     "deviceVersion": {
}
    },
    {
      "deviceName": "测试",
      "deviceId": "100823",
      "wifiType": "0000000000000000000000000",
      "deviceType": "00000",
      " totalPermission": {
        "view": true,
        "set": true,
        "control": true
      }，"permissions": [
        {
          "authType": "home",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "share",
          "auth": {
            "view": true,
            "set": false,
            "control": false
          }
        },
        {
          "authType": "owner",
          "auth": {
            "view": true,
            "set": true,
            "control": true
          }
        }
      ],
      "online": false,
 "deviceVersion": {
}
    }
  ],
  "retCode": "00000",
  "retInfo": "成功"
}
```
##### 3、Interface error code
> G20202

#### Binding device
> Establish a binding relationship between the user and the device  

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|body|yes|Device ID |
|name|String|body|yes|Device name |   
|bindReqSn|String|body|yes|Random number generated by uSDK|  
|data|String|body|yes|Bind encrypted data (get encrypted string from sdk)|  

**Output parameters**    
Output standard response parameters  

##### 2、Request sample
**User request**
```
Header：
    Connection: keep-alive
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body
{
    "deviceId": "DC330D01FBF1",
    "name": "test002",
    "data": "5f10bf4f5af08db934c8165c32140227"
}
```
**Request response**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}
```
##### 3、Interface error code
> G20202、G20904、G20908




#### Unbind device
> Unbind the user from the device

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/unbindDevice` </br>
**HTTP Method：** POST

**Input parameters**  
  
|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|Device ID  |

**Output parameters**    
Output standard response parameters

##### 2、Request sample

**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```
**Request response**
```
{
    "retCode":"00000",
    "retInfo":"成功！"
}
```
##### 3、Interface error code
> G20202



#### Modify device alias
> Update device alias

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/updateAliasName`</br>
**HTTP Method：** PUT

**Input parameters**  

|parameter name|types|location|required|description |   
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|Device ID|    
|aliasName|String|Body|yes|A new alias for the device, up to 50 characters, cannot have special characters|     

**Output parameters**  
Output standard output parameters.  

##### 2、Request sample
**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body:
{
  "aliasName ": "My device"
}

```

**Request response**

```
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、Interface error code
>G20202  


#### Edit device location information
> Modify device latitude and longitude, city code information  

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/updateLocation`</br>
**HTTP Method：** PUT

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID  |
|loc|Location|body|yes|location information  |  

**Output parameters**  
Output standard output parameters  
##### 2、Request sample
**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
body：
	{
    "loc": {
    "longitude ": "120",
    " latitude ": "45",
    " cityCode ": "010"
  		}
	}

```

**Request response**

```
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、Interface error code
> G20202  



#### Get device alias
> Query device alias  

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID|  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|alisaName|String|body|yes|Device alias  |


##### 2、Request sample
**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```

**Request response**

```
{
    "aliasName": "test002",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、Interface error code
> G20202  



#### Get device location information
> Query device location information

##### 1、Query device location information
?> **Access address：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID|  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|loc|Lociation|body|yes|Device location information|      


##### 2、Request sample
**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```

**Request response**

```
{
  "loc"：{
    "longitude": "120",
    "latitude": "45",
    "cityCode": "010"
  },
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、Interface error code
> G20202  




#### Get device details  
> Query device details  

##### 1、Interface definition  

?> **Access address：** `/uds/v1/protected/{deviceId}/deviceInfo`</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID  |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceVersion|DeviceVersion|body|yes|Device Information |   

##### 2、Request sample

**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```

**Request response**

```
{
  " deviceVersion": {
    "deviceId ": "100823",
    "modules": [
      {
        "moduleId": "1",
        "moduleType": "1",
        "moduleInfos": [
          {
            "key1": "value1"
          },
          {
            "key2": "value2"
          }
        ]
      },
      {
        "moduleId": "2",
        "moduleType": "1",
        "moduleInfos": [
          {
            "key1": "value1"
          },
          {
            "key2": "value2"
          }
        ]
      },
      {
        "moduleId": "1",
        "moduleType": "1",
        "moduleInfos": [
          {
            "key1": "value1"
          },
          {
            "key2": "value2"
          }
        ]
      }
    ],
    "wifiType": "00000",
    "deviceType": "00000",
    "baseProperty": {
      "brand": "1",
      "model": "1",
      "others": [
        {
          "key1": "value1"
        },
        {
          "key1": "value1"
        }
      ]
    }，"location": {
      "longitude": "120",
      "latitude": "45",
      "cityCode ": "010"
    }
  },
  "retCode": "00000",
  "retInfo": "成功"
}


```

##### 3、Interface error code

> G20202


#### Query whether the device is online  
> Users can check if the device is online  

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/isOnline`</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|isOnline|String|Body|yes|status information|  

##### 2、Request sample

**User request**

```
Header：
    appId: MB-FRIDGEGENE1-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
    timestamp: 1491014596343 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
```

**Request response**

```
{
    "isonline": "true",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、Interface error code
> G20202  


## Way of use

### Opening process
![开通流程][DevicesStandard_liucheng]

### Application scenario
The device management service (standard version) mainly implements the basic management services related to the smart connected device, such as binding the user to the device, unbinding the device, and obtaining the user device list for the developed application.

## Documentation
[DeviceManagementServiceStandardEditionV2.0.1][DevicesStandard_document_url]


[^-^]:文本连接注释
[DevicesStandard_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[DevicesStandard_type]:_media/_devicesStandard/DevicesStandard_type.png
[DevicesStandard_liucheng]:_media/_devicesStandard/DevicesStandard_liucheng.png
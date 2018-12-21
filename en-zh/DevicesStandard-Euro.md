
> **current version：** [UWS DeviceManagementServiceStandardEditionV2.0.2](en-us/ChangeLog/DevicesStandard)  
**Update time** {docsify-updated} 

## Introduction  
The Device Management Service Standard Edition provides developers with a basic capability service for the management of intelligent terminal device information. By integrating this service module, the developer can provide the basic maintenance capability of binding the user to the device, presenting the device status, location, and other attributes of the device, and realize rapid development of the basic capability integration related to the smart device.  
![设备管理标准版图片][DevicesStandard_type]


**Binding and unbinding**</br>
1、Bind device: The user associates with the device to form a binding relationship. The binding user becomes the device administrator.  
2、Unbind the device: The user unbinds the device and the related sharing of the device is also released.</br>
3、My device list: Used to query all smart connected devices that are bound to a single user.</br>

**Query and obtain device information**</br>
1、Information inquiry: including the ability to query whether the device is online, query the brand information of the device, and query the location of the room where the device is located.</br>
2、Obtain device attribute information: including obtaining device alias, acquiring device location new information, device detail information, device signal strength, and other related information for application presentation or other service operations based on device information.</br>

**Device information modification**</br>
1、The device management service standard version can be used to modify device-related attributes and information, including updating location information, adding device brand information, and updating device aliases.</br>



### Application scenario
The device management service (standard version) mainly implements the basic management services related to the smart connected device, such as binding the user to the device, unbinding the device, and obtaining the user device list for the developed application.


## Public structure description   

### AuthInfo
Permission content, at least one of which is ture

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
view|Boolean|Is there a right to view|
set|Boolean|Is there a configuration permission|
control|Boolean|Is there control|

### Permission
Permission information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
auth|AuthInfo|Permission content|
authType|String|Permission type|Home: family sharing</br>share: personal sharing</br>owener: device owner</br>server: permissions to appserver  

### DeviceBriefInfo
Brief information about the device

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
deviceName|String|Device name, equivalent to alias name|
deviceId|String|Device ID|
wifiType|String|Device wifitype|
deviceType|String|Device  category|
online|Boolean|Whether online|  

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


### Location
Location information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
longitude|Double|longitude|
latitude|Double|Dimension|
cityCode|String|City code|

### DeviceNetQualityDto
Equipment signal strength 

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
softwareType|String|Software type Platform information|
hardwareType|String|Hardware version type|
hardwareVers|String|Hardware version number|
softwareVers|String|Software version number  
netType|String|Network type|Possible value:</br Unknown, unknown network or device does not support network quality report;</br>Wifi, WIFI network;
strength|String|Signal strength|  

### DaviceStatus
Davice status

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
timestamp|long|The time stamp|
deviceId|String|DeviceId|
statuses|Map<String, String>|Davice status| 

### RoomInfoLocation
Room info location

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
userId|String|UserId|
deviceId|String|DeviceId|
room|String|Equipment room location information|  

### DeviceRoomInfoDto
Device room location information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
deviceName|String|Device name, equivalent to alias name|
deviceId|String|DeviceId|
wifiType|String|Device wifitype|  
deviceType|String|Device category|  
room|String|quipment room location information|  
permissions|Permission[]|Permission information|  
online|Boolean|Whether online|  


### BrandInfo
Brand/Model of information

Field name|Types|Description|Remarks  
:-|:-:|:-:|:-
brand|String|Brand|
model|String|Model|
deviceId|String|DeviceId|  

## Interface list

### Device class interface

> API interface overview


| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
|User binding device  | Establish a binding relationship between the user and the device| yes|  no |  
| Unbind device  |The user unbundles the device and unshares the device| yes|  no |  
| Get device alias   | Query device alias| yes|  no |  
| Get device location information  | Query device location information| yes|  no |  
| Get device details  | Query device details | yes|  no |  
|Update location information | Users with configuration privileges change the device's location information| yes|  no |  
| Update device alias | Update device alias| yes|  no |  
| Query user device list   | Get a list of devices that users bind themselves to and share with others| yes|  no |  
| Query whether the device is online  | Users can check if the device is online| yes|  no |  
| Obtain the signal strength of the device |Obtain the signal strength of the device| yes|  no |  
| Get the latest status of the device |Get the latest status of the device | yes|  no |  
| Save device room location information |Save device room location information| yes|  no |  
| Query device room location information |Query device room location information| yes|  no |  
|Add device brand information |Add device brand information| yes|  no |  
|Query device brand information |Query device brand information| yes|  no |  


#### User binding device
> Establish a binding relationship between the user and the device.<font color=red>(Single user binding equipment number < = 100, binding equipment must platform online.)</font>   

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
> A00001、B00001、G20202、A00004、B00001、D00006、G20904、G20908、G20910


#### Unbind device
> The user unbundles the device and unshares the device

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
> A00001、B00001、G20202、A00004、B00001、D00006

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
|deviceId|String|body|yes|DeviceID  |  

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
    "deviceId": "DC330D01FBF1",
    "retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、Interface error code
> A00001、B00001、G20202、A00004、B00001、D00006  



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
> A00001、B00001、G20202、A00004、B00001、D00006 




#### Get device details  
> Have device view and above users, inquiry device information 

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

> A00001、B00001、G20202、A00004、B00001、D00006



#### Update location information
> Users with configuration privileges change the device's location information 

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/location`</br>
 **Historical reasons for the European environment old version App uses the following address:**  
`/uds/v1/protected/{deviceId}/updateLocation`  
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
> A00001、B00001、G20202、A00004、B00001、D00006  


#### Update device alias
> Users with configuration privileges change the device alias

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/aliasName`</br>
 **Historical reasons for the European environment old version App uses the following address:**  
`/uds/v1/protected/{deviceId}/updateAliasName`  
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
>A00001、B00001、G20202、A00004、B00001、D00006  



#### Query user device list 
> Query all my devices, including my devices, personal sharing to my devices, family sharing to my devices.  

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
> A00001、B00001、G20202、A00004、B00001、D00006  

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
> A00001、B00001、G20202、A00004、B00001、D00006 


#### Obtain the signal strength of the device  
> Obtain the signal strength of the device   

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/deviceNetQuality`</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceNetQualityDto|DeviceNetQualityDto|Body|yes|Signal strength|  

##### 2、Request sample

**User request**

```
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: 59e44ce6ddda0378f75fba0ec381fabbcaf1d22d94078495da3da0e51609b94d
timestamp: 1491014535850 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```

**Request response**

```
{
"deviceNetQualityDto":{"hardwareType":"R","hardwareVers":"1.0.00","softwareType":"UDISCOVERY_UWT","softwareVers":"2.3.12","netType":"Wifi","strength":"100"},
"retCode":"00000",
"retInfo":"成功!"
}

```

##### 3、Interface error code
> A00001、B00001、G20202、A00004、D00006



#### Get the latest status of the device  
> Get the latest status of the device    

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/lastReportStatus `</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|yes|DeviceID |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|DeviceStatus|DeviceStatus|Body|yes|Device status|  

##### 2、Request sample

**User request**

```
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: 59e44ce6ddda0378f75fba0ec381fabbcaf1d22d94078495da3da0e51609b94d
timestamp: 1491014535850 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```

**Request response**

```
{
"devicestatus":{"timestamp":"1502854990142","deviceId":"DC330D003121","statuses":[{"name":"2000ZX","value":""},{"name":"623006","value":"323000"}]},
"retCode":"00000",
"retInfo":"成功!"
}


```

##### 3、Interface error code
> A00001、B00001、G20202、A00004、D00006  


####  Save device room location information  
> Save device room location information    

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/saveDeviceRoom`</br>
**HTTP Method：** POST

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|token|String|Context|yes|user token |  
|deviceId|String|url|yes|DeviceID |  
|room|String|body|yes|Device  room location information |  


**Output parameters**  
Output standard response parameters

##### 2、Request sample

**User request**

```
POST data:
{
  "room":"living room"
}

[no cookies]
Request Headers:
Connection: keep-alive
appId: MB-UZHSH-0000
appVersion: 99.99.99.99990
clientId: 2
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: ebcd5fc1387d647fa890cfa0ea91fc2d3c382316d9ae24ddf762cbbe76175aca
timestamp: 1503395034831 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json

```

**Request response**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、Interface error code
> A00001、B00001、G20202、A00004、D00006



#### Query device room location information  
> Query device room location information    

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/deviceAndRoom `</br>
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|token|String|Context|yes|user token |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceRoomInfos|DeviceRoomInfoDto[]|Body|yes|Device room location information|  

##### 2、Request sample

**User request**

```
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: 59e44ce6ddda0378f75fba0ec381fabbcaf1d22d94078495da3da0e51609b94d
timestamp: 1491014535850 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```

**Request response**

```
{
    " deviceRoomInfos": [
       {
           "deviceId": "0007A8C17DBD",
           "deviceName": "Air purifier",
           "deviceType": "21001001",
           "online": false,
           "permissions": [
               {
                   "auth": {
                       "control": true,
                       "set": true,
                       "view": true
                   },
                   "authType": "owner"
               }
           ],
           "room": "Living room",
           "wifiType": "111c12002400081021010000005a4e4b32303134303931313031000000000000"
        }
    ],  
"retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、Interface error code
> A00001、B00001、G20202、A00004、D00006  



#### Add device brand information  
> Add device brand information     

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/saveDeviceBrand `</br>
**precondition:** User has bound the device  
**HTTP Method：** POST

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|token|String|Header|yes|user token |  
|brandInfo|BrandInfo|Body|yes|Brand information |  

**Output parameters**  

Output standard output parameters.  

##### 2、Request sample

**User request**

```
POST data:
{   
	"brandInfo":{
		"brand":"brand",
		"model":"model",
		"deviceId":"0007A8947C62"
	}
	
}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: xb001
sequenceId: 20161020153428000015
accessToken: TGT25OW8U5CU9QP02GSFD4PR676C20
sign: 116639d4405230df33486ceb5ea68bd2c685549c48cf83ce1cafd8dfaa336c7a
timestamp: 1506061471587 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json 


```

**Request response**

```
{
	"retCode": "00000",
    "retInfo": "成功!"
}


```

##### 3、Interface error code
> A00001、B00001、G20202、20903、D00006



#### Query device brand information 
> Query device brand information   

##### 1、Interface definition
?> **Access address：** `/uds/v1/protected/{deviceId}/deviceBrand`</br>
**precondition:** User and device are related  
**HTTP Method：** GET

**Input parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|token|String|Header|yes|user token |  
|deviceId|String|url|yes|deviceId |  

**Output parameters**  

|parameter name|types|location|required|description |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|brandInfo|BrandInfo|Body|yes|Device brand information|  

##### 2、Request sample

**User request**

```
Request Headers:
Connection: keep-alive
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: xb001
sequenceId: 20161020153428000015
accessToken: TGT25OW8U5CU9QP02GSFD4PR676C20
sign: 384a9e720d4e218c7d7d44d81f88c8b8c198bb660b900f8679607aaeba198a61
timestamp: 1506061897331 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8 
Content-type: application/json


```

**Request response**

```
{
"brandInfo":{
"brand":"brand",
"deviceId":"0007A8947C62",
"model":"model"
},
"retCode":"00000",
"retInfo":"成功!"
}


```

##### 3、Interface error code
> A00001、B00001、G20202、20903、D00006、G24001





[^-^]:文本连接注释
[DevicesStandard_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[DevicesStandard_type]:_media/_devicesStandard/DevicesStandard_type.png
[DevicesStandard_liucheng]:_media/_devicesStandard/DevicesStandard_liucheng.png

!> **更新时间**：{docsify-updated}  


## 简介

设备管理服务为开发者提供对智能终端设备信息的管控的基础能力服务。开发者通过集成此服务模块，可提供用户与设备绑定、展现设备状态、位置等信息以及设备部分属性的基础维护能力，实现智能设备相关的基础能力集成的快速开发。

Haier U+云平台提供的对智能互联设备的操作设备进行命令下发、读写等相关设备的操作能力。


### 基础信息管理服务流程
基础信息管理服务主要为开发的应用程序实现用户与设备绑定、解绑设备、获取用户设备列表等与智能互联设备相关的基础管理服务。
![设备管理场景流程][DevicesStandard_flow]

### 设备控制服务流程

设备管理服务基于用户拥有对应的设备权限，即设备管理员（绑定用户）或权限用户。

设备操作命令可通过业务服务上报到平台，平台将控制命令解析并下发到智能设备进行控制

![设备管理企业版场景流程][DeviceE_flow]



## 接口公共部分说明
### AuthInfo
权限内容，其中至少一项为ture

参数名|类型|说明|备注
:-|:-:|:-:|:-
view|Boolean|是否有查看权限|
set|Boolean|是否有配置权限|
control|Boolean|是否有控制权限|

### Permission
权限信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
auth|AuthInfo|权限内容|
authType|String|权限类型|home：家庭分享</br>share：个人分享</br>owener：设备主人</br>server：给appserver的权限

### DeviceBriefInfo

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备WiFitype|
deviceType|String|设备类别|
online|Boolean|是否在线|



### DeviceInfo

绑定设备信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifi|
deviceType|String|设备类型|
totalPermission|AuthInfo|权限和，权限信息的综合|
permissions|Permission[]|权限信息|
online|Boolean|是否在线|

### BaseProperty
基础属性

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|设备品牌|
model|String|设备型号|
others|Map<String,Strng>|其他属性|

### Deviceversion
设备详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|String|设备ID|
modules|Set<Module>|模块信息|
wifiType|String|wifi类型|
deviceType|String|设备类型|
baseProperty|BaseProperty|品牌信息|
location|Location|位置信息|

### Module
模块信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
moduleId|String|模块ID|
moduleType|String|模块类型|
ModuleInfos|Map<String,String>|模块其他信息|

### Location
定位信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
longitude|Double|经度|
latitude|Double|维度|
cityCode|String|城市编码|

### DeviceNetQualityDto
设备信号强度

参数名|类型|说明|备注
:-|:-:|:-:|:-
softwareType|String|软件类型，平台信息|
hardware|String|硬件版本类型|
hardwareVers|String|硬件版本号|
softdwareVers|String|软件版本号|
netType|String|网络类型|可取值：</br>unknown,位置网络或设备不支持挽留过质量上报；</br>Wifi：WIFI网络
strength|String|信号强度|

### DeviceStatus
设备状态

参数名|类型|说明|备注
:-|:-:|:-:|:-
timestamp|long|时间戳|
deviceId|String|设备Id|
statuses|Map<String,String>|设备状态|

### RoomInfoLocation
房间位置

参数名|类型|说明|备注
:-|:-:|:-:|:-
userId|String|用户ID|
deviceId|String|设备Id|
room|String|设备房间位置信息|

### DeviceRoomInfoDto
设备房间位置信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifitype|
deviceType|String|设备类别|
room|String|设备房间位置信息|
permission|Permission[]|权限信息|
online|Boolean|是否在线|

### BrandInfo
品牌/型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|品牌|
model|String|型号|
deviceId|String|设备ID|  


### DevFWVersion  
设备整机固件版本信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
model|String|U+产品整机型号编码|
vers|String|设备当前固件版本号。字符串格式，不做其余格式校验|
firmware|Firmware|设备可升级的新版本固件信息|如果设备不支持升级或者当前没有更新的固件，则不存在firmware字段    


### Firmware
设备可升级的新版本固件信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
vers|String|新版本固件版本号。字符串格式，不做其余格式校验|
desc|String|新版本固件描述信息|
timeout|Int|升级超时时间，单位：秒，取值范围：10~604800（7天）|升级超时时间平台仅用于记录，并且在升级进度及结果中返回给用户，具体超时判断由App进行  
digestAlg|String|设备固件文件摘要算法，可取值：sha256 -- SHA256计算摘要|   
digest|String|设备固件文件摘要信息，格式：全小写十六进制字符串|
url|String|设备固件文件URL地址|
keyAlg|String|设备固件文件加密算法。如果设备固件未加密，则值为空字符串。可取值：aes256 -- AES256加密|  
key|String|设备固件文件加密密钥，格式：全小写十六进制字符串。如果设备固件未加密，则值为空字符串|  
size|int|原始升级包大小|


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

### 绑定与解绑

#### 绑定
> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=300个，绑定时设备必须平台上线。）</font>

?> 具体绑定逻辑见uSDK的绑定方法


<div style='display: none'> 
##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID
name|String|body|必填|设备名称
data|String|body|必填|绑定加密数据

**输出参数:** 输出标准应答参数

##### 2、请求样例
**请求样例**
```
请求地址：/uds/v1/protected/bindDevice
Header：
    Connection: keep-alive
    appId: MB-****-0000
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
##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006、G20904、G20908、G20910

</div>


#### 解绑设备
> 用户解绑设备，同时解除设备的相关分享

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/unbindDevice` </br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/B00000000002/unbindDevice
Header：
    appId: MB-****-0000
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
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功！"
}
```
##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006

### 设备信息管理接口

#### 获取设备别名
> 获取设备aliasName（别名）

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
alisaName|String|body|必填|设备别名
deviceId|String|body|必填|设备ID

##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/aliasName
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "aliasName": "test002",
    "deviceId": "DC330D01FBF1",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 获取设备位置信息
> 获取设备的loaction（位置）内容

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
loc|Lociation|body|必填|设备位置信息


##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/location
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "loc": 
    {
        "cityCode": "101031900",
        "latitude": 0,
        "longitude": 0
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006




#### 获取设备详细信息
> 拥有设备查看权限的用户，查询设备的详细信息

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceInfo`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceVersion|DeviceVersion|body|必填|设备信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/deviceInfo
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "deviceVersion": {
        "baseProperty": {
            "model": "33"
        },
        "deviceId": "DC330D01FBF1",
        "deviceType": "03001001",
        "location": {
            "cityCode": "101031900",
            "latitude": 0,
            "longitude": 0
        },
        "modules": [
            {
                "moduleId": "",
                "moduleInfos": {
                    "hardwareVers": "1.0.00",
                    "softwareVers": "2.3.01",
                    "hardwareType": "R",
                    "softwareType": "eSDK_WIFI_AC",
                    "supportUpgrade": "1"
                },
                "moduleType": "basemodule"
            },
            {
                "moduleId": "DC330D01FBF1",
                "moduleInfos": {
                    "hardwareVers": "R_1.0.00",
                    "platform": "eSDK_WIFI_AC",
                    "protocolVers": "1.00",
                    "configVers": "0.0.0",
                    "softwareVers": "e_2.3.01",
                    "ip": "42.123.65.3"
                },
                "moduleType": "wifimodule"
            }
        ],
        "wifiType": "00000000000000008080000000041410"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}



```

##### 3、接口错误码

> A00001、B00001、G20202、A00004、B00001、D00006



#### 更新位置信息
> 有配置权限的用户更改设备的位置信息

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** PUT

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID
loc|Location|body|必填|位置信息

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/location
PUT data:
{"loc":{"cityCode":"101010200","latitude":39.975675,"longitude":116.322712} }

Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: dd9e75faed4fe74b49494012512515f787c9fb28f74d286dde47790139509cd2
timestamp: 1491014310003 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 更新设备别名
> 有配置权限的用户更改设备别名

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** PUT

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID
aliasName|String|body|必填|设备新别名

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/aliasName

PUT data:
{"aliasName":"拨测的设备"}

Header：
    appId: MB-****-0000
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
Body
{
    "aliasName": "s"
}


```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码


> A00001、B00001、G20202、A00004、B00001、D00006



#### 我的设备列表
> 查询我所有的设备,包含我的设备,个人分享给我的设备,家庭分享给我的设备

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/deviceinfos`</br>
**HTTP Method：** GET

**输入参数：** 无

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceinfos|DeviceInfo[]|body|必填|共享设备信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/deviceinfos
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "deviceinfos": [
        {
            "deviceId": "0007A8C17DBD",
            "deviceName": "空气净化器1数",
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
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "111c12002400081021010000005a4e4b32303134303931313031000000000000"
        },
        {
            "deviceId": "C8934640B1DE",
            "deviceName": "空气盒子1",
            "deviceType": "14001001",
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
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "101c120024000810140d00118003940000000000000000000000000000000000"
        },
        {
            "deviceId": "DC330D01FBF1",
            "deviceName": "test002",
            "deviceType": "03001001",
            "online": true,
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
            "totalPermission": {
                "control": true,
                "set": true,
                "view": true
            },
            "wifiType": "00000000000000008080000000041410"
        }
    ],
    "retCode": "00000",
    "retInfo": "成功!"
}


```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006




#### 查询设备是否在线
> 用户可以查看设备是否在线

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/isOnline`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
isOnline|String|Body|必填|状态信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/isOnline
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "isonline": "true",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、B00001、D00006



#### 查询设备信号强度
> 获取设备的信号强度

##### 1、接口定义

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceNetQuality`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceNetQuality|DeviceNetQualityDto|Body|必填|设备ID

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/deviceNetQuality
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "deviceNetQualityDto":
    {
        "hardwareType":"R",
        "hardwareVers":"1.0.00",
        "softwareType":"UDISCOVERY_UWT",
        "softwareVers":"2.3.12",
        "netType":"Wifi",
        "strength":"100"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006



#### 获取设备最新状态
> 获取到用户最新上报的状态信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/lastReportStatus`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
DeviceStatus|DeviceStatus|Body|必填|设备状态

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/lastReportStatus
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "devicestatus":
    {
        "timestamp":"1502854990142",
        "deviceId":"DC330D003121",
        "statuses":
        [
            {"name":"2000ZX","value":""},
            {"name":"623006","value":"323000"}
        ]
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006



#### 保存房间位置信息<sup style="color:red">(已作废，业务移植到家庭模型中)<sup>
> 保存房间位置信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/saveDeviceRoom`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|上下文|必填|用户token
deviceId|String|url|必填|设备ID
room|String|body|必填|设备房间位置信息

**输出参数:** 输出标准应答参数

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/DC330D01FBF1/saveDeviceRoom
POST data:
{
  "room":"客厅"
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

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006



#### 查询房间位置信息<sup style="color:red">(已作废，业务移植到家庭模型中)<sup>
> 获取设备信息及房间位置信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/deviceAndRoom`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|上下文|必填|用户token

**输出参数** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceRoomInfos|DeviceRoomInfoDto[]|body|必填|房间位置信息

##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/deviceAndRoom
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

**请求应答**

```
{
    " deviceRoomInfos": [
       {
           "deviceId": "0007A8C17DBD",
           "deviceName": "空气净化器1数",
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
           "room": "卧室",
           "wifiType": "111c12002400081021010000005a4e4b32303134303931313031000000000000"
        }
    ],  
"retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、接口错误码
> A00001、B00001、G20202、A00004、D00006



#### 添加设备品牌信息
> 为设备添加品牌信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/saveDeviceBrand`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|Header|必填|用户token
brantInfo|BrandInfo|body|必填|设备品牌信息

**输出参数：** 输出标准应答参数 

##### 2、请求样例

**请求样例**
```
请求地址：/uds/v1/protected/saveDeviceBrand
POST data:
{   
	"brandInfo":{
		"brand":"品牌",
		"model":"型号",
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

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、20903、D00006



#### 查询设备品牌信息
> 查询设备的品牌信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceBrand`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|上下文|必填|用户token
deviceId|String|url|必填|设备ID

**输出参数：** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
brantInfo|BrandInfo|body|必填|设备品牌信息

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/0007A8947C62/deviceBrand
Header：
    appId: MB-****-0000
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

**请求应答**

```
{
    "brandInfo":
    {
        "brand":"品牌",
        "deviceId":"0007A8947C62",
        "model":"型号"
    },
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、接口错误码
> A00001、B00001、G20202、20903、D00006、G24001  


#### 检测设备整机固件版本信息  
> 检测设备整机固件版本信息  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/fota/checkDevFMVersion`</br>
**HTTP Method：** POST  
**前置条件：** 用户和设备有绑定关系   

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
token|String|Header|必填|用户token
deviceId|String|Body|必填|设备ID

**输出参数：** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
devFWVersionDto|DevFWVersionDto|body|必填|设备整机固件版本信息  

##### 2、请求样例

**请求样例**

```
请求地址：/uds/v1/protected/fota/checkDevFMVersion  
POST data:
{   
	"deviceId":"0007A8947C62"
}

[no cookies]

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

**请求应答**

```
{
   "devFWVersionDto":
   {
      "firmware":
      {
         "desc":"2222222",
         "digest":"4387f38fa0ac5e911dd02c32b2beeba6",
         "digestAlg":"",
         "key":" ",
         "keyAlg":" ",
         "timeout":10,
      "url":"http://uhome.haier.net:80/resource/enabling/operation/mgmtUpgradeFileUpload/1558496591300.bin",
         "vers":"1.2.00",
         “size”:111
      },
      "model":"1a2b3c4d5e6f7g8h",
      "vers":"1.0.00"
   },
   "retCode":"00000",
   "retInfo":"成功"
}

```

##### 3、接口错误码
> A00001、B00001、G20202、G00003  


#### 查询设备绑定的用户

查询设备绑定的用户

##### 1、接口定义
?> **接入地址：** `/udse/v1/devBindUsers`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
users|User[]|Body|是|用户列表

##### 2、请求示例

**请求样例**
```
请求地址	/udse/v1/devBindUsers

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

**请求应答**
```
{
    "retCode": "00000",
    "retInfo": "成功!",
    "users": [{
        "loginId": "mnxyxxxh2@163.com",
        "userId": "838670064340172800"
    },
    {
        "loginId": "mnxyxxxh1@163.com",
        "userId": "844794014807883776""userProfile": {
            "country": "italy",
            "nickName": "名字",
            "avatar": "123"
        }
    }]
}

```


#### 非标准设备命令下发

非标准设备命令（单命令、组命令）下发


##### 1、接口定义
?> **接入地址：** `/udse/v1/devOp`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备id
sn|String|Body|必填|操作流水号。必须唯一
category|String|Body|必填|操作的分类。单命令："AttrOp"组命令："GroupOp"其他：见下方备注
name|String|Body|必填|操作的名称
operateCodes|String|Body|必填|操作命令Base64加密值
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议		

**备注**

名称|category|Name|operateCodes|说明				
:-|:-:|:-:|:-:|:-
查询设备所有状态|Op|queryAllStatuses|e30=|&nbsp;
查询设备所有状态|Op|queryAttrStatus|e30=|只有社区洗设备支持 			
查询设备告警|Op|queryCurrentFaults|e30=|&nbsp; 		
					
**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
usn|String|Body|是|操作序列号

##### 2、请求示例

**请求样例**
```
请求地址	/udse/v1/devOp
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

**请求应答**
```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}


```

**操作应答数据**
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

##### 3、错误码

> C00002 C00004 C00006 00008 G20202 G03002




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

**请求样例**

```
请求地址	/udse/v1/devOpStatus
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


##### 3、错误码

> 00000 B00001 C00002 C00006 D00001 G20202


#### 读取标准模型设备属性
> 读取标准模型设备属性

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
> 00000 B00001 C00002 C00006 D00001 G20202



#### 写入标准模型设备属性

> 写入标准模型设备属性


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

**请求样例**

```
请求地址	/udse/v1/stdDevPropertyWrite
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

**请求应答**

```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```

**操作应答数据**

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

##### 3、错误码

> 00000 B00001 C00002 C00006 D00001 G20202


#### 操作标准模型设备

> 操作标准模型设备


##### 1、接口定义
?> **接入地址：** `/udse/v1/stdDevOperate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
operationName|List<OpPropertyValue>|Body|必填|操作名称
operationValue|String|Body|必填|属性值的列表，由模型文档决定是否必填及如何填。
sn|String|Body|必填|设备操作请求序列号
callbackUrl|String|Body|非必填|操作应答回调地址,只支持http协议


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-			
usn|String|Body|必填|操作序列号
				

##### 2、请求示例

**请求样例**

```
请求地址	/udse/v1/stdDevOperate
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

**请求应答**

```
{
"retCode": "00000",
"retInfo": "成功!",
"usn": "600ce95da3e14fc7a68f483dd14db864"
}

```
**操作应答数据**

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

##### 3、错误码

> 00000 B00001 C00002 C00006 D00001 G20202



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

> 用户读属性-异步接口，支持标准模型的



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


#### 业务通道（下行）

> 业务通道（下行）


##### 1、接口定义
?> **接入地址：** `/udse/v1/opBusinessDown`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
args|String|Body|必填|Base64加密值：e++协议中，8D下行透传数据内容，源数据Base64编码后的字符串 

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-		
usn|String|Body|必填|操作序列号
				

##### 2、请求示例

**请求样例**

```
请求地址	/udse/v1/opBusinessDown
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
"args": "e30="
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


##### 3、错误码

> C00002 C00004 G20202 G03002 00000





#### 保存设备房间位置信息

> 保存设备房间位置信息


##### 1、接口定义
?> **接入地址：** `/udse/v1/protected/{deviceId}/saveDeviceRoom`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID
token|String|上下文|必填|用户token
room|String|Body|必填|设备房间位置信息

**输出参数**

输出标准参数
				

##### 2、请求示例

**请求样例**

```
请求地址	/udse/v1/protected/ DC330D01FBF1/saveDeviceRoom
POST data:
{
  "room":"客厅"
}

[no cookies]
Request Headers:
Connection: keep-alive
appId: SV-UZHSH-0000
appVersion: XX.XX.XX.XXXXX
clientId: 1234
sequenceId: sdfsadf
sign: a5dfb2c8b9b9b75edc99ef79a16310e80da016c1431b897bbaaf741882f40546
timestamp: 1558594327794
language: zh-cn
timezone: +8
appKey: 2ba149e67de2e4dfae30a82abec26a3a
Content-Encoding: utf-8
Content-type: application/json
accessToken: TGTH5R94VU4X6AD27K304JHF0M8R00

```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```


##### 3、错误码

> A00001、B00001、G20202、A00004、D00006



#### 更新位置信息

> 有配置权限的用户更改设备的位置信息


##### 1、接口定义
?> **接入地址：** `/udse/v1/protected/{deviceId}/location`</br>
**HTTP Method：** PUT

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|url|必填|设备ID
loc|Location|Body| &nbsp;|位置信息

**输出参数**

输出标准参数
				

##### 2、请求示例

**请求样例**

```
请求地址	/udse/v1/protected/DC330D01FBF1/location
PUT data:
{"loc":{"cityCode":"3555","latitude":22.33,"longitude":11.22}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-UZHSH-0000
appVersion: XX.XX.XX.XXXXX
clientId: 1234
sequenceId: sdfsadf
sign: a0a47ff522c0226b1f0ba50b0a9c336d8d84c200745b0b07d04b5cb2ae06e2b9
timestamp: 1558594625859
language: zh-cn
timezone: +8
appKey: 2ba149e67de2e4dfae30a82abec26a3a
Content-Encoding: utf-8
Content-type: application/json
accessToken: TGTH5R94VU4X6AD27K304JHF0M8R00

```

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```


##### 3、错误码

> A00001、B00001、G20202、A00004、B00001、D00006




### 附录
### 公共错误码

错误码|描述
:-:|:-:
00000|成功
A00001|服务不可用
A00002|网络异常
A00003|访问或操作超时
A00004|系统内部错误
A00005|数据库访问异常
A00006|未知异常
A00007|邮件服务异常
A00008|邮件发送失败
A00009|邮件发送次数超限
B00001|缺少必填参数
B00002|参数类型错误
B00003|参数数值超出值域或不是枚举值
B00004|参数不符合规则要求
B00006|参数长度错误
B00007|参数与接口定义不匹配
C00001|appId与appKey验证失败
C00002|appServer无访问授权
C00003|访问权限不足
C00004|操作权限不足
C00005|重复请求
C00006|未知设备类型
C00007|appId配置信息为空
C00008|appKey为空
D00001|sign签名错误
D00003|Token不存在，未通过token验证
D00004|Token已过期，未通过token验证
D00005|Token不是由此应用创建，未通过token验证。
D00006|会话失效
D00007|不是系统内部用户
D00008|用户不合法





[^-^]:常用图片注释
[DevicesStandard_flow]:_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:_media/_DevicesEnterprise/DeviceE_flow.png

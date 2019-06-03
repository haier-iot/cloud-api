
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

### DeviceBaseInfo

设备基本信息

参数名|类型|说明|备注
:-|:-:|:-:|:- 
deviceId|String|设备Id|
sern|String|机器编码|
productCodeT|String|产品型号编码|
productNameT|String|产品型号名称|
connectionStatus|String|设备连接状态|online：在线；offline：离线

### ProductInfo
产品型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
ProdcutCodeT|String|产品型号编码|
ProcutNameT|String|产品型号名称|



## 接口清单

### 绑定与解绑

#### 绑定
> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=300个，绑定时设备必须平台上线。）</font>

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
    "deviceVersion": 
    {
        "baseProperty":
        {
            "model": "33"
        },
        "deviceId": "DC330D01FBF1",
        "deviceType": "03001001",
        "location": 
        {
            "cityCode": "101031900",
            "latitude": 0,
            "longitude": 0
        },
        "modules": 
        [
            {
                "moduleId": "",
                "moduleInfos": 
                {
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
                "moduleInfos": 
                {
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
body
{
    "loc":
    {
        "longitude":"120",
        "latitude":"45",
        "cityCode":"010"
    }
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
deviceinfos|DeviceInfo|body|必填|设备信息

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
            "permissions": 
            [
                {
                    "auth": 
                    {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": 
            {
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
            "permissions": 
            [
                {
                    "auth": 
                    {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": 
            {
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
            "permissions": 
            [
                {
                    "auth": 
                    {
                        "control": true,
                        "set": true,
                        "view": true
                    },
                    "authType": "owner"
                }
            ],
            "totalPermission": 
            {
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



#### 保存房间位置信息
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
body
{
  "room":"客厅"
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
> A00001、B00001、G20202、A00004、D00006



#### 查询房间位置信息
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
deviceRoomInfoDtos|DeviceRoomInfo[]|body|必填|房间位置信息

##### 2、请求样例
**请求样例**

```
请求地址：/uds/v1/protected/deviceAndRoom
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
    " deviceRoomInfoDtos": 
    [
       {
           "deviceId": "0007A8C17DBD",
           "deviceName": "空气净化器1数",
           "deviceType": "21001001",
           "online": false,
           "permissions": 
           [
               {
                   "auth": 
                   {
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
请求地址：/uds/v1/protected/deviceAndRoom
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
POST data
{   
	"brandInfo":{
		"brand":"品牌",
		"model":"型号",
		"deviceId":"0007A8947C62"
	}
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


#### 获取设备型号信息

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

##### 1、接口定义
?> **接入地址：** `/stdudse/v1/protected/getBaseInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息

##### 2、请求示例

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/getBaseInfo
Body：
{
  “deviceId”: “DC330DC4B6EE”
}

```

**请求应答**
```
{
  "deviceBaseInfo": {
"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码

> G10001

#### 修改设备型号信息

领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

修改设备型号信息，产品编码补习已在海极网注册


##### 1、接口定义
?> **接入地址：** `/stdudse/v1/protected/modifyModelInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备id
codeType|Integer|Body|是|1、机器编码；2、产品型号编码
code|String|Body|是|具体编码
modifyType|String|Body|否|修改方式：1、APP扫码更新；2、APP用户选择；3、其他，请描述

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceBaseInfo|DeviceBaseInfo|Body|是|设备基本信息
possibleList|List<ProductInfo>|Body|否|设备可能的其他型号

##### 2、请求示例

**请求样例**
```
https://uws.haier.net/stdudse/v1/protected/modifyModelInfo
Body：
{
  "deviceId": "DC330DC4B6EE",
  "codeType": 1,
  "code": "AURGH4JIOJAVU"
}

```

**请求应答**
```
{
  "deviceBaseInfo": {
"deviceId": "DC330DC4B6EE",
    "productCodeT": "AA9Y31E00",
    "productNameT": "AS09CB1HRA内机(丹麦Haier-CF)",
    "connectionStatus": "online"
  },
"possibleList": [
  ],
  "retCode": "00000",
  "retInfo": "成功"
}

```


##### 3、错误码

> C00003


### 设备操作高级动能


#### 设备指令执行操作接口（经过逻辑运算）

> 领域模型接口使用Https协议，使用`https://uws.haier.net /+接口地址`进行访问

统一接收标准模型的命令，命令经过逻辑运算、转换、补偿后下发到设备


##### 1、接口定义
?> **接入地址：** `stdudse/v1/modfier/operate`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|上下文|必填|用户token
deviceId|String|Body|必填|设备ID
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


### 设备控制类接口

设备控制类接口通过APPSERVER访问

若APPSERVER部署在外网，则通过https://uws.haier.net/udse访问；
若APPSERVER部署在内网，则通过https://internal.uws.haier.net/udse访问；


**权限申请**

设备控制类接口服务授权，使用服务IP白名单策略，需要是用此服务请联系IOT平台技术支持开通系统IP白名单

**公共头部分**

Header 中appid 字段填写内容为系统ID，即systemid。 此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”



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





## 关于设备控制接口参数填写与返回值

### 非标准设备单命令参数

```
category: 'AttrOp',
name: '20g10o',
operateCodes:base64({"value":"30g106"})
```
### 非标准设备组命令重要参数
```
category: 'GroupOp',
name: '000001',
operateCodes:base64({"value":[{"name":"20g10o","value":"30g106"},{"name":"20g10m","value":"12"},{"name":"20g10d","value":"30g101"}]})
```
>ID文档中，name有可能写成0x0001，请传递参数时，使用000001

### sn与usn

sn由调用方生成，指请求的流水号。云平台应答该请求时，会返回usn，如果涉及到回调，回调中的usn也表示是对请求sn的回答。</br>
即，完整会话为：</br>
请求：</br>
sn，callback  ——>对应示例中的“用户请求” </br>
云平台响应请求：usn ——>对应示例中的“请求应答” </br>
调用方callback接收：usn ——>对应示例中的“操作应答数据” </br>
> sn与usn一一对应，请保证sn的唯一。

### retCode与resCode
retCode为云平台对请求的执行应答，见各接口的返回值及公共返回值</br>
resCode为设备对命令的执行情况的应答，在回调中出现，当为0时，表示设备成功执行，其他值均为错误，具体错误信息需查看错误码列表或查询ID文档或咨询设备开发工程师。

### 控制resCode错误码列表

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




[^-^]:常用图片注释
[DevicesStandard_flow]:_media/_DevicesStandard/DevicesStandard_flow.png
[DeviceE_flow]:_media/_DevicesEnterprise/DeviceE_flow.png

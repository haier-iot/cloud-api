
> **当前版本：** [UWS 设备管理服务标准版 V2.0.3](zh-cn/ChangeLog/DevicesStandard)  
**更新时间**：{docsify-updated}  

## 简介 
设备管理服务标准版为开发者提供对智能终端设备信息的管控的基础能力服务。开发者通过集成此服务模块，可提供用户与设备绑定、展现设备状态、位置等信息以及设备部分属性的基础维护能力，实现智能设备相关的基础能力集成的快速开发。
![设备管理标准版图片][DevicesStandard_type]



## 规则与约束

1、一个用户可绑定多个设备,绑定设备数据<=300个。</br>
2、一个设备只能被一个海尔用户绑定。</br>
3、发起请求绑定设备时，设备必须在平台上线，且设备上报版本信息时间不大于20分钟，即小于等于20分钟。</br>






## 应用场景
设备管理服务（标准版）主要为开发的应用程序实现用户与设备绑定、解绑设备、获取用户设备列表等与智能互联设备相关的基础管理服务。
![设备管理场景流程][DevicesStandard_flow]

**设备绑定**

设备绑定：配置设备，获取绑定信息（USDK获取，详见对应文档），调用设备绑定借口，完成设备绑定。用户与设备形成绑定关系，即可控制设备

设备解绑：用户调用设备解绑接口，即可实现设备解绑

**设备信息管理**

信息查询：包括查询设备是否在线、查询设备的品牌信息、查询设备所在房间位置等实时查询的能力服务。

获取设备属性信息：包括获取设备别名、获取设备位置新信息、设备详细信息、设备信号强度等相关信息用于应用的展现或基于设备信息的其他业务操作

**设备状态**

建立设备连接：应用端用户接入用户网关，设备处于就绪状态（设备连接设备网关并上报状态），用户和设备之间存在绑定关系，即在云端建立设备连接

设备属性上报：设备与云平台建立连接之后，将状态、属性、报警等信息上报到平台，应用端可以调用对应接口进行查看

云端设备控制：设备控制命令下发，调用设备控制接口（设备管理企业版），下发设备控制命令








## 公共结构说明
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

## 接口清单

### 绑定与解绑

#### 绑定
> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=100个，绑定时设备必须平台上线。）</font>

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


### 设备信息相关接口

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





[^-^]:常用图片注释
[DevicesStandard_type]:_media/_DevicesStandard/DevicesStandard_type.png
[DevicesStandard_liucheng]:_media/_DevicesStandard/DevicesStandard_liucheng.png
[DevicesStandard_flow]:_media/_DevicesStandard/DevicesStandard_flow.png
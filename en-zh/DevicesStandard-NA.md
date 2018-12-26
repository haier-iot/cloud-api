
> **当前版本：** [UWS 设备管理服务标准版-北美环境 V1.4.0](en-zh/ChangeLog/DevicesStandard)  
**更新时间**：{docsify-updated} 


## 简介 
设备管理服务标准版为开发者提供对智能终端设备信息的管控的基础能力服务。开发者通过集成此服务模块，可提供用户与设备绑定、展现设备状态、位置等信息以及设备部分属性的基础维护能力，实现智能设备相关的基础能力集成的快速开发。  
![设备管理标准版图片][DevicesStandard_type]

**绑定与解绑**</br>
1、绑定设备：用户与设备建立关联，形成绑定关系，绑定用户成为设备管理员；
2、解绑设备：用户与设备解除绑定关系，同时用户关于此设备的相关分享也解除；</br>
3、我的设备列表：用于查询与某单一用户绑定的所有智能互联设备。</br>

**查询与获取设备信息**</br>
1、信息查询：包括查询设备是否在线、查询设备的品牌信息、查询设备所在房间位置等实时查询的能力服务。</br>
2、获取设备属性信息：包括获取设备别名、获取设备位置新信息、设备详细信息、设备信号强度等相关信息用于应用的展现或基于设备信息的其他业务操作。</br>

**设备信息修改**</br>
1、可以通过设备管理服务标准版进行包括更新位置信息、添加设备品牌信息、更新设备别名等在内的对于设备相关属性、信息的修改操作。</br>

## 规则与约束

1、一个用户可绑定多个设备,绑定设备数据<=100个。</br>
2、一个设备只能被一个用户绑定。</br>
3、发起请求绑定设备时，设备必须在平台上线，且设备上报版本信息时间不大于10分钟，即小于等于10分钟。</br>


## 应用场景
设备管理服务（标准版）主要为开发的应用程序实现用户与设备绑定、解绑设备、获取用户设备列表等与智能互联设备相关的基础管理服务。


## 公共结构说明

### DeviceInfo  
绑定设备信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
wifiType|String|设备wifi|指typeId
deviceType|String|设备类型|大中小类别组成的8位码
totalPermission|AuthInfo|权限和权限信息的综合|
permissions|Permission[]|权限信息|
online|Boolean|是否在线|   
deviceVersion|DeviceVersion|设备版本信息| 

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

### Location
定位信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
longitude|Double|经度|
latitude|Double|维度|
cityCode|String|城市编码|

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
deviceType|String|设备类型|8位码，typeId
baseProperty|BaseProperty|品牌信息|见BaseProperty的定义
location|Location|位置信息|

### Module
模块信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
moduleId|String|模块ID|
moduleType|String|模块类型|
ModuleInfos|Map<String,String>|模块其他信息|属性和值的集合 


## 接口清单

### 设备类接口

> API接口总览


| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| 查询用户设备列表  | 获取用户自己绑定和别人分享的设备列表|是|  无 |  
|绑定设备 | 建立用户与设备之间的绑定关系| 是|  无 |  
| 解绑设备 | 解除用户与设备之间的绑定关系|是|  无 |  
| 修改设备别名  |更新设备别名| 是|  无 |   
|修改设备位置信息 | 修改设备经纬度、城市代码信息| 是|  无 |   
|获取设备别名 | 查询设备别名| 是|  无 |  
|获取设备位置信息  | 查询设备位置信息| 是|  无 |   
| 获取设备详细信息  | 查询设备详细信息 | 是|  无 |   
|查询设备是否在线  | 用户可以查看设备是否在线| 是|  无 |   


#### 查询用户设备列表
> 获取用户自己绑定和别人分享的设备列表。可通过authType=owner过滤获取自己绑定的设备。  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/deviceinfos`</br>
**HTTP Method：** GET

**输入参数**  

标准输入
   
|参数名|类型|位置|必填|说明 |  
| ------ |:---------:|:-----:|:--------:|:------:|  
| | | | |&emsp;|  


**输出参数:**  
  
|参数名|类型|位置|必填|说明 |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceinfos| DeviceInfo[]| Body|是 | 设备信息列表 |  


##### 2、请求示例
**用户请求**  
```
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
    Content-Encoding: utf-8
    Content-type: application/json
```
**请求应答**  
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
##### 3、错误码
> G20202

#### 绑定设备
> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=100个，绑定时设备必须平台上线。）</font> 

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST

**输入参数**  

|参数名|类型|位置|必填|说明 |  
| --------- |:--------:|:-----:|:---------:|:--------:|  
|deviceId|String|body|是|设备id |
|name|String|body|是|设备名称 |   
|bindReqSn|String|body|是|uSDK生成的随机数|  
|data|String|body|是|绑定加密数据（从sdk获取加密字符串）|  

**输出参数**    
输出标准输出参数  

##### 2、请求示例
**用户请求**
```
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
    Content-Encoding: utf-8
    Content-type: application/json
Body
{
    "deviceId": "DC330D01FBF1",
    "name": "test002",
    "data": "5f10bf4f5af08db934c8165c32140227"
}
```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}
```
##### 3、错误码
> G20202、G20904、G20908




#### 解绑设备
> 解除用户与设备之间的绑定关系

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/unbindDevice` </br>
**HTTP Method：** POST

**输入参数**  

|参数名|类型|位置|必填|说明 |   
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|是|设备id |

**输出参数**    
输出标准输出参数

##### 2、请求示例  

**用户请求**

```
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
##### 3、错误码
> G20202



#### 修改设备别名
> 更新设备别名

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/updateAliasName`</br>
**HTTP Method：** PUT

**输入参数**  

|参数名|类型|位置|必填|说明 |    
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|是|设备id|    
|aliasName|String|Body|是|设备新别名，50字符以内，不能有特殊字符|     

**输出参数**  
输出标准输出参数.  

##### 2、请求示例  

**用户请求**

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

**请求应答**

```
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码
>G20202  


#### 修改设备位置信息
> 修改设备经纬度、城市代码信息  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/updateLocation`</br>
**HTTP Method：** PUT

**输入参数**  

|参数名|类型|位置|必填|说明 |   
| --------- |:--------:|:-----:|:----------:|:--------:|  
|deviceId|String|url|是|设备id  |
|loc|Location|body|是|位置信息  |  

**输出参数**  
输出标准输出参数  

 
##### 2、请求示例

**用户请求**

```
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

**请求应答**

```
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码
> G20202  



#### 获取设备别名
> 查询设备别名s  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/aliasName`</br>
**HTTP Method：** GET

**输入参数**  

|参数名|类型|位置|必填|说明 |   
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|是|设备id|  

**输出参数**  

|参数名|类型|位置|必填|说明 |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|alisaName|String|body|是|设备别名 |


##### 2、请求示例
**用户请求**

```
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
    Content-Encoding: utf-8
    Content-type: application/json
```

**请求应答**

```
{
    "aliasName": "test002",
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、错误码
> G20202  



#### 获取设备位置信息
> 查询设备位置信息

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/location`</br>
**HTTP Method：** GET

**输入参数**  

|参数名|类型|位置|必填|说明 |  
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|是|设备id|  

**输出参数**  

|参数名|类型|位置|必填|说明 |   
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|loc|Lociation|body|是|位置信息|      


##### 2、请求示例  

**用户请求**

```
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
    Content-Encoding: utf-8
    Content-type: application/json
```

**请求应答**

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

##### 3、错误码
> G20202  




#### 获取设备详细信息 
> 查询设备详细信息 

##### 1、接口定义 

?> **接入地址：** `/uds/v1/protected/{deviceId}/deviceInfo`</br>
**HTTP Method：** GET

**输入参数**  

|参数名|类型|位置|必填|说明 |   
| --------- |:---------:|:-----:|:--------:|:------:|  
|deviceId|String|url|是|设备id  |  

**输出参数**  

|参数名|类型|位置|必填|说明 |     
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceVersion|DeviceVersion|body|是|设备信息 |   

##### 2、请求示例

**用户请求**

```
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
    Content-Encoding: utf-8
    Content-type: application/json
```

**请求应答**

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

##### 3、错误码

> G20202


#### 查询设备是否在线  
> 用户可以查看设备是否在线  

##### 1、接口定义
?> **接入地址：** `/uds/v1/protected/{deviceId}/isOnline`</br>
**HTTP Method：** GET

**输入参数**  

|参数名|类型|位置|必填|说明 |     
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|deviceId|String|url|是|设备id |  

**输出参数**  

|参数名|类型|位置|必填|说明 |   
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|  
|isOnline|String|Body|是|状态信息|  

##### 2、请求示例

**用户请求**

```
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

##### 3、错误码
> G20202  


[^-^]:文本连接注释
[DevicesStandard_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[DevicesStandard_type]:_media/_devicesStandard/DevicesStandard_type.png
[DevicesStandard_liucheng]:_media/_devicesStandard/DevicesStandard_liucheng.png
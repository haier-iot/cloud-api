# 设备管理-三方云

## 简介  

> 为了方便第三方接入海尔的设备,提供接口说明。


## 刷新设备列表接口  

**使用说明**

> 刷新设备列表

**接口描述**   

?> **接入地址：** `/dcs/third-party-cloud/add/user/third-party/device`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
accessToken|String|Header|必填|安全令牌 token，30位字符。  
systemId|String|Header|必填||    


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|List<DeviceInfoVO>|Body|必填|返回数据


**DeviceInfoVO**

| **名称** | 绑定设备信息 |&emsp;| DeviceInfoVO |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|deviceName| String | 设备名称，等同于别名 ||  
|deviceId| String | 设备ID ||  
|wifiType| String | 设备wifitype ||
|deviceType| String | 设备类别 ||
|totalPermission| AuthInfo | 权限和，权限信息的综合 ||
|permissions| Permission[] | 权限信息 ||
|online| Boolean | 是否在线 ||
|productNameT| String | 产品型号名称 ||
|productCodeT| String | 产品型号编码 ||
|deviceRole| String | 设备角色：1 普通设备;2 网关设备;3 附件设备;4 子设备;仅返回数字，角色信息不返回 ||
|parentDeviceId| String | 主设备Id：若该设备本身为主设备，该字段为null；若该设备为从设备，该字段为主设备Id ||
|deviceRoleType| String | 设备类别：三种：主设备；从设备；无 ||
|slaveDeviceIds| List<String> | 所属从设备Id的集合：若无从设备该字段为空 ||
|brand| String | 设备品牌 ||
|brandCode| String | 品牌编码 |&emsp;|    


**AuthInfo**

| **名称** | 权限内容信息，至少一项为true |&emsp;| AuthInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|view| Boolean | 是否有查看权 ||  
|set| Boolean | 是否有配置权限 ||  
|control| Boolean | 是否有控制权 |&emsp;|  


**Permission**

| **名称** | 权限信息 |&emsp;| Permission |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|auth| AuthInfo | 权限内容 ||  
|authType| String | 权限类型 | home:家庭分享，share：个人分享，owner：设备主人，server：给appserver的权限 |  

**示例**

**请求样例**

```
POST http://uws.haier.net /dcs/third-party-cloud/add/user/third-party/device

POST data:
[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:c6533d557d12f98276a9cc3b0b0bca31bdf30ea4ad82e879fb3d445c3a70ee8a
	timestamp: 1590053691484 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 0
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
  "payload": [{
	            "deviceId":"DC330D23CC5E",
	            "deviceImg":"http://file.c.haier.net/images/2019/04/30/2786b13db59481c6e52f9b690f406d4f.png",
	            "deviceName":"空气魔方CC5E",
	            "deviceType":"33001001",
	            "online":"false",
	            "permissions":[
	                {
	                    "auth":{"control":true,"set":true,"view":true},
                        "authType":"owner"
	                }
	            ],
	            "productCodeT":"AQ0055M6N",
	            "productNameT":"XG-150QH/AA恒氧新风机",
	            "totalPermission":{"control":true,"set":true,"view":true},          "wifiType":"111c120024000810330200118999990000000000000000000000000000000000"
              },
              {
	            "deviceId":"DC330D861B7C",
	            "deviceImg":"http://file.c.haier.net/images/2017/09/13/40bcac3fbb72a2cd002122367e5ec7d5.jpg",
	            "deviceName":"我的设备1B7C",
	            "deviceType":"03001001",
	            "online":"false",
	            "permissions":[
	                {
	                    "auth":{"control":true,"set":true,"view":true},
	                    "authType":"owner"
	                }
	            ],
	            "productCodeT":"AJ0261M07",
	            "productNameT":"除湿",
	             "totalPermission":{"control":true,"set":true,"view":true},      "wifiType":"200861080082032421059278f9beccb244ad62bd7dd223670db151b71678bc40"
               }
             ],
  "retCode":"00000",
  "retInfo":"成功"
}

```


## 查询设备详情接口  

**使用说明**

> 查询单个设备的详情(需要uag校验，并且增加token校验)

**接口描述**  

?> **接入地址：** `/dcs/third-party-cloud/get/device/detail`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|| 
systemId|String|Header|必填|| 
accessToken|String|Header|必填||    


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|DeviceVersion|Body|必填|返回数据


**DeviceVersion**

| **名称** | 设备详细信息 |&emsp;| DeviceVersion |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|deviceId| String | 设备ID ||  
|modules| Set<Module> | 模块信息 ||  
|wifiType| String | wifi类型 ||
|deviceType| String | 设备类型 |&emsp;|  


**Module**

| **名称** | 模块信息 |&emsp;| Module |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|moduleId| String | 模块ID ||  
|moduleType| String | 模块类型 ||  
|moduleInfos| Map<String,String> | 模块其他信息|&emsp;|  

**示例**

**请求样例**


```
POST http://uws.haier.net/dcs/third-party-cloud/get/device/detail

POST data:
	{
	  "deviceId": "DC330D861B7C"
	}
[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
  "payload":{
              "baseProperty":{"brand":"紫水晶","model":"三厢对开门","others":{}},
              "deviceId":"DC330D861B7C",
              "deviceType":"03001001",
              "module":[{
                           "moduleId":"",
                           "moduleInfos":{"$ref":"$.payload.baseProperty.others"},
                           "moduleType":"basemodule"
                         },
                         {
                           "moduleId":"DC330D861B7C",
                           "moduleInfos":{"$ref":"$.payload.baseProperty.others"},
                           "moduleType":"wifimodule"
                         }
                        ],
              "wifiType":"200861080082032421059278f9beccb244ad62bd7dd223670db151b71678bc40"
            },    
  "retCode":"00000",
  "retInfo":"成功"
}

```


## 查询设备状态接口  

**使用说明**

> 查询单个设备的状态(需要uag校验，并且增加token校验)

**接口描述**

?> **接入地址：** `/dcs/third-party-cloud/get/device/status`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|| 
part|String|Body|必填|过滤参数；查询结果一定返回shadowBaseInfo部分；通过过滤参数控制返回其他部分；等于1时，其他只返回reported部分；等于2时，其他只返回desired部分；等于0时，其他部分全部返回；默认值为0；   


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|ShadowBeanVO|Body|必填|返回数据


**ShadowBeanVO**

| **名称** | 设备影子信息 |&emsp;| ShadowBeanVO |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|shadowInfo| ShadowInfo | 影子基本信息 | 只读  
|deviceInfo| DeviceInfo | 设备基本信息 | 只读（可为空，可以不含此项） 
|preChangeReported| Map<String,String> | 设备上报状态的前一个变化版本，key是设备上报状态的属性名，value是上报的状态属性值 | 只读（可为空，可以不含此项）
|reported| Map<String,String> | 设备上报的状态，key是设备上报状态的属性名，value是上报的状态属性值 | 只读（可为空，可以不含此项）
|desired| Map<String,String> | 设备的预期操作和预期状态，key是设备预期的操作名或设备预期状态的属性名，value是设备预期操作的json的字符串形式或设备预期状态的属性值 | 只读（可为空，可以不含此项）
|events| Map<String,String> | 设备上报的事件，key是事件标示，value是事件内容 | 只读（可为空，可以不含此项）
|metadata| Metadata | metadata部分 | 只读（可为空，可以不含此项）  


**ShadowInfo**

| **名称** | 设备影子基础内容信息 |&emsp;| ShadowInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|shadowVersion| String | 设备影子模型版本号 | 只读
|reportedTimestamp| long | 缓存时间戳：影子最后更新时间戳，只有设备状态发生变化及新增事件通知才更新该时间戳；新增设备下发指令不更新该时间戳； | 只读（可为空，可以不含此项）
|eventTimestamp| long | 缓存时间戳：影子最后告警时间戳，只有设备有告警上报才更新该时间戳； | 只读（可为空，可以不含此项）
|onoffTimestamp| long | 缓存时间戳：影子最后上下线时间戳，只有设备上线或者下线才更新该时间戳； | 只读（可为空，可以不含此项）
|isOnline| boolean | 设备在线状态，在线/离线 | 只读（可为空，可以不含此项）
|netQuality| NetQualityInfo | 设备网络质量 | 只读（可为空，可以不含此项）
|ssid| String | 路由器热点名称：wifi设备为连接路由器的ssid | 只读（可为空，可以不含此项）


**DeviceInfo**

| **名称** | 设备软件信息 |&emsp;| DeviceInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|typeid| String | typeid：设备上报 | 只读
|deviceType| String | 型号：业务进行调用接口存储 | 非必填，（可为空，可以不含此项）
|softVersion| String | 模块软件版本号（海尔wifi模块） | 只读（可为空，可以不含此项）
|hardVersion| String | 模块硬件版本号（海尔WIFI模块） | 只读（可为空，可以不含此项）
|connectType| String | 设备连网类型：蓝牙、zigbee、wifi等 | 只读（可为空，可以不含此项）
|timeVersion| String | 时间版本号：格式如：2018032608，年月日小时格式 | 只读（可为空，可以不含此项）


**NetQualityInfo**

| **名称** | 设备联网的网络质量信息 |&emsp;| NetQualityInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|netQuality| String | 网络信号强度 | 只读（可为空，可以不含此项）
|timeDelayOfAP| double | 4次ping AP的平均延时，单位ms，如果4次ping都超时，值为-1，代表100%丢包 | 只读（可为空，可以不含此项）
|packetLossOfAP| double | 4次ping AP的丢包数 | 只读（可为空，可以不含此项）
|timeDelayOfGateway| double | ping 网关/心跳时延，单位ms，如果ping超时，值为-1 | 只读（可为空，可以不含此项）
|packetLossOfGateway| int | ping 网关/心跳是否丢失，1代表丢失，0代表未丢失 | 只读（可为空，可以不含此项）
|timestamp| long | 平台收到设备上报的网络质量数据的时间戳 | 只读（可为空，可以不含此项）


**MetadataItem**

| **名称** | MetadataItem |&emsp;| MetadataItem |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|timestamp| long | previousReported、reported、desired和 events部分中每个属性的时间戳，因此，可以确定状态的更新时间 | 只读


**Metadata**

| **名称** | Metadata |&emsp;| Metadata |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|preChangeReported| Map<String, MetadataItem > | previousReported部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备上报状态的属性名 | 只读（可为空，可以不含此项）
|reported| Map<String, MetadataItem > | reported部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备上报状态的属性名 | 只读（可为空，可以不含此项）
|desired| Map<String, MetadataItem > | desired部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备预期的操作名或设备预期状态的属性名 | 只读（可为空，可以不含此项）
|events| Map<String, MetadataItem > | events部分中每个属性的时间戳，因此，key确定状态的更新时间，可以是事件标示 | 只读（可为空，可以不含此项）

**示例**

**请求样例**

```
POST http://uws.haier.net/dcs/third-party-cloud/get/device/status

POST data:
	{
	  "deviceId": "DC330D23CC5E",
	  "part":0
	}

[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:031a0ca6324cf7e5c5eda75c1bc08dbd557d856c9c8723e15382f03762b0f770
	timestamp: 1590053937813 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 47
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```

{
    "payload": {
        "deviceInfo": {
            "hardVersion": "1.0.00", 
            "softVersion": "2.6.02", 
            "typeid": "111c120024000810330200118999990000000000000000000000000000000000"
        }, 
        "events": {
            "tempHumidSensorCommunicationErr": "tempHumidSensorCommunicationErr"
        }, 
        "metadata": {
            "events": {
                "tempHumidSensorCommunicationErr": {
                    "timestamp": 1589176701374
                }
            }, 
            "preChangeReported": {
                "mode": {
                    "timestamp": 1589170072569
                }, 
                "timingMode": {
                    "timestamp": 1560480244564
                }, 
                "envTemperature": {
                    "timestamp": 1560428330633
                }, 
                "forceDelete": {
                    "timestamp": 1560504894742
                }, 
                "timingHH": {
                    "timestamp": 1560480244564
                }, 
                "windSpeed": {
                    "timestamp": 1589170072569
                }, 
                "onOffStatus": {
                    "timestamp": 1589176590234
                }
            }, 
            "reported": {
                "envTemperature": {
                    "timestamp": 1560504894742
                }, 
                "pm2p5Level": {
                    "timestamp": 1589176665519
                }, 
                "greenChilledStatus": {
                    "timestamp": 1560504894742
                }, 
                "echoStatus": {
                    "timestamp": 1589176665519
                }, 
                "anionStatus": {
                    "timestamp": 1589176665519
                }, 
                "rgbBlue": {
                    "timestamp": 1560504894742
                }, 
                "lightStatus": {
                    "timestamp": 1589176665519
                }, 
                "indoorHumidity": {
                    "timestamp": 1589176665519
                }, 
                "partyStatus": {
                    "timestamp": 1560504894742
                }, 
                "mode": {
                    "timestamp": 1589176665519
                }, 
                "softFreezeStatus": {
                    "timestamp": 1560504894742
                }, 
                "runningStatus": {
                    "timestamp": 1589176665519
                }, 
                "envHumidity": {
                    "timestamp": 1560428330633
                }, 
                "indoorTemperature": {
                    "timestamp": 1589176665519
                }, 
                "springFestivalStatus": {
                    "timestamp": 1560504894742
                }, 
                "windSpeed": {
                    "timestamp": 1589176665519
                }, 
                "sleepMode": {
                    "timestamp": 1589176665519
                }, 
                "targetHumidity": {
                    "timestamp": 1589176665519
                }, 
                "timingMode": {
                    "timestamp": 1589176665519
                }, 
                "dataBackup1": {
                    "timestamp": 1560504894742
                }, 
                "dataBackup2": {
                    "timestamp": 1560504894742
                }, 
                "dataBackup3": {
                    "timestamp": 1560504894742
                }, 
                "freezerTemperatureC": {
                    "timestamp": 1560504894742
                }, 
                "refIcyStatus": {
                    "timestamp": 1560504894742
                }, 
                "dataBackup4": {
                    "timestamp": 1560504894742
                }, 
                "dataBackup5": {
                    "timestamp": 1560504894742
                }, 
                "compressorStatus": {
                    "timestamp": 1589176665519
                }, 
                "dataBackup6": {
                    "timestamp": 1560504894742
                }, 
                "dataBackup7": {
                    "timestamp": 1560504894742
                }, 
                "dehumidModuleExist": {
                    "timestamp": 1589176665519
                }, 
                "humidModuleExist": {
                    "timestamp": 1589176665519
                }, 
                "volume": {
                    "timestamp": 1560504894742
                }, 
                "vocLevel": {
                    "timestamp": 1589176665519
                }, 
                "cleaningModuleExist": {
                    "timestamp": 1589176665519
                }, 
                "refrigeratorTargetTempLevel": {
                    "timestamp": 1560504894742
                }, 
                "quickFreezingMode": {
                    "timestamp": 1560504894742
                }, 
                "timingHH": {
                    "timestamp": 1589176665519
                }, 
                "quickRefrigeratingMode": {
                    "timestamp": 1560504894742
                }, 
                "freezerStatus": {
                    "timestamp": 1560504894742
                }, 
                "refrigeratorDoorStatus": {
                    "timestamp": 1560504894742
                }, 
                "tone": {
                    "timestamp": 1560504894742
                }, 
                "freezerDoorStatus": {
                    "timestamp": 1560504894742
                }, 
                "fridgeStatus": {
                    "timestamp": 1560504894742
                }, 
                "indoorPM2p5Value": {
                    "timestamp": 1589176665519
                }, 
                "forceDelete": {
                    "timestamp": 1589176665519
                }, 
                "lockStatus": {
                    "timestamp": 1589176665519
                }, 
                "onOffStatus": {
                    "timestamp": 1589176665519
                }, 
                "rgbRed": {
                    "timestamp": 1560504894742
                }, 
                "freezerTargetTempLevel": {
                    "timestamp": 1560504894742
                }, 
                "rgbGreen": {
                    "timestamp": 1560504894742
                }, 
                "zeroFreshStatus": {
                    "timestamp": 1560504894742
                }, 
                "valentineStatus": {
                    "timestamp": 1560504894742
                }, 
                "refrigeratorTemperatureC": {
                    "timestamp": 1560504894742
                }
            }
        }, 
        "preChangeReported": {
            "mode": "1", 
            "timingMode": "2", 
            "envTemperature": "-8", 
            "forceDelete": "true", 
            "timingHH": "2", 
            "windSpeed": "1", 
            "onOffStatus": "false"
        }, 
        "reported": {
            "envTemperature": "-11", 
            "pm2p5Level": "1", 
            "greenChilledStatus": "true", 
            "echoStatus": "false", 
            "anionStatus": "false", 
            "rgbBlue": "8", 
            "lightStatus": "false", 
            "indoorHumidity": "55", 
            "partyStatus": "true", 
            "mode": "7", 
            "softFreezeStatus": "false", 
            "runningStatus": "1", 
            "envHumidity": "12", 
            "indoorTemperature": "25", 
            "springFestivalStatus": "false", 
            "windSpeed": "4", 
            "sleepMode": "false", 
            "targetHumidity": "0", 
            "timingMode": "0", 
            "dataBackup1": "1", 
            "dataBackup2": "0", 
            "dataBackup3": "0", 
            "freezerTemperatureC": "-4", 
            "refIcyStatus": "false", 
            "dataBackup4": "0", 
            "dataBackup5": "0", 
            "compressorStatus": "false", 
            "dataBackup6": "0", 
            "dataBackup7": "0", 
            "dehumidModuleExist": "true", 
            "humidModuleExist": "true", 
            "volume": "1", 
            "vocLevel": "1", 
            "cleaningModuleExist": "true", 
            "refrigeratorTargetTempLevel": "", 
            "quickFreezingMode": "false", 
            "timingHH": "0", 
            "quickRefrigeratingMode": "false", 
            "freezerStatus": "true", 
            "refrigeratorDoorStatus": "false", 
            "tone": "", 
            "freezerDoorStatus": "false", 
            "fridgeStatus": "false", 
            "indoorPM2p5Value": "20", 
            "forceDelete": "false", 
            "lockStatus": "false", 
            "onOffStatus": "true", 
            "rgbRed": "18", 
            "freezerTargetTempLevel": "31", 
            "rgbGreen": "2", 
            "zeroFreshStatus": "false", 
            "valentineStatus": "false", 
            "refrigeratorTemperatureC": "158"
        }, 
        "shadowInfo": {
            "flag": 1, 
            "online": false, 
            "onoffTimestamp": 1589178060009, 
            "reportedTimestamp": 1589176701374, 
            "shadowVersion": "101"
        }
    }, 
    "retCode": "00000", 
    "retInfo": "成功"
}

```


## 设备控制接口  

**使用说明**

> 控制设备(需要uag校验，并且增加token校验)

**接口描述**

?> **接入地址：** `/dcs/third-party-cloud/update/device/cmd`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|必填|| 
accessToken|String|Header|必填|| 
deviceId|String|Body|必填||
cmdName|String|Body|非必填| 组命令id（1.若该操作为组命令操作，则该值必填。2.若该操作为单命令操作，则该值不需要传递）
cmdArgs|Map<String,String>|Body|必填| 一组命令,即属性集合（key-value）。（若该操作为单命令操作，则该值必须只有一对key-value。）
callbackUrl|String|Body|非必填| 操作应答回调地址,只支持http协议    


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|String|Body|必填|当次请求的sn


**示例**

**请求样例**

```
POST http://uws.haier.net/dcs/third-party-cloud/update/device/cmd

POST data:
	{
	  "callbackUrl": "string",
	  "cmdArgs": {
	    "onOffStatus": "false"
	  },
	  "cmdName": "",
	  "deviceId": "DC330D23CC5E"
	}

[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:579c04727f5d922bd24669cd432746be0a5f8be20acdb274e27fe5c2da309192
	timestamp: 1590054016960 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 130
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
	"retCode":"00000",
	"retInfo":"成功"，
	"usn":"1234343534534"
}

```


## 取消单个设备的授权  

**使用说明**

> 取消单个设备的授权(需要uag校验，并且增加token校验)

**接口描述**

?> **接入地址：** `/dcs/third-party-cloud/delete/user/third-party/device`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|| 
accessToken|String|Header|必填|| 
systemId|String|Header|必填||


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Boolean|Body|必填|返回数据


**示例**

**请求样例**


```
POST http://uws.haier.net/dcs/third-party-cloud/delete/user/third-party/device

POST data:
	{
	  "deviceId": "DC330D23CC5E1"
	}

[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:c5a0a2c3118aa937dc080274d17bfe389f6b13d62b276f77e52af6babef327e6
	timestamp: 1590054583156 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 35
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
	"retCode":"00000",
	"retInfo":"成功"，
	"payload":true
}

```


## 查询第三方开通预授权的型号列表  

**使用说明**

> 查询第三方开通预授权的型号列表(需要uag校验，但是无需token校验)

**接口描述**

?> **接入地址：** `/dcs/third-party-cloud/get/third-party/device-models`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|必填|| 


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|Body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|List<DeviceModel>|Body|必填|返回数据


**DeviceModel**

| **名称** | 型号基本信息 |&emsp;| DeviceModel |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|modelCode| String | 型号名称 || 
|appTypeName| String | 产品类型 ||
|appTypeCode| String | 产品类型编码 ||
|bigTypeCode| String | 大类 ||
|midTypeCode| String | 中类 ||
|typeId| String | typeId ||
|productCode| String | 成品编码 ||
|productImg1| String | 实物图片1 ||

**示例**

**请求样例**

```
POST http://uws.haier.net/dcs/third-party-cloud/get/third-party/device-models

POST data:

[no cookies]

Request Headers:
	Connection: keep-alive
	systemId: SV-SBZXFWFWPTQ656-0000
	appVersion: 01.00.00.00000
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT3RALJTMOTE3MH28K9FZDX1QNZA0
	sign:1b5396e398fc7e16e3323c197481b894a70f19492b8a8170e52568c04b0779c7
	timestamp: 1590054123585 
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	identification: tuya
	Content-Length: 0
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
    "payload": [
        {
            "appTypeCode": "A069", 
            "appTypeName": "新风机", 
            "bigTypeCode": "24", 
            "midTypeCode": "24011", 
            "modelCode": "XG-150QH/AA恒氧新风机", 
            "productCode": "AQ0055M6N", 
            "productImg1": "http://file.c.haier.net/images/2019/04/30/2786b13db59481c6e52f9b690f406d4f.png", 
            "typeId": "111c120024000810330200118999990000000000000000000000000000000000"
        }, 
        {
            "productCode": "DE12A", 
            "typeId": "200861080082032421059278f9beccb244ad62bd7dd223670db151b71678bc40"
        }, 
        {
            "appTypeCode": "A006", 
            "appTypeName": "除湿机", 
            "bigTypeCode": "21", 
            "midTypeCode": "21005", 
            "modelCode": "DE12A除湿机", 
            "productCode": "AJ0261M07", 
            "productImg1": "http://file.c.haier.net/images/2017/09/13/40bcac3fbb72a2cd002122367e5ec7d5.jpg", 
            "typeId": "200861080082032421059278f9beccb244ad62bd7dd223670db151b71678bc40"
        }
    ], 
    "retCode": "00000", 
    "retInfo": "成功"
}

```

>  **当前版本**：[UWS 设备影子服务 V1.0.0](zh-cn/ChangeLog/DevicesShadow)  
 **更新时间**：{docsify-updated} 

## 简介  

> 设备影子对外提供设备的标准模型数据，提供设备在云端的影子（缓存）。

为Haier U+云平台提供用户以服务端的方式对云端设备影子进行命令下发、读写操作等相关管理企业版服务的指导说明。作为第三方应用程序开发的依据和输入。提供设备影子查询服务和设备功能描述查询服务。


## 应用场景

由于网络的不稳定，导致设备频繁的上下线。设备影子可以很好解决，影子会存储设备最新状态，当设备状态发生变化，设备就会把状态同步到影子。这样应用程序在请求设备当前状态时，只需要获取影子中的状态即可，不需要关心设备是否在线，随时随地请求。  
  
  

在设备网络稳定的情况下，当有很多应用程序请求设备状态时，设备需要响应多次或重复相应多个相同的请求，势必加大了设备的负载。设备影子可以很好解决，设备只需要主动同步状态一次给设备影子，然后多个应用程序只需要请求设备影子即可获取设备最新状态，做到了应用程序和设备的解耦，设备的能力得到了解放。
  

  
假设设备网络不稳定，设备频繁的上下线。当应用程序发送控制指令给设备时，设备无法正常接收指令。。设备影子可以很好解决，应用程序不需要关注设备是否在线，只需要发送指令，指令会带着时间戳保存在设备影子，当设备掉线重连时就获取指令，并根据时间戳来确定是否执行，当设备判断指令过期可以选择不执行指令。  




## 公共结构  

### NetQualityInfo  


| **名称** | 设备联网的网络质量信息 |&emsp;| NetQualityInfo |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|netQuality| String | 网络信号强度 |只读（可为空，可以不含此项）|  
|timeDelayOfAP| double | 4次ping AP的平均延时，单位ms，如果4次ping都超时，值为-1，代表100%丢包 |只读（可为空，可以不含此项）|  
|packetLossOfAP| double | 4次ping AP的丢包数 |只读（可为空，可以不含此项）|    
|timeDelayOfGateway| double | ping 网关/心跳时延，单位ms，如果ping超时，值为-1 |只读（可为空，可以不含此项）|  
|packetLossOfGateway| int | ping 网关/心跳是否丢失，1代表丢失，0代表未丢失 |只读（可为空，可以不含此项）|  
|timestamp| long | 平台收到设备上报的网络质量数据的时间戳 |只读（可为空，可以不含此项）|  

### ShadowInfo  
  
| **名称** | 设备影子基础内容信息 |&emsp;| ShadowInfo |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|shadowVersion| String | 设备影子模型版本号 |只读|  
|reportedTimestamp| long | 缓存时间戳：影子最后更新时间戳，只有设备状态发生变化及新增事件通知才更新该时间戳；新增设备下发指令不更新该时间戳； |只读（可为空，可以不含此项）|  
|onoffTimestamp| long | 缓存时间戳：影子最后上下线时间戳，只有设备上线或者下线才更新该时间戳；  |只读（可为空，可以不含此项）|    
|online| boolean | 设备在线状态，在线/离线  |只读（可为空，可以不含此项）|   
|netQuality| NetQualityInfo | 设备网络质量  |只读（可为空，可以不含此项）|   
|ssid| String | 路由器热点名称：wifi设备为连接路由器的ssid  |只读（可为空，可以不含此项）|   

### DeviceInfo  
  
| **名称** | 设备软件信息 |&emsp;| DeviceInfo |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|typeid| String | typeid：设备上报 |只读|  
|deviceType| String | 型号：业务进行调用接口存储 |非必填，（可为空，可以不含此项）|  
|softVersion| String | 模块软件版本号（海尔wifi模块） |只读（可为空，可以不含此项）|    
|hardVersion| String | 模块硬件版本号（海尔WIFI模块） |只读（可为空，可以不含此项）|   
|connectType| String | 设备连网类型：蓝牙、zigbee、wifi等  |只读（可为空，可以不含此项）|   
|timeVersion| String | 时间版本号：格式如：2018032608，年月日小时格式|只读（可为空，可以不含此项）|   


### MetadataItem  
  
| **名称** | MetadataItem |&emsp;| MetadataItem |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|timestamp| long | previousReported、reported、desired和 events部分中每个属性的时间戳，因此，可以确定状态的更新时间 |只读|  


### Metadata  
  
| **名称** | Metadata |&emsp;| Metadata |   
| :----------:|:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|preChangeReported| Map<String, MetadataItem > | previousReported部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备上报状态的属性名 |只读, （可为空，可以不含此项）|  
|reported| Map<String, MetadataItem > | reported部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备上报状态的属性名 |只读, （可为空，可以不含此项）|  
|desired| Map<String, MetadataItem > |desired部分中每个属性的时间戳，因此，可以确定状态的更新时间，key是设备预期的操作名或设备预期状态的属性名 |只读（可为空，可以不含此项）|    
|events| Map<String, MetadataItem > | events部分中每个属性的时间戳，因此，key确定状态的更新时间，可以是事件标示|只读（可为空，可以不含此项）|   


### Shadow  
  
| **名称** | 设备影子信息 |&emsp;| Shadow |   
|:----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|shadowInfo| ShadowInfo | 影子基本信息 |只读|  
|deviceInfo| DeviceInfo | 设备基本信息 |只读（可为空，可以不含此项）|  
|preChangeReported| Map<String,String> |设备上报状态的前一个变化版本，key是设备上报状态的属性名，value是上报的状态属性值 |只读（可为空，可以不含此项）|    
|reported| Map<String,String> |设备上报的状态，key是设备上报状态的属性名，value是上报的状态属性值 |只读（可为空，可以不含此项）|   
|desired|Map<String,String> | 设备的预期操作和预期状态，key是设备预期的操作名或设备预期状态的属性名，value是设备预期操作的json的字符串形式或设备预期状态的属性值 |只读（可为空，可以不含此项）|   
|events| Map<String,String> | 设备上报的事件，key是事件标示，value是事件内容|只读（可为空，可以不含此项）|   
|metadata|Metadata | metadata部分|只读（可为空，可以不含此项）|

### ReportedModel  
  
| **名称** | 设备上报状态标准模型信息 |&emsp;| ReportedModel |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|name| String | 设备上报状态属性名称 |只读|  
|desc| String |设备上报状态属性的中文描述 |只读|  
|valueRangeType| String | 标准模型设备状态属性值范围类型 |只读|    


### DesiredModel  
  
| **名称** | 设备预期状态标准模型信息 |&emsp;| DesiredModel |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|name| String | 设备预期的预期操作名称或预期状态属性名称；type=1时，是预期操作名称；type=2时是预期状态属性名称 |只读|  
|desc| String | 设备预期的预期操作中文描述或预期状态属性的中文描述；type=1时，是预期操作中文描述；type=2时是预期状态属性中文描述 |只读|  
|valueRangeType| String | 标准模型设备状态属性值范围类型 |只读；type=2时，不为空；其他值时，可为空，可以不含此项|    
|valueRange| jsonObject | 标准模型设备状态属性值范围描述json参考《设备功能逻辑描述文件格式说明 》 |只读；type=2时，不为空；其他值时，可为空，可以不含此项|   
|attrNameList| List<String> | 预期操作涉及的设备状态属性名称列表 |只读；type=1时，不为空；其他值时，可为空，可以不含此项|   
|operationType| String | 设备预期的属性的名称或设备预期的指令的操作命令类型|只读|   
|type| int | 预期状态类型，1为预期操作；2为预期状态|只读|   


### AlarmsModel  
  
| **名称** | 设备告警标准模型信息 |&emsp;| AlarmsModel |   
| :----------:|:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|name| String | 事件标识；默认设备告警码 |只读|  
|desc| String | 事件描述；默认设备告警描述 |只读|  
|clear| boolean |事件清除标记；默认设备告警清除标记 |只读|    
|type| String | 事件类型 |只读|   


### ShadowModel  
  
| **名称** | 设备影子的模型信息 |&emsp;| ShadowModel |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|deviceInfo| DeviceInfo | 设备基本信息（只有typeid） |只读|  
|reported| Map<String, ReportedModel > | 设备上报状态标准模型信息 |只读|  
|desired| Map<String, DesiredModel > |设备预期标准模型信息|只读|    
|events| Map<String, AlarmsModel > | 设备告警标准模型信息 |&emsp;|   

## 接口清单  
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|  
| :-------------: |:-------------:|:-----:|:-------------:|
| 设备影子查询 | 按指定的设备id，查询该设备的云端影子信息，验证token合法性，验证token所属用户与待查询设备存在查看和操作权限| 是| 无| 
| 设备功能描述查询|按设备影子模块分类返回设备标准模型，验证token合法性| 是| 无| 


### 设备影子查询
> 按指定的设备id，查询该设备的云端影子信息，验证token合法性，验证token所属用户与待查询设备存在查看和操作权限  

##### 1、接口定义
?> **接入地 址：**  `/shadow /v1/info `  
 **HTTP Method：** POST

**输入参数**  

| 类型   | 参数名   | 位置  | 必填|说明|  
| :-----:|:------:|:-----:|:---:|:----:|   
|  deviceId    | String | Body| 必填|设备id|  
|  part    | int | Body| 选填|过滤参数；查询结果一定返回shadowBaseInfo部分；</br>通过过滤参数控制返回其他部分；</br>等于1时，其他只返回reported部分；</br>等于2时，其他只返回desired部分；</br>等于0时，其他部分全部返回；默认值为0；|  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| :-------------:|:----------:|:-----:|:--------:|:---------:|
| shadowInfo |  ShadowInfo  |   Body  |  必填  | 查询到的设备影子信息 |

##### 2、请求样例  

**用户请求**
```java  
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
	"deviceId": "123"
}

```  

**请求应答**

```java
{
    "retCode": "00000",
"retInfo": "成功",
"shadowInfo": {
	"shadowInfo": {
		"shadowVersion": "20160101",
		"reportedTimestamp": 123456,
		"online": true
	},
	"deviceInfo": {
		"typeid": "string2",
		"deviceType": "string2",
		"softVersion": "string2",
		"hardVersion": "string2",
		"connectType": "string2",
		"timeVersion": "string2"
	},
	"preChangeReported": {
		"attribute1": "string2",
		"attribute2": "string1",
		"attributeN": "string2"
	},
	"reported": {
		"attribute1": "string2",
		"attribute2": "string1",
		"attributeN": "string2"
	},
	"desired": {
		"attribute1": "string2",
		"attribute2": "string2",
		"optname1": "{\"attr1\":\"val1\",\"attr2\":\"val2\",\"attr3\":123}"
	},
	"events": {
		"code1": "value1",
		"code2": "value2"
	},
	"metadata": {
		"preChangeReported": {
			"attribute1": {
				"timestamp": 123456
			},
			"attribute2": {
				"timestamp": 123456
			},
			"attributeN": {
				"timestamp": 123456
			}
		},
		"reported": {
			"attribute1": {
				"timestamp": 123456
			},
			"attribute2": {
				"timestamp": 123456
			},
			"attributeN": {
				"timestamp": 123456
			}
		},
		"desired": {
			"attribute1": {
				"timestamp": 123456
			},
			"attribute2": {
				"timestamp": 123456
			},
			"attributeN": {
				"timestamp": 123456
			}
		},
		"events": {
			"code1": {
				"timestamp": 123456
			},
			"code2": {
				"timestamp": 123456
			}
		}
	}
}
}

```

##### 3、错误码  
> A00003、A00004、B00001、C00003、C00004、Y00001  

### 设备功能描述查询
> 按设备影子模块分类返回设备标准模型，验证token合法性  

##### 1、接口定义
?> **接入地 址：**  `/shadow /v1/model`  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名     | 位置  | 必填|说明|  
| :----: |:------:|:----:|:------:|:------:|    
|  typeid | String   | Body  | 必填 | 设备typeid|  
  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| :-------------: |:----------:|:-----:|:--------:|:---------:|
| shadowModel |  ShadowModel  |   Body  |  必填  | 查询到的设备影子模型信息 |

##### 2、请求样例  

**用户请求**
```java  
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
Body:
{
	"typeid": "123"
}


```  

**请求应答**

```java
{
    "retCode": "00000",
"retInfo": "成功",
"shadowModel": {
  "deviceInfo": {
		"typeid": "20160101"
  },
  "reported": [
    {
      "name": "getAllProperty",
      "desc": "查询状态",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "getAllProperty"
          }
        ]
      },
	  "operationType": "I"
    },
    {
      "name": "getAllAlarm",
      "desc": "查询报警",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "getAllAlarm"
          }
        ]
      },
      "operationType": "I"
    },
    {
      "name": "stopCurrentAlarm",
      "desc": "停止报警",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "stopCurrentAlarm"
          }
        ]
      },
      "operationType": "I"
    },
     {
      "name": "currentTemperature",
      "desc": "当前温度",
	  "valueRangeType": "STEP",
      "valueRange": {
        "type": "STEP",
        "dataStep": {
          "dataType": "Integer",
          "step": "1",
          "minValue": "1",
          "maxValue": "75"
        }
      }
    },
    {
      "name": "powerSettingSupported",
      "desc": "是否支持功率设置",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "true",
            "desc": "支持"
          },
          {
            "data": "false",
            "desc": "不支持"
          }
         ]
      }
    },
    {
      "name": "resnSettingSupported",
      "desc": "是否支持预约",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "true",
            "desc": "支持"
          },
          {
            "data": "false",
            "desc": "不支持"
          }
         ]
      }
    }
  ],
  "desired": [
    {
      "name": "getAllProperty",
      "desc": "查询状态",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "getAllProperty"
          }
        ]
      },
      "operationType": "I",
	  "type": "2"
    },
    {
      "name": "getAllAlarm",
      "desc": "查询报警",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "getAllAlarm"
          }
        ]
      },
      "operationType": "I",
	  "type": "2"
    },
    {
      "name": "stopCurrentAlarm",
      "desc": "停止报警",
	  "valueRangeType": "LIST",
      "valueRange": {
        "type": "LIST",
        "dataList": [
          {
            "data": "stopCurrentAlarm"
          }
        ]
      },
      "operationType": "I",
	  "type": "2"
    },
	{
      "name": "grSetResn1",
      "desc": "设置预约1",
      "attrNameList": [
        "resn1TimeHH",
        "resn1TimeMM",
        "resn1Temperature",
        "resn1RunningStatus",
        "resn1CycleStatus"
      ],
	  "operationType": "groupCommand",
	  "type": "1"
    },
    {
      "name": "grSetResn2",
      "desc": "设置预约2",
      "attrNameList": [
        "resn2TimeHH",
        "resn2TimeMM",
        "resn2Temperature",
        "resn2RunningStatus",
        "resn2CycleStatus"
      ],
	  "operationType": "groupCommand",
	  "type": "1"
    }
  ],
  "events": [
    {
      "name": "alarmCancel",
      "desc": "报警解除",
      "clear": true,
	  "type":"alarms"
    },
    {
      "name": "middleTempSensorErr",
      "desc": "水箱温度中传感器故障报警",
	  "type":"alarms"
    },
    {
      "name": "dryHeatingAlarm",
      "desc": "干烧超温报警",
	  "type":"alarms"
    },
    {
      "name": "fireWallTempSensorErr",
      "desc": "防火墙温度传感器故障",
	  "type":"alarms"
    }
  ]
}
}


```

##### 3、错误码  
> A00003、A00004、B00001、C00003、C00004、Y00001







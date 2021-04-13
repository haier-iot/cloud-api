#  消息发送


## 终端功能接口列表

?>  使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>
访问地址：`https://uws.haier.net/ums/v3`


### 按设备推送消息

> 从用户的某个终端推送消息到该用户的多个终端，消息的发送和接收终端都同属该用户（可以是不同APP）

#### 1、接口定义

?> **接入地址：** `/msg/pushByClients`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
toClients|Set<TerminalSimpleDto>|body|是|终端列表
message|UpMsg|body|是|推送消息内容定义
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识


**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|本次发送的任务标识|

#### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/pushByClients

POST data:
{
"toClients":["1ebc148c322da136b8e8f3439e3fa90e","bbcbdaad3483b3be60cf584cd2aba975"],
"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: fa4de4b47448c32e151f6228575027d58a8b0774d92e788e229498aba5c3af1a
timestamp: 1546850546246 
language: zh-cn
appKey: 2ba149e67de2e4dfae30a82abec26a3a
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData":"TKcb27343560914c76a3d21ce3bac187a8"}
```




### 按设备推送模板消息

> 向用户的某个或多个设备推送消息;接收终端为用户绑定的设备或被分享的设备。

#### 1、接口定义

?> **接入地址：** `/msg/pushWithTmplByClients`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
toDevices|List<String>|body|是|deviceIDs列表
options|Options|body|是|推送消息辅助信息，如消息名，到期时间，优先级等。
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
templateID|Integer|body|是|由UMS分配的模板序号，与UMS管理的业务号和版本号字典表对应
templateParams|Map<String, Object>|body|是|String 表示占位参数值；Object表示实际参数值。
version|String|body|是|版本号：v3

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|无|

#### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/pushWithTmplByClients

POST data: {
	"toDevices": ["MAC"],
	"options": {
		"msgName": "",
		"businessType ": 1,
		"priority ": 1,
		"expires": 60
	},
	"tag": "标签",
	"templateID": 1, //十进制整数 
	"templateParams": {
 "STUFF_ID": 2,//十进制整数
		"ALERT_SWITCH": 193, //十进制整数
		"ALERT_YEAR": 19, //十进制整数
		"ALERT_MONTH": 11, //十进制整数
		"ALERT_DAY": 13, //十进制整数
		"ALERT_1_HOUR": 21, //十进制整数
		"ALERT_1_MINUTE": 59, //十进制整数
		"ALERT_1_FREQ": 2, //十进制整数
		"ALERT_2_HOUR": 11, //十进制整数
		"ALERT_2_MINUTE": 19, //十进制整数
		"ALERT_2_FREQ": 2, //十进制整数
"ALERT_3_HOUR": 1, //十进制整数
		"ALERT_3_MINUTE": 9, //十进制整数
		"ALERT_3_FREQ": 2, //十进制整数
       "ALERT_4_HOUR": 0, //十进制整数
		"ALERT_4_MINUTE": 0, //十进制整数
		"ALERT_4_FREQ": 0 //十进制整数
	},
	"version": "v3"
}


[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546854308557 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************   //accessToken（header）必传
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)



```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData":""}
```




## 云端功能接口

?> 使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>
访问地址(内网访问)：`https://internal.uws.haier.net/umse/v3`  </br>
**为访问安全，云端接口在调用时，需要设置调用方IP白名单。**


Header 中appid 字段填写内容为系统ID，即systemid。 此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”


**云端应用请求Header**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
appId|String|header|是|终端应用的身份ID，通过海极网申请云应用获得appId和appKey
appVersion|String|header|否|应用版本标识，最多32位字符
sequenceId|String|header|是|报文流水号，6-32位；由客户端自行定义，自行生成；建议使用日期+顺序编号的方式。
sign|String|header|是|通过该参数，对调用方进行鉴权，算法详见公共说明
timetamp|long|header|是|Unix时间戳，精确到毫秒
content-type|String|header|是|必须为applicationg/json;charset=UTF-8



### 消息推送

#### 按用户推送消息

> 接收消息的用户列表必须是从指定的APP中注册的用户。

##### 1、接口定义

?> **接入地址：** `/msg/pushByUsers`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toUsers|List<String>|body|是|接受消息的用户ID列表，list 最大200
toApps|List<String>|body|是|接受消息的APP列表，list 最大200
messages|UpMsg|body|是|推送消息内容定义
tag|String|body|否|标签。例如家庭推送时可以存入家庭
isBurn|Integer|body|否|是否阅后即焚

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|本次发送的任务标识

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/umse/v3/msg/pushByUsers

POST data:
{"toApps":["MB-****-0000","MB-****-0001"],"toUsers":["100013957366168858","100013957366169184"],"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData":"TKcb27343560914c76a3d21ce3bac187a8"}

```




#### 按应用推送消息

> 云端按应用列表给终端推送消息

##### 1、接口定义

?> **接入地址：** `/msg/pushByApps`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toApps|List<String>|body|是|接受消息的appId列表
message|UpMsg|body|是|推送消息内容定义
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
isBurn|Integer|body|否|是否是阅后即焚

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|本次发送的任务标识

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/umse/v3/msg/pushByApps

POST data:
{"toApps":["MB-****-0000","MB-****-0001"],"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":"TKc33b74ae08424ec0a5411d37d5fc7bce"}
```





#### 按设备推送消息

> 云端给终端设备推送消息

##### 1、接口定义

?> **接入地址：** `/msg/pushByClients`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toClients|Set<ClientTargetVo>|body|是|终端列表
message|UpMsg|body|是|推送消息内容定义
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<ClientTargetDto>|body|是|本次发送的任务标识



#### 按用户推送消息（支持模板）

> 接收消息的用户列表必须是从指定的APP中注册的用户。

##### 1、接口定义

?> **接入地址：** `/msg/pushWithTmplByUsers`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toUsers|List<String>|body|是|接受消息的用户ID列表
toApps|List<String>|body|是|接受消息的APP列表
message|UpMsg|body|是|推送消息内容定义，只需传入message.options字段，忽略其他字段
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
isBurn|Integer|body|否|是否阅后即焚
templateId|String|body|是|模板标识
templateParams|Map<String,string>|body|是|Map.Entry.key必须唯一

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|本次发送的任务标识


#### 按应用推送消息（支持模板）

> 支持给应用列表推送消息。

##### 1、接口定义

?> **接入地址：** `/msg/pushWithTmplByApps`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toApps|List<String>|body|是|appId列表
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
message|UpMsg|body|是|推送消息内容定义，只需传入message.options字段，忽略其他字段
isBurn|Integer|body|否|是否是阅后即焚
templateId|String|body|是|模板标识
templateParams|Map<String,string>|body|是|Map.Entry.key必须唯一

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
ratData|String|body|是|本次发送的任务标识




#### 根据taskId查询历史消息


> 查看某次推送任务，推送的消息列表。

##### 1、接口定义

?> **接入地址：** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 

**输入参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|Sting|body|是|消息标识
startPage|Integer|body|否|起始页，范围1-10000
pageSize|Integer|body|否|每页显示数量，范围1-1000

**输出参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgCloudHistoryDto>|body|是|推送记录信息
totalRecords|Integer|body|否|
##### 2、请求样例

**输入参数**

```
POST /umse/v3/msg/getMsgHistory

{"taskId":"MTtest2019-01-15 18:02:31_6e2e8f63-a6b4-4ea8-bf48-e64b165d855b"}

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 01.00.00.00000
clientId: ufmtest123
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1553703080084 
language: zh-cn
timezone: Asia/Shanghai
Content-Encoding: utf-8
Content-type: application/json


```

**输出参数**
```
{
	"retCode": "00000",
	"retInfo": "success",
	"retData": [
{
		"taskId":"MTtest2019-01-1518:02:31_6e2e8f63-a6b4-4ea8-bf48-e64b165d855b",
		"userId": "ptuid-2",
		"appId": "ptaid1000000000",
		"clientId": "ptcid-2-8",
		"businessType": 6,
		"msgStatus": 1,
		"readStatus": 1,
		"tag": null,
		"msgCode":" H3016" ,
		"priority": 8,
		"pushTime":"2019-01-01 00:00:00",
		"message": "用户海尔优家，设备海尔空调已经绑定成功"
	}]
}


```







#### 按家庭推送消息


> 云端按家庭给用户注册的终端推送消息。接收消息的家庭列表必须是从指定的APP中注册的用户

##### 1、接口定义

?> **接入地址：** `/msg/pushByFamilies`</br>
**HTTP Method：** POST 

**输入参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toApps|List<String>|body|是|接收消息的应用ID列表。
toFamilies|List<String>|body|是|接收消息的家庭ID列表。
message|UpMsg|body|是|推送消息内容定义。
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
isBurn|Integer|body|否|是否是阅后即焚

**输出参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgCloudHistoryDto>|body|是|本次发送的任务标识

##### 2、请求样例

**输入参数**

```
POST https://uws.haier.net/umse/v3/msg/ pushByFamilies

POST data:
{
    "toApps":[
        "MB-****-0000",
        "MB-****-0001"],
    "toFamilies":[
        "1111",
        "2222"],
    "message":{
        "data":{
            "body":{
                "view":{
                    "showType":-1
                },
                "extData":{
                    "api":{
                        "apiType":"UPDATE_BO_STATUS",
                        "params":{
                            "bsms":[
                                {
                                    "boId":"123",
                                    "boStatus":0,
                                    "boInfo":{
                                        "familyId":"11111",
                                        "boName":"场景名称",
                                        "createdTime":"2020-07-09 13:53:05"
                                    }
                                }]
                        }
                    }
                }
            }
        },
        "options":{
            "businessType":-1,
            "priority":1,
            "msgExpires":0,
            "expires":0,
            "msgName":"SCENE_BSM",
            "jiguangOptions":{
                "apnsProduction":true
            }
        },
        "version":"v3"
    }
} [no cookies]

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData":"TKcb27343560914c76a3d21ce3bac187a8"}

```





#### 按用户获取业务状态列表


> 云端按用户获取BSM记录，默认获取最近（按updateTime倒序）最新三个月的最新业务状态记录(page)

##### 1、接口定义

?> **接入地址：** `/bsm/findBSMByUserID`</br>
**HTTP Method：** POST 

**输入参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
userId|Sting|body|是|接收业务状态的用户ID。
familyId|String|body|是|接收业务状态的家庭ID。
boCategory|Integer|body|是|标识业务类型0：场景；1：家庭
boStatus|Integer|body|是|默认是new :0 read:1
queryTag|Integer|body|是|标识查询起始时间之前、还是之后的消息。1代表之前，0代表之后。
queryTime|TimeStamp|body|是|起始查询时间
startPage|Integer|body|否|起始页，范围1-10000
queryTime|Integer|body|否|每页显示数量，范围1-1000


**输出参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgBoUserDto>|body|是|返回符合条件的业务对象记录

##### 2、请求样例

**输入参数**

```
POST https://uws.haier.net/umse/v3/ bsm/findBSMByUserID

POST data:
{
    "userId":"12345",
    "familyId":"123",
"boStatus":0,
"boCategory":0,
    "queryTag ":0,
    "queryTime ":,"2020-07-09 13:53:05"

}
[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)



```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData":""}


```

## 新终端功能


### 	基本介绍
mcs为终端提供了注册、注销及终端推送消息到终端的能力。
特别说明：终端注册是使用mcs服务的前提或先决条件。


### 公共属性说明
参数名|类型|说明|备注
:-|:-:|:-|:-
userId|	String|	用户唯一标识	| &ensp;|
appId|	String	|应用唯一标识	| &ensp;|
clientId|	String	|终端唯一标识| &ensp;|	
pushId|	String	|通道的推送标识|	通道会为每个终端（通过appId+clientId识别）生成的唯一标识|
msgId	|String	|消息的唯一标识| &ensp;|	
taskId|	String	|任务的唯一标识|	一个任务下包含多个消息|
businessType|	Integer|	消息业务类型|	0:系统类（系统类消息，例如推送升级，热修复等）</br>1:设备类（洗衣机、安防、菜谱分享等）</br>2:运营类（广告，运营等）</br>3:场景类</br>4:家庭类</br>5:活动类</br>&ensp;&ensp;<font color=red>未定义枚举值不允许私自使用</font>|


### 公共对象说明



#### payload

字段名|类型|说明|备注
:-|:-:|:-|:-
version|String|MCSP协议版本号，此版为“v4” |v4
method|String|协议操作类型，主要包括注册/注销、推送消息、状态报告及同步、删除/撤回消息等几类操作 |registerClient
channel|String|承载操作的底层传输通道，包括M2M消息通道、消息中心直接提供的https接口通道、及第三方通道（包括厂商通道）等|mc
sn|String|业务方提供sn；操作序列号，仅出现在部分method中 |sn
from|Json String|操作的发起方，仅出现在部分method中（对于pushMsg，就是消息的发送方）|如client等
registration|json String|MCS Client能力信息，包括Client支持的推送通道、支持的消息交互方式、所在时区、使用的语言等，仅出现在registerClient操作中|
timestamp|String|操作时间|年-月-日_时:分:秒

#### from

字段名|&ensp;|说明|&ensp;
:-|:-:|:-|:-
client|&ensp;|发起操作的MCS Client|&ensp;
&ensp;|pushId|MCS Client的唯一标识|&ensp;
&ensp;|type|MCS Client的类型，仅当registerClient操作时才填写|&ensp;
&ensp;|device.deviceId|设备ID（唯一标识一个设备），仅当registerClient操作时填写|&ensp;
&ensp;|device.nickName|设备昵称，仅当MCS Server发送给MCS Client的pushMsg操作中填写|&ensp;
&ensp;|app.appId|AppID（海极网开通），仅当registerClient操作时填写|&ensp;
&ensp;|app.clientId|ClientID（标识APP在该设备/移动端的实例），仅当registerClient操作时填写|&ensp;
&ensp;|app.name|APP名称，仅当MCS Server发送给MCS Client的pushMsg操作中填写|&ensp;
&ensp;|system.systemId|云应用ID，仅当registerClient操作时填写|&ensp;
user|&ensp;|发起操作的MCS Client上的用户|&ensp;
&ensp;|userId|用户ID，MCS Client能拿到userId时，直接填写userId|&ensp;
&ensp;|token|用户token，MCS Client拿不到userId、但能拿到用户token时，填写token|&ensp;
&ensp;|avatar|用户头像url，仅当MCS Server发送给MCS Client的pushMsg操作中填写|&ensp;
&ensp;|nickName|用户昵称，仅当MCS Server发送给MCS Client的pushMsg操作中填写|&ensp;
&ensp;|mobile|用户手机号码，仅当MCS Server发送给MCS Client的pushMsg操作中填写|&ensp;

#### registration

字段名|&ensp;|说明|&ensp;
:-|:-:|:-|:-
from.client|pushId|MCS Client的唯一标识，注册时不填写，注册成功后由MCS Server返回（随registerClientResp返回）|&ensp;
&ensp;|type|MCS Client的类型|&ensp;
&ensp;|device.deviceId|设备ID（唯一标识一个设备），MCS Client为设备（device）时填写|&ensp;
&ensp;|app.appId|AppID（海极网开通），MCS Client为端应用（deviceApp、mobileApp）时填写|&ensp;
&ensp;|app.clientId|ClientID（标识APP在该设备/移动端的实例），MCS Client为端应用时填写|&ensp;
&ensp;|system.systemId|云应用ID，MCS Client为云应用（cloudApp）时填写|&ensp;
registration|3rdId|采用第三方推送通道时，在第三方注册时、第三方生成的推送ID（唯一标识一个客户端）|&ensp;
&ensp;|acceptChannel|支持的推送通道（取值范围同顶层字段channel）|&ensp;
&ensp;|acceptUiStyle|支持的消息交互方式（客户端收到消息时的消息通知及交互方式），数组，元素同message.ui.style|&ensp;
&ensp;|timezone|客户端终端当前设置的时区|&ensp;
&ensp;|language|客户端终端当前设置的语言偏好|&ensp;


#### templateArgs 

字段名|类型|是否必填|说明|备注
:-|:-:|:-|:-|:-
sn			|String|是|业务方提供sn；操作序列号 |"sn": "sn3" 最多32位
fromPushId	|String|是|推送消息的端，其拥有的pushId|"fromPushId":"pushId0"
toToken		|String|否|消息接收端token|"toToken":"token1"
toDeviceId	|String|否|消息接收端deviceId|"toDeviceId":"deviceId1"
toTypeId	|String|否|	消息接收端TypeId	|"toTypeId":"typeId1"
toAppId		|String|否|消息接收端appId|"toAppId": "appId1"
toFamilyId	|String|否|消息接收端familyId|"toFamilyId": 123
toFamilyBy	|String|否|消息推送终端ID通过用户或设备维度进行|toFamilyBy: "user"</br>toFamilyBy: "device"
toUserId	|String|否|消息接收端userId|"toUserId":1234567
msgTitle	|Object[]|否|推送消息标题|"msgTitle":[{"value"，"title1","lang":"zh-cn"}]
msgBody		|Object[]|否|推送消息主体|"msgBody":[{"value"，" body1","lang":"zh-cn"}]
audioUrl	|String|否|推送语音消息url|"audioUrl":"url1"
audioLength	|String|否|推送语音消息长度|"audioLength":30
audioSize	|String|否|推送语音消息语音大小|"audioSize":10200


#### retData

字段名|类型|说明|备注
:-|:-:|:-|:-
version|String|MCSP协议版本号，此版为“v4”|v4
method|String|协议操作类型，主要包括注册/注销、推送消息、状态报告及同步、删除/撤回消息等几类操作|pushMsgResp/registerClientResp
channel|String|承载操作的底层传输通道，包括M2M消息通道、消息中心直接提供的https接口通道、及第三方通道（包括厂商通道）等|mc
sn|	String|业务方提供sn；操作序列号，仅出现在部分method中|sn
message|json string|	返回处理状态消息提示|taskId
to.client|	String|	注册时返回的是消息中心生成的pushId终端唯一标识|	&ensp;
result.code|	String|	本次请求处理结果码|	成功：00000；异常：其它码
result.desc|	String|	本次请求处理结果描述|	&ensp;




### 接入地址

?>  使用REST接口的风格对外提供服务，仅支持HTTPS协议。访问地址：https://uws.haier.net/mcs/hermes

### 功能接口

#### 账号模块

##### 终端注册/注销

> 1、实现将端的信息绑定到消息中心的功能，在调用本接口后可授权端使用消息中心推送通道向其它端推送消息。</br>
> 2、在注册时，若需要注册用户信息userId，则需要在header中传递accessToken参数；否则，不需要传递该参数。

###### 1、接口定义
?> **接入地址：** `/client-request`</br>
**HTTP Method：** POST

**输入参数：**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|JSON String|body|是|填写参见示例


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode| String|body|是|本次请求状态码
retInfo| String|body|是|本次请求状态信息
retData|JSON String|body|是|本次请求状态具体信息


###### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/mcs/hermes/client-request

POST data:
{
"payload":
注册示例：
{
    "version": "v4",
    "method": "registerClient",
    "channel": "mc",
    "sn": "sn1",
    "from": {
         "client": {
            "type": "device",
            "device": {"deviceId": "value"}
        }
    },
    "registration": {
        "acceptChannel": "mc",
        "acceptUiStyle": ["dialog", "voice"],
        "timezone": "value",
        "language": "value"
    },
    "timestamp": "value"
}
注销示例：
{
    "version": "v4",
    "method": "unregisterUser",
    "channel": "mc",
    "sn": "sn3",
    "from": {
        "client": {
            "pushId": "value",
        },
        "user": {"token": "value"}
    },
    "timestamp": "value"
}
}
[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1545817794954 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************
Content-Length: 121
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)
Version:v4

```

**输出参数**
```
{"retCode":"00000","retInfo":"success","retData": {
    "version": "v4",
    "method": "registerClientResp",
    "channel": "mc",
    "sn": "sn1",
    "to": {
         "client": {
            "pushId": "pushId1"
         }
    },
    result”: {
    “code”: “value”,
    “desc”: “value”
},
    "timestamp": "value"
}
 }

```




#### 消息模块

##### 按模板向端推送消息
> 1、判断消息发送端与接收端是否在消息中心注册成功，成功则通过相应的通道推送消息</br>
> 2、消息发送端与接收端必须先在消息中心注册后方可使用消息中心的发送通道推送消息</br>
> 3、调用方需要提前调用注册接口在消息中心注册终端</br>

###### 1、接口定义
?> **接入地址：** `/push-msg`</br>
**HTTP Method：** POST </br>


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
templateId|String|body|是|预先设计好的消息模板编号，如“100002”
templateArgs|JSON String|body|是|预先设计好的消息模板中特定符号字段替换成真实参数，最后组成实际推送消息体。

**输出参数：**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode| String|body|是|本次请求状态码
retInfo| String|body|是|本次请求状态信息
retData|JSON String|body|是|本次请求状态具体信息



###### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/mcs/hermes/push-msg

POST data:
{
	"templateId": "100001",
	"templateArgs": {
		"sn": "sn3",
		"fromPushId": "pushId0",
		"toUserToken":"token1",
		"toDeviceId": "deviceId1",
		"toTypeId":"typeId1",
		"toAppId": "appId1",
		"toFamilyId": 123,
		"msgTitle": [{"value": "title1"}],
		"msgBody": [{"value": "body1"}],
		"audioUrl": "url1",
		"audioLength": 30,
		"audioSize": 10200
	}
}

Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)
Version:v4



```
**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":{
    "version": "v4",
    "method": "pushMsgResp",
    "channel": "mcs",
    "sn": "sn3",
    "message": {
        "msgId": "msgId1"
    },
    result”: {
    “code”: “value”,
    “desc”: “value”
},
    "timestamp": "2020-02-10_11:30:40"
}}

```

### 附录

#### 大数据业务参考示例：

> 终端注册/注销

**输入参数**
```
POST https://uws.haier.net/mcs/hermes/client-request
Header:
Request Headers:
Connection: keep-alive
appId: MB-*****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1545817794954 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
Content-Length: 121
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)
Version:v4

注册示例：

POST data:
{
"payload":
{
    "version": "v4",
    "method": "registerClient",
    "channel": "mcs",
    "sn": "sn2",
    "from": {
         "client": {
            "type": "cloudApp",
            "system": {
                "systemId": "value"
            }
        }
    },
    "timestamp": "value"
}



```
**输出参数**

```
{
    "version": "v4",
    "method": "registerClientResp",
    "channel": "mcs",
    "sn": "sn2",
    "to": {
         "client": {
            "pushId": "pushId0"
         }
    },
    "result": {
        "code": "00000",
        "desc": "successful"
    },
    "timestamp": "value"
}


```



> 按模板向端推送消息

**输入参数**
```
POST https://uws.haier.net/mcs/hermes/push-msg
Header:
Request Headers:
Connection: keep-alive
appId: SV-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546850546246 
language: zh-cn
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
Content-Length: 639
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)
Version:v4


POST data:
{
	"templateId": "100002",
	"templateArgs": {
		"sn ": "sn3 ",
		"fromPushId": "pushId0",
		"toUserId": "userId1",
		"toDeviceId": "deviceId1",
		"files": [{
			"fileSize": 2048,
			"fileUrl": "url1",
			"fileHash": "hash1",
			"fileToken": "token1"
		}]
	}
}



```
**输出参数**

```
{
    "version": "v4",
    "method": "pushMsgResp",
    "channel": "mcs",
    "sn": "sn3",
    "message": {
        "msgId": "123"
    },
    "result": {
        "code": "00000",
        "desc": "successful"
    },
    "timestamp": "2020-02-10 11:30:40"
}



```




[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:../Message/_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:../Message/_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:../Message/_media/_MessagePush/MessagePush_flow.png

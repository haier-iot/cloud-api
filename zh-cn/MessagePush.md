
!> **更新时间：** {docsify-updated} 

## 简介
为开发者提供基于物联网的消息推送服务，消息推送支持个性化的推送服务模式，开发者可以根据应用特性或者特定场景要求通过配置自定义消息的方式给App用户推送相关信息。
![消息推送标准版图片][MessagePush_type]



### 应用场景
适用于物联网的消息推送服务，包括消息从一个设备终端发送到另一个设备终端，或者从应用服务端发送到设备终端。

![消息推送场景流程][MessagePush_flow]

**权限与注册**

IOT消息推送，根据需求选择FCM 或 极光推送，使用端需要先初始化对应ID

使用消息推送模块，首先需要注册终端获取用户终端信息，根据设备类型进行推送

**通道与推送**

消息通道：从UMS注册消息通道，获取后台推送消息服务

推送授权：授权云端给应用或相应客户推送服务消息

业务消息：业务方通过调用云端消息推送服务，推送应用消息或者推送服务邮件



**免打扰机制**

提供消息免打扰机制，用户可自主设置免打扰规则、查询和管理免打扰机制等。


## 公共结构说明
### TerminalDto
终端信息

参数名|类型|说明|备注
:-|:-:|:-|:-
userId|String| |用户Id，唯一标识
clientId|String|如果能直接从uSDK中获取则需要从uSDK中获取；如果没有uSDK，则可以取设备mac地址
devAlias|String|终端别名|终端别名
appId|String| |应用ID，40位以内字符
isOnline|Integer|1，在线；2，离线|1,代表10min内在线；</br>2,代表10min内不在线
lastOnlineTime|Date|设备最近一次在线时间|当isOnline位1时，该字段不返回；</br>当isOnline为2，且该字段不返回时，则表示最后一次在线时间是两天之前

### UpMsg

透传字段：data、androids、ioss，请严格按照消息模型定义以及第三方通道对于三者字段内容的定义

字段名|类型|说明|备注
:-|:-:|:-|:-
notification|Map<String,Object>|定义通知的内容，详见Notification对象定义|
data|Map<String,Object>|定义自定义消息的数据内容，详见Data对象定义|
android|Map<String,Object>|定义Android系统消息定制化内容，详见android对象定义|
ios|Map<String,Object>|定义IOS系统消息定制化内容，详见IOS对象定义
options|Options|定义消息的选项设置，详见Option对象定义
version|String|定义消息的版本，次版本为V1|

### MsgClientHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
taskId|String|消息任务ID|终端收到的msgId即ums的taskId
businessType|String|业务类型|0：系统类（系统类消息，例如推送升级，热修复等）</br>1：设备类（场景引擎，菜谱分享等）</br>2：运营类（广告，运营等）
message|UpMsg|消息模型|消息内容
msgStatus|Integer|消息发送状态|1，待发送；2，发送中；3，成功；4，失败
readStatus|Integer|消息读取状态|消息是否被读取
pushTime|DateTime|ums通道推送时间|推送时间`yyyy-MM-dd HH:mm:ss`


### MsgCloudHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
userId|String|用户ID|
appId|String|应用ID|
clietnId|String|终端ID|
busineeType|String|业务类型|0：系统类（系统类消息，例如推送升级，热修复等）</br>1：设备类（场景引擎，菜谱分享等）</br>2：运营类（广告，运营等）
msgStatus|Integer|消息发送状态|1，待发送；2，发送中；3，成功；4，失败
readStatus|Integer|消息读取状态|消息是否被读取
tag|String|标签|自定义标签
pushTime|DateTime|ums消息推送时间|`yyyy-MM-dd HH:mm:ss`
msgCode|String|返回码|

### DoNotDisturbDto

字段名|类型|说明|备注
:-|:-:|:-|:-
dndId|String|免打扰标识|
beginTime|Integer|开始时间|
endTime|Integer|结束时间|
businessType|Integer|消息业务类型|
priority|Integer|消息优先级|1，2，3



### MsgUnreadNumDto

字段名|类型|说明|备注
:-|:-:|:-|:-
business|String|业务类型|
msgNums|String|未读消息数量|

## 消息推送模型说明

### 消息发送方数据模型说明

#### 模型定义

> 消息推送数据模型定义了云服务给需要接收应用内消息的客户端推送消息的内容格式，是云服务和App端消息格式的基本要求，所有通过消息推送服务推送的消息必须满足该模型的定义，否则，将不予推送。

消息推送数据模型采用JSON格式进行定义，主要内容包括：通知、数据、消息选项、版本以及推送渠道相关的透传属性。以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
notification|notification和data至少一个必填|Notification|定义通知的内容，详见Notification对象定义
data|notification和data至少一个必填|Data|定义自定义消息的数据内容，详见Data对象定义
android|否|android或者Android[]|定义安卓系统消息定制化内容，详见Android对象定义
ios|否|IOS或IOS[]|定义IOS系统消息定制化内容，详见IOS对象定义
options|是|Options|定义消息的选项设置，详见Options对象定义
version|是|string|定义消息版本，此版本为V3

#### Notification

> 定义通知的内容，此部分会由接收消息的App所在的操作系统或第三方推送通道（国内）自动展示为系统通知，通常会显示为标题和内容并显示App的图标，也可以由消息推送方进一步定义通知的展示方式包括播放通知音、震动、折叠显示等。

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
title|否|String|定义在所有系统使用通知标题，如果此项不填，则默认显示APP的名称
body|是|String|定义在所有系统使用通知的消息体


> 请注意：在android、ios中定义了title、body将在对应的系统通知中覆盖公共定义。

#### Android


> 定义Android消息的内容，此部分内容主要定义更加精细的Android系统通知展示方式。根据第三方推送服务方的不同，此部分定义会有所区别，如果接收方存在集成不同的第三方推送服务的情况则需要分别进行定义。

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
jpush|否|Json对象|极光推送通道notification.android透传属性
fcm|否|Json对象|FCM推送通道message.android透传属性
m2m|否|Json对象|m2m本期暂不支持

其他属性定义请参考如下资料：  

第三方推送服务|资料
:-:|:-:
极光推送|`https：//docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#notification`中Android部分
FCM|`https：//firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages`



#### IOS  

> 定义IOS消息的内容，此部分内容主要定义更加精细的IOS系统通知展示方式。根据第三方推送服务方的不同，此部分定义会有所区别，如果接收方存在集成不同的第三方推送服务的情况则需要分别进行定义。  

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
jpush|否|Json对象|极光推送notification.ios透传属性
fcm|否|Json对象|FCM推送通道message.apns透传属性
m2m|否|Json对象|m2m本期暂不支持  


其他属性定义请参考如下资料：  

第三方推送服务|资料
:-:|:-:
极光推送|`https：//docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#notification`中IOS部分
FCM|`https：//firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages`

#### Data  

> 定义实时消息的数据内容，此部分内容在App被唤醒或处于前台时，由第三方推送服务主动传递给App并由App来处理。此部分内容由云服务和App自行确定，但给U+ App及成套产品推送消息需遵循以下定义。  


以下为各属性的具体说明：  


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
body|是|Body|实时消息在app端的具体业务及展示方式，详见Body对象定义

##### Body  

以下为各属性的具体说明： 

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
view|否|View|实时消息的展示方式，详见View对象定义
extData|否|ExtData|实时消息的业务数据，详见ExtData对象定义
extParam|否|ExtParam|自定义业务数据    

###### View   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
showType|否|int|实时消息的显示样式：</br>-1：不显示，app一般用于无UI展示，直接处理消息内容；</br>0： toast；</br>2： 弹框，业务事件及button 按钮自定义见 btns 封装；</br>4： 红色感叹号显示；</br>20：弹框，无按钮；</br>21：弹框,一个按钮；无相应业务事件处理；</br>22：弹框，确定、取消两个按钮，均无业务事件处理
title|否|string|实时消息的弹框标题
content|否|string|实时消息的弹框内容 
btns|否|Button|弹框显示的按钮，详见Button对象定义  


###### Button   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
text|否|string|按钮的展示文本
callId|否|int|按钮事件调用id，用来标识此按钮单击事件在ExtData中需要的参数信息    

###### ExtData   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
expireTime|否|int|业务消息在本地系统的有效时间。若该值超出时间范围则在 APP 端视为无效消息，不进行业务处理。若为空或无此字段不处理
isMsgCenter|否|int|实是否存储在 APP 消息中心。若为空或无此字段不处理，取值为 0：不存储 1：存储
device|否|Device[]|控制类消息要控制的设备信息，详见Device对象定义 
devControl|否|DevControl|设备控制指令信息，与device配合使用，详见DevControl对象定义  
api|否|API|调用App端API，详见API对象定义
page|否|Page|消息分页信息，详见Page对象定义    


###### Device   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
typeId|否|string|设备typeid
deviceId|是|string|设备mac地址
deviceName|否|string|设备名称    

###### DevControl   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用，此时ExtData中的device对象数据不能为空，否则无法执行
deviceId|是|string|设备mac地址
groupName|否|string|组命令名称  
cmdList|是|json object|标准模型的命令键值对集合   

###### API  

> 自定义事件消息，执行API调用处理。APP端对此业务的处理逻辑：消息监听者或事件执行者收到此类消息时，执行相应API接口调用，并将携带的参数传至API接口内，若参数与API接口定义不符则失败。   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用 
apiType|是|string|api定义。如附录中的删除家庭处理为：DELEATE_FAMILY
params|否|json object|按钮在alert索引序号，由0开始。该API接口定义的入参集合   



###### Page  

> 自定义事件消息，执行Page页面跳转处理。APP端对此业务的处理逻辑：消息监听者或事件执行者收到此类消息时，交由VDN 执行页面跳转处理。   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用，否则根据Button 的callId响应 
url|是|string|页面唯一地址。如是native页面，则需在VDN的DNS表中页面保持一致
params|否|int[]|参数键值对集合。页面跳转参数集合   



###### ExtParam  

> 自定义业务数据，可自行扩展。  

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
actWithNotify|否|Boolean|消息附属在通知里有效。</br>true：收到通知后，同时处理内部附属的消息内容</br>false：收到通知后，待用户点击通知栏时处理UI业务


#### Options  

> 定义消息的设置信息，包括消息的名称、消息的类型等。 

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgName|否|String|消息名字
businessType|是|int|消息类型，App端根据此分类进行消息展示。取值如下：</br>0：系统类（系统类消息，例如推送升级，热修复等）</br>1：设备类（场景引擎，菜谱分享等）</br>2：运营类（广告，运营等）
expires|否|int|消息在客户端离线时在第三方推送平台缓存时间，过期将不再推送给客户端。单位为秒，最长86400秒，如未指定则默认为86400秒。  
priority|否|int|见priority备注
iguangOptions|否|json object|见jiguangOptions备注

priority备注：  

消息优先级别，如设置则默认为1。取值为：</br>0 ：紧急消息，一般需要唤醒屏幕，播放消息提示音</br>1：一般消息，在屏幕黑屏时候，播放消息提示音，无需唤醒屏幕</br>2：中低消息，无需声音提示</br>3： 低级此类消息，可能接收消息后，APP 无需立刻处理，等系统空闲或者 wifi 状态下处理即可</br>以上优先级定义主要在实时消息中由App实现相关优先级的效果，不同优先级的实时消息如果同时定义了通知，则与通知的优先级对应关系为：</br>通知：高优先级 ←→实时消息：紧急</br>通知：普通优先级←→实时消息：一般，中低，低级
 
jiguangOptions备注：  

极光推送选项设置，请参考：</br>`https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#options` </br>若使用极光推送在生产环境给ios端推送消息，该字段中必须包含apnsProduction，具体格式如下：
```
"jiguangOptions": {
				    "apnsProduction": true
			      }
```


#### 举例  

云服务发送的消息内容举例如下: 

```  

	"notification": {
		"title": "test message",
		"body": "This is a test message "
	},
	"android": {
		"jpush": {
			"title": "test message",
			"body": "This is a test message ",
			"alert_type": 1
		},
		"fcm": {
			"title": "test message",
			"body": "This is a test message ",
			"notification ": {
				"sound": "default"
			}
		}
	},
	"data": {
		"body": {
			"view": {
				"showType": 21,
				"title": "test message",
				"content": "This is a test message"
			},
			"extData": {
				"isMsgCenter": 1
			}
		}
	},
	"options": {
		"msgName": "",
		"businessType ": 1,
		"priority ": 1,
		"expires": 60
	}
}
```

## 消息接收模型说明

### 消息接收方数据模型说明

#### 通知数据模型  

> 通知的数据模型请参考如下第三方文档中对通知部分的相关说明。

##### App集成极光推送接收消息

安卓请参考：
  
`https://docs.jiguang.cn/jpush/client/Android/android_sdk/`  

IOS请参考：

`https://docs.jiguang.cn/jpush/client/iOS/ios_sdk/`  

说明：对于极光推送，发送方同时定义Notification和Data时，Data部分从通知的extras.content的内容，具体内容的模型请参照`实时消息普通数据模型`。


##### App集成FCM接收消息  

安卓请参考：  

`https://firebase.google.com/docs/cloud-messaging/android/client`  


IOS请参考：  

`https://firebase.google.com/docs/cloud-messaging/ios/client`


#### 实时消息普通数据模型  

> 定义自定义消息的数据内容，此部分内容在App被唤醒或处于前台时，由第三方推送服务主动传递给App并由App来处理。   

以下为各部分的具体说明：  

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgId|是|String|消息的唯一标识，默认此属性值由消息推送服务自动填充
msgName|否|String|消息的名称，取值请参考Options中msgName定义，默认此属性值由消息推送服务自动根据Options中msgName填充，如发送方设置值将被覆盖
businessType|否，正常业务为必填，阅后即焚、空消息为非必填|int|消息分类，取值请参考Options中businessType定义，默认此属性值由消息推送服务自动根据Options中businessType填充
priority|否|int|默认此属性值由消息推送服务自动根据Options中priority填充，如果发送方设置数值则以发送方设置为准
body|是|Body|实时消息在app端的具体业务及展示方式，详见body章节各对象定义。  


需要注意：客户端收到的数据为base64编码后的数据，App需要对数据进行base64解码才能获取原始的json数据。

接收到的消息内容（FCM需要从返回内容的content属性取）举例：  

```
eyJtc2dJZCI6IjAwMDAwMDAwMDAxIiwibXNnTmFtZSI6IiIsImJvZHkiOiB7InZpZXciOiB7InNob3dUeXBlIjoyMSwJInRpdGxlIjoidGVzdCBtZXNzYWdlIiwiY29udGVudCI6IlRoaXMgaXMgYSB0ZXN0IG1lc3NhZ2UifSwiZXh0RGF0YSI6eyJpc01zZ0NlbnRlciI6MX19fQ==
```  
base64解密为：  

```
{"msgId":"00000000001","msgName":"","body": {"view": {"showType":21,	"title":"test message","content":"This is a test message"},"extData":{"isMsgCenter":1}}}
```  

#### 阅后即焚数据模型

> 阅后即焚消息是一种特殊的实时消息，本节定义阅后即焚消息的数据模型，当消息接收方收到阅后即焚消息后，需对消息立即做出响应，停止显示相应的消息并删除。  

以下为各部分的具体说明：  

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgId|是|String|消息的唯一标识，默认此属性值由消息推送服务自动填充
msgName|是|String|消息的名称，取值为UPN_CANCEL  
body|是|Body|见body备注
 
需要注意：客户端收到的数据为base64编码后的数据。

body备注：  

消息内容，具体定义为：</br>  
```
{ 
“extData”:{
“isMsgCenter”:0,
“api”:{
            “callId”:0,
            “apiType”:0,
            “params”:{
                “op-msgId”:”要阅后即焚的msgId，由系统自动填充”
            }
        }
}
}
```


#### 空业务数据模型  

> 空业务消息是一种特殊的实时消息，本节定义空业务数据模型，当消息接收方收到空业务消息后无需处理，此消息主要用来更新之前阅后即焚消息。  

以下为各部分的具体说明：  

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgId|是|String|消息的唯一标识，默认此属性值由消息推送服务自动填充
msgName|是|String|消息的名称，取值为UPN_NULL 
msgType|是|int|消息的来源，取值：</br>3：状态类，此类消息由消息推送自动触发  （消息状态类，例如阅后即焚通知， 空消息）  
body|是|Body|消息内容，内容为{}。  

需要注意：客户端收到的数据为base64编码后的数据。  
 

## 终端功能接口列表

?>  使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>
访问地址：`https://uws.haier.net/ums/v3`


### 账号模块

#### 终端注册

> 通过该接口，在系统中注册接收消息的终端，建立appId、userId、clientId、pushId之间的关系；该接口是使用ums和umse的先决条件。</br>
> 在注册时，若需要注册用户信息userId，则需要在header中传递accessToken


##### 1、接口定义
?> **接入地址：** `/account/register`</br>
**HTTP Method：** POST

**输入参数：**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
channel|Integer|body|是|通道类型。</br>0代表极光，</br>1代表m2m通道，</br>2代表fcm通道，</br>3代表邮件，</br>4代表不使用或无通道。
pushId|String|body|否|终端的通道推动标识。</br>当channel为4时可以为空，其它情况不能为空，fcm通道时长度应该为152。
devAlias|String|body|否|设备别名
msgVersion|String|body|是|消息模型版本，对应消息模型中的version


**输出参数:** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/account/register

POST data:
{
	"channel":2,
	"pushId":"fbIyFJWV_M4:APA91bFYu308MAM5PyJxvUMiJKHT6yJl_O4z3HTyjr",
	"devAlias":"ios of yy",
	"msgVersion":"v3"
}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: 234297626c79198546d965cedaef915264f47eaca7a21e1a508301ee1b81db9b
timestamp: 1545817794954 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0

```

**输出参数**
```
{
	"retCode":"00000",
	"retInfo":"success"
}
```

#### 终端注销
> 注销已注册的终端信息</br>
> 一般在用户账号注销或APP卸载时滴啊用该接口


##### 1、接口定义
?> **接入地址：** `/account/logout`</br>
**HTTP Method：** POST

**输入参数：** 无参数输入

**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/account/logout

POST data:


[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: a9f87157f94c1c2848aa221d19016a768d936070f2642c5819183256953310d2
timestamp: 1545817872035 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0
Content-Length: 0
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```
**输出参数**

```
{
	"retCode":"00000",
	"retInfo":"success"
}
```



#### 获取用户终端信息
> 查询该用户下所有处于激活状态的终端</br>
> 根据userId查询，该userId注册的所有激活状态的终端信息

##### 1、接口定义
?> **接入地址：** `/account/getTerminals`</br>
**HTTP Method：** POST

**输入参数：**  无输入参数

**输出参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<TerminalDto>|Body|是|终端信息列表


### 设置模块

#### 设置终端免打扰
> 设置终端能够按照业务类型、优先级、时间段进行免打扰；</br>
> 1.以userId+appId+clientId标识唯一终端；</br>
> 2.同一终端可以设置多条免打扰信息；</br>
> 3.不同消息业务类型（businessType）需要分别设置免打扰；</br>
> 4.同一终端下设置多条免打扰时时间不允许有交叉；</br>
> 5.每条免打扰配置支持设置多个优先级</br>


##### 1、接口定义
?> **接入地址：** `/config/setNotDisturb`</br>
**HTTP Method：** POST </br>


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|是|消息业务类型
priority|Integer|body|是|消息优先级，priority定义见消息模型
beginTime|String|body|是|开始时间
endTime|String|body|是|结束时间

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndId|String|body|是|免打扰唯一标识


#### 取消终端免打扰

删除设定的免打扰配置

1、用户登陆后，方可使用（Header中accessToken参数必填）
2、已设置过免打扰


##### 1、接口定义
?> **接入地址：** `/config/cancelNotDisturb`</br>
**HTTP Method：** POST 


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndId|String|body|是|免打扰设置唯一标识


**输出参数：**标准输出参数



#### 查询免打扰信息

> 获取已设定的免打扰配置列表，以userId+appId+clientId（即终端）为粒度查询

##### 1、接口定义

?> **接入地址：** `/config/getNotDisturbs`</br>
**HTTP Method：** POST 


**输入参数：** 无

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<DoNotDisturbDto>|body|是||


### 消息模块

#### 按设备推送消息

> 从用户的某个终端推送消息到该用户的多个终端，消息的发送和接收终端都同属该用户（可以是不同APP）

##### 1、接口定义

?> **接入地址：** `/msg/pushByClients`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
toClients|List<String>|body|是|属于该用户的clientId集合
message|UpMsg|body|是|推送消息内容定义

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|本次发送的任务标识|

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/umse/v3/msg/pushByClients

POST data:
{"toClients":["1ebc148c322da136b8e8f3439e3fa90e","bbcbdaad3483b3be60cf584cd2aba975"],"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-UZHSH-0000
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



#### 上报消息的读取状态

> 更新消息的读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息。

##### 1、接口定义

?> **接入地址：** `/msg/reprotStatus`</br>
**HTTP Method：** POST 

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|终端收到的任务标识


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatus

POST data:
{ 	"taskId":"TK123456789123456789" }

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: 0ce87f502a6f020d17466f0971eeedd6e3a1ce81d7ca2d8074c87c5fb4c5cfc7
timestamp: 1546854308557 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```


#### 批量更新读取状态

> 批量更新当前登录用户的消息读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息，该接口需在终端读取消息时调用。  

##### 1、接口定义

?> **接入地址：** `/msg/reportStatusByPatch`</br>
**HTTP Method：** POST   

**前置条件：**   
1.用户登录后使用（即：调用接口时Header中accessToken参数必填）。  
2.终端收到并读取消息。  

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskIds|List|body|是|终端收到的任务标识


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatusByPatch

POST data:



[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: 0ce87f502a6f020d17466f0971eeedd6e3a1ce81d7ca2d8074c87c5fb4c5cfc7
timestamp: 1546854308557 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```

#### 按消息类型更新读取状态

> 按消息的业务类型更新当前登录用户的消息读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息，该接口需在终端读取消息时调用。  

##### 1、接口定义

?> **接入地址：** `reportStatusByType`</br>
**HTTP Method：** POST   

**前置条件：**   
1.用户登录后使用（即：调用接口时Header中accessToken参数必填）。  
2.终端收到并读取消息。
  

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|String|body|是|消息的业务类型


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatusByType

POST data:



[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: 0ce87f502a6f020d17466f0971eeedd6e3a1ce81d7ca2d8074c87c5fb4c5cfc7
timestamp: 1546854308557 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```


#### 查询未读消息的数量

> 查询当前登录用户下各业务类型的未读消息数量</br>
> 按业务类型统计未读消息数量。  

##### 1、接口定义

?> **接入地址：** `/msg/getUnreadNum`</br>
**HTTP Method：** POST   

**前置条件：** 

 用户登录后使用（即：调用接口时Header中accessToken参数必填）  
  

**输入参数：** 无输入参数

**输出参数：**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgUnreadNumDto >|body|是|各消息业务类型下未读消息的数量  

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatusByType

POST data:



[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: 0ce87f502a6f020d17466f0971eeedd6e3a1ce81d7ca2d8074c87c5fb4c5cfc7
timestamp: 1546854308557 
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: TGT28NIRF26AOAB72CU1ZR8BDL4AR0
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)

```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```



#### 查询历史消息

> 1、用户可以查询最长1年以内的历史消息 </br>
> 2、历史消息不支持跨APP查询 </br>
> 3、支持按终端查询（appId+userId+clientId）、按业务类型查询(appId+userId+clientId+businessType)、标签查询（appId+userId+clientId+tag）</br>
> 4、支持分页，按时间倒序排列</br>

##### 1、接口定义

?> **接入地址：** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|否|消息业务类型
tag|String|body|否|自定义标签
pageIndex|Integer|body||当前页
pageSize|Integer|body||每页显示数量



**输出参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgClientHiustoryDto>|body|是|推送记录信息



#### 删除应用内历史消息


>1、用户提交申请删除一条或批量删除多条应用内消息</br>
>2、系统收到请求后，将相关消息标记为删除状态(MSG_DISPATCH表MSG_STATUS=5)

##### 1、接口定义

?> **接入地址：** `/msg/delMsgHistory`</br>
**HTTP Method：** POST 

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|消息任务Id一个或多个，逗号分隔

**输出参数：** 标准输出参数

## 云端功能接口

?> 使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>
访问地址：`https://uws.haier.net/umse/v3`  </br>
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
toUsers|List<String>|body|是|接受消息的用户ID列表
toApps|List<String>|body|是|接受消息的APP列表
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
POST https://uws.haier.net/umse/v3/msg/pushByApps

POST data:
{"toApps":["MB-UZHSH-0000","MB-UZHSH-0001"],"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-UZHSH-0000
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
{"retCode":"00000","retInfo":"success","retData":"TKc33b74ae08424ec0a5411d37d5fc7bce"}
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
businesssType|Integer|body|是|消息业务类型
message|UpMsg|body|是|推送消息内容定义
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
{"toApps":["MB-UZHSH-0000","MB-UZHSH-0001"],"message":{"notification":{"title":"test message","body":"ums : hello world "},"options":{"msgName":"","businessType":0,"expires":60,"priority":1,"jiguangOptions":{"apnsProduction":true}},"android":{"jpush":{"collapseKey":"test message","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"test message","body":"This is a test message ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"test message","content":"ums : hello world"},"extData":{"isMsgCenter":1}}},"version":"v3"}}

[no cookies]

Request Headers:
Connection: keep-alive
appId: SV-UZHSH-0000
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
{"retCode":"00000","retInfo":"success","retData":"TKc33b74ae08424ec0a5411d37d5fc7bce"}
```


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
messages|UpMsg|body|是|推送消息内容定义
tag|String|body|否|标签。例如家庭推送时可以存入家庭
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
toApps|List<String>|body|是|接受消息的appId列表
tag|String|body|否|标签，例如家庭推送时可以存入家庭标识
message|UpMsg|body|是|推送消息内容定义
isBurn|Integer|body|否|是否是阅后即焚
templateId|String|body|是|模板标识
templateParams|Map<String,string>|body|是|Map.Entry.key必须唯一

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
ratData|String|body|是|本次发送的任务标识

#### 推送邮件信息

> 推送邮件信息，根据systemId使用相应的邮件模板推送消息，3.1.0版本中只有默认模板，向用户推送邮件信息

##### 1、接口定义

?> **接入地址：** `/msg/sendEmail`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toList|List<String>|body|是|目标用户，最多16个目标地址（目前16个，系统可配）ccList,toList,bccList最少一个不为空
ccList|List<String>|body|否|抄送用户，最多16个目标地址(目前16个,系统可配) ccList,toList,bccList最少一个不为空
bccList|List<String>|body|否|密抄用户，最多16个目标地址(目前16个,系统可配) ccList,toList,bccList最少一个不为空
priority|Integer|body||
formaType|String|body||邮件格式。1是text，2是html
title|String|body||
content|String|body||

##### 2、请求样例

**输入参数**
```
{
    "toList": [
        "chenjinlei.uh@haier.com",
        "abk@haier.com"
    ],
    "ccList": [
        "lifeng.uh@haier.com"
    ],
    "bccList": [
        "lifeng.uh@haier.com"
    ],
    "priority": "1",
    "subject": "test",
    "contentType": "TXT",
    "content": "test email send template",
    "emailTemplateId": "1000123"
}

```

**输出参数**
```
{
"retCode":"00000",
"retInfo":"成功",
"msgId":"57c0f0a28a1e481c9833b858db4b675e"
}
```

##### 3、错误码

错误码|描述|情景
:-:|:-:|:-
B00001|缺少必填参数|
B00004|参数不符合规则要求|
A00002|网络异常|
B00002|参数类型错误|
A00007|邮件服务异常|
D00008|用户不合法|
A00008|邮件发送失败|

#### 推送邮件消息（支持模板）

> 推送邮件信息，根据systemId使用相应的邮件模板推送消息，3.1.0版本中只有默认模板，向用户推送邮件信息

##### 1、接口定义

?> **接入地址：** `/msg/sendEmailWithTmpl`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
toList|List<String>|body|是|目标用户，最多16个目标地址（目前16个，系统可配）ccList,toList,bccList最少一个不为空
ccList|List<String>|body|否|抄送用户，最多16个目标地址(目前16个,系统可配) ccList,toList,bccList最少一个不为空
bccList|List<String>|body|否|密抄用户，最多16个目标地址(目前16个,系统可配) ccList,toList,bccList最少一个不为空
priority|Integer|body||优先级
language|String|body||默认语言
templateld|String|body||模板ID
templateParams|Map<String,String>|body||模板参数


##### 2、请求样例

**输入参数**

```
{
    "toList": [
        "chenjinlei.uh@haier.com",
        "abk@haier.com"
    ],
    "ccList": [
        "lifeng.uh@haier.com"
    ],
    "bccList": [
        "lifeng.uh@haier.com"
    ],
    "priority": "1",
    "subject": "test",
    "contentType": "TXT",
"content": ["param1": "value1",  
"param2": "value2",  
" param3": " value3",  
" param4": " value4"],
    "emailTemplateId": "1000123"
}

```

**输出参数**

```
{
"retCode":"00000",
"retInfo":"成功",
"msgId":"57c0f0a28a1e481c9833b858db4b675e"
}

```
##### 3、接口错误码

错误码|描述|情景
:-:|:-:|:-
B00001|缺少必填参数|
B00004|参数不符合规则要求|
A00002|网络异常|
B00002|参数类型错误|
A00007|邮件服务异常|
D00008|用户不合法|
A00008|邮件发送失败|


#### 根据taskId查询历史消息


> 查看某次推送任务，推送的消息列表。

##### 1、接口定义

?> **接入地址：** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 

**输入参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|Sting|body|否|消息标识



**输出参数**


参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgCloudHistoryDto>|body|是|推送记录信息

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
sign: 1ca44240e53ac7f69f732a721c29c8906827feb873e5ac0a29010ba545f1cec4
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


[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:_media/_MessagePush/MessagePush_flow.png

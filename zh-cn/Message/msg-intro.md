#  介绍

## 简介
为开发者提供基于物联网的消息推送服务，消息推送支持个性化的推送服务模式，开发者可以根据应用特性或者特定场景要求通过配置自定义消息的方式给App用户推送相关信息。


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



## 公共属性说明
参数名|类型|说明|备注
:-|:-:|:-|:-
userId|	String|	用户唯一标识	| &ensp;|
appId|	String	|应用唯一标识	| &ensp;|
clientId|	String	|终端唯一标识| &ensp;|	
pushId|	String	|通道的推送标识|	通道会为每个终端（通过appId+clientId识别）生成的唯一标识|
msgId	|String	|消息的唯一标识| &ensp;|	
taskId|	String	|任务的唯一标识|	一个任务下包含多个消息|
businessType|	Integer|	消息业务类型|	-1:设置免打扰失效</br>0:系统类（系统类消息，例如推送升级，热修复等）</br>1:设备类（洗衣机、安防、菜谱分享等）</br>2:运营类（广告，运营等）</br>3:场景类</br>4:家庭类</br>5:活动类</br>6:服务提醒</br>7:交易物流</br>8:会员服务</br>9:在线客服</br>10:问题反馈</br>11:众播消息</br>&ensp;&ensp;<font color=red>未定义枚举值不允许私自使用</font>|
channel|	Integer|	通道标识|	0:极光 1:m2m通道 2:fcm通道 3:邮件 4:不使用或无通道|
isBurn|	Integer|	消息撤回标识	|0:正常消息 1:阅后即焚|
priority|	Integer|	消息优先级	|0:最高级；1:重要；2:正常；3:次要|
msgExpires|	Integer|	消息过期标识	|-1:一年；0:立即过期|
msgVersion|	String|	消息模型版本	|v2:v3之前版本；v3:v3版本|
queryTag|	Integer|标识查询起始时间之前、还是之后的消息	|0:给定时间之后；1:给定时间之前|



## 公共结构说明

>  所有返回参数为null时不返回。

### TerminalDto
终端信息

参数名|类型|说明|备注
:-|:-:|:-|:-
userId|String| |用户Id，唯一标识
clientId|String| |如果能直接从uSDK中获取则需要从uSDK中获取；如果没有uSDK，则可以取设备mac地址
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
version|String|定义消息的版本，此版本为V3|

### MsgClientHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
taskId|String|消息任务ID|终端收到的msgId即ums的taskId
businessType|Integer|业务类型|见公共属性说明
message|UpMsg|消息模型|消息内容
msgStatus|Integer|消息发送状态|1，待发送；2，发送中；3，成功；4，失败
readStatus|Integer|消息读取状态|1:未读，2:已读
pushTime|Date|ums通道推送时间|推送时间`yyyy-MM-dd HH:mm:ss.SSS`


### MsgCloudHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
userId|String|用户ID|见公共属性说明
appId|String|应用ID|见公共属性说明
clietnId|String|终端ID|见公共属性说明
busineeType|Integer|业务类型|见公共属性说明
message|	UpMsg|	消息模型	|见UpMsg
msgStatus|Integer|消息发送状态|1，待发送；2，发送中；3，成功；4，失败
readStatus|Integer|消息读取状态|1:未读，2:已读
tag|String|标签|自定义标签，可定义家庭ID等
pushTime|Date|ums消息推送时间|`yyyy-MM-dd HH:mm:ss.SSS`
retCode|String|消息发送状态码|

### DoNotDisturbDto

字段名|类型|说明|备注
:-|:-:|:-|:-
dndId|String|免打扰标识|
beginTime|Date|开始时间|时间格式: HH:ss
endTime|Date|结束时间|时间格式: HH:ss
businessType|	Integer|见公共属性说明
dndTag|	String|	免打扰标签|	同推送系列接口中的tag
dndType|	Integer|	免打扰类型|	0代表按类型免打扰，此时businessType有值，tag为null;</br> 1代表按标签免打扰，此时businessType为null，tag有值;
priority|	Integer|	消息优先级|	见公共属性说明





### MsgUnreadNumDto

字段名|类型|说明|备注
:-|:-:|:-|:-
business|String|业务类型|
msgNums|String|未读消息数量|

### TerminalSimpleDto

字段名|类型|说明|备注
:-|:-:|:-|:-
clientId|	String	|	应用的clientId
appId	|String		|应用id，40位以内字符


### ClientTargetVo

字段名|类型|说明|备注
:-|:-:|:-|:-
clientId|	String	|	应用的clientId
appId	|String		|应用id，40位以内字符
userId	|String		|用户标识

### ClientTargetDto

字段名|类型|说明|备注
:-|:-:|:-|:-
clientId|	String	|	应用的clientId
appId	|String		|应用标识，40位以内字符
taskId|	String	|	消息批次标识
userId	|String		|用户标识


### MsgCategoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
businessType|	String	|消息类别
totalRecords	|Long		|该分类下的消息总数
unreadMsg|	MsgClientHistoryDto	|	未读消息内容
unreadMsgNum	|Long		|未读消息数
doNotDisturbDtos|	List<DoNotDisturbDto>	|	免打扰信息列表
latestMsgTime	|Date		|该分类下最新的消息时间，格式为：yyyy-MM-dd HH:mm:ss.SSS


### MsgBoUserDto

字段名|类型|说明|备注
:-|:-:|:-|:-
userId|	String	|	用户id
familyId	|String		|家庭Id
boId|	String	|业务对象Id
lastBoStatus	|Interger		|业务对象状态 0:new/1:read/2:deleted
createTime|	timeStamp	|	创建时间
updateTime	|timeStamp		|更新时间



## 消息推送模型说明

### 消息发送方数据模型说明

**模型定义**

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

**Notification**

> 定义通知的内容，此部分会由接收消息的App所在的操作系统或第三方推送通道（国内）自动展示为系统通知，通常会显示为标题和内容并显示App的图标，也可以由消息推送方进一步定义通知的展示方式包括播放通知音、震动、折叠显示等。

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
title|否|String|定义在所有系统使用通知标题，如果此项不填，则默认显示APP的名称
body|是|String|定义在所有系统使用通知的消息体


> 请注意：在android、ios中定义了title、body将在对应的系统通知中覆盖公共定义。

**Android**


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
极光推送|`https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#notification`中Android部分
FCM|`https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages`



**IOS**  

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
极光推送|`https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#notification`中IOS部分
FCM|`https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages`

**Data**  

> 定义实时消息的数据内容，此部分内容在App被唤醒或处于前台时，由第三方推送服务主动传递给App并由App来处理。此部分内容由云服务和App自行确定，但给U+ App及成套产品推送消息需遵循以下定义。  


以下为各属性的具体说明：  


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
body|是|Body|实时消息在app端的具体业务及展示方式，详见Body对象定义

**Body**  

以下为各属性的具体说明： 

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
view|否|View|实时消息的展示方式，详见View对象定义
extData|否|ExtData|实时消息的业务数据，详见ExtData对象定义
extParam|否|ExtParam|自定义业务数据    

**View**   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
showType|否|int|实时消息的显示样式：</br>-1：不显示，app一般用于无UI展示，直接处理消息内容；</br>0：toast；</br>2：弹框，业务事件及button按钮自定义见btns封装；
title|否|string|实时消息的弹框标题
content|否|string|实时消息的弹框内容 
btns|否|Button[]|弹框显示的按钮，详见Button对象定义  


**Button**   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
text|否|string|按钮的展示文本
callId|否|int|按钮事件调用id，用来标识此按钮单击事件在ExtData中需要的参数信息    

**ExtData**   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
expireTime|否|int|业务消息在本地系统的有效时间。若该值超出时间范围则在 APP 端视为无效消息，不进行业务处理。若为空或无此字段不处理
api|否|API|调用App端API，详见API对象定义
targetPage|否|String|目标页面地址，收到消息时App打开的目标页面
reviewPage|否|String|消息中心查看地址，若存在，消息中心点击跳转此页面查看内容（由业务定制此页面）
pages|否|Page[]|页面跳转，支持多个跳转，与Button中callId匹配，若相等代表该Button的目标页面
devControls|否|DevControl[]|设备控制，支持多个设备控制，与Button中callId匹配，若相等代表该Button的目标设备控制消息    


**Device**   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
typeId|否|string|设备typeid
deviceId|是|string|设备mac地址
deviceName|否|string|设备名称    

**DevControl**   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用，此时ExtData中的device对象数据不能为空，否则无法执行
deviceId|是|string|设备mac地址
groupName|否|string|组命令名称  
cmdList|是|json object|标准模型的命令键值对集合   

**API**  

> 自定义事件消息，执行API调用处理。APP端对此业务的处理逻辑：消息监听者或事件执行者收到此类消息时，执行相应API接口调用，并将携带的参数传至API接口内，若参数与API接口定义不符则失败。   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用 
apiType|是|string|api定义。如附录中的删除家庭处理为：DELEATE_FAMILY
params|否|json object|按钮在alert索引序号，由0开始。该API接口定义的入参集合
apiType|否|string|BSM业务中设置为UPDATE_BO_STATUS   



**Page**  

> 自定义事件消息，执行Page页面跳转处理。APP端对此业务的处理逻辑：消息监听者或事件执行者收到此类消息时，交由VDN 执行页面跳转处理。   

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
callId|否|int|调用者id。若为空或无此字段，则代表自动调用，否则根据Button 的callId响应
url|是|string|页面唯一地址。如是native页面，则需在VDN的DNS表中页面保持一致
params|否|int[]|参数键值对集合。页面跳转参数集合   



**bsms**  

>业务对象相关内容    

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
boCategory|是|int|业务对象类型，默认是0
boId|是|string|业务对象ID
boInfo|是|boInfo|业务对象信息
boStatus|是|int|业务对象状态 0新增   



**boInfo**  

>业务对象信息    

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
familyId|是|string|家庭ID
boName|是|string|业务对象名称
createdTime|是|date|创建时间   



**ExtParam**  

> 自定义业务数据，可自行扩展。  

以下为各属性的具体说明：


属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
actWithNotify|否|Boolean|消息附属在通知里有效。</br>true：收到通知后，同时处理内部附属的消息内容</br>false：收到通知后，待用户点击通知栏时处理UI业务


**Options**  

> 定义消息的设置信息，包括消息的名称、消息的类型等。 

以下为各属性的具体说明：

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgName|否|String|消息名字 如SCENE_BSM
businessType|是|int|消息类型，App端根据此分类进行消息展示。取值如下：</br>-1：不属于任何类型</br>0：系统类（系统类消息，例如推送升级，热修复等）</br>1：设备类（洗衣机、安防、菜谱分享等）</br>2：运营类（广告，运营等）</br>3：场景类</br>4：家庭类</br>5：活动类</br>6：服务提醒</br>7：交易物流</br>8：会员服务</br>9：在线客服</br>10：问题反馈</br>11：众播消息</br>未定义枚举值不允许私自使用
expires|否|int|消息在客户端离线时在第三方推送平台缓存时间，过期将不再推送给客户端。单位为秒，最长86400秒，如未指定则默认为86400秒。
priority|否|int|见priority备注
msgExpires|否|int|存储在历史消息中的消息过期时间，过期后在App消息中心将无法查询。单位为小时。取值如下：</br>-1：系统默认设置（1年后消息将被自动清除）；</br>0：立即过期；</br>大于0：过期时间(单位小时，不超过一年8760)
jiguangOptions|否|json object|见jiguangOptions备注

priority备注：  

消息优先级别，如设置则默认为1。取值为：</br>0 ：紧急消息，一般需要唤醒屏幕，播放消息提示音</br>1：一般消息，在屏幕黑屏时候，播放消息提示音，无需唤醒屏幕</br>2：中低消息，无需声音提示</br>3： 低级此类消息，可能接收消息后，APP 无需立刻处理，等系统空闲或者 wifi 状态下处理即可</br>以上优先级定义主要在实时消息中由App实现相关优先级的效果，不同优先级的实时消息如果同时定义了通知，则与通知的优先级对应关系为：</br>通知：高优先级 ←→实时消息：紧急</br>通知：普通优先级←→实时消息：一般，中低，低级
 
jiguangOptions备注：  

极光推送选项设置，请参考：</br>`https://docs.jiguang.cn/jpush/server/push/rest_api_v3_push/#options` </br>若使用极光推送在生产环境给ios端推送消息，该字段中必须包含apnsProduction，具体格式如下：
```
"jiguangOptions": {
				    "apnsProduction": true
			      }
```


**举例**  

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

**通知数据模型**  

> 通知的数据模型请参考如下第三方文档中对通知部分的相关说明。

**App集成极光推送接收消息**

安卓请参考：
  
`https://docs.jiguang.cn/jpush/client/Android/android_sdk/`  

IOS请参考：

`https://docs.jiguang.cn/jpush/client/iOS/ios_sdk/`  

说明：对于极光推送，发送方同时定义Notification和Data时，Data部分从通知的extras.content的内容，具体内容的模型请参照`实时消息普通数据模型`。


**App集成FCM接收消息**  

安卓请参考：  

`https://firebase.google.com/docs/cloud-messaging/android/client`  


IOS请参考：  

`https://firebase.google.com/docs/cloud-messaging/ios/client`


**实时消息普通数据模型**  

> 定义自定义消息的数据内容，此部分内容在App被唤醒或处于前台时，由第三方推送服务主动传递给App并由App来处理。   

以下为各部分的具体说明：  

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgId|是|String|消息的唯一标识，默认此属性值由消息推送服务自动填充
msgName|否|String|消息的名称，取值请参考Options中msgName定义，默认此属性值由消息推送服务自动根据Options中msgName填充，如发送方设置值将被覆盖
businessType|否，正常业务为必填，阅后即焚、空消息为非必填|int|消息分类，取值请参考Options中businessType定义，默认此属性值由消息推送服务自动根据Options中businessType填充
priority|否|int|默认此属性值由消息推送服务自动根据Options中priority填充，如果发送方设置数值则以发送方设置为准
uTraceId|否|String|链路跟踪uTraceId标识
uSpanId|否|String|链路跟踪uSpanId标识
body|是|Body|实时消息在app端的具体业务及展示方式，详见body章节各对象定义。  


需要注意：客户端收到的数据为base64编码后的数据，App需要对数据进行base64解码才能获取原始的json数据。

接收到的消息内容（FCM需要从返回内容的content属性取）举例：  

```
eyJtc2dJZCI6IjAwMDAwMDAwMDAxIiwibXNnTmFtZSI6IiIsImJvZHkiOiB7InZpZXciOiB7InNob3dUeXBlIjoyMSwJInRpdGxlIjoidGVzdCBtZXNzYWdlIiwiY29udGVudCI6IlRoaXMgaXMgYSB0ZXN0IG1lc3NhZ2UifSwiZXh0RGF0YSI6eyJpc01zZ0NlbnRlciI6MX19fQ==
```  
base64解密为：  

```
{"msgId":"00000000001","msgName":"","body": {"view": {"showType":21,"title":"test message","content":"This is a test message"}}}
```  

**阅后即焚数据模型**

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


**空业务数据模型**  

> 空业务消息是一种特殊的实时消息，本节定义空业务数据模型，当消息接收方收到空业务消息后无需处理，此消息主要用来更新之前阅后即焚消息。  

以下为各部分的具体说明：  

属性|是否必填|值类型|描述
:-:|:-:|:-:|:-
msgId|是|String|消息的唯一标识，默认此属性值由消息推送服务自动填充
msgName|是|String|消息的名称，取值为UPN_NULL 
msgType|是|int|消息的来源，取值：</br>3：状态类，此类消息由消息推送自动触发  （消息状态类，例如阅后即焚通知， 空消息）
body|是|Body|消息内容，内容为{}。  

需要注意：客户端收到的数据为base64编码后的数据。  




[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:../Message/_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:../Message/_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:../Message/_media/_MessagePush/MessagePush_flow.png

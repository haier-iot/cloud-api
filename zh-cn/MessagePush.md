
>**当前版本：** [UWS 消息推送服务标准版 V 3.0](zh-cn/ChangeLog/MessagePush)   
**更新时间：** {docsify-updated} 

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

### msgClientHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
taskId|String|消息任务ID|终端收到的msgId即ums的taskId
msgId|String|消息ID|
userId|String|用户ID|
appId|String|应用ID|
clientId|String|终端ID|
busineeType|String|业务类型|
message|UpMsg|消息模型|
msgStatus|Integer|消息发送状态|
readStatus|Integer|消息读取状态|
pushTime|DateTime|ums通道推送时间|


### MsgCloudHistoryDto

字段名|类型|说明|备注
:-|:-:|:-|:-
msgId|String|消息ID|
userId|String|用户ID|
appId|String|应用ID|
clientId|String|终端ID|
busineeType|String|业务类型|
messgae|UpMsg|消息模型|
msgStatusStatus|Integer|消息发送状态|
raadStatus|Integer|消息读取状态|
tag|String|标签|
pushTime|DateTime|ums消息推送时间|
retCode|String|返回码|

### DoNotDisturbDto

字段名|类型|说明|备注
:-|:-:|:-|:-
dndId|String|免打扰标识|
beginTime|Integer|开始时间|
endTime|Integer|结束时间|
businessType|Integer|消息业务类型|
priorities|Integer|消息业务类型|
poriorities|Integer|消息优先级|




## 终端功能接口列表

?>  使用REST接口的风格对外提供服务，仅支持HTTPS协议。</br>访问地址：`https://uws.haier.net/ums/v3`


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



#### 终端注销
> 注销已注册的终端信息</br>
> 一般在用户账号注销或APP卸载时滴啊用该接口


##### 1、接口定义
?> **接入地址：** `/account/logout`</br>
**HTTP Method：** POST

**输入参数：** 无参数输入

**输出参数：** 标准输出参数


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


### 设备模块

#### 设备终端免打扰
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


#### 取消设备终端免打扰
用户可以关闭免打扰功能


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
taskId|String|body|是|本次发送的任务标识|


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
messages|Message|body|是|推送消息内容定义
tag|String|body|否|标签。例如家庭推送时可以存入家庭
isBurn|Integer|body|否|是否阅后即焚

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|本次发送的任务标识


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
message|Message|body|是|推送消息内容定义
isBurn|Integer|body|否|是否是阅后即焚

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|本次发送的任务标识

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
messages|Message|body|是|推送消息内容定义
tag|String|body|否|标签。例如家庭推送时可以存入家庭
isBurn|Integer|body|否|是否阅后即焚
templateId|String|body|是|模板标识
templateParams|Map<String,string>|body|是|Map.Entry.key必须唯一

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|本次发送的任务标识

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
message|Message|body|是|推送消息内容定义
isBurn|Integer|body|否|是否是阅后即焚
templateId|String|body|是|模板标识
templateParams|Map<String,string>|body|是|Map.Entry.key必须唯一

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|本次发送的任务标识

### 根据taskId查询历史消息


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





[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:_media/_MessagePush/MessagePush_flow.png

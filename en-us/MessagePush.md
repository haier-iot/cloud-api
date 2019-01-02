
>**当前版本：** [UWS 消息推送服务标准版 V3.0.3](zh-cn/ChangeLog/MessagePush)   
**更新时间：** {docsify-updated} 

## 简介
为开发者提供基于物联网的消息推送服务，消息推送支持个性化的推送服务模式，开发者可以根据应用特性或者特定场景要求通过配置自定义消息的方式给App用户推送相关信息。
![消息推送标准版图片][MessagePush_type]

**消息通道注册**</br>
1、设备注册: 用户登录终端前，允许设备注册通道，具备接收推送能力；</br>
2、用户注册：用户登录终端后，赋予自己登陆（或授权）的设备具有业务消息分享和接收能力，必须向UMS注册服务。</br>

**获取用户注册设备列表**</br>
1、用户登录设备并且注册设备成功后，获取可推送消息的用户注册设备列表；</br>
2、APP server 在没有token clientId情况下获取用户注册设备列表。</br>

**端-端消息推送**</br>
消息从一个设备发送到另外一个设备的分享过程。
例如：馨厨将菜谱分享给手机

**云-端消息推送&**</br>
APP server等在未登陆情况对终端进行消息下发</br>

**信息状态查询** </br>
1、合法用户对消息状态查询；</br>
2、终端消息状态上报；</br>
3、消息终端显示后，消息已读/已处理等状态上报UMS；</br>  

### 应用场景
适用于物联网的消息推送服务，包括消息从一个设备终端发送到另一个设备终端，或者从应用服务端发送到设备终端。

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

### msgVlientHistoryDto

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
















**message示例**
```
海尔成套定义 message 业务大概分为三部分
1.msgName   2.msgType  3. body
{  
"msgName": "msg-name",
  "msgType": 3,
    body{
      "view": {  
         },       
        "extData":{
        }
      }
}
此部分格式以及内容通道不会校验，只是透传
```

**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据或null

**data示例**
```
data: {'msgId':”2345345345”}
msgId唯一标识一条消息，由服务端端产生服务端没收到一个推送消息请求，都会产生一个消息ID，用于标记一条用户消息
msgId产生原则:时间戳+消息源信息
```

##### 2、错误码
> msgPush：B00001、B00006、B00004、D00008、H32006、A00004、B00002、B00003
 

> msgPushtry:B00001、B00006、B00004、D00008、A00004、H32011、B00003、B00002


#### 终端信息状态查询接口

> 查询消息是否已经成功插入第三方平台，查询消息下发状态

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v2/getmsgstat`</br>
**HTTP Method：** POST </br>


**输入参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
megId|String|Body|必填|终端再推送完毕后服务端返回的消息ID

**输出参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据或null

##### 2、错误码

> B00001、B00004、H32007


#### 终端消息状态上报

> 查询信息，终端在收到消息之后必须调用此接口上报“消息已接收”状态

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v2/report`</br>
**HTTP Method：** POST </br>

**输入参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
appPackage|String|Body|必填|APP包名
msgId|String|Body|必填|终端在推送完毕后，服务端返回的data中的msgId
status|int|Body|必填|2：终端已接受</br>3：终端已展示</br>4：终端已反馈（参考massageInfo结构）

**输出参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据或null

##### 2、错误码

> B00001、H32005、B00004、A00004、D00008、H32007、H32008


#### 终端消息推送——设备绑定者

> 消息推送至设备绑定者</br>
> 【1】根据设备的deviceId发送信息到设备的在M2M系统绑定的主人的手机</br>
> 【2】主人的手机，即为注册消息通道时候，注册类型deviceType为“01：手机”的所有APP

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v2/msgPushToHost`</br>
**HTTP Method：** POST </br>

**输入参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
deviceId|String|Body|必填|绑定时设备的deviceId
encryptedData|String|Body|必填|APP通过USDK获取的bindkey，获取方式详见USDK部门提供的API文档
message|Json-obj|Body|必填|业务基础信息

**message示例**
```
例如: 海尔成套定义 message 业务大概分为三部分
1.msgName   2.msgType  3. body
{  
"msgName": "msgName",
  "msgType": 3,
    body{
      "view": {  
         },       
        "extData":{
        }
      }
}
此部分格式以及内容通道不会校验，只是透传
```

**输出参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据或null

##### 2、错误码

> B00001、B00006、B00004、H32009、H32010、A00004、H32011

### 云端业务接口

#### 云端获取用户已注册通道设备列表

> 获取当前账号下已经注册过消息通道的带屏设备列表


##### 1、接口定义

?> **接入地址：** `https://uws.haier.net/umse/v2/devlist`</br>
**HTTP Method：** POST </br>

**输入参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
userid|String|Body|必填|


**输出参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据

##### 2、错误码

> B00001、B00004、A00004


#### 云端消息推送

> APP server 调用直接下发到终端，数组一次最多支持发送200个


##### 1、接口定义

?> **接入地址：** `/umse/v2/msgPush`</br>
**HTTP Method：** POST </br>
**说明：**严格的参数校验，如果存在未注册，不合法的目的端，直接返回失败


?> **接入地址：** `/umse/v2/msgPushtry`</br>
**HTTP Method：** POST </br>
**说明：**最大限度发送消息，会进发送正确的目的端，存在未注册，非法的会主动过滤掉


**输入参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
expires|int|Body|非必填|消息有效期，单位秒
dst|Json-obj</br> {"type":0,"Id":["A","B"]} </br> {"type":1,"Id":["mac1","mac2"]}</br>{"type":2,"Id":["userId1","userId2"]"devType":["01","02"]}</br> {"type":3,"Id":["userId1","userId2"],"appType":["appPackage_A","appPackage_B"]}|Body|必填|type：0--Id为clientId，例如：A、B是目的端clientId;</br>type:1--Id为deviceId，例如：mac1、mac2是目的段的deviceId；</br>type:2，ID为userId,按照设备类型发送dstType</br>type:3,ID为userId，按照应用类型appType发送</br>数组上限200，userID不支持一次发给多个用户,参数合法判断（获取的类型值不在支持的设备列表中），参数合法的情况下UMS去除重复参数再去发送
message|Json-obj|Body|否|业务基础信息

**message说明**

```
例如: 海尔成套定义 message 业务大概分为三部分
1.msgName   2.msgType  3. body
{  
"msgName": "msg-name",
  "msgType": 3,
    body{
      "view": {  
         },       
        "extData":{
        }
      }
}
此部分格式以及内容通道不会校验，只是透传
```

**输出参数**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据

**data说明**

```
data: {'msgId' :”2345345345”}
msgId唯一标识一条消息，由服务端端产生服务端没收到一个推送消息请求，都会产生一个消息ID，用于标记一条用户消息
msgId产生原则:时间戳+消息源信息
```

##### 3、错误码

**msgPush**

>B00001、B00004、D00008、B00002、A00004、H32006、B00003

**msgPushtry**

> B00001、B00004、A00004、B00002、B00003

#### 云端信息状态查询接口

> 查询消息是否已经成功插入第三方平台


##### 1、接口定义

?> **接入地址：** `https://uws.haier.net/umse/v2/getmsgstat`</br>
**HTTP Method：** POST </br>

**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
msgId|String|Body|必填|终端在推送完毕后服务端返回的消息Id


**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|Json-obj|Body|是|返回数据或null

##### 2、错误码

> B00001、B00004、H32007


[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png


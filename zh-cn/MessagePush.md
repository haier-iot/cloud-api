!> 待完善


!>**当前版本：** [消息推送服务标准版v3.0.3]()   
**更新时间：** 2018-07-19 

## 简介
为开发者提供基于物联网的消息推送服务，消息推送支持个性化的推送服务模式，开发者可以根据应用特性或者特定场景要求通过配置自定义消息的方式给App用户推送相关信息。
![消息推送标准版图片][MessagePush_type]

### 名词解释

### 功能介绍
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

## 公共结构说明
### UserRegInfo
用户注册信息

参数名|类型|说明|备注
:-|:-:|:-|:-
userId|String|账号登陆后返回的userId|
clientId|String|带屏设备终端的唯一标识|如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址。
deviceName|String|设备名称|设备昵称
pushId|String|推送标识|第三方终端SDK产生的用于区分每个终端的唯一标识，例如极光是regID
deviceType|String|终端类型|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
typeId|String|设备类型编码|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
appPackage|String|终端标识|依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名，IOS = Bundle ID，Linux =服务名称，Window = 服务名称。</br>为避免重复，接入前需要和UMS做好沟通
regTime|String|注册时间|第一次正确注册时的时间
status|int|状态值。</br>1：已注册，</br>2：已注销，</br>3：已更新|IF 1 -> 3 ;</br> IF 2 -> 1 ; </br>IF 3 -> 3
collab3th|int|当前合作方通道类型|0：极光 ；</br>1：haier-M2M

### MessageInfo
消息信息

参数名|类型|说明|备注
:-|:-:|:-|:-
mesgId|String|消息ID|平台产生的消息ID
userId|String|用户登录系统产生的|
revTime|String|平台接收消息时间|
pushTime|String|将消息下发的时间|
status|int|0：发送失败，</br>1：发送成功，</br>2：终端已接收，</br>3：终端已展示，</br>4：终端已反馈，</br>5：取消展示，</br>6：消息已更新|0：UMS发送失败,尚未发送 , </br>1：UMS发送成功 ,</br> 2: 终端接收消息后，上报状态 , </br>3: 终端展示消息后，上报状态, </br> 4：终端反馈消息后，上报状态 ,</br> 5: UMS针对这条消息下发通知【阅后即焚】 , </br>6：UMS已经将这条消息更新为 【空消息】


## 接口清单

### 消息通道注册
#### 设备初始化注册消息通道
> 按设备注册消息通道

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/deviceReg`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
clientId|String|长度为1到100|Body|必填|带屏设备终端的唯一标识，</br>建议：如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址，并且尽量与header中保持统一
pushId|String|长度为1到100|Body|必填|极光返回的推送id
deviceName|String|长度为1到100|Body|非必填|设备昵称
deviceType|String|长度为1到100|Body|必填|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
typeId|String|长度为1到64|Body|非必填|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
collab3th|int|长度为1|Body|必填|0：极光 ；</br>1：haier-M2M

**输出参数:** 输出标准应答参数

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/deviceReg
Body
{
	"clientId":"********",
	"pushId":"pushId",
	"deviceType":"01",
	"appPackage":"com.a.b",
	"collab3th":0
}
```
**请求应答**
```
{
	"retCode":"00000",
	"retInfo":"success"
}
```

##### 3、请求错误码




#### 用户登录后注册消息通道
> 用户登录成功后，注册消息通道

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/userReg`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
clientId|String|长度为1到100|Body|必填|带屏设备终端的唯一标识，</br>建议：如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址，并且尽量与header中保持统一
pushId|String|长度为1到100|Body|必填|极光返回的推送id
deviceName|String|长度为1到100|Body|非必填|设备昵称
deviceType|String|长度为1到100|Body|必填|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
typeId|String|长度为1到64|Body|非必填|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
appPackage|String|长度1到100|Body|是|终端标识，依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名</br>IOS = Bundle ID</br>Linux =服务名称</br>Window = 服务名称</br>为避免重复，接入前需要和UMS做好沟通
collab3th|int|长度为1|Body|必填|0：极光 ；</br>1：haier-M2M


**输出参数:** 输出标准应答参数

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/userReg
{
    "clientId":"******",
    "pushId":"pushId",
    "deviceType":"01",
    "appPackage":"com.a.b",
    "collab3th":0
}
```
**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success"
}
```

##### 3、请求错误码



#### 用户注销消息通道
> 当设备不再需要推送功能或者此设备授权已经移交给他人，可注销消息通道

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/unRegister`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
clientId|String|长度为1到100|Body|必填|带屏设备终端的唯一标识，</br>建议：如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址，并且尽量与header中保持统一
appPackage|String|长度1到100|Body|是|终端标识，依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名</br>IOS = Bundle ID</br>Linux =服务名称</br>Window = 服务名称</br>为避免重复，接入前需要和UMS做好沟通

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/unRegister
{
    "clientId":"****",
    "appPackage":"com.a.b"
}

```
**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success"
}
```
##### 3、请求错误码


#### 用户注销消息通道
> 当设备不再需要推送功能或者此设备授权已经移交给他人，可注销消息通道

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/unRegister`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
clientId|String|长度为1到100|Body|必填|带屏设备终端的唯一标识，</br>建议：如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址，并且尽量与header中保持统一
appPackage|String|长度1到100|Body|必填|终端标识，依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名</br>IOS = Bundle ID</br>Linux =服务名称</br>Window = 服务名称</br>为避免重复，接入前需要和UMS做好沟通

**输出参数:** 输出标准应答参数

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/unRegister
{
    "clientId":"****",
    "appPackage":"com.a.b"
}
```

**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success"
}
```

##### 3、请求错误码


### 获取用户注册设备列表

#### 获取用户注册消息通道的设备列表
> 获取当前账号下已经注册过消息通道的带屏设备列表

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/deviceList`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
appPackages|String[]|数组最大支持200个|Body|非必填|app包名

**输出参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
deviceList|UserRegInfo[]|Body|非必填|用户注册设备列表

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/deviceList
{
    "appPackage":"com.a.b"
}
```

**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success",
   "deviceList":[]
}
```

##### 3、请求错误码

### 端-端消息推送

#### 按设备向自己发送消息
> 用户按设备发送消息，可指定发送给某几个app

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/msg/pushByDev</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
deviceId|String[]|数组最大支持200个|Body|必填|设备标识
deviceType|String[]|数组最大支持200个|Body|非必填|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
appPackages|String[]|数组最大支持200个|Body|非必填|app包名

**输出参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
msgId|String|Body|必填|消息ID

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/msg/pushByDev
{
    "isTry": 1,
    "priority": 0, 
    "expires": 3600,
    "isLong": 0,
    "message": {},
    "deviceIds": ["dev001","dev002"],
    "deviceTypes": [ "01"],
    "appPackages": ["com.a.b"]
}
```

**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success",
   "msgId":"xxx"
}
```

##### 3、接口错误码


#### 按终端向自己发送消息
> 用户按终端类型发送消息

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/msg/pushByDevType</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
deviceType|String[]|数组最大支持200个|Body|非必填|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱


**输出参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
msgId|String|Body|必填|消息ID

##### 2、请求样例
 
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/msg/pushByDevType
{
    "isTry": 1,
    "priority": 0,
    "expires": 3600,
    "isLong": 0,
    "message": {},
    "deviceTypes": [ "01" ]
}
```

**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success",
   "msgId":"xxx"
}
```

##### 3、接口错误码


#### 按APP向自己发送消息
> 用户指定发送给某几个app

##### 1、接口定义
?> **接入地址：** `https://uws.haier.net/ums/v3/msg/pushByApp</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
appPackages|String[]|数组最大支持200个|Body|非必填|app包名


**输出参数**

参数名|类型|取值范围|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
msgId|String|Body|必填|消息ID

##### 2、请求样例
**请求样例**
```
请求地址：https://uws.haier.net/ums/v3/msg/pushMsgByApp
{
    "isTry": 1,
    "priority": 0,
    "expires": 3600,
    "isLong": 0,
    "message": {},
    "appPackages": ["com.a.b"]
}

```

**请求应答**
```
{
  "retCode":"00000",
  "retInfo":"success",
   "msgId":"xxx"
}
```

##### 3、接口错误码


### 应用场景

## 使用方式

### 开通流程
![开通流程][MessagePush_liucheng]

### 应用场景
适用于物联网的消息推送服务，包括消息从一个设备终端发送到另一个设备终端，或者从应用服务端发送到设备终端。



[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png


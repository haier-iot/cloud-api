!> **当前版本：** [消息推送服务标准版v3.0.3][MessagePush_document_url]</br>
**日期：** 2018-07-19 

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
**UserRegInfo：** 用户注册信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
userId|String|账号登陆后返回的userId|
clientId|String|带屏设备终端的唯一标识|如果能直接从uSDK中获取则需要从uSDK中获取；</br>如果没有uSDK，则可以取设备mac地址。
deviceName|String|设备名称|设备昵称
pushId|String|推送标识|第三方终端SDK产生的用于区分每个终端的唯一标识，例如极光是regID
deviceType|String|终端类型|01：手机，02：平板电脑 ，03：电视 ，04：带屏冰箱 ，</br>05：带屏烟机 ，06：SmartCenter，07：魔镜 ，08：智能音箱
typeId|String|设备类型编码|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
appPackage|String|终端标识|依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名，IOS = Bundle ID，Linux =服务名称，Window = 服务名称。</br>为避免重复，接入前需要和UMS做好沟通
regTime|String|注册时间|第一次正确注册时的时间
status|int|状态值。</br>1：已注册，2：已注销，3：已更新|IF 1 -> 3 ; IF 2 -> 1 ; IF 3 -> 3
collab3th|int|当前合作方通道类型|0：极光 ； 1：haier-M2M

**MessageInfo：** 消息信息

参数名|类型|说明|备注
:-|:-:|:-|:-
mesgId|String|消息ID|平台产生的消息ID
userId|String|用户登录系统产生的|
revTime|String|平台接收消息时间|
pushTime|String|将消息下发的时间|
status|int|0：发送失败，</br>1：发送成功，</br>2：终端已接收，</br>3：终端已展示，</br>4：终端已反馈，</br>5：取消展示，</br>6：消息已更新|0：UMS发送失败,尚未发送 , </br>1：UMS发送成功 ,</br> 2: 终端接收消息后，上报状态 , </br>3: 终端展示消息后，上报状态, </br> 4：终端反馈消息后，上报状态 ,</br> 5: UMS针对这条消息下发通知【阅后即焚】 , </br>6：UMS已经将这条消息更新为 【空消息】


## 接口清单

### 设备初始化注册消息通道
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
deviceType|String|长度为2|Body|必填|01：手机，02：平板电脑 ，03：电视 ，04：带屏冰箱 ，</br>05：带屏烟机 ，06：SmartCenter，07：魔镜 ，08：智能音箱
typeId|String|长度为1到64|Body|非必填|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
collab3th|String|长度为1到64|Body|必填|当前合作方通道,</br>0：极光 ； 1：haier-M2M




### 应用场景

## 使用方式

### 开通流程
![开通流程][MessagePush_liucheng]

### 应用场景
适用于物联网的消息推送服务，包括消息从一个设备终端发送到另一个设备终端，或者从应用服务端发送到设备终端。

## 文档资料
[消息推送服务标准版v3.0.3][MessagePush_document_url]


[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png


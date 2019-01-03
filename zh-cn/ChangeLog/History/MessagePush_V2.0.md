
>**当前版本：** [UWS 消息推送服务标准版 V 2.0](zh-cn/ChangeLog/MessagePush)   
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
### UserRegInfo
用户注册信息

参数名|类型|说明|备注
:-|:-:|:-|:-
userId|String|账号登陆后返回的userId|
userName|String|用户名|
clientId|String|海尔带屏终端设备的唯一标识|由U+SDK产生如U+APP登录使用的clientId</br>PS：如是M2M通道，建议此值填写 deviceId即可。
deviceId|String|带屏设备终端的唯一标识|如设备的mac地址</br>PS:如是M2M通道，建议填写deviceId即可
deviceName|String|设备名称|设备昵称
pushId|String|推送标识|第三方终端SDK产生的用于区分每个终端的唯一标识，例如极光是regID
deviceType|String|终端类型|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
typeId|String|设备类型编码|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
appPackage|String|终端标识|依此来对应推送第三方APPID相关重要推送参数信息</br>Android =包名，IOS = Bundle ID，Linux =服务名称，Window = 服务名称。</br>为避免重复，接入前需要和UMS做好沟通
regTime|String|注册时间|第一次正确注册时的时间
status|int|状态值。</br>1：已注册，</br>2：已注销，</br>3：已更新|IF 1 -> 3 ;</br> IF 2 -> 1 ; </br>IF 3 -> 3
collab3th|int|当前合作方通道类型|0：极光 ；</br>1：haier-M2M</br>当前版本仅智能音箱支持此通道

### MessageInfo
消息信息

参数名|类型|说明|备注
:-|:-:|:-|:-
mesgId|String|消息ID|平台产生的消息ID
userId|String|用户登录系统产生的|
revTime|String|平台接收消息时间|
pushTime|String|将消息下发的时间|
status|int|0：发送失败，</br>1：发送成功，</br>2：终端已接收，</br>3：终端已展示，</br>4：终端已反馈，</br>5：取消展示，</br>6：消息已更新|0：UMS发送失败,尚未发送 , </br>1：UMS发送成功 ,</br> 2: 终端接收消息后，上报状态 , </br>3: 终端展示消息后，上报状态, </br> 4：终端反馈消息后，上报状态 ,</br> 5: UMS针对这条消息下发通知【阅后即焚】 , </br>6：UMS已经将这条消息更新为 【空消息】


## 接口定义
### 终端业务接口

#### 用户设备通道消息注册

> 通道注册用于发送接收业务消息</br>
此函数接口UAG安全过滤，如果是UGW（如智能音箱）clientId（包括header中clientId）、deviceId、pushId统一填写为deviceId即可

APP在如下两种情况下需要注册消息通道：

1、APP初始化：APP安装后进行初始化时，需注册消息通道，必填参数：clientId , deviceId, pushId；

2、登录APP：用户登录APP后，需再次注册消息通道，必填参数：userId, clientId , deviceId, pushId；


##### 1、接口定义
?> **接入地址：** `/ums/v2/register`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
userId|String|Body|非必填|1、APP初始化时机型通道注册userId为空</br>2、用户登陆APP时进行通道注册userId不为空</br>PS:如果填写userId，纳闷系统校验此userId与消息头的token是否匹配
userName|String|Body|非必填|
clientId|String|Body|必填|clientid丢失或变更后是否需要立即重新注册 PS：此值与消息头clientId一致
deviceId|String|Body|非必填|能获取deviceId则填写，如冰箱；否则不填写，如手机
pushId|String|Body|必填|
deviceName|String|Body|非必填|设备昵称
deviceType|String|Body|必填|01：手机，</br>02：平板电脑 ，</br>03：电视 ，</br>04：带屏冰箱 ，</br>05：带屏烟机 ，</br>06：SmartCenter，</br>07：魔镜 ，</br>08：智能音箱
typeId|String|Body|非必填|设备类型码(长串)，手机没有可以填空</br>PS：APP获取的设备类型码
collab3th|int|长度为1|Body|必填|0：极光 ；</br>1：haier-M2M</br>当前版本仅智能音箱支持此通道

**输出参数:** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据

##### 2、请求错误码

> B00001、00004、D00008、H32004、H32005、C00006



#### 用户设备消息通道注销
> 当设备不再需要推送功能或者此设备授权已移交他人

##### 1、接口定义
?> **接入地址：** `/ums/v2/unreg`</br>
**HTTP Method：** POST

**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
appPackage|String|Body|必填|

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据

##### 2、错误码
>B00001、H32005、B00004、A00004、D00008、H32006

#### 终端获取用户已注册通道设备列表
> 获取当前账号下已经注册过消息通道的带屏设备列表

##### 1、接口定义
?> **接入地址：** `/ums/v2/devlist`</br>
**HTTP Method：** POST

**输入参数：**  无，填写空字符串

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|返回信息
data|String|Body|是|返回数据

##### 2、错误码
> B00004、A00004

#### 终端消息推送
> dst 中 type：0，到端的消息推送，不限于当前账户；</br>
> dst 中 type：1，2，3，一个用户的多屏互动场景。

##### 1、接口定义
?> **接入地址：** `/ums/v2/msgPush`</br>
**HTTP Method：** POST </br>
**说明：** 严格的参数校验，如果存在未注册，不合法的目的端，直接返回失败

?> **接入地址：** `/ums/v2/msgPushtry`</br>
**HTTP Method：** POST </br>
**说明：** 最大限度发送消息，会进发送正确的目的端，存在未注册，非法的会主动过滤掉

**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
expires|int|Body|非必填|消息有效期，单位秒
dst|Json-obj</br> {"type":0,"Id":["A","B"]} </br> {"type":1,"Id":["mac1","mac2"]}</br>{"type":2,"devType":["01","02"]}</br> {"type":3,"appType":["appPackage_A","appPackage_B"]}|Body|必填|type：0--Id为clientId，例如：A、B是目的端clientId;</br>type:1--Id为deviceId，例如：mac1、mac2是目的段的deviceId；</br>type:2按照设备类型发送，例如：用户发给用户下是当前用户下（header中token）的appPackage_A、appPackage_B</br>数组上限200，userID不支持一次发给多个用户,参数合法判断（类型值不在支持的设备列表中），参数合法的情况下UMS去除重复参数再去发送
message|Json-obj|Body|否|业务基础信息

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
?> **接入地址：** `/ums/v2/getmsgstat`</br>
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
?> **接入地址：** `/ums/v2/report`</br>
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
?> **接入地址：** `/ums/v2/msgPushToHost`</br>
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

?> **接入地址：** `/umse/v2/devlist`</br>
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

#### 云端信息状态查询接口

> 查询消息是否已经成功插入第三方平台


##### 1、接口定义

?> **接入地址：** `/umse/v2/getmsgstat`</br>
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
data|String|Body|是|返回数据


[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png


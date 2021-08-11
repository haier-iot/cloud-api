

# 场景推荐服务



## 删除推荐的场景

> 基本信息

?> **接入地址：** `/scs/recommend/delete/templates`</br>

**HTTP Method：** POST

**接口描述**

```
入参：
{
"familyId":"aaa1",
  "templateIdList":["2107021449361435003445760001003"]
}
出参：
{"payload":{"deletedTemplateIds":["2107021549292335003445760001009","2107021549578105003445760001010"]},"retCode":"00000","retInfo":"成功"}

```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|

**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
familyId|string|非必须|&nbsp;|&nbsp;|&nbsp;|
templateIdList|string[]|非必须|&nbsp;|&nbsp;|item 类型: string|

> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
payload|object	|非必须|&nbsp;|&nbsp;	|&nbsp;|
``` ```├─ deletedTemplateIds|string[]|非必须|&nbsp;|&nbsp;|item 类型: string|
retCode|string	|非必须|&nbsp;|&nbsp;	|&nbsp;	|
retInfo|string	|非必须|&nbsp;|&nbsp;	|&nbsp;	|

## 推荐场景重命名


> 基本信息

?> **接入地址：** `/scs/recommend/patch/scene-name`</br>

**HTTP Method：** POST

**接口描述**

```
入参：
{
  "familyId":"aaa1",
  "templateId":"2107021449361435003445760001003",
  "sceneName":"hahaha",
  "userId":"456456456"
}
出参：
{"payload":{"templateId":"2107021449361435003445760001003"},"retCode":"00000","retInfo":"成功"}
```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
familyId|string|非必须|&nbsp;|&nbsp;|&nbsp;|
templateId|string|非必须|&nbsp;|&nbsp;|&nbsp;|
sceneName|string|非必须|&nbsp;|&nbsp;|&nbsp;|
userId|string|非必须|&nbsp;|&nbsp;|&nbsp;|

> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
payload|object	|非必须|&nbsp;|&nbsp;	|&nbsp;|
``` ```├─ templateId|string|非必须|&nbsp;|&nbsp;|&nbsp;|
retCode|string	|非必须|&nbsp;|&nbsp;	|&nbsp;	|
retInfo|string	|非必须|&nbsp;|&nbsp;	|&nbsp;	|

## 更新推荐场景


> 基本信息

?> **接入地址：** `/iftttscene/scene/update/recommend-scene`</br>

**HTTP Method：** POST

**接口描述**


> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/x-www-form-urlencoded|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|示例|备注|
:-|:-:|:-:|:-:|:-
userSceneDto|text|是|UserSceneDto|UserSceneDto|

> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
retCode	|string|必须|&nbsp;|retCode|&nbsp;|
retInfo	|string|必须|&nbsp;|retInfo|&nbsp;|
data	|string|必须|&nbsp;|data|&nbsp;|



## 查询家庭下推荐场景数和已启用数

> 基本信息

?> **接入地址：** `/scs/recommend/get/count`</br>

**HTTP Method：** POST

**接口描述**

```
入参：
{
  "familyId":"aaa1"
}
出参：
{"payload":{"recommendCount":7,"startupCount":2},"retCode":"00000","retInfo":"成功"}

```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
familyId|string|非必须|&nbsp;|&nbsp;|&nbsp;|


> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
payload|object|非必须|&nbsp;|&nbsp;|&nbsp;|
``` ```├─ recommendCount|number|非必须|&nbsp;|&nbsp;|&nbsp;|
``` ```├─ startupCount|number|非必须|&nbsp;|&nbsp;|&nbsp;|
retCode|string|非必须|&nbsp;|&nbsp;|&nbsp;|
retInfo|string|非必须|&nbsp;|&nbsp;|&nbsp;|


## 查询推荐场景列表

> 基本信息

?> **接入地址：** `/scs/recommend/list/template`</br>

**HTTP Method：** POST

**接口描述**

```
入参：
{
  "familyId":"aaa1"
}
出参：
{"retCode":"00000","retInfo":"成功","payload":[{"recommendType":"update","id":"2107211316394122256365370004575","templateId":"2107211316394122256365370004576","recSystemId":"MB-SDSJZDFW-0000","type":"ifttt","userId":"53566867","familyId":"640167158892000000","sceneName":"回家","sceneAlias":"回家","sceneDesc":"回家后可一键实现开灯，关窗帘，安防设备（门磁窗磁、红外探测器）撤防，打开空调、空气净化器、新风机、热水器、电视、智能开关、智能插座等。也可通过语音来实现上述功能","isOpen":1,"rules":[{"id":"21072113163941222563653700045750","rule":"回家31","salience":1,"when":{"conditions":null},"then":{"actions":[{"id":"2107211328409645443938200006252","type":"DeviceControl","dealyTime":0,"control":{"args":[{"name":{"id":"582bc0572bd343bea0f6989bcd22540e","value":"onOffStatus","desc":"开关机状态","required":false},"value":{"value":"true","desc":"开机","required":true}}],"componentId":"681d5629bad54163b8a93d7f9f6bdf53","object":{"value":"D058C039D556,D058C039D557","required":false}},"isOpen":1,"sceneId":"2107211316394122256365370004575","componentType":"PRODTYPE","priority":0},{"recommendType":"delete","id":"2107211328409655443938200006253","type":"DeviceControl","dealyTime":0,"control":{"args":[{"name":{"id":"77961bd5673c4c46ae8b8804501da9b9","value":"onOffStatus","desc":"开关机状态","required":false},"value":{"value":"true","desc":"开机","required":false}}],"componentId":"8cb51120b4614bb7b22856628def2902","object":{"value":"002592217E86","required":false}},"isOpen":1,"sceneId":"2107211316394122256365370004575","componentType":"MODEL","priority":0},{"id":"2107211328409675443938200006254","type":"Delayed","delayControl":{"args":[{"name":{"id":"bf8c29bf17b84815902f3b220161d367","value":"delay","desc":"延时","required":false},"value":{"id":"3","value":"3","required":false}}],"componentId":"eeb6cc120d3a4a45adb1952ffb713a1a"},"isOpen":1,"sceneId":"2107211316394122256365370004575","priority":0,},{"recommendType":"update","id":"2107211328409685443938200006255","type":"DeviceControl","dealyTime":0,"control":{"args":[{"name":{"id":"2cfa091b1b6b86d9d670ee15d5cc8923","value":"operationMode","desc":"功能模式","required":false},"value":{"value":"1","oldValue":"0","desc":"制冷","oldDesc":"制热","required":false}}],"componentId":"8cb51120b4614bb7b22856628def2902","object":{"value":"002592217E86","required":false}},"isOpen":1,"sceneId":"2107211316394122256365370004575","componentType":"MODEL","priority":0},{"recommendType":"add","id":"2107211328409695443938200006256","type":"DeviceControl","dealyTime":0,"control":{"args":[{"name":{"id":"fb3fb238a2854883ba1110908fe683c5","value":"onOffStatus","desc":"开关状态","required":false},"value":{"value":"true","desc":"开","required":false}}],"componentId":"fe1e9a2657fdf8fcc8d135cab365c795","object":{"value":"2C6B7D4D747501090711","required":false}},"isOpen":1,"sceneId":"2107211316394122256365370004575","componentType":"MODEL","priority":0}]},"isOpen":1,"sceneId":"2107211316394122256365370004575"}],"createTime":"2021-07-21 13:16:39","updateTime":"2021-07-21 13:29:34","version":"0.6","engineVersion":"V24","systemId":"SV-UZHSH-0000","sceneEnableStatus":1,"sourceId":"52f035739a1243199b4c87acef558d3c","triggerType":"manually","appId":"MB-UZHSH-0001","tagList":[{"id":"1910310148484881616990600010240","name":"全屋"}],"taskInfoList":[{"cron":{"minutes":"","hours":"","day":"","month":"","week":"","year":""},"activeBeginTime":"","activeEndTime":"","status":""}],"aiKeyword":"HomeScene","createType":"User","status":"publish","sceneStatus":1,"auto":false,"sceneLocation":"cloud"}]}

```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
familyId|string|非必须|&nbsp;|&nbsp;|&nbsp;|

> 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> retCode</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> retInfo</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> payload</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-0><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> recommendType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-1><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-2><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> templateId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-3><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> recSystemId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-4><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> type</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-5><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> userId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-6><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> familyId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-7><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneName</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-8><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneAlias</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-9><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneDesc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-10><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> rules</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> rule</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-2><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> salience</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-3><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> when</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-3-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> conditions</span></td><td key=1><span>null</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> then</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> actions</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-0><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-1><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> type</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-2><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> dealyTime</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> control</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> args</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-3-0-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-3><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> scope</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-3-0><td key=0><span style="padding-left: 160px"><span style="color: #8c8a8a">├─</span> type</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-3-1><td key=0><span style="padding-left: 160px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-4><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-3><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> oldValue</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-4><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> oldDesc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-1><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> componentId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> object</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-4><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-5><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> sceneId</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-6><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> componentType</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-7><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> priority</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> recommendType</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> delayControl</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> args</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-9-0-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-0-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-0-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-0-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-0-3><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-1-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-1-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-0-1-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9-1><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> componentId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-5><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-6><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> sceneId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-12><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> createTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-13><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> updateTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-14><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> version</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-15><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> engineVersion</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-16><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> systemId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-17><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneEnableStatus</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-18><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sourceId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-19><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> triggerType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-20><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> appId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-21><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> tagList</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-21-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-21-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> taskInfoList</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-22-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> cron</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> minutes</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-1><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> hours</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-2><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> day</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-3><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> month</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-4><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> week</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-5><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> year</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> activeBeginTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-2><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> activeEndTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-3><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> status</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-23><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> aiKeyword</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-24><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> createType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-25><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> status</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-26><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneStatus</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-27><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> auto</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-28><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneLocation</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr>
               </tbody>
              </table>



## 查询推荐场景详情



> 基本信息

?> **接入地址：** `/scs/recommend/find/template-info`</br>

**HTTP Method：** POST

**接口描述**

```
sceneId和templateId至少有一个，优先级templateId>sceneId

```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
sceneId|string|必须|&nbsp;|&nbsp;|&nbsp;|
templateId|string|必须|&nbsp;|&nbsp;|&nbsp;|
familyId|string|必须|&nbsp;|&nbsp;|&nbsp;|

> 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> retCode</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> retInfo</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> payload</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-0><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> recommendType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-1><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-2><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> templateId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-3><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> recSystemId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-4><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> type</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-5><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> userId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-6><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> familyId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-7><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneName</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-8><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneAlias</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-9><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneDesc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-10><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> rules</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> rule</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-2><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> salience</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-3><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> when</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-3-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> conditions</span></td><td key=1><span>null</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> then</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> actions</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-0><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-1><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> type</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-2><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> dealyTime</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> control</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> args</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-3-0-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-0-3><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>object</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-0-1-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-1><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> componentId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> object</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-3-2-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-4><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-5><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> sceneId</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-6><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> componentType</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-7><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> priority</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> delayControl</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> args</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-11-4-0-8-0-0><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-0-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-0-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-0-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> desc</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-0-3><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-1><td key=0><span style="padding-left: 120px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-1-0><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-1-1><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> value</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-0-1-2><td key=0><span style="padding-left: 140px"><span style="color: #8c8a8a">├─</span> required</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-8-1><td key=0><span style="padding-left: 100px"><span style="color: #8c8a8a">├─</span> componentId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-4-0-9><td key=0><span style="padding-left: 80px"><span style="color: #8c8a8a">├─</span> recommendType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-5><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> isOpen</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-11-6><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> sceneId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-12><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> createTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-13><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> updateTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-14><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> version</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-15><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> engineVersion</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-16><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> systemId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-17><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneEnableStatus</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-18><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sourceId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-19><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> triggerType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-20><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> appId</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-21><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> tagList</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-21-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> id</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-21-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> taskInfoList</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2-22-0><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> cron</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-0><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> minutes</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-1><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> hours</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-2><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> day</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-3><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> month</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-4><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> week</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-0-5><td key=0><span style="padding-left: 60px"><span style="color: #8c8a8a">├─</span> year</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-1><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> activeBeginTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-2><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> activeEndTime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-22-3><td key=0><span style="padding-left: 40px"><span style="color: #8c8a8a">├─</span> status</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-23><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> aiKeyword</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-24><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> createType</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-25><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> status</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-26><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneStatus</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-27><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> auto</span></td><td key=1><span>boolean</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr><tr key=0-2-28><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> sceneLocation</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr>
               </tbody>
              </table>


## 确认推荐场景

> 基本信息

?> **接入地址：** `/iftttscene/scene/update/recommend-confirm`</br>

**HTTP Method：** POST

**接口描述**

> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/x-www-form-urlencoded|是|&nbsp;|&nbsp;|


**Body** 

名称|类型|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
sceneId|text|必须|&nbsp;|场景ID|
sourceId|text|必须|&nbsp;|属性id|
familyId|text|必须|&nbsp;|属性id|
sceneConfirm|text|必须|&nbsp;|属性id|

> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
retCode|string|必须|&nbsp;|retCode|&nbsp;|
retInfo|string|必须|&nbsp;|retInfo|&nbsp;|
data|object|必须|&nbsp;|data|&nbsp;|

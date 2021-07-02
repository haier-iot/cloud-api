# 用户场景管理

!> **更新时间**：{docsify-updated}  




## 修改场景昵称

**使用说明**

>修改场景昵称（别名）。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateAppSceneAlias`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| sceneId| String |32| Body| 必填|场景id|  
| basicSceneAlias| String|64| Body| 必填|基础场景昵称|   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String | Body  |  必填 |  &emsp;  |
|  retInfo  |  String | Body  |  必填 |  &emsp; |
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |

**示例**  

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 14a5736f619376ffad7d539e4540e644c711a39996ff2f4acb396d99bd3586cd
timestamp: 1542356687190 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"sceneId": "56240541ee1848e69b672d42303b037e", "appSceneAlias":"jiayk001" 
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 



## 删除用户下载的场景

**使用说明**

>删除用户下载的场景。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/deleteAppScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭id|  
| sceneIds| String[]|32| Body| 必填|要删除的场景id数组|  
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null |


**示例**    

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: e7ff69d158c8faf7db9bd90b61571395e96dc4f0b3689a295c08593b830a8f65
timestamp: 1542357835611 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "191092663982000000",
	"appId": "MB-****-0001",
	"sceneIds": ["61b830e6fc3940daaca7c487ce7f288c"]
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 



## 用户创建平台触发类场景

**使用说明**

>根据用户填写的参数保存场景

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/addUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT/IFTTT_jsonDataStructure)|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |


**示例**  

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 8a4447bead76f47f85580b1291f36c6c59ca84415307f4ca89d1ffa3cae0b11d
timestamp: 1542614635624 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "userSceneDto": {
    "auto": false,
    "familyId": "717042585118000000",
    "isOpen": 0,
    "rules": [
      {
        "then": {
          "actions": [
            {
              "control": {
                "args": [
                  {
                    "name": {
                      "id": "5cb7157b972b47648fd48cadd2b03380",
                      "required": true
                    },
                    "value": {
                      "desc": "低风",
                      "required": true,
                      "value": "3"
                    }
                  }
                ],
                "componentId": "fd1519209d7c11e88943fa163eb273a5",
                "object": {
                  "required": false,
                  "value": "DC330D630E49"
                },
                "operation": {
                  "desc": "设置风速",
                  "id": "ff58285a9e184eacb7a84d5a9e643aef",
                  "required": false
                }
              },
              "dealyTime": 0,
              "type": "DeviceControl"
            }
          ]
        },
        "when": {
          "conditions": [
            {
              "componentId": "fd1519209d7c11e88943fa163eb273a5",
              "desc": "开关机状态设置为等于开机",
              "key": {
                "id": "bd0ebf1efb5f45f18beced92ebcec529",
                "required": true
              },
              "object": {
                "desc": "除湿机1",
                "required": false,
                "value": "DC330D630E49"
              },
              "operationSign": "equal",
              "value": {
                "desc": "开机",
                "required": true,
                "value": "true"
              }
            }
          ]
        }
      }
    ],
    "sceneDesc": "中国版123",
    "sceneName": "中国版123",
    "userId": "5458199"
  }
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": "6aa9a62f720d460da7293d7fc40453aa"
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 


## 用户创建手动触发类场景

**使用说明**

>根据用户填写的参数保存场景。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/addUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT/IFTTT_jsonDataStructure)|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |




## 修改用户平台触发类场景

**使用说明**

>修改用户平台触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、 更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存平台触发类场景。


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT/IFTTT_jsonDataStructure)|    



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |





## 修改用户手动触发类场景

**使用说明**

>修改用户手动触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存手动执行类场景。



**接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT/IFTTT_jsonDataStructure)|   



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |   





## 修改用户场景

**使用说明**

>修改用户场景（传入参数userSceneDto中必须传入需要修改的id,familyId不能修改）。
说明：
1，	支持场景开启状态下的编辑功能；
2，	不区分场景类型，所有场景均可编辑；
3，	编辑后场景规则ID、条件、动作ID均会重新生成


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/modifyUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 |           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | Object| String  |  必填 |&nbsp; |  
|  retInfo  | Object| String  |  必填 |&nbsp; |  
|  data  | Object| Body  |  必填 |返回修改成功的场景ID |   



## 规则类获取规则详情

**使用说明**

>根据规则Id查询规则具体信息。返回结构和保存用户场景结构一致。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/rule/getRuleInfo`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|          
| ruleId| String |32| Body| 必填|规则Id|     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | RuleTemplateDto| Body  |  必填 |&emsp;  |




## 查询场景详情

**使用说明**

>根据场景id查询场景的基本信息、条件和动作信息。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/getSceneDetailBySceneId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |N/A| Body| 必填|场景id|  
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | UserSceneDto| Body  |必填| &nbsp;|



## 查询家庭下场景列表

**使用说明**

>获取家庭下场景列表（包括用户自建场景和通过模板下载的场景）。


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/listSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|   
| limit| Int|N/A| Body| 必填|每页显示的记录数最多20条，大于20条，默认20条| 
| cursor| Int|N/A| Body| 必填|从0开始| 
| sceneTagList| List<SceneTagDto>|N/A| Body| 选填|场景位置标签列表| 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |Pagination<SceneDto>| Body  |  必填 |显示场景中的描述信息,其中的规则rules中带有规则Id和规则名称以及规则描述,同时记录按照创建时间倒序|

**示例**  

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 89998291f2d490e3339ed7f08247f85b8fc8185179ab0937b240c2aaceba9d76
timestamp: 1542184530374 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "385062139898000000",
	"limit": 10,
	"cursor": 0
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": {
		"pageSize": 10,
		"recordCount": 13,
		"currentPage": 1,
		"totalPage": 2,
		"cursor": 10,
		"list": [{
			"id": "98c2af1901b84eb1b4be99cedb53e96b",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "回家模式",
			"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
			"isOpen": 1,
			"rules": [{
				"id": "d8c4d6fad4854aac9c23e2aaaf6a9ebd",
				"rule": "点击即可执行"
			}],
			"createTime": "2018-09-12 14:58:46",
			"updateTime": "2018-09-12 14:58:46",
			"sourceId": "1c10d415fa3444dd9f900a53a70964cb",
			"type": "deviceFamily",
			"triggerType": "manually",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 1,
				"cron": {
					"minutes": "12",
					"hours": "15",
					"day": "?",
					"month": "*",
					"week": "6,7",
					"year": "*",
					"cronExp": "* 12 15 ? * 6,7 *",
					"weekCronExp": "* * * ? * 6,7 *"
				},
				"status": true
			},
			"aiKeyword": "一键回家",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "642fe4695da84edb8e70d3dd241ce952",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "62da131f1ea94f78918eabfa228e334d",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-12 15:23:00",
			"updateTime": "2018-09-17 17:56:10",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": true,
				"activeBeginTime": "01:08:00",
				"activeEndTime": "02:10:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "30d0ed93a24941d985ccfc39bbb47074",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "49b2151037944d8c98da9fb07afdcadb",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-03 16:05:44",
			"updateTime": "2018-09-07 13:04:55",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "1,2,3,4,5",
					"year": "*",
					"cronExp": "* * * ? * 1,2,3,4,5 *",
					"weekCronExp": "* * * ? * 1,2,3,4,5 *"
				},
				"status": false,
				"activeBeginTime": "13:04:00",
				"activeEndTime": "13:04:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aed9381c28c84008b2dc73ac0e6ab9ec",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 1,
			"rules": [{
				"id": "dd1a4d8e14084ed5bbeaa81dc862f531",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-25 16:54:28",
			"updateTime": "2018-09-25 16:54:28",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%80-0815_20180815131434035.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%802_20180730134743357.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aa93a3a938834748a9060a177669b606",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "1d034095b1a747c6a683abf8aa552930",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-17 14:10:59",
			"updateTime": "2018-09-17 14:10:59",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "935d7d4c0a5647b29461c631de3f0b0d",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "61e8fd04d8ac4aa7a0b1a81065288a16",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-15 15:04:34",
			"updateTime": "2018-09-15 15:04:34",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "2bcbb7186a0440ee9aab1392e7018fe9",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "3ac637309eb14c41b777f2f85e9c80e2",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-14 09:21:27",
			"updateTime": "2018-09-14 09:21:27",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "ac57eb2f1ea543dc9029e3bb4003e798",
			"userId": "100013957366168786",
			"familyId": "385062139898000000",
			"sceneName": "内测-满足所有条件（不同设备）",
			"sceneDesc": "空调开机时执行恒氧",
			"isOpen": 0,
			"rules": [{
				"id": "aec688187f6346edbdce48c43667f697",
				"rule": "空调挂机/柜机同时关机时新风机恒氧"
			}],
			"createTime": "2018-09-07 09:44:03",
			"updateTime": "2018-09-07 13:05:10",
			"engineVersion": "V24",
			"sourceId": "ddbad81f41a64fdcb7d787c3ebb12a10",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"status": true,
				"activeBeginTime": "00:00:00",
				"activeEndTime": "13:59:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "484eca88bc6344f18ead3fb3e90f0471",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "330eccefe2b6434ea5fcea2821516df6",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "56b04883b62249e2b94b7d50c1c45044",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "82a7dcb10766450697aa032163123de4",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "c150dfaa249b4781b825d695621f6e97",
				"rule": "中央空调温度过高"
			},
			{
				"id": "e5cc8ff3c8a946d198a78e0471f387a1",
				"rule": "中央空调温度过低"
			},
			{
				"id": "efc6e8636f05494f98ddb7f51a6eafbb",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:15:03",
			"updateTime": "2018-09-11 11:15:03",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": false,
				"activeBeginTime": "18:09:00",
				"activeEndTime": "12:00:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "77cd027bd6814b87a558778a5367a714",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "0907c3c943bb4649bb5265287b0b47db",
				"rule": "中央空调温度过低"
			},
			{
				"id": "2475174255934a67b5ce6ae6dc5f11a8",
				"rule": "中央空调温度过高"
			},
			{
				"id": "67f088f060e04cc18412dc700c207245",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "84ffc0b731414e7aa000155b8250ded3",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "ca95580592614382b37b0777a98be850",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "e3c5580edd4c4bb295f049cdee3e9dfc",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:14:05",
			"updateTime": "2018-09-11 11:14:05",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		}],
		"fromIndex": 0,
		"toIndex": 10
	}
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





## 手动执行类场景列表查询

**使用说明**

>查询家庭下手动执行类场景列表

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/ listManuallySceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|




## 开关类场景列表查询

**使用说明**

>平台触发类场景列表查询

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/listPlatformSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|




## 根据系统id查询场景列表

**使用说明**

>根据系统id查询场景列表，进行应用隔离,系统id根据UAG获取区分。

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/listFamilySceneBySystemId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|默认10条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|



## 根据属性id查询场景

**使用说明**

>根据设备id 查询场景列表

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/listSceneBySettings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| param| Map<String,String> |N/A| Body| 必填|{“id”: “mac”}id值：设备mac值| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|





## 根据场景名称查询场景

**使用说明**

>根据场景名称模糊查询场景列表

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/ listSceneBySceneName`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| sceneName| String |场景名称| Body| 选填|场景名称| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |只读| &emsp;|
|  retInfo  |String| Body  |只读| &emsp;|
|  data  | List<UserSceneDto >| Body  |只读|返回为null的字段被过滤掉，不显示|




## 修改场景描述信息

**使用说明**

>修改场景描述信息（场景名称、场景别名、场景描述）,多余参数不要传，防止修改出错。

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateSceneDesc`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|




## 修改场景标签

**使用说明**

>修改场景标签，场景id必须，标签信息必须

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateSceneTag`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |32| Body| 必填|场景id| 
| familyId| String |32| Body| 必填|家庭Id| 
| sceneTagList| List<SceneTagDto> |N/A| Body| 必填|场景标签列表| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|




## 查询家庭下标签信息

**使用说明**

>获取家庭下场景标签列表

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/listSceneTagByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|    
| familyId| String |32| Body| 必填|家庭Id| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  tagList  |List<SceneTagDto>| Body  |必填| 用户场景标签|



## 组件ID查询家庭场景列表

**使用说明**

>通过组件ID和家庭ID查询家庭下的场景列表。 


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/getSceneListByComponentId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| componentId| String |N/A| Body| 必填|组件Id|  
| familyId| String |N/A| Body| 必填|家庭Id| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | String| Body  |必填|&emsp;|
|  retInfo  | String| Body  |必填|&emsp;|
|  data  | List<UserSceneDto >| Body  |必填|返回为null的字段被过滤掉，不显示。|


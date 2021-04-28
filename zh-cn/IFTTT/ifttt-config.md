
# 家庭配置类

!> **更新时间**：{docsify-updated}  

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

 2、请求样例  

**用户请求**
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



## 判断该场景下规则的条件以及动作等是否正确

**使用说明**

>根据场景id判断该场景下规则的条件以及动作等是否正确，返回Map<String,List<String>> ,key 为规则id,value为校验符合要求,可以开启的动作集合。

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/ruleJudgement`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |&emsp;| Body|必填 |&emsp; | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  ruleMap  |Map<String,List<String>| Body  |必填| &emsp;|



## 根据场景模板Id保存规则参数信息

**使用说明**

>根据场景模板Id下载场景,并且更新下载后的场景的规则参数信息。
 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/saveTemplate`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateId| String |32| Body| 必填|场景模板Id | 
| sceneTagList| List<SceneTagDto> |N/A| Body|选填|场景位置标签 |
| ruleSettings| RuleSettings[] |N/A| Body| 必填|type:必填，枚举类型可选值condition或者action；</br>id：模板的条件或者行为Id，取决于type；数据结构定义：</br>`{`</br>`"mac":"A123456",`</br>`"clazz":"00123"  //设备大类加中类；这个必须跟海极网一致`</br>`}`</br>如果同一型号需要传入多个设备，数据格式如下：</br>`{"mac":"mac1,mac2,mac3","clazz":"02012"}`</br>和V2.3不一样的是去除了wifitype</br>value:选填 数据结构定义：</br>`{`</br>` "value":"open", //具体的值；`</br>`"desc":"开机"   //具体值的描述`</br>`}`|   
   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  sceneId  |String| Body  |必填|返回保存下载后场景更新规则参数成功的场景Id|



## 批量更新规则的参数

**使用说明**

>批量更新规则详情 说明：如果需要更新的场景是手动触发类（例如，一键离家）那么每次更新规则系统都会判断当前更新后的场景是否满足实例化条件，如果满足则立即实例化并且场景的isOpen为true表示场景已经开启满足触发要求；如果是开关类场景，则只会更新最新场景的数据不会进行实例化，如果需要实例化需要APP调用手动开启场景接口。
 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/rule/updateSettings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| settings| RuleSettings[] |N/A| Body| 必填|如果同一型号需要传入多个设备，数据格式如下：`{"mac":"mac1,mac2,mac3","clazz":"02012"}`和V2.3不一样的是去除了wifitype| 
| familyId| String |32| Body| 必填|&emsp;| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Object| Body  |必填|如果成功返回success；如果失败返回错误信息|





## 获取家庭规则参数设置

**使用说明**

>根据familyId和场景id查询家庭规则设置，其中条件或者动作未设置参数(即没有配置设备信息，外部条件信息等)也会返回记录。

 **接口描述**
?> **接入地 址：**  `/iftttscene/scene/getRuleSettingsByFamilyId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |N/A| Body| 必填|家庭Id| 
| sceneIds| String[] |N/A| Body| 选填|场景id数组| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | List<RuleSettings>| Body  |必填|显示为null|



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
| param| Map<String,String> |N/A| Body| 必填|{“id”: “mac”}id值：设备mac值
| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|




## 查询场景执行状态

**使用说明**

>根据场景id 查询场景场景执行状态

 **接口描述**
?> **接入地 址：**  `/iftttscene/scene/getOperationLogInfo`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| sceneId| String |32| Body| 必填|场景id
| 
| sequenceid| String |32| Body| 必填|场景执行id| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |只读| &emsp;|
|  retInfo  |String| Body  |只读| &emsp;|
|  data  | SceneOperationLogDto| Body  |只读|返回家庭下场景的操作记录|



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




## 根据场景模板Id启用场景，优化接口

**使用说明**

>根据场景模板Id下载场景,并设置参数，如果该场景是开关类场景则把场景打开。

 **接口描述**
?> **接入地 址：**  `/iftttscene/scene/sceneEnabled`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|    
| familyId| String |32| Body| 必填|家庭Id| 
| templateId| String |32| Body| 必填|场景模板Id| 
| ruleSettings| RuleSettings[] |N/A| Body| 必填|条件或者动作配置参数| 
| sceneName| String |255| Body| 选填|场景名称| 
| sceneTagList| List<SceneTagDto> |N/A| Body| 选填|场景位置标签| 
| taskInfoList| List<TaskInfoDto> |N/A| Body| 选填|场景生效时间| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  sceneId  |String| Body  |必填| 返回保存下载后场景更新规则参数成功的场景Id|


## 场景模版一键启用

**使用说明**

>根据家庭id、场景模板Id（目前只支持可启用的场景模版id）自动下载场景。 

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/sceneAutoDownload`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateIds| String[] |512| Body| 必填|模板Id数组   | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|


## 判断该场景下规则的条件以及动作等是否正确

**使用说明**

>根据场景id判断该场景下规则的条件以及动作等是否正确，返回Map<String,List<String>> ,key 为规则id,value为校验符合要求,可以开启的动作集合。

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/ruleJudgement`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |&emsp;| Body|必填 |&emsp; | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  ruleMap  |Map<String,List<String>| Body  |必填| &emsp;|



## 根据场景模板Id保存规则参数信息

**使用说明**

>根据场景模板Id下载场景,并且更新下载后的场景的规则参数信息。
 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/saveTemplate`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateId| String |32| Body| 必填|场景模板Id | 
| sceneTagList| List<SceneTagDto> |N/A| Body|选填|场景位置标签 |
| ruleSettings| RuleSettings[] |N/A| Body| 必填|type:必填，枚举类型可选值condition或者action；</br>id：模板的条件或者行为Id，取决于type；数据结构定义：</br>`{`</br>`"mac":"A123456",`</br>`"clazz":"00123"  //设备大类加中类；这个必须跟海极网一致`</br>`}`</br>如果同一型号需要传入多个设备，数据格式如下：</br>`{"mac":"mac1,mac2,mac3","clazz":"02012"}`</br>和V2.3不一样的是去除了wifitype</br>value:选填 数据结构定义：</br>`{`</br>` "value":"open", //具体的值；`</br>`"desc":"开机"   //具体值的描述`</br>`}`|   
   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  sceneId  |String| Body  |必填|返回保存下载后场景更新规则参数成功的场景Id|



## 批量更新规则的参数

**使用说明**

>批量更新规则详情 说明：如果需要更新的场景是手动触发类（例如，一键离家）那么每次更新规则系统都会判断当前更新后的场景是否满足实例化条件，如果满足则立即实例化并且场景的isOpen为true表示场景已经开启满足触发要求；如果是开关类场景，则只会更新最新场景的数据不会进行实例化，如果需要实例化需要APP调用手动开启场景接口。
 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/rule/updateSettings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| settings| RuleSettings[] |N/A| Body| 必填|如果同一型号需要传入多个设备，数据格式如下：`{"mac":"mac1,mac2,mac3","clazz":"02012"}`和V2.3不一样的是去除了wifitype| 
| familyId| String |32| Body| 必填|&emsp;| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Object| Body  |必填|如果成功返回success；如果失败返回错误信息|





## 获取家庭规则参数设置

**使用说明**

>根据familyId和场景id查询家庭规则设置，其中条件或者动作未设置参数(即没有配置设备信息，外部条件信息等)也会返回记录。

 **接口描述**
?> **接入地 址：**  `/iftttscene/scene/getRuleSettingsByFamilyId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |N/A| Body| 必填|家庭Id| 
| sceneIds| String[] |N/A| Body| 选填|场景id数组| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | List<RuleSettings>| Body  |必填|显示为null|



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
| param| Map<String,String> |N/A| Body| 必填|{“id”: “mac”}id值：设备mac值
| 
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



[^-^]:常用图片注释
[IFTTT_type]:../_media/_IFTTT/IFTTT_type.png




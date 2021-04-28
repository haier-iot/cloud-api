
# 场景日志
!> **更新时间**：{docsify-updated}  



## 获取场景最新操作日志

**使用说明**

>APP下拉刷新，获取家庭下场景的操作日志。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/getNewOperationLog`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id|  
| limit| Int |N/A| Body| 必填|查询记录数 取值范围: 10-100 | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | List< SceneOperationLogDto >| Body  |  必填 |返回家庭下场景的操作记录列表，根据记录产生的时间倒序排列  |

 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: a9a246405762255773e6d569ad65c745e265e783fafce1c880af475f809cbe12
timestamp: 1542620093937 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{"familyId":"385062139898000000","limit":50}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": [{
		"id": 17074,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1542352332000",
		"sn": "a2dfa747-9bdc-4816-b961-046e876e15a2",
		"status": 1
	},
	{
		"id": 17045,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541833930000",
		"sn": "71851546-6827-437d-adf2-6fb492e4cff5",
		"status": 1
	},
	{
		"id": 17042,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541747530000",
		"sn": "5a0ed6e4-795c-468d-8c72-c58b3909c567",
		"status": 1
	},
	{
		"id": 17003,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541229129000",
		"sn": "0baf1355-9a8c-4934-9ccd-286440fd7367",
		"status": 1
	},
	{
		"id": 17000,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541142729000",
		"sn": "d79e20f6-5321-4bca-9093-c24ef49094da",
		"status": 1
	}]
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





## 获取场景历史操作日志

**使用说明**

>上拉加载，获取家庭下场景的操作日志</br>APP下拉刷新，获取家庭下场景的执行日志，精确到动作级别。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/getHistoryOperationLog`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id|  
| number| Long |N/A| Body| 查询此number(不包括这一条) 之前的limit条数据，如果满足条件的数据条数不足limit条也正常返回； | 
| limit| Int |N/A| Body| 必填|查询记录数 取值范围: 10-100 | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | List< SceneOperationLogDto>| Body  |  显示家庭下场景的操作记录列表，根据记录产生的时间倒序排列 |





## 根据触发因素和触发类型查询触发的场景执行日志记录

**使用说明**

>智家app点击设备进入设备详情页，切换场景日志tag，显示该设备触发的场景执行日志记录，只有场景执行日志，不必展示场景下动作执行明细.
**接口描述**

?> **接入地 址：**  `/iftttscene/scene/find/scene-operation-logs-by-trigger-value`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| triggerValue| String |32| Body| 必填|场景执行的触发因素，设备触发传设备mac、天气触发的传citycode、定时的传cron表达式、地理围栏串经纬度，手动触发传触发场景的用户id | 
| limit| Int |N/A| Body|必填|场每页显示的记录数最多20条，大于20条，默认20条 |
| cursor| Int |N/A| Body| 必填|从0开始|   
| triggerType| String[] |N/A| Body| 必填|场景执行的触发因素类型：目前有5类：device、manual、timing、weather、timer、geofence；本期查询设备触发的场景执行日志triggerType参数传[“device”]即可|   
   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |Pagination< SceneOperationLogDto >| Body  |必填|1、	返回家庭下某一设备触发执行的场景分页列表数据，按照场景执行日志记录时间的倒序排列。2、	返回为null的字段被过滤掉，不显示|
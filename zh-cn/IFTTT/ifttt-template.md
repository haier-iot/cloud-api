
# 场景类



## 批量下载基础场景
>APP从场景Store中下载基础场景。</br>
注：同一个家庭可以多次下载同一个场景

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/store/downloadStoreScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| storeSceneIds| String[] |32| Body| 必填|需要下载的场景|  
| familyId     | String |32| Body| 必填|家庭Id|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String | Body  |  必填 |  &emsp;  |
|  retInfo  |  String | Body  |  必填 |  &emsp; |
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |


 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 51f839ee62312c41931a42d7353b4e74f50d9f03bedfcd1a227f1be2efc7a91e
timestamp: 1542183603622 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"storeSceneIds": ["2258bce4c54d422b940167a8f30f04f3",
	"6e5faad84ef143e9a497c310e903baa4"],
	"familyId": "zf_platform"
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": ["7fc6f082f77343e2baac4a71b26044f7",
	"72b1f0084ead44229331e477d0de282a"]
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 


## 修改场景昵称
>修改场景昵称（别名）。

 1、接口定义

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

 2、请求样例  

**用户请求**
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
>删除用户下载的场景。

 1、接口定义

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


 2、请求样例  

**用户请求**
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
>根据用户填写的参数保存场景

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/addUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |


 2、请求样例  

**用户请求**
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
>根据用户填写的参数保存场景。

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/addUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |




## 修改用户平台触发类场景
>修改用户平台触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、 更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存平台触发类场景。


 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |





## 修改用户手动触发类场景
>修改用户手动触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存手动执行类场景。



 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |   





## 修改用户场景
>修改用户场景（传入参数userSceneDto中必须传入需要修改的id,familyId不能修改）。
说明：
1，	支持场景开启状态下的编辑功能；
2，	不区分场景类型，所有场景均可编辑；
3，	编辑后场景规则ID、条件、动作ID均会重新生成


 1、接口定义

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
>根据规则Id查询规则具体信息。返回结构和保存用户场景结构一致。

 1、接口定义

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






## 查询支持的关系表达式
>查询支持的关系表达式。

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getRelationOperator`  
 **HTTP Method：** POST

**输入参数**  

标准输入，无输入参数。    

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | JSON| Body  |  必填 |详见下方 |

data字段说明：</br>
```  
[{
	"value": "greaterThan",
	"desc": "高于",
	"required": false
},
{
	"value": "greaterThanEqual",
	"desc": "高于或等于",
	"required": false
},
{
	"value": "equal",
	"desc": "等于",
	"required": false
},
{
	"value": "lessThan",
	"desc": "低于",
	"required": false
},
{
	"value": "lessThanEqual",
	"desc": "低于或等于",
	"required": false
}]
```

 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 1ef2c702773e4da4e8e7994478f97d4d6313012c66f6437bae4f25bc8fb13980
timestamp: 1542619838663 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": [{
		"value": "greaterThan",
		"desc": "大于",
		"required": false
	},
	{
		"value": "greaterThanEqual",
		"desc": "大于等于",
		"required": false
	},
	{
		"value": "equal",
		"desc": "等于",
		"required": false
	},
	{
		"value": "lessThan",
		"desc": "小于",
		"required": false
	},
	{
		"value": "lessThanEqual",
		"desc": "小于等于",
		"required": false
	}]
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 


## 获取场景最新操作日志
>APP下拉刷新，获取家庭下场景的操作日志。

 1、接口定义

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
>上拉加载，获取家庭下场景的操作日志</br>APP下拉刷新，获取家庭下场景的执行日志，精确到动作级别。

 1、接口定义

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


## 查询场景详情
>根据场景id查询场景的基本信息、条件和动作信息。

 1、接口定义

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




## 根据触发因素和触发类型查询触发的场景执行日志记录
>智家app点击设备进入设备详情页，切换场景日志tag，显示该设备触发的场景执行日志记录，只有场景执行日志，不必展示场景下动作执行明细.
 1、接口定义

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


## 根据设备mac查询设备功能
>查询当前家庭下设备的功能信息
 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/find/device-functions`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| deviceId| String |N/A| Body| 必填|设备id  | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |FunctionsDto| Body  |必填|&emsp;|


## 查询模板详情
>根据家庭id、模板id查询场景的模板详情信息
模版条件的desc描述拼接规则：
（1）ifLable=1时，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值。其中条件value的desc由前端H5拼接，其余部分场景引擎拼接。        如果没有设备，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值，由场景引擎拼接模版条件描述。
（2）ifLable=0时，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值，由场景引擎拼接模版条件描述。

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/template/find/template-info`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |N/A| Body| 必填|家庭Id | 
| templateId| String |N/A| Body| 必填|模板id   | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |SceneTemplateDto| Body  |必填|&emsp;|


## 统计场景使用
>根据家庭id、用户ID查询场景使用信息
统计规则：filter参数为查询的条件，dimL1和dimL2为需要统计的维度。

 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/find/user-data`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| filter| List<Map> |N/A| Body| 必填|[{"name": "familyId","value": "123"	},{	"name": "userId","value": "234"	}] | 
| dimL1| List<String> |N/A| Body| 必填|[“paltform”,”manually”] | 
| dimL2| List<String> |N/A| Body| 选填|[“open”,”close”] | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |SceneTemplateDto| Body  |必填|&emsp;|



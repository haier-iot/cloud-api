
# 用户场景配置

!> **更新时间**：{docsify-updated}  


## 查询支持的关系表达式

**使用说明**

>查询支持的关系表达式。

**接口描述**

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



## 更新多个生失效时间段接口

**使用说明**

>更新多个生失效时间段功能（支持平台），场景开启或关闭状态都可以更新。  


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/updateTaskInfoList`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |N/A| Body| 必填|用户场景Id|  
| taskInfoList| List<TaskInfoDto> |5| Body| 必填|需要更新的生失效时间段| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Object| Body  |必填|显示为null|








[^-^]:常用图片注释
[IFTTT_type]:../_media/_IFTTT/IFTTT_type.png




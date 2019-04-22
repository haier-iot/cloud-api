
!>  **更新时间**：{docsify-updated}  

## 简介

> 为Haier U+云平台建立统一的定时预约服务，不同App均使用相同的预约服务定时操控设备。


预约定时服务提供单任务预约执行与周期性任务的管理与设置，包括智能互联设备定时开关机、设备的模式设置、操作指令下发等控制能力。  

通过统一的在云端建立定时执行任务,各应用之间使用统一的定时服务控制设备,避免了不同服务端对智能互联设备控制造成的冲突。
提供预约定时任务的新建、修改、删除与任务执行状态（启动、暂停）的管理。

![预约定时图片][scheduler_type]

**预约任务管理**  
支持新建、修改或删除预约任务，用户通过设置预约事假周期、时间点、自动执行指令等创建和修改预约任务，对于同一台设备可以设定多条预约。
预约状态管理，预约任务分为启动与暂停两种状态，可根据使用需求进行状态开关设置。

**预约任务执行分析**  
执行预览，为保证定时或周期性任务正确执行可对预约任务进行预览，即立即执行一次以预览相关控制指令设定效果。
查询与运行历史，已设定预约与历史执行情况，提供了查询接口可查看已有预约任务与其历史运行情况，包括运行时间、执行的操作、执行结果等数据。

## 应用场景

预约定时任务应用于按照时间规则触发的自动化任务的执行的场景，包括单任务预约定时和批量设备预约定时。
![预约定时场景流程][scheduler_flow]

**定时任务**

定时任务分为提供对单个设备或多设备的单定时任务和批量定时任务，可根据需求自行设计任务场景

**定时任务管理**

查看、删除、执行等相关的任务操作


## 公共结构

### TaskInfo

|名称	|预约定时任务信息|&emsp;|TaskInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|taskId	|String	|任务id|	varchar(50)
|taskName|	String	|任务名称|	varchar(50)
|schedulerType|	int	|预约类型（1=设备），此版本仅支持设备预约；|	int(2)|
|typeId|	String	|设备型号|	varchar(100)
|deviceId|	String	|设备mac	|varchar(50)
|appInfo|	appInfo[]	|应用信息 |	&emsp;|
|createUserInfo|	UserInfo[]	|创建者|	&emsp;|
|createTime|	dateTime	|创建时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|modifyUserInfo|	UserInfo[]|	修改者	|&emsp;|
|modifyTime|	dateTime|	修改时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|cron|	Cron[]|	cron对象	|&emsp;|
|endTime|	dateTime|	任务终止时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|endTimeSource|	int	|endTime来源	|0: 系统默认，1:用户设置|
|argsInfo|	ArgsInfo[]	|指令	|&emsp;|
|status|	int|	定时预约状态 0 启用；1 已完成； 2 暂停；3 删除；4失效 |	int(2)|
|taskDesc|	String|	任务描述	|varchar(100)
|taskSeq|	int|	子任务序号id|	1：系统默认
|taskAmount|	int|	子任务总数|	&emsp;|


### UserInfo

|名称	|用户信息|&emsp;|UserInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|userId|	String|	用户id|	从uag获取|
|phone|	String|	手机号	|中间四位加密|
|userName|	String|	用户昵称	|选填(本期不实现，返回空)|
|headImg|	String|	头像|	选填(本期不实现，返回空)|



### AppInfo

|名称	|应用信息|&emsp;|AppInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|appId|	String|	应用id|	从uag获取|
|appVersion|	String|	应用版本标识|	选填(本期不实现，返回空)|
|appName|	String|	应用名称	|选填(本期不实现，返回空)|
|appLogo|	String|	应用Logo	|选填(本期不实现，返回空)|



### Cron

|名称	|Cron表达式说明|&emsp;|Cron|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|minutes|	String|	必填，分钟；|	取值范围（0-59）；允许特殊字符（, - * /）|
|hours|	String	|必填，小时|	取值范围（0-23）；允许特殊字符（, - * /）|
|day|	String	|必填，天|	取值范围（1-31）；允许特殊字符（, - * ? / L W）|
|month|	String	|必填，月|	取值范围（1-12 or JAN-DEC）；允许特殊字符（, - * /）|
|week|	String	|必填，周|	取值范围（1-7 or SUN-SAT）；允许特殊字符（, - * ? / L #）|
|year|	String	|选填，年|	可为空，取值范围（1970-2099）；允许特殊字符（, - * /）|


> 表达式举例：
 0/5 * * * ?  ，表示每5分钟执行一次 ，
0 0/1 * * ?  , 表示每小时执行一次，
0 8 * * ? *   , 表示每天早上8点执行一次，
0 8 3 * ?   , 表示每月3号早上八点执行一次，
0 8 ? * MON，表示每周一早上八点执行一次，



### ArgsInfo

|名称	|Args指令集合|&emsp;|Args|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|args|	Args[]	|必填，指令	|&emsp;|
|optName	|String|	选填，操作名|	如果optName不为空则为组操作，根据optName可以区分操作指令是否需要补全。非标准模型对应组命令id|


### Args

|名称	|Args指令集合|&emsp;|Args|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|name	|String|	必填，指令名称|	指令名称为模型文件中的属性|
|desc	|String|	选填，指令名称描述|	指令名称描述为模型文件中对属性名称的描述；或者非标准模型组命令的操作名的描述；|
|value	|String|	必填，指令值|	表示 将属性设置为该值；|


### TaskEditInfo

|名称	|预约编辑日志|&emsp;|TaskEditInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|taskId	|String	|任务id|	&emsp;|	
|taskSeq|	int|	任务序号|&emsp;|	
|deviceId|	String|	设备mac	|&emsp;|	
|userInfo|	UserInfo[]|	用户信息	|&emsp;|	
|appInfo|	AppInfo[]|	应用信息	|&emsp;|	
|status|	int|	定时预约状态 0 启用；1 已完成； 2 暂停； 3 删除；|&emsp;|	
|type|	int|	1=增加、2=修改、3=删除	|&emsp;|	
|modifyTime|	String|	修改时间	|&emsp;|	
|endTime|	String	|终止时间	|&emsp;|	
|argsInfo|	ArgsInfo[]|	指令	|&emsp;|	



### TaskExecInfo

|名称	|预约定时执行日志|&emsp;|TaskExecInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|taskId|	String|	任务id		|&emsp;|
|taskSeq|	int|	任务序号		|&emsp;|
|deviceId|	String|	设备mac		|&emsp;|
|sn|	String|	下发序列		|&emsp;|
|execTime|	String|	执行时间		|&emsp;|
|step|	int	|执行阶段:0 开始 1 调用M2M 2 执行结果	|&emsp;|	
|retCode|	String|	下发成功为0，失败根据M2M错误码表填写；结果信息根据命令返回的ACK信息中的错误码填写此编码		|&emsp;|
|retInfo|	String|	执行信息		|&emsp;|



### TaskRestLogDataInfo

|名称	|预约定时执行日志|&emsp;|TaskRestLogDataInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|total|	String|	必填，总数	|&emsp;|
|curSize|	String|	必填，当前页条数	|&emsp;|
|currentPage|	String|	必填，当前页码|&emsp;|	
|pageSize|	String|	必填，每页条数	|&emsp;|
|logInfos|	List<TaskExecInfo>|	必填，日志结果信息	|&emsp;|


## 接口清单

> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 预约定时新增     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量新增  |适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 预约定时删除     | 将指定任务ID的预约定时设置为删除状态 | 是| 无|
| 预约定时修改     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量修改 | 适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 根据userid查询预约| 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约） | 是| 无|
| 根据deviceid查询预约| 根据deviceid查询预约详情 | 是| 无|
| 根据taskId查询预约| 根据taskid查询预约详情 | 是| 无|
| 预约定时执行日志查询| 预约定时执行日志查询 | 是| 无|
| 预约任务执行预览| 任务立即下发一次 | 是| 无|



### 预约定时服务接口

#### 预约定时新增

> 预约定时新增，适用于单设备单任务场景

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/add   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| taskInfo     | TaskInfo | Body| 必填|任务信息|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  detailInfo  |  &emsp;  |   Body   | 必填| 返回任务id(taskId) |

**输入参数TaskInfo对象说明:**

| 参数名        | 类型          | 必填|说明|
| ------------- |:-------------:|:-------------:|:-----:|
|deviceId|	String varchar(50)|	必填	|设备id| 
|schedulerType|	int|	必填	|预约类型（1=设备），此版本仅支持设备预约|
|typeId	|String varchar(100)|	必填|	设备typeId|
|cron	|Cron[]|	必填|	预约定时表达式，任务执行时间|
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo[]|	必填	|多套指令集；当前版本只支持一套|
|endTime	|dateTime|	选填|	任务终止时间；不填默认值三个月有效期；如果有值，按照此值的有效期|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskSeq	|int	|选填|	子任务序号id；默认值为1|
##### 2、请求样例  

**用户请求**
```java  
Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
timestamp: 1491013448628 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"cron": [{
		"minutes": "0 / 5",
		"hours": "*",
		"day": " * ",
		"monty": "*",
		"week": "?",
		"year": ""
	}],
	"endTime": "2018 - 05 - 21 12: 00: 00",
	"argsInfo": [{
		"optName": "OnOffStatus",
		"args": [
			"esdFilterCleanTime",
			"desc",
			"-12"
		]
	}],
	"taskName": "空调开机",
	"taskDesc": "空调开机",
    "taskSeq":"1"
}
}

```  

**请求应答**

```java
{
    "retCode": "00000",
	"retInfo": "预约定时创建成功",
	"detailInfo": {
    "taskId": “111111”
}
}
```

#### 预约定时批量新增

> 预约定时批量新增，适应于单设备批量定时和批量设备单定时场景

###### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/group/add   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| taskInfos     | List<TaskInfo> | Body| 必填|批量任务添加；list长度上限为50，超出则分批次调用；|

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  detailInfo  |  &emsp;  |   Body   | 必填| 返回任务id(taskId) |

**输入参数TaskInfo对象说明:**

| 参数名        | 类型          | 必填|说明|
| ------------- |:-------------:|:-------------:|:-----:|
|deviceId|	String varchar(50)|	必填	|设备id| 
|schedulerType|	int|	必填	|预约类型（1=设备），此版本仅支持设备预约|
|typeId	|String varchar(100)|	必填|	设备typeId|
|cron	|Cron[]|	必填|	预约定时表达式，任务执行时间|
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo[]|	必填	|多套指令集；当前版本只支持一套|
|endTime	|dateTime|	选填|	任务终止时间；不填默认值三个月有效期；如果有值，按照此值的有效期|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskId	|String varchar(50)	|选填|	任务id;在分次批量添加任务时，第二次此字段为必填，表示和上批次对应|
|taskSeq|	int|	必填	|子任务序号id；每组中的此字段不能重复；从1开始的正整数；|

##### 2、请求样例  

**用户请求**
```java  
Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
timestamp: 1491013448628 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
				"cron": [{
					"minutes": "0 / 5",
					"hours": "*",
					"day": " * ",
					"monty": "*",
					"week": "?",
					"year": ""
				}],
				"endTime": "2018 - 05 - 21 12: 00: 00",
				"argsInfo": [{
					"optName": "OnOffStatus",
					"args": [
						"esdFilterCleanTime",
						"desc",
						"-12"
					]
				}],
				"taskName": "空调开机",
				"taskDesc": "空调开机"，
                "taskId":"111111",
                "taskSeq":"1"
			}                  
		]

   }
}
```  

**请求应答**

```java
{
    "retCode": "00000",
	"retInfo": "预约定时创建成功",
	"detailInfo": {
    "taskId": "111111"
}
}

```

#### 预约定时删除
> 将指定任务ID的预约定时设置为删除状态

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/delete   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String varchar(50)|	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	选填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
  "taskId": "111111"，
  "taskSeq":1
}


```

**请求应答**
```java
{
	"retCode": "00000",
	"retInfo": "成功!"
}


```

#### 预约定时修改
> 预约定时修改，适用于单设备单任务场景

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/modify   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:---------:|
|taskInfo|	TaskInfo|	Body|	必填	|任务信息|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |

>输入参数TaskInfo对象说明：

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId	|String varchar(50)|	Body|	必填|	预约定时任务id|
|taskSeq	|int|	Body|	必填	|子任务序号id|
|taskName	|String varchar(50)	|Body|	选填	|任务名称|
|taskDesc	|String varchar(100)	|Body|	选填	|任务描述|
|status	|int|	Body|	选填	|定时预约状态 0 启用；2 暂停；|
|endTime	|dateTime	|Body	|选填	|如果有值，按照此值的有效期|
|cron	|cron[]|	Body	|选填	|预约定时表达式|
|argsInfo	|ArgsInfo[]|	Body|	选填|	多套指令集；当前版本只支持一套|




##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
	"cron": [
	{
		"seconds":"0",
		"minutes":"0/5",
		"hours":"*",
		"day":"*",
		"monty":"*",
		"week":"?",
		"year":""
	}
	],
	"argsInfo": [
	{
		"optName": "OnOffStatus", 
		"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
		]
	}
	]
}


```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```



#### 预约定时批量修改
> 预约定时批量修改，适应于单设备批量定时和批量设备单定时场景

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/group/modify   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskInfos|	List<TaskInfo>|	Body|	必填	|批量任务的修改；list长度上限为50，超出则分批次调用；|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |  

>输入参数TaskInfo对象说明：

|参数名	|类型	|位置|	必填|	说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|taskId	|String varchar(50)|	Body|	必填|	预约定时任务id|
|taskSeq|	int|	Body|	必填|	子任务序号id；每组中的此字段不能重复|
|taskName|	String varchar(50)|	Body|	选填|	任务名称|
|taskDesc|	String varchar(100)|	Body|	选填|	任务描述|
|status|	Int|	Body|	选填	|定时预约状态 0 启用；2 暂停；|
|endTime	|dateTime|	Body|	选填	|如果有值，按照此值的有效期|
|cron	|cron[]	|Body	|选填	|预约定时表达式|
|argsInfo|	ArgsInfo[]|	Body|	选填|	多套指令集；当前版本只支持一套|



##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
			"cron": [
				{
				   "seconds":"0",
				   "minutes”:"0/5",
				   "hours":"*",
				   "day":"*",
				   "monty":"*",
				   "week":"?",
					"year":""
				}
			],
		    "argsInfo": [
				{
					"optName": "OnOffStatus", 
					"args": [
						"esdFilterCleanTime", 
						"desc", 
						"-12"
					]
				}
			]
			}
		]
   }
}


```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```



#### 根据userid查询预约
> 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约）

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/query/userid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	选填	|设备id；如果不为空，根据userId和deviceId查询，为空根据userId查询|
|startNumber|	int	|Body 	|选填	|分页参数，起始值；如果不填写，默认值为1|
|length	|int|	Body| 	选填	|分页参数，长度；如果不填写，默认值为100|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"deviceId":"DC330D01FBF1",
  	"startNumber":1,
	"length":10
}


```

**请求应答**
```java
{
  "retCode": "00000",
"retInfo": "查询成功",
“detailInfo “:{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		"taskId": "111111"，
		"taskSeq": 1，
		"taskName": "空调开机"，
		"deviceId": "DC330D01FBF1"，
		"scheduleType"：1，
		"wifiType":" 111c120024000810040100318000614300000001000000000000000000000000",
		"appInfo":[
			{
			"appId":"1111111",
			"appVersion":"",
			"appName":"",
			"appLogo":""
			}
			]， 
		"createUserInfo": [
			{
			"userId":"111111",
			"userName":"",
			"headImg":"",
			"phone”:"139****1234"
			}
		]，
		"createTime": "2018-04-19 17:00:00"，
		"modifyUserInfo": "[
			{
			"userId":"111111",
			"userName":"test",
			"headImg":"b.jpg",
			"phone":"139****1234"
			}
		]，
		"modifyTime": "2018-04-19 17:00:00"，
		"cron": [
			{
			"seconds":"0",
			"minutes":"0/5",
			"hours":"*",
			"day":"*",
			"monty":"*",
			"week":"?",
			"year":""
			}
		],
		"endTime":"2018-05-21 12:00:00",
		"status": 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		"taskDesc":"空调开机"
		}
	]
    }
}

```


#### 根据deviceid查询预约
>根据deviceid查询预约详情

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/deviceid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	必填|	设备id|
|startNumber|	int|	Body|	选填	|分页参数，起始值；如果不填写，默认值为1|
|length|	int|	Body|	选填	|分页参数，长度；如果不填写，默认值为100|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"deviceId": "DC330D32C193",
 	"startNumber":1,
	"length":10
}


```

**请求应答**
```java
{
"retCode": "00000",
"retInfo": "查询成功",
"detailInfo":{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		"taskId": "111111"，
		"taskSeq": 1，
		"taskName": "空调开机"，
		"deviceId": "DC330D01FBF1"，
		"scheduleType"：1，
		"wifiType":" 111c120024000810040100318000614300000001000000000000000000000000",
		"appInfo":[
			{
			"appId":"1111111",
			"appVersion":"",
			"appName":"",
			"appLogo”:""
			}
			]， 
		"createUserInfo": [
			{
			"userId":"111111",
			"userName":"",
			"headImg":"",
			"phone":"139****1234"
			}
		]，
		"createTime":"2018-04-19 17:00:00"，
		"modifyUserInfo": "[
			{
			"userId":"111111",
			"userName":"test",
			"headImg":"b.jpg",
			"phone":"139****1234"
			}
		]，
		"modifyTime": "2018-04-19 17:00:00"，
		"cron": [
			{
			"seconds":"0",
			"minutes":"0/5",
			"hours”:"*",
			"day":"*",
			"monty":"*",
			"week":"?",
			"year":""
			}
		],
		"endTime":"2018-05-21 12:00:00",
		"status": 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		"taskDesc":"空调开机"
		}
	]
    }
}

```



#### 根据taskId查询预约
>根据taskid查询预约详情

##### 1、接口定义

?> **接入地 址：**  `/scheduler/v1/device/query/taskid   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String varchar(50)|	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	选填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "taskId": "111111'
}


```

**请求应答**
```java
{
"retCode": "00000",
"retInfo": "查询成功",
"detailInfo":{
    [
		{
		"retCode": "00000",
		"retInfo": "查询成功",
		"detailInfo": {
		"taskId": "111111"，
		"taskSeq": 1，
		"taskName": "空调开机"，
		"deviceId": "DC330D01FBF1"，
		"scheduleType"：1，
		"wifiType":" 111c120024000810040100318000614300000001000000000000000000000000",
		"appInfo":[
			{
			"appId":"1111111",
			"appVersion":"",
			"appName":"",
			"appLogo”:""
			}
			]， 
		"createUserInfo": [
			{
			"userId":"111111",
			"userName":"",
			"headImg":"",
			"phone":"139****1234"
			}
		]，
		"createTime":"2018-04-19 17:00:00"，
		"modifyUserInfo": "[
			{
			"userId":"111111",
			"userName":"test",
			"headImg":"b.jpg",
			"phone":"139****1234"
			}
		]，
		"modifyTime": "2018-04-19 17:00:00"，
		"cron": [
			{
			"seconds":"0",
			"minutes":"0/5",
			"hours”:"*",
			"day":"*",
			"monty":"*",
			"week":"?",
			"year":""
			}
		],
		"endTime":"2018-05-21 12:00:00",
		"status": 1,
		"argsInfo": [
			{
			"optName": "OnOffStatus", 
			"args": [
			"esdFilterCleanTime", 
			"desc", 
			"-12"
			]
			}
		],
		"taskDesc":"空调开机"
		}
	]
    }
}

```



#### 预约定时执行日志查询
>预约定时执行日志查询

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/query/execlog   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId	|String varchar(50)	|Body|	可选	|任务id|
|deviceId|	String varchar(50)|	Body|	必填|	设备id|
|beginTime|	Date|	Body|	可选|	任务执行开始时间: 日期类型的字符串|
|endTime|	Date|	Body|	可选	|任务执行结束时间: 日期类型的字符串|
|currentPage|	int|	Body|	可选|	分页参数，当前页，最小为1;默认值1|
|pageSize|	int|	Body|	可选|	分页参数，每页条数;默认值10|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskRestLogDataInfo>|	Body|	必填	|返回详情；|


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  	"taskId":"111111",
  	"deviceId":"DC330D32C193",
  	"beginTime":"2018-04-19 17:00:00",
  	"endTime":"2018-04-20 17:00:00",
  	"pageSize":10,
	"currentPage":1
}


```

**请求应答**
```java
{
"retCode": "00000",
"retInfo": "查询成功",
"detailInfo":{
	[
		{
			"total": 6,
			"curSize": 6,
			"currentPage": 1,
			"pageSize": 10,
			"logInfos": [{
				"taskId": 1,
				"deviceId": "DC330D32C193 ",
				"sn": "dd753e6da718402dbd32fef9549e9676",
				"excuteTime": "2018 - 04 - 19 17: 00: 00",
				"step": 0,
				"retCode": "00000",
				"retInfo": "成功"
			}]
		}
	]
}
}

```


#### 预约任务执行预览
>预约任务执行预览

##### 1、接口定义

?> **接入地 址：**  `/scheduler /v1/device/preview   `  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String varchar(50)|	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	必填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |


##### 2、请求样例  

**用户请求**
```java 
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
timestamp: 1491014596343 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
	"taskId": "111111"
  	"taskSeq":1
}


```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```






[^-^]:常用图片注释
[scheduler_type]:_media/_scheduler/scheduler_type.png
[scheduler_liucheng]:_media/_scheduler/scheduler_liucheng.png
[scheduler_flow]:_media/_scheduler/scheduler_flow.png


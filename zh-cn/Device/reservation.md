# 设备预约定时

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

**TaskInfo**

|名称	|预约定时任务信息|&emsp;|TaskInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|taskId	|String	|任务id|	varchar(50)
|taskName|	String	|任务名称|	varchar(50)
|schedulerType|	int	|预约类型（1=设备），此版本仅支持设备预约；|	int(2)|
|typeId|	String	|设备型号|	varchar(100)
|deviceId|	String	|设备mac	|varchar(16) 格式：大写字母和数字 不包含特殊字符
|appInfo|	appInfo[]	|应用信息 |	&emsp;|
|createUserInfo|	UserInfo[]	|创建者|	&emsp;|
|createTime|	dateTime	|创建时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|modifyUserInfo|	UserInfo[]|	修改者	|&emsp;|
|modifyTime|	dateTime|	修改时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|cron|	Cron[]|	cron对象	|&emsp;|
|endTime|	dateTime|	任务终止时间: 日期时间类型的字符串|`yyyy-MM-dd hh:mm:ss`|
|endTimeSource|	int	|endTime来源	|0: 系统默认，1:用户设置|
|argsInfo|	ArgsInfo	|指令	|&emsp;|
|status|	int|	定时预约状态 0 启用；1 已完成； 2 暂停； |	int(2)|
|taskDesc|	String|	任务描述	|varchar(100)
|taskSeq|	int|	子任务序号id|	1：系统默认
|taskAmount|	int|	子任务总数|	&emsp;|
|systemId|	String|	云应用Id|	varchar(40)|


**UserInfo**

|名称	|用户信息|&emsp;|UserInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|userId|	String|	用户id|	从uag获取|
|phone|	String|	手机号	|中间四位加密|
|userName|	String|	用户昵称	|选填(本期不实现，返回空)|
|headImg|	String|	头像|	选填(本期不实现，返回空)|



**AppInfo**

|名称	|应用信息|&emsp;|AppInfo|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|appId|	String|	应用id|	从uag获取|
|appVersion|	String|	应用版本标识|	选填(本期不实现，返回空)|
|appName|	String|	应用名称	|选填(本期不实现，返回空)|
|appLogo|	String|	应用Logo	|选填(本期不实现，返回空)|
|clientId|	String|	创建者clientId	|选填|


**Cron**

|名称	|Cron表达式说明|&emsp;|Cron|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|seconds|	String|	必填，秒；|	取值范围（0-59）；允许特殊字符（, - * /）|
|minutes|	String|	必填，分钟；|	取值范围（0-59）；允许特殊字符（, - * /）|
|hours|	String	|必填，小时|	取值范围（0-23）；允许特殊字符（, - * /）|
|day|	String	|必填，天|	取值范围（1-31）；允许特殊字符（, - * ? / L W）|
|month|	String	|必填，月|	取值范围（1-12 or JAN-DEC）；允许特殊字符（, - * /）|
|week|	String	|必填，周|	取值范围（1-7 or SUN-SAT）；允许特殊字符（, - * ? / L #）|
|year|	String	|选填，年|	可为空，取值范围（1970-2099）；允许特殊字符（, - * /）|


> 表达式举例：
 0/5 * * * ?  ，表示每5分钟执行一次 ，</br>
0 0/1 * * ?  , 表示每小时执行一次，</br>
0 8 * * ? *   , 表示每天早上8点执行一次，</br>
0 8 3 * ?   , 表示每月3号早上八点执行一次，</br>
0 8 ? * MON，表示每周一早上八点执行一次，</br>



**ArgsInfo**

|名称	|Args指令集合|&emsp;|Args|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|cmdMsgList|	List<CmdItem>	|必填	|批量命令（详情见公共接口类）|
|backUrl	|String|	非必填|	回调地址；将批量命令执行结果发送到这个地址，可以为空，为空则不回调，目前该地址由预约中心统一做配置控制，相关调用端（如APP端）无需填写（填写也不会生效）|


**CmdItem**

|名称	|Args指令集合|&emsp;|Args|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|index	|int|	在批量指令中的顺序号，从0开始，步长为1，不能重复、不能缺号；下发指令时将，按这个序号，逐个指令下发。（编号错误，将导致指令下发错误）|	用户必填|
|name	|String|	单个属性设置时，属性名|用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|
|value	|String|	单个属性设置时，属性值|用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|
|cmdName|String|	组命令时，操作名|	用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|
|cmdArgs|Map<String,String>|	组命令时，属性集合|	用户可选name和value与cmdName和cmdArgs这两组由用户选择，必填其一|



**BatchResultDto**

|名称	|批量命令执行结果明细|&emsp;|BatchResultDto|
| ------------- |:-------------:|:-----:|:-------------:|
|字段名|	类型	|说明|	备注(数据字典描述，规范入参)|
|oid|long|	数据代理主键		|只读|
|cmdSn|String|	批量指令的总sn		|只读|
|cmdSubSn|String|	本条指令的sn，发送到领域模型。组成为：总sn+“：”+指令序号|只读|
|deviceId|String|	设备Id	|只读| 长度范围：1~16 格式：大写字母和数字 不包含特殊字符
|execStep|int	|本条指令的执行步骤|只读|	
|execResult|boolean|	本条指令本步骤的执行结果；true为成功、false为失败：如超时、异常等|只读|
|execResultCode|String	|本条指令本步骤的执行结果说明，错误码|只读|	
|execResultInfo|String|	本条指令本步骤的执行结果说明，错误信息|只读|
|resultTime|long|	本条指令本步骤的反馈时间|只读|


## 预约定时服务接口

### 接口清单

> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 预约定时新增     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量新增  |适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 预约定时删除     | 将指定任务ID的预约定时设置为删除状态 | 是| 无|
| 预约定时修改     | 适用于单设备单任务场景 | 是| 无|
| 预约定时批量修改 | 适应于单设备批量定时和批量设备单定时场景 | 是| 无|
| 根据userid查询预约| 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约） | 是| 无|
| 根据deviceid查询预约| 根据deviceid查询预约详情 | 是| 长度范围：1~16 格式：大写字母和数字 不包含特殊字符|
| 根据taskId查询预约| 根据taskid查询预约详情 | 是| 无|
| 预约定时执行日志查询| 预约定时执行日志查询 | 是| 无|
| 预约任务执行预览| 任务立即下发一次 | 是| 无|

### 预约定时新增

**使用说明**

> 预约定时新增，适用于单设备单任务场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

**接口描述**

?> **接入地 址：**  `/scheduler/v1/device/add`  
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
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo|	必填	|多套指令集；当前版本只支持一套|
|cron	|Cron[]|	选填|	任务执行表达式；cron和intervals必填其一|
|intervals	|int|	选填|	任务执行距当前的间隔时间，以分钟为单位，限制一天以（0-1440），如为0需要立即执行；cron和intervals必填其一。|
|endTime	|dateTime|	选填|任务终止时间；不填默认值2999-12-31 23 : 59 : 59；如果有值，按照此值的有效期|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskSeq	|int	|选填|	子任务序号id；默认值为1|
|systemId	|String	|选填|	云应用Id|

**示例** 

**用户请求**
```java  
Header:
	Connection: keep-alive
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
	timestamp: 1491013448628 
	language: zh-cn
	timezone: Asia/Shanghai
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  "status": 0,
	  "deviceId": "DC330D000023",
	  "schedulerType": 1,
	  "typeId": "edd032ff-7113-4970-a780-392d44c423b3",
	  "cron": [
	{
	      "seconds ": "30",
	      "minutes": " 5",
	      "hours": "*",
	      "day": " * ",
	      "month": "*",
	      "week": "?",
	      "year": ""
	    }
	  ],
	  "endTime": "2018 - 05 - 21 12: 00: 00",
	  "argsInfo": {
	    "cmdMsgList": [
	      {
	        "index": 0,
	        "name": "onOffStatus",
	        "value": "true"
	      }
	    ]
	  },
	  "taskName": "空调开机",
	  "taskDesc": "空调开机",
	  "taskSeq": "1",
	"systemId": "SV-YDDSYY111-0010"
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "预约定时创建成功",
	"detailInfo": {
	     “taskId”: “878442f1550c4c189c5307873ca9b1dd”
	}
}

```

### 预约定时批量新增

**使用说明**

> 预约定时批量新增，适应于单设备批量定时和批量设备单定时场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/group/add`  
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
|status|int	|必填	|任务状态； 0 启用 ；2暂停|
|argsInfo	|ArgsInfo|	必填	|多套指令集；当前版本只支持一套|
|cron	|Cron[]|	选填|	任务执行表达式；cron和intervals必填其一|
|intervals	|int|	选填|	任务执行距当前的间隔时间，以分钟为单位，限制一天以（0-1440），如为0需要立即执行；cron和intervals必填其一。|
|endTime	|dateTime|	选填|任务终止时间；不填默认值2999-12-31 23 : 59 : 59；如果有值，按照此值的有效期|
|taskName	|String varchar(50)|	选填|	任务名称|
|taskDesc	|String varchar(100)|	选填	|任务描述|
|taskId	|String varchar(50)	|选填|	任务id;在分次批量添加任务时，第二次此字段为必填，表示和上批次对应|
|taskSeq|	int|	必填	|子任务序号id；每组中的此字段不能重复；从1开始的正整数；|
|systemId|	String|	选填	|云应用Id|

**示例**

**用户请求**
```java  
Header:
	Connection: keep-alive
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
	timestamp: 1491013448628 
	language: zh-cn
	timezone: Asia/Shanghai
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  "taskInfos": [
	    {
	      "status": 0,
	      "deviceId": "DC330D003121",
	      "schedulerType": 1,
	      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
	      "intervals": 60,
	      "endTime": "2019-07-31 16:00:00",
	      "argsInfo": {
	          "cmdMsgList": [
	          {
	            "index": 0,
	            "name": "onOffStatus",
	            "value": "true"
	          },
	          {
	            "index": 1,
	            "name": "onOffStatus",
	            "value": "true"
	          }
	        ]
	      },
	      "taskName": "批量关机",
	      "taskDesc": "批量关机",
	      "taskSeq": "1",
	"systemId": "SV-YDDSYY111-0010"
	    },
	    {
	      "status": 0,
	      "deviceId": "DC330D003121",
	      "schedulerType": 1,
	      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
	      "intervals": 60,
	      "endTime": "2019-07-31 13:00:00",
	      "argsInfo": {
	        "cmdMsgList": [
	          {
	            "index": 0,
	            "name": "onOffStatus",
	            "value": "true"
	          },
	          {
	            "index": 1,
	            "name": "onOffStatus",
	            "value": "true"
	          }
	        ]
	      },
	      "taskName": "批量关机2",
	      "taskDesc": "批量关机2",
	      "taskSeq": "2",
	"systemId": "SV-YDDSYY111-0010"
	    }
	  ]
	}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "预约定时创建成功",
	"detailInfo": {
	     “taskId”: “878442f1550c4c189c5307873ca9b1dd”
	}
}

```

### 预约定时删除

**使用说明**

> 将指定任务ID的预约定时设置为删除状态

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/delete`  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId|	String |	Body|	必填	|预约定时任务id|
|taskSeq|	int	|Body|	选填	|子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
| |   |     |  |  &emsp; |


**示例**

**用户请求**
```java 
Header：
	appId: MB-****-0000
	appVersion: *****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: *******
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: +8
	appKey: ********
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  “taskId”: “878442f1550c4c189c5307873ca9b1dd”，
	   “taskSeq”:1
	}

```

**请求应答**
```java
{
	"retCode": "00000",
	"retInfo": "成功!"
}

```

### 预约定时修改

**使用说明**

> 预约定时修改，适用于单设备单任务场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/modify`  
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
|endTime	|dateTime	|Body	|选填	|任务终止时间；不填默认值2999-12-31 23 : 59 : 59；如果有值，按照此值的有效期|
|cron	|cron[]|	Body	|选填	|任务执行表达式；|
|intervals	|int|	Body	|选填	|任务执行距当前的间隔时间，以分钟为单位，限制一天以内（0-1440），如为0需要立即执行；|
|argsInfo	|ArgsInfo|	Body|	选填|	多套指令集；当前版本只支持一套|
|systemId	|String|	Body|	选填|云应用Id


**示例**  

**用户请求**
```java 
Header：
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: Asia/Shanghai
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  "taskId": "a2edc5e0b9ba485f9e63abae35e49983",
	  "taskSeq": 1,
	  "status": 0,
	  "intervals": 60,
	  "endTime": "2019-07-31 13:00:00",
	  "argsInfo": {
	     "cmdMsgList": [
	      {
	        "index": 0,
	        "name": "onOffStatus",
	        "value": "true"
	      },
	      {
	        "index": 1,
	        "name": "onOffStatus",
	        "value": "true"
	      }
	    ]
	  },
	  "taskName": "空调关机-修改",
	  "taskDesc": "空调开机-修改",
	"systemId": "SV-YDDSYY111-0010"
	}

```

**请求应答**
```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```

### 预约定时批量修改

**使用说明**

> 预约定时批量修改，适应于单设备批量定时和批量设备单定时场景<font color=red>（注意：请求公共参数header中的timezone 为必填，代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可。 ）</font>

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/group/modify`  
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
|endTime	|dateTime|	Body|	选填	|任务终止时间；不填默认值2999-12-31 23 : 59 : 59；如果有值，按照此值的有效期|
|cron	|cron[]|	Body	|选填	|任务执行表达式；|
|intervals	|int|	Body	|选填	|任务执行距当前的间隔时间，以分钟为单位，限制一天以内（0-1440），如为0需要立即执行；|
|argsInfo|	ArgsInfo|	Body|	选填|	多套指令集；当前版本只支持一套|
|systemId	|String|	Body|	选填|云应用Id|


**示例** 

**用户请求**
```java 
Header：
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: Asia/Shanghai
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  "taskInfos": [
	    {
	      "status": 2,
	      "deviceId": "DC330D003121",
	      "schedulerType": 1,
	      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
	      "intervals": 60,
	      "endTime": "2019-07-30 14:00:00",
	      "argsInfo": {
	          "cmdMsgList": [
	          {
	            "index": 0,
	            "name": "onOffStatus",
	            "value": "true"
	          },
	          {
	            "index": 1,
	            "name": "onOffStatus",
	            "value": "true"
	          }
	        ]
	      },
	      "taskName": "批量关机a111",
	      "taskDesc": "批量关机a111",
	      "taskId": "bad2adb0d756469b8ae0e292c81240ab",
	      "taskSeq": 1,
	"systemId": "SV-YDDSYY111-0010"
	    },
	    {
	      "status": 2,
	      "deviceId": "DC330D003121",
	      "schedulerType": 1,
	      "typeId": "101c120024000810e20105400000440000000000000000000000000000000000",
	      "intervals": 60,
	      "endTime": "2019-07-30 15:00:00",
	      "argsInfo": {
	        "cmdMsgList": [
	          {
	            "index": 0,
	            "name": "onOffStatus",
	            "value": "true"
	          },
	          {
	            "index": 1,
	            "name": "onOffStatus",
	            "value": "true"
	          }
	        ]
	      },
	      "taskName": "批量关机a112",
	      "taskDesc": "批量关机a112",
	      "taskId": "bad2adb0d756469b8ae0e292c81240ab",
	      "taskSeq": "2",
	"systemId": "SV-YDDSYY111-0010"
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

### 根据userid查询预约

**使用说明**

> 根据userId查询创建人或编辑人为自己预约（不做用户鉴权，有可能查到用户已取消分享的设备预约）

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/query/userid`  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	选填	|设备id；长度范围：1~16 格式：大写字母和数字 不包含特殊字符；如果不为空，根据userId和deviceId查询，为空根据userId查询|
|startNumber|	int	|Body 	|选填	|分页参数，起始值；如果不填写，默认值为1|
|length	|int|	Body| 	选填	|分页参数，长度；如果不填写，默认值为100|



**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


**示例** 

**用户请求**
```java 
Header：
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: Asia/Shanghai
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
	"detailInfo":{
	    [
			{
			"retCode": "00000",
			"retInfo": "查询成功",
			"detailInfo": {
			"taskId": "bad2adb0d756469b8ae0e292c81240ab"，
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
				"appLogo":"" ,
	            "clientId":"110001" 
				}
				]， 
			"createUserInfo": [
				{
				"userId":"111111",
				"userName":"",
				"headImg”:"",
				"phone":"139****1234"
				}
			]，
			"createTime": "2018-04-19 17:00:00"，
			"modifyUserInfo": "[
				{
				"userId":"111111",
				"userName”:"test",
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
				"week”:"?",
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
			"taskDesc":"空调开机",
	"systemId": "SV-YDDSYY111-0010"
			}
		]
	    }
	}

```


### 根据deviceid查询预约

**使用说明**

>根据deviceid查询预约详情

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/query/deviceid`  
 **HTTP Method：** POST

**输入参数**

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|deviceId|	String|	Body|	必填|设备id 长度范围：1~16 格式：大写字母和数字 不包含特殊字符|
|startNumber|	int|	Body|	选填	|分页参数，起始值；如果不填写，默认值为1|
|length|	int|	Body|	选填	|分页参数，长度；如果不填写，默认值为100|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<TaskInfo>|	Body|	必填|	返回详情；|


**示例** 

**用户请求**
```java 
Header:
	appId: MB-****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: Asia/Shanghai
	appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  “deviceId”: “DC330D32C193”,
	  “startNumber”:1,
	“length”:10
	}

```

**请求应答**
```java
{
	"retCode": "00000",
	"retInfo": "查询成功",
	“detailInfo”:{
	    [
			{
			"retCode": "00000",
			"retInfo": "查询成功",
			"detailInfo": {
			“taskId”: “bad2adb0d756469b8ae0e292c81240ab”，
			“taskSeq”: 1，
			“taskName”: “空调开机”，
			“deviceId”: “DC330D01FBF1”，
			“scheduleType”：1，
			“wifiType”:” 111c120024000810040100318000614300000001000000000000000000000000”,
			“appInfo”:[
				{
				“appId”:”1111111”,
				“appVersion”:””,
				“appName”:””,
				“appLogo”:””,
	            “clientId”:” 110001”
				}
				]， 
			“createUserInfo”: [
				{
				“userId”:”111111”,
				“userName”:"",
				“headImg”:””,
				“phone”:”139****1234”
				}
			]，
			“createTime”: “2018-04-19 17:00:00”，
			“modifyUserInfo”: “[
				{
				“userId”:”111111”,
				“userName”:”test”,
				“headImg”:”b.jpg”,
				“phone”:”139****1234”
				}
			]，
			“modifyTime”: “2018-04-19 17:00:00”，
			“cron”: [
				{
				“seconds”:”0”,
				“minutes”:”0/5”,
				“hours”:”*”,
				“day”:”*”,
				“monty”:”*”,
				“week”:”?”,
				“year”:””
				}
			],
			“endTime”:”2018-05-21 12:00:00”,
			“status”: 1,
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
			“taskDesc”:”空调开机” ,
	        "systemId": "SV-YDDSYY111-0010"
			}
		]
	    }
	}

```


### 根据taskId查询预约

**使用说明**

>根据taskid查询预约详情

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/query/taskid`  
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


**示例** 

**用户请求**
```java 
Header：
	appId: MB-*****-0000
	appVersion: *****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: *************
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: +8
	appKey: **************
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  “taskId”: “bad2adb0d756469b8ae0e292c81240ab”
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
			“taskId”: “bad2adb0d756469b8ae0e292c81240ab”，
			“taskSeq”: 1，
			“taskName”: “空调开机”，
			“deviceId”: “DC330D01FBF1”，
			“scheduleType”：1，
			“wifiType”:” 111c120024000810040100318000614300000001000000000000000000000000”,
			“appInfo”:[
				{
				“appId”:”1111111”,
				“appVersion”:””,
				“appName”:””,
				“appLogo”:”” ,
	            “clientId”:” 110001”
				}
				]， 
			“createUserInfo”: [
				{
				“userId”:”111111”,
				“userName”:"",
				“headImg”:””,
				“phone”:”139****1234”
				}
			]，
			“createTime”: “2018-04-19 17:00:00”，
			“modifyUserInfo”: “[
				{
				“userId”:”111111”,
				“userName”:”test”,
				“headImg”:”b.jpg”,
				“phone”:”139****1234”
				}
			]，
			“modifyTime”: “2018-04-19 17:00:00”，
			“cron”: [
				{
				“seconds”:”0”,
				“minutes”:”0/5”,
				“hours”:”*”,
				“day”:”*”,
				“monty”:”*”,
				“week”:”?”,
				“year”:””
				}
			],
			“endTime”:”2018-05-21 12:00:00”,
			“status”: 1,
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
			“taskDesc”:”空调开机” ,
	"systemId": "SV-YDDSYY111-0010"
			}
		]
	    }
}

```


### 预约定时执行日志查询

**使用说明**

>预约定时执行日志查询

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/query/execlog`  
 **HTTP Method：** POST

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|taskId	|String 	|URL|	可选	|任务id|
|taskSeq|	int|	Body|	必填|	子任务序号id|


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ----- |:----------:|:-----:|:--------:|:---------:|
|detailInfo|	List<BatchResultDto>|	Body|	必填	|查询到的结果|


**示例** 

**用户请求**
```java 
Header：
	appId: MB-*****-0000
	appVersion: ****.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: *******
	sign: e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: +8
	appKey: *****
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  “taskId”:“bad2adb0d756469b8ae0e292c81240ab”,
	    “taskSeq”:  “1”
	 }

```

**请求应答**
```java
{
    "batchResult": true, 
    "cmdSn": "181206135609768fa163e26700a3593", 
    "detailInfo": [
        {
            "cmdSn": "181206135609768fa163e26700a3593", 
            "cmdSubSn": "181206135609768fa163e26700a3593:_:000", 
            "deviceId": "DC330D861BB6", 
            "execResult": true, 
            "execResultCode": "00000", 
            "execResultInfo": "成功", 
            "execStep": 2, 
            "oid": 14839, 
            "resultTime": 1544075772000
        }
    ], 
    "retCode": "00000", 
    "retInfo": "成功"
}

```


### 预约任务执行预览

**使用说明**

>预约任务执行预览，任务立即下发一次

**接口描述**

?> **接入地址：**  `/scheduler/v1/device/preview`  
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


**示例**  

**用户请求**
```java 
Header：
	appId: MB-********-0000
	appVersion: *******.99990
	clientId: 123
	sequenceId: 2014022801010
	accessToken: ************
	sign:e81bc61691c9c2e6f1b8590e93a6130fb3498b8fd2786592d9265bdfc506d830
	timestamp: 1491014596343 
	language: zh-cn
	timezone: +8
	appKey: ************
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
	  “taskId”: “bad2adb0d756469b8ae0e292c81240ab”
	    “taskSeq”:1
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
[scheduler_type]:../Device/_media/_scheduler/scheduler_type.png
[scheduler_liucheng]:../Device/_media/_scheduler/scheduler_liucheng.png
[scheduler_flow]:../Device/_media/_scheduler/scheduler_flow.png



>  **当前版本**：[UWS 家庭模型服务 V1.6.0](zh-cn/ChangeLog/Family)  
 **更新时间**：{docsify-updated} 


## 简介

> 家庭模型服务实现的是一种以“家庭”为单位的智能互联设备共享组，使用此服务可以建立“家庭”组，在组下管理家庭成员及相关消息。

服务提供了创建/解散家庭、添加/删除家庭成员、邀请加入家庭及相关查询功能。可以在家庭组中进行设备共享与场景公用等操作。家庭成员（包括家庭创建者）可将自有设备分享到家庭，分享到家庭的设备，家庭成员自动具有控制权限，包含查看权，配置权，控制权等。

![家庭模型图片][family_type]


**家庭的创建与成员管理**  
为了方便家庭用户进行一致性的管理，可以通过此模型创建“家庭组”，并将家庭相关人员加入到“家庭组”，“家庭组”中的用户可以贡献家庭智能设备与家庭场景。组成员发生变动可以删除或者用户主动退出“家庭组”。  
**家庭信息维护**  
此服务包括对家庭的整体操作（如创建家庭、修改家庭名称、删除家庭等）、家庭基础信息的维护操作（如查询家庭信息列表、修改家庭名称等）、家庭成员管理（如添加家庭成员、邀请家庭成员、查看家庭成员列表等），以及其他对于家庭的管理操作。  
**家庭设备共享与权限控制**  
使用此服务将设备分享给家庭成员，实现多人共享。同时，可以限制分享权限，实现对设备的访问的可控性，增强的设备的安全性。设备的权限被划分为查看，控制，配置权。控制过程中，会检查用户的权限。实际用户控制资源时，会把用户不同途径获取的访问权限进行综合计算用户的实际权限。操作设备过程中，用户如果没有权限，设备将会被禁止操作或无法看见。

## 流程图  

![家庭模型流程][family_flow]



## 规则与约束

1、支持用户建立/解散一个或多个家庭，同时支持用户不建立家庭。<br/>
2、支持且仅支持管理员邀请/删除家庭成员。<br/>
3、支持成员加入/主动退出家庭，成员加入家庭需要家庭管理员确认。<br/>
4、不存在设备与家庭的关系，只存在设备与人的关系，人可分享设备的相关权限至家庭和个人。<br/>
5、用户的设备只可以在一个家庭中共享，不可以在多个家庭中共享，用户可选择要共享的家庭。<br/>
6、平台支持用户权限分级别：设备绑定权，设备控制权，设备信息查看权，为便携设备的绑定留足扩展空间，各APP可自行决定是否呈现给用户。<br/>
7、当用户各自带着设备进入一个家庭时，设备的相关权限可以共享，当用户离开的时候，各自带着各自设备离开，分享的相关权限解除。<br/>
8、基于扩展性考虑，平台家庭模型不做过多限制，但APP呈现端可做多种限制 。<br/>
9、用户角色：(1).用户绑定设备，成为"设备管理员" (2).自己创建家庭为"家庭管理员"(3).加入到他人家庭为"家庭成员"






## 应用场景

**家庭模型流程：**
适用于多个用户间组建管理组，即创建“家庭”；同时拥有家庭组的用户，可把自有的设备分享到家庭组中，分享设备的使用，即实现以群组为单位的设备共享。


**家庭管理**

家庭管理：用户可以创建家庭并成为家庭管理员，拥有家庭管理的相关权限，修改家庭名称、添加家庭成员等

家庭成员管理：家庭成员邀请，用户主动申请加入，修改家庭成员别名等，详见家庭模型场景流程图


**设备分享**

设备分享给个人：设备管理员（即与设备绑定的用户）根据用户ID,将设备控制、查看权限分享给其他用户

设备分享给家庭：设备管理员（即与设备绑定的用户）根据familyID，将设备分享给对应家庭，其家庭成员可以控制或查看设备



## 公共结构  
### AuthInfo  
描述权限内容信息结构。   
  
| **名称** | 权限内容信息，至少一项为true |&emsp;| AuthInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|view| Boolean | 是否有查看权 ||  
|set| Boolean | 是否有配置权限 ||  
|control| Boolean | 是否有控制权 |&emsp;|    

### QueryInfo  
描述查询条件的接口。  
 
| **名称** | 查询条件对象 | &emsp; | QueryInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|queryKey|String|查询关键字|mobile：手机，email：邮箱|  
|queryValue|String|关键字值|根据queryKey确定值|  
|accType|String|账号类型|&emsp;|  

### QueryUserInfoResult   
查询用户信息结果。  
     
| **名称** | 查询用户信息结果对象 |&emsp;| QueryUserInfoResult |
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|mobile|String|用户手机号|可能为空，由用户信息是否完整决定|  
|email|String|电子邮件|可能为空，由用户信息是否完整决定|  
|userId|String|临时分配用户userid|不为空|  
|loginName|String|用户登录名（ids注册的登录名）|不为空|  

### Permission  
描述权限信息结构。  

| **名称** | 权限信息 |&emsp;| Permission |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|  
|auth|AuthInfo|权限内容||  
|authType|String|权限类型|home:家庭分享，share：个人分享，owner：设备主人，server：给appserver的权限|  


### ShareDevice   
描述设备的共享信息内容,包含设备分享到家庭的id,设备的属主,设备名字,分享权限集。 
  
| **名称** | 设备共享信息 | &emsp;|ShareDevice |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|devInfo|DeviceBriefInfo|设备简明信息||  
|devName|String|设备名称||  
|devShareUser|UserBriefInfo|分享用户简明信息||  
|devFamilyId|String|设备所属家庭Id||  
|devOwner|UserBriefInfo|设备管理员简明信息||  
|permission|Permision|权限|&emsp;| 
|devRoomId|String|设备所属房间ID||  
|devRoomName|String|设备房间名称|&emsp;|  

### FamilyInfo    
描述家庭信息,包含家庭主人,家庭创建时间,家庭名称。     
  
| **名称** | 家庭信息 | &emsp;|FamilyInfo |    
| ------------- |:-------------:|:-----:|:-------------:|    
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型|长度19，添加请求时不填|      
|familyName|String|家庭名称| |    
|familyOwner|UserBriefInfo|家庭管理员用户简明信息|添加请求时不填| 
|ownerName|String|家庭管理员用户简明信息|最长不超过50|  
|familyLable|String|家庭标签|APP定义，如父母等|  
|familyDesc|String|家庭描述| |  
|appId|String|应用Id| |  
|createtime|date|家庭建时间|&emsp;|  
|familyLogo|String|家庭logo|默认值为平台内置家庭logo url  
|familyPicture|String|家庭图片|默认值为平台内置家庭图片 url|  
|familyLocation|Location|家庭位置信息|家庭位置信息|  
|familyPosition|String|家庭位置|小区等信息|   
|familyExternData|String|扩展信息|IOT平台可定义，jason|  
|familyLastUpdater|String|家庭最后修改人|添加请求时不填|  
|LastUpdateTime|date|家庭最后修改时间|精确到秒，含年月日信息，，添加操作请求时不填|  
|securityLevel|int|安全级别|添加操作请求时不填|  
|deviceCount|int|设备数量|添加操作请求时不填|    
|memberCount|int|成员数量|添加操作请求时不填|    

### FamilyInfoOwnerName   
描述家庭信息,包含家庭主人,家庭创建时间,家庭名称。  
  
| **名称** | 家庭信息,包含管理员的昵称 | &emsp;|FamilyInfoOwnerName |  
| ------------- |:-------------:|:-----:|:-------------:|    
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型|长度19|      
|familyName|String|家庭名称||    
|familyOwner|UserBriefInfo|家庭管理员用户简明信息||    
|ownerName|String|家庭管理员用户简明信息||  
|appId|String|应用Id|&emsp;|  
|createtime|date|家庭创建时间|&emsp;|  

### RoomInfo  
描述房间信息   
 
|**名称**|描述房间信息 |&emsp;|RoomInfo|      
| ------------- |:-------------:|:-----:|:-------------:|
|**字段名**|**类型**|**说明**|**备注**| 
|roomName|String|房间名称||
|roomId|String|房间ID||
|familyId|String|房间所属家庭ID||
|roomClass|String|房间类型||
|roomLabel|String|房间标签||
|roomCreater|UserBriefInfo|房间添加人||
|roomLogo|String|房间logo url||
|roomPicture|String|房间图片 url||
|roomExternData|String|扩展信息，使用json格式||
|roomCreateTime|Date|房间创建时间||
|fromAppid|String|来源APPID或者systemId||
|lastUpdateTime|Date|房间最后修改时间||
|lastUpdateUser|UserBriefInfo|房间最后编辑用户信息|&emsp;|

### FamilyMemberInfo    
描述家庭成员信息,包含家庭成员id,成员名称,所属家庭id。  
  
| **名称** |家庭信息 | &emsp;|FamilyMemberInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|memberInfo|UserBriefInfo|用户简明信息||    
|memberName|String|用户在家庭中的昵称||    
|familyId|String|家庭id||    
|joinTime|String|加入家庭时间|格式：`YYYY-MM-DD HH:mm:ss`|   

### DeviceBriefInfo    
| **名称** | 设备简明信息 | &emsp;|DeviceBriefInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|deviceName|String|设备名称，等同于别名||  
|deviceId|String|设备ID||  
|wifiType|String|设备wifitype||  
|deviceType|String|设备类别|| 
|online|Boolean|是否在线|&emsp;|     


### UserBriefInfo    
Map<String,String> 用户属性值key/value  

### DeviceInfo    
| **名称** | 绑定设备信息 | &emsp;|DeviceInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|deviceName|String|设备名称，等同于别名||  
|deviceId|String|设备ID||  
|wifiType|String|设备wifitype||  
|deviceType|String|设备类别|| 
|totalPermission|AuthInfo|权限和，权限信息的综合|| 
|permissions|Permission[]|权限信息 ||  
|online|Boolean|是否在线|&emsp;|     

### BaseProperty   
| **名称** | 基础属性 |&emsp;| BaseProperty |   
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|brand|String|设备品牌||  
|model|String|设备型号||  
|others|Map<String,String>|其他属性|&emsp;|       


### DeviceVersion    
| **名称** | 设备版本信息 | &emsp;|DeviceVersion |
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|deviceId|String|模块信息||  
|modules|Set<Module>|设备型号||  
|wifiType|String|wifi类型||  
|deviceType|String|设备类型||  
|basePropertiy|BaseProperty|品牌信息||  
|location|Location|位置信息|&emsp;|      
  
### Module    
| **名称** | 模块信息 | &emsp;|Module |
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|moduleId|String|模块ID||  
|moduleType|String|模块类型||  
|moduleInfos|Map<String,String>|模块其他信息|&emsp;|         

### Location 
|**名称**	|模块信息 |&emsp;|Location|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|longitude|Double|经度||  
|latitude|Double|维度||  
|cityCode|String|城市编码|&emsp;|       


### QueryClause 
|**名称**	|查询条件 |&emsp;|QueryClause|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|queryInfo |Map<String,String>|查询条件信息|&emsp;|   
 

## 接口清单  

### 家庭基础服务
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| 家庭管理员创建家庭     | 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码 | 是| 无| 
| 家庭管理员创建家庭带管理员昵称    | 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码| 是| 无| 
| 家庭管理员修改家庭名称     | 用户修改拥有的家庭的名称，发送修改家庭信息消息给家庭成员,记录发送结果到日志 | 是| 无| 
| 家庭管理员或家庭成员查询家庭信息     | 家庭管理员或家庭成员查询家庭信息 | 是| 无| 
| 家庭管理员查询创建家庭信息列表     | 验证用户token,确认用户是否为家庭主人,查询家庭信息,并返回 | 是| 无| 
| 家庭成员查询加入的家庭信息列表     | 验证用户token,确认用户是否为家庭成员,查询加入的家庭信息,并返回 | 是| 无| 
| 家庭管理员删除家庭     | 家庭管理员删除家庭,收回用户分享给家庭的设备权限,收回家庭成员的家庭分享设备权限，向删除前家庭成员发送删除家庭消息，记录消息推送结果到日志 | 是| 无| 
| 家庭管理员添加家庭成员    | 家庭管理员添加家庭成员,分享家庭设备权限给成员，发送家庭成员添加家庭成员消息,支持memberId为临时的userid | 是| 无| 
| 家庭管理员邀请用户加入家庭     | 家庭管理员邀请用户加入家庭，并向用户下发推送邀请码, 支持targetId为临时的userid | 是| 无| 
| 家庭成员回复家庭管理员邀请     | 家庭成员接受家庭管理员邀请，加入家庭,分享家庭设备权限给成员，并发送添加家庭成员消息给家庭成员；如果拒绝，本次邀请结束，发送用户拒绝加入家庭消息到家庭管理员，记录消息推送结果到日志| 是| 无| 
| 家庭管理员或家庭成员修改家庭成员名称     | 家庭管理员修改家庭成员信息，其中包含家庭管理员在家庭中的昵称家庭成员可以修改自己的信息，发送修改家庭成员信息消息给家庭成员，记录消息推送结果到日志| 是| 无| 
| 家庭管理员删除家庭成员     | 家庭主人删除家庭成员,并解除成员在家庭中分享的设备关系，并收回成员分享给家庭的设备，发送删除家庭成员消息给家庭全体成员，记录消息推送结果到日志 | 是| 无| 
| 家庭成员主动退出家庭     | 家庭成员主动退出家庭,撤销家庭成员的家庭设备分享,撤销其他家庭成员对本家庭成员设备的分享权限，向家庭其他成员发送删除家庭成员消息 | 是| 无| 
| 家庭管理员或家庭成员查询家庭成员     | 家庭管理员或家庭成员查询家庭成员 | 是| 无| 
| 家庭成员或家庭管理员查询家庭成员所有成员（包含管理员）     | 家庭管理员或家庭成员查询家庭成员 | 是| 无| 
| 根据关键字精确检索好友信息     | 精确查找用户信息，用于执行需要用户ID的场景，本次用户id有时效性，临时分配，有效期为1天，支持其他接口使用，在相关接口中有说明 | 是| 无|  
|家庭管理员变更 | 家庭管理员可以主动移交管理员角色 | 是| 无|  
|查询家庭下的房间列表 | 查询家庭下的房间列表信息 | 是| 无| 

#### 家庭管理员创建家庭
> 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/family `  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|

**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"familyInfo":{
"familyName":"我家"
	}
}


```  

**请求应答**

```java
{
"familyInfo":{
"familyName":"我家",
"familyId":"10086",
"familyOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
		}
	},
"appId":"MB-PORTAL-0000"，
"createtime":"2016-10-01 12:00:00"
}
   "retCode": "00000",
   "retInfo": "成功"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员创建家庭带管理员昵称(新增)
> 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/familyWithOwnerName `  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名   | 位置  | 必填|说明|
| ---- |:-------:|:-----:|:------:|:------:|
|  FamilyInfoWithOwnerName   | familyInfo | Body| 必填|家庭信息|

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfoWithOwnerName |  familyInfo  |   Body  |  必填  | 家庭信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"familyInfo":{
"familyName":"我家"，
"ownerName":"admin100013957366154693"
	}
}



```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功","familyInfo":{"familyId":"566070358640000000","familyName":"我的家庭1","familyOwner":{"userId":"100013957366154693","mobile":"136****8934"},"ownerName":"admin100013957366154693","appId":"MB-UBOT-0009","createTime":"2018-01-03 16:17:20"}}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员修改家庭名称
> 用户修改拥有的家庭的名称，发送修改家庭信息消息给家庭成员,记录发送结果到日志    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family `  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:--------:|:--------:|
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|  
|  String    | familyId | url| 必填|家庭id|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"familyInfo":{
"familyName":"你家"
	}
}

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}
```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108   

#### 家庭管理员或家庭成员查询家庭信息
> 家庭管理员或家庭成员查询家庭信息    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family `  
 **HTTP Method：** GET

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | url| 必填|家庭id|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"familyInfo":{
"familyName":"我家",
"familyId":"10086",
"familyOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
},
"appId":"MB-PORTAL-0000"
}
   "retCode": "00000",
   "retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109   

#### 家庭管理员查询创建家庭信息列表
> 验证用户token,确认用户是否为家庭主人,查询家庭信息,并返回    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/createdFamilies `  
 **HTTP Method：** GET

**输入参数**  

| 类型  | 参数名 | 位置  | 必填|说明|
| ---- |:------:|:-----:|:-------:|:-------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo[] |  families  |   Body  |  必填  | 家庭信息列表 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"families":[{
"familyName":"我家",
"familyId":"10005618",
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"
},
{
"familyName":"你家",
"familyId":"1000569",
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"
}]
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

#### 家庭成员查询加入的家庭信息列表
> 验证用户token,确认用户是否为家庭成员,查询加入的家庭信息,并返回    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/families`  
 **HTTP Method：** GET

**输入参数**  

| 类型      | 参数名   | 位置  | 必填|说明|
| -------- |:--------:|:-----:|:---------:|:---------:|  
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo[] |  families  |   Body  |  必填  | 家庭信息列表 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"families":[{
"familyName":"测试1",
"familyId":"20021003012012",
"familyOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
},
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"
},
{
"familyName":"测试3",
"familyId":"20021003012012",
"familyOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
},
"appId":"MB-PORTAL-0000",
"createtime":"2016-09-28 10:00:00"

}]
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

#### 家庭管理员删除家庭
> 家庭管理员删除家庭,收回用户分享给家庭的设备权限,收回家庭成员的家庭分享设备权限，向删除前家庭成员发送删除家庭消息，记录消息推送结果到日志   

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名    | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url | 必填 |家庭id |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108    

#### 家庭管理员添加家庭成员
> 家庭管理员添加家庭成员,分享家庭设备权限给成员，发送家庭成员添加家庭成员消息,支持memberId为临时的userid  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | memberId   | Body |必填|家庭成员id |  
| String  | memberName   | Body |必填|家庭成员名称 |  
| String  | familyId   | Body |必填|家庭id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"familyId":"105876",
"memberId":"10008573",
"memberName":"老大"
}

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31105、E31108  

#### 家庭管理员邀请用户加入家庭
> 家庭管理员邀请用户加入家庭，并向用户下发推送邀请码, 支持targetId为临时的userid,邀请生成正确,推送失败时,返回给发起方家庭ID和邀请码。

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/{familyId}/{targetId}/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | targetId   | url |必填|被邀请用户id |  
| String  | familyId   | url |必填|家庭id |  
| String  | familyId  | Body |必填|家庭成员在家庭中名称 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| String |  familyId  |   body |  必填  | 家庭id|
| String |  inviteCode  |   body |  必填  | 用户邀请码|  

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"memberName": "大家长"
}

```  

**请求应答**

```java
{
"familyId": "11389",
"inviteCode": "123034",
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、H00001  
    
#### 家庭成员回复邀请加入家庭
> 家庭成员接受家庭管理员邀请，加入家庭，,分享家庭设备权限给成员，并发送添加家庭成员消息给家庭成员；如果拒绝，本次邀请结束，发送用户拒绝加入家庭消息到家庭管理员，记录消息推送结果到日志  


##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/agreeInvitation/{familyId}/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId   | url |必填|家庭id|  
| String  | inviteCode   | body |必填|邀请码 |  
| String  | memberName  | Body |必填|家庭成员在家庭中名称 | 
| Boolean  | agree  | Body |必填|true，同意，false不同意 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"inviteCode": "123034",
"agree": true
 }

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、E31131、E31132、E31133    

#### 家庭管理员或家庭成员修改家庭成员名称
> 家庭管理员修改家庭成员信息，其中包含家庭管理员在家庭中的昵称，家庭成员可以修改自己的信息，发送修改家庭成员信息消息给家庭成员，记录消息推送结果到日志  
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/{memberId}/familyMember`  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| FamilyMemberInfo  | memberInfo   | Body |必填|家庭成员信息|  
| String  | familyId   | url |必填|家庭id |  
| String  | memberId  | url |必填|家庭成员id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"memberInfo":
{
"memberName":"老大"
}
}


```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109  

#### 家庭管理员删除家庭成员
> 家庭主人删除家庭成员,并解除成员在家庭中分享的设备关系，并收回成员分享给家庭的设备，发送删除家庭成员消息给家庭全体成员，记录消息推送结果到日志 
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/{memberId}/familyMember`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | memberId   | url |必填|用户id |  
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31107、E31108  


#### 家庭成员主动退出家庭
> 家庭成员主动退出家庭,撤销家庭成员的家庭设备分享,撤销其他家庭成员对本家庭成员设备的分享权限，向家庭其他成员发送删除家庭成员消息  
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/familyMember`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp;  |  &emsp;   |   &emsp;  |  &emsp;   | &emsp; |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}


```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108   

#### 家庭管理员或家庭成员查询家庭成员
>家庭管理员或家庭成员查询家庭成员  
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/familyMembers`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{

"familyMembers":[
{"familyId":"105876",
"memberInfo":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"memberName":"测试1","joinTime ":"2016-11-01 00:00:00"},
{"familyId":"105876",
"memberInfo":{
"userId: "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
"userSex": "male"
},
"memberName":"测试2""joinTime ":"2016-11-01 00:00:00"},
{"familyId":"105876",
"memberInfo":{
"userId: "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
"userSex": "male"
},
"memberName":"测试3""joinTime ":"2016-11-01 00:00:00"}
],
"retCode":"00000",
"retInfo":"成功"

}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109     

#### 家庭成员或家庭管理员查询家庭成员所有成员（包含管理员）
>家庭管理员或家庭成员查询家庭成员 
 
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/allFamilyMembers`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java

{"retCode":"00000","retInfo":"成功","familyMembers":[{"memberInfo":{"userId":"100013957366154693","mobile":"136****8934"},"memberName":"admin100013957366154693","familyId":"566070358640000000","joinTime":"2018-01-03 16:17:20"}]}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109      

#### 根据关键字精确检索好友信息（新增）
>精确查找用户信息，用于执行需要用户ID的场景，本次用户id有时效性，临时分配，有效期为1天，支持其他接口使用，在相关接口中有说明,同时屏蔽用户敏感信息,包含手机号,邮箱,登录名。
 
##### 1、接口定义
?> **接入地 址：**  `//ufm/v1/protected/familyService/userInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| QueryInfo  | queryInfo  | body |必填|查询条件 | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| QueryUserInfoResult |  qUserR   |   Body  |  必填   | 用户信息 |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"queryInfo":{
"queryKey":"mobile",
"queryValue":"13800000000"，
"accType":"0"，
}
}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功","qUserInfo":{"userId":"1566070351894000000","mobile":"13621108934","email":"chenj@haierubic.com","loginName":"H642R1510027712631"}}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31301  

### 设备分享管理服务
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| 家庭管理员或家庭成员查询家庭的所有设备     | 家庭管理员或家庭成员查询家庭成员分享给家庭的所有设备 | 是| 无| 
| 设备管理员查询设备管理员分享给个人的所有设备    | 查询设备管理员分享给个人的所有设备| 是| 无| 
| 查询设备管理员分享给家庭的设备     | 查询设备管理员分享给家庭的所有设备 | 是| 无| 
| 家庭成员查询分享给我的所有家庭设备    | 家庭成员查询分享给我的所有家庭设备 | 是| 无| 
| 普通用户查询分享给我的个人分享设备     | 查询分享给我的所有个人分享设备 | 是| 无| 
| 设备管理员查询自有设备列表     | 查询用户自有设备列表 | 是| 无| 
| 家庭成员或家庭管理员分享设备给家庭    | 家庭成员或家庭管理员作为设备的管理员分享设备给家庭，发送分享家庭设备消息给家庭成员，包含管理员，记录消息发送结果到日志 | 是| 无| 
| 家庭管理员取消设备的家庭分享    | 用户取消分享给家庭的设备,收回分享给家庭用户的设备家庭权限，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志 | 是| 无| 
| 设备管理员取消设备的家庭分享     | 设备管理员取消分享给家庭的设备，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志 | 是| 无| 
| 设备管理员分享设备给个人     | 设备管理员分享设备给个人，发送分享个人设备消息给目标用户，记录消息发送结果到日志，支持targetId为临时的userid| 是| 无| 
| 设备管理员取消设备分享    | 设备管理员取消用户分享，发送取消个人分享设备消息给目标用户，记录消息发送结果到日志, 支持targetId为临时的userid| 是| 无| 
| 用户删除分享自己的个人设备    | 用户取消设备管理员分享给自己的设备 | 是| 无| 
| 家庭管理员或设备管理员修改设备所属房间 | 家庭管理员或设备管理员修改设备所属房间 | 是| 无|


#### 家庭管理员或家庭成员查询家庭的所有设备
> 家庭管理员或家庭成员查询家庭成员分享给家庭的所有设备

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ family/{familyid}/shareDevices`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  String    | familyId | url| 必填|家庭Id|

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{

" shareDevs ":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devFamilyId":"1",
"devOwner":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}
},
{
"devId":"12346",
"devName":"测试1",
"devFamilyId":"1",
"devOwner":{
"userId: "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
},
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}
}
],
"retCode":"00000",
"retInfo":"成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109   

#### 设备管理员查询设备管理员分享给个人的所有设备
> 查询设备管理员分享给个人的所有设备  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ person/shareDevices `  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;    | &emsp; | &emsp;| &emsp;|&emsp;|

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  |共享设备信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"shareDevs":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},

"devName":"测试1",
"devShareUser":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}
},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试2",
"devShareUser":{
"userId": "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
},
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}
}
],
"retCode":"00000",
"retInfo":"成功"

}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

#### 设备管理员查询分享给家庭的所有设备
> 查询设备管理员分享给家庭的所有设备   

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ family/shareDevices`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |    

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs |   Body|  必填  | 共享设备信息 |  

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"shareDevs ":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
}
,
"devName":"测试1",
"devFamilyId":"1006785",
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}

},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devFamilyId":"1006785",
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}
}
],
"retCode":"00000",
"retInfo":"成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

#### 家庭成员查询分享给我的所有家庭设备
> 家庭成员查询分享给我的所有家庭设备    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ family/shareDevices2me `  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
shareDevs ":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devFamilyId":"1006785",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}

},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devFamilyId":"1006785",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"home",
"auth":{"view":true,"set":false,"control":false}
}
}],
    "retCode": "00000", 
    "retInfo": "成功", 
   
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008     

#### 普通用户查询分享给我的个人分享设备
> 查询分享给我的所有个人分享设备   

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/shareDevices2me `  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
shareDevs ":[
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

},
{
"devInfo":{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
},
"devName":"测试1",
"devOwner":{
"userId":  "10811563273",
"userNickName": "xiaoyi",
"userHeadImg": "https://uhome.haier.com/resource/headimg",
"userAge": "10811563273",
"userAddr": "10811563273",
“userSex”: “male”
}
}，
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

}
],
    "retCode": "00000", 
    "retInfo": "成功", 
   
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008     

#### 设备管理员查询自有设备列表
> 查询用户自有设备列表    

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/devices`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  &emsp;     | &emsp;  | &emsp; | &emsp; |&emsp; |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| DeviceBriefInfo [] |  devices  |   Body  |  必填  |  &emsp;|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
```  

**请求应答**

```java
{
"devices ":[{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
}, 
{
"deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
"deviceType": "00000",
online":false
}], 
"retCode":"00000","retInfo":"成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

#### 家庭成员或家庭管理员分享设备给家庭
> 家庭成员或家庭管理员作为设备的管理员分享设备给家庭，发送分享家庭设备消息给家庭成员，包含管理员，记录消息发送结果到日志   

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/shareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice  | shareDev  | Body | 必填 |设备的分享信息 |  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"shareDev":{
"devInfo":{           
"deviceId": "100823"
},
"devName":"test",
"permission":{ "authType":"home",
"auth":{ "view": true, "set": false, "control": false}},
" devFamilyId":"1256738"
}
}

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31111      

#### 家庭管理员取消设备的家庭分享
> 用户取消分享给家庭的设备,收回分享给家庭用户的设备家庭权限，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/ {familyId}/manager/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | familyId   | url |必填|家庭id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108   

#### 设备管理员取消设备的家庭分享
> 设备管理员取消分享给家庭的设备，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/ {familyId}/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | familyId   | url |必填|家庭id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108  

#### 家庭管理员取消设备的家庭分享
> 用户取消分享给家庭的设备,收回分享给家庭用户的设备家庭权限，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/ {familyId}/manager/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | familyId   | url |必填|家庭id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108   

#### 设备管理员分享设备给个人
> 设备管理员分享设备给个人，发送分享个人设备消息给目标用户，记录消息发送结果到日志，支持targetId为临时的userid

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{targetId}/shareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice  | shareDev   | Body |必填|设备的分享信息 |  
| String  | targetId   | url |必填|分享设备的目标用户 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body:
{
"shareDev":{
"devInfo":{           
"deviceId": "100823",
},
"devName":"test",
"permission":{
"authType":"share",
"auth":{"view":true,"set":false,"control":false}
}

}
}


```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，F31225   
  

#### 设备管理员取消设备分享
> 设备管理员取消用户分享，发送取消个人分享设备消息给目标用户，记录消息发送结果到日志, 支持targetId为临时的userid

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/ {targetId}/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | targetId   | url |必填|设备分享者 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| -------------|:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

#### 用户删除分享给自己的个人设备
> 用户取消设备管理员分享给自己的设备

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{devId}/shareDeviceToMe`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| -------------|:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

#### 家庭管理员或设备管理员修改设备所属房间
> 用户作为管理员或者作为家庭成员，并且是设备管理员修改设备信息，其中包含设备所在房间，设备在家庭中的昵称，设备分享权限，要修改的设备为1个或多个，这些设备都属于一个家庭

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/updateShareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice[]  | shareDevices   | body |必填|设备信息 |  
| String  | familyId   | body |必填|家庭ID |  

**输入参数说明**   
**名称**	|设备共享信息 |&emsp;|ShareDevice|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|devId|String|设备MAC|必填|  
|devName|String|设备名称|选填| 
|devRoomId|String|设备所属房间ID|选填| 
|permission|Permision|分享权限此处权限authType 必须为home|选填| 


**输出参数**  
标准输出

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

Body：
{
	"shareDevices": [{
		"devRoomId": "694111057263000000",
		"devInfo": {
			"deviceId": "MOCKDEV_CJL-UFM-MEMBER"
		}
	}],
	"familyId": "332111080977000000"
}

```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功"
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

### 房间管理服务
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|  
| ------------- |:-------------:|:-----:|:-------------:|
| 用户创建房间     | 用户创建房间到家庭下，房间必须属于家庭 | 是| 无|   
| 用户修改房间信息 | 用户修改家庭下房间信息| 是| 无|  
| 用户删除房间信息     | 房间必须没有设备才能被删除 | 是| 无|  
| 用户查询家庭房间下的设备| 家庭管理员或家庭成员查询用户房间下的设备信息列表 | 是| 无|  

#### 用户创建房间
>用户创建房间到家庭下，房间必须属于家庭  

##### 1、接口定义  
?> **接入地 址：**  `/ufm/v1/protected/roomService/addRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| --------- |:----------:|:-----:|:-----------:|:-----------:|  
|  RoomInfo    | roomInfo | Body| 必填|房间信息|   

**输入参数对象说明**   

|**名称**	|描述房间信息 |&emsp;|RoomInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomName|String|房间名称|必填|  
|familyId|String|房间所属家庭ID|必填|  
|roomClass|String|房间类型|非必填|  
|roomLabel|String|房间标签|非必填| 
|roomLogo|String|房间logo url|非必填| 
|roomPicture|String|房间图片 url|非必填| 
|roomExternData|String|扩展信息，使用json格式|非必填|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| RoomInfo |  roomInfo  |   Body  |  必填  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

Body:
{
	"roomInfo": {
		"roomName": "testroorm1",
		"familyId": 878111118405000000
	}
}


```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"roomInfo": {
		"roomName": "testroorm1",
		"roomId": "150111135560000000",
		"familyId": "878111118405000000",
		"roomCreater": {
			"userId": "100013957366152524",
			"mobile": "136****8934"
		},
		"roomCreateTime": "2019-02-15 09:52:40"
	}
}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008

#### 用户修改房间信息  
>用户修改家庭下房间信息，有值的属性信息会被更新，为空或未配置的不填  
##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/roomService/updateRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**  
 
|**名称**	|描述房间信息 |&emsp;|RoomInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomName|String|房间名称|非必填|  
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填|  
|roomClass|String|房间类型|非必填|  
|roomLabel|String|房间标签|非必填| 
|roomLogo|String|房间logo url|非必填| 
|roomPicture|String|房间图片 url|非必填| 
|roomExternData|String|扩展信息，使用json格式|非必填|  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| RoomInfo |  roomInfo  |   Body  |  必填  | &emsp; |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

Body:
{
	"roomInfo": {
		"familyId": "11111",
		"roomId": "1111111",
		"roomLabel": "测试"
	}
}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008

#### 用户删除房间信息
> 用户删除家庭下的房间，用户为家庭成员或家庭管理员，房间必须没有设备才能被删除  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/roomService/delRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**  
   
|**名称**	|描述房间信息 |&emsp;|RoomInfo|  
| ------ |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填|  
 


**输出参数**  

标准输出

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

Body:
{
"roomInfo":{ "familyId":"11111","roomId":"1111111"}
}


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```
##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008

#### 用户查询家庭房间下的设备
> 家庭管理员或家庭成员查询用户房间下的设备信息列表，其他查询用户返回用户无权限  

##### 1、接口定义
?> **接入地 址：**  `/ufm/v1/protected/roomService/queryRoomDevices`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------- |:-------------:|:-----:|:-----------:|:----------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**    
 
|**名称**	|描述房间信息 |&emsp;|RoomInfo|
| ---------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填|  
 


**输出参数**   
 
| 类型         | 参数名         | 位置  | 必填|说明|
| ---------- |:----------:|:-----:|:-------------:|:-------------:|
|  shareDevice[]    | shareDevices | Body| 必填|&emsp;| 

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功", "shareDevices":[{},{},{}]}

```
##### 3、错误码  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008

   
  


[^-^]:常用图片注释
[family_type]:_media/_family/family_type.png
[family_liucheng]:_media/_family/family_liucheng.png
[family_flow]:_media/_family/family_flow.png




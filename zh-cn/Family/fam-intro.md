# 介绍
!> **更新时间**：{docsify-updated}  

## 简介
>  家庭类服务实现的是一种以物理“家庭”为单位的数字化家庭体现，围绕物理家庭中的人、物、空间、信息及服务构建用户在数字家庭中的智能体验。



## 公共参数


**规则与约束**

>请应用放提前了解；

1、支持用户建立/解散一个或多个家庭，同时支持用户不建立家庭。<br/>
2、支持且仅支持管理员邀请/删除家庭成员。<br/>
3、支持成员加入/主动退出家庭，成员加入家庭需要家庭管理员确认。<br/>
4、不存在设备与家庭的关系，只存在设备与人的关系，人可分享设备的相关权限至家庭和个人。<br/>
5、用户的设备只可以在一个家庭中共享，不可以在多个家庭中共享，用户可选择要共享的家庭。<br/>
6、平台支持用户权限分级别：设备绑定权，设备控制权，设备信息查看权，为便携设备的绑定留足扩展空间，各APP可自行决定是否呈现给用户。<br/>
7、当用户各自带着设备进入一个家庭时，设备的相关权限可以共享，当用户离开的时候，各自带着各自设备离开，分享的相关权限解除。<br/>
8、基于扩展性考虑，平台家庭模型不做过多限制，但APP呈现端可做多种限制 。<br/>
9、用户角色：(1).用户绑定设备，成为"设备管理员" (2).自己创建家庭为"家庭管理员"(3).加入到他人家庭为"家庭成员"<br/>
10、创建家庭数限制：一个用户最多是20个家庭的管理员（ps：默认创建家庭用户成为家庭管理员，当管理员移交角色后，可重新创建家庭。）<br/>
11、加入家庭数限制：一个用户最多可加入50个家庭，即最多是50个家庭的成员（不含管理员）。<br/>
12、一个用户最多在70个家庭中。<br/>
13、家庭资源数限制：<br/>
	（1）设备数：一个家庭下最多加入500个设备。<br/>
	（2）成员数：一个家庭下最多50个成员，含管理员用户。<br/>
	（3）虚拟成员数：一个家庭下最多20个虚拟成员。


**AuthInfo** 
描述权限内容信息结构。   
  
| **名称** | 权限内容信息，至少一项为true |&emsp;| AuthInfo |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|view| Boolean | 是否有查看权 ||  
|set| Boolean | 是否有配置权限 ||  
|control| Boolean | 是否有控制权 |&emsp;|    

**QueryInfo**  
描述查询条件的接口。  
 
| **名称** | 查询条件对象 | &emsp; | QueryInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|queryKey|String|查询关键字|mobile：手机，email：邮箱|  
|queryValue|String|关键字值|根据queryKey确定值|  
|accType|String|账号类型|&emsp;|  

**QueryUserInfoResult**   
查询用户信息结果。  
     
| **名称** | 查询用户信息结果对象 |&emsp;| QueryUserInfoResult |
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|mobile|String|用户手机号|可能为空，由用户信息是否完整决定|  
|email|String|电子邮件|可能为空，由用户信息是否完整决定|  
|userId|String|临时分配用户userid|不为空|  
|loginName|String|用户登录名（ids注册的登录名）|不为空|  

**Permission**  
描述权限信息结构。  

| **名称** | 权限信息 |&emsp;| Permission |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|  
|auth|AuthInfo|权限内容||  
|authType|String|权限类型|home:家庭分享，share：个人分享，owner：设备主人，server：给appserver的权限|  


**ShareDevice**   
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

**FamilyInfo**    
描述家庭信息,包含家庭主人,家庭创建时间,家庭名称。     
  
| **名称** | 家庭信息 | &emsp;|FamilyInfo |    
| ------------- |:-------------:|:-----:|:-------------:|    
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型|长度19，添加请求时不填|      
|familyName|String|家庭名称| 不超过64位|    
|familyOwner|UserBriefInfo|家庭管理员用户简明信息|添加请求时不填| 
|ownerName|String|家庭管理员用户简明信息|最长不超过32|  
|familyLable|String|家庭标签|APP定义，如父母等|  
|familyDesc|String|家庭描述| 不超过1K|  
|appId|String|应用Id| |  
|createtime|date|家庭建时间|&emsp;|  
|familyLogo|String|家庭logo|默认值为平台内置家庭logo url，不超过1K|  
|familyPicture|String|家庭图片|默认值为平台内置家庭图片 url，不超过1K|  
|familyLocation|Location|家庭位置信息|家庭位置信息|  
|familyPosition|String|家庭位置|小区等信息|   
|familyExternData|String|扩展信息|IOT平台可定义，json|  
|familyLastUpdater|String|家庭最后修改人|添加请求时不填|  
|LastUpdateTime|date|家庭最后修改时间|精确到秒，含年月日信息，，添加操作请求时不填|  
|securityLevel|int|安全级别|添加操作请求时不填|  
|deviceCount|int|设备数量|添加操作请求时不填|    
|memberCount|int|成员数量|添加操作请求时不填|    

**FamilyInfoOwnerName**   
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

**RoomInfo**  
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

**FamilyMemberInfo**    
描述家庭成员信息,包含家庭成员id,成员名称,所属家庭id。  
  
| **名称** |家庭信息 | &emsp;|FamilyMemberInfo |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|memberInfo|UserBriefInfo|用户简明信息||    
|memberName|String|用户在家庭中的昵称||    
|familyId|String|家庭id||    
|joinTime|String|加入家庭时间|格式：`YYYY-MM-DD HH : mm : ss`|   

**DeviceBriefInfo**    

| **名称** | 设备简明信息 | &emsp;|DeviceBriefInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|deviceName|String|设备名称，等同于别名|&emsp;|  
|deviceId|String|设备ID|&emsp; |  
|bindTime|String|设备被绑定时间|格式“YYYY-MM-DD HH : mm : ss” |  
|wifiType|String|设备wifitype|&emsp;|  
|deviceType|String|设备类别|&emsp;| 
|online|Boolean|是否在线|&emsp;|     
|productNameT|String|设备型号名称|&emsp;|  
|productCodeT|String|设备型号代码|&emsp;|  
|slaveDeviceIds|String[]|子设备ID列表|没有子设备不返回|  
|parentsDeviceId|String|父设备ID|没有父设备不返回|  
|deviceRole|String|设备角色|主设备/子设备/无主从关系，枚举值：设备角色：</br>1 普通设备;</br>2 网关设备;</br>3 附件设备;</br>4 子设备;</br>仅返回数字，角色信息不返回，没有角色信息不返回|  
|deviceRoleType|String|设备角色类型|目前为附件设备，其他设备暂时未扩展 ，1，附件设备，目前改字段未使用，没有角色信息不返回|  
|IsMeshDevice|Boolean|是否为mesh设备|&emsp;| 
|meshGroupId|String|组设备ID|如果不是组设备，该值不存在| 
|meshGroupType|String|组设备类型|组内设备groupInner，组设备group| 

**UserBriefInfo**    
Map<String,String> 用户属性值key/value  
下表为存在的key,其他key属性需要忽略

| **名称** | 用户简明信息 | &emsp;|UserBriefInfo |
| ------------- |:-------------:|:-----:|:-------------:|   
|**字段名**|**类型**|**说明**|**备注**|    
|name|	String|	昵称|&emsp;|	
|email|	String|	电子邮件|&emsp;|	
|userId|	String|	用户id|	不为空|
|mobile|	String|	手机号|	&emsp;|
|avatarUrl|	String|	头像url|	用户中心提供的头像地址|
|isVirtualUser|	String|	是否为虚拟用户|	true，虚拟用户，false，实体用户|
|hostUserId|	String|	2.7.1.6	FamilyInfo宿主用户的IOT平台userid|	&emsp;|
|ucUserId|	String|	用户中心userId|&emsp;|	



**Location** 

|**名称**	|模块信息 |&emsp;|Location|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|longitude|Double|经度||  
|latitude|Double|维度||  
|cityCode|String|城市编码|&emsp;|       


**QueryClause** 

|**名称**	|查询条件 |&emsp;|QueryClause|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|queryInfo |Map<String,String>|查询条件信息|&emsp;|   



**FamilyShareDevice**

描述设备诶的共享信息内容，包含设备分享到家庭的id、设备主人、设备名称、  
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符
devName|String|设备名称|
devOwner|String|设备主人|
devFamilyId|String|设备所属家庭Id|
permission|Permission|权限|   




**DeviceRoomInfoDto**  

  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|
shareTime|String|设备分享时间|
wifiType|String|设备wifitype|
deviceType|String|设备类别|
room|String|设备房间位置信息|   
permission|Permission[]|权限信息|   
online|Boolean|是否在线|
productNameT|String|设备型号名称| 
productCodeT|String|设备型号代码|    
slaveDeviceIds|String|子设备ID列表|
parentsDeviceId|String|父设备ID|
deviceRole|String|设备角色|主设备/子设备/无主从关系，枚举值：设备角色：</br>1 普通设备;</br>2 网关设备;</br>3 附件设备;</br>4 子设备;</br>仅返回数字，角色信息不返回
deviceRoleType|String|设备角色类型|目前为附件设备，其他设备暂时未扩展 ，1，附件设备，目前改字段未使用
IsMeshDevice|Boolean|是否为mesh设备|
meshGroupId|String|组设备ID|如果不是组设备，该值不存在
meshGroupType|String|组设备类型|组内设备groupInner，组设备group

**FamilyQRCodeInfo**    

家庭模型快速响应码  
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
shareUrl|String|分享url|
event|String|快速响应码要处理的事件|
familyId|String|家庭ID|
timeout|int|按秒计算，为二维码有效期|
params|String|附加参数，数字字母最长64位|   


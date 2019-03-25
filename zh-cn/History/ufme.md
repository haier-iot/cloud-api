
>  **当前版本**：[UWS 家庭模型ufme服务 V0.1.0](zh-cn/ChangeLog/Family)  
 **更新时间**：{docsify-updated} 


## 企业版注意事项

### 权限申请

企业版服务授权，使用服务IP白名单策略，需要是用此服务请联系IOT平台技术支持开通系统IP白名单

### 公共头部分

Header 中appid 字段填写内容为系统ID，即systemid。 此字段需要在海极网开通云应用获得。

**开通流程如下**

> “海极网” -->  “开发者中心” --> “我的产品” --> “我的云应用”



## 公共结构  

### AuthInfo

描述权限内容信息结构。   
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
view|Boolean|是否有查看权|
set|Boolean|是否有配置权限|
control|Boolean|是否有控制权限|


### Permission

描述权限信息结构   
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
auth|AuthInfo|权限内容|
authType|String|权限类型|home:家庭分享</br>share：个人分享</br>owner：设备主人<//br>server：给appserver的权限

### FamilyShareDevice

描述设备诶的共享信息内容，包含设备分享到家庭的id、设备主人、设备名称、  
  
字段名|类型|说明|备注
:-:|:-:|:-:|:-
deviceId|String|设备ID|
devName|String|设备名称|
devOwner|String|设备主人|
devFamilyId|String|设备所属家庭Id|
permission|Permission|权限|

### FamilyInfo

描述家庭信息，包含家庭创建时间，家庭名称

字段名|类型|说明|备注
:-:|:-:|:-:|:-
familyId|String|家庭id，以字符串形式传递Long型变量，会自动转换字符串未核实的整型|
familyName|String|家庭名称|
familyOwner|UserBriefInfo|家庭管理员用户简明信息|
appId|String|应用Id|
createtime|date|家庭创建时间|

### FamilyMemberInfo

描述家庭成员信息，包含家庭主人，家庭创建时间，家庭名称

字段名|类型|说明|备注
:-:|:-:|:-:|:-
memberInfo|String|用户ID|
memberName|String|用户在家庭中的昵称|
familyId|String|家庭id|
joinTime|String|加入家庭时间|格式YYYY-MM-DD HH:mm:ss


## 接口清单  

### 家庭基础服务

#### 根据家庭ID查询家庭信息


##### 1、接口定义
?> **接入地 址：**  `/ufme/familyService/familyInfosById`  
 **HTTP Method：** POST

**输入参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyId|String|Body|必填|家庭id

**输出参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyInfo[]|FamilyInfo|Body|必填|家庭信息


**输出参数对象说明**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyId|String|家庭id|必填
familyName|String|家庭名称|必填
familyOwner|String|家庭管理员ID|必填
appId|String|应用Id|必填
createtime|date|家庭创建时间|必填

##### 2、请求样例  

**用户请求**
```java
{
	"familyId":"********"
}
```
**请求应答**

```java
{
"familyInfo":{
"familyName":"我家",
"familyId":"10086",
"familyOwner":"111111022233",
"appId":"MB-PORTAL-0000"
}
   "retCode": "00000",
   "retInfo": "成功"
}
```

#### 查询设备所属家庭信息

> 根据设备mac查询设备被分享的家庭信息


##### 1、接口定义
?> **接入地 址：**  `/ufme/familyService/deviceBelongFamilies`  
 **HTTP Method：** POST

**输入参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID

**输出参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyInfo|FamilyInfo[]|Body|必填|家庭信息

**输出参数对象说明**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyId|String|家庭id|必填
familyName|String|家庭名称|必填
familyOwner|String|家庭管理员ID|必填
appId|String|应用Id|必填
createtime|date|家庭创建时间|必填

##### 2、请求样例

**请求明细**

```java
{
	"deviceId":"********"
}
```

**请求应答**

```java
{
	"familyInfos":
	[
		{
			"familyName":"我家",
			"familyId":"10086",
			"familyOwner":"111111022233",
			"appId":"MB-PORTAL-0000"
		}
	],
   "retCode": "00000",
   "retInfo": "成功"
}
```

#### 查询家庭成员列表，包含家庭管理员

##### 1、接口定义
?> **接入地 址：**  `/ufme/familyService/familyMembers`  
 **HTTP Method：** POST

**输入参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyId|String|body|必填|家庭ID


**输出参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyMembers|FamilyMemberInfo[]|Body|必填|家庭信息


**输出参数对象说明**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
memberUserId|String|家庭成员用户ID|必填|
memberName|String|家庭成员在家庭中的名称|非必填|
familyId|String|家庭ID|必填|
joinTime|String|加入家庭时间|必填|

##### 2、请求样例

**请求明细**

```java
{
	"familyId":"******"
}
```

**请求应答**

```java
{
	"familyMembers":
	[
		{
			"memberUserId":"111111111112",
			"familyId":"10086",
			"memberName":"111111022233",
			"joinTime":"2018-12-01 00:00:00"
		}
	],
   "retCode": "00000",
   "retInfo": "成功"
}
```


### 设备分享管理

#### 查询家庭下的共享设备

##### 1、接口定义
?> **接入地 址：**  `/ufme/shareResourceService/familyShareDevices`  
 **HTTP Method：** POST



**输入参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
familyId|String|body|必填|家庭ID



**输出参数**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
shareDevs|ShareDevice[]|Body|必填|共享设备信息


**输出参数对象说明**


参数名|类型|说明|备注
:-:|:-:|:-:|:-
deviceId|String|设备ID|必填
devName|String|设备分享名称|必填
devOwner|String|设备Owner|必填
devFamilyId|string|设备所属家庭id|必填
permission|Permission|权限|必填



##### 2、请求样例

**请求明细**

```java
{
	"familyId": "111111111"
}
```

**输出明细**

```java
{
	"shareDevs":
	[
		{
			"deviceId": "100823",
			"devName": "测试",
			"devOwner": "11111111333",
			"devFamilyId": "111111111",
			"permission":
				{
					"authType":"share",
					"auth":
					{
						"view":true,
						"set":false,
						"control":false
					}
				}
		}
	],
	"retCode":"00000",
	"retInfo":"成功"
}

```
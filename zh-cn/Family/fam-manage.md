# 家庭管理
!> **更新时间**：{docsify-updated}  



家庭管理实现的是一种以“家庭”为基础的相关服务，使用此服务可以建立“家庭”组，在组下管理家庭及相关消息。



## 家庭管理员创建家庭

**使用说明**

> 用户创建家庭,在家庭模型管理平台中创建家庭,请求者作为家庭管理员,返回家庭的基础信息和错误码

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/family `  

 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|  


**输入参数对象说明**   
  
| **名称** | 家庭信息对象 |&emsp;| FamilyInfo |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|familyId| String | 家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型 |必填|  
|familyName| String | 家庭名称 |必填|  
|familyOwner| UserBriefInfo | 家庭管理员用户简明信息 |必填|  
|ownerName| String | 家庭管理员家庭中用户昵称 |可不填，自动生成名字admin+六位随机数字| 
|familyLable| String | 家庭标签 |非必填 APP定义，如父母等| 
|familyDesc| String | 家庭描述 |非必填| 
|appId| String | 应用Id |必填| 
|createtime| date | 家庭创建时间 |必填| 
|familyLogo| String | 家庭logo |必填，默认值为平台内置家庭logo url| 
|familyPicture| String | 家庭图片 |非必填，默认值为平台内置家庭图片 url| 
|familyLocation| Location | 家庭位置信息 |必填，家庭位置信息| 
|familyPosition| String | 家庭位置 |必填，小区等信息| 
|familyExternData| String | 扩展信息 |非必填，IOT平台可定义，json|    
|familyLastUpdater| String | 家庭最后修改人 |必填|  
|LastUpdateTime| date | 家庭最后修改时间 |精确到秒，含年月日信息，必填|  
|securityLevel| int | 安全级别 |必填|  
|memberCount| int | 家庭成员数 |为虚拟成员，真实成员，家庭管理员的总数|  
|deviceCount| int | 设备数量 |必填|  
  


**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

**示例**

**请求样例** 

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
{"familyInfo": {"familyName": "科技"}}


```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "科技",
        "familyOwner": {
            "userId": "100013957366154693",
            "mobile": "136****8934"
        },
        "appId": "MB-***-0009",
        "createTime": "2019-02-26 11:27:42",
        "familyLocation": {},
        "securityLevel": "0",
        "deviceCount": "0",
        "memberCount": "0"
    }
}



```

**接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31405  


## 家庭管理员修改家庭名称

**使用说明**

> 用户修改拥有的家庭的名称，发送修改家庭信息消息给家庭成员,记录发送结果到日志    

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/family `  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:--------:|:--------:|
|  FamilyInfo    | familyInfo | Body| 必填|家庭信息|  

 
**输入参数对象说明**   
  
| **名称** | 家庭信息对象 |&emsp;| FamilyInfo |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|familyId| String | 家庭id，以字符串形式传递的Long型变量，会自动转换字符串为合适的整型 |必填|  
|familyName| String | 家庭名称 |必填|  
|familyOwner| UserBriefInfo | 家庭管理员用户简明信息 |必填|  
|ownerName| String | 家庭管理员家庭中用户昵称 |可不填，自动生成名字admin+六位随机数字| 
|familyLable| String | 家庭标签 |非必填 APP定义，如父母等| 
|familyDesc| String | 家庭描述 |非必填| 
|appId| String | 应用Id |可不填| 
|createtime| date | 家庭创建时间 |可不填| 
|familyLogo| String | 家庭logo |可不填，默认值为平台内置家庭logo url| 
|familyPicture| String | 家庭图片 |可不填，默认值为平台内置家庭图片 url| 
|familyLocation| Location | 家庭位置信息 |可不填，家庭位置信息| 
|familyPosition| String | 家庭位置 |可不填，小区等信息| 
|familyExternData| String | 扩展信息 |可不填，IOT平台可定义，json|    
|familyLastUpdater| String | 家庭最后修改人 |必填|  
|LastUpdateTime| date | 家庭最后修改时间 |精确到秒，含年月日信息，不填|  
|securityLevel| int | 安全级别 |必填|  
|deviceCount| int | 设备数量 |必填|  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

**示例**  

**请求样例**
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
    "familyInfo": {
        "familyName": "ke",
        "familyLocation": {
            "latitude": "23.5",
            "cityCode": "29544",
            "longitude": "45.62"
        },
        "familyPosition": "ke",
        "securityLevel": "2"
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

**接口错误码**
 
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108   

## 家庭管理员或家庭成员查询家庭信息

**使用说明**

> 家庭管理员或家庭成员查询家庭信息    

**接口描述**

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

**示例**

**请求样例**
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
    "retInfo": "成功",
    "familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "ke",
        "familyOwner": {
            "userId": "100013957366154693",
            "mobile": "136****8934"
        },
        "appId": "MB-UBOT-0009",
        "createTime": "2019-02-26 11:27:42",
        "familyLocation": {
            "latitude": "23.5",
            "cityCode": "29544",
            "longitude": "45.62"
        },
        "familyPosition": "ke",
        "securityLevel": "2",
        "deviceCount": "0",
        "memberCount": "1"
    }
}

```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109   

## 家庭管理员查询创建家庭信息列表

**使用说明**

> 验证用户token,确认用户是否为家庭主人,查询家庭信息,并返回    

**接口描述**

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

**示例** 

**请求样例**
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

**接口错误码** 

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008   

## 家庭成员查询加入的家庭信息列表

**使用说明**

> 验证用户token,确认用户是否为家庭成员,查询加入的家庭信息,并返回    

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/families?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型      | 参数名   | 位置  | 必填|说明|
| -------- |:--------:|:-----:|:---------:|:---------:|  
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理| 

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo[] |  families  |   Body  |  必填  | 家庭信息列表 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数据|
|String|    pageNumber| Body|   必填  |当前页，从1开始|

**示例**

**请求样例**

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
    "retInfo": "成功",
    "families": [
        {
            "familyId": "755111162274000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-15 17:17:55",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "1",
            "memberCount": "2"
        },
        {
            "familyId": "880111442716000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-18 11:51:57",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "0",
            "memberCount": "2"
        },
        {
            "familyId": "115111466664000000",
            "familyName": "科技",
            "familyOwner": {
                "userId": "100013957366152524",
                "mobile": "136****8934"
            },
            "appId": "MB-UBOT-0009",
            "createTime": "2019-02-18 18:31:05",
            "familyLocation": {
                "latitude": "39.92767",
                "cityCode": "101010300",
                "longitude": "116.45011"
            },
            "familyPosition": "北京市 朝阳区",
            "securityLevel": "0",
            "deviceCount": "0",
            "memberCount": "2"
        }
]，
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1"
}


```

**接口错误码** 

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

## 家庭管理员删除家庭

**使用说明**

> 家庭管理员删除家庭,收回用户分享给家庭的设备权限,收回家庭成员的家庭分享设备权限，向删除前家庭成员发送删除家庭消息，记录消息推送结果到日志   

**接口描述**

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

**示例**

**请求样例**

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

**接口错误码** 

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108    



## 家庭管理员邀请用户加入家庭

**使用说明**

> 家庭管理员邀请用户加入家庭，并向用户下发推送邀请码, 支持targetId为临时的userid,邀请生成正确,推送失败时,返回给发起方家庭ID和邀请码.推送消息  

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/{familyId}/{targetId}/familyMember`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | targetId   | url |必填|被邀请用户id |  
| String  | familyId   | url |必填|家庭id |  
| String  | memberName  | Body |必填|家庭成员在家庭中名称 |  
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| String |  familyId  |   body |  必填  | 家庭id|
| String |  inviteCode  |   body |  必填  | 用户邀请码|  
| String |  invitationUID  |   body |  必填  | 该条邀请的uid|  

**示例**

**请求样例**
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
 "memberName":"邀请成员拨测名称"
}

```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功"，
    "familyId": "647112241261000000",
    "inviteCode": "541180"
}

```

**接口错误码** 

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、H00001、E31405、E31406    
    
## 家庭成员回复邀请加入家庭

**使用说明**

> 家庭成员接受家庭管理员邀请，加入家庭，,分享家庭设备权限给成员，并发送添加家庭成员消息给家庭成员；如果拒绝，本次邀请结束，发送用户拒绝加入家庭消息到家庭管理员，记录消息推送结果到日志  


**接口描述**

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


**示例**

**请求样例**

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

**接口错误码** 

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、E31131、E31132、E31133、E31141,E31142,E31405、E31406、E31411、E31143、E31144      



## 用户查询被邀请加入家庭记录  

**使用说明**

> 用户根据查询邀请唯一邀请Id查询邀请加入家庭的记录，在调用家庭管理员邀请用户加入家庭接口时会推送消息，含有invitationUID字段，使用此参数查询邀请历史 


**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/invitationHistory`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | invitationUID   | body |必填|邀请信息唯一ID|  

  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| String  | familyId   | body |必填|家庭id |  
| String  | inviteCode  | body |必填|邀请码 | 
| String  | memberName  | body |必填|家庭成员由家庭管理员邀请时定义给用户的家庭昵称，在用户回复邀请时，调用家庭成员回复家庭管理员邀请接口 ，该参数可被接受邀请用户重新命名，并且以家庭成员回复家庭管理员邀请接口命名为准，如果不填，则 以本参数为准。 |  
| String  | invitationUID  | body |必填|邀请信息唯一ID | 
| String  | invatationStatus  | body |必填|0，未处理状态，1，用户已经同意；2用户拒绝；3，已过期；4，用户已经取消邀请 | 

**示例**

**请求样例**

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
"invitationUID": " 9de5705ae9fb4567a3735aedaef853e8 "
}

```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "familyId": "164131078929000000",
    "inviteCode": "994925",
    "memberName": "邀请成员拨测名称",
    "invitationUID": "a7fb8855152047368e221c2883ddbe15",
    "invatationStatus": 2
}

```

**接口错误码**
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31130、E31131、E31132、E31133、E31405、E31406、E31411、 E31141、 E31142、E31143、E3144   



## 家庭成员主动退出家庭

**使用说明**

> 家庭成员主动退出家庭,撤销家庭成员的家庭设备分享,撤销其他家庭成员对本家庭成员设备的分享权限，向家庭其他成员发送删除家庭成员消息  
 
**接口描述**

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


**示例**

**请求样例**

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

**接口错误码** 
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108   



## 获取家庭二维码参数

**使用说明**

> 家庭管理员或家庭成员获取家庭二维码参数，获取加入家庭的二维码参数

**接口描述**

?> **接入地址：** `/ufm/v1/protected/familyService/family/QRCode`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
FamilyQRCodeInfo|QRCode|body|是|  




**输入参数对象说明**   
  
| **名称** | 家庭模型快速响应码 |&emsp;| FamilyQRCodeInfo |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|shareUrl|String|分享url|非必填，不传或者传空传空字符，返回默认url，默认值为`http://uplusapp.cn/U/0005H`|  
|event|String|快速响应码要处理的事件|非必填，目前只支持`1uplus://joinFamily`其他事件返回参数错误，可不填，当event为空或者空字符时使用默认值`uplus://joinFamily`|  
|familyId|String|家庭ID|必填|  
|timeout|int|按秒计算，为二维码有效期|必填|   
|params|String|附加参数，数字字母最长64位|非必填|  


**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
String|familyId|body|必填|家庭id
String|qrcode|body|必填|二位码url


**示例**

**请求样例**

```
{ 
    "familyId":"647112241261000000",
    "timeout":30
}

```

**请求应答**
```
  {
    "retCode": "00000",
    "retInfo": "成功",
    "familyId": "647112241261000000",
    "qrcode": "http://uplusapp.cn/U/0005H?token=748dae0417fe4361b7ff882785b4f021&content=uplus://joinFamily/748dae0417fe4361b7ff882785b4f021",
    "expiresTime": "2019-06-01 11:47:11"
}

```

**接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31137



## 使用二维码加入家庭


**使用说明**

> 用户使用App扫描二维码后由app处理参数后，本接口执行加入家庭，如果用户已经是家庭成员或者家庭管理员，返回成功。

**接口描述**

?> **接入地址：** `/ufm/v1/protected/familyService/family/QRCode/param`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String|familyQRCode|Body|必填|用户通过接口“获取家庭二维码参数”获取的qrcode中，token的值
userFamilyName|用户在家庭中的名称|Body|必填|用户加入家庭附属条件

**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
String|familyId|body|必填|家庭id,加入家庭成功或者用户已经加入家庭时会有该字段对应的retCode为00000和E31105


**示例**

**请求样例**
```
{
    "familyQRCode":"748dae0417fe4361b7ff882785b4f021",
    "userFamilyName":"cindy"
}

```

**请求应答**

```
{
   "familyId": "647112241261000000",
    "retCode": "00000",
    "retInfo": "成功"
}
```

**接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、E31405、E31406      






## 用户设置默认家庭

**使用说明**

> 设置用户分享设备的默认家庭，更新时依然使用此接口，只能设备用户管理的家庭或用户加入的家庭。    

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/setDefaultFamily`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 必填|默认的家庭|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

**示例**

**请求样例**
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
body：
{
" familyId ":"647112241261000000"
}


```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "ke",
        "familyOwner": {
            "userId": "100013957366154693",
            "mobile": "136****8934"
        },
        "appId": "MB-UBOT-0009",
        "createTime": "2019-02-26 11:27:42",
        "familyLocation": {
            "latitude": "23.5",
            "cityCode": "29544",
            "longitude": "45.62"
        },
        "familyPosition": "ke",
        "securityLevel": "2",
        "deviceCount": "0",
        "memberCount": "1"
    }

}

```

**接口错误码** 
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408  





## 用户获取默认家庭

**使用说明**

> 用户获取默认家庭信息,用户未设置返回用户创建的第一个家庭；用户设置后返回用户设置家庭；用户未设置且用户没有管理家庭，创建后设置为默认家庭，并返回；家庭被删除后返回，默认家庭被清理，取消该默认家庭。   

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/defaultFamily`  
 **HTTP Method：** GET

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  &emsp;    | &emsp; | &emsp;| &emsp;|&emsp;|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo |  familyInfo  |   Body  |  必填  | 家庭信息 |

**示例**

**请求样例**
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
    "retInfo": "成功",
    "familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "ke",
        "familyOwner": {
            "userId": "100013957366154693",
            "mobile": "136****8934"
        },
        "appId": "MB-UBOT-0009",
        "createTime": "2019-02-26 11:27:42",
        "familyLocation": {
            "latitude": "23.5",
            "cityCode": "29544",
            "longitude": "45.62"
        },
        "familyPosition": "ke",
        "securityLevel": "2",
        "deviceCount": "0",
        "memberCount": "1"
    }
}

```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408    





## 用户撤销家庭邀请

**使用说明**

> 用户撤销家庭邀请，撤销后本家庭中所有邀请被邀请用户的邀请记录都将被撤销。   

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/cancelInvitation`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 必填|家庭ID|  
|  String    | userId | Body| 必填|被邀请用户ID|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |

**示例**  

**请求样例**
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
body：
{
" familyId ":"647112241261000000",
" userId ":"100013957366154693"

}


```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功
}


```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408    




## 用户查询家庭邀请记录  

**使用说明**

> 家庭成员查询家庭的邀请记录， 包含家庭ID，被邀请用户信息，邀请时间，家庭名称，当前邀请状态，每个用户只取最后发生的一条，每页最大20个，请求参数超过20个按20个处理。   

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/invitationRecords`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 必填|家庭ID|  
|  String    | pageNumber | Body| 必填|当前请求页数，起始页数为1|  
|  String    | pageSize | Body| 必填|每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|   



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInvitationInfo[] |  invitations |  Body  | 必填 | 家庭邀请信息 |  
| String |  familyId |  Body  | 必填 | 家庭ID |
| String |  familyName |  Body  | 必填 | 家庭名称 |
| String|  totalCount |  Body  | 必填 | 总数 |
| String|  pageSize |  Body  | 必填 | 当前返回页实际数量，不超过规定的最大数上限 |


**示例**  

**请求样例**
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
body：
{
" familyId ":"647112241261000000",
" pageNumber ":"1",
" pageSize ":"20",
}



```  

**请求应答**

```java
{
"retCode": "00000",
"retInfo": "成功",
" familyId ": "647112241261000000",
" familyName ": "测试家庭",
"invitations":[{
" userInfo ":{
" name ":"测试用户",
" email ":"aa@sina.com",
" userId ":"647112241261000000",
" mobile ":"136****8934",
" avatarUrl ":"",
" isVirtualUser ":"false",
" ucUserId ":"1122567"},
" invitationUID ":"9de5705ae9fb4567a3735aedaef853e8",
" invitationStatus ":"0",
" invitationtTime": "2019-10-01 00:00:00"},
{
" userInfo ":{
" name ":"测试用户",
" email ":"aa@sina.com",
" userId ":"647112241261000000",
" mobile ":"136****8934",
" avatarUrl ":"",
" isVirtualUser ":"false",
" ucUserId ":"1122567"},
" invitationUID ":"9de5705ae9fb4567a3735aedaef853e8",
" invitationStatus ":"0",
"invitationtTime": "2019-10-01 00:00:00"}
],
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1"
}

```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408   




## 用户查询二维码家庭信息  

**使用说明**

> 用户查询二维码对应的家庭信息。   

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/family/QRCode/familyInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | qrCode | Body| 必填|二维码ID|  



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyInfo|  familyInfo |  Body  | 必填 | 家庭信息 |  
| UserBriefInfo |  inviter |  Body  | 必填 | 邀请者信息 |



**示例**

**请求示例**

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
body：
{
" qrCode ":"9de5705ae9fb4567a3735aedaef853e8"
}


```  

**请求应答**

```java
{
    "retCode": "00000",
"retInfo": "成功",
"familyInfo": {
        "familyId": "647112241261000000",
        "familyName": "ke",
        "appId": "MB-UBOT-0009"
}，
"inviter":{
"isVirtualUser": "false",
        "email": "",
        "name": "187****6123",
        "userId": "100013957366158663",
        "ucUserId": "2005021119",       "avatar":"https://account.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
        "mobile": "18730000000"
}

}


```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408    




## 用户删除家庭邀请信息  

**使用说明**

> 用户删除对指定家庭，指定用户的家庭邀请记录。   

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/invitation/removeInvitation`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 必填|家庭ID|  
|  String    | userId | Body| 必填|被邀请用户ID|  



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |



**示例** 

**请求样例**

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
body：
{
" familyId ":"647112241261000000",
" userId ":"100013957366154693"

}


```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功
}


```

**接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408     







## 用户申请加入家庭  

**使用说明**

> 用户申请加入家庭，优先familyId，不填则使用设备ID，推送UMS消息给家庭管理员   

**接口描述**

?> **接入地 址：**  `/fcs/apply/joinFamily`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 非必填|用户要加入家庭的家庭ID和deviceId二选一|  
|  String    | deviceId | Body| 非必填|用户要加入家庭的家庭ID和deviceId二选一|  
|  String    | memberName | Body| 非必填|用户在家庭中的昵称,默认用户手机号（脱敏）|  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| String | applicationId  |   body  |  必填 | 用户申请的ID，识别邀请休息，可通过推送消息或其他诸如短信之类的方式通知家庭管理员，家庭管理员以此为依据同意用户加入家庭。 |



**示例**  

**请求样例**
```
https://{baseuri}/fcs/apply/joinFamily

{
“ familyId “:”647112241261000000”
}


```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "payload": {
        "applicationId": "647112241261000000"
    }
}


```

**接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408




## 家庭管理员同意/拒绝用户加入家庭  

**使用说明**

> 家庭管理员同意/拒绝用户加入家庭，并向申请用户以及其他家庭成员推送加入家庭消息或向申请用户拒绝用户加入家庭消息   

**接口描述**

?> **接入地 址：**  `/fcs/apply/agreeJoinFamily`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | agree | Body| 必填|true或false，true为同意，false为拒绝|  
|  String    | applicationId | Body| 必填|申请ID，从推送消息中获得，或其他渠道获得|  




**示例**  

**请求样例**
```
https://{baseuri}/fcs/apply/agreeJoinFamily

{
" applicationId ":"9de5705ae9fb4567a3735aedaef853e8",
" agree": " true "

}


```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功"
}



```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408



## 用户查询申请记录、邀请记录  

**使用说明**

> 用户通过deviceId查询到自己申请该设备所在家庭的申请记录以及该家庭管理员邀请当前用户加入家庭的记录  

**接口描述**

?> **接入地 址：**  `/fcs/find/records`  
 **HTTP Method：** POST

**输入参数**  

| 类型  | 参数名      | 位置  | 必填|说明|
| ------- |:--------:|:-----:|:-------:|:-------:|
|  String    | familyId | Body| 非必填|用户要加入家庭的家庭ID和deviceId二选一|  
|  String    | deviceId | Body| 非必填|用户要加入家庭的家庭ID和deviceId二选一|    


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| List<ApplicationInfoDto> | applicationInfoDtos  |   body  |  必填 | 用户申请记录 |



**示例**  

**请求样例**
```
https://{baseuri}/fcs/find/records

{
"deviceId":"04FA83D829C3"
}



```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "payload": {
        "applicationInfos": [{
            "applicationId": "123912983129873",
            "userId": "123",
            "familyId": "12345678",
            "familyName": "family",
            "applicationTime": "2020-12-09 00:00:00",
            "applicationStatus": 1,
            "applicationMessage": "家庭管理员已经同意加入家庭"
        }],
        "invitationInfos": [{
            "invitationId": "123912983129873",
            "userId": "123",
            "familyId": "12345678",
            "familyName": "family",
            "invitationTime": "2020-12-09 00:00:00",
            "invitationStatus": 1,
            "invitationMessage": "当前用户已经同意加入该家庭",
            "invitationCode": "123456",
            "inviterName":"用户1234",
            "inviterId":"1234567890"
        }]
    }
}



```

**接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408









[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
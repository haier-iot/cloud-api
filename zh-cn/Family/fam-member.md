# 家庭成员

!> **更新时间**：{docsify-updated}  




家庭成员管理服务实现的是对“家庭”下的成员进行查询、管理等操作。



## 家庭管理员添加家庭成员

**使用说明**

> 家庭管理员添加家庭成员,分享家庭设备权限给成员，发送家庭成员添加家庭成员消息,支持memberId为临时的userid  

**接口描述**

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
    "familyId": "647112241261000000",
    "memberId": "100013957366152179",
    "memberName": "添加成员拨测名称"
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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31105、E31108、E31405、E31406、E31409  



## 家家庭管理员或家庭成员修改家庭成员家庭名称  

**使用说明**

> 家庭管理员修改家庭成员信息，其中包含家庭管理员在家庭中的昵称；家庭成员可以修改自己的信息，发送修改家庭成员信息消息给家庭成员，记录消息推送结果到日志
 
 
 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/{memberId}/familyMember`  
 **HTTP Method：** PUT

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| FamilyMemberInfo  | memberInfo   | Body |必填|家庭成员信息|  
| String  | familyId   | url |必填|家庭id |  
| String  | memberId  | url |必填|家庭成员id |   


**输入参数对象说明**   
  
| **名称** | 家庭成员信息 |&emsp;| FamilyMemberInfo |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|memberName| String | 用户家庭名称 |必填，成员修改后的新名字|  

  

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

**接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109 



## 家庭管理员删除家庭成员

**使用说明**

> 家庭主人删除家庭成员,并解除成员在家庭中分享的设备关系，并收回成员分享给家庭的设备，发送删除家庭成员消息给家庭全体成员，记录消息推送结果到日志 
 
 **接口描述**

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
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31107、E31108  




## 家庭管理员或家庭成员查询家庭成员

**使用说明**

>家庭管理员或家庭成员查询家庭成员  

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/familyMembers?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|   

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数上限|
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
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
"retCode":"00000",
"retInfo":"成功"

}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109    



## 家庭成员或家庭管理员查询家庭成员所有成员（包含管理员）

**使用说明**

>家庭管理员或家庭成员查询家庭成员 
 
 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/{familyId}/allFamilyMembers?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | url |必填|家庭id | 
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|   

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| FamilyMemberInfo[]  |  familyMembers   |   Body  |  必填   | 家庭成员信息列表 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数上限|
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
    "familyMembers": [
        {
            "memberInfo": {
                "userId": "100013957366154693",
                "mobile": "136****8934"
            },
            "memberName": "admin100013957366154693",
            "familyId": "647112248577000000",
            "shareDevicesCnt": "0",
            "joinTime": "2019-02-26 13:29:37"
        },
        {
            "memberInfo": {
                "userId": "100013957366152179",
                "mobile": "187****6123"
            },
            "memberName": "添加成员拨测名称",
            "familyId": "647112248577000000",
            "shareDevicesCnt": "0",
            "joinTime": "2019-02-26 13:29:40"
        }
    ]
}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109 



## 根据关键字精确检索好友信息

**使用说明**

>精确查找用户信息，用于执行需要用户ID的场景，本次用户id有时效性，临时分配，有效期为1天，支持其他接口使用，在相关接口中有说明,同时屏蔽用户敏感信息,包含手机号,邮箱,登录名。
 
 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/userInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| QueryInfo  | queryInfo  | body |必填|查询条件 | 
  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| QueryUserInfoResult |  qUserR   |   Body  |  必填   | 用户信息 |


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
    "queryInfo":{
    "queryKey":"mobile",
    "queryValue":"13000000135"，
    "accType":"0"，} 
}

```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "qUserInfo": {
        "userId": "1647112237498000000",
        "mobile": "130****135",
        "email": "",
        "loginName": "H340**********0526"
    }
}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31301




## 家庭管理员变更

**使用说明**

>家庭管理员可以主动移交管理员角色，变更时，只能变更给当前家庭下其他家庭成员；变更完成时，家庭管理员变为家庭普通成员
 
 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/changeAdmin`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| familyId  | String  | body |必填|家庭id  | 
| newFamilyOwner  | String  | body |必填|新家庭管理员的userId  |   

**输出参数**  

标准输出


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
{"familyId":"647111777513000000","newFamilyOwner":"100013957366152179"}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、F31214  




## 虚拟用户加入家庭

**使用说明**

> 实体账户作为家庭成员添加虚拟用户进入家庭，实体账户必须为虚拟账户的宿主账户

 **接口描述**

?> **接入地址：** `/ufm/v1/protected/familyService/joinFamily/virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String| virtualUCId|    Body|   必填| 虚拟用户用户中心ID
String|     familyId|   Body|   必填| 要加入的家庭ID
String| userFamilyName| Body|   必填  |用户加入家庭附属参数,为用户在家庭中昵称


**输出参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String| virtualUserId|  Body|   必填| 用户在iot平台分配的userId


 **示例**
**请求样例**
```
{
    "familyId":"444130753006000000",
    "userFamilyName":"test",
    "virtualUCId":"237"
}



```

**输出参数**

```
{
"retCode":"00000",
"retInfo":"成功",
"familyId":"164131078929000000",
"virtualUserId":"100013957366167109"
}


```

 **接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408、E31409  




## 虚拟用户退出家庭

**使用说明**

> 实体账户必须为虚拟账户的宿主账户，实体账户协助虚拟账户退出家庭

 **接口描述**

?> **接入地址：** `/ufm/v1/protected/familyService/leaveFamily/virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String| virtualUserId|  Body|   必填| 虚拟用户在IOT平台的用户ID
String|     familyId|   Body|   必填  |要加入的家庭ID



**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-



 **示例**
**请求样例**
```
{ 
"familyId":"164131078929000000",
"virtualUserId":"100013957366167109"
}   




```

**输出参数**

```
{
    "retCode": "00000",
    "retInfo": "成功"
}


```

 **接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408  





## 查询虚拟成员所在家庭

**使用说明**

> 实体账户查询虚拟用户所在的家庭

 **接口描述**

?> **接入地址：** `/ufm/v1/protected/familyService/queryFamily /virtualFamilyMember`

**HTTP Method：** `POST`

**输入参数**

类型|参数名|位置|必填|说明
:-:|:-:|:-:|:-:|:-
String| virtualUserId|  Body|   必填  |虚拟用户在IOT平台的用户ID，本参数兼容用户中心虚拟用户userId，以便在用户未加入过家庭时返回正确的提示。



**输出参数**

类型|参数名|位置|必要性|说明
:-:|:-:|:-:|:-:|:-
FamilyInfo[]|   families|   Body|   必填  |家庭信息


 **示例**
**请求样例**
```
{
"virtualUserId":"1111111111111"
}


```

**输出参数**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "families": [{
        "familyId": "693130509064000000",
        "familyName": "Aalitest",
        "familyOwner": {
            "isVirtualUser": "false",
            "email": "",
            "name": "187****6123",
            "userId": "100013957366158663",
            "ucUserId": "2005021119",
            "avatar": "https://account.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
            "mobile": "18730000000"
        },
        "appId": "MB-UBOT-0009",
        "createTime": "2019-08-28 02:31:04",
        "familyLocation": {},
        "securityLevel": "0",
        "deviceCount": "0",
        "memberCount": "3"
    }, {
        "familyId": "693130508700000000",
        "familyName": "我的家庭",
        "familyOwner": {
            "isVirtualUser": "false",
            "email": "",
            "name": "187****6123",
            "userId": "100013957366158663",
            "ucUserId": "2005021119",
            "avatar": "https://account.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
            "mobile": "1873000000"
        },
        "appId": "MB-UBOT-0009",
        "createTime": "2019-08-28 02:25:01",
        "familyLocation": {},
        "securityLevel": "0",
        "deviceCount": "0",
        "memberCount": "4"
    }
],
    "totalCount": "2"
}


```

 **接口错误码**

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31105、E31108、E31137、E31138、 E31140、E31141、E31142、E31408




[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
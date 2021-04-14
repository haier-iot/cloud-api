# 房间管理

!> **更新时间**：{docsify-updated}  


家庭房间管理服务实现的是对“家庭”下的房间进行查询、管理等操作。


## 用户创建房间

**使用说明**

>用户创建房间到家庭下，房间必须属于家庭  

 **接口描述** 

?> **接入地 址：**  `/ufm/v1/protected/roomService/addRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| --------- |:----------:|:-----:|:-----------:|:-----------:|  
|  RoomInfo    | roomInfo | Body| 必填|房间信息|   

**输入参数对象说明**   

|**名称** |描述房间信息 |&emsp;|RoomInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomName|String|房间名称|必填，ufmVersion如果小于v2，则本字段为楼层floorName·roomName|  
|familyId|String|房间所属家庭ID|必填|  
|floorId|String|房间所属楼层ID|非必填，默认一层|  
|floorOrderId|String|房间所属楼层|非必填，和floor二选一，floorId不填默认一层|  
|roomClass|String|房间类型|非必填|  
|roomLabel|String|房间标签|非必填| 
|roomLogo|String|房间logo url|非必填| 
|roomPicture|String|房间图片 url|非必填| 
|roomExternData|String|扩展信息，使用json格式|非必填|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| RoomInfo |  roomInfo  |   Body  |  必填  | &emsp; |

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

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31401  

## 用户修改房间信息  

**使用说明**

>用户修改家庭下房间信息，有值的属性信息会被更新，为空或未配置的不填  

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/roomService/updateRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**  
 
|**名称** |描述房间信息 |&emsp;|RoomInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomName|String|房间名称|非必填|  
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填|  
|roomLabel|String|房间标签|非必填| 
|roomLogo|String|房间logo url|非必填| 
|roomPicture|String|房间图片 url|非必填| 
|roomExternData|String|扩展信息，使用json格式|非必填|  


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

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E31401 E31403 E31136

## 用户删除房间信息

**使用说明**

> 用户删除家庭下的房间，用户为家庭成员或家庭管理员，房间必须没有设备才能被删除  

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/roomService/delRoomInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**  
   
|**名称** |描述房间信息 |&emsp;|RoomInfo|  
| ------ |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填|  

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
{
"roomInfo":{ "familyId":"11111","roomId":"1111111"}
}


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```
 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E31402，E31403   

## 用户查询家庭房间下的设备

**使用说明**

> 家庭管理员或家庭成员查询用户房间下的设备信息列表，其他查询用户返回用户无权限  

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/roomService/queryRoomDevices`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------- |:-------------:|:-----:|:-----------:|:----------:|
|  RoomInfo    | roomInfo | Body| 必填|房间信息|  


**输入参数对象说明**    
 
|**名称** |描述房间信息 |&emsp;|RoomInfo|
| ---------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomId|String|房间ID|必填|  
|familyId|String|房间所属家庭ID|必填| 

**输出参数**   
 
| 类型         | 参数名         | 位置  | 必填|说明|
| ---------- |:----------:|:-----:|:-------------:|:-------------:|
|  shareDevice[]    | shareDevices | Body| 必填|&emsp;| 

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
{"retCode":"00000","retInfo":"成功", "shareDevices":[{},{},{}]}

```
 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31401、E31109、E31108  






## 查询家庭下的房间列表

**使用说明**

>查询家庭下的房间列表信息
 
**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/roomInfoList`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| QueryClause  | queryInfo  | body |必填|查询条件  | 

**输入参数对象说明**  

|**名称** |查询条件 |&emsp;|QueryClause|  
| ------ |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|familyId|String|家庭id |&emsp;|  
 

**输出参数**  


| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| RoomInfo[]  | roomInfos  | body |必填|用户信息  | 



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
"familyId":"11111111"
}
}

```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    "roomInfos": [{
        "roomName": "testroorm1",
        "roomId": "150111135560000000",
        "familyId": "878111118405000000",
        "roomCreateTime": "2019-02-15 09:52:40"
    }]
}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31301  




## 查询用户设备房间位置信息  

**使用说明**

> 用户查询指定家庭下设备的房间位置信息。

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/deviceAndRoom`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | body |必填|家庭ID | 


**输出参数**  

| 类型   | 参数名  | 位置  | 必填|说明|
| ------ |:--------:|:-----:|:--------:|:-------:|
| DeviceAndRoomInfo[] | deviceAndRooms | body|必填|房间位置信息 | 
| String | familyId | body|必填|家庭id |  
| String  | familyName | body|必填|家庭名称 |  



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

Body：
{
    "familyId": 878111118405000000
}



```  

**请求应答**

```java
{
   "familyId": 878111118405000000，
   "familyName":"测试家庭"，
    "deviceAndRooms": 
    [
       {
           "deviceId": "0007A8C17DBD",
           "deviceName": "空气净化器1数",
           "deviceType": "21001001",
           "online": false,
           "permissions": 
           [
               {
                   "auth": 
                   {
                       "control": true,
                       "set": true,
                       "view": true
                   },
                   "authType": "owner"
               }
           ],
           "room": "卧室",
           "wifiType":"111c12002400081021010000005a4e4b32303134303931313031000000000000"
        }
    ],  
    "retCode": "00000",
    "retInfo": "成功!"
}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、G20202   









[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
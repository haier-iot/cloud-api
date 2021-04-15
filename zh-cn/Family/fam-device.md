# 家庭设备管理

!> **更新时间**：{docsify-updated}  


家庭设备管理实现的是对“家庭”下的智能互联设备进行查询、管理等操作。




## 家庭管理员或家庭成员查询家庭的所有设备

**使用说明**

> 家庭管理员或家庭成员查询家庭成员分享给家庭的所有设备

 **接口描述**

?> **接入地址：**  `/ufm/v1/protected/shareDeviceService/family/{familyid}/shareDevices?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  String    | familyId | url| 必填|家庭Id|
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理| 

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数|
|String|    pageNumber| Body|   必填  |当前页，从1开始|


**输出参数对象说明**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|devInfo |  DeviceBriefInfo  |  设备简明信息  |  必填  | 
|devName|   String| 设备名称|   必填  |
|devFamilyId|   String|设备所属家庭Id|    必填  |
|devRoomName|   String| 设备所在家庭房间名称| 必填，如果ufmVersion 小于v2或为空，则本值包含devFloorName+devRoomName   |
|devRoomId| String| 设备所在家庭房间ID|  必填 |
|devFloorId|    String| 设备所属家庭楼层|   非必填|
|devFloorName|  String| 设备所属家庭楼层名称| 非必填|
|devOwner|  UserBriefInfo|  设备管理员简明信息|  必填|
|permission|    Permission| 权限|必填   |



 **示例**  

**请求样例**
```  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/family/{familyid}/shareDevices

```  

**请求应答**

```
{
    "shareDevs": [{
        "devInfo": {
        "deviceName": "测试",
        "deviceId": "100823",
        "wifiType": "0000000000000000000000000",
        "deviceType": "00000",
        "online": false
        },
        "devName": "测试1",
        "devFamilyId": "1",
        "devOwner": {
            "userId": "10811563273",
            "userNickName": "xiaoyi",
            "userAge": "10811563273",
            "userAddr": "10811563273",
            "userSex": "male"
        },
        "permission": {
                "authType": "home",
                "auth": {
                    "view": true,
                    "set": false,
                    "control": false
                }
            }
    },
    {
        "devInfo": {
        "deviceName": "测试2",
        "deviceId": "1008232",
        "wifiType": "0000000000000000000000000",
        "deviceType": "00000",
        "online": false
        },
        "devName": "测试1",
        "devFamilyId": "1",
        "devOwner": {
                "userId": "10811563273",
                "userNickName": "xiaoyi",
                "userAge": "10811563273",
                "userAddr": "10811563273",
                "userSex": "male"
        },
        "permission": {
            "authType": "home",
            "auth": {
                "view": true,
                "set": false,
                "control": false
            }
        }
    }
    ],
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
    "retCode": "00000",
    "retInfo": "成功"
}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31109   


## 设备管理员查询设备管理员分享给个人的所有设备

**使用说明**

> 查询设备管理员分享给个人的所有设备  

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ person/shareDevices?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs |   Body|  必填  | 共享设备信息 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数|
|String|    pageNumber| Body|   必填  |当前页，从1开始|  


**输出参数对象说明**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|devInfo |  DeviceBriefInfo  |  设备简明信息  |  必填  | 
|devName|   String| 设备名称|   必填  |
|devShareUser|  UserBriefInfo|分享用户简明信息| 必填  |
|permission|    Permision|  权限| 必填  |


 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    
    

## 设备管理员查询分享给家庭的所有设备

**使用说明**

> 查询设备管理员分享给家庭的所有设备   

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/shareDevices?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:| 
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs |   Body|  必填  | 共享设备信息 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数|
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
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
"retCode":"00000",
"retInfo":"成功"
}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

## 家庭成员查询分享给我的所有家庭设备

**使用说明**

> 家庭成员查询分享给我的所有家庭设备

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/ family/shareDevices2me?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理| 

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数|
|String|    pageNumber| Body|   必填  |当前页，从1开始|

**输出参数对象说明**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|devInfo |  DeviceBriefInfo  |  设备简明信息  |  必填  | 
|devName|   String| 设备名称|   必填  |
|devFamilyId|   String|设备所属家庭Id|    必填  |
|devRoomName|   String| 设备所在家庭房间名称| 必填，如果ufmVersion 小于v2或为空，则本值包含devFloorName+devRoomName|
|devRoomId| String|设备所在家庭房间ID|  必填  |
|devFloorId|    String|设备所属家庭楼层|    非必填 |
|devFloorName|  String|设备所属家庭楼层名称|  非必填 |

 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/ family/shareDevices2me
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
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
    "retCode": "00000", 
    "retInfo": "成功", 
   
}



```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008     

   
## 普通用户查询分享给我的个人分享设备

**使用说明**

> 查询分享给我的所有个人分享设备

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/shareDevices2me?pageNumber={curpage}&pageSize={pageSize}`  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|String |pageNumber|    url|    否   |当前访问信息的起始页，从1开始|
|String |pageSize   |url|   否   |每页的对象数，如果不足，有多少显示多少，最大不超过系统规定的上限数，超过按上限处理| 

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| ShareDevice[] |  shareDevs  |   Body  |  必填  | 共享设备信息 |
|String|    totalCount| Body|   必填  |总数|
|String|    pageSize|   Body|   必填  |当前返回页实际数量，不超过规定的最大数|
|String|    pageNumber| Body|   必填  |当前页，从1开始|

**输出参数对象说明**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|devInfo |  DeviceBriefInfo  |  设备简明信息  |  必填  | 
|devName|   String| 设备名称|   必填  |
|devOwner|  UserBriefInfo|设备管理员简明信息|    必填  |
|permission|    Permission| 权限| 必填 |

 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/person/shareDevices2me
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
"totalCount": "20",
"pageSize": "3",
"pageNumber": "1",
    "retCode": "00000", 
    "retInfo": "成功", 
   
}



```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008     


## 设备管理员查询自有设备列表

**使用说明**

> 查询用户自有设备列表    

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/devices`  
 **HTTP Method：** GET



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| DeviceBriefInfo [] |  devices  |   Body  |  必填  |  &emsp;|

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

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008    

## 家庭成员或家庭管理员分享设备给家庭

**使用说明**

> 家庭成员或家庭管理员作为设备的管理员分享设备给家庭，发送分享家庭设备消息给家庭成员，包含管理员，记录消息发送结果到日志,分享过程中：
>1. 如果同一用户已经分享，自动取消原来分享，建立新的分享关系；
>2. 如果不是同一用户分享，判断此用户是不是绑定者，如果不是，作为垃圾数据处理，如果是，提示设备已经被分享；如果分享没有房间信息，系统会分享设备到默认房间
>3.用户如果不填写房间信息，则分享到设备默认房间，默认家庭根据设备wifiType确定，如果不存在则默认房间
>4.如果不填写家庭ID参数，则分享到用户设备的默认家庭，用户没设置，则指定用户创建的第一个家庭。
>5.支持主从设备分享，主设备分享成功即为成功，从设备分享结果会体现在返回结果中为map结构。
 

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/shareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice  | shareDev  | Body | 必填 |设备的分享信息 |  


**输入参数对象说明**   
  
| **名称** | 设备共享信息 |&emsp;| ShareDevice |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|devInfo| DeviceBriefInfo | 设备简明信息|必填|  
|devRoomId| String | 设备所在房间 |选填，不填进入默认房间，按规则生成|  
|devRoomName| String | 设备所在房间名称 |选填，优先级低于devRoomId，devRoomId如果填写，|  
|devName| String | 设备名称 |必填| 
|devFloorOrderId| String | 设备所在楼层 |如果填写devFloorId，本字段无效，如填写房间名，不填写devFloorId，则本字段不填写默认为一层，填写则按照填写设备设备位置| 
|devFloorId| Permision | 设备所在楼层ID|本字段填写，devFloorOrderId不起作用| 
|devFamilyId| String | 设备所属家庭Id |选填，不填使用用户设置默认房间| 
|permission| Permision | 分享权限此处权限authType 必须为home |必填| 



| **名称** | 设备简明信息 |&emsp;| DeviceBriefInfo |   
| ------------- |:----------:|:-----:|:--------:|  
|**字段名**|**类型**|**说明**|**备注**|  
|deviceId| String | 设备ID|必填 |  



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| Map<String,String> |  results  | body  |  &emsp;  | 如果被分享的设备存在子设备附件设备等，则本结构返回该主设备下的子设备的分享结果 |

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
"retCode":"00000",
"retInfo":"成功",
"results":{"pengfei-2":"00000","pengfei-5":"00000"}
}



```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31111、E31129        

## 家庭管理员取消家庭设备分享

**使用说明**

> 用户取消分享给家庭的设备,收回分享给家庭用户的设备家庭权限，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志。同时，将设备转移到默认家庭的默认楼层的默认房间中，默认家庭由默认家庭逻辑指定。默认房间由wifiType配置决定，如果wifitype没有配置，则转移到客厅

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/{familyId}/manager/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | familyId   | url |必填|家庭id |  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| Map<String,String> |  results  |  body |  &emsp;  | 如果被分享的设备存在子设备附件设备等，则本结构返回该主设备下的子设备的分享结果 |

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
 "retCode":"00000",
 "retInfo":"成功"，
 "results":{"pengfei-2":"00000","pengfei-5":"00000"}

}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31104、E31108 、F31229   

## 设备管理员取消家庭设备分享

**使用说明**

> 设备管理员取消分享给家庭的设备，发送取消家庭设备分享消息给家庭成员，记录消息发送结果到日志。默认房间由wifiType配置决定，如果wifitype没有配置，则转移到客厅

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/{familyId}/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | familyId   | url |必填|家庭id |  


**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| Map<String,String> |  results  |  body |  &emsp;  | 如果被分享的设备存在子设备附件设备等，则本结构返回该主设备下的子设备的分享结果 |

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
  "retCode":"00000",
  "retInfo":"成功"，
  "results":{"pengfei-2":"00000","pengfei-5":"00000"}

}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108、F31229   

   


## 设备管理员分享设备给个人

**使用说明**

> 设备管理员分享设备给个人，发送分享个人设备消息给目标用户，记录消息发送结果到日志，支持targetId为临时的userid

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{targetId}/shareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| ShareDevice  | shareDev   | Body |必填|设备的分享信息 |  
| String  | targetId   | url |必填|分享设备的目标用户 |  


**输入对象参数说明**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|devInfo |  DeviceBriefInfo  |  设备简明信息  |  必填  | 
|devName|   String| 设备名称|   必填  |
|permission|    Permision|  分享权限，authType必须为share|  必填  |
|deviceId|  String|设备ID|    必填  |




 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/person/ {targetId}/ShareDevice

{
    "shareDev": {
        "devInfo": {
            "deviceId": "100823"
        },
        "devName": "test",
        "permission": {
            "authType": "share",
            "auth": {
                "view": true,
                "set": false,
                "control": false
            }
        }
    }
}


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31107、E31108、F31229   

   



## 设备管理员取消设备分享

**使用说明**

> 设备管理员取消用户分享，发送取消个人分享设备消息给目标用户，记录消息发送结果到日志, 支持targetId为临时的userid

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/ {targetId}/{devId}/shareDevice`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | devId   | url |必填|设备id |  
| String  | targetId   | url |必填|设备分享者 |  


**输出参数**  

|    字段名    |     类型      | 说明  |备注 |
| ------------- |:----------:|:-----:|:--------:|
|&emsp; |  &emsp;  |  &emsp;  |  &emsp;  | 





 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/person/ {targetId}/{devId}/shareDevice


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  

   
## 用户删除分享给自己的个人设备

**使用说明**

> 用户取消设备管理员分享给自己的设备

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/person/{devId}/shareDeviceToMe`  
 **HTTP Method：** DELETE

**输入参数**  

| 类型   | 参数名 | 位置  | 必填|说明|  
| ---- |:-----:|:-----:|:-----:|:------:|
| String  | devId   | url |必填|设备id |   



**输出参数**  
标准输出

 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/person/ {devId}/shareDeviceToMe

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008  





## 家庭管理员或设备管理员修改设备属性信息

**使用说明**

> 用户作为管理员或者作为家庭成员，如果是家庭成员，必须是设备管理员，拥有权限，可以修改设备信息，其中包含设备所在房间，设备在家庭中的昵称，设备分享权限，要修改的设备为一个或多个，这些设备都属于一个家庭，多个设备是，遇到失败，则本次操作中断。

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/updateShareDevice`  
 **HTTP Method：** POST

**输入参数**  

| 类型   | 参数名 | 位置  | 必填|说明|  
| ---- |:-----:|:-----:|:-----:|:------:|
| ShareDevice[]  | shareDevices   | body |必填|设备信息 |  
| String  | familyId   | body |必填|家庭ID |   


**输入参数说明**  

|**名称** |设备共享信息 |&emsp;|ShareDevice|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|      
|devInfo|DeviceBriefInfo|设备简明信息|成员deviceId必填，其他字段被忽略|  
|devName|String|设备名称|选填| 
|devRoomId|String|设备所属房间ID|选填| 
|permission|Permision|分享权限此处权限authType 必须为home|选填| 


**输出参数**  
标准输出

 **示例**  

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: *****
sequenceId: 2*****10
accessToken: TGT1****IYRRAVLOMS0
sign: e5bd9aefd68c16a9*****ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b*****23b94eb6ff9
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

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F31218、E31108、E31120、E31401  






## 家庭管理员或家庭成员批量更新设备房间属性

**使用说明**

> 用户作为管理员或者作为家庭成员，修改设备所属房间信息

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/updateDevicesShareRoom`  
 **HTTP Method：** POST

**输入参数**  

| 类型   | 参数名 | 位置  | 必填|说明|  
| ---- |:-----:|:-----:|:-----:|:------:|
| ShareDevice[] | shareDevices   | body |必填|设备信息 |  


**输入参数说明**  

|**名称** |设备共享信息 |&emsp;|ShareDevice|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|      
|familyId|String|设备所属家庭|必填|  
|devRoomName|String|设备所属房间名称|必填| 
|devFloorId|String|设备所在楼层ID|和devFloorOrderId二选一，如果本字段标识的楼层不存在，返回错误| 
|devFloorOrderId|String|设备所在楼层|不填默认为一层| 
|devInfo|DeviceBriefInfo|设备信息|内容中只填写deviceId，且未必填，其他字段忽略| 


**输出参数**  

标准输出

 **示例**  

**请求样例**
```java  
请求地址    https://{baseuri}/ufm/v1/protected/shareDeviceService/family/updateDevicesShareRoom

{
    "shareDevices": [{
            "devInfo": {
                "deviceId": "MOCKDEV_CJL-UFM-MANAGER"
            },
            "devRoomName": "测试房间名",
            "familyId": "755111162274000000"
        },
        {
            "devInfo": {
                "deviceId": "MOCKDEV_CJL-UFM-MANAGER"
            },
            "devRoomName": "测试房间名",
            "familyId": "755111162274000000"
        }
    ]
}


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31109




## 查询用户设备房间位置信息

**使用说明**

> 用户查询指定家庭下设备的房间位置信息

 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/deviceAndRoom`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| familyId  | String   | body |必填|家庭ID | 





**输出参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| DeviceRoomInfoDto[]  | deviceAndRooms    | body |必填|房间位置信息 | 
| String  | familyId   | body |必填|家庭ID | 
| String   | familyName   | body |必填|家庭名称 | 

 **示例**  

**请求样例**
```java  

请求地址    https://uws.haier.net/ufm/v1/protected/shareDeviceService/family/deviceAndRoom

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






## 家庭成员批量分享设备到家庭

**使用说明**

> 家庭成员或家庭管理员作为设备的管理员分享设备给家庭，发送分享家庭设备消息给家庭成员，包含管理员，记录消息发送结果到日志,分享过程中</br>
>1、如果同一用户已经分享，自动取消原来分享，建立新的分享关系；</br>
>2、如果不是同一用户分享，判断此用户是不是绑定者，如果不是，作为垃圾数据处理，如果是，提示设备已经被分享。如果分享没有房间信息，系统会分享设备到默认房间；</br>
>3、如果未填写房间ID，未填写楼层ID时默认将设备放入一楼，如果填写了楼层ID，将设备放入指定的楼层下的房间；</br>
>4、如果请求没有填写家庭ID，则会把设备分享到默认家庭,本次分享会返回每个设备的分享结果；


 **接口描述**

?> **接入地 址：**  `/ufm/v1/protected/shareDeviceService/family/shareDeviceList`  
 **HTTP Method：** POST

**输入参数**  

| 类型   | 参数名 | 位置  | 必填|说明|  
| ---- |:-----:|:-----:|:-----:|:------:|
| ShareDevice[] | shareDevices   | body |必填|设备的分享信息，最大一次请求数为10 | 


**输入参数说明**  

|**名称** |设备共享信息 |&emsp;|ShareDevice|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|      
|devInfo|DeviceBriefInfo|设备简明信息|必填|  
|devRoomId|String|设备所在房间|选填，不填进入默认房间，按规则生成| 
|devRoomName|String|设备所在房间名称|选填，优先级低于devRoomId，devRoomId如果填写，则本字段被忽略| 
|devFloorId|String|设备所在楼层|如果填写devRoomId，本字段可不填写，如填写房间名，则本字段不填写默认为一层，填写则按照填写设备设备位置| 
|devName|String|设备名称|必填| 
|devFamilyId|String|设备所属家庭Id|非必填，不填会分享设备到默认家庭，家庭由用户配置| 
|permission|Permision|分享权限此处权限authType 必须为home|必填| 
|deviceId|String|设备ID|必填| 

**输出参数**  

| 类型   | 参数名 | 位置  | 必填|说明|  
| ---- |:-----:|:-----:|:-----:|:------:|
| Map<String,String>| shareResults    | body |必填|Key为设备ID，value为设备分享结果，值同错误码|  

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
"shareDevices":[{
"devInfo":{           
"deviceId": "100823"
},
"devName":"test",
"devfamilyId":"",
"devRoomId":"",
"devRoomName":"测试一下",
"permission":{ "authType":"home",
"auth":{ "view": true, "set": false, "control": false}},
" devFamilyId":"1256738"
},{
"devInfo":{           
"deviceId": "100823"
},
"devName":"test",
"devfamilyId":"",
"devRoomId":"",
"devRoomName":"测试一下",
"permission":{ "authType":"home",
"auth":{ "view": true, "set": false, "control": false}},
" devFamilyId":"1256738"
},{
"devInfo":{           
"deviceId": "100823"
},
"devName":"test",
"devfamilyId":"",
"devRoomId":"",
"devRoomName":"测试一下",
"permission":{ "authType":"home",
"auth":{ "view": true, "set": false, "control": false}},
" devFamilyId":"1256738"
}]
}


```  

**请求应答**

```java
{
    "retCode": "00000", 
"retInfo": "成功",
"shareResults":[ "100823": "00000","100824": "00000","100825": "00000"]
 }


```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108、E31111、E31129








[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
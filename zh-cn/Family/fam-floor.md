# 楼层管理

!> **更新时间**：{docsify-updated}  




## 用户创建楼层

**使用说明**

>1. 使用默认楼层数据不需要填写输入参数</br>
>2. 楼层取值为-3到10，其他楼层则不允许创建，创建后返回</br>
>3. 本期不支持自定义名称以及其他属性</br>


**接口描述**  

?> **接入地 址：**  `/ufm/v1/protected/floorService/addFloorInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| --------- |:----------:|:-----:|:-----------:|:-----------:|  
|  FloorInfo[]    | floorInfos | Body| 必填|楼层信息|   
|  String    | familyId | Body| 必填|家庭ID|   

**输入参数对象说明**   

|**名称** |描述楼层信息 |&emsp;|RoomInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|familyId|String|楼层所属家庭ID|非必填|  
|floorOrderId|String|楼层ID|固定值范围为：-3到5，必填|  
|floorName|String|楼层名称|可自定义（v2.0版本不支持）|  
|floorLabel|String|楼层标签|非必填（v2.0版本不支持）| 
|floorLogo|String|楼层logo url|非必填（v2.0版本不支持）| 
|floorPicture|String|楼层图片 url|非必填（v2.0版本不支持）| 
|floorExternData|String|楼层扩展信息，使用json格式，包含不需要平台关注用户终端业务的简短属性|非必填（v2.0版本不支持）|  

**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|FloorInfo[] |  floorInfos  |   Body  |  必填  | &emsp; |

**示例**  

**请求样例**
```
请求地址    https://{baseuri}/ufm/v1/protected/floorService/addFloorInfo

{
    "familyId": "258178824707000000",
    "floorInfos": [{
        "floorOrderId": "-2"
    }]
}

```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "floorInfos": [{
        "floorId": "100000000000000102",
        "familyId": "258178824707000000",
        "floorClass": "FLOOR",
        "floorOrderId": "-2"
    }]
}


```

 **接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E35002、E31108 



## 用户编辑楼层（v2.0版本不支持)

**使用说明**

>用户定制楼层信息，返回楼层的定制信息。


**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/floorService/updateFloorInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  FloorInfo    | floorInfo | Body| 必填|楼层信息|  


**输入参数对象说明**  
 
|**名称** |描述楼层信息 |&emsp;|FloorInfo|
| ------------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|familyId|String|楼层所属家庭ID|必填|  
|floorId|String|楼层ID|必填|  
|floorName|String|楼层名称|可自定义|  
|floorLabel|String|楼层标签|非必填| 
|floorLogo|String|楼层logo url|非必填| 
|floorPicture|String|楼层图片 url|非必填| 
|floorExternData|String|楼层扩展信息，使用json格式，包含不需要平台关注用户终端业务的简短属性集合|非必填|  

**输出参数**  

标准输出

**示例**  

**请求样例**
```

请求地址    https://{baseuri}/ufm/v1/protected/ floorService/updateFloorInfo

{
    "floorInfo": {
        "familyId": "11111",
        "floorId": "2",
        "floorName": "空中花园"
    }
}


```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功"}

```

 **接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E35001、E31108

## 用户删除楼层

**使用说明**

>1. 用户删除楼层信息，如果楼层内有设备，则楼层不能删除 </br>
>2. 删除楼层时，房间一并删除 </br>
>3. 楼层只允许删除两端楼层，一层不能删除。(IOT接口不限制)
 

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/floorService/delFloorInfo`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
|  List<String>    | floorIds | Body| 必填|要删除的楼层Id列表|  
|  String    | familyId | Body| 必填|要删除楼层所在的家庭|  


**输出参数**  
   
|**名称** |描述房间信息 |&emsp;|RoomInfo|  
| ------ |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|List<FloorOperationResult>|results|删除结果列表|必填|  



**示例**  

**请求样例**
```
请求地址    https://{baseuri}/ufm/v1/protected/floorService/ delFloorInfo


{
    "floorIds":["1234444","1234445"],
"familyId":"22222222”
}



```  

**请求应答**

```
{“retCode”:”00000”,”retInfo”:”成功”, results:[{ “floorId”,”11111111”, “retCode”,”00000”, “retMsg”,”成功”},{ “floorId”,”122222222”, “retCode”,”00000”, “retMsg”,”成功”}]}

```
 **接口错误码** 
 
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E35001,E35003、E31108


## 添加房间到楼层

**使用说明**

> 用户作为家庭成员或家庭管理员，添加房间到楼层。
>1. floorId楼层不存在，则返回异常码；
>2. floorId入参不存在，使用floorOrderId，创建楼层，同时添加房间到楼层.
>3. 支持限制为10个房间
>4. 以上遇到重名忽略并返回，原数据不变。
  

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/floorService/addRoomsToFloor`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------- |:-------------:|:-----:|:-----------:|:----------:|
|  List<RoomInfo>    | roomInfos | Body| 必填|房间id列表|  
|  String    | familyId | Body| 必填|家庭id|  
|  String    | floorId | Body| 选填|楼层Id|  
|  String    | floorOrderId | Body| 选填|以floorId为准，floorId不填，则以floorOrderId为准。floorId不存在，则返回失败，floorOrderId不存在|  

**输入参数对象说明**    
 
|**名称** |描述房间信息 |&emsp;|RoomInfo|
| ---------- |:-------------:|:-----:|:-------------:|  
|**字段名**|**类型**|**说明**|**备注**|    
|roomName|String|房间名称|必填|  

 


**输出参数**   
 
| 类型         | 参数名         | 位置  | 必填|说明|
| ---------- |:----------:|:-----:|:-------------:|:-------------:|
|  List<RoomInfo>    | roomInfos | Body| 必填|添加的家庭房间信息| 

**示例**  

**请求样例**
```
请求地址    https://{baseuri}/ufm/v1/protected/floorService/addRoomsToFloor

{"familyId":"258178824707000000",
"floorOrderId":"1",
"roomInfos":[
{"roomName":"客厅"}
]}


```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "roomInfos": [{
        "roomName": "客厅",
        "roomId": "100000000000000001",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2020-01-16 17:24:01"
    }]
}

```
 **接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E35001、E31108







## 查询楼层内房间信息

**使用说明**

>家庭管理员或家庭成员查询家庭下指定楼层的房间信息列表
 
**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/floorService/queryFloorRooms`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | body |必填|家庭ID  | 
| String  | floorId  | body |必填|楼层ID  | 

 
**输出参数**  


| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| RoomInfo[]  | roomInfos  | body |&emsp;|&emsp;  | 



**示例**  

**请求样例**

```
请求地址    https://{baseuri}/ufm/v1/protected//roomService/ queryFloorRooms

{"familyId":"258178824707000000","floorId":"100000000000000104"}

```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "roomInfos": [{
        "roomName": "卧室",
        "roomId": "664178830080000000",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2020-12-23 08:21:20"
    }, {
        "roomName": "客厅",
        "roomId": "100000000000000001",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2020-01-16 17:24:01"
    }, {
        "roomName": "洗漱间",
        "roomId": "100000000000000003",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2019-02-20 03:07:04"
    }, {
        "roomName": "厨房",
        "roomId": "100000000000000002",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2020-01-17 10:51:38"
    }, {
        "roomName": "客厅2",
        "roomId": "278178841686000000",
        "familyId": "258178824707000000",
        "roomClass": "ROOM",
        "roomOrderId": "1",
        "floorId": "100000000000000104",
        "floorOrderId": "1",
        "roomCreateTime": "2020-12-23 11:34:46"
    }]
}


```

 **接口错误码** 
 
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E35001、E31108





## 查询家庭楼层下设备信息  

**使用说明**

> 家庭管理员或家庭成员查询家庭楼层设备信息。

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/floorService/queryFloorDevices`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | body |必填|家庭ID | 
| String  | floorId  | body |必填|楼层ID |


**输出参数**  

| 类型   | 参数名  | 位置  | 必填|说明|
| ------ |:--------:|:-----:|:--------:|:-------:|
| shareDevice[] | shareDevices | body|&emsp;|&emsp; | 


**示例**  

**请求样例**
```

请求地址    https://{baseuri}/ufm/v1/protected/floorService/ queryFloorDevices

{
"familyId": "258178824707000000",
"floorId": "100000000000000104"
}



```  

**请求应答**

```

{
    "retCode": "00000",
    "retInfo": "成功",
    "shareDevs": [{
        "devInfo": {
            "deviceName": "卧室",
            "deviceId": "MOCKDEV_UFM_MANAGER",
            "wifiType": "101c120024000810180100418000054300000000000000000000000000000000",
            "deviceType": "18011001",
            "isMeshDevice": false,
            "online": true,
            "bindTime": "2020-03-16 21:12:19"
        },
        "devName": "卧室",
        "devFamilyId": "258178824707000000",
        "devRoomId": "664178830080000000",
        "shareTime": "2020-12-23 07:16:57",
        "devOwner": {
            "isVirtualUser": "false",
            "email": "136****8934",
            "avatarUrl": "https://accountstatic.haier.com/avatar/b1120a5ef93ed15e792e557124139a12.jpg",
            "name": "136****8934",
            "userId": "100013957366169529",
            "ucUserId": "51072545",
            "mobile": "136****8934"
        },
        "permission": {
            "authType": "home",
            "auth": {
                "view": true,
                "set": false,
                "control": true
            }
        }
    }]
}


```

 **接口错误码**  

> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008，E35001、E31108
  





## 查询家庭下的楼层列表 

**使用说明**

> 查询家庭下的楼层列表信息，按照floorOrderId排序

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/familyService/floorInfoList`  
 **HTTP Method：** POST

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-------------:|
| String  | familyId  | body |必填|家庭ID | 


**输出参数**  

| 类型   | 参数名  | 位置  | 必填|说明|
| ------ |:--------:|:-----:|:--------:|:-------:|
| FloorInfo[] | floorInfos | body|必填|楼层信息 | 


**示例**  

**请求样例**

```

请求地址    https://{baseuri}/ufm/v1/protected/familyService/floorInfoList

{
"familyId": "258178824707000000"
}




```  

**请求应答**

```
{
    "retCode": "00000",
    "retInfo": "成功",
    "floorInfos": [{
        "floorName": "三层",
        "floorId": "100000000000000106",
        "familyId": "258178824707000000",
        "floorClass": "FLOOR",
        "floorOrderId": "3"
    }, {
        "floorName": "一层",
        "floorId": "100000000000000104",
        "familyId": "258178824707000000",
        "floorClass": "FLOOR",
        "floorOrderId": "1"
    }, {
        "floorName": "负二层",
        "floorId": "100000000000000102",
        "familyId": "258178824707000000",
        "floorClass": "FLOOR",
        "floorOrderId": "-2"
    }]
}




```

 **接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、E31108

  






[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png
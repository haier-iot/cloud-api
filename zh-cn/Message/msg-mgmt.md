#  业务消息管理


#### 上报消息的读取状态

> 更新消息的读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息，该接口需在终端读取消息时调用。

##### 1、接口定义

?> **接入地址：** `/msg/reportStatus`</br>
**HTTP Method：** POST 

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskId|String|body|是|终端收到的任务标识


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatus

POST data:
{ 	"taskId":"TK123456789123456789" }

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546854308557 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```


#### 批量更新读取状态

> 批量更新当前登录用户的消息读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息，该接口需在终端读取消息时调用。  

##### 1、接口定义

?> **接入地址：** `/msg/reportStatusByPatch`</br>
**HTTP Method：** POST   

**前置条件：**   
1.用户登录后使用（即：调用接口时Header中accessToken参数必填）。  
2.终端收到并读取消息。  

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskIds|Set<String>|body|是|终端收到的任务标识


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatusByPatch

POST data:
{"taskIds":["TK348b8e2b23184067a6d1dc3b94a138b8"]}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555292892282 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 50
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```

#### 按消息类型更新读取状态

> 按消息的业务类型更新当前登录用户的消息读取状态为已读</br>
> 若是阅后即焚的消息，该终端更新消息为已读状态后，其他终端将不再收到相同的消息，该接口需在终端读取消息时调用。  

##### 1、接口定义

?> **接入地址：** `/msg/reportStatusByType`</br>
**HTTP Method：** POST   

**前置条件：**   
1.用户登录后使用（即：调用接口时Header中accessToken参数必填）。  
2.终端收到并读取消息。
  

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|是|消息的业务类型


**输出参数：** 标准输出参数

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/reportStatusByType

POST data:
{"businessType":1}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555293031670 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 18
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```


#### 查询未读消息的数量

> 查询当前登录用户下各业务类型的未读消息数量</br>
> 按业务类型统计未读消息数量。  

##### 1、接口定义

?> **接入地址：** `/msg/getUnreadNum`</br>
**HTTP Method：** POST   

**前置条件：** 

 用户登录后使用（即：调用接口时Header中accessToken参数必填）  
  

**输入参数：** 无输入参数

**输出参数：**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgUnreadNumDto >|body|是|各消息业务类型下未读消息的数量  

##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/getUnreadNum

POST data:
{}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555293070703 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 2
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":[{"businessType":0,"msgNums":46}]}

```



#### 查询历史消息

> 1、用户可以查询最长1年以内的历史消息 </br>
> 2、历史消息不支持跨APP查询 </br>
> 3、支持按终端查询（appId+userId+clientId）、按业务类型查询(appId+userId+clientId+businessType)、标签查询（appId+userId+clientId+tag）</br>
> 4、支持分页，按时间倒序排列</br>

##### 1、接口定义

?> **接入地址：** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|否|消息业务类型
tag|String|body|否|自定义标签
msgTime|String|body|是|查询消息的起始时间,格式为：`yyyy-MM-dd HH:mm:ss.SSS`
queryTag|Integer|body|是|标识查询起始时间之前、还是之后的消息。1代表之前，0代表之后
querySize|Integer|body|是|每次查询消息的数量


**输出参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgClientHiustoryDto>|body|是|推送记录信息
totalRecords|Integer|body|是|返回记录总数


##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/getMsgHistory

POST data:
{"businessType":1,"queryTag":"0","msgTime":"2019-1-8 16:31:10.0","querySize":5}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555293778307 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 79
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success","totalRecords":61,"retData":[{"taskId":"TK56f786c40384472b944125a71f87ae8a","businessType":1,"msgStatus":4,"readStatus":2,"pushTime":"2019-04-13 13:20:00.000","message":{"notification":null,"android":null,"ios":null,"data":{"body":{"view":{"showType":21,"title":"push by clients","content":"push by clients"},"extData":{"isMsgCenter":1}}},"options":{"msgName":"","businessType":1,"expires":60,"priority":null,"jiguangOptions":null},"version":"v3"}},{"taskId":"TKa54cd61ece804df29bb76c4824eb57c4","businessType":1,"msgStatus":4,"readStatus":2,"pushTime":"2019-04-13 13:20:00.000","message":{"notification":{"title":"push by clients","body":"push by clients "},"android":{"jpush":{"collapseKey":"push by clients","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"push by clients","body":"push by clients ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"push by clients","content":"push by clients"},"extData":{"isMsgCenter":1}}},"options":{"msgName":"","businessType":1,"expires":60,"priority":null,"jiguangOptions":null},"version":"v3"}},{"taskId":"TK2db6d00fead446488e3ce75d564ed20c","businessType":1,"msgStatus":4,"readStatus":2,"pushTime":"2019-04-13 13:20:00.000","message":{"notification":null,"android":null,"ios":null,"data":{"body":{"view":{"showType":21,"title":"push by clients","content":"push by clients"},"extData":{"isMsgCenter":1}}},"options":{"msgName":"","businessType":1,"expires":60,"priority":null,"jiguangOptions":null},"version":"v3"}},{"taskId":"TK10c2fc87508144b7a135ec88b45f8064","businessType":1,"msgStatus":4,"readStatus":2,"pushTime":"2019-04-13 13:20:00.000","message":{"notification":{"title":"push by clients","body":"push by clients "},"android":{"jpush":{"collapseKey":"push by clients","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"push by clients","body":"push by clients ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"push by clients","content":"push by clients"},"extData":{"isMsgCenter":1}}},"options":{"msgName":"","businessType":1,"expires":60,"priority":null,"jiguangOptions":null},"version":"v3"}},{"taskId":"TK0da2717daf44449ab5701c94821ec67c","businessType":1,"msgStatus":4,"readStatus":2,"pushTime":"2019-04-13 13:20:00.000","message":{"notification":{"title":"push by clients","body":"push by clients "},"android":{"jpush":{"collapseKey":"push by clients","priority":0,"ttl":86400,"restrictedPackageName":"0"},"fcm":{"title":"push by clients","body":"push by clients ","notification ":{"sound":"default"}}},"ios":null,"data":{"body":{"view":{"showType":21,"title":"push by clients","content":"push by clients"},"extData":{"isMsgCenter":1}}},"options":{"msgName":"","businessType":1,"expires":60,"priority":null,"jiguangOptions":null},"version":"v3"}}]}
```


#### 删除历史消息


>1、用户提交申请删除一条或批量删除多条应用内消息</br>
>2、系统收到请求后，将相关消息标记为删除状态(MSG_DISPATCH表MSG_STATUS=5)

##### 1、接口定义

?> **接入地址：** `/msg/delMsgHistory`</br>
**HTTP Method：** POST 

**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
taskIds|Set<String>|body|是|消息任务id(taskId)列表

**输出参数：** 标准retCode、retInfo输出。


##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/msg/delMsgHistory

POST data:
{"taskIds":["TKb21a34992ead4246a9adf61a61b9c338"]}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555293756682 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 50
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)


```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```


#### 查询消息汇总


>1、按消息分类返回统计信息；</br>
>2、每一类消息的消息总数；</br>
>3、每一类消息的未读消息数；</br>
>4、每一类消息若有未读消息返回最近一条未读消息的内容，否则不返回；</br>
>5、每一类消息若设置了免打扰则返回免打扰信息列表，否则不返回；</br>
>6、每一类消息若有消息返回最新消息的时间，否则不返回；</br>
>7、消息分类按最新消息的时间倒序排列</br>


##### 1、接口定义

?> **接入地址：** `/msg/getMsgSubtotal`</br>
**HTTP Method：** POST 


**输入参数：** 无输入参数

**输出参数：**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<MsgCategoryDto>|body|是|消息分类汇总信息 








#### 上报BSM业务消息状态


>1、用户登录后使用（即：调用接口时Header中accessToken参数必填）


##### 1、接口定义

?> **接入地址：** `/msg/getMsgSubtotal`</br>
**HTTP Method：** POST 


**输入参数：** 

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
boId|String|body|是|boID业务对象ID
boStatus|Integer|body|是|业务对象状态0:new/1:read/2:deleted

**输出参数：**  

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|String|body|是|返回更新成功与否 




##### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/ bms/reportBSMStatus

POST data:
{ 	"taskId":"TK123456789123456789",
   "boId":"1234567",
 "userId":"1234566",
   "appId":"MB-****-0000",
   "clientId":"1234",
   "boStatus":"0"
 }

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1546854308557 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
appVersion: 99.99.99.99990
timezone: Asia/Shanghai
language: zh-cn
clientId: 123456
accessToken: ************************
Content-Length: 36
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)



```

**输出参数**

```
{"retCode":"00000","retInfo":"success"}
```






[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:../Message/_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:../Message/_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:../Message/_media/_MessagePush/MessagePush_flow.png

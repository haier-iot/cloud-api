#  免打扰管理

## 按类型设置免打扰
> 设置终端能够按照业务类型、优先级、时间段进行免打扰；</br>
> 1.以userId+appId+clientId标识唯一终端；</br>
> 2.同一终端可以设置多条免打扰信息；</br>
> 3.不同消息业务类型（businessType）需要分别设置免打扰；</br>
> 4.同一终端下设置多条免打扰时时间不允许有交叉；</br>
> 5.每条免打扰配置支持设置多个优先级</br>
> 用户登录后使用（即：调用接口时Header中accessToken参数必填）。

### 1、接口定义
?> **接入地址：** `/config/setNotDisturb`</br>
**HTTP Method：** POST </br>


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|是|消息业务类型
priority|Integer|body|是|消息优先级，priority定义见消息模型
beginTime|String|body|是|开始时间，格式 HH:ss
endTime|String|body|是|结束时间，格式 HH:ss

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndId|String|body|是|免打扰唯一标识



### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/config/setTagNotDisturb

POST data:
{"priority":1,"beginTime":"21:00","dndTag":"abcd","endTime":"07:00"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1566443152916 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 1234
accessToken: ************************
Content-Length: 68
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)



```
**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":"DND9b8f5d01de9e4ef6a5a69323e68dd5a8"}

```




## 按标签设置免打扰
> 设置终端能够按照发送标签、优先级、时间段进行免打扰；</br>
> 1.以userId+appId+clientId标识唯一终端；
> 2.同一终端可以设置多条免打扰信息；
> 3.不同标签需要分别设置免打扰；
> 4.同一终端下同一标签设置多条免打扰时，时间不允许有交叉；
> 5.每条免打扰配置支持设置多个优先级



### 1、接口定义
?> **接入地址：** `/config/setTagNotDisturb`</br>
**HTTP Method：** POST </br>


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndTag|string|body|是|免打扰标签
priority|Integer|body|是|消息优先级，priority定义见消息模型
beginTime|String|body|是|开始时间，格式 HH:ss
endTime|String|body|是|结束时间，格式 HH:ss

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndId|String|body|是|免打扰唯一标识



### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/config/setTagNotDisturb

POST data:
{"priority":1,"beginTime":"00:00","tag":"DC330D5EE767 ",endTime":"07:00"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555292586708 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 69
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)




```
**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":"DND9b8f5d01de9e4ef6a5a69323e68dd5a8"}

```



## 取消终端免打扰

删除设定的免打扰配置

1、用户登陆后，方可使用（Header中accessToken参数必填）
2、已设置过免打扰


### 1、接口定义
?> **接入地址：** `/config/cancelNotDisturb`</br>
**HTTP Method：** POST 


**输入参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
dndId|String|body|是|免打扰设置唯一标识


**输出参数：**标准输出参数



### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/config/cancelNotDisturb

POST data:
{"dndId":"DNDd64c4dc8b7e441b8a0558ec92818e534"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555292708273 
appKey: *************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 47
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)




```
**输出参数**

```
{"retCode":"00000","retInfo":"success"}

```


## 查询免打扰信息

> 获取已设定的免打扰配置列表，以userId+appId+clientId（即终端）为粒度查询

### 1、接口定义

?> **接入地址：** `/config/getNotDisturbs`</br>
**HTTP Method：** POST 


**输入参数：** 无

**输出参数：**

参数名|类型|位置|是否必填|说明
:-:|:-:|:-:|:-:|:-
retData|List<DoNotDisturbDto>|body|是||



### 2、请求样例

**输入参数**
```
POST https://uws.haier.net/ums/v3/config/getNotDisturbs

POST data:


[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 99.99.99.99990
sequenceId: 20161020153428000015
sign: ************************
timestamp: 1555292642458 
appKey: ************************
Content-Encoding: utf-8
Content-type: application/json
timezone: Asia/Shanghai
language: zh-cn
clientId: 9c510d7c64f7a570874884e0a94f6a9e
accessToken: ************************
Content-Length: 0
Host: uws.haier.net
User-Agent: Apache-HttpClient/4.5.3 (Java/1.8.0_192)



```
**输出参数**

```
{"retCode":"00000","retInfo":"success","retData":[{"dndId":"DND96e921b990764adfa913e6da1887e955","dndType":1,"priority":1,"beginTime":"21:00","endTime":"07:00","dndTag":"abcd"}]}

```



[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:../Message/_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:../Message/_media/_MessagePush/MessagePush_liucheng.png
[MessagePush_flow]:../Message/_media/_MessagePush/MessagePush_flow.png

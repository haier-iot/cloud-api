
!> **更新时间**：{docsify-updated} 

## 范围

本规范从规范使用U+IOT平台UWS服务的角度出发，提出了各应用端或应用服务调用UWS服务所必须遵循的要求。


## 总体要求

本规范旨在为各应用端或应用服务开发使用U+IOT平台UWS服务提供规范化要求，对使用UWS服务进行约束和规范。


## 使用前提
业务方使用UWS服务，必须在海极网（ http://www.haigeek.com/ ）上提前创建应用，不同业务，创建不同应用；
应用上线需要经过海极网审核认证，未经过审核认证的服务，海极网有权随时下线该应用访问UWS服务权限；

## 访问限制
1. 使用用户accessToken进行访问的接口，<font color="red">存在单ip每分钟访问次数限制</font>，这类接口尽量不要从服务端server发起访问，若必须，请联系产品及支持沟通具体使用场景。<br/>
2. 非用户accessToken及白名单ip从海尔DTS域外访问，<font color="red">存有每小时访问次数限制，使用小规模业务开展。</font><br/>
3. 非用户accessToken及白名单ip从海尔DTS域内访问，在正常业务开展下，无访问量限制。但平台监控存在异常访问时，向业务方发起询问后，若业务方5工作日内未反馈的，平台有权限制访问。


## 规范描述及要求
### UWS服务内容

|序号|	服务名称|	应用名|	备注|
:-:|:-:|:-:|:-: 
1|	[账户服务](zh-cn/Account)|	N/A	|集团690用户中心提供
2|	[设备管理](zh-cn/DeviceManage)	|uds、stdudse、udse|	设备注册、设备管理、设备控制
3|	[数据订阅](zh-cn/DataSubscription)|	无|	设备数据订阅、应用数据订阅
4|	[家庭模型](zh-cn/FamilyManage)	|ufm、ufme|	家庭成员管理、家庭设备管理
5|	[场景引擎](zh-cn/IFTTT)|	iftttscene|	场景创建、场景执行、场景日志管理
6|	[云定时](zh-cn/Scheduler)|	scheduler|	设备预约控制、场景预约控制  
7|	[设备影子](zh-cn/DevicesShadow)|	shadow|	设备云状态查询、预期设备控制管理
8|	[消息推送](zh-cn/MessagePush)|	ums、umse|	设备消息推送、APP消息推送、语音消息推送
9|	[设备资源云存储](zh-cn/CapacityService_DeviceCloudStorage)|	css|	设备图片/视频存储、查看 
10|[账户授权](zh-cn/Session)|uaccount|应用OAuth授权，用户授权管理


### UWS服务在线文档

各应用端或应用服务开发时遵照UWS服务文档的指南说明。<br/>
UWS服务在线文档URL： https://haier-iot.github.io/guide/#/zh-cn/

### 老服务下线通知
下为平台老服务停止运维下线计划，请仍在使用以下老服务的使用方在2019年6月30日前完成切换使用UWS服务。<br/>
**届时若由于使用方未切换升级服务造成的影响，由使用方自行承担。**

|提供方式|	服务名称|	说明|	停止运维时间|	下线时间|
:-:|:-:|:-:|:-:|:-: 
Open API|	Open API V1|	请升级使用UWS 服务|	2019年6月|	2019年12月
Open API|	Open API V2	|请升级使用UWS 服务	|2019年6月	|2019年12月
Common  API	|Common API|	请升级使用UWS 服务|	2019年6月|	2019年12月
能力服务	|论坛服务	|服务下线	|2019年6月|	2019年12月
能力服务	|意见反馈	|服务下线	|2019年6月|	2019年6月
其他	所有 |Dubbo接口	|服务下线	|2019年6月|	2019年12月


###  服务更新升级

随着智能物联业务的开展以及平台产品服务迭代，IOT平台服务也会相应的升级UWS服务和新服务发布。
所有服务升级变动都会通过微信公众号“HaierUplus-beijing”消息通知，以及各产业主要人员的邮件通知。<br/>
**业务升级服务要求** <br/>
1、	业务侧需要在接收到服务升级通知后，最迟6个月内升级各自服务使用最新版本；

2.升级通知消息
搜索“HaierUplus-beijing” 关注公众号，回复 “通知+姓名”；
注：公众号内回复 “升级计划”，可获取IOT云服务本月发布计划；


### 数据类型

1. **字段类型说明**

|限定类型|	说明|	格式|	json示例|
|---|---|---|---|
|DateTime	|日期时间类型的字符串|	`yyyy-MM-dd hh:mm:ss`| `	{“lgTime”:“2013-10-08 08:00:00”}`|
|Date	|日期类型的字符串	|`yyyy-MM-dd` 	|`{“lgDate”:“2013-10-08”}`|
|String	|字符串		|&nbsp;|`{“address”:“street 123”}`|
|int	|整形数字	|&nbsp;|	`{“age”:1234}`|
|long	|长整形数字	|&nbsp;|	`{“oid”:1234567890123}`|
|double	|浮点数数字	|&nbsp;|	`{“price”:12.35}`|
|boolean	|布尔型（true或false）	|&nbsp;|	`{“idOld”:true}`|

2.**null值说明**  

为避免解析错误， uws各接口的返回参数，不返回null值。
必填参数，无论输入还是输出，必须有值，不能为null
非必填参数，则说明如下：
数值类型数据（int、long、double）只会返回数字，包括正数、零及负数。
布尔类型数据（boolean）只返回true和false
以上基本类型本身不包含null值。

DateTime、Date、String及结构体类型的数据，如果为null时，所对应的属性将不返回。

**DateTime类型**  

birthday为DateTime类型，不为null时：

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

birthday为null时，则birthday属性不返回

    {"name":"Tom","age":23, "address":{"city":"beijing","street":"haidian"} }  

**String类型**  

name是String类型，不为null时：

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

name为null时，则name属性不返回

    {"age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }


**结构体类型**

address为结构体类型，不为null时：
  
    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

address为null时，address属性不返回

	{"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00" }


### 	服务接口健壮性要求

**数据解析要求**  

为保证服务API升级，接口中字段，不影响业务方使用，业务方需要处理数据时，严禁按照固定格式解析；


### 	安全性规范

**数据安全**

App端或应用服务要保证接入UWS时通过海极网获取的appid、appkey等信息的使用安全，出现因外泄导致违规使用UWS服务而被平台禁用的风险由开发者自行承担。
App端或应用服务通过UWS获取的数据等要确保存储安全。

**接口访问安全**


要严格按照UWS接口服务的使用文档调用,开发相关安全参照“海尔优家安全开发规范V1.0 .pdf”执行。


### 	特别服务要求

**云端控制设备要求**


关于面向普通用户的控制设备场景，必须通过账户授权token版的设备控制服务进行控制。
账户授权token获取方式参见UWS在线服务文档的账户授权章节。

**预约服务要求**


新增云端预约服务，必须接入U+ IOT预约定时服务，实现预约功能；



## 公共说明


### 接入协议    

对外提供的服务统一使用HTTPS协议，默认使用443端口， 在服务端使用TSL进行单向加解密处理。服务调用方在进行调用时，无需下载或安装证书。

### 接入地址    

应用开发时，请连接开发者环境进行开发、测试；

> 生产环境域名：`https://uws.haier.net`  
> 开发者环境域名：`https://uws.haier.net`  通过设置本地路由器DNS为39.97.52.209跳转访问服务。



###  公共参数
公共字段说明了每个请求、应答应包含的公共字段。这些字段无特殊情况，不在业务类接口报文定义中重复说明。

**输入参数**  

|参数名|	类型|位置|是否必填|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|appId|	String|	Header	|必填	|应用ID40位以内字符,Haier uHome 云平台全局唯一。开发者通过海极网申请获得。|
|appVersion	|String	|Header	|必填	|应用版本32 位字符,Haier uHome 云平台全局唯一。|  
|<font color="#FF0000">version</font>	|<font color="#FF0000">String</font>|<font color="#FF0000">Header</font>	|<font color="#FF0000">选填</font>|<font color="#FF0000">场景引擎服务必填，接口版本号,本期默认值：0.3</font>|  
|clientId	|String	|Header	|必填	|客户端ID27 位字符,客户端机编码与客户端 MAC 地址 拼合成唯一的客户端标识。 主要用途为唯一标识客户端 (例如,手机)。手机机编码为 IMEI 码。 手机 MAC 为 12 位地址。命名规范:客户端机编码(15 位)-客户 端 MAC 地址(12 位)格式: XXXXXXXXXXXXXXX-XXXXXXXXXXXX 举例: 356877020056553-08002700DC94。APP端可调用usdk获取，其他服务端自定义标识，不能为空。 |
|sequenceId	|String	|Header|必填	|报文流水(客户端唯一)客户端交易流水号。20 位, 前 14 位时间戳（格式：yyyyMMddHHmmss）,后 6 位流水 号。交易发生时,根据交易 笔数自增量。App应用访问uws接口时必须确保每次请求唯一，不能重复。|
|accessToken	|String	|Header|必填（登录后不为空，登录前可为空）|安全令牌 token，30 位字符。 用户登录 Haier U+ 云平台,由系统创建。用户退出 Haier U+ 云平台,由系统销毁。未登录时，访问不需要登录的平台接口，仍然需要传入本参数，参数值可为空或任意值（不超过30字符）|
|sign	|String|	Header|	必填|对请求进行签名运算产生的签名,签名算法见附录。|
|timestamp	|String	|Header	|必填|	应传入用户所在地时间戳，long型时间戳,精确到毫秒|
|language	|String	|Header|	必填	|该参数为多语言版提供支持。默认填写zh-cn即可。|
|timezone	|String|	Header|	必填	|代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可|
|Content-Type|String|	Header|	必填	|互联网媒体信息，填写为"application/json;charset=UTF-8" |


**输出参数**

|参数名|	类型|位置|是否返回|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	是	|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）|
|retInfo	|String	|Body	|是	|返回信息（用于调试的返回信息，不支持国际化，也不能直接显示在UI上）|


###  媒体类型
对于请求UWS规定用以下三种Content-Type媒体类型：

1.Content-Type: application/json
UWS服务与应用的交互数据接口统一为基于JSON的REST-RPC接口
```
POST /v1/animal HTTP/1.1
Host: uws.haier.net
Accept: application/json
Content-Type: application/json
Content-Length: 24

{   
  "name": "Gir",
  "animalType": "12"
}
```

2.Content-Type: multipart/form-data
适用于表单文件上传

3.Content-Type:application/octet-stream
适用于文件的上传下载




### 签名算法

1. **说明**


调用方需要对发送到uws的请求进行签名，签名后赋值到Header头中的sign属性（见公共部分说明），以便服务端进行签名验证。

2. **参数介绍**


**待签名字符串为：** url字符串 + Body字符串+appId+appKey +timestamp；

**url 字符串：**指请求的接口地址去除https://uws.haier.net 后剩余的路径部分；

**Body字符串：**指应用发送请求的Body部分去除所有空白字符后的JSON字符串，没有body时为空字符串（不是null）。

**appId：**Header头中的属性（见公共部分说明）；

**appKey：**在海极网给应用申请的appKey，不能明文发送；

**timestamp：**Header头中的属性（见公共部分说明）；

3. **算法**

签名算法就是对待签名字符串计算32位小写SHA-256值，算法示例如下。

```java
String getSign(String appId, String appKey, String timestamp, String body,String url){：
    URL urlObj = new URL(url);
    url=urlObj.getPath();
	appKey = appKey.trim();
	appKey = appKey.replaceAll("\"", "");
	if (body != null) {
		body = body.trim();
	}
	if (!body.equals("")) {
		body = body.replaceAll(" ", "");
		body = body.replaceAll("\t", "");
		body = body.replaceAll("\r", "");
		body = body.replaceAll("\n", "");
	}
	log.info("body:"+body);
	StringBuffer sb = new StringBuffer();
	sb.append(url).append(body).append(appId).append(appKey).append(timestamp);

	MessageDigest md = null;
	byte[] bytes = null;
	try {
		md = MessageDigest.getInstance("SHA-256");
		bytes = md.digest(sb.toString().getBytes("utf-8"));
	} catch (Exception e) {
		e.printStackTrace();
	}
	
	return BinaryToHexString(bytes);
}

String BinaryToHexString(byte[] bytes) {
	StringBuilder hex = new StringBuilder();
	String hexStr = "0123456789abcdef";
	for (int i = 0; i < bytes.length; i++) {		
		hex.append(String.valueOf(hexStr.charAt((bytes[i] & 0xF0) >> 4)));		
		hex.append(String.valueOf(hexStr.charAt(bytes[i] & 0x0F)));
	}
	return hex.toString();
}

```



[^-^]:常用图片注释

!> **当前版本：** [uws统一接入规范v1.3]()  
**更新时间**：{docsify-updated} 

## 接入地址

1. **接入前提**  
应用开发方使用海尔U+云平台的UWS服务前需在海极网 http://www.haigeek.com  注册成为开发者，并且申请创建自己的应用产品（应用类型包括Andriod、IOS）。
按照应用产品的开发测试、发布上线的阶段步骤对应使用所创建应用分配的appid、appkey的信息接入海尔U+云平台的UWS服务。

2. **接入方式**  
云平台UWS与应用的交互接口统一为基于JSON的REST接口。
GET、DELETE原语的请求参数为url的QueryParam，需要进行URLEncode。
UWS的使用方应做如下假设：
>   1.	云平台将强校验发送请求的参数，如果有与接口规范不一致的情况，接口调用将出错。
>   2.	云平台未来扩展接口功能时可能会增加接口应答返回时的参数数量。接口使用方应能对此兼容。

3. **接入协议**  
对外提供的服务统一使用HTTPS协议，默认使用443端口， 在服务端使用TSL进行单向加解密处理。服务调用方在进行调用时，无需下载或安装证书。

4. **接入地址**  
应用开发时，请连接开发者环境进行开发、测试；

|连接区域|	生产环境地址	|开发者环境地址|
|---|---|---|
|中国|`https://uws.haier.net`|	`https://dev-uws.haigeek.com`	|
|欧洲|`https://uws-euro.haieriot.net`|	无|
|北美|`https://uws-gea-us.haieriot.net`|无|


## 服务清单
U+平台现有的UWS服务说明

序号|UWS服务名|应用名|版本号（当前版本）
:-:|:-:|:-:|:-:  
1|[账户服务](zh-cn/Account)|uam|v1.0.0
2|[设备管理服务标准版](zh-cn/DevicesStandard)|uds|v2.0.2
3|[设备管理服务企业版](zh-cn/DevicesEnterprise)|udse|v1.5.2 
4|[数据订阅](zh-cn/DataSubscription)|无|v1.0.0
5|[家庭模型](zh-cn/Family)|ufm|v1.5.1  
6|[场景引擎](zh-cn/IFTTT)|iftttscene|v2.6.1  
7|[预约定时](zh-cn/Scheduler)|scheduler|v1.0.0  
8|[影子设备](zh-cn/DevicesShadow)|shadow|v1.0.0
9|[消息推送](zh-cn/MessagePush)|ums|v3.0.3
10|[天气服务](zh-cn/CapacityService_Weather)|暂未开放|暂未开发 
11|[智能设备资源云存储](zh-cn/CapacityService_DeviceCloudStorage)|css|v1.0.0   



##  公共参数
公共字段说明了每个请求、应答应包含的公共字段。这些字段无特殊情况，不在业务类接口报文定义中重复说明。

**输入参数**  

|参数名|	类型|位置|是否必填|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|appId|	String|	Header	|必填	|应用ID40位以内字符,Haier uHome 云平台全局唯一。开发者通过海极网申请获得。|
|appVersion	|String	|Header	|必填	|应用版本32 位字符,Haier uHome 云平台全局唯一。|
|clientId	|String	|Header	|必填	|客户端ID27 位字符,客户端机编码与客户端 MAC 地址 拼合成唯一的客户端标识。 主要用途为唯一标识客户端 (例如,手机)。手机机编码为 IMEI 码。 手机 MAC 为 12 位地址。命名规范:客户端机编码(15 位)-客户 端 MAC 地址(12 位)格式: XXXXXXXXXXXXXXX-XXXXXXXXXXXX 举例: 356877020056553-08002700DC94。APP端可调用usdk获取，其他服务端自定义标识，不能为空。 |
|sequenceId	|String	|Header|必填	|报文流水(客户端唯一)客户端交易流水号。20 位, 前 14 位时间戳（格式：yyyyMMddHHmmss）,后 6 位流水 号。交易发生时,根据交易 笔数自增量。App应用访问uws接口时必须确保每次请求唯一，不能重复。|
|accessToken	|String	|Header|必填（登录后不为空，登录前可为空）|安全令牌 token，30 位字符。 用户登录 Haier U+ 云平台,由系统创建。用户退出 Haier U+ 云平台,由系统销毁。未登录时，访问不需要登录的平台接口，仍然需要传入本参数，参数值可为空或任意值（不超过30字符）|
|sign	|String|	Header|	必填|对请求进行签名运算产生的签名,签名算法见附录。|
|timestamp	|String	|Header	|必填|	long型时间戳,精确到毫秒，该参数为多国家地区提供支持。应传入用户所在地时间戳。|
|language	|String	|Header|	必填	|该参数为多语言版提供支持。默认填写zh-cn即可。|
|timezone	|String|	Header|	必填	|代表客户端使用的时区。传入用户所在时区ID，默认填写"Asia/Shanghai"即可。具体参照国际时区ID列表。|
|Content-Type|String|	Header|	必填	|该参数不同的服务会有所不同，一般为"application/json;charset=UTF-8" 具体参照媒体类型|


**输出参数**

|参数名|	类型|位置|是否返回|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	是	|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）|
|retInfo	|String	|Body	|是	|返回信息（用于调试的返回信息，不支持国际化，也不能直接显示在UI上）|


## 用户隐私权限

为切实保护用户隐私权，优化用户体验，海尔优家根据现行法规及政策，制定了海尔家电隐私权政策。海尔了解个人信息对客户的重要性，我们力求明确说明我们获取、管理及保护用户个人信息的政策及措施。

在用户注册、下载更新、登录访问等情况下必须提供隐私权政策的内容或指向所在页面，且需要用户点击表示“同意”隐私权政策，不能太过隐蔽、不能设置默认“同意”；
在获得用户“同意”之后，也要确保用户在使用的过程中可以随时便利查看到隐私权政策全文，不能隐藏起来不展示。

 **开发者需提供使用海尔优家账号服务应用的用户服务协议条款给到[海尔优家商务BD][Business]，我们为应用配置对应的隐私权政策及服务协议条款**


##  媒体类型
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


## 数据类型

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

## 签名算法

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
		body = body.replaceAll("", "");
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
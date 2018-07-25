## 平台介绍

### 海尔优家 {docsify-ignore}
?> U+智慧生活平台（简称U+平台），是海尔旗下全球首个智慧家庭领域全开放、全兼容、全交互的智慧生活平台。

U+平台以U+物联平台、U+大数据平台、U+交互平台、U+生态平台为基础，以引领物联网时代智慧家庭为目标，
以用户社群为中心，通过自然的人机交互和分布式场景网器，搭建U+智慧生活平台的物联云和云脑，为行业提供物联网时代智慧家庭全场景生态解决方案，实现智能全场景，共赢新生态；为用户提供厨房美食、卫浴洗护、起居、安防、娱乐等家庭生态体验。 
 
-网站: [海尔优家][haieruplus]  
-网站: [加入我们][joinhaieruplus]  

### 海极网 {docsify-ignore}
?> 针对硬件开发者、APP开发者、服务开发者、内容开发者，提供不同的自助开发服务，配套开发资源、测试工具、调试环境，满足不同开发者的开发需求。
   
-网站: [海极网 开发者社区][haigeek]

### U+ IOT物联网平台 {docsify-ignore}

?> U+互联互通具备安全可靠的物联网能力，其全互联、快连接、高安全、专业云的行业引领优势，实现了设备、云端和APP的快速连接，帮助行业用户打造更具吸引力的智能家电产品，实现全场景多设备联动的极致体验。

U+互联互通技术能力具有快连接、全互联、高安全、专业云四大特点，从硬件技术到软件服务，为行业提供安全、开放、生态、定制的物联云解决方案。    
![图片2](http://resource.haigeek.com/haierupfile/views/pc/img/cloud_qh2.png)


## UWS API 调用说明

UWS ( U+ WEB Servcie)

### 接入说明

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
在开发、联调时，应用开发者应该连接联调环境或开发者环境，通过配置应用开发者访问外网的路由器（设置路由器的dns），可以连接到不同的开发环境

|接口分类|	生产环境接入地址	|开发者环境接入地址	|
|---|---|---|---|
|UWS接口	|https://uws.haier.net|	https://dev-uws.haigeek.com	|


###  公共字段
公共字段说明了每个请求、应答应包含的公共字段。这些字段无特殊情况，不在业务类接口报文定义中重复说明。

**输入参数**  

|参数名|	类型|位置|是否必填|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|appId|	String|	Header	|必填	|应用ID40位以内字符,Haier uHome 云平台全局唯一。开发者通过海极网申请获得。|
|appVersion	|String	|Header	|必填	|应用版本32 位字符,Haier uHome 云平台全局唯一。|
|clientId	|String	|Header	|必填	|客户端ID27 位字符,客户端机编码与客户端 MAC 地址 拼合成唯一的客户端标识。 主要用途为唯一标识客户端 (例如,手机)。手机机编码为 IMEI 码。 手机 MAC 为 12 位地址。命名规范:客户端机编码(15 位)-客户 端 MAC 地址(12 位)格式: XXXXXXXXXXXXXXX-XXXXXXXXXXXX 举例: 356877020056553-08002700DC94 |
|sequenceId	|String	|Header|必填	|报文流水(客户端唯一)客户端交易流水号。20 位, 前 14 位时间戳（格式：yyyyMMddHHmmss）,后 6 位流水 号。交易发生时,根据交易 笔数自增量。App应用访问uws接口时必须确保每次请求唯一，不能重复。|
|accessToken	|String	|Header|必填|（登录后不为空，登录前可为空）	请求令牌（用户登陆后）安全令牌 token。30 位字符。 用户登录 Haier uHome 云平 台,由系统创建。用户退出 Haier uHome 云平 台,由系统销毁。|
|sign	|String|	Header|	必填|（登录后不为空，登录前可为空）	详见签名认证章节|
|timestamp	|String	|Header	|必填|	long型时间戳,精确到毫秒，该参数为多国家地区提供支持。应传入用户所在地时间戳。|
|language	|String	|Header|	必填	|该参数为多语言版提供支持。默认填写zh-cn即可。|
|timezone	|String|	Header|	必填	|时区， -11 至 13。传入用户所在时区，默认填写8即可。|

**输出参数**

|参数名|	类型|位置|是否返回|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	是	|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）|
|retInfo	|String	|Body	|是	|返回信息（用于调试的返回信息，不支持国际化，也不能直接显示在UI上）|

### 签名认证
> 调用方需要对发送到uws的请求进行签名，签名后赋值到Header头中的sign属性（见公共部分说明），以便服务端进行签名验证。

1. **参数介绍**  

|参数名|	说明|
|:-----|-----|
|待签名字符串为|url字符串 + Body字符串+appId+appKey +timestamp；|
|url 字符串|指请求的接口地址去除https://uws.haier.net 后剩余的路径部分；|
|Body字符串|指应用发送请求的Body部分去除所有空白字符后的JSON字符串，没有body时为空字符串（不是null）。|
|appId|Header头中的属性（见公共部分说明）；|
|appKey|在海极网给应用申请的appKey，不能明文发送；|
|timestamp|Header头中的属性（见公共部分说明）；|

2. **签名算法**
签名算法就是对待签名字符串计算32位小写SHA-256值,详见如下示例；

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

### 国际化处理
对接口响应返回的retCode和retInfo不做国际化处理，由接口调用方处理。
对于接口涉及业务数据的国际化通过在header中传递language参数来定义，具体的国际化语言代码见下。

|语言编码|	英文名称|	中文名称|	是否支持|
|-----|----|----|----|
|af	|Afrikaans - South Africa	|南非荷兰语|	否|
|ar-ae	|Arabic(U.A.E.)	|阿拉伯语 - 阿拉伯联合酋长国	|否|
|ar-bh	|Arabic(Bahrain)	|阿拉伯语 - 巴林	|否|
|ar-dz	|Arabic(Algeria)	|阿拉伯语 - 阿尔及利亚	|否|
|ar-eg	|Arabic(Egypt)	|阿拉伯语 - 埃及	|否|
|ar-iq	|Arabic(Iraq)	|阿拉伯语 - 伊拉克	|否|
|ar-jo	|Arabic(Jordan)	|阿拉伯语 - 约旦	|否|
|ar-kw	|Arabic(Kuwait)	|阿拉伯语 - 科威特	|否|
|ar-lb	|Arabic(Lebanon)	|阿拉伯语 - 黎巴嫩	|否|
|ar-ly	|Arabic(Libya)	|阿拉伯语 - 利比亚	|否|
|ar-ma	|Arabic(Morocco)	|阿拉伯语 - 摩洛哥	|否|
|ar-om	|Arabic(Oman)	|阿拉伯语 - 阿曼	|否|
|ar-qa	|Arabic(Qatar)	|阿拉伯语 - 卡塔尔	|否|
|ar-sa	|Arabic(Saudi Arabia)	|阿拉伯语 - 沙特阿拉伯	|否|
|ar-sy	|Arabic(Syria)	|阿拉伯语 - 叙利亚	|否|
|ar-tn	|Arabic(Tunisia)	|阿拉伯语 - 突尼斯	|否|
|ar-ye	|Arabic(Yemen)	|阿拉伯语 - 也门	|否|
|be	|Belarusian	|白俄罗斯语	|否|
|bg	|Bulgarian	|保加利亚语	|否|
|ca	|Catalan	|加泰罗尼亚语	|否|
|cs|	Czech	|捷克语|	否|
|da	|Danish	|丹麦语	|否|
|de	|German(Standard)	|德语 - 标准|	否|
|de-at	|German(Austrian)	|德语 - 奥地利	|否|
|de-ch	|German(Swiss)	|德语 - 瑞士|	否|
|de-li	|German(Liechtenstein)	|德语 - 列支敦士登|	否|
|de-lu	|German(Luxembourg)	|德语 - 卢森堡	|否|
|el	|Greek	|希腊语	|否|
|en	|English	|英语|	是|
|en-au	|English(Australian)	|英语 - 澳大利亚	|否|
|en-bz	|English(Belize)	|英语 - 伯利兹	|否|
|en-ca	|English(Canadian)	|英语 - 加拿大	|否|
|en-gb	|English(British)	|英语 - 英国	|否|
|en-ie	|English(Ireland)	|英语 - 爱尔兰|	否|
|en-jm	|English(Jamaica)	|英语 - 牙买加	|否|
|en-nz	|English(New Zealand)	|英语 - 新西兰	|否|
|en-tt	|English(Trinidad)	|英语 - 特立尼达岛	|否|
|en-us	|English(United States)	|英语 - 美国	|否|
|en-za	|English(South Africa)	|英语 - 南非	|否|
|es	|Spanish(Spain - Modern Sort)	|西班牙语 - 标准|	否|

### 数据类型限定

1. **字段类型说明**

|限定类型|	说明|	格式|	json示例|
|---|---|---|---|
|DateTime	|日期时间类型的字符串|	`yyyy-MM-dd hh:mm:ss`| `	{“lgTime”:“2013-10-08 08:00:00”}`|
|Date	|日期类型的字符串	|yyyy-MM-dd 	|`{“lgDate”:“2013-10-08”}`|
|String	|字符串		||`{“address”:“street 123”}`|
|int	|整形数字	||	`{“age”:1234}`|
|long	|长整形数字	||	`{“oid”:1234567890123}`|
|double	|浮点数数字	||	`{“price”:12.35}`|
|boolean	|布尔型（true或false）	||	`{“idOld”:true}`|

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

### 公共错误码

[haigeek]:http://www.haigeek.com
[haieruplus]:http://www.haieruplus.com
[joinhaieruplus]:http://www.haieruplus.com/zhaopinlist.htm


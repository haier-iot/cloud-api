
!> **当前版本：** [uws统一接入规范v1.3]()  
**更新时间**：{docsify-updated} 

## 接入说明 

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

|接口分类|	生产环境接入地址	|开发者环境接入地址	|开发者环境DNS配置|
|---|---|---|---|
|UWS接口	|https://uws.haier.net|	https://dev-uws.haigeek.com	|221.122.92.13|

## UWS服务说明
U+平台现有的UWS服务说明

序号|UWS服务名|应用名|版本号（当前版本）
:-:|:-:|:-:|:-:  
1|[账户服务](zh-cn/Account)|uam|v1.0.0
2|[设备管理服务标准版](zh-cn/DevicesStandard)|uds|v2.0.1
3|[设备管理服务企业版](zh-cn/DevicesEnterprise)|udse|v1.5.1  
4|[数据订阅](zh-cn/DataSubscription)|无|v1.0.0
5|[家庭模型](zh-cn/Family)|ufm|v1.5.1  
6|[场景引擎](zh-cn/IFTTT)|iftttscene|v3.0.11  
7|[预约定时](zh-cn/Scheduler)|scheduler|v1.0.0  
8|[设备影子](zh-cn/DevicesShadow)|shadow|暂未开放
9|[消息推送](zh-cn/MessagePush)|ums|v3.0.3
10|[天气服务](zh-cn/CapacityService_Weather)|暂未开放|暂未开发 
11|[智能设备资源云存储](zh-cn/CapacityService_DeviceCloudStorage)|css|v1.0.0   



##  公共字段
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


## 数据类型限定

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

## 签名认证

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

## 国际化处理
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

## 安全规范

1. **数据安全**

App应用要保证自己接入UWS时通过海极网获取的appid、appkey等信息的使用安全，出现因外泄导致违规使用UWS服务而被平台禁用的风险由APP开发者自行承担。
App通过UWS获取的数据等要确保存储安全。

2. **接口访问安全**

要严格按照UWS接口服务的使用文档调用,开发相关安全参照“海尔优家安全开发规范V1.0.pdf”执行。


## 常见HTTP状态码解说

状态码|含义|解决思路
:-:|:-|:-
200|（成功）</br>请求已成功，请求所希望的响应头或数据体将随此响应返回|
400|（错误请求）</br>1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。</br>2、请求参数有误。|对照接口文档，检查请求参数、方法等是否正确
401|（身份验证错误）</br>请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。|对照接口文档，检查请求参数、方法等是否正确
403|（禁止）</br>服务器已经理解请求，但是拒绝执行它。与401响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。|对照接口文档，检查请求参数、方法等是否正确
404|（未找到）</br>请求失败，请求所希望得到的资源未被在服务器上发现。|对照接口文档，检查请求参数、方法等是否正确
500|（服务器内部错误）</br>服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现。|排查对应的服务端出错情况或异常
501|（尚未实施）</br>服务器不具备完成请求的功能。例如，当服务器无法识别请求方法时，服务器可能会返回此代码。 |排查对应的服务端出错情况或异常
502|（错误网关)</br>服务器作为网关或代理，从上游服务器收到了无效的响应。 |排查对应的服务端出错情况或异常
503|（服务不可用）</br>由于临时的服务器维护或者过载，服务器当前无法处理请求。|排查对应的服务端出错情况或异常
504|（网关超时）</br>作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI标识出的服务器，例如HTTP、FTP、LDAP）或者辅助服务器（例如DNS）收到响应。|排查对应的服务端出错情况或异常


## 公共错误码
|retCode错误码|	retInfo描述|
|---|---|---|---|
|A00001|	服务不可用                                    |
|A00002|	网络异常                                      |
|A00003|	访问或操作超时                                |
|A00004|	系统内部错误                                  |
|A00005|	数据库访问异常                                |
|A00006|	未知异常                                      |
|A00007|	邮件服务异常                                  |
|A00008|	邮件发送失败                                  |
|A00009|	邮件发送次数超限                              |
|B00001|	缺少必填参数                                  |
|B00002|	参数类型错误                                  |
|B00003|	参数数值超出值域或不是枚举值                  |
|B00004|	参数不符合规则要求                            |
|B00006|	参数长度错误                                  |
|B00007|	参数与接口定义不匹配                          |
|C00001|	appId与appKey验证失败                         |
|C00002|	appServer无访问授权                           |
|C00003|	访问权限不足                                  |
|C00004|	操作权限不足                                  |
|C00005|	重复请求                                      |
|C00006|	未知设备类型                                  |
|C00007|	appId配置信息为空                             |
|C00008|	appKey为空                                    |
|D00001|	sign签名错误                                  |
|D00002|	账号或密码错误                                |
|D00003|	Token不存在，未通过token验证                  |
|D00004|	Token已过期，未通过token验证。                |
|D00005|	Token不是由此应用创建，未通过token验证。      |
|D00006|	会话失效                                      |
|D00007|	不是系统内部用户                              |
|D00008|	用户不合法                                    |
|D00009|	登录失败超限，需使用验证码登录                |
|D00010|	账号被锁定                                    |
|D00011|	帐号未激活                                    |
|D00012|	账号已存在                                    |
|D00013|	登录失败                                      |
|D00015|	验证码错误                                    |
|D00016|	你已退出或尚未登录                            |
|D00017|	账户不存在                                    |
|E31104|	用户不是家庭管理员                            |
|E31105|	用户已经是家庭成员                            |
|E31106|	成员记录不存在                                |
|E31107|	用户不是家庭成员                              |
|E31108|	家庭id不存在                                  |
|E31109|	用户不是家庭管理员或者家庭成员                |
|E31111|	设备不能分享给多个家庭或者不能重复分享        |
|E31129|	家庭成员不是资源拥有者                        |
|E31130|	家庭管理员不能邀请自己                        |
|E31131|	邀请码不能为空                                |
|E31132|	用户没有被邀请                                |
|E31133|	邀请码错误                                    |
|E31134|	两个资源id不能相同                            |
|F31211|	用户不是家庭的创建者                          |
|F31212|	用户已经是家庭成员                            |
|F31213|	家庭的创建者不能加入家庭                      |
|F31214|	用户不是家庭成员                              |
|F31215|	用户不是设备的拥有者                          |
|F31216|	找不到对应的家庭                              |
|F31217|	设备没有被分享到家庭                          |
|F31218|	用户没有对该设备的任何权限                    |
|F31219|	设备已分享到该家庭，不要重复分享              |
|F31221|	设备已分享给该用户                            |
|F31222|	设备没有分享给该用户                          |
|F31223|	设备已绑定给该用户                            |
|F31224|	设备已绑定给其它用户                          |
|F31225|	设备不能分享给自己                            |
|F31301|	查询不到用户信息                              |
|G20003|	无法获取到适配器/应用信息                     |
|G20201|	获取用户绑定的设备列表失败                    |
|G20202|	当前用户与该设备不匹配                        |
|G20301|	设备信息查询失败                              |
|20903	|主数据访问异常                                   |
|G20904|	已超过绑定数量                                |
|G20908|	安全校验未通过                                |
|G20910|	非安全设备校验失败                            |
|G23401|	设备未绑定                                    |
|G23501|	用户绑定设备安全校验失败                      |
|G24001|	设备品牌型号信息为空                          |
|H00001|	邀请消息推送失败，请重新发送邀请              |
|H32004|	第三方通道暂不支持                            |
|H32005|	APP类型暂不支持                               |
|H32006|	设备重复注销或尚未注册                        |
|H32007|	本条消息不存在                                |
|H32008|	消息状态转换规则非法                          |
|H32009|	设备M2M加密信息验证失效                       |
|H32010|	查找M2M设备主人失败                           |
|H32011|	没有注册消息通道的用户                        |
|LC0001|	组件信息不存在                                |
|LS0001|	场景信息不存在                                |
|LI0001|	手动实例化规则参数不完整                      |
|LG0001|	组命令功能信息不存在                          |
|S00001|	CRON表达式冲突                                |
|S00002|	task记录不存在                                |
|S00003|	task操作失败                                  |
|S00004|	设备类型任务超出上限                          |
|Y00001|	设备云端影子未开启或已关闭。                  |
|Y00002|	历史设备，没有建立标准模型转换关系。          |
|Y00003|	历史设备，上报数据无法识别。转换标准模型失败。|


## 国际时区 TimeZone ID列表
|时区 TimeZone ID|
|---|
|Etc/GMT+12
|Etc/GMT+11
|Pacific/Midway
|Pacific/Niue
|Pacific/Pago_Pago
|Pacific/Samoa
|US/Samoa
|America/Adak
|America/Atka
|Etc/GMT+10
|HST
|Pacific/Honolulu
|Pacific/Johnston
|Pacific/Rarotonga
|Pacific/Tahiti
|SystemV/HST10
|US/Aleutian
|US/Hawaii
|Pacific/Marquesas
|AST
|America/Anchorage
|America/Juneau
|America/Nome
|America/Sitka
|America/Yakutat
|Etc/GMT+9
|Pacific/Gambier
|SystemV/YST9
|SystemV/YST9YDT
|US/Alaska
|America/Dawson
|America/Ensenada
|America/Los_Angeles
|America/Metlakatla
|America/Santa_Isabel
|America/Tijuana
|America/Vancouver
|America/Whitehorse
|Canada/Pacific
|Canada/Yukon
|Etc/GMT+8
|Mexico/BajaNorte
|PST
|PST8PDT
|Pacific/Pitcairn
|SystemV/PST8
|SystemV/PST8PDT
|US/Pacific
|US/Pacific-New
|America/Boise
|America/Cambridge_Bay
|America/Chihuahua
|America/Creston
|America/Dawson_Creek
|America/Denver
|America/Edmonton
|America/Hermosillo
|America/Inuvik
|America/Mazatlan
|America/Ojinaga
|America/Phoenix
|America/Shiprock
|America/Yellowknife
|Canada/Mountain
|Etc/GMT+7
|MST
|MST7MDT
|Mexico/BajaSur
|Navajo
|PNT
|SystemV/MST7
|SystemV/MST7MDT
|US/Arizona
|US/Mountain
|America/Bahia_Banderas
|America/Belize
|America/Chicago
|America/Costa_Rica
|America/El_Salvador
|America/Guatemala
|America/Indiana/Knox
|America/Indiana/Tell_City
|America/Knox_IN
|America/Managua
|America/Matamoros
|America/Menominee
|America/Merida
|America/Mexico_City
|America/Monterrey
|America/North_Dakota/Beulah
|America/North_Dakota/Center
|America/North_Dakota/New_Salem
|America/Rainy_River
|America/Rankin_Inlet
|America/Regina
|America/Resolute
|America/Swift_Current
|America/Tegucigalpa
|America/Winnipeg
|CST
|CST6CDT
|Canada/Central
|Canada/East-Saskatchewan
|Canada/Saskatchewan
|Etc/GMT+6
|Mexico/General
|Pacific/Galapagos
|SystemV/CST6
|SystemV/CST6CDT
|US/Central
|US/Indiana-Starke
|America/Atikokan
|America/Bogota
|America/Cancun
|America/Cayman
|America/Coral_Harbour
|America/Detroit
|America/Eirunepe
|America/Fort_Wayne
|America/Guayaquil
|America/Havana
|America/Indiana/Indianapolis
|America/Indiana/Marengo
|America/Indiana/Petersburg
|America/Indiana/Vevay
|America/Indiana/Vincennes
|America/Indiana/Winamac
|America/Indianapolis
|America/Iqaluit
|America/Jamaica
|America/Kentucky/Louisville
|America/Kentucky/Monticello
|America/Lima
|America/Louisville
|America/Montreal
|America/Nassau
|America/New_York
|America/Nipigon
|America/Panama
|America/Pangnirtung
|America/Port-au-Prince
|America/Porto_Acre
|America/Rio_Branco
|America/Thunder_Bay
|America/Toronto
|Brazil/Acre
|Canada/Eastern
|Cuba
|EST
|EST5EDT
|Etc/GMT+5
|IET
|Jamaica
|SystemV/EST5
|SystemV/EST5EDT
|US/East-Indiana
|US/Eastern
|US/Michigan
|America/Caracas
|America/Anguilla
|America/Antigua
|America/Aruba
|America/Asuncion
|America/Barbados
|America/Blanc-Sablon
|America/Boa_Vista
|America/Campo_Grande
|America/Cuiaba
|America/Curacao
|America/Dominica
|America/Glace_Bay
|America/Goose_Bay
|America/Grenada
|America/Guadeloupe
|America/Guyana
|America/Halifax
|America/Kralendijk
|America/La_Paz
|America/Lower_Princes
|America/Manaus
|America/Marigot
|America/Martinique
|America/Moncton
|America/Montserrat
|America/Port_of_Spain
|America/Porto_Velho
|America/Puerto_Rico
|America/Santo_Domingo
|America/St_Barthelemy
|America/St_Kitts
|America/St_Lucia
|America/St_Thomas
|America/St_Vincent
|America/Thule
|America/Tortola
|America/Virgin
|Atlantic/Bermuda
|Brazil/West
|Canada/Atlantic
|Etc/GMT+4
|PRT
|SystemV/AST4
|SystemV/AST4ADT
|America/St_Johns
|CNT
|Canada/Newfoundland
|AGT
|America/Araguaina
|America/Argentina/Buenos_Aires
|America/Argentina/Catamarca
|America/Argentina/ComodRivadavia
|America/Argentina/Cordoba
|America/Argentina/Jujuy
|America/Argentina/La_Rioja
|America/Argentina/Mendoza
|America/Argentina/Rio_Gallegos
|America/Argentina/Salta
|America/Argentina/San_Juan
|America/Argentina/San_Luis
|America/Argentina/Tucuman
|America/Argentina/Ushuaia
|America/Bahia
|America/Belem
|America/Buenos_Aires
|America/Catamarca
|America/Cayenne
|America/Cordoba
|America/Fortaleza
|America/Godthab
|America/Jujuy
|America/Maceio
|America/Mendoza
|America/Miquelon
|America/Montevideo
|America/Paramaribo
|America/Recife
|America/Rosario
|America/Santarem
|America/Sao_Paulo
|Antarctica/Rothera
|Atlantic/Stanley
|BET
|Brazil/East
|Etc/GMT+3
|America/Noronha
|Atlantic/South_Georgia
|Brazil/DeNoronha
|Etc/GMT+2
|America/Scoresbysund
|Atlantic/Azores
|Atlantic/Cape_Verde
|Etc/GMT+1
|Africa/Abidjan
|Africa/Accra
|Africa/Bamako
|Africa/Banjul
|Africa/Bissau
|Africa/Casablanca
|Africa/Conakry
|Africa/Dakar
|Africa/El_Aaiun
|Africa/Freetown
|Africa/Lome
|Africa/Monrovia
|Africa/Nouakchott
|Africa/Ouagadougou
|Africa/Sao_Tome
|Africa/Timbuktu
|America/Danmarkshavn
|Antarctica/Troll
|Atlantic/Canary
|Atlantic/Faeroe
|Atlantic/Faroe
|Atlantic/Madeira
|Atlantic/Reykjavik
|Atlantic/St_Helena
|Eire
|Etc/GMT
|Etc/GMT+0
|Etc/GMT-0
|Etc/GMT0
|Etc/Greenwich
|Etc/UCT
|Etc/UTC
|Etc/Universal
|Etc/Zulu
|Europe/Belfast
|Europe/Dublin
|Europe/Guernsey
|Europe/Isle_of_Man
|Europe/Jersey
|Europe/Lisbon
|Europe/London
|GB
|GB-Eire
|GMT
|GMT0
|Greenwich
|Iceland
|Portugal
|UCT
|UTC
|Universal
|WET
|Zulu
|Africa/Algiers
|Africa/Bangui
|Africa/Brazzaville
|Africa/Ceuta
|Africa/Douala
|Africa/Kinshasa
|Africa/Lagos
|Africa/Libreville
|Africa/Luanda
|Africa/Malabo
|Africa/Ndjamena
|Africa/Niamey
|Africa/Porto-Novo
|Africa/Tunis
|Africa/Windhoek
|Arctic/Longyearbyen
|Atlantic/Jan_Mayen
|CET
|ECT
|Etc/GMT-1
|Europe/Amsterdam
|Europe/Andorra
|Europe/Belgrade
|Europe/Berlin
|Europe/Bratislava
|Europe/Brussels
|Europe/Budapest
|Europe/Busingen
|Europe/Copenhagen
|Europe/Gibraltar
|Europe/Ljubljana
|Europe/Luxembourg
|Europe/Madrid
|Europe/Malta
|Europe/Monaco
|Europe/Oslo
|Europe/Paris
|Europe/Podgorica
|Europe/Prague
|Europe/Rome
|Europe/San_Marino
|Europe/Sarajevo
|Europe/Skopje
|Europe/Stockholm
|Europe/Tirane
|Europe/Vaduz
|Europe/Vatican
|Europe/Vienna
|Europe/Warsaw
|Europe/Zagreb
|Europe/Zurich
|MET
|Poland
|ART
|Africa/Blantyre
|Africa/Bujumbura
|Africa/Cairo
|Africa/Gaborone
|Africa/Harare
|Africa/Johannesburg
|Africa/Kigali
|Africa/Lubumbashi
|Africa/Lusaka
|Africa/Maputo
|Africa/Maseru
|Africa/Mbabane
|Africa/Tripoli
|Asia/Amman
|Asia/Beirut
|Asia/Damascus
|Asia/Gaza
|Asia/Hebron
|Asia/Istanbul
|Asia/Jerusalem
|Asia/Nicosia
|Asia/Tel_Aviv
|CAT
|EET
|Egypt
|Etc/GMT-2
|Europe/Athens
|Europe/Bucharest
|Europe/Chisinau
|Europe/Helsinki
|Europe/Istanbul
|Europe/Kaliningrad
|Europe/Kiev
|Europe/Mariehamn
|Europe/Nicosia
|Europe/Riga
|Europe/Sofia
|Europe/Tallinn
|Europe/Tiraspol
|Europe/Uzhgorod
|Europe/Vilnius
|Europe/Zaporozhye
|Israel
|Libya
|Turkey
|Africa/Addis_Ababa
|Africa/Asmara
|Africa/Asmera
|Africa/Dar_es_Salaam
|Africa/Djibouti
|Africa/Juba
|Africa/Kampala
|Africa/Khartoum
|Africa/Mogadishu
|Africa/Nairobi
|Antarctica/Syowa
|Asia/Aden
|Asia/Baghdad
|Asia/Bahrain
|Asia/Kuwait
|Asia/Qatar
|Asia/Riyadh
|EAT
|Etc/GMT-3
|Europe/Minsk
|Europe/Moscow
|Europe/Simferopol
|Europe/Volgograd
|Indian/Antananarivo
|Indian/Comoro
|Indian/Mayotte
|W-SU
|Asia/Riyadh87
|Asia/Riyadh88
|Asia/Riyadh89
|Mideast/Riyadh87
|Mideast/Riyadh88
|Mideast/Riyadh89
|Asia/Tehran
|Iran
|Asia/Baku
|Asia/Dubai
|Asia/Muscat
|Asia/Tbilisi
|Asia/Yerevan
|Etc/GMT-4
|Europe/Samara
|Indian/Mahe
|Indian/Mauritius
|Indian/Reunion
|NET
|Asia/Kabul
|Antarctica/Mawson
|Asia/Aqtau
|Asia/Aqtobe
|Asia/Ashgabat
|Asia/Ashkhabad
|Asia/Dushanbe
|Asia/Karachi
|Asia/Oral
|Asia/Samarkand
|Asia/Tashkent
|Asia/Yekaterinburg
|Etc/GMT-5
|Indian/Kerguelen
|Indian/Maldives
|PLT
|Asia/Calcutta
|Asia/Colombo
|Asia/Kolkata
|IST
|Asia/Kathmandu
|Asia/Katmandu
|Antarctica/Vostok
|Asia/Almaty
|Asia/Bishkek
|Asia/Dacca
|Asia/Dhaka
|Asia/Kashgar
|Asia/Novosibirsk
|Asia/Omsk
|Asia/Qyzylorda
|Asia/Thimbu
|Asia/Thimphu
|Asia/Urumqi
|BST
|Etc/GMT-6
|Indian/Chagos
|Asia/Rangoon
|Indian/Cocos
|Antarctica/Davis
|Asia/Bangkok
|Asia/Ho_Chi_Minh
|Asia/Hovd
|Asia/Jakarta
|Asia/Krasnoyarsk
|Asia/Novokuznetsk
|Asia/Phnom_Penh
|Asia/Pontianak
|Asia/Saigon
|Asia/Vientiane
|Etc/GMT-7
|Indian/Christmas
|VST
|Antarctica/Casey
|Asia/Brunei
|Asia/Chita
|Asia/Choibalsan
|Asia/Chongqing
|Asia/Chungking
|Asia/Harbin
|Asia/Hong_Kong
|Asia/Irkutsk
|Asia/Kuala_Lumpur
|Asia/Kuching
|Asia/Macao
|Asia/Macau
|Asia/Makassar
|Asia/Manila
|Asia/Shanghai
|Asia/Singapore
|Asia/Taipei
|Asia/Ujung_Pandang
|Asia/Ulaanbaatar
|Asia/Ulan_Bator
|Australia/Perth
|Australia/West
|CTT
|Etc/GMT-8
|Hongkong
|PRC
|Singapore
|Australia/Eucla
|Asia/Dili
|Asia/Jayapura
|Asia/Khandyga
|Asia/Pyongyang
|Asia/Seoul
|Asia/Tokyo
|Asia/Yakutsk
|Etc/GMT-9
|JST
|Japan
|Pacific/Palau
|ROK
|ACT
|Australia/Adelaide
|Australia/Broken_Hill
|Australia/Darwin
|Australia/North
|Australia/South
|Australia/Yancowinna
|AET
|Antarctica/DumontDUrville
|Asia/Magadan
|Asia/Sakhalin
|Asia/Ust-Nera
|Asia/Vladivostok
|Australia/ACT
|Australia/Brisbane
|Australia/Canberra
|Australia/Currie
|Australia/Hobart
|Australia/Lindeman
|Australia/Melbourne
|Australia/NSW
|Australia/Queensland
|Australia/Sydney
|Australia/Tasmania
|Australia/Victoria
|Etc/GMT-10
|Pacific/Chuuk
|Pacific/Guam
|Pacific/Port_Moresby
|Pacific/Saipan
|Pacific/Truk
|Pacific/Yap
|Australia/LHI
|Australia/Lord_Howe
|Antarctica/Macquarie
|Asia/Srednekolymsk
|Etc/GMT-11
|Pacific/Bougainville
|Pacific/Efate
|Pacific/Guadalcanal
|Pacific/Kosrae
|Pacific/Noumea
|Pacific/Pohnpei
|Pacific/Ponape
|SST
|Pacific/Norfolk
|Antarctica/McMurdo
|Antarctica/South_Pole
|Asia/Anadyr
|Asia/Kamchatka
|Etc/GMT-12
|Kwajalein
|NST
|NZ
|Pacific/Auckland
|Pacific/Fiji
|Pacific/Funafuti
|Pacific/Kwajalein
|Pacific/Majuro
|Pacific/Nauru
|Pacific/Tarawa
|Pacific/Wake
|Pacific/Wallis
|NZ-CHAT
|Pacific/Chatham
|Etc/GMT-13
|MIT
|Pacific/Apia
|Pacific/Enderbury
|Pacific/Fakaofo
|Pacific/Tongatapu
|Etc/GMT-14
|Pacific/Kiritimati
|Antarctica/Palmer
|America/Grand_Turk
|America/Santiago
|Pacific/Easter
|Chile/EasterIsland
|Chile/Continental

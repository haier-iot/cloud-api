## Platform Introduction

### Haier U+ {docsify-ignore}
?>U+ Smart Life Platform (U+ platform for short) is Haier's world's first smart home platform with full openness, full compatibility and full interaction.

Based on U+IoT platform, U+ big data platform, U+ interactive platform and U+ ecological platform, U+ platform aims to lead smart homes in the Internet of Things era, with user community as the center, through natural human-computer interaction and distributed scenarios. The network device, building the U+ smart life platform of the object cloud and cloud brain, to provide the industry with the IoT era smart home full scene ecological solution, to achieve intelligent full scene, win-win new ecology; provide users with kitchen food, bathroom care, Family, ecological experience such as living, security, and entertainment.  
 
-website: [Haier U+][haieruplus]  
-website: [join us][joinhaieruplus]  

### Haigeek {docsify-ignore}
?>For hardware developers, APP developers, service developers, content developers, provide different self-development services, supporting development resources, testing tools, debugging environment to meet the development needs of different developers.  
   
-website: [Haigeek Developer Community][haigeek]

### U+ Internet of Things platform {docsify-ignore}

?> U+ Interconnect has a secure and reliable IoT capability, and its full-interconnect, fast-connect, high-security, professional cloud industry leads the way, enabling fast connection of devices, cloud and APP, helping industry users to create more attractive smart appliances. The product realizes the ultimate experience of multi-device linkage in the whole scene.  

U+ interoperability technology has four characteristics: fast connection, full interconnection, high security and professional cloud. From hardware technology to software service, it provides a safe, open, ecological and customized IoT cloud solution for the industry.      



## UWS API Call description

UWS ( U+ WEB Servcie)

### Access instructions

1. **Access premise**  
Before using the UWS service of Haier U+ Cloud Platform, the application developer needs to register as a developer at Haigeek(http://www.haigeek.com)and apply to create their own application products (application types include Andriod, IOS). According to the development test of the application product, the stage of the release process corresponds to the UWS service of the Haier U+ cloud platform by using the information of the appid and appkey assigned by the created application.  

2. **Access method**  
The interactive interface between the cloud platform UWS and the application is unified into a JSON-based REST interface. The request parameter of the GET and DELETE primitives is the QueryParam of the url, and the URLEncode needs to be performed. The user of UWS should make the following assumptions:  
>   1.	The cloud platform will strongly check the parameters of the sending request. If there is any inconsistency with the interface specification, the interface call will be wrong.  
>   2.	When the cloud platform expands the interface function in the future, it may increase the number of parameters when the interface response returns. The interface consumer should be able to do this.  

3. **Access protocol**  
The externally provided services use the HTTPS protocol uniformly. By default, port 443 is used, and the server uses TSL for one-way encryption and decryption. The service caller does not need to download or install a certificate when making a call.  

4. **Access address**  
In development and joint debugging, the application developer should connect to the joint environment or the developer environment, and configure the application developer to access the external network router (set the router's dns) to connect to different development environments.  

|Interface classification|	European address	|North American address	|
|---|---|---|---|
|UWS interface	|https://uws-gea-euro.haieriot.net|	https://uws-gea-us.haieriot.net|  


###  Public field
The public field describes the public fields that should be included for each request and response. These fields have no special conditions and are not repeated in the service class interface message definition.  

**Input parameters**  

|parameter name|	types |location|required or not|description|  
|:-----:|:-----:|:-----:|:-----:|--|
|appId|	String|	Header	|yes	|With the ID 40 characters or less, the Haier uHome cloud platform is globally unique. Developers apply through the Haigeek.|
|appVersion	|String	|Header	|yes	|With the application version 32-bit characters, the Haier uHome cloud platform is globally unique.|
|clientId	|String	|Header	|yes	|The client ID is 27 characters, and the client code is combined with the client MAC address to form a unique client ID. The primary purpose is to uniquely identify the client (for example, a mobile phone). The mobile phone is coded as an IMEI code. The phone MAC is a 12-bit address. Naming convention: client machine code (15 bit) - client MAC address (12 bit) format: XXXXXXXXXXXXXXX-XXXXXXXXXXXX Example: 356877020056553-08002700DC94|
|sequenceId	|String	|Header|yes	|Message flow (client only) client transaction serial number. 20-bit, first 14-bit timestamp (format: yyyyMMddHHmmss), last 6-digit serial number. When a transaction occurs, it is incremented according to the number of transactions. App applications must ensure that each request is unique and cannot be repeated when accessing the uws interface.|
|accessToken	|String	|Header|yes|（Not vacant after login, can be empty before login) Request token (after user login) security token token. 30 characters. The user logs in to the Haier uHome cloud platform, which is created by the system. The user quits the Haier uHome cloud platform and is destroyed by the system.|
|sign	|String|	Header|	yes|(Not empty after login, can be empty before login) See the signature certification section for details.|
|timestamp	|String	|Header	|yes|Long timestamp, accurate to milliseconds, this parameter provides support for multi-country regions. The user's location timestamp should be passed in.|
|language	|String	|Header|	yes	|This parameter provides support for multi-language versions. By default, you can fill in zh-cn.|
|timezone	|String|	Header|	yes	|Time zone, -11 to 13. In the time zone of the incoming user, you can fill in 8 by default.|
|Content-Type|String|	Header|	yes	|Different parameters of this service will be different, generally "application/json; charset=UTF-8" specific reference media type|


**Output parameters**

|parameter name|	types|location|Whether to return|description|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	yes	|Return code (where 00000 means the request is successful, others represent errors, error codes and descriptions are listed in the Appendix Error Code Table)|
|retInfo	|String	|Body	|yes	|Return information (return information for debugging, does not support internationalization, and cannot be directly displayed on the UI)|


###  media type
The following three Content-Type media types are specified for requesting UWS:  

1.Content-Type: application/json
The interactive data interface between the UWS service and the application is unified into a JSON-based REST-RPC interface.  

2.Content-Type: multipart/form-data
Suitable for form file upload  

3.Content-Type:application/octet-stream
Applicable to file upload and download  




### Signature authentication
> The caller needs to sign the request sent to uws, and then assign it to the sign attribute in the header of the header (see the public section) for the server to perform signature verification.  

1. **Parameter introduction**  

|parameter name|	Description|
|:-----|-----|
|Signature string|Url string + Body string +appId+appKey +timestamp;|
|url |Refers to the path part of the requested interface address after removing https://uws.haier.net;|
|Body|Refers to the JSON string after the body part of the application sends the request to remove all whitespace characters. If there is no body, it is an empty string (not null);|
|appId|The attributes in the Header header (see the public section);|
|appKey|The appKey applied to the application on the Haigeek cannot be sent in clear text;|
|timestamp|The attributes in the Header header (see the public section);|

2. **Signature algorithm**
The signature algorithm is to calculate the 32-bit lowercase SHA-256 value for the signature string. See the following example for details:  

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

### Internationalization 
The retCode and retInfo returned by the interface response are not internationalized and are processed by the interface caller.  
The internationalization of the interface involving business data is defined by passing the language parameter in the header. The specific international language code is shown below.  

|Language coding|	English name|	Chinese name|	Whether to support|
|-----|----|----|----|
|af	|Afrikaans - South Africa	|南非荷兰语|	no|
|ar-ae	|Arabic(U.A.E.)	|阿拉伯语 - 阿拉伯联合酋长国	|no|
|ar-bh	|Arabic(Bahrain)	|阿拉伯语 - 巴林	|no|
|ar-dz	|Arabic(Algeria)	|阿拉伯语 - 阿尔及利亚	|no|
|ar-eg	|Arabic(Egypt)	|阿拉伯语 - 埃及	|no|
|ar-iq	|Arabic(Iraq)	|阿拉伯语 - 伊拉克	|no|
|ar-jo	|Arabic(Jordan)	|阿拉伯语 - 约旦	|no|
|ar-kw	|Arabic(Kuwait)	|阿拉伯语 - 科威特	|no|
|ar-lb	|Arabic(Lebanon)	|阿拉伯语 - 黎巴嫩	|no|
|ar-ly	|Arabic(Libya)	|阿拉伯语 - 利比亚	|no|
|ar-ma	|Arabic(Morocco)	|阿拉伯语 - 摩洛哥	|no|
|ar-om	|Arabic(Oman)	|阿拉伯语 - 阿曼	|no|
|ar-qa	|Arabic(Qatar)	|阿拉伯语 - 卡塔尔	|no|
|ar-sa	|Arabic(Saudi Arabia)	|阿拉伯语 - 沙特阿拉伯	|no|
|ar-sy	|Arabic(Syria)	|阿拉伯语 - 叙利亚	|no|
|ar-tn	|Arabic(Tunisia)	|阿拉伯语 - 突尼斯	|no|
|ar-ye	|Arabic(Yemen)	|阿拉伯语 - 也门	|no
|be	|Belarusian	|白俄罗斯语	|no|
|bg	|Bulgarian	|保加利亚语	|no|
|ca	|Catalan	|加泰罗尼亚语	|no|
|cs|	Czech	|捷克语|	no|
|da	|Danish	|丹麦语	|no|
|de	|German(Standard)	|德语 - 标准|	no|
|de-at	|German(Austrian)	|德语 - 奥地利	|no|
|de-ch	|German(Swiss)	|德语 - 瑞士|	no|
|de-li	|German(Liechtenstein)	|德语 - 列支敦士登|	no|
|de-lu	|German(Luxembourg)	|德语 - 卢森堡	|no|
|el	|Greek	|希腊语	|no|
|en	|English	|英语|	yes|
|en-au	|English(Australian)	|英语 - 澳大利亚	|no|
|en-bz	|English(Belize)	|英语 - 伯利兹	|no|
|en-ca	|English(Canadian)	|英语 - 加拿大	|no|
|en-gb	|English(British)	|英语 - 英国	|no|
|en-ie	|English(Ireland)	|英语 - 爱尔兰|	no|
|en-jm	|English(Jamaica)	|英语 - 牙买加	|no|
|en-nz	|English(New Zealand)	|英语 - 新西兰	|no|
|en-tt	|English(Trinidad)	|英语 - 特立尼达岛	|no|
|en-us	|English(United States)	|英语 - 美国	|no|
|en-za	|English(South Africa)	|英语 - 南非	|no|
|es	|Spanish(Spain - Modern Sort)	|西班牙语 - 标准|	no|

### Data type qualification

1. **Field type description**

|Limited type|	Description|	format|	Json example|
|---|---|---|---|
|DateTime	|Date time type string|	`yyyy-MM-dd hh:mm:ss`| `	{“lgTime”:“2013-10-08 08:00:00”}`|
|Date	|Date type string	|yyyy-MM-dd 	|`{“lgDate”:“2013-10-08”}`|
|String	|String		||`{“address”:“street 123”}`|
|int	|int	||	`{“age”:1234}`|
|long	|long	||	`{“oid”:1234567890123}`|
|double	|double	||	`{“price”:12.35}`|
|boolean	|boolean（true or false）	||	`{“idOld”:true}`|

2.**Null value description**  

To avoid parsing errors, the return parameters of each interface of uws do not return a null value.Required parameters, whether input or output, must have a value, cannot be null Non-required parameters are as follows:  
Numeric type data (int, long, double) only returns numbers, including positive numbers, zeros, and negative numbers.  
Boolean type data (boolean) only returns true and false.The above basic types do not themselves contain null values.   

DateTime, Date, String, and structure type data. If it is null, the corresponding attribute will not be returned.  

**DateTime type**  

Birthday is a DateTime type, not null:

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

When birthday is null, the birthday attribute is not returnd.  

    {"name":"Tom","age":23, "address":{"city":"beijing","street":"haidian"} }  

**String type**    

Name is a String type, not null:  

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

When name is null, the name attribute is not returned.

    {"age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

### Public error code
|retCode|	retInfo|
|---|---|---|---|
|A00001|	service is not available                                   |
|A00002|	Network exception                                      |
|A00003|	Access or operation timeout                                |
|A00004|	Internal System Error                                 |
|A00005|	Database access exception                                |
|A00006|	Unknown exception                                      |
|A00007|	Mail service exception                                  |
|A00008|	Mail failed to send                                  |
|A00009|	The number of emails sent exceeded                             |
|B00001|	Missing required parameters                                 |
|B00002|	Incorrect parameter type                                |
|B00003|	Parameter value is out of range or not enumerated                  |
|B00004|	The parameter does not meet the rule requirements                            |
|B00006|	Incorrect parameter length                                  |
|B00007|	The parameter does not match the interface definition                          |
|C00001|	appId and appKey validation failed                         |
|C00002|	appServer no access authorization                          |
|C00003|	Insufficient access                                 |
|C00004|	Insufficient authority                                 |
|C00005|	Repeat request                                     |
|C00006|	Unknown device type                                 |
|C00007|	appId configuration information is empty                             |
|C00008|	appKey is empty                                    |
|D00001|	Sign signature error                                 |
|D00002|	Incorrect username or password                               |
|D00003|	Token does not exist, not verified by token                |
|D00004|	Token has expired and has not been verified by token                |
|D00005|	Token is not created by this application, not verified by token      |
|D00006|	Session invalidation                                      |
|D00007|	Not an internal user                              |
|D00008|	User is not legal                                    |  


[haigeek]:http://www.haigeek.com
[haieruplus]:http://www.haieruplus.com
[joinhaieruplus]:http://www.haieruplus.com/zhaopinlist.htm


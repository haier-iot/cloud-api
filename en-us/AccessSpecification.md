## Access Instructions

1. **Access premise**  
Before using the UWS service of Haier U+ Cloud Platform, the application developer needs to register as a developer at Haigeek(http://www.haigeek.com) and apply to create their own application products (application types include Andriod, IOS). According to the development test of the application product, the stage of the release process corresponds to the UWS service of the Haier U+ cloud platform by using the information of the appid and appkey assigned by the created application. 

2. **Access method**  
The interactive interface between the cloud platform UWS and the application is unified into a JSON-based REST interface. The request parameter of the GET and DELETE primitives is the QueryParam of the url, and the URLEncode needs to be performed. The user of UWS should make the following assumptions:  
>   1.	The cloud platform will strongly check the parameters of the sending request. If there is any inconsistency with the interface specification, the interface call will be wrong.  
>   2.	When the cloud platform expands the interface function in the future, it may increase the number of parameters when the interface response returns. The interface consumer should be able to do this.  

3. **Access protocol**  
The externally provided services use the HTTPS protocol uniformly. By default, port 443 is used, and the server uses TSL for one-way encryption and decryption. The service caller does not need to download or install a certificate when making a call.  

4. **Access address**  
<!--
In development and joint debugging, application developers should connect to the production environment (currently only the production environment in the future), and application developers can build their own joint or developer environment as needed.  
-->  

|Interface classification|	European address	|North American address	|
|---|---|---|---|
|UWS interface	|https://uws-gea-euro.haieriot.net|	https://uws-gea-us.haieriot.net|  

## UWS Service Description
U+ platform existing UWS service description

Serial number|UWS service name|Application name|Version number (current version)
:-:|:-:|:-:|:-:
1|[Account Service-North American Environment](en-us/Account-NorthAmericanEnvironment)|uam|v1.4.0  
2|[Account Service-European Environment](en-us/Account-EuropeanEnvironment)|uam|v2.0.0
3|[Equipment Management Standard Edition](en-us/DevicesStandard)|uds|v1.5.2
4|[Equipment Management Enterprise Edition](en-us/DevicesEnterprise)|udse|v1.5.2
5|[Data Subscription](en-us/DataSubscription)|udp|v1.0.0


##  Public Field
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
|privacyVersion	|String|Header|	yes(Only for the European environment)|Latest Privacy Agreement Version|  
|timestamp	|String	|Header	|yes|Long timestamp, accurate to milliseconds, this parameter provides support for multi-country regions. The user's location timestamp should be passed in.|
|language	|String	|Header|	yes	|This parameter provides support for multi-language versions. By default, you can fill in zh-cn.|
|timezone	|String|	Header|	yes	|Time zone, -11 to 13. In the time zone of the incoming user, you can fill in 8 by default.|
|Content-Type|String|	Header|	yes	|Different parameters of this service will be different, generally "application/json; charset=UTF-8" specific reference media type|  


**Output parameters**

|parameter name|	types|location|Whether to return|description|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	yes	|Return code (where 00000 means the request is successful, others represent errors, error codes and descriptions are listed in the Appendix Error Code Table)|
|retInfo	|String	|Body	|yes	|Return information (return information for debugging, does not support internationalization, and cannot be directly displayed on the UI)|

##  Media Type
The following three Content-Type media types are specified for requesting UWS:  

1.Content-Type: application/json
The interactive data interface between the UWS service and the application is unified into a JSON-based REST-RPC interface.  
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
Suitable for form file upload  

3.Content-Type:application/octet-stream
Applicable to file upload and download 

## Data Type Qualification

1. **Field type description**

|Limited type|	Description|	format|	Json example|
|---|---|---|---|
|DateTime	|Date time type string|	`yyyy-MM-dd hh:mm:ss`| `	{“lgTime”:“2013-10-08 08:00:00”}`|
|Date	|Date type string	|`yyyy-MM-dd` 	|`{“lgDate”:“2013-10-08”}`|
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



**Structure type**

Address is a struct type, not null:
  
    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

When address is null, the address attribute does not return:

	{"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00" }

## Signature Authentication

1. **Description**


The caller needs to sign the request sent to uws, and then assign it to the sign attribute in the header of the header (see the public section) for the server to perform signature verification.

2. **Parameter introduction**

|parameter name|	Description|
|:-----|-----|
|Signature string|Url string + Body string +appId+appKey +timestamp;|
|url |Refers to the path part of the requested interface address after removing https://uws-gea-euro.haieriot.net or https://uws-gea-us.haieriot.net;|
|Body|Refers to the JSON string after the body part of the application sends the request to remove all whitespace characters. If there is no body, it is an empty string (not null);|
|appId|The attributes in the Header header (see the public section);|
|appKey|The appKey applied to the application on the Haigeek cannot be sent in clear text;|
|timestamp|The attributes in the Header header (see the public section);|



3. **Signature algorithm**
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



## Internationalization 
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

## Safety Regulations

1. **Data Security**

The App application should ensure the safe use of the appid, appkey and other information obtained through the Haiji network when accessing the UWS. The risk of being disabled by the platform due to the leakage of the UWS service due to the leakage is the responsibility of the APP developer.  
The data acquired by the App through UWS, etc., must ensure storage security.

2. **Interface access security**

It must be executed in strict accordance with the usage documentation of the UWS interface service.


## Common HTTP Status Code Commentary

Status Code|Meaning|Solutions
:-:|:-|:-
200|(success)</br>The request has been successful, and the response header or data body that the request expects will be returned with this response.|
400|（Wrong request）</br>1. The semantics are incorrect and the current request cannot be understood by the server. The client should not submit this request repeatedly unless it is modified. </br>2, the request parameters are incorrect.|Check the request parameters, methods, etc. correctly against the interface documentation.
401|(authentication error)</br> The request requires authentication. The server may return this response for web pages that require login.|Check the request parameters, methods, etc. correctly against the interface documentation.
403|(prohibited)</br> The server has understood the request but refused to execute it. Unlike the 401 response, authentication does not provide any help, and this request should not be submitted repeatedly.|Check the request parameters, methods, etc. correctly against the interface documentation.
404|(Not found)</br> The request failed, and the requested resource was not found on the server.|Check the request parameters, methods, etc. correctly against the interface documentation.
500|(Intra-server error)</br> The server encountered an unexpected condition that prevented it from completing the processing of the request. In general, this problem will occur when the server's code is wrong.|Troubleshoot the corresponding server error or exception
501|(Not yet implemented)</br> The server does not have the ability to complete the request. For example, the server might return this code when the server does not recognize the request method. |Troubleshoot the corresponding server error or exception
502|(Error Gateway)</br> The server received an invalid response from the upstream server as a gateway or proxy. |Troubleshoot the corresponding server error or exception
503|(Service not available)</br> The server is currently unable to process the request due to temporary server maintenance or overload.|Troubleshoot the corresponding server error or exception
504|(Gateway timeout)</br> When a server working as a gateway or proxy attempts to execute a request, it fails to receive a response from the upstream server (the server identified by the URI, such as HTTP, FTP, LDAP) or the secondary server (for example, DNS).|Troubleshoot the corresponding server error or exception


## Public error code
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
|D00009|	Login Failure Number More than Limit, Need Verification Code|  
|D00010|	Account Locked|  
|D00011|	Account Not Activated|  
|D00012|	Account Already Excit|  
|D00015|	Wrong Verification Code| 
|D00016|Already Logout or Havn't Login|  
|D00017	|Account Not Excit|  
|G20202	|User Doesn't match the Device|  
|G20904|	Device Has Been bound|  
|G20908|	Equipment out of Safety Period  

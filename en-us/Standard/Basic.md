**Update time**：{docsify-updated} 

## Access address

1. **Access premise**  
Before using the UWS service of Haier U+ Cloud Platform, the application developer needs to register as a developer at Haigeek(http://www.haigeek.com) and apply to create their own application products (application types include Andriod, IOS). According to the development test of the application product, the stage of the release process corresponds to the UWS service of the Haier U+ cloud platform by using the information of the appid and appkey assigned by the created application. 

2. **Access method**  
The interactive interface between the cloud platform UWS and the application is unified into a JSON-based REST interface. The request parameter of the GET and DELETE primitives is the QueryParam of the url, and the URLEncode needs to be performed. The user of UWS should make the following assumptions:  
>   1.	The cloud platform will strongly check the parameters of the sending request. If there is any inconsistency with the interface specification, the interface call will be wrong.  
>   2.	When the cloud platform expands the interface function in the future, it may increase the number of parameters when the interface response returns. The interface consumer should be able to do this.  

3. **Access protocol**  
The externally provided services use the HTTPS protocol uniformly. By default, port 443 is used, and the server uses TSL for one-way encryption and decryption. The service caller does not need to download or install a certificate when making a call.  

4. **Access address** 
When developing applications, please connect to the developer environment for development and testing;

|Connection area|Production environment address|Production environment address|
|---|---|---|
|China|`https://uws.haier.net`|	`https://dev-uws.haigeek.com`	|
|Europe|`https://uws-euro.haieriot.net`|No|
|North America|`https://uws-gea-us.haieriot.net`|No|

## Service list
U+ platform existing UWS service description

Serial number|UWS service name|Application name|Version number (current version)
:-:|:-:|:-:|:-:
1|[Account Service-NorthAmericanEnvironment](en-us/Account-NorthAmericanEnvironment)|uam|v1.4.0  
2|[Account Service-EuropeanEnvironment](en-us/Account-EuropeanEnvironment)|uam|v2.1.0
3|[Equipment Management Standard Edition-NorthAmericanEnvironment](en-us/DevicesStandard)|uds|v1.4.0
4|[Equipment Management Enterprise Edition-NorthAmericanEnvironment](en-us/DevicesEnterprise)|udse|v1.5.2
5|[Equipment Management Standard Edition-EuropeanEnvironment](en-us/DevicesStandard-EuropeanEnvironment)|uds|v2.0.2
6|[Equipment Management Enterprise Edition-EuropeanEnvironment](en-us/DevicesEnterprise-EuropeanEnvironment)|udse|v1.5.2
7|[Data Subscription](en-us/DataSubscription)|udp|v1.0.0


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
|timezone	|String|	Header|	yes	|Represents the time zone used by the client. Pass in the user's time zone ID, referring to the list of [international time zone ids](/Other).|
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
|url |Refers to the path part of the requested interface address after removing `https://uws-euro.haieriot.net` or `https://uws-gea-us.haieriot.net`;|
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



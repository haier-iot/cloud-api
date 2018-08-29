!>  **current version**：[UWS Accountservice V1.4.0][account_document_url]  
 **release time**：2018-07-19  


### Introduction

> The account service is designed to provide access control services covering the entire process of the IoT, and to build a unified user login system for developers.   

By integrating the U+IOT platform account service, the developer not only provides account management services such as registration, login, and password recovery of user accounts, but also helps developers to build unified control including device and user rights management. Consistent IOT systemic control mode.  

![账户图片][account_type]

### Noun explanation

<!--
-  **Haier OAuth authorization**
>  Refers to the unified account authorization provided by Haier Group User Center. It can use this kind of user authorization to log in to Haier Youjia IOT platform to obtain user equipment related rights. At the same time, it has the right to log in to other related business services of Haier Group, such as Haier Mall and Haier Community. 

- **Haier U+ Account**
>  It refers to the self-owned IoT account system provided by Haier Youjia Platform of Haier Group. The account system has the authority to bind/control Haier IoT appliances.

If you use Haier OAuth to authorize login, according to Haier OAuth access requirements, you can automatically obtain Haier Youjia account information at the same time; that is, you can use Haier account to obtain Haier Youjia account at the same time.  
-->
- **Haier U+ OAuth**
> Refers to the OAuth service provided by Haier Youjia, which requires the use of Haier Youjia account for login authorization.  

Since Haier account has Haier Youjia account right at the same time, Gu can also use Haier account to log in under this kind of authorization service;Haier account and Haier Youjia account one-way interoperability, with Haier excellent home OAuth authority does not mean that Haier Group's business authority.  

<!--
- **Haier U+ Third Party Login**
> Refers to the use of third-party platform accounts to log in Haier Youjia platform, such as WeChat, Jingdong, Taobao and so on.  

Third-party social accounts support QQ, WeChat, Weibo, Douban, Renren.com account login. If there are other third-party platform account login requirements, online feedback is available.
-->
- **Haier U+  Developer Account Login**
> It means that the developer has an account system and wants to use the own account system to log in to the Haier Youjia platform. 

Haier Youjia provides inter-platform account docking solution, with standard OAuth scheme and application front-end scheme. This kind of docking method requires offline application process. If there is demand, it can be feedback in the developer community, or through Haier Youjia Business BD feedback.  

### Function is introduced  
**Account base capability**  
1. IOT platform account registration: Users can use this interface to register an IOT account with a mobile phone or email, and call the verification code interface to obtain a verification code for registration activation.    
2. The IOT platform account login and logout, login authentication to obtain the security token (accessToken) created by the system, and the system verifies the accessToken for the user to log out.    
3. IOT account verification code application and verification. Use this interface to apply for and verify the verification code of the mobile phone or mailbox to ensure the security of registration and login.  
**Account system association ability**  
1. Third-party social account login, support QQ, WeChat, Weibo, Douban, Renren account login.   
2. The developer's own account login, generate the corresponding dark account on the U+IOT platform and authorize the user to log in to the U+ platform as the U+ account. The developer can establish its own independent developer account system.  

### Security of user password  

#### Safety instructions  
Since openapi USES md5+salt password processing mode, if the uws-uam interface also USES md5 mode, the app needs to transmit the password in clear text, which will reduce the security of the entire original gea environment, so the app transmission password must be encrypted. The login is encrypted as sha256, and the password is encrypted as sha256 when registering. Aes is used for encryption and base64 encoding.  
#### Password flow  
![密码传输流程图片][account_PasswordFlow]
#### Cipher encryption and decryption algorithm  
algorithm:aes
secret key:App encryption and server decryption use the same secret key
algorithm code:  

```java
import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;

public class AESUtil {
	private final static String CIPHER_ALGORITHM = "AES";
	private final static String CIPHER_ALGORITHM_FULL = "AES/CBC/PKCS5Padding";
	private final static String VIPARA = "1269571569321021";
	public static String encryptContent(String secret, String content) throws Exception {
	    IvParameterSpec zeroIv = new IvParameterSpec(VIPARA.getBytes());  
	    SecretKeySpec key = new SecretKeySpec(secret.getBytes(), CIPHER_ALGORITHM);  
	    Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM_FULL);  
        cipher.init(Cipher.ENCRYPT_MODE, key, zeroIv);  
		byte[] byteContent =content.getBytes("UTF-8");
		byte[] result = cipher.doFinal(byteContent);
		return new String(Base64Util.encode(result));
	}
	public static String decryptContent(String secret, String content) throws Exception {
	    byte[] byteMi = Base64Util.decode(content); 
        IvParameterSpec zeroIv = new IvParameterSpec(VIPARA.getBytes());  
        SecretKeySpec key = new SecretKeySpec(secret.getBytes(), CIPHER_ALGORITHM);  
        Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM_FULL);  
        cipher.init(Cipher.DECRYPT_MODE, key, zeroIv);  
        byte[] decryptedData = cipher.doFinal(byteMi);  
        return new String(decryptedData, "UTF-8");   
	}
}

```  

### Public structure  

#### no  
<!--
#### UserProfile  
User extended attributes. A key-value pair object with an unfixed attribute is structured as follows:   
{  
	"key1":"value1",  
	"key2":"value2",  
	"key3":"value3",  
	 …  
	"keyn":"valuen",  
}  
It is used to meet the different needs of user information for different applications. When the application needs to expand the user attributes, you can apply to the cloud platform user system. When applying, specify the attributes that need to be extended, and specify the key, type, and length of each attribute.  
Here are the user extension properties for the Grill app:    
| **name** | User attribute | &emsp; |&emsp; | UserProfile |  
| ------------- |:-------------:|:-----:|:-------------:|  
|**field name**|**types**|**description**|**length**|**remarks**|  
|id|String|userID|20||  
|nickName|String|nickname|32||  
|userName|String|username|32||  
|avatar|String|User avatar resource id||&emsp; |  
|points|long|integral|8||  
|focusCount|int|number of followers|8||  
|followCount|int|number of fans|8|&emsp;|    
-->
## Interface list


### Haier U+ User class interface
> API interface overview

| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| User registration     | Register new user | yes|  no |  
| User login     | User login to get accessToken and openId | yes| no|  
| Resend activation email | After the registration is successful, but the user failed to receive the activation email due to the mail network, etc. | yes| no|  
| sign out   |Mobile APP users exit the Haier U+ cloud platform interface| yes| no|  
| Query user information | Obtain user information based on the registrant token | yes| no|  
| User information modification    | Modify the extended attribute of the currently logged in user according to the token of the logged in person |  yes| no|  
| Request a reset password     | When the user requests to reset the password, the user will send a link to reset the password in the user's mailbox, and the user clicks the link to reset the password. |  yes| no|    
| Get image verification code    | Get image verification code |  yes| no|    

#### User registration 
> Register new user    



##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/register`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types         | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| loginId     | String | Body| yes|Mailbox, need to match the mailbox format Use the following regular expression:^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$|  
| password     | String | Body| yes |Password: Length: 6 – 16 characters, ie a minimum of 6 digits, a maximum of 16 digits.|  
| captcha     | String | Body| yes |Verification code, a combination of 4 letters and numbers.|  
| userProfile     | Map | Body| no |Added to meet the different needs of user information for different applications.|  


**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:    
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "loginId": "14759167292@qq.com",
  "password": "Abcd!123456",
  "userProfile": {
    "updateTime": "20141115",
    "tel": "0596",
    "companyCode": 333,
    "address": "china",
    "email": "848421322@qq.com",
    "QQ": "848421322",
    "name": "test",
    "realname": "test"
  }
}



```  

**Request response**

```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、error code  
> D00012、D00015  
 

#### User login
> User login to get accessToken and openId.   
  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/login`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types        | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| loginId     | String | Body| yes|Mailbox, need to match the mailbox format Use the following regular expression:^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$|  
| password     | String | Body| yes|Password: Length: 6 – 16 characters, ie a minimum of 6 digits, a maximum of 16 digits. |  
| captcha     | String | Body| no |Verification code, a combination of 4 letters and numbers.|  


**Output parameters**  

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| accessToken   |   String |  Header   |  yes   |  Security token  |  
| userId   |   String |  Body   |  yes   | User id, user ID   |
| status   |   String |  Body   |  yes   | 0:activation   1:inactivation   |  



##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:  
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "loginId": "14759167292@qq.com",
  "password": " Abcd!123456"
}


```  

**Request response**

```java
Header：
accessToken:TGT13OOQL5O7TEAB21WVIKCJTEL470
body
{
  "retCode": "00000",
  "retInfo": "成功", 
  "userId ": "1234567",
"status":"0"
}


```

##### 3、error code  
> D00002、D00009、D00010、D00015  
 

#### Resend activation email
> After the registration is successful, but the user fails to receive the activation email due to the mail network, etc., and the user receives the activation email but does not perform the activation, and the activation email expires, the user can resend the activation through the interface. mail. The prerequisite for using this interface is that the user has already registered but is not activated. The activation time for the activation email is 120 minutes. Can be accessed without login.     
  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/sendActiveMail`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| loginId     | String | Body| yes|Mailbox, need to match the mailbox format Use the following regular expression:^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$|  
   
 


**Output Parameters**  

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "loginId": "14759167292@qq.com"
}

```  

**Request response**

```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、error code  
> D00011、D00017  
 

#### sign out
>Mobile APP users exit the Haier U+ cloud platform interface  

  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/logout`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|    |  | ||&emsp;|   
   
 


**Output parameters**  
**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT13OOQL5O7TEAB21WVIKCJTEL470 
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**Request response**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}


```

##### 3、error code  
> D00005、D00016


#### Query user information
>Obtain user information based on the registrant token.  


  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/get`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|  
|    |    |     |     |  &emsp;   |   



**Output parameters**  

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| userProfile     | Map | Body| no|User extension information, including nicknames, avatars, etc|  


##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT13OOQL5O7TEAB21WVIKCJTEL470 
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**Request response**

```java
{
  “retCode”: “00000”,
  “retInfo”: “正确”,
  “userProfile”: {
“nickName”: ,
“avatar”: ,
       “phone”: ,
       “updateTime”: “20141115”,
       “status”: null,
       “tel”: “0596”,
       “applyTime”: null,
       “idcard”: null,
       “companyName”: null,
       “type”: null,
       “postcode”: null,
       “legalPerson”: null,
       “contacts”: null,
       “companyCode”: 333,
       “businessLicense”: null,
       “address”: “china”,
       “contactsPhone”: null,
       “email”: “848421322@qq.com”,
       “QQ”: “848421322”,
       “name”: “test”,
       “realname”: “test”,
       “idcardPhoto”: null
  }
}

```

##### 3、error code  
> D00008 

#### User information modification
>Modify the extended attribute of the currently logged in user according to the token of the logged in person  
  

##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/update`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
| userProfile     | Map | Body| yes|User extension information, including nicknames, avatars, etc.|    


**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT13OOQL5O7TEAB21WVIKCJTEL470
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "userProfile": {
"nickName": ,
"avatar": ,
       "updateTime": "20141115",
       "tel": "0596",
       "companyCode": 333,
       "address": "china",
       "email": "848421322@qq.com",
       "QQ": "848421322",
       "name": "test",
       "realname": "test"
  }
}


```  

**Request response**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、error code  
> D00008    
    
#### Request a reset password
>When the user requests to reset the password, the user will send a link to reset the password in the user's mailbox, and the user clicks the link to reset the password.      
 

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/pwd/applyReset`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|   loginId   | String | body | yes | Mailbox, need to match the mailbox format Use the following regular expression:^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$|      


**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;  |  
 

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "loginId": "13679101193@qq.com"
}

```  

**Request response**

```java

{"retCode":"00000","retInfo":"操作成功"}

```

##### 3、error code    
> D00011、D00017  

#### Get image verification code
>Get image verification code.      
 

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/captcha`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|    |    |     |     |  &emsp;  |  
      


**Output parameters**  

文件流，image/png
response.setContentType("image/png");


##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**Request response**

```java

HTTP/1.1 200 OK
Date: Fri, 21 Nov 2008 01:57:21 GMT
Connection: close
Accept-Ranges: bytes
Pragma: No-cache
PATH=/; DOMAIN=.rd139.com;
Content-Type: image/png
Content-Length: 1381

```

##### 3、error code    
> See the home page public error code  



### Capability class interface
> API interface overview

| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| Get app version information     | Get app version information | yes|  no |  
| Upload resource file    | Upload resource files to the server | yes| no|   

 

#### Get app version information 
> Get app version information     



##### 1、Interface definition

?> **Access address：**  `/uas/v1/appVersion/getLatest`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types         | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|:---------:|
| appId     | String | Header| yes| AppId  |

**Output parameters**  

|   name      |     types      | location  |location |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  version |  String  |  Body  |  yes  |  Version number (format 2015110501) |  
|  versionName |  String  |  Body  |  yes  |  Version name, which can be returned as an empty string |  
|  description |  String  |  Body  |  yes  | Description, can return an empty string |  
|  resId |  String  |  Body  |  yes  |  The resource number or the url of the resource store can be returned as an empty string |  
|  status |  String  |  Body  |  yes  |  App status |  
|  force |  String  |  Body  |  yes  |  Whether to force |  

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:    
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
" appId ":"MB-UZHSH-0000"
}

```  

**Request response**

```java
{
  "retCode": "00000",
  "retInfo": "成功",
  "data": {
    "version": "20140911",
    "versionName": "V01.10.15.09101",
    "description": "V01.10.15.09101",
    "resId": "/uam/v1/resource/enabling/uzhsh/100013957366155388.jpg ",
    "status": 1,
    "force": "true"
  }
}

```

##### 3、error code  
> B00001、C00002、C00006、C00007、D00001  

 

#### Upload resource file 
> Upload the resource file to the server. (Note: To use this interface, you need to contact the capability administrator first, upload and authorize the APPId, and configure the file format and size of the uploaded resource. Otherwise, there is no error in returning the file configuration.)  
  


##### 1、Interface definition

?> **Access address：**  `/uas/v1/resource/uploadFile`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types        | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| file     | multipart/form-data | Body| yes|Uploaded file|  
| description     | String | Body| yes|PFile description, within 255 characters |  
| ownerType     | Integer | Body| yes |Owner type: 0: user, 1: device, 9: other|  


**Output parameters**  

|   name      |     pypes      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| resourceInfo  |   ResourceInfo |  Body   |  yes   |  Uploaded resource information  |  




##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-FRIDGEGENE1-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT1OY0RUUAH5D242SB68E9WX0W930  
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json
Body
{
"description":"Test file upload new interface",
"ownerType":0
}



```  

**Request response**

```java
{
"retCode": "00000",
"retInfo": "成功"，
"resourceInfo":{
		"id":30121,
"createTime":"2016-09-22 16:09:14",
"description":"Test file upload new interface",
"fileType":"png",
"ownerType":0,
"fileName":"table.png",
"systemId":"SV-UZHSH-0000",
"url":"/uam/v1/resource/enabling/uzhsh/100013957366155388.jpg",
"creator":"100013957366155388"
}
}
```

##### 3、error code  
> C00002、C00004、C00006、C00007、D00008

 

## Way of use

### Opening process  
![开通流程][account_callingProcess]

### Application scenario
**Account management**  
Developers do not have an account system and can integrate U+ account related services.  

**Developer account**  
Developers have their own account system, accessing U+ account services through cloud-connected interconnection.  

<!-- 
## Documentation
[UWS AccountService][account_document_url]
-->
## common problem

[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecificationV1.4-NorthAmericanEnvironment.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_liucheng.png
[account_callingProcess]:_media/_account/account_callingProcess.png
[account_PasswordFlow]:_media/_account/account_PasswordFlow.png



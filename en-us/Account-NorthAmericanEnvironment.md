
>  **current version**：[UWS Accountservice V1.4.0](en-us/ChangeLog/Account)  
 **Update time**：{docsify-updated} 


### Introduction

> The account service is designed to provide access control services covering the entire process of the IoT, and to build a unified user login system for developers.   

By integrating the U+IOT platform account service, the developer not only provides account management services such as registration, login, and password recovery of user accounts, but also helps developers to build unified control including device and user rights management. Consistent IOT systemic control mode.  

![账户图片][account_type]  

**Account base capability**  
1. IOT platform account registration: Users can use this interface to register an IOT account with a mobile phone or email, and call the verification code interface to obtain a verification code for registration activation.    
2. The IOT platform account login and logout, login authentication to obtain the security token (accessToken) created by the system, and the system verifies the accessToken for the user to log out.    
3. IOT account verification code application and verification. Use this interface to apply for and verify the verification code of the mobile phone or mailbox to ensure the security of registration and login.  

**Account information related ability** 
1. Query IOT platform account information and request to obtain user information (including id, loginName, email, mobile and other user attributes).     
2. To modify the account information of the IOT platform, users should actively modify their application property information, user basic properties, etc., which requires permission authentication.   


**Account system association ability**   
 
1. Third-party social account login, support QQ, WeChat, Weibo, Douban, Renren account login.   
2. The developer's own account login, generate the corresponding dark account on the U+IOT platform and authorize the user to log in to the U+ platform as the U+ account. The developer can establish its own independent developer account system.  

### Noun explanation


- **Haier U+ OAuth**
> Refers to the OAuth service provided by Haier U+, which requires the use of Haier U+ account for login authorization.  

Since Haier account has Haier U+ account right at the same time, Gu can also use Haier account to log in under this kind of authorization service;Haier account and Haier U+ account one-way interoperability, with Haier excellent home OAuth authority does not mean that Haier Group's business authority.  


- **Haier U+  Developer Account Login**
> It means that the developer has an account system and wants to use the own account system to log in to the Haier U+ platform. 

Haier U+ provides inter-platform account docking solution, with standard OAuth scheme and application front-end scheme. This kind of docking method requires offline application process. If there is demand, it can be feedback in the developer community, or through Haier U+ Business BD feedback.  

### Application scenario
**Account management**  
Developers do not have an account system and can integrate U+ account related services.  

**Developer account**  
Developers have their own account system, accessing U+ account services through cloud-connected interconnection.  

### User privacy rights  

In order to effectively protect users' privacy and optimize user experience, haier U+ has formulated haier household appliance privacy policy in accordance with existing laws and policies.Haier understands the importance of personal information to customers, and we strive to clarify our policies and measures to obtain, manage and protect users' personal information.  


In the case of user registration, download and update, login and access, the content of the privacy policy must be provided or pointed to the page, and the user needs to click to "agree" to the privacy policy. The default "agree" cannot be too hidden.After obtaining the user's "consent", it is also necessary to ensure that users can easily view the full text of the privacy policy in the process of use, and cannot hide it from display.  

**If the developer needs to provide the user service agreement terms for using the service application of haier U+ account, please contact **[ **haier U+ business BD**](en-us/Business)**. We will configure the corresponding privacy policy and service agreement terms for the application**


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
 **Token authentication：** No (header can not pass accessToken)  

**Input parameters**  

| parameter name        | types         | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:    
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00012   |   Account already exists |  &emsp;   |    
| D00015   |   Verification code error |  &emsp;   |  
 

#### User login
> User login to get accessToken and openId.   
  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/login`  
 **HTTP Method：** POST  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types        | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:  
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00002   |  Account or password error|  &emsp;   |    
| D00009   |   Logon failure exceeded limit, need to use authentication code login |  After the login failed twice, start the verification code login  |   
| D00010   |  Account locked|  After 5 failed login attempts, lock the account   |  
| D00015   |  Verification code error|   &emsp;   |  

#### Resend activation email
> After the registration is successful, but the user fails to receive the activation email due to the mail network, etc., and the user receives the activation email but does not perform the activation, and the activation email expires, the user can resend the activation through the interface. mail. The prerequisite for using this interface is that the user has already registered but is not activated. The activation time for the activation email is 120 minutes. Can be accessed without login.     
  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/sendActiveMail`  
 **HTTP Method：** POST  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types          | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| loginId     | String | Body| yes|Mailbox, need to match the mailbox format Use the following regular expression:`^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$`|  
   
 


**Output Parameters**  
**Output standard output parameters.**

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00011   |  Account not activated|  &emsp;   |    
| D00017   |  Account does not exist |  &emsp;  |   

#### Log out
>Mobile APP users exit the Haier U+ cloud platform interface  

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/logout`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT13OOQL5O7TEAB21WVIKCJTEL470 
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00005   |  The Token is not created by this application and does not pass Token validation.|  Operation is successful  |    
| D00016   |  You are logged out or not logged in |  &emsp;  |   


#### Query user information
>Obtain user information based on the registrant token.  


  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/get`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|    |    |     |     |  &emsp;   |   



**Output parameters**  

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| userProfile     | Map | Body| no|User extension information, including nicknames, avatars, etc|  


##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken: TGT13OOQL5O7TEAB21WVIKCJTEL470 
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00008   |  Illegal user|  AccessToken error  |    
  

#### User information modification
>Modify the extended attribute of the currently logged in user according to the token of the logged in person  
  

##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/update`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT13OOQL5O7TEAB21WVIKCJTEL470
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00008   |  Illegal user|  AccessToken error  |   
    
#### Apply for reset password
>When the user requests to reset the password, the user will send a link to reset the password in the user's mailbox, and the user clicks the link to reset the password.      
 

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/pwd/applyReset`  
 **HTTP Method：** POST  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00011   |  Account not activated|  &emsp;  |   
| D00017   |  Account does not exist|  &emsp;  |   

#### Get image verification code
>Get image verification code.      
 

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/captcha`  
 **HTTP Method：** POST  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
|    |    |     |     |  &emsp;  |  
      


**Output parameters**  

File stream，image/png
response.setContentType("image/png");


##### 2、Request sample  

**User request**
```java  
Header：
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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


<!-- 注释开始
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:    
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| B00001   |  Lack of required parameters|  AppId is empty  |   
| C00002  |  Appserver has no access|  Appserver has no access  |    
| C00006  | Product configuration information is empty|  Product configuration   |   
| C00007  |  AppKey is empty|  The appkey is empty according to the appId |  
| D00001  |  Digital signature error|  ThDigital signature error |  

#### Upload resource file 
> Upload the resource file to the server. (Note: To use this interface, you need to contact the capability administrator first, upload and authorize the APPId, and configure the file format and size of the uploaded resource. Otherwise, there is no error in returning the file configuration.)  
  


##### 1、Interface definition

?> **Access address：**  `/uas/v1/resource/uploadFile`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types        | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
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
appId: MB-****-0000
appVersion: 99.99.99.99990
clientId: 123
sequenceId: 2014022801010
accessToken:TGT1OY0RUUAH5D242SB68E9WX0W930  
sign: e5bd9aefd68c16a9d441a636081f11ceaed51ff58ec608e5d90048f975927e7f
timestamp: 1491014447260 
language: zh-cn
timezone: +8
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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| C00002  |  Appserver has no access|  Appserver has no access  |   
| C00004   | Insufficient operation permission|  Size format error  |   
| C00006  | Product configuration information is empty|  Product configuration   |   
| C00007  |  AppKey is empty|  The appkey is empty according to the appId |  
| D00008  | Illegal user| AccessToken error |  

注释结束 -->

[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecificationV1.4-NorthAmericanEnvironment.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_liucheng.png
[account_callingProcess]:_media/_account/account_callingProcess.png
[account_PasswordFlow]:_media/_account/account_PasswordFlow.png



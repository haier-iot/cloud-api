
>  **当前版本**：[UWS 账号服务 北美版 V1.4.0](en-us/ChangeLog/Account)  
 **更新时间**：{docsify-updated} 


### 简介

> 账号服务旨在提供涵盖物联全流程的访问控制服务，为开发者搭建统一的用户登录系统。  

开发者通过集成U+IOT平台账号服务，不仅提供用户账号的注册、登录、找回密码等账户管理服务，同时帮助开发者构建包括物联设备的统一控制、设备与用户权限管理等在内的一致性的IOT系统化管控模式。

![账户图片][account_type]
 
**账号基础能力**  

1、	IOT平台账号注册：使用此接口用户可以使用手机或邮箱注册IOT账号，并调用验证码接口获取验证码，进行注册激活
  
2、	IOT平台账号登录与退出，进行登录验证获取系统创建的安全令牌（accessToken），系统校验accessToken进行用户退出登录。  

3、	IOT账号验证码申请与验证，使用此接口可以申请和验证手机或邮箱的验证码保证注册、登录的安全性。  

**账号信息相关能力**  

1、	查询IOT平台账号信息，请求获取用户信息（包括id、loginName、email、mobile等用户属性）。

2、	修改IOT平台账号信息，用户主动修改其应用属性信息、用户基础属性等，需要进行权限认证。  

**账号体系关联能力**  

1、	第三方社交账号登录，支持QQ、微信、微博、豆瓣、人人网账号登录。

2、	开发者的自有账号登录，在U+IOT平台生成对应的暗账号并以U+账号身份进行用户授权登录到U+平台，开发者可建立其独立的开发者账号体系。


### 名词解释

- **海尔优家 OAuth**
> 指海尔优家对外提供的OAuth服务，需要使用海尔优家账号进行登录授权；


- **海尔优家 第三方登录**
> 指使用第三方平台账号登录海尔优家平台，如微信、京东、淘宝等；


- **海尔优家 开发者自有账号登录**
> 指开发者已有账户体系，且希望使用自有账户体系登录海尔优家平台；  
该种对接方式需要走线下申请流程，如有需求，可在开发者社区反馈，或通过[海尔优家商务BD][Business]反馈； 

### 应用场景
**账号管理**  
开发者没有账户系统，可集成U+账号的相关服务。  

**开发者账号**  
开发者有自己的账户系统，通过云云对接互联方式接入U+账户服务。  

### 用户密码的安全性
#### 安全性说明

由于openapi使用md5+salt 密码处理方式，如果uws-uam接口也使用md5方式，需要app将密码明文传输，此方式会降低整个原gea环境的安全性，所以app传输密码必须加密。登录加密为sha256，注册时需将密码加密为sha256，再利用aes加密为密文，base64编码。

 
##### 密码传输流程  
![密码传输流程图片][account_liucheng]  

##### 密码加密和解密算法
算法：RSA

秘钥长度:1024

秘钥：app端持有公钥，服务端持有私钥，公钥私钥为一对秘钥，公钥加密，私钥解密

算法代码：

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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|   
| C00002  |  Appserver has no access|  Appserver has no access  |   
| C00004   | Insufficient operation permission|  Size format error  |   
| C00006  | Product configuration information is empty|  Product configuration   |   
| C00007  |  AppKey is empty|  The appkey is empty according to the appId |  
| D00008  | Illegal user| AccessToken error |  



[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecificationV1.4-NorthAmericanEnvironment.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_liucheng.png
[account_callingProcess]:_media/_account/account_callingProcess.png
[account_PasswordFlow]:_media/_account/account_PasswordFlow.png



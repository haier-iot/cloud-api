
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

1、	第三方社交账号登录，支持google、amazon等第三方账号登录。

2、	开发者的自有账号登录，在U+IOT平台生成对应的暗账号并以U+账号身份进行用户授权登录到U+平台，开发者可建立其独立的开发者账号体系。


### 应用场景
**账户服务场景应用流程：**

账号服务应用流程包括用户注册、登录、密码找回、会话分享、第三方社交账号、账号注销以及用户信息修改等相关流程

![场景应用流程][scene_flow]

**注册**

用户注册填写邮箱/手机号信息，获取验证码完成认证注册；

用户首次注册需要同意并接受隐私协议（应用端从平台获取）。

**登录**

输入账号密码认证登录，当输入密码错误3次时开始需要获取图形验证码填写，密码错误超过5次时锁定账号5小时；

账号密码验证无误后检测用户隐私协议版本，用户接受最新版隐私协议后完成登录流程

**找回密码**

忘记密码可以通过邮箱/手机验证码的方式重置

**会话分享**

跨应用访问提供会话分享服务，APP_1的会话可以分享给APP_2；

会话分享需要会话分享验证码进行认证

**第三方账号**

支持第三方账号认证登录，使用oauth账号授权方式登录U+账户服务；

第三方账号与U+ 账号实现绑定，并维护绑定关系与U+账号信息

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

### 接口列表


#### 用户注册 
> 注册新用户



##### 1、接口定义

?> **请求地址：**  `/uam/v1/security/register`  
 **HTTP Method：** POST   </br>
 **Token验证：** 否 (header可以不传入accessToken)  

**输入参数**  

| 参数名        | 类型         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| loginId     | String | Body| 是|邮箱，需要符合邮箱格式，使用如下正则表达式:`^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$`|  
| password     | String | Body| 是 |密码：长度：6 – 16个字符之间 ，即最少6位，最大16位</br>必须包含如下至少两种字符的组合:小写字母、大写字母、数字、特殊字符（`~!@#$%^&*()-_=+\|[{}];:'",<.>/?`）和空格</br>密码不能和帐号或者帐号的倒写一样；</br>注：密码强度是app的要求，传入服务端为 密文， 密文= `aes（sha256（password））`,aes的秘钥本文档不提供，开发时再由双发约定|  
| captcha     | String | Body| 是 |验证码，4位字母和数字组合。当密码输入错误三次及以上，强制用户输入验证码，如输入5次，锁定账号5小时|  
| userProfile     | Map | Body| 否 |添加用于满足各不同应用对用户信息的不同需求。当应用需要扩展用户属性时，可以向云平台用户系统申请，申请时列明需要扩展的属性，并列明每个属性对应的key、类型及长度|  


**输出参数**  

输出标准输出参数 


##### 2、请求样例  

**用户请求**
```java  
POST	

/uam/v1/security/register

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

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}

```

##### 3、错误码  

> D00012、D00015


#### 用户登录
> 用户获取登录accessToken和openId
  

##### 1、接口定义

?> **接入地址：**  `/uam/v1/security/login`  
 **HTTP Method：** POST   </br>
 **Token验证：** 否  

**输入参数**  

| 参数名        | 类型        | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----
| loginId     | String | Body| yes|邮箱，需要符合邮箱格式，使用如下正则表达式：`^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$`|  
| password     | String | Body| 是|密码：长度：6 – 16个字符之间 ，即最少6位，最大16位</br>必须包含如下至少两种字符的组合:小写字母、大写字母、数字、特殊字符（`~!@#$%^&*()-_=+\|[{}];:'",<.>/?`）和空格</br>密码不能和帐号或者帐号的倒写一样；</br>注：密码强度是app的要求，传入服务端为 密文， 密文= `aes（sha256（password））`,aes的秘钥本文档不提供，开发时再由双发约定|  
| captcha     | String | Body| no |验证码，4位字母和数字组合。</br>当密码输入错误三次及以上，强制用户输入验证码，如输入5次，锁定账号5小时|  


**输出参数**  

|   参数名      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| accessToken   |   String |  Header   |  是   |  安全令牌  |  
| userId   |   String |  Body   |  是   | 用户标识  |
| status   |   String |  Body   |  是   | 0:激活   1:未激活   |  



##### 2、请求样例  

**用户请求**
```java  
POST	

/uam/v1/security/login

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

**请求应答**

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

##### 3、错误码 

> D00002、D00009、D00010、D00015
 

#### 发送激活邮件
> 在注册成功后，但因邮件网络等原因，用户未能收到激活邮件的情况，以及用户收到激活邮件但没有执行激活，导致激活邮件过期等情况下，可通过本接口向用户重新发送激活邮件。</br>
> 使用本接口的前提条件是用户已经注册，但未激活。激活邮件的有效时间为120分钟。不需登录可以访问。   
  


##### 1、接口定义

?> **接入地址：**  `/uam/v1/security/sendActiveMail`  
 **HTTP Method：** POST  
 **token验证：** 否 

**Input parameters**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| loginId     | String | Body| yes|邮箱，需要符合邮箱格式，使用如下正则表达式：`^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$`|  
   
 


**输出参数** 标准输出参数  


##### 2、请求样例  

**用户请求**
```java  
POST

/uam/v1/security/sendActiveMail`

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

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功!"
}
```

##### 3、error code  

> D00011、D00017

#### 退出登录

> 移动APP用户退出Haier U+ 云平台接口

##### 1、接口定义

?> **接入地址：**  `/uam/v1/security/logout`  
 **HTTP Method：** POST   </br>
 **Token验证：** 是  

**输入参数** 

输入标准输入参数 

   
**输出参数**    

输出标准输出参数

##### 2、请求样例  

**用户请求**

```java
POST

/uam/v1/security/logout

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

**请求应答**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}
```

##### 3、error code  

> D00005、D00016


#### 查询用户信息
> 根据登录者token，获取用户信息


##### 1、接口定义

?> **接入地址：**  `/uam/v1/users/get`  
 **HTTP Method：** POST  
 **Token验证：** 是  

**输入参数** 

输入标准输入参数  



**输出参数**  

|   参数名      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------|
| userProfile     | Map | Body| 否|用户扩展信息,包括昵称、头像等。|  


##### 2、请求样例

**用户请求**
```java  
POST

/uam/v1/users/get

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

**请求应答**

```java
{
	“retCode”: “00000”,
	“retInfo”: “正确”,
	“userProfile”: 
	{
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

##### 3、错误码

> D00008
  

#### 用户信息修改

> 根据登录人员的token，修改当前登录用户的扩展属性
  

##### 1、接口定义

?> **接入地址：**  `/uam/v1/users/update`  
 **HTTP Method：** POST  
 **Token验证：** 是  

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| userProfile     | Map | Body| 是|用户拓展信息，包括昵称、头像等|    


**输出参数** 

标准输出参数

##### 2、请求样例 

**用户请求**
```java  
POST

/uam/v1/users/update

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
	"userProfile": 
	{
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

**请求应答**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}

```

##### 3、错误码

> D00008
 
    
#### 申请重置密码
> 用户申请重置密码，会往用户邮箱中发送重置密码的链接，用户点击链接进行重置密码操作。     
 

##### 1、接口定义

?> **接入地址：**  `/uam/v1/security/pwd/applyReset`  
 **HTTP Method：** POST  
 **Token验证：** 否  

**Input parameters**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|   loginId   | String | body | 是 | 邮箱地址，需要符合邮箱格式,使用如下正则表达式：`^\w+([.+-]\w+)*@\w+([.-]\w+)*(\.\w{2,5})+$`|      


**输出参数** 

标准输出参数 

##### 2、请求样例  

**用户请求**
```java  
POST

/uam/v1/security/pwd/applyReset

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

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"操作成功"
}
```

##### 3、错误码

> D00011、D00017


#### 获取图片验证码

> 获取图片验证码   
 

##### 1、接口定义

?> **接入地址：**  `/uam/v1/security/captcha`  
 **HTTP Method：** POST  
 **Token验证：** 否  


      


**输出参数**  

文件流，image/png
response.setContentType("image/png");


##### 2、请求样例  

**用户请求**
```java  
POST

/uam/v1/security/captcha

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
appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
Content-Encoding: utf-8
Content-type: application/json

```  

**请求应答**

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


[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecificationV1.4-NorthAmericanEnvironment.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_liucheng.png
[account_callingProcess]:_media/_account/account_callingProcess.png
[account_PasswordFlow]:_media/_account/account_PasswordFlow.png
[scene_flow]:_media/_account/scene_flow.png



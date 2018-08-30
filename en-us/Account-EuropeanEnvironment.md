!>  **current version**：[UWS Accountservice V2.0.0][account_document_url]  
 **release time**：2018-07-19  


### Introduction

> The account service is designed to provide access control services covering the entire process of the IoT, and to build a unified user login system for developers.   

By integrating the U+IOT platform account service, the developer not only provides account management services such as registration, login, and password recovery of user accounts, but also helps developers to build unified control including device and user rights management. Consistent IOT systemic control mode.  

![账户图片][account_type]

### Noun explanation

- **Haier U+ OAuth**
> Refers to the OAuth service provided by Haier Youjia, which requires the use of Haier Youjia account for login authorization.  

Since Haier account has Haier Youjia account right at the same time, Gu can also use Haier account to log in under this kind of authorization service;Haier account and Haier Youjia account one-way interoperability, with Haier excellent home OAuth authority does not mean that Haier Group's business authority.  


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

### User privacy data security  
#### Safety instructions  
User privacy data item.  
| **Field** | **Encryption processing**  |  **Interface** |  
| ------------- |:-------------:|  
|email|	RSA encryption	|Register, login, reset password, change password|   
|password|	RSA encryption	|Register, login, reset password, change password|   
#### Secret key usage process  
![密码传输流程图片][account_PasswordFlow1]  
![密码传输流程图片][account_PasswordFlow2]  
#### Cipher encryption and decryption algorithm  
algorithm:RSA  
secret key:The app side holds the public key, the server side holds the private key, the public key private key is a pair of secret keys, the public key is encrypted, and the private key is decrypted.  
algorithm code:  

```java
package com.hshbic.cloud.user.util.encrypt;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.security.Key;
import java.security.KeyFactory;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.NoSuchAlgorithmException;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;
import javax.crypto.Cipher;
import org.apache.commons.codec.binary.Base64;
import com.hshbic.cloud.user.model.AppRsaKeys;

public class RSAUtil {

	public static final String CHARSET = "UTF-8";
	public static final String RSA_ALGORITHM = "RSA";
	public static Map<String, AppRsaKeys> KEYSMAP = new ConcurrentHashMap<String, AppRsaKeys>();
	public static Map<String, String> createKeys(int keySize) {
		// Create a KeyPairGenerator object for the RSA algorithm
		KeyPairGenerator kpg;
		try {
			kpg = KeyPairGenerator.getInstance(RSA_ALGORITHM);
		} catch (NoSuchAlgorithmException e) {
			throw new IllegalArgumentException("No such algorithm-->["
					+ RSA_ALGORITHM + "]");
		}
		// Initialize the KeyPairGenerator object, key length
		kpg.initialize(keySize);
		// Generate key pair
		KeyPair keyPair = kpg.generateKeyPair();
		// Get the public key
		Key publicKey = keyPair.getPublic();
		String publicKeyStr = Base64.encodeBase64URLSafeString(publicKey
				.getEncoded());
		// Get the private key
		Key privateKey = keyPair.getPrivate();
		String privateKeyStr = Base64.encodeBase64URLSafeString(privateKey
				.getEncoded());
		Map<String, String> keyPairMap = new HashMap<String, String>();
		keyPairMap.put("publicKey", publicKeyStr);
		keyPairMap.put("privateKey", privateKeyStr);
		return keyPairMap;
	}
	/**
	 * Get the public key
	 * 
	 * @param publicKey
	 *  Key string (base64 encoded)
	 * @throws Exception
	 */
	public static RSAPublicKey getPublicKey(String publicKey)
			throws NoSuchAlgorithmException, InvalidKeySpecException {
		// Get the public key object by X509 encoded Key instruction
		KeyFactory keyFactory = KeyFactory.getInstance(RSA_ALGORITHM);
		X509EncodedKeySpec x509KeySpec = new X509EncodedKeySpec(
				Base64.decodeBase64(publicKey));
		RSAPublicKey key = (RSAPublicKey) keyFactory
				.generatePublic(x509KeySpec);
		return key;
	}
	/**
	 * Get the private key
	 * 
	 * @param privateKey
	 *   Key string (base64 encoded)
	 * @throws Exception
	 */
	public static RSAPrivateKey getPrivateKey(String privateKey)
			throws NoSuchAlgorithmException, InvalidKeySpecException {
		// Obtain the private key object by the Key instruction encoded by PKCS#8
		KeyFactory keyFactory = KeyFactory.getInstance(RSA_ALGORITHM);
		PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(
				Base64.decodeBase64(privateKey));
		RSAPrivateKey key = (RSAPrivateKey) keyFactory
				.generatePrivate(pkcs8KeySpec);
		return key;
	}
	/**
	 * Public key encryption
	 * 
	 * @param data
	 * @param publicKey
	 * @return
	 */
	public static String publicEncrypt(String data, RSAPublicKey publicKey) {
		try {
			Cipher cipher = Cipher.getInstance(RSA_ALGORITHM);
			cipher.init(Cipher.ENCRYPT_MODE, publicKey);
			return Base64.encodeBase64URLSafeString(rsaSplitCodec(cipher,
					Cipher.ENCRYPT_MODE, data.getBytes(CHARSET), publicKey
							.getModulus().bitLength()));
		} catch (Exception e) {
			throw new RuntimeException("Encountered an exception while encrypting the string [" + data + "] ", e);
		}
	}
	/**
	 * Private key decryption
	 * 
	 * @param data
	 * @param privateKey
	 * @return
	 */
	public static String privateDecrypt(String data, RSAPrivateKey privateKey) {
		try {
			Cipher cipher = Cipher.getInstance(RSA_ALGORITHM);
			cipher.init(Cipher.DECRYPT_MODE, privateKey);
			return new String(rsaSplitCodec(cipher, Cipher.DECRYPT_MODE,
					Base64.decodeBase64(data), privateKey.getModulus()
							.bitLength()), CHARSET);
		} catch (Exception e) {
			throw new RuntimeException("Encountered an exception while decrypting the string [" + data + "] ", e);
		}
	}
	/**
	 * Private key encryption
	 * 
	 * @param data
	 * @param privateKey
	 * @return
	 */
	public static String privateEncrypt(String data, RSAPrivateKey privateKey) {
		try {
			Cipher cipher = Cipher.getInstance(RSA_ALGORITHM);
			cipher.init(Cipher.ENCRYPT_MODE, privateKey);
			return Base64.encodeBase64URLSafeString(rsaSplitCodec(cipher,
					Cipher.ENCRYPT_MODE, data.getBytes(CHARSET), privateKey
							.getModulus().bitLength()));
		} catch (Exception e) {
			throw new RuntimeException("Encountered an exception while encrypting the string [" + data + "] ", e);
		}
	}
	/**
	 * Public key decryption
	 * 
	 * @param data
	 * @param publicKey
	 * @return
	 */
	public static String publicDecrypt(String data, RSAPublicKey publicKey) {
		try {
			Cipher cipher = Cipher.getInstance(RSA_ALGORITHM);
			cipher.init(Cipher.DECRYPT_MODE, publicKey);
			return new String(rsaSplitCodec(cipher, Cipher.DECRYPT_MODE,
					Base64.decodeBase64(data), publicKey.getModulus()
							.bitLength()), CHARSET);
		} catch (Exception e) {
			throw new RuntimeException("Encountered an exception while decrypting the string [" + data + "] ", e);
		}
	}
	private static byte[] rsaSplitCodec(Cipher cipher, int opmode,
			byte[] datas, int keySize) {
		int maxBlock = 0;
		if (opmode == Cipher.DECRYPT_MODE) {
			maxBlock = keySize / 8;
		} else {
			maxBlock = keySize / 8 - 11;
		}
		ByteArrayOutputStream out = new ByteArrayOutputStream();
		int offSet = 0;
		byte[] buff;
		int i = 0;
		try {
			while (datas.length > offSet) {
				if (datas.length - offSet > maxBlock) {
					buff = cipher.doFinal(datas, offSet, maxBlock);
				} else {
					buff = cipher.doFinal(datas, offSet, datas.length - offSet);
				}
				out.write(buff, 0, buff.length);
				i++;
				offSet = i * maxBlock;
			}
			byte[] resultDatas = out.toByteArray();
			return resultDatas;
		} catch (Exception e) {
			throw new RuntimeException("An exception occurs when the encryption/decryption threshold is [" + maxBlock + "] ", e);
		} finally {
			try {
				out.close();
			} catch (IOException e) {
				
			}
		}
	}
	public static void main(String[] args) throws Exception {
		Map<String, String> keyMap = RSAUtil.createKeys(1024);
		String publicKey = keyMap.get("publicKey");
		String privateKey = keyMap.get("privateKey");
		System.out.println("Public key: \n\r" + publicKey);
		System.out.println("Private key: \n\r" + privateKey);
		String str = "123";
		System.out.println("\r Clear text：\r\n" + str);
		System.out.println("\r Clear text size：\r\n" + str.getBytes().length);
		String encodedData = RSAUtil.publicEncrypt(str,
				RSAUtil.getPublicKey(publicKey));
		System.out.println("Ciphertext：\r\n" + encodedData);
		String decodedData = RSAUtil.privateDecrypt(encodedData,
				RSAUtil.getPrivateKey(privateKey));
		System.out.println("Decrypted text: \r\n" + decodedData);
	}
}

```  

### Use of language templates
#### Support for overseas oem version app  
The appId in the header is oem type, and the oem template is used to register, activate, and reset the password.  
OEM APPID is limited to MB-OEM-0000, MB-OEM-0001

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
| User login     | User login to get accessToken | yes| no|  
| Get verification code     | Apply for a verification code before registration to verify the user's real email address | yes| no|  
| Reset Password     | To reset the password, you need to apply for a verification code first. | yes| no|  
| Change password | To reset the password, you need to apply for a verification code first. | yes| no|  
|Get the public key | Get the public key | yes| no|  
|Verify the public key |Provide the front-end application with an interface to verify the validity of the local public key| yes| no|  
|Get graphic verification code |Obtaining a graphics verification code, unlike the V1 interface, is to increase the limit, and tomorrow 20 requests per application limit.| yes| no| 
| Log out v1   |Mobile APP users exit the Haier U+ cloud platform interface| yes| no|  
| Query user information v1 | Obtain user information based on the registrant token | yes| no|  
| User information modification v1   | Modify the extended attribute of the currently logged in user according to the token of the logged in person |  yes| no|     
| User accepts privacy policy v1  | User accepts the privacy policy and records the privacy policy version number |  yes| no|    

#### User registration 
> Register new user  
> Preconditions:Get the verification code interface  



##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/registerEmailAcounnt`  
 **HTTP Method：** POST

**Input parameters**  

| parameter name        | types         | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
|email	|String	|Body|	Yes|	Public key encryption is required, the backend service decrypts and verifies the rules|  
|password|String|Body|Yes|	Password: Use public key encryption. The long backend service decrypts and verifies the rules. See section 2.6 for details.|  
|captcha|	String|	Body|	Yes	|Graphic verification code, a combination of 4 letters and numbers. Each verification code can only be used once. It will be invalid after use or expired and needs to be re-acquired. According to the requirement, msgCode fails to verify more than three times, and the user is required to input the graphic verification code.|
|userProfile|Map|Body|No|Added to meet the user information needs of different applications. When the application needs to extend user attributes, it can apply to the cloud platform user system. The attributes that need to be extended are listed in the application, and the key, type and length corresponding to each attribute are also listed.|
|msgCode|String|	Body|	yes	|Verification code, the user applies for verification before registration, and sends it to the user's mailbox. You need to fill in this verification code when registering, 6 random numbers.|



**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  

**User request**
```java  
Header：
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT20QGFDOIY0U8S2754W0ZH3XY390
sign: 5a46d3ca2a7f4589e97fbf4f2eae4b74c56eeb685884d91f136d339ea523dd0a
timestamp: 1533868371182 
language: en
timezone: +8
appKey: dg00ad3ea782c34aa86c656f2a401d8e
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 434
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

[no cookies]

Body:
{
	"email": "mzyc5-J0Cucq84wqFh7KfkWWqd3P3EagdvW2Eb8f6fqoQ3oX1Llhdt2o_YRpnu0D6xLUeocU7ckagnr5YlpNwh2OlVO6SKUNsmp9sXetFpjd9riOFeaJRqGeta8oPDMqPOnTIGt-9XaZ4nr5v2zH44eNalPSwL1kyUykVdHjbrU",
	"password": "ZrZjvu0dpDNqKoQDUHCnyPyNw1gpvy6_b3BoVRQnpPW4Gj31Ieyr8B0DkbiayEWV2x5slwqvf4HU_b-ZF_NdMC-V_OQ5VZxZixqmH-piZ8uAMzmZaiVf5Hxn26g6w1x679Oma2xiEnRdm2YpsVKhzwHiBn0-uZxNQnUxLZ9YI6k",
	"msgCode": "050289",
	"userProfile": {
		"name": "test"
	},
	"captcha": "dmpp"
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
![开通流程][account_liucheng]

### Application scenario
**Account management**  
Developers do not have an account system and can integrate U+ account related services.  

**Developer account**  
Developers have their own account system, accessing U+ account services through cloud-connected interconnection.  


## Documentation
[UWS AccountService][account_document_url]

## common problem

[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_callingProcess.png
[account_PasswordFlow1]:_media/_account/account_PasswordFlow1.png
[account_PasswordFlow2]:_media/_account/account_PasswordFlow2.png



!>  **current version**：UWS Accountservice V2.0.0  
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
| ------------- |:----------:|:-----:|  
|email    |	RSA encryption	|Register, login, reset password, change password|    
|password |	RSA encryption	|Register, login, reset password, change password|  
 
#### Secret key usage process  
![密码传输流程图片][account_PasswordFlow1]  
![密码传输流程图片][account_PasswordFlow2]  
#### Cipher encryption and decryption algorithm  
algorithm:RSA  
Secret key length: 1024  
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
### User privacy agreement  

#### User privacy agreement version  

Version: V1.0.0, uppercase V  

#### View user privacy agreement content  

Https://uws-gea-euro.haieriot.net/userweb/agreement?v=v1.0.0&lg={language}
The lg parameter refers to the internationalized language table in the [access specification](en-us/AccessSpecification).
  
### Use of language templates
#### Support for overseas oem version app  
The appId in the header is oem type, and the oem template is used to register, activate, and reset the password.  
OEM APPID is limited to MB-OEM-0000, MB-OEM-0001

### Public structure  

#### no  

## Interface list


### Haier U+ User class interface
> API interface overview

| API name        | effect          | Whether open | Special Note|
| ------------- |:-------------:|:-----:|:-------------:|
| User registration     | Register new user | yes|  no |  
| Email Login     | User login to get accessToken | yes| no|  
| Get verification code     | Apply for a verification code before registration to verify the user's real email address | yes| no|  
| Use email to reset the password    | To reset the password, you need to apply for a verification code first. | yes| no|  
| Change password | To reset the password, you need to apply for a verification code first. | yes| no|  
|Get the public key | Get the public key | yes| no|  
|Verify the public key |Provide the front-end application with an interface to verify the validity of the local public key| yes| no|  
|Get graphic verification code |Obtaining a graphics verification code, unlike the V1 interface, is to increase the limit, and tomorrow 20 requests per application limit.| yes| no|  
|	Apply to cancel account and device information|The user requests to delete the account information and the device binding relationship, and send an email notification.|yes|no|  
| Log out v1   |Mobile APP users exit the Haier U+ cloud platform interface| yes| no|  
| Query user information v1 | Obtain user information based on the registrant token | yes| no|  
| User information modification v1   | Modify the extended attribute of the currently logged in user according to the token of the logged in person |  yes| no|     
| User accepts privacy policy v1  | User accepts the privacy policy and records the privacy policy version number |  yes| no|    

#### User registration 
> Register new user  


##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/registerEmailAcounnt`  
 **HTTP Method：** POST  
 **Preconditions:** Get the verification code interface  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types         | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
|email	|String	|Body|	Yes|	Public key encryption is required, the backend service decrypts and verifies the rules|  
|password|String|Body|Yes|	Password: Use public key encryption. The long backend service decrypts and verifies the rules. See section User privacy data security  for details. Server verification rules: uppercase and lowercase letters, numbers, special characters, three or more `^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])[a-zA-Z0-9]{6,20}$`|  
|captcha|	String|	Body|	Yes	|Graphic verification code, a combination of 4 letters and numbers. Each verification code can only be used once. It will be invalid after use or expired and needs to be re-acquired. According to the requirement, msgCode fails to verify more than three times, and the user is required to input the graphic verification code.|
|userProfile|Map|Body|No|Added to meet the user information needs of different applications. When the application needs to extend user attributes, it can apply to the cloud platform user system. The attributes that need to be extended are listed in the application, and the key, type and length corresponding to each attribute are also listed.|
|msgCode|String|	Body|	yes	|Verification code, the user applies for verification before registration, and sends it to the user's mailbox. You need to fill in this verification code when registering, 6 random numbers.|



**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  

**Request address**  
```  
https://uws-gea-euro.haieriot.net/uam/v2/user/registerEmailAcounnt
```  

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
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| D00012   |  The mailbox already exists|  &emsp;  |   
|D00015  |  Graphic verification code error|  &emsp;  |   
|D00022 | Msgcode verification code error| 6-digit random number requested by the user before registration  |  
|D00009 |  The number of attempts exceeds the limit, the number of failed attempts is exceeded, and a graphic verification code is required.|  msgCode verification failed more than three times |  
|B00010 |  Public key error|  Server decryption failed  |  
|B00004 | Incorrect mailbox and password| Parameter error handling  |  
   


 

#### Email Login
> User login to get accessToken  
  


##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/loginEmailAcounnt`  
 **HTTP Method：** POST  
 **Preconditions:** Use email registration  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types        | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| email    | String | Body| yes|Public key encryption is required, the backend service decrypts and verifies the rules|  
| password     | String | Body| yes|Public key encryption is required, the backend service decrypts and verifies the rules |  
| captcha     | String | Body| no |Graphic verification code, a combination of 4 letters and numbers. Each verification code can only be used once. It will be invalid after use or expired and needs to be re-acquired. Log in to enter the wrong password. You must enter the graphic verification code when the number of times is greater than or equal to three.When the user enters the wrong password 5 times, the account is locked for 5 hours.|  


**Output parameters**  

|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| accessToken   |   String |  Header   |  yes   |  Security token  |  



##### 2、Request sample  

**Request address**  
```  
https://uws-gea-euro.haieriot.net/uam/v2/user/loginEmailAcounnt
```

**User request**
```java  
Header：  
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTWCHUYVYMTS7L2QMJ5L6K8ERHI00
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

[no cookies]

Body:
{
	"email": "cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
	"password": "hPeBKqC3OH8uG1yDM3l2CEk6zmvAzUonBxIcR6Z40IIPV8U20A_26nXX7wMn39kL5pJ-Irybmua9YUMYgmfNFZoZIyQzP6UTznuupqwZrVMr9vDH_4rLRzlcno8rBgxvWL6EUwL_ivK-Bwx6pOqIawjVTGoxGX8OSzwHcKK7Dvw",
	"captcha": "kq6g"
}


```  

**Request response**

```java
header:
accessToken: TGT2SI3VVPHX630U2VWJRYV3K25MM0

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
| 10000   | The login is successful, but the password security level is increased. Please change the password.| User uses weak password  |   
| 00001   | Successful login but did not accept the latest version of the privacy agreement| Old account does not accept the privacy policy  |   
|D00002 |  Account or password error| User uses wrong password  |   
|D00009 | Msgcode verification code error| 6-digit random number requested by the user before registration  |  
|D00009 | Logon failure exceeded limit, need to use authentication code login| After the login fails 3 times, the verification code is started. |  
|D00010 |  Account locked| After 5 failed login attempts, lock the account  |  
|D00015 | Verification code error| &emsp;  |  
|B00010 | Public key error| Server decryption failed  | 
 

#### Get verification code
> Apply for a verification code before registration to verify the user's real email address.  

##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/applyVerificationCode`  
 **HTTP Method：** POST  
 **Preconditions:** Registered  
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types          | location  | required|description|
| ------------- |:-------------:|:-----:|:-------------:|
| email    | String | Body| yes|Public key encryption is required, the backend service decrypts and verifies the rules|  
| type    | String | Body| yes|1: Registration  2: Retrieve password 5: cancel account|  
   

**Output Parameters**  
**Output standard output parameters.**  


|   name      |     types      | location  |required |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;   |

##### 2、Request sample  
**Request address**  
```
https://uws-gea-euro.haieriot.net/uam/v2/user/applyVerificationCode
```  

**User request**
```java  
Header：
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: 0cdb49b20d552b6f99384373b1e7f3c34136237b7a39d91ab63b35035f54f8d0
timestamp: 1533886290915 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 194
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

[no cookies]

Body:
{
	"email": "Vfg5EFcuBWBMhs7MkSfWkXmvNaE0uuUcF6abMJ1NrrpaWbNwD9qVndJABZeHm0OJL4Lndxih1HMuB16lWWPSupFybylkgl-ztjjKhoPc2K7QCLV0n2i73ei2LhTLIZdQ_VsPUhtM9jET50MFNRDAUpnBHwQwrj2JUDi0SAztMbg",
	"type": "2"
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
| D00017   | Account does not exist| Unregistered user reset password,Return when input parameter type is 2 |   
| D00012 |  Account already exists| Registered users apply for registration verification code,Return when the input parameter type is 1.  |   
|B00010 | Public key error| Server decryption failed  | 
  
 

#### Use email to reset the password
>To reset the password, you need to apply for a verification code first.  

  
##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/resetPassword`  
 **HTTP Method：** POST  
 **Preconditions:** Registered,Application verification code 
 **Token authentication：** No  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|email	|String	|Body|	Yes|	Public key encryption is required, the backend service decrypts and verifies the rules|  
|password|String|Body|Yes|	New password: Public key encryption is required. The long backend service decrypts and verifies the rules.See section User privacy data security  for details.|  
|captcha|	String|	Body|	No	|Graphic verification code, a combination of 4 letters and numbers. Each verification code can only be used once. It will be invalid after use or expired and needs to be re-acquired. For the same App, the same mobile phone terminal mailbox verification code fails to verify the authentication 3 times, you need to enable the graphic verification code for verification, 3 times configurable, the default is 3 times. After the verification of the mailbox verification code is successful, the number of allowed failures is restored to 0.|  
|msgCode|String|Body|yes|Verification code, the user applies for the verification code to reset the password, and sends it to the user's mailbox. You need to fill in this verification code when registering, 6 random numbers.|  
   
 


**Output parameters**  
**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 

##### 2、Request sample  
**Request address**  
```
https://uws-gea-euro.haieriot.net/uam/v2/user/resetPassword
```

**User request**
```java  
Header：
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: 2e997f503323fcbabfab0bf5f54da2a3bdecc60a6924519b7c90d9b20e0b62dd
timestamp: 1533886628775 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 404
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

[no cookies]

Body:
{
	"email": "qra5hNX9V5c57aj4mbjaxqnfcWbCBXncyNocZLvWfWRsMc1mJ2rnqUpELkgJ8qHJbKMkQgNUnDpvGN9Sj24LbvcL4a9UosZtpLRF4ZSGSiuXbpL75U0mSrkQvB8QulmW_tk9rLZP_cRPkImI_OS0ar24ZkO1yNnAA7y0XgsFXZs",
	"password": "FRR_c9JhWIL5JCtZ0-lD54MbnRHxzLtdejp_45E72iWC5ARhL36R_R3XiQqpDL8LqvM11seGx86W59T8Topq_54V4z_4lTsVod7N_VuHSRsFNQLwlu3Hnaat1_OK6HWN5hhk1DPhXcAgsj8JTn8UhPAONHdPROgtr7vDpIPgxc0",
	"msgCode": "632438",
	"captcha": "9mf2"
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
| D00017   | Account does not exist| Unregistered user reset password  |   
| B00010   |Public key error| Server decryption failed  |   
| D00009   | Login failed to exceed the limit, you need to use the graphical verification code to log in| After the login fails 3 times, the verification code is started.  |   
| D00022   | msgCode verification code error| 6-digit random number requested by the user before resetting the password  |   

#### Change password
>To reset the password, you need to apply for a verification code first.

  


##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/changePassword`  
 **HTTP Method：** POST  
 **Preconditions:** Login  
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|  
|email	|String	|Body|	Yes|	Public key encryption is required, the backend service decrypts and verifies the rules|  
|newPassword|String|Body|Yes|	New password: Public key encryption is required. Long backend service decryption and verification rules|  
|captcha|	String|	Body|	Yes	|Graphic verification code, for the same App, the same mobile phone terminal to modify the password when the continuous verification fails m times, you need to enable the graphic verification code for verification, m configurable, the default is 3 times. After the verification code is enabled, if the graphic verification code is entered correctly, the corresponding account will be locked for n times after the same day, and n can be configured. The default is 10 times.|  
     



**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 


##### 2、Request sample  
**Request address**  
```
https://uws-gea-euro.haieriot.net/uam/v2/user/changePassword
```  

**User request**
```java  
Header：
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTWCHUYVYMTS7L2QMJ5L6K8ERHI00
sign: 52b96f85e392ab0172d98f3ac1d7a5f7a284c387d4eeb1ea0d428df73aa55fe7
timestamp: 1533880044559 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 391
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

[no cookies]

Body:
{"password":"M86jq-ZxErDwBcy_p2gztuBKVbk_nlrHTvePtQP2YGqzXW5q6zbY-a5sV7rltbLD8Kl4Jc8O6emWz3zEOeANJfVQkIbjq2wPc4XEeHls3AwVqnDR6GB2ncljRvX-ZlD2UHcM4AzuJk3I-stOYLnbTPVoDTSxbyNfxEREXq2e2ZI","newPassword":"MrGIHA_edrlvBIvxYSn5Kstp7pj89ccQ_UFYQNVHQPuulIfJ1QRoEoi-Oq8mpYX-58BU2eglmfPcV8bpQRXH9ILRXbiVLaU0ZhCStkv68l9Q_GuSYrjEISAKK62naHwbMQrzb_seUnmQsGtCwrJx8WtQdnm6Gcz7aGZ7DGWzLC8","captcha":"73ll"}


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
| D00003   |Token does not exist|User not logged in  |   
| D00002   |The original password is wrong|Enter the wrong original password when changing the password  |  
| B00010   |Public key error| Server decryption failed  |   
| D00009   | Login failed to exceed the limit, you need to use the graphical verification code to log in| After the login fails 3 times, the verification code is started.  |  
| B00004   |The parameter does not meet the rule requirements|Wrong password format  |  

#### Get the public key  
>Get the public key  
  

##### 1、Interface definition

?> **Access address：**  `/uam/v2/mgr/getPublicKey`  
 **HTTP Method：** POST  
 **Preconditions:** Use a valid appId  
 **Token authentication：** No 

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|     |  | | |&emsp;|    


**Output parameters**  

publicKey

##### 2、Request sample  
**Request address**  
```  
https://uws-gea-euro.haieriot.net/uam/v2/mgr/getPublicKey
```  

**User request**
```java  
[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT36PQONTJY5UY02L214BESBJT9S0
sign: 9972aec958ec085bf5305f81ff135b933def3bde65fdb59cb12bc6caa0ccb17a
timestamp: 1533894864095 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 0
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


```  

**Request response**

```java
{"retCode":"00000","retInfo":"成功","publicKey":"MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC7jrpGqqsUERuS3RhjKGoKSFaoUyfk5eKwjxSq3ARW5sMg8hsnuC0dmLEOrUoaJYSoaLOn7V1uvpX4PYVJ89BjnDJtjbFoYAsk3VMVsTiJ8RBEp4bCaX9bfHqs4f04Ii_U3IeodHnHL2XKzjdNFv7M1g27Y-Ao-HZVbQJm8d-0PwIDAQAB"}
```

##### 3、error code  
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| C00001  |appId and appKey validation failed|Use the invalid appId to get the public key  |   

#### Verify the public key
>Provide the front-end application with an interface to verify the validity of the local public key.      
 

##### 1、Interface definition

?> **Access address：**  `/uam/v2/mgr/verifyPublicKey`  
 **HTTP Method：** POST  
 **Preconditions:** Get the public key interface 
 **Token authentication：** No 

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|   sn   | String | body | yes | The public key is used to encrypt the timestamp, and the backend service decrypts successfully. The timestamp number is correct.|      


**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |    |     |     |  &emsp;  |  
 

##### 2、Request sample  
**Request address**  
```
https://uws-gea-euro.haieriot.net/uam/v2/mgr/verifyPublicKey
```  

**User request**
```java  
Header：
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT36PQONTJY5UY02L214BESBJT9S0
sign: 2a2aa21216e50ab61f6b846658356a88e827fbba0fadb40bc2c6e7ec647f66d7
timestamp: 1533895007298 
language: en
timezone: +8
appKey: d4tg0ad3ea78cc23fa86c656f2a401d8r
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 180
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


[no cookies]


Body:
{"sn":"V3O5gPCk9JQm_lPiScTeyDXpf-6pIb4Vl0mR9fB7cUocn_RQizg0ica0bJ0-65fJpLolkCNiVZ78jTDfTlj6o_HraGUiIpz-sBp5UZrO6ffBIPr4LhPL1Aew3XNrThIQNlleVKDkLHrHq2hMXxLx9M6BQro_SfrrGdInxk9Fu8Y"}

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
| A00005  |Database exception|verification failed  |    
| B00002  |Parameter verification failed|verification failed  |    

#### Get image verification code
>Obtaining a graphics verification code, unlike the V1 interface, is to increase the limit, and tomorrow 20 requests per application limit.     
 

##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/captcha`  
 **HTTP Method：** POST  
 **Preconditions:** Use a valid appId, and clientId 
 **Token authentication：** No 

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
|    |    |     |     |  &emsp;  |  
      


**Output parameters**  

Content-Type: image/png;charset=UTF-8


##### 2、Request sample  
**Request address**  
```  
https://uws-gea-euro.haieriot.net/uam/v2/user/captcha
```  

**User request**
```java  
[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-TEST-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: cf328601c6a2249f38fc0055b00ff781c4cc357745fe6ff0302e113a810a7c89
timestamp: 1533884947784 
language: en
timezone: +8
appKey: dg11ad3ea78cc19aa86c656f2a401d7e
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 0
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```  

**Request response**

![验证码图片][account_captcha] 


##### 3、error code    
|   errorcode      |     description      | scenario  |  
| ------------- |:----------:|:-----:|  
| C00001  |appId and appKey validation failed|Use invalid appId  |    


###	Apply to cancel account and device information  
> The user applies to delete the account information and device binding relationship, and sends an email notification. After the user successfully applies, the account cannot be logged in, and the login returns the password error  

##### 1、Interface definition

?> **Access address：**  `/uam/v2/user/applyDeleteAccount`  
 **HTTP Method：** POST  
 **Preconditions:** Login and apply for dynamic verification code   
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
| msgCode   | String |Body |Yes|Verification code, the user applies for the verification code to cancel the account, the verification code is sent to the user's mailbox, and the verification code is required before the logout, 6 random numbers. Application type type=5|   
   
 


**Output parameters**  
**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 

##### 2、Request sample  

**User request**
```java  
POST https://uws-gea-euro.haieriot.net/uam/v2/user/applyDeleteAccount

POST data:
{"msgCode":"123333"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-HKQHWBB-0001
appVersion: 2.4.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT19G55PP0TWL812NAZVNWRLPN7P0
sign: 2cb6c701bcad8e141972e44afa58b5b12d920b5303b8429cd3880b9499375d06
timestamp: 1534215683804 
language: en
timezone: +8
appKey: d44625bb0556fb0b1611ad6a073fb6f5
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 20
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

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
| D00022   |  Verification code is incorrect or has expired| User enters wrong verification code  |    



###	Log out v1
> Mobile APP users exit Haier U+ cloud platform interface

##### 1、Interface definition

?> **Access address：**  `/uam/v1/security/logout`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

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
appId:MB-ABC-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
accessToken: TGT1OY0RUUAH5D242SB68E9WX0W930
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
language:zh-cn
timezone:8
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

 

#### Query user information v1
> Get the user information according to the login token     

##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/get`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

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
appId:MB-ABC-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
accessToken: TGT1OY0RUUAH5D242SB68E9WX0W930
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
language:zh-cn
timezone:8
Content-type:application/json

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


 

#### User information modification v1
> Modify the extended properties of the current logged in user according to the token of the logged in person  


##### 1、Interface definition

?> **Access address：**  `/uam/v1/users/update`  
 **HTTP Method：** POST  
 **Token authentication：** Yes  

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
appId:MB-ABC-0000
appVersion:2015110401
clientId:356877020056553-08002700DC94
sequenceId:08002700DC94-15110519074300001
accessToken: TGT1OY0RUUAH5D242SB68E9WX0W930
sign:bd4495183b97e8133aeab2f1916fed41
timestamp:1446639090139
language:zh-cn
timezone:8
Content-type:application/json

Body:
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
 
#### Accept the privacy policy v1
> User accepts the privacy policy and records the privacy policy version number   


##### 1、Interface definition

?> **Access address：**  `/uam /v1/security/acceptUserPrivacy`  
 **HTTP Method：** POST  
 **Preconditions:** Login status  
 **Token authentication：** Yes  

**Input parameters**  

| parameter name        | types          | location  | requierd|description|
| ------------- |:-------------:|:-----:|:-------------:|
| privacyVersion    | String | Body| yes|Privacy Policy Version Number|    


**Output parameters**  

**Output standard output parameters.**

|   name      |     types      | location  |requierd |description|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|    |  | ||&emsp;| 

##### 2、Request sample  

**User request**
```java
  
{"privacyVersion":"V1.0.0"}

[no cookies]

Request Headers:
Connection: keep-alive
appId: MB-HKQHWBB-0001
appVersion: 2.4.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT2CXEYLXJ7EL3225D8DDZJ7BAGZ0
sign: aa97d5db9989d6f4b37659d2eb679c4e873533d280413fa2f04f9dcb2f44279c
timestamp: 1533901887123 
language: en
timezone: +8
appKey: d44625bb0556fb0b1611ad6a073fb6f5
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 27
Host: 10.2.0.16:6353
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

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
| B00004  |  Parameter error|  Wrong privacy clause version number  |   
| D00003 |  Token does not exist, failed to pass the token|  Token does not exist or the wrong token  |   

## Way of use

### Opening process  
![开通流程][account_liucheng]

### Application scenario
**Account management**  
Developers do not have an account system and can integrate U+ account related services.  

**Developer account**  
Developers have their own account system, accessing U+ account services through cloud-connected interconnection.  



## common problem

[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_callingProcess.png
[account_PasswordFlow1]:_media/_account/account_PasswordFlow1.png
[account_PasswordFlow2]:_media/_account/account_PasswordFlow2.png
[account_captcha]:_media/_account/account_captcha.png  


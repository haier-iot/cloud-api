
>  **当前版本**：[UWS 账户服务 V2.1.0](zh-cn/ChangeLog/Account)  
 **更新时间**：{docsify-updated}  

### 简介

> 账号服务旨在提供涵盖物联全流程的访问控制服务，为开发者搭建统一的用户登录系统。  

开发者通过集成U+IOT平台账号服务，不仅提供用户账号的注册、登录、找回密码等账户管理服务，同时帮助开发者构建包括物联设备的统一控制、设备与用户权限管理等在内的一致性的IOT系统化管控模式。

 
**账号基础能力**  

1、	提供包括注册、登录、密码管理等账号基础服务能力；

2、	提供权限认证与管理服务，包括用户认证与用户之间权限的分享认证管理。



**账号信息相关能力**  

1、	查询IOT平台账号信息，请求获取用户信息（包括id、loginName、email、mobile等用户属性）；

2、	修改IOT平台账号信息，用户主动修改其应用属性信息、用户基础属性等，需要进行权限认证。  



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

支持第三方账号认证登录，使用oauth账号授权方式登录U+账户服务,支持google，amazon 等社交账号登录；

第三方账号与U+ 账号实现绑定，并维护绑定关系与U+账号信息



### 用户隐私权限

为切实保护用户隐私权，优化用户体验，海尔优家根据现行法规及政策，制定了海尔家电隐私权政策。海尔了解个人信息对客户的重要性，我们力求明确说明我们获取、管理及保护用户个人信息的政策及措施。

在用户注册、下载更新、登录访问等情况下必须提供隐私权政策的内容或指向所在页面，且需要用户点击表示“同意”隐私权政策，不能太过隐蔽、不能设置默认“同意”；

在获得用户“同意”之后，也要确保用户在使用的过程中可以随时便利查看到隐私权政策全文，不能隐藏起来不展示。

 **开发者需提供使用海尔优家账号服务应用的用户服务协议条款请联系**[**海尔优家商务BD**](zh-cn/Business)，**我们为应用配置对应的隐私权政策及服务协议条款**

#### 用户隐私数据的安全性
##### 安全性说明
用户隐私数据项目 
 
| **字段** | **加密处理**  |  **规则** |   
| ------------- |:----------:|:-----:|  
|email    |	RSA 加密	|默认为用户名@服务器域名，用户名和服务器域名为非空字符|    
|password |	RSA 加密	|长度为6~20位，有大写字母、小写字母、数字或特殊字符中的三种|  
 
##### 秘钥使用流程  

**获取秘钥流程**

![密码传输流程图片][account_PasswordFlow2]  

**秘钥更新流程**

![密码传输流程图片][account_PasswordFlow1]  


##### 加密和解密算法
算法：RSA

秘钥长度:1024

秘钥：app端持有公钥，服务端持有私钥，公钥私钥为一对秘钥，公钥加密，私钥解密

算法代码：


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
		// 为RSA算法创建一个KeyPairGenerator对象
		KeyPairGenerator kpg;
		try {
			kpg = KeyPairGenerator.getInstance(RSA_ALGORITHM);
		} catch (NoSuchAlgorithmException e) {
			throw new IllegalArgumentException("No such algorithm-->["
					+ RSA_ALGORITHM + "]");
		}

		// 初始化KeyPairGenerator对象,密钥长度
		kpg.initialize(keySize);
		// 生成密匙对
		KeyPair keyPair = kpg.generateKeyPair();
		// 得到公钥
		Key publicKey = keyPair.getPublic();
		String publicKeyStr = Base64.encodeBase64URLSafeString(publicKey
				.getEncoded());
		// 得到私钥
		Key privateKey = keyPair.getPrivate();
		String privateKeyStr = Base64.encodeBase64URLSafeString(privateKey
				.getEncoded());
		Map<String, String> keyPairMap = new HashMap<String, String>();
		keyPairMap.put("publicKey", publicKeyStr);
		keyPairMap.put("privateKey", privateKeyStr);

		return keyPairMap;
	}

	/**
	 * 得到公钥
	 * 
	 * @param publicKey
	 *            密钥字符串（经过base64编码）
	 * @throws Exception
	 */
	public static RSAPublicKey getPublicKey(String publicKey)
			throws NoSuchAlgorithmException, InvalidKeySpecException {
		// 通过X509编码的Key指令获得公钥对象
		KeyFactory keyFactory = KeyFactory.getInstance(RSA_ALGORITHM);
		X509EncodedKeySpec x509KeySpec = new X509EncodedKeySpec(
				Base64.decodeBase64(publicKey));
		RSAPublicKey key = (RSAPublicKey) keyFactory
				.generatePublic(x509KeySpec);
		return key;
	}

	/**
	 * 得到私钥
	 * 
	 * @param privateKey
	 *            密钥字符串（经过base64编码）
	 * @throws Exception
	 */
	public static RSAPrivateKey getPrivateKey(String privateKey)
			throws NoSuchAlgorithmException, InvalidKeySpecException {
		// 通过PKCS#8编码的Key指令获得私钥对象
		KeyFactory keyFactory = KeyFactory.getInstance(RSA_ALGORITHM);
		PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(
				Base64.decodeBase64(privateKey));
		RSAPrivateKey key = (RSAPrivateKey) keyFactory
				.generatePrivate(pkcs8KeySpec);
		return key;
	}

	/**
	 * 公钥加密
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
			throw new RuntimeException("加密字符串[" + data + "]时遇到异常", e);
		}
	}

	/**
	 * 私钥解密
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
			throw new RuntimeException("解密字符串[" + data + "]时遇到异常", e);
		}
	}

	/**
	 * 私钥加密
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
			throw new RuntimeException("加密字符串[" + data + "]时遇到异常", e);
		}
	}

	/**
	 * 公钥解密
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
			throw new RuntimeException("解密字符串[" + data + "]时遇到异常", e);
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
			throw new RuntimeException("加解密阀值为[" + maxBlock + "]的数据时发生异常", e);
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
		System.out.println("公钥: \n\r" + publicKey);
		System.out.println("私钥： \n\r" + privateKey);

		String str = "123";
		System.out.println("\r明文：\r\n" + str);
		System.out.println("\r明文大小：\r\n" + str.getBytes().length);
		String encodedData = RSAUtil.publicEncrypt(str,
				RSAUtil.getPublicKey(publicKey));
		System.out.println("密文：\r\n" + encodedData);
		String decodedData = RSAUtil.privateDecrypt(encodedData,
				RSAUtil.getPrivateKey(privateKey));
		System.out.println("解密后文字: \r\n" + decodedData);

	}
}

```  
#### 用户隐私协议

##### 用户隐私协议版本

版本：V1.0.0，大写V

##### 查看用户隐私协议内容

`Https://uws-euro.haieriot.net/userweb/agreement?v=v1.0.0&lg={language}`

lg参数参考附录章节国际语言代码表[access specification](en-zh/AccessSpecification).
  
### 国际化处理

对接口响应返回的retCode和retInfo不做国际化处理，由接口调用方处理。

对于接口涉及业务数据的国际化通过在header中传递language参数来定义，具体的国际化语言代码见[access specification](en-zh/AccessSpecification)。

### 语言模板的使用

#### 邮件内容

注册，激活，重置密码时根据header中的据语言类型（language字段）发送对应语言的邮件内容。

#### 对海外oem版本app的支持
  
请求头（header）中的appId为oem类型，注册、激活、重置密码的邮件内容使用oem模板。
OEM APPID 限定为MB-OEM-0000,MB-OEM-0001




### 接口列表


#### 用户注册
> 使用邮箱地址注册新账号


##### 1、接口定义

?> **接入地址：**  `/uam/v2/user/registerEmailAcounnt`  
 **HTTP Method：** POST  
 **前置条件:** 获取验证码   </br>
 **Token 验证：** 否  

**输入参数**  

| 参数名        | 类型         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|email	|String	|Body|	是|	需使用公钥加密，后端服务解密并校验规则|  
|password|String|Body|是|	密码需使用公钥加密，后端服务解密并校验规则。</br>服务端校验规则：大小写字母、数字、特殊字符三种或三种以上</br> `^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])[a-zA-Z0-9]{6,20}$`|  
|captcha|	String|	Body|	Yes	|图形验证码，4位字母和数字组合。每个验证码只能使用一次，使用后或过期即作废，需重新获取。</br>根据需求msgCode验证失败超过三次需强制用户输入图形验证码|
|userProfile|Map|Body|否|添加用于满足各不同应用对用户信息的不同需求。当应用需要扩展用户属性时，可以向云平台用户系统申请，申请时列明需要扩展的属性，并列明每个属性对应的key、类型及长度。|
|msgCode|String|	Body|	是	| 验证码,注册前用户申请验证,发送至用户邮箱，注册时需填写此验证码，6位随机数字|

**输入参数**  标准输出参数



##### 2、请求样例  

**用户请求**  
```java
POST

https://uws-euro.haieriot.net/uam/v2/user/registerEmailAcounnt

Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT20QGFDOIY0U8S2754W0ZH3XY390
sign: 5a46d3ca2a7f4589e97fbf4f2eae4b74c56eeb685884d91f136d339ea523dd0a
timestamp: 1533868371182 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 434
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


Body:
{
	"email": "mzyc5-J0Cucq84wqFh7KfkWWqd3P3EagdvW2Eb8f6fqoQ3oX1Llhdt2o_YRpnu0D6xLUeocU7ckagnr5YlpNwh2OlVO6SKUNsmp9sXetFpjd9riOFeaJRqGeta8oPDMqPOnTIGt-9XaZ4nr5v2zH44eNalPSwL1kyUykVdHjbrU",
	"password": "ZrZjvu0dpDNqKoQDUHCnyPyNw1gpvy6_b3BoVRQnpPW4Gj31Ieyr8B0DkbiayEWV2x5slwqvf4HU_b-ZF_NdMC-V_OQ5VZxZixqmH-piZ8uAMzmZaiVf5Hxn26g6w1x679Oma2xiEnRdm2YpsVKhzwHiBn0-uZxNQnUxLZ9YI6k",
	"msgCode": "050289",
	"userProfile": 
	{
		"name": "test"
	},
	"captcha": "dmpp"
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

>  D00012、D00015、D00022、D00009、B00010、B00004
   

 

#### 邮箱登录
> 用户使用邮箱登录获取accessToken
  

##### 1、接口定义

?> **接入地址：**  `/uam/v2/user/loginEmailAcounnt`  </br>
 **HTTP Method：** POST  </br>
 **前置条件:** 用户使用邮箱注册账号 </br>
 **Token 验证：** 否  </br>

**输入参数**  

| 参数名       | 类型        | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| email    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| password     | String | Body| 是|需使用公钥加密，后端服务解密并校验规则 |  
| captcha     | String | Body| no |图形验证码，4位字母和数字组合。每个验证码只能使用一次，使用后或过期即作废，需重新获取。登录输入错误的密码，次数大于等于三次时必须输入图形验证码。当用户输入错误密码5次时,锁定账号5小时|  


**输出参数**  

|   参数名      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------|
| accessToken   |   String |  Body   |  是   |   安全令牌  |  
| refreshToken   |   String |  Body   |  是   |   刷新令牌 |  
| scope   |   String |  Body   |  是   |   访问资源的范围 |  
| expire   |   String |  Body   |  是   |   有效期 |  

##### 2、请求样例 

**用户请求**  
```java
POST

https://uws-euro.haieriot.net/uam/v2/user/loginEmailAcounnt
 
Header：  
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTWCHUYVYMTS7L2QMJ5L6K8ERHI00
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


Body:
{
	"email": "cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
	"password": "hPeBKqC3OH8uG1yDM3l2CEk6zmvAzUonBxIcR6Z40IIPV8U20A_26nXX7wMn39kL5pJ-Irybmua9YUMYgmfNFZoZIyQzP6UTznuupqwZrVMr9vDH_4rLRzlcno8rBgxvWL6EUwL_ivK-Bwx6pOqIawjVTGoxGX8OSzwHcKK7Dvw",
	"captcha": "kq6g"
}


```  

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":null,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}
```

##### 3、错误码  

> 10000、00001、D00002、D00009、D00010、D00015、B00010
 

#### 获取验证码
> 在注册前申请验证码，用于验证用户的真实邮箱

##### 1、接口描述

?> **接入地址：**  `/uam/v2/user/applyVerificationCode`  
 **HTTP Method：** POST  
 **前置条件:** 注册  
 **Token 验证：** 否

**输入参数**  

|  参数名        | 类型      | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| email    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| type    | String | Body| 是|1: 注册  2: 找回密码  5: 注销账号|  
   

**输出参数：** 标准输出参数  


##### 2、请求样例   
**用户请求**
```java
POST

https://uws-euro.haieriot.net/uam/v2/user/applyVerificationCode

Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: 0cdb49b20d552b6f99384373b1e7f3c34136237b7a39d91ab63b35035f54f8d0
timestamp: 1533886290915 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 194
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

Body:
{
	"email": "Vfg5EFcuBWBMhs7MkSfWkXmvNaE0uuUcF6abMJ1NrrpaWbNwD9qVndJABZeHm0OJL4Lndxih1HMuB16lWWPSupFybylkgl-ztjjKhoPc2K7QCLV0n2i73ei2LhTLIZdQ_VsPUhtM9jET50MFNRDAUpnBHwQwrj2JUDi0SAztMbg",
	"type": "2"
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

> D00017、D00012、B00010
  
 

#### 使用邮箱重置密码
> 重置密码，需要先申请验证码
 
##### 1、接口定义

?> **接入地址：**  `/uam/v2/user/resetPassword`  
 **HTTP Method：** POST  
 **前置条件:** 用户注册，获取验证码  </br>
 **Token 验证：** 否  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|email	|String	|Body|	是|	需使用公钥加密，后端服务解密并校验规则|  
|password|String|Body|是|新密码：需使用公钥加密。后端服务解密并校验规则，|  
|captcha|	String|	Body|	否	|图形验证码，4位字母和数字组合。每个验证码只能使用一次，使用后或过期即作废，需重新获取。对于同一App、同一手机终端邮箱验证码连续校验失败3次后需启用图形验证码进行验证，3次可配置，默认为3次。邮箱验证码校验成功后，允许失败次数重新恢复为0|  
|msgCode|String|Body|是|验证码,用户申请验证码用于重置密码,发送至用户邮箱，注册时需填写此验证码，6位随机数字|  
   


**输出参数：**标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws-euro.haieriot.net/uam/v2/user/resetPassword
 
Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: 2e997f503323fcbabfab0bf5f54da2a3bdecc60a6924519b7c90d9b20e0b62dd
timestamp: 1533886628775 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 404
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

Body:
{
	"email": "qra5hNX9V5c57aj4mbjaxqnfcWbCBXncyNocZLvWfWRsMc1mJ2rnqUpELkgJ8qHJbKMkQgNUnDpvGN9Sj24LbvcL4a9UosZtpLRF4ZSGSiuXbpL75U0mSrkQvB8QulmW_tk9rLZP_cRPkImI_OS0ar24ZkO1yNnAA7y0XgsFXZs",
	"password": "FRR_c9JhWIL5JCtZ0-lD54MbnRHxzLtdejp_45E72iWC5ARhL36R_R3XiQqpDL8LqvM11seGx86W59T8Topq_54V4z_4lTsVod7N_VuHSRsFNQLwlu3Hnaat1_OK6HWN5hhk1DPhXcAgsj8JTn8UhPAONHdPROgtr7vDpIPgxc0",
	"msgCode": "632438",
	"captcha": "9mf2"
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

> D00017、B00010、D00009、D00022
  

#### 修改密码 
> 用户在登录状态可修改密码


##### 1、接口定义

?> **接入地址：**  `/uam/v2/user/changePassword`  
 **HTTP Method：** POST  
 **前置条件:** 邮箱登录  
 **Token验证：** 是  

**输入参数**  

|  参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|  
|email	|String	|Body|	是|	旧密码：需使用公钥加密。长后端服务解密并校验规则 |  
|newPassword|String|Body|是|	新密码：需使用公钥加密。长后端服务解密并校验规则|  
|captcha|	String|	Body|	是	|图形验证码，对于同一App、同一手机终端修改密码时连续校验失败m次后需启用图形验证码进行验证，m可配置，默认为3次。</br>在启用验证码后，如果图形验证码输入正确的情况下，同一天对同一账户的错误尝试达到n次后将对应的账户锁定，n可配置，默认为10次。|  
     



**输出参数：**  

标准输出参数  


##### 2、请求样例  

**用户请求**  

```java
https://uws-euro.haieriot.net/uam/v2/user/changePassword

Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTWCHUYVYMTS7L2QMJ5L6K8ERHI00
sign: 52b96f85e392ab0172d98f3ac1d7a5f7a284c387d4eeb1ea0d428df73aa55fe7
timestamp: 1533880044559 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 391
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

Body:
{
	"password":"M86jq-ZxErDwBcy_p2gztuBKVbk_nlrHTvePtQP2YGqzXW5q6zbY-a5sV7rltbLD8Kl4Jc8O6emWz3zEOeANJfVQkIbjq2wPc4XEeHls3AwVqnDR6GB2ncljRvX-ZlD2UHcM4AzuJk3I-stOYLnbTPVoDTSxbyNfxEREXq2e2ZI",
	"newPassword":"MrGIHA_edrlvBIvxYSn5Kstp7pj89ccQ_UFYQNVHQPuulIfJ1QRoEoi-Oq8mpYX-58BU2eglmfPcV8bpQRXH9ILRXbiVLaU0ZhCStkv68l9Q_GuSYrjEISAKK62naHwbMQrzb_seUnmQsGtCwrJx8WtQdnm6Gcz7aGZ7DGWzLC8",
	"captcha":"73ll"
}
```  

**请求应答**

```java
{
  “retCode”: “00000”,
  “retInfo”: “成功”,
}
```

##### 3、error code  

> D00003、D00002、B00004、 

#### 获取公钥
> 获取公钥
  

##### 1、接口定义

?> **接入地址：**  `/uam/v2/mgr/getPublicKey`  
 **HTTP Method：** POST  
 **前置条件:** 使用有效的appid  </br>
 **Token 验证：** 否 

**输入参数：** 

标准输入参数  


**输出参数：** 输出公钥 publicKey 


##### 2、请求样例   
**用户请求**  
```java
POST

https://uws-euro.haieriot.net/uam/v2/mgr/getPublicKey

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT36PQONTJY5UY02L214BESBJT9S0
sign: 9972aec958ec085bf5305f81ff135b933def3bde65fdb59cb12bc6caa0ccb17a
timestamp: 1533894864095 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 0
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```  

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"publicKey":"MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC7jrpGqqsUERuS3RhjKGoKSFaoUyfk5eKwjxSq3ARW5sMg8hsnuC0dmLEOrUoaJYSoaLOn7V1uvpX4PYVJ89BjnDJtjbFoYAsk3VMVsTiJ8RBEp4bCaX9bfHqs4f04Ii_U3IeodHnHL2XKzjdNFv7M1g27Y-Ao-HZVbQJm8d-0PwIDAQAB"
}
```

##### 3、错误码

> C00001

#### 验证公钥
> 给前端应用提供本地公钥合法性的接口  
 

##### 1、接口定义

?> **接入地址：**  `/uam/v2/mgr/verifyPublicKey`  
 **HTTP Method：** POST  
 **前置条件:** 获取公钥  </br>
 **Token 验证：** 否 

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|   sn   | String | body | 是 | 需使用公钥加密时间戳，后端服务解密成功，验证为时间戳数字即正确|      


**输出参数：** 标准输出参数  
 

##### 2、请求样例   
**用户请求**
```java  
POST

https://uws-euro.haieriot.net/uam/v2/mgr/verifyPublicKey

Header：
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGT36PQONTJY5UY02L214BESBJT9S0
sign: 2a2aa21216e50ab61f6b846658356a88e827fbba0fadb40bc2c6e7ec647f66d7
timestamp: 1533895007298 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 180
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

Body:
	{
	"sn":"V3O5gPCk9JQm_lPiScTeyDXpf-6pIb4Vl0mR9fB7cUocn_RQizg0ica0bJ0-65fJpLolkCNiVZ78jTDfTlj6o_HraGUiIpz-sBp5UZrO6ffBIPr4LhPL1Aew3XNrThIQNlleVKDkLHrHq2hMXxLx9M6BQro_SfrrGdInxk9Fu8Y"
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

> A00005、B00002    

#### 获取图形验证码
> 获取图形验证，每天每个终端20次请求限制。

##### 1、接口定义 

?> **接入地址：**  `/uam/v2/user/captcha`  
 **HTTP Method：** POST  
 **前置条件:** 使用有效的appId、clientId  </br>
 **Token 验证：** 否 

**输入参数：** 

标准输入参数    
     


**输出参数：**  

Content-Type: image/png;charset=UTF-8


##### 2、 请求样例  

**用户请求**
```java  
POST

https://uws-euro.haieriot.net/uam/v2/user/captcha

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
accessToken: TGTNS633MLE2OHV2P03YB3Q6E44K00
sign: cf328601c6a2249f38fc0055b00ff781c4cc357745fe6ff0302e113a810a7c89
timestamp: 1533884947784 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 0
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

```  

**请求应答**

![验证码图片][account_captcha] 


##### 3、错误码   

> C00001


####  申请注销账号和设备信息
> 用户申请删除账号信息和设备绑定关系，并发邮件通知，用户申请成功后，账号不能登录，登录返回密码错误 

##### 1、接口定义

?> **接入地址：**  `/uam/v2/user/applyDeleteAccount`  
 **HTTP Method：** POST  
 **前置条件:** 用户登录，申请动态验证码  </br>
 **Token 验证：** 是  

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| msgCode   | String |Body |是|验证码,用户申请验证码用于注销账号,验证码发送至用户邮箱，注销前需验证验证码，6位随机数字。申请类型type=5|   
   
 
**输出参数：**   

标准输出参数  


##### 2、请求样例  

**用户请求**
```java
POST

POST https://uws-euro.haieriot.net/uam/v2/user/applyDeleteAccount

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 2.4.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT19G55PP0TWL812NAZVNWRLPN7P0
sign: 2cb6c701bcad8e141972e44afa58b5b12d920b5303b8429cd3880b9499375d06
timestamp: 1534215683804 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 20
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


POST data:
{
	"msgCode":"123333"
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

> D00022


####	退出登录 V1
> 账号退出登录，会话accessToken失效

##### 1、接口定义

?> **接入地址 ：**  `/uam/v1/security/logout`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数：** 

标准输入参数  
   
 


**输出参数：** 

标准输出参数 


##### 2、请求样例   

**用户请求**

```java 
POST  

https://uws-euro.haieriot.net/uam/v1/security/logout
 
Header：
appId:MB-****-0000
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

**请求应答**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}
```

##### 3、错误码  

> D00005、D00016  

 

#### 查询用户信息V2
> 根据登录者token，获取用户信息     

##### 1、接口定义

?> **接入地址：**  `/uam/v2/users/get`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数：** 

标准输入参数  



**输出参数：**  

|   参数名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------|
userId|String|Body|是|账号唯一标示
email|String|Body|是|账号邮箱地址，按需求进行脱敏处理
| userProfile     | Map | Body| 否|用户扩展信息,包括昵称、头像等。|  


##### 2、请求样例  

**用户请求**
```java  
POST  

https://uws-euro.haieriot.net/uam/v1/users/get

Header：
appId:MB-****-0000
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


 

#### 用户信息修改 V1
> 根据登录人员token，修改当前登录用户的拓展属性


##### 1、接口定义

?> **接入地址 ：**  `/uam/v1/users/update`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| userProfile     | Map | Body| 是|用户扩展信息,包括昵称、头像等|    


**输出参数：** 标准输出参数  

##### 2、请求样例

**用户请求**
```java  
POST  

https://uws-euro.haieriot.net/uam/v1/users/update

Header：
appId:MB-****-0000
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


#### 接受隐私条款V1
> 用户接受隐私条款，记录隐私条款版本号 

##### 1、接口定义

?> **接入地址：**  `/uam /v1/security/acceptUserPrivacy`  
 **HTTP Method：** POST  
 **前置条件:** 登录状态  </br>
 **Token 验证：** 是  

**输入参数**  

| 参数名        | 类型          | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| privacyVersion    | String | Body| 是|隐私条款版本号|    


**输出参数：** 

标准输出参数  

##### 2、请求样例
  
**用户请求**
```java
POST  

https://uws-euro.haieriot.net/uam/v1/security/acceptUserPrivacy

Request Headers:
Connection: keep-alive
appId: MB-****-0001
appVersion: 2.4.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT2CXEYLXJ7EL3225D8DDZJ7BAGZ0
sign: aa97d5db9989d6f4b37659d2eb679c4e873533d280413fa2f04f9dcb2f44279c
timestamp: 1533901887123 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 27
User-Agent: Apache-HttpClient/4.2.6 (java 1.5) 

POST Data：
{
	"privacyVersion":"V1.0.0"
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

> B00004、D00003

#### 会话刷新

> accessToken过期后，可以使用对应的refreshToken获取新的accessToken

##### 1、接口定义
?>**接入地址：**`/uam/v2/auth/token` </br>
**HTTP Method:** POST  </br>
**前置条件：**使用邮箱登录获取refreshToken   </br>
**Token 验证：** 否

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
refreshToken|String|Body|是|app端持有refreshToken，用于会话accessToken延期，延期会话会生成新的refreshToken和accessToken
grantType|String|Body|是|授权方式，当前默认为refresh_token

**输出参数：**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
accessToken|String|Body|是|安全令牌
refershToken|String|Body|是|刷新令牌
scope|String|Body|是|访问资源的范围
expire|String|Body|是|有效期，单位秒

##### 2、请求样例

**用户请求**
```java
POST  

https://uws-euro.haieriot.net/uam/v2/auth/token

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

POST data:
{
	"refreshToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000",
	"grantType":"refresh_token"
}
```
**请求应答**
```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken": TGTV5FR3XH20S0B2E7G56V1CMQ4T67,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}

```
##### 3、错误码

> D00005、D00025、D00005


#### 获取会话分享验证码
> 通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程

##### 1、接口定义
?>**接入地址：**`/uam/v2/auth/shareCode`  </br>
**HTTP Method:** POST		</br>
**前置条件：** 登录app，可获取其他终端应用的appId和clientId		</br>
**Token 验证：** 是

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
shareAppId|String|Body|是|请求分享会话的appId
shareClientId|String|Body|是|请求分享的clientId
accessToken|String|Body|是|请求分享会话的accessToken


**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
code|String|Body|是|会话分享验证码

##### 2、请求样例
**用户请求**
```java

POST 

https://uws-euro.haieriot.net/uam/v2/auth/shareCode

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

POST data:
{
	"accessToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000",
	"shareClientId":"MB-TEST2-0000",
	"shareClientId":"456FEW334DD" 
}

```
**输出明细**
```java
{"
	retCode":"00000",
	"retInfo":"成功",
	"code": 72f7b235dd3afee2c77907d160c66539850b3224da60cb6e6638809005f48ec5"
}

```


#### 会话分享

> 通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程

##### 1、接口定义
?>**接入地址：**`/uam/v2/auth/sharToken`  </br>
**HTTP Method:** POST		</br>
**前置条件：** 获取会发分享的验证码    </br>
**Token 验证：** 否

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
code|String|Body|是|会话分享验证码

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
accessToken|String|Body|是|安全令牌
refreshToken|String|Body|是|刷新令牌
scope|String|Body|是|访问资源的范围
expire|String|Body|是|有效期，单位秒

##### 2、代码样例
**用户请求**
```java
POST 

https://uws-euro.haieriot.net/uam/v2/auth/shareToken

POST data: 
{
	"code":" da48b7de0a9bd0639b43fc40948176821784d3c01276870cceccf0b6564624e7 " 
}


Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 456FEW334DD
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)
```
**请求应答**
```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":"TGTV5FR3XH20S0B2E7G56V1CMQ4T67",
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}
```
##### 3、错误码

> B00004、00001、B00001、B00002


#### 第三方社交账号获取IOT平台token

> 用户使用第三方社交账号登录海尔app，通过第三方账号token获取海尔iot平台accessToken

##### 1、接口定义
?>**接入地址：**`/uam/v1/thirdpart/social/getAccessToken`  </br>
**HTTP Method:** POST  </br>
**前置条件：** APP获取第三方社交账号的token   </br>
**Token 验证：** 否   </br>

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
thirdpartAccessToken|String|Body|是|第三方社交账号的token，通过第三方社交账号SDK获取的票据
socialType|String|Body|是|类别：facebook，twitter，amazon
thirdpartOpenId|String|Body|否|当前接入Facebook，Twitter，Amazon需传openId，此参数非必填定义兼容后期无openId 的模式

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
accessToken|String|Body|是|安全令牌
scope|String|Body|是|访问资源的范围
expire|String|Body|是|有效期，单位秒

##### 2、请求样例

**用户请求**
```java
POST 

https://uws-euro.haieriot.net/uam/v1/thirdpart/social/getAccessToken

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

POST data:
{
	"thirdpartAccessToken":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
	"socialType":"amazon","
	thirdpartOpenId":"110347635"
}

```

**请求应答**

```java
{
	"retCode":"00000",
	"retInfo":"成功",
	"refreshToken":null,
	"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
	"scope":"auth_app",
	"expire":"2160000"
}
```

##### 3、错误码

> 00001、B00010、A00006

#### 第三方社交账号绑定IOT自有账号

> 用户使用自有账号登录iot平台，成功后登录第三方账号，与自有账号绑定成组；原第三方账号暗账号设备关系拷贝至自有账号下

##### 1、接口定义 
?>**接入地址：**`/uam/v1/thirdpart/social/bindGroup` </br>
**HTTP Method:** POST </br>
**前置条件：** APP支持自有账号登录，和支持第三方账号登录 </br>
**Token 验证：** 是

**输入参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
uhomeAccessToken|String|Body|是|自有账号登录的IOT平台accessToken
thirdpartUhomeAccessToken|String|Body|是|第三方账号登录的iot平台token

**输出参数** 

标准输出参数

##### 2、请求样例
**用户请求**
```java
POST 

https://uws-euro.haieriot.net/uam/v1/thirdpart/social/bindGroup

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)

POST data:
{
	"thirdpartAccessToken":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
	"accessToken":"TGTUE8J3JHIDF12WEWHRH0912300"
}
```
**输出结果**
```java
{
	"retCode":"00000",
	"retInfo":"成功"
}
```

##### 3、错误码

> D00004、A00005、D00024


#### 查询账号绑定成组管理

> 查询自有账号下绑定的全部第三方账号的信息，包括扩展信息

##### 1、接口定义
?>**接入地址：**`/uam/v1/thirdpart/social/getBindingGroup` </br>
**HTTP Method:** POST  </br>
**前置条件：** APP登录  </br>
**Token 验证：** 是

**输入参数：**

标准输入参数  

**输出参数**

参数名|类型|位置|必填|说明
:-:|:-:|:-:|:-:|:-
group|String|Body|是|详见代码样例

##### 2、请求样例

**用户请求**
```java
POST 

https://uws-euro.haieriot.net/uam/v1/thirdpart/social/getBindingGroup

Request Headers:
Connection: keep-alive
appId: MB-****-0000
appVersion: 2.4.0
clientId: 123
sequenceId: 20161020153428000015
sign: da55be21096d188394c39dd307e7ce7aa3e4c5c38f9f171da39d3a151d0595bb
timestamp: 1533882163013 
language: en
timezone: +8
Content-Encoding: utf-8
Content-type: application/json
privacyVersion: V1.0.0
Content-Length: 385
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)
```

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    " groups": 
	[
        {
            "userId": "1234",
            "openId": "11098764",
            "socialType":amazon"
        },
        {
            "userId": "4567",
            "openId": "112098764",
            " socialType ":amazon"
        }
    ]
}
```

##### 3、错误码

> B00004、A00005




[^-^]:文本连接注释
[account_document_url]:_document/_account/GEAProjectInterfaceDefinitionSpecification.docx

[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/account_callingProcess.png
[account_PasswordFlow1]:_media/_account/account_PasswordFlow1.png
[account_PasswordFlow2]:_media/_account/account_PasswordFlow2.png
[account_captcha]:_media/_account/account_captcha.png  
[scene_flow]:_media/_account/scene_flow.png  


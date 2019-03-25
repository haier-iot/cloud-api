
>  **当前版本**：[UWS 账户服务 V2.1.0](zh-cn/ChangeLog/Account)  
 **更新时间**：{docsify-updated}  


## 简介

> 账号服务旨在提供涵盖物联全流程的访问控制服务，为开发者搭建统一的用户登录系统。  

开发者通过集成U+IOT平台账号服务，不仅提供用户账号的注册、登录、找回密码等账户管理服务，同时帮助开发者构建包括物联设备的统一控制、设备与用户权限管理等在内的一致性的IOT系统化管控模式。  
 
**账号基础能力**  

1、	提供包括注册、登录、密码管理等账号基础服务能力；

2、	提供权限认证与管理服务，包括用户认证与用户之间权限的分享认证管理。

**账号信息相关能力**  

1、	查询IOT平台账号信息，请求获取用户信息（包括id、loginName、email、mobile等用户属性）；

2、	修改IOT平台账号信息，用户主动修改其应用属性信息、用户基础属性等，需要进行权限认证。

![账户图片][account_type]  



## 应用场景  

**账户服务场景应用流程：**

账号服务应用流程包括用户注册、登录、密码找回、会话分享、第三方社交账号、账号注销以及用户信息修改等相关流程。


![服务流程图片][account_liucheng] 

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



## 用户隐私权限  

为切实保护用户隐私权，优化用户体验，海尔优家根据现行法规及政策，制定了海尔家电隐私权政策。海尔了解个人信息对客户的重要性，我们力求明确说明我们获取、管理及保护用户个人信息的政策及措施。

在用户注册、下载更新、登录访问等情况下必须提供隐私权政策的内容或指向所在页面，且需要用户点击表示“同意”隐私权政策，不能太过隐蔽、不能设置默认“同意”；
在获得用户“同意”之后，也要确保用户在使用的过程中可以随时便利查看到隐私权政策全文，不能隐藏起来不展示。

 **开发者需提供使用海尔优家账号服务应用的用户服务协议条款请联系**[**海尔优家商务BD**](zh-cn/Business)，**我们为应用配置对应的隐私权政策及服务协议条款**

### 用户隐私数据的安全性
#### 安全性说明
用户隐私数据项目 
 
| **字段** | **加密处理**  |  **规则** |   
| ------------- |:----------:|:-----:|  
|email    |	RSA 加密	|默认为用户名@服务器域名，用户名和服务器域名为非空字符|
|mobile   |RSA 加密	|1开头11位数字|    
|password |	RSA 加密	|长度为6~20位，有大写字母、小写字母、数字或特殊字符中的三种|  
 
#### 秘钥使用流程  

**获取秘钥流程**

![密码传输流程图片][account_PasswordFlow2]  

**秘钥更新流程**

![密码传输流程图片][account_PasswordFlow1]  


#### 加密和解密算法
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


## 接口清单

### 注册登录

#### 邮箱账号注册
> 使用邮箱地址注册新账号


##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/registerEmailAcounnt`  
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

https://uws.haier.net/uaccount/v2/user/registerEmailAcounnt

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
   
#### 手机账号注册
> 使用手机号注册海尔自有账号


##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/registerMobileAcounnt`  
 **HTTP Method：** POST  
 **前置条件:** 获取验证码   </br>
 **Token 验证：** 否  

**输入参数**  

| 参数名        | 类型         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|mobile	|String	|Body|	是|需使用公钥加密，后端服务解密并校验规则|  
|password|String|Body|是|	密码需使用公钥加密，后端服务解密并校验规则。</br>服务端校验规则：大小写字母、数字、特殊字符三种或三种以上</br> `^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])[a-zA-Z0-9]{6,20}$`|  
|captcha|	String|	Body|	Yes	|图形验证码，对于同一App、同一手机终端请求接口时连续失败m次后需启用图形验证码进行验证，m可配置，默认为3次。|
|userProfile|Map|Body|否|添加用于满足各不同应用对用户信息的不同需求。当应用需要扩展用户属性时，可以向云平台用户系统申请，申请时列明需要扩展的属性，并列明每个属性对应的key、类型及长度。|
|msgCode|String|	Body|	是	| 验证码,用户申请的验证码以短信方式发送至用户手机，6位随机数字|

**输入参数**  标准输出参数



##### 2、请求样例  

**用户请求**  
```java
POST

https://uws.haier.net/uaccount/v2/user/applySmsCode

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
{"mobile":"mPuLONXZXYQuGHg0yM+L7PQAJRhEMgYiEseSlaVQow7FwuVjKQl08+A9ukmH QW/twZmxivfy2ELrC2vmKO7jjbp8R6GK1T//rW4O6u63uK+wvT3GC4qhzZRA TexLDpnILpE0KE2ddPz0E1m2DMAwHHA27/J8p85FaBMn66RP03s=","password":"pahJ6YE+zbBnqDBax+6r0xnBwvrIeMjhPoYWDfvmTFMr05x0p1fTY1a9sWBw oCARnR3MbPZvcWDcCAhxRAt9w+8Ei18ceQN1fbgKNcb/uRcT8eGPSsCt9SK8 Iv/cJB5dkw3kfwbTZJQsAEA7E2Q0SFa/d5NQc7trEDsZIxJz5Jo=","msgCode":"630326","userProfile":{"name":"test"},"captcha":"ekq6"}

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


#### 邮箱账号登录
> 用户使用邮箱登录获取accessToken
  

##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/loginEmailAcounnt`  </br>
 **HTTP Method：** POST  </br>
 **前置条件:** 用户使用邮箱注册账号 </br>
 **Token 验证：** 否  </br>

**输入参数**  

| 参数名       | 类型        | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| email    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| password     | String | Body| 是|需使用公钥加密，后端服务解密并校验规则 |  
| captcha     | String | Body| no |图形验证码，4位字母和数字组合。每个验证码只能使用一次，使用后或过期即作废，需重新获取。登录输入错误的密码，次数大于等于三次时必须输入图形验证码。当用户输入错误密码5次时,锁定账号5小时|  


**输出参数 **  

|   参数名      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------|
| accessToken   |   String |  Body   |  是   |   安全令牌  |  
| refreshToken   |   String |  Body   |  是   |   刷新令牌 |  
| scope   |   String |  Body   |  是   |   访问资源的范围 |  
| expire   |   String |  Body   |  是   |   有效期 ，单位秒 |  

##### 2、请求样例 

**用户请求**  
```java
POST

https://uws.haier.net/uaccount/v2/user/loginEmailAcounnt
 
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
{"retCode":"00000","retInfo":"成功",
 "refreshToken": TGTNS588MLE1OHV2P04BB3Q6E54K35,"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
 "scope":"auth_app",
 "expire":"2160000"
}

```

##### 3、错误码  

> 10000、00001、D00002、D00009、D00010、D00015、B00010

#### 手机账号登录
> 用户使用手机登录获取accessToken
  

##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/loginMobileAcounnt`  </br>
 **HTTP Method：** POST  </br>
 **前置条件:** 用户使用手机注册账号 </br>
 **Token 验证：** 否  </br>

**输入参数**  

| 参数名       | 类型        | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
| mobile    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| password     | String | Body| 是|需使用公钥加密，后端服务解密并校验规则 |  
| captcha     | String | Body| no |对于同一App、同一手机终端登录时连续校验失败m次后需启用图形验证码进行验证，m可配置，默认为3次。登录成功后，允许失败次数重新恢复为m次；对于同一App、同一手机终端登录时连续校验失败m次后需启用图形验证码进行验证，m可配置，默认为3次。登录成功后，允许失败次数重新恢复为m次|  


**输出参数 **  

|   参数名      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------|
| accessToken   |   String |  Body   |  是   |   安全令牌  |  
| refreshToken   |   String |  Body   |  是   |   刷新令牌 |  
| scope   |   String |  Body   |  是   |   访问资源的范围 |  
| expire   |   String |  Body   |  是   |   有效期 ，单位秒 |  

##### 2、请求样例 

**用户请求**  
```java
POST

https://uws.haier.net/uaccount/v2/user/loginMobileAcounnt
 
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
{"mobile":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4","password":"hPeBKqC3OH8uG1yDM3l2CEk6zmvAzUonBxIcR6Z40IIPV8U20A_26nXX7wMn39kL5pJ-Irybmua9YUMYgmfNFZoZIyQzP6UTznuupqwZrVMr9vDH_4rLRzlcno8rBgxvWL6EUwL_ivK-Bwx6pOqIawjVTGoxGX8OSzwHcKK7Dvw","captcha":"kq6g"}

```  

**请求应答**

```java
{"retCode":"00000","retInfo":"成功",
 "refreshToken": TGTNS588MLE1OHV2P04BB3Q6E54K35,"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
 "scope":"auth_app",
 "expire":"2160000"
}

```

##### 3、错误码  

> 10000、00001、D00002、D00009、D00010、D00015、B00010  



#### 获取邮箱验证码
> 在注册前申请验证码，用于验证用户的真实邮箱

##### 1、接口描述

?> **接入地址：**  `/uaccount/v2/user/applyVerificationCode`  
 **HTTP Method：** POST  
 **前置条件:** 邮箱注册  
 **Token 验证：** 否

**输入参数**  

|  参数名        | 类型      | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| email    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| type    | String | Body| 是|1：邮箱账号注册 2：利用邮箱找回密码 4：修改登录的邮箱 5：注销邮箱账号|  
   

**输出参数：** 标准输出参数  


##### 2、请求样例   
**用户请求**
```java
POST

https://uws.haier.net/uaccount/v2/user/applyVerificationCode

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

> B00004、B00010
  


#### 获取手机短信验证码
> 在注册前申请验证码，用于验证用户的真实手机号 

##### 1、接口描述

?> **接入地址：**  `/uaccount/v2/user/applySmsCode`  
 **HTTP Method：** POST  
 **前置条件:** 手机号码注册  
 **Token 验证：** 否

**输入参数**  

|  参数名        | 类型      | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----:|
| mobile    | String | Body| 是|需使用公钥加密，后端服务解密并校验规则|  
| type    | String | Body| 是|1：邮箱账号注册 2：利用邮箱找回密码 4：修改登录的邮箱 5：注销邮箱账号|  
   

**输出参数：** 标准输出参数  


##### 2、请求样例   
**用户请求**
```java
POST

https://uws.haier.net/uaccount/v2/user/applySmsCode

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
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)码

Body:
{
"mobile":"U49hyTXZzJ5PlIRBrwSSb3Y0LAdue2cXlJCGGaynQjLxJDOQM+qgz7srmEbf riFteVzUtNtC1fSwrrf6GCePfrQyYadK6SrescjlMoONIuedHB+cgznTXj9j g5L0QboWgPHMQRXgTGXIC1pSDyRYdQQEODu86PUMgbLy0EBmP2c=",
"type":"1"
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

> B00004、B00010 

#### 获取图形验证码
> 获取图形验证码，与V1接口不同的是增加限制，明天每个终端20次请求限制
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/captcha`  
 **HTTP Method：** POST  
 **Token 验证：** 否  

**输入参数**  无输入参数

**输出参数：** 
Content-Type: image/png;charset=UTF-8 

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/user/captcha
 
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

```  

**请求应答**

![验证码图片][account_captcha] 

##### 3、错误码

> C00001   

#### 退出登录
> 账号退出登录，会话accessToken失效  
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/security/logout`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  无输入参数


**输出参数：** 标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/security/logout
 
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
    
### 账号信息管理

#### 使用邮箱重置密码
> 重置密码，需要先申请验证码
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/resetPassword`  
 **HTTP Method：** POST  
 **前置条件:** 用户邮箱注册，获取验证码  </br>
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

https://uws.haier.net/uaccount/v2/user/resetPassword
 
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

> D00017、B0004、D00009、D00015、D00022  

#### 使用手机号重置密码
> 使用手机号重置密码，需先申请短信验证码
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/resetPasswordByMobile`  
 **HTTP Method：** POST  
 **前置条件:** 用户手机号注册，获取验证码  </br>
 **Token 验证：** 否  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----|
|mobile	|String	|Body|	是|	需使用公钥加密，后端服务解密并校验规则|  
|password|String|Body|是|新密码：需使用公钥加密。后端服务解密并校验规则，|  
|captcha|	String|	Body|	否	|图形验证码，4位字母和数字组合。每个验证码只能使用一次，使用后或过期即作废，需重新获取。对于同一App、同一手机终端邮箱验证码连续校验失败3次后需启用图形验证码进行验证，3次可配置，默认为3次。图形验证码校验成功后，允许失败次数重新恢复为0|  
|msgCode|String|Body|是|验证码,用户申请验证码用于重置密码,发送至用户手机号，注册时需填写此验证码，6位随机数字|  
   


**输出参数：**标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/user/resetPasswordByMobile
 
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
"mobile":"kiGyXcG/LpQcs871xkTIDvFVf8VdLLQtm8dkqjCJecnJYDYz1vDmkHUBxkE5 1oX6qIgglUQsY88Gex5VmjmjlqoYi448ENuUCvQc2Gfflb5AjjVoStkG3eV+ BSeHPF4JM7iiWKxFiTolntopbYEnpPVGjgP3iS/td/nO+20pBQo=",
"password":"huFWFHP0axAa5ymUmHmJgzuwcOFZozKXvT01yiHJh51ZqBUdVteZ3dtzPFUy 6EdQS1R4ha8uN2u9qIeoogKFfEr0zvT1Kd2Q5p2OonDppzO3zV6VPdWbjPcA mRArIvVkBpL4ycZKVFoEKmhnzWDMOWqf0yNV/DApxNqNBJjHfGI=",
"msgCode":"445780",
"captcha":""}


```  

**请求应答**

```java
{
  "retCode": "00000",
  "retInfo": "成功"
}


```

##### 3、错误码

> D00017、B0004、D00009、D00015、D00022  

#### 修改账号密码
> 用户在登录状态可修改登录密码 
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/changePassword`  
 **HTTP Method：** POST  
 **前置条件:** 用户登录  </br>
 **Token 验证：** 是  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----| 
|password|String|Body|是|旧密码：需使用公钥加密。后端服务解密并校验规则| 
|newPassword|String|Body|是|新密码：需使用公钥加密。后端服务解密并校验规则|  
|captcha|String|Body|	否	|图形验证码，对于同一App、同一手机终端修改密码时连续校验失败m次后需启用图形验证码进行验证，m可配置，默认为3次。在启用验证码后，如果图形验证码输入正确的情况下，同一天对同一账户的错误尝试达到n次后将对应的账户锁定，n可配置，默认为10次。|  
   


**输出参数：**标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/user/changePassword
 
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
"password":"M86jq-ZxErDwBcy_p2gztuBKVbk_nlrHTvePtQP2YGqzXW5q6zbY-a5sV7rltbLD8Kl4Jc8O6emWz3zEOeANJfVQkIbjq2wPc4XEeHls3AwVqnDR6GB2ncljRvX-ZlD2UHcM4AzuJk3I-stOYLnbTPVoDTSxbyNfxEREXq2e2ZI",
"newPassword":"MrGIHA_edrlvBIvxYSn5Kstp7pj89ccQ_UFYQNVHQPuulIfJ1QRoEoi-Oq8mpYX-58BU2eglmfPcV8bpQRXH9ILRXbiVLaU0ZhCStkv68l9Q_GuSYrjEISAKK62naHwbMQrzb_seUnmQsGtCwrJx8WtQdnm6Gcz7aGZ7DGWzLC8",
"captcha":"73ll"
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

> D00003、D00002、B00004、D00009、D00015、D00010

#### 修改账号邮箱
> 用户在登录状态可修改账号的邮箱 
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/modifyEmail`  
 **HTTP Method：** POST  
 **前置条件:** 用户登录  </br>
 **Token 验证：** 是  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----| 
|email|String|Body|是|需使用公钥加密，后端服务解密并校验规则| 
|msgCode|String|Body|是|验证码；用户申请验证码,系统将验证码发送至用户邮箱，注册时需填写此验证码，6位随机数字|  
|captcha|String|Body|	否	|图形验证码，对于同一App、同一手机终端请求接口时连续失败m次后需启用图形验证码进行验证，m可配置，默认为3次。|  


**输出参数：** 标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/user/modifyEmail
 
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
"email":"jlNMp6KDF6mvDQ6dO1oPQo8SiQd71fAm09sVhmQ7I3P742APhvcW3NrifHDc f7kY+I+Im/lEB1UgRJJ0wf14Gc+vdVxrCJZEYKmQpPXkY+ws8u8upGO5x6xJ S+Gr7YFieFwN0CwYpX0IJJDwQ1T26KQtKEy3wIsXvd5BcAvZS3U=",
"msgCode":"946202",
"captcha":""
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

> D00008、B00004、D00009、D00015  


#### 修改账号手机号
> 用户在登录状态可修改账号的手机号
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/modifyMobile`  
 **HTTP Method：** POST  
 **前置条件:** 用户登录  </br>
 **Token 验证：** 是  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----| 
|mobile|String|Body|是|需使用公钥加密，后端服务解密并校验规则| 
|msgCode|String|Body|是|验证码,用户申请的验证码以短信方式发送至用户手机，6位随机数字|  
|captcha|String|Body|	否	|图形验证码，对于同一App、同一手机终端请求接口时连续失败m次后需启用图形验证码进行验证，m可配置，默认为3次。|  


**输出参数：** 标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/user/modifyMobile
 
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
"mobile":"jlNMp6KDF6mvDQ6dO1oPQo8SiQd71fAm09sVhmQ7I3P742APhvcW3NrifHDc f7kY+I+Im/lEB1UgRJJ0wf14Gc+vdVxrCJZEYKmQpPXkY+ws8u8upGO5x6xJ S+Gr7YFieFwN0CwYpX0IJJDwQ1T26KQtKEy3wIsXvd5BcAvZS3U=",
"msgCode":"946202",
"captcha":""
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

> D00008、B00004、D00009、D00015  


#### 用户申请注销账号和设备信息
> 用户申请删除账号信息和设备绑定关系，并发送邮件通知，用户申请成功后，账号不能登录，登录返回密码错误
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/user/applyDeleteAccount`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|msgCode|String|Body|是|验证码,用户申请验证码用于注销账号,验证码发送至用户邮箱，注销前需验证验证码，6位随机数字。申请类型type=5|  


**输出参数：** 标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/user/applyDeleteAccount
 
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
{"msgCode":"123333"}
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

### 公钥管理

#### 获取公钥
> 获取公钥
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/mgr/getPublicKey`  
 **HTTP Method：** POST  
 **Token 验证：** 否  

**输入参数**  无输入参数

**输出参数：** publicKey  

##### 2、请求样例  

**用户请求**

```java
POST
 https://uws.haier.net/uaccount/v2/mgr/getPublicKey
 
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
> 给前端应用提供验证本地公钥合法性的接口
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/mgr/verifyPublicKey`  
 **HTTP Method：** POST  
 **前置条件:** 获取公钥  </br>
 **Token 验证：** 否  

**输入参数**  

| 参数名         | 类型          | 位置  |必填|说明|
| ------------- |:-------------:|:-----:|:-------------:|:-----| 
|sn|String|Body|是|需使用公钥加密时间戳，后端服务解密成功，验证为时间戳数字即正确| 


**输出参数：** 标准输出参数  

##### 2、请求样例  

**用户请求**

```java
POST

https://uws.haier.net/uaccount/v2/mgr/verifyPublicKey
 
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
"sn":"V3O5gPCk9JQm_lPiScTeyDXpf-6pIb4Vl0mR9fB7cUocn_RQizg0ica0bJ0-65fJpLolkCNiVZ78jTDfTlj6o_HraGUiIpz-sBp5UZrO6ffBIPr4LhPL1Aew3XNrThIQNlleVKDkLHrHq2hMXxLx9M6BQro_SfrrGdInxk9Fu8Y"}


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




### 用户信息管理


#### 查询用户信息
> 根据登录者token，获取用户信息，不包括手机号
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/users/get`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  无输入参数

**输出参数：** 

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|userId|String|Body|是|账号唯一标示|  
|email|String|Body|是|账号邮箱地址，按需求进行脱敏处理|  
|userProfile|Map|Body|否|用户扩展信息,包括昵称、头像等|
    

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/users/get
 
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

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功",
"userId":"1027887557792235520",
"email":"bu***i@126.com"，
"userProfile":	{"birthday":null,"country":null,"address":null,"serviceEmail	":null,"avatarUrl":"http://uhome.haieriot.net:6410/resource/	enabling/	hkqhwbbscb/19002243/1517816146943.jpg","nickName":"","hotlin	e":"12345678","sex":null,"weight":null,"phone":null,"name":"	test","serviceProviders":null,"height":null
	}
}


```

##### 3、错误码

> D00008     

#### 查询用户信息
> 根据登录者token，获取用户信息，包括手机号
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/users/get`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  无输入参数

**输出参数：** 

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|userId|String|Body|是|账号唯一标示|  
|email|String|Body|是|账号邮箱地址，按需求进行脱敏处理|  
|mobile|String|Body|是|手机号，按需求要求脱敏处理| 
|userProfile|Map|Body|否|用户扩展信息,包括昵称、头像等|
    

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/users/get
 
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

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功",
"userId":"1027887557792235520",
"email":"bu***i@126.com"，
"mobile":"138****2",
"userProfile":	{"birthday":null,"country":null,"address":null,"serviceEmail	":null,"avatarUrl":"http://uhome.haieriot.net:6410/resource/	enabling/	hkqhwbbscb/19002243/1517816146943.jpg","nickName":"","hotlin	e":"12345678","sex":null,"weight":null,"phone":null,"name":"	test","serviceProviders":null,"height":null
	}
}


```

##### 3、错误码

> D00008   


#### 用户信息修改
> 根据登录人员的token，修改当前登录用户的扩展属性
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/users/update`  
 **HTTP Method：** POST  
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|userProfile|Map|Body|是|用户扩展信息,包括昵称、头像等|

**输出参数：**  标准输出参数 
    

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/users/update
 
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
  "userProfile": {
  		"nickName": ,
  		"avatar": ,
        "updateTime": "20141115",
        "tel": "0596",
        "companyCode": 333,
        "address": "china",
        "email": "8****322@qq.com",
        "QQ": "8****322",
        "name": "test",
        "realname": "test"
  }
}

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功"
}


```

##### 3、错误码

> D00008   

### 权限认证管理
#### 用户接受隐私条款
> 用户接受隐私条款，记录隐私条款版本号
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/security/acceptUserPrivacy`  
 **HTTP Method：** POST   
 **前置条件：** 登录状态  
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|privacyVersion|Map|Body|是|隐私条款版本号|

**输出参数：**  标准输出参数 
    

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/security/acceptUserPrivacy
 
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
{"privacyVersion":"V1.0.0"}

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功"
}


```

##### 3、错误码

> B00004、D00003   

#### token校验接口
  
> 验证token是否可用     
 
##### 1、接口定义

?> **接入地址：**  `https://uws.haier.net/ouath/2.0/tokenInfo`  
 **HTTP Method：** GET  
 **前置条件：**登录获取会话token      
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|accessToken|String|&nbsp;|是&nbsp;|  

**输出参数：** 

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|open_id|String|Body|是|账号open_id|  
|app_id|String|Body|是|客户端应用标识|    
|iss|String|Body|是|表示此令牌的颁发者|    
|exp|String|Body|是|有效期 单位秒|    
|iat|String|Body|是|颁发时间|    
|aud|String|Body|是|表示此令牌的目标受众的标识符|      


##### 2、请求样例  

**用户请求**

```java
GET
https://uws.haier.net/oauth/2.0/tokeninfo?access_token=TGT3QHRFERNRLKH82COVN79ZTKBRP0
 
Header：
Request Headers:
Connection: keep-alive
Content-Encoding: utf-8
Content-type: application/json
Host: 192.168.190.27:8081
User-Agent: Apache-HttpClient/4.2.6 (java 1.5)


```  

**请求应答**

```java
{
"open_id":"********",
"iss":"http://uws.uhome.net","exp":"86400",
"app_id":"MB-UZHSH-0000",
"iat":"1552383052007",
"aud":"uws.application-oa2-client.af6b5c6c93d22ede41c9b791dcf2b1d3"
}


{"error_description":"Token已过期，未通过token验证","error":"D00004"}
```

##### 3、错误码

> D00004   


### 会话分享

#### 登录会话刷新
> accessToken过期后，可以使用对应的refreshToken
> 获取新的accessToken  
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/auth/token`  
 **HTTP Method：** POST     
 **Token 验证：** 否  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|refreshToken|String|Body|是|app端持有refreshToken，用于会话accessToken延期，延期会话会生成新的refreshToken和accessToken|  
|grantType|String|Body|是|授权方式 ，当前默认为refresh_token|  



**输出参数：**  
 
| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|accessToken|String|Body|是|安全令牌 |  
|refreshToken|String|Body|是|刷新令牌|    
|scope|String|Body|是|访问资源的范围|       
|expire|String|Body|是|有效期 ，单位秒|    

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/auth/token  
 
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

> 00001、D00005、D00025   



#### 获取会话分享验证码
> 通过accessToken，请求分享的appId，clientId获取会话分享的验证码，该验证码可用于生成请求分享终端的会话，即实现同一个账号通过一个应用授权登录其他应用终端的过程  
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/auth/shareCode`  
 **HTTP Method：** POST     
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|shareAppId|String|Body|是|请求分享会话的appId|  
|shareClientId|String|Body|是|请求分享会话的clientId|  
|accessToken|String|Body|是|用于分享会话的accessToken|  



**输出参数：**  
 
| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|code|String|Body|是|会话分享验证码  |  
  

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/auth/shareCode  
 
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
"accessToken":"TGTH5FR2XH20S0C2E7G56V1CMQ4000",
"shareClientId":"MB-TEST2-0000",
"shareClientId":"456FEW334DD"
 }

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功",
"code": 72f7b235dd3afee2c77907d160c66539850b3224da60cb6e6638809005f48ec5"
}



```

##### 3、错误码

> B00004、D00004、D00026        


#### 会话分享
> 登录会话分享 
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/auth/shareToken`  
 **HTTP Method：** POST     
 **Token 验证：** 否  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|code|String|Body|是|会话分享验证码|  



**输出参数：**  
 
| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|accessToken|String|Body|是|安全令牌   |  
|refreshToken|String|Body|是|刷新令牌   | 
|scope|String|Body|是|访问资源的范围| 
|expire|String|Body|是|有效期 ，单位秒 | 
  

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/auth/shareToken 
 
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
"code":"da48b7de0a9bd0639b43fc40948176821784d3c01276870cceccf0b6564624e7 " 
}

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功",
"refreshToken":TGTV5FR3XH20S0B2E7G56V1CMQ4T67,"accessToken":"TGTNS633MLE2OHV2P03YB3Q6E44K00",
"scope":"auth_app",
"expire":"2160000"
}


```

##### 3、错误码

> B00004、00001、B00001、B00002  


#### 查询会话分享列表  
> 通过accessToken，查询此用户进行会话分享的客户端列表   
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/auth/queryShareList`  
 **HTTP Method：** POST     
 **Token 验证：** 是  

**输入参数**  无输入参数


**输出参数：**  
 
| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|shareData|String|Body|是|客户端列表信息|  
  

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/auth/shareToken
 
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

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功",
"shareData":
	{"appId":"MB-UZHSH-0000",
	"clientId":"uaccount123",
	"shareTokenInfoList":[{"shareAppId":"MB-HEYJOZB-0001","shareClientId":"123456789","state":"1"}]
    }
}

```

##### 3、错误码

> B00004、D00008      


#### 取消会话分享  
> 用户提交登录获取的Token（或登录进行会话延期获取的Token）和会话分享的appId和clientId，Token校验成功且会话分享存在的情况下，将通过会话分享获取的RefreshToken和accessToken及会话延期的Token全部设置为失效  
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v2/auth/cancelShare`  
 **HTTP Method：** POST     
 **Token 验证：** 否  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|shareAppId|String|Body|是|会话分享的appId|  
|shareClientId|String|Body|是|会话分享的clientId|  

**输出参数：**  标准输出参数
 

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v2/auth/shareToken
 
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
Body：
{
"shareAppId":"MB-HE****B-0001",
"shareClientId":"123456789"
}

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功"
}

```

##### 3、错误码

> B00004、B00001、D00008、D00027    

### 第三方社交账号

#### 第三方社交账号获取IOT平台token接口  
> 用户使用第三方社交账号登录海尔app，通过第三方账号token获取海尔iot平台accessToken    
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/thirdpart/social/getAccessToken`  
 **HTTP Method：** POST     
 **Token 验证：** 否  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|thirdpartAccessToken|String|Body|是|第三方社交账号的token，通过第三方社交账号SDK获取的票据|  
|socialType|String|Body|是|类别：facebook twitter amazon|  
|thirdpartOpenId|String|Body|否|当前接入Facebook，Twitter，Amazon需传openId，此参数非必填定义兼容后期无openId 的模式| 

**输出参数：**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|accessToken|String|Body|是|安全令牌 |  
|scope|String|Body|是|访问资源的范围|  
|expire|String|Body|是|有效期 ，单位秒| 

##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/thirdpart/social/getAccessToken
 
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
Body：
{
"thirdpartAccessToken":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
"socialType":"amazon",
"thirdpartOpenId":"110347635"
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

?> **接入地址：**  `/uaccount/v1/thirdpart/social/bindGroup`  
 **HTTP Method：** POST     
 **Token 验证：** 是  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|uhomeAccessToken|String|Body|是|自有账号登录的iot平台 accessToken|  
|thirdpartUhomeAccessToken|String|Body|是|第三方账号登录的iot平台token|  


**输出参数：**  标准输出参数

##### 2、请求样例  

**用户请求**

```java
POST
 https://uws.haier.net/uaccount/v1/thirdpart/social/bindGroup
 
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
Body：
{
"thirdpartAccessToken":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
"accessToken":"TGTUE8J3JHIDF12WEWHRH0912300"
}

```  

**请求应答**

```java
{
"retCode":"00000",
"retInfo":"成功"
}


```

##### 3、错误码

> D00004、A00005、D00024      


#### 查询账号绑定第三方账号组关系  
> 查询自有账号下绑定的全部第三方账号的信息，包括扩展信息     
 
##### 1、接口定义

?> **接入地址：**  `/uaccount/v1/thirdpart/social/getBindingGroup`  
 **HTTP Method：** POST     
 **Token 验证：** 是  

**输入参数**  无输入参数

**输出参数：** 

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|groups|String|Body|是|示例：见请求样例|  


##### 2、请求样例  

**用户请求**

```java
POST
https://uws.haier.net/uaccount/v1/thirdpart/social/getBindingGroup
 
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
Body：
{
"thirdpartAccessToken":"cok53pt9F5vABcD1HNGwP1YqGKbL8VfLdILvK-wR_fY7esjDLGlIkhilu6QNeApvOouMcJSl5a5R9OATONGQDQpbRZk-vo2CtTKf3Tuzgf0SBfJfL1AXVog7cjlZpZc9TNh7HB4WiaSS7-SfbhOAwJC1Qh5J9lGmLBk8yUfnhj4",
"accessToken":"TGTUE8J3JHIDF12WEWHRH0912300"
}

```  

**请求应答**

```java
{
    "retCode": "00000",
    "retInfo": "成功",
    " groups": [
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


### Oauth登录

#### 开发流程  

![开发流程][kaifaliucheng]  

#### H5登录页  

![H5登录页][H5] 

##### 接口定义

?> **接入地址：**  `https://uws.haier.net/ouath/2.0/authorize`  
 **HTTP Method：** GET       
  

**输入参数**  

| 参数名   | 类型   | 位置  |必填|说明|
| --------|:------:|:-----:|:-----:|:-----| 
|app_id|String|&nbsp;|是|应用ID|  
|state|String|&nbsp;|是|表示客户端的当前状态,可以指定任意值,认证服务器会原封不动地返回这个值，用于客户端校验防止CSRF攻击|  
|response_type|String|&nbsp;|是|表示授权类型，此处的值固定为access_token|  
|scope|String|&nbsp;|是|申请的权限范围，默认implicit ，后续支持authorization_code|  
|redirect_uri|String|&nbsp;|是|客户端重定向地址|  

#### 回调地址  

`redirect_uri? access_token ={ access_token } &state={ state }`  
1、	返回access_token  
2、	客户端对state进行校验，防止CSRF(跨站请求伪造)攻击
 






[^-^]:常用图片注释
[account_type]:_media/_account/account_type.png
[account_liucheng]:_media/_account/scene_flow.png
[account_PasswordFlow1]:_media/_account/account_PasswordFlow1.png
[account_PasswordFlow2]:_media/_account/account_PasswordFlow2.png
[account_captcha]:_media/_account/account_captcha.png  
[kaifaliucheng]:_media/_account/kaifaliucheng.png
[H5]:_media/_account/H5.png


[Business]:/zh-cn/Business

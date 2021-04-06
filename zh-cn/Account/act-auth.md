#  用户授权

## 海尔账号授权
	
### 介绍

为第三方应用提供海尔账号的Oauth授权服务，允许用户使用海尔帐号登录第三方应用，访问和控制自己的海尔设备。

接入流程

![接入流程][pic1]

### 注册成为开发者

访问[海级网-开发者中心](https://www.haigeek.com/developercenter/static/develop.html#/system/login)
选择我是企业用户表单，填写和上转相关信息，完成注册


### 创建云应用

**1	创建云应用**

经过审核后获得systemId和systemKey：	

| 名称      | 说明         | 
| ------------- |:-------------:|
|systemId| 云应用ID |
|systemKey| 云应用ID的秘钥 |


**2	添加服务**

云应用 -> 服务信息->添加服务->海尔帐号控制设备

![海尔账号控制设备][pic2]

接入信息

![接入信息][pic3]

### 接入流程的开发

** 1 海尔授权登录H5页链接**

第三方App通过发现技能或者绑定第三方账号的方式打开海尔账号Oauth授权登录H5页，此时第三方 App 要生成 授权 URL（标准的OAuth 授权码模式的认证的URI）。 用户 进入 授权 URL， 登录并完成 对 应用 的 授权，用户中心将重定向 用户 至第三方App回跳页，并带上code和是state。


```
 测试环境 ：https://taccount.haier.com/oauth/authorize? client_id=rptest&amp;response_type=code&amp;state=xyz&amp;redirect_uri=https://r p.com/login_callback  

 生产环境：https://account.haier.com/oauth/authorize? client_id=rptest&amp;response_type=code&amp;state=xyz&amp;redirect_uri=https://r p.com/login_callbac

```

|Parameter | Desc| Require| 
| ------------- |:-------------:|:-------------:|
|client_id |为海极网分配的systemid, 我们使用例子中的 rptest| Y|
|response_type| 为授权方式, 这里固定为 code| Y|
|redirect_uri |指定回跳地址, 这里为 https://rp.com/login_callback| Y|
|state|为 应用 生成的随机字符, 在 用户 授权 回调时会原样返回给 应 用, 藉此可以判断来自 本平台 的回跳是否被伪造; 此参数非必传, 但推荐传送以增强安全性|N|
|display|告知 本平台 以何种登录界面展示给 用户, 如设定为 qr 时, 本平 台 仅展示只有二维码的页面(意在让 用户 扫码登录), 同时此页面 可作为 iframe 嵌入 应用 当前页面; 若未指定, 则默认展示登录界 面|N|



输入上面的授权请求地址后，出现如下界面：

![开通服务1][pic8]

1、	输入海尔账号登录名（支持手机号和用户名）和密码，点击授权并登录；如未注册国海尔账号，请点击右上方的app下载注册链接，注册海尔账号；如图1所示。</br>
2、	支持使用手机号直接登录，手机接收验证码短信，输入短信验证码即可完成授权，如图2所示。</br>
3、	用户需要勾选权限和登录协议才能点击授权并登录按钮。</br>
4、	如输入密码错误次数过多，会提示图形验证码界面，强制用户输入验证才能完成授权，当用户输入密码错误次数达到限制，系统会锁定该用户帐号，5小时后自动解锁。


** 2	授权成功返回**
```
Location: {redirect_uri}?code=SplxlOBeZQQYbYS6WxSbIA&state={state}
```
|       |          |          |
| ------------- |:-------------:|:-------------:|
|redirect_uri| 授权回调地址，务必和授权表单里填写的一致|
|code| 用户授权给应用的授权码 |
|state| 应用生成的随机字符,藉此判断此次回跳是否被伪造 |
				
** 3	获取授权token **

>  通过oauth登录获取的token获取海尔token 

?> **请求地址：** `https://uws.haier.net/ucs/uia/get/token-code `</br>
**HTTP Method：** POST </br>



**输入参数**

参数名|类型|位置|是否必填|说明
:-|:-:|:-:|:-:|:-
code|String|body|是|Oauth登录返回的code ，使用授权码换取token，code有效期为 10 分钟，只能使用1次
redirectUrl |String|body|是|和调用登录h5 时使用的回调地址一致 
systemId|String|Header|是|海极网申请的systemId 
sequenceId |String|Header |否|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用日期+顺序编号的方式。 
apiVersion |String|Header |是|此处默认填v1 
sign |String|Header |是|签名 
timestamp |long |Header |是|Unix时间戳，精确到毫秒。 
Content-Type  |String |Header |是|application/json;charset=UTF-8  

**输出参数**

参数名|类型|位置|说明
:-|:-:|:-:|:-
retCode| String| body| 错诨码 
retInfo |String| body |错诨详细信息 
payload |Object  |Body|  
accessToken|String| payload| 授权凭证 
refreshToken |String |payload |刷新凭证 
uhomeClientId| String| payload| 访问Iot设备中心绑定控制设备接口，头信息必填参数（注明：设备中心接口参数定义为 clientId） 
source |String| payload| 来源 
scope |String |payload| 权限范围 
expiresIn |Long| payload| accessToken的有效时间（以秒为单位） 


用户请求 

```
POST data: 
{ 
 "code": "abcopdfoiekdsm", 
 "redirectUrl": "https://www.haigeek.com" 
} 
```

请求应答

```
{ 
 "retCode": "0000", 
 "retInfo": "成功", 
 "payload": { 
  "accessToken": "wfhwdh", 
  "expiresIn": 300, 
  "refreshToken": "wfwidoijwdmpocop", 
  "scope": "write,read", 
  "source": "haier" 
 } 
} 
```

** 4	刷新授权token **


?> **请求地址：** `https://uws.haier.net/ucs/uia/refresh/token `</br>
**HTTP Method：** POST </br>


**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
refreshToken  |String|body|是|刷新凭证 
systemId|String|Header|是|海极网申请的systemId 
sequenceId |String|Header |否|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用日期+顺序编号的方式。 
apiVersion |String|Header |是|此处默认填v1 
sign |String|Header |是|签名 
timestamp |long |Header |是|Unix时间戳，精确到毫秒。 
Content-Type  |String |Header |是|application/json;charset=UTF-8  

**输出参数**

参数名|类型|位置|说明
:-|:-:|:-:|:-
retCode| String| body| 错诨码 
retInfo |String| body |错诨详细信息 
payload |Object  |Body|  
accessToken|String| payload| 授权凭证 
refreshToken |String |payload |刷新凭证 
uhomeClientId| String| payload| 访问Iot设备中心绑定控制设备接口，头信息必填参数（注明：设备中心接口参数定义为 clientId） 
source |String| payload| 来源 
scope |String |payload| 权限范围 
expiresIn |Long| payload| accessToken的有效时间（以秒为单位） 


用户请求 

```
{ 
 "refreshToken": "refreshToken" 
} 
```

请求应答
```
{ 
 "retCode": "0000", 
 "retInfo": "成功", 
 "payload": { 
  "accessToken": "wfhwdh", 
  "expiresIn": 300, 
  "refreshToken": "wfwidoijwdmpocop", 
  "scope": "write,read", 
  "source": "haier" 
 }
} 
```




[^-^]:常用图片注释
[pic1]:../_media/_account/pic1.jpg
[pic2]:../_media/_account/pic2.png
[pic3]:../_media/_account/pic3.png
[pic4]:../_media/_account/pic4.png
[pic5]:../_media/_account/pic5.png
[pic6]:../_media/_account/pic6.png
[pic7]:../_media/_account/pic7.png
[pic8]:../_media/_account/pic8.jpg
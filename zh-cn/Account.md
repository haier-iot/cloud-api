
>  **当前版本**：[海尔集团690用户中心API_SOCIAL_UHOME_V3_2](zh-cn/ChangeLog/Account)  
 **更新时间**：{docsify-updated}  


## 简介
	
>  账号服务由海尔集团690用户中心提供。	



## 注册成为开发者  

>  请联系用户中心相关人员，陈子琦（chenziqi@haier.com）获取开发者权限。

## 申请应用

>  联系相关人员，申请应用通过后，获得测试环境和正式环境的client_id与client_secret，前者为应用Id，后者为应用密钥。测试环境和正式环境的client_id与client_secret不会相同。  



## 前提

1.接口中需要的client_id和client_secret由用户中心授权下发，根据接入应用的不同进行不同的授权。<br/>

2.文档提供的接口为海尔品牌接入接口，卡萨帝接口及GE品牌接口，分别随品牌不同而区分不同调用域名。<br/>

3.文档提供的接口大多受到access_token保护，接口分为应用级token保护和个人级token保护：应用级
保护大多发生于登录动作之前，token由以下o接口获取；个人级token保护大多发生于登录动作之后，token
由登录等动作接口返回。受保护的接口在被调用时，调用方需要在HTTP请求的Header中新增Authorization,
值为Bearer[access_token]，后面接口将直接备注受应用级还是设备级保护，请调用方自行甄别。<br/>

4.本接口的鉴权文档，需要透传Uhome参数，注意，接口中提到的client_id为用户中心下发的应用标识，uhome_client_id为Uhome要求的客户端标识，请注意区分，不要混淆。<br/>

5.鉴权通过后，接口返回的access_token字段为用户中心下发的用户访问令牌，uhome_access_token
为U+云平台下发的设备访问令牌，请注意区分，不要混淆。<br/>

6.关于透传的uhome三个参数，uhome_app_id、uhome_client_id、uhome_sign，做出以下说明：uhome_app_id是由U+云平台(海极网)颁发的应用ID，40位以内字符，Haier U+云平台全局唯一;uhome_client_id是客户端ID，主要用途为唯一标识客户端(例如,手机)。可调用U+云平台usdk得到客户端ID的值。<br/>
uhome_sign是U+云平台要求的安全验证签名，需要加密的字段只需要Uhome的appId+appKey+clientid(顺序不可变化)，不用加密原来文档里的url和body字符串等，具体请参考U+云平台的签名认证章节。<br/>
7.以下所有接口的正常Response下的HTTP Status Code为200，后无特殊情况不再说明。
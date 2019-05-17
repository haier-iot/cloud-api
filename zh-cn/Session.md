
!> **更新时间**：{docsify-updated}  


## 简介
	
>  会话分享服务为开发者提供登录应用App1用户的会话授权分享给应用App2使用，以及相关会话管理服务内容。

详细的业务流程参考下图：

![业务流程][session1]

1）请求需提供要分享到的AppId、ClientId，和当前终端的AccessToken，对AccessToken进行校验后，生成授权码，此授权码只能用于指定的AppId、ClientId和账户。授权码的有效期为10分钟（有效期可配置）。<br/>

2）用户收到授权码后，通过安全渠道传输给需要分享到的App，App通过提交自己的AppId、ClientId和授权码，系统对授权码与AppId和ClientId的对应关系和授权码的有效期进行校验，校验合格则为对应的账户生成新的RefreshToken和AccessToken返回给用户；校验失败，则返回授权码校验失败的错误。<br/>

3）RefreshToken的有效期默认永久有效，AccessToken的有效期默认25天。<br/>


4）仅通过登录或RefreshToken获取的AccessToken，方可获取授权码，通过授权码获取到的AccessToken及由RefreshToken再次获取的AccessToken不可用来申请授权码。<br/>



## 应用场景
>  关于面向普通用户的控制设备场景，可通过使用App登录用户会话分享给应用服务，获得用户授权应用的token，再通过调用平台的token版设备控制服务来进行设备控制。

![应用场景][session2]

## 接口列表

### aaaaa接口



[^-^]:常用图片注释
[session1]:_media/_session/session1.png
[session2]:_media/_session/session2.png

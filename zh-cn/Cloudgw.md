
!> **更新时间：** {docsify-updated} 


## 云设备  

### 简介
作为第三方云服务设备接入的依据和输入。

### 术语和定义  

| **名称** | **解释** |  
| :-----: |:------:|  
|Haier U+云平台|为物联网云平台，提供设备和应用的接入服务、数据分析服务等|   

### 公共说明  

#### 整体架构与接入流程介绍  

Iot云平台支持第三方设备通过云云对接的方式接入，整体架构如下图  

![架构图][framework]

#### HTTP接入地址定义  

本文档提供的所有接口仅支持https协议请求，接入地址如下：  
`https://uws.haier.net:443/cloudgw/接口地址`   
访问非生产环境时，需要配置第三方云服务DNS，以便是域名指向对应的环境。  

#### HTTP Header参数定义  

第三方云服务调用Iot平台HTTPS接口时，需要携带以下HTTP Header参数：  


参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|是|系统ID。40位以内字符，系统全局唯一标识  
sequenceId|String|Header|是|报文流水(客户端唯一)客户端交易流水号。6-32 位。由客户端自行定义，自行生成。建议使用不带中横线的UUID，例如：e3480efdef6a410f9f60801707671bcc  
sign|String|Header|是|对请求进行签名运算产生的签名 [签名算法][sign]   
timestamp|String|Header|是|Unix时间戳，当前UTC标准时间，精确到毫秒，例如：1557900146503    
Content-Type|String|Header|是|本接口Payload内容仅支持UTF-8编码的Json格式数据，值必须为application/json;charset=UTF-8
   









[^-^]:常用图片注释
[framework]:_media/_Cloudgw/framework.png 
[sign]:https://haier-iot.github.io/guide/#/zh-cn/Standard/Basic?id=%e7%ad%be%e5%90%8d%e7%ae%97%e6%b3%95












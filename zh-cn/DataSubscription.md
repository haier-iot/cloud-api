!>  **当前版本**：[UWS 数据订阅 V1.0.0][dataSubscription_document_url2]  
 **发布时间**：2018-07-19  

### 简介
>  数据订阅服务功能旨在帮助开发者或应用快速获取实时的增量数据，包括设备信息数据、用户数据以及其他开发者根据实际需求订阅的自定义数据。

智能互联设备上传的实时数据会进入IOT平台，当开发者或应用需要参考设备数据进行相关分析或操作时，开发者可以通过数据订阅连接到U+IOT平台设备上报的实时数据。

![数据订阅图片][dataSubscription_type]

### 功能介绍

第三方可以通过数据推送服务，订阅海尔设备消息功能，实现海尔设备数据与第三方平台的互通。设备消息包括设备的上下线，设备状态属性，设备告警，以及设备的大数据信息。目前支持根据设备的typeid进行推送。
设备数据订阅包括设备基础配置信息以及设备运行状态的实时信息。
1、	设备基础信息，包括设备版本、绑定信息、型号等；
2、	运行状态，包括故障、wifi信息、心跳、离线原因等。


### 应用场景

适用于第三方平台要获取在海尔平台上的设备数据（上下线，设备状态属性，设备告警，以及设备的大数据信息）用来实现某些业务逻辑。

面向使用或销售海尔平台设备的第三方合作伙伴，服务支持海尔U+云平台向第三方合作商推送设备的上下线、属性、报警、大数据信息功能。实现海尔云平台向第三方平台服务的数据推送功能。


### 使用方式
#### 开通流程  
![开通流程][dataSubscription_liucheng]
具体开通方法详见“数据推送服务接入说明书”。
<!--
### 文档资料

[数据推送平台接口规范说明书][dataSubscription_document_url1]    
[数据推送服务接入说明书][dataSubscription_document_url2]
-->

### 常见问题


[^-^]:文本连接注释
[dataSubscription_document_url1]:_document/_dataSubscription/数据推送平台接口规范说明书.pdf
[dataSubscription_document_url2]:_document/_dataSubscription/数据推送服务接入说明书.pdf

[^-^]:常用图片注释
[dataSubscription_type]:_media/_dataSubscription/dataSubscription_type.png
[dataSubscription_liucheng]:_media/_dataSubscription/dataSubscription_liucheng.png


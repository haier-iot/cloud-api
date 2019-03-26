
### 1简述

移动端SDK是一款移动应用开发套件，它能够支持Android（4.0及以上）和iOS（7.0及以上）系统。允许您运用Android、Object-C、Swift等语言，在移动终端和智能硬件上进行开发，通过SDK实现智能设备的配置入网、搜索、状态查询、控制、报警接收等功能。

SDK具备如下特点：

![usdk图片][usdk_1]

###  功能介绍

1.能力图集
![usdk图片][usdk_2]

2.搜索设备能力

当移动端SDK成功启动后，具备搜索当前WIFI网络中的海尔智能设备、与海尔合作厂商的智能设备的一种能力。

3.配置设备能力


配置设备能力就是使用移动端SDK将设备加入指定无线网络，或更改U+设备所在网络的一项操作。移动端SDK支持SmartLink、SoftAP两种方式入网。

3.1SmartLink配置能力

SmartLink配置方式是使用移动端SDK将无线配置信息通过路由器发送给待入网设备，完成配置入网的一种方式，是U+平台物联设备的主流入网方案之一。

3.2SoftAP配置能力

SoftAP配置方式是将智能设备设置为WIFI热点，通过移动端SDK连接智能设备热点，把无线配置信息发送给智能设备的一种入网方式。

4.控制设备能力


控制设备能力是通过移动端SDK向智能设备发送控制命令，完成的设备控制，更改设备的实际状态的一种能力。

5.属性读取能力

读取标准设备的某个单独属性的一种能力，了解设备当前的运行状态。

6.授权设备能力

移动端SDK能够对SmartDevice SDK接入设备进行授权的一种能力，该接入设备被授权后，具备控制其他设备的能力。
### 应用场景

用于和设备进行功能交互，完成对设备的控制，更改设备运行状态、了解设备当前运行状态和报警等场景。能够使用在Android（4.0及以上）和iOS（7.0及以上）系统的手机、pad和终端上，允许您运用Android、Object-C、Swift等语言，进行功能开发。

![usdk图片][usdk_3]

### 使用方式

![usdk图片][usdk_4]




[^-^]:常用图片注释
[usdk_1]:../_media/_usdk/usdk_1.png
[usdk_2]:../_media/_usdk/usdk_2.png
[usdk_3]:../_media/_usdk/usdk_3.png
[usdk_4]:../_media/_usdk/usdk_4.png

[^-^]:文本连接注释
[usdk_document_url]:_document/_usdk/uSDK5.0_Phone_Android开发手册_20180717173928794.pdf
[usdk_document_url]:_document/_usdk/uSDK5.0_Phone_iOS开发手册_20180717172937633.pdf

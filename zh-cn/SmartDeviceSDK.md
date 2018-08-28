
### 简介

设备端SDK（SmartDevice SDK），是一款基于Android系统或Linux系统的应用开发套件，能够使用在有屏、无屏的Android系统或Linux系统的智能设备上，将设备接入到U+平台，并与U+设备交互。主要包括设备接入功能和设备控制功能。

设备接入功能，主要的业务功能包括启动/停止SDK、添加/删除设备、属性和报警上报、大数据上报、开启绑定时间窗等功能。

设备控制功能，主要的业务包括设备入网功能、设备搜索功能、设备控制功能、接收状态变化上报功能、接收报警信息上报功能等。

SDK具备如下特点：

![SmartDeviceSDK图片][SmartDeviceSDK_1]

### 设备端SDK能力介绍

1.能力图集

![SmartDeviceSDK图片][SmartDeviceSDK_2]

1.1 上线宣告能力

当设备端SDK成功启动并添加设备后，该接入设备会在当前WIFI网络中宣告上线信息，使自己能够被该网络中的APP（移动端SDK）发现，这种能力被称作上线宣告能力。

1.2 设备被控能力

作为接入设备，能够被手机APP或云服务进行设备控制，更改设备状态的一种能力。

1.3 属性上报能力

作为接入设备，当设备属性状态发生变化时、或接受到设备控制命令后，需要及时上报设备的实际属性状态，这种能力被成为属性上报能力。

1.4 设备控制设备能力

通过设备端SDK添加授权设备后，使用移动端SDK对该设备进行授权，使该设备具备控制其他设备的一种能力。

### 应用场景

可以把设备端SDK理解为一套标准的中间连接件，能够完成控制命令的传输，设备报警、设备属性状态、大数据等数据转发。用于和设备底板、U+云、移动端SDK进行数据通信，完成大小循环的建立，功能与海尔uPlug模块功能基本相同，使用在有屏、无屏的Android系统或Linux系统的智能设备上。开发者只需要集中精力在底板和设备端SDK之间的数据转换上，就可以快速完成设备的接入。

![SmartDeviceSDK图片][SmartDeviceSDK_3]

### 使用方式

![SmartDeviceSDK图片][SmartDeviceSDK_4]



[^-^]:常用图片注释
[SmartDeviceSDK_1]:_media/_SmartDeviceSDK/SmartDeviceSDK_1.png
[SmartDeviceSDK_2]:_media/_SmartDeviceSDK/SmartDeviceSDK_2.png
[SmartDeviceSDK_3]:_media/_SmartDeviceSDK/SmartDeviceSDK_3.png
[SmartDeviceSDK_4]:_media/_SmartDeviceSDK/SmartDeviceSDK_4.png

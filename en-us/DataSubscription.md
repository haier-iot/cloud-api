!>  **current version**：[UWS DataSubscription V1.0.0][dataSubscription_document_url2]  
 **release time**：2018-07-19  

### Introduction
>  The Data Subscription Service feature is designed to help developers or applications quickly access real-time incremental data, including device information data, user data, and other custom data that developers subscribe to based on actual needs.  

The real-time data uploaded by the smart connected device will enter the IOT platform. When the developer or the application needs to refer to the device data for related analysis or operation, the developer can connect to the real-time data reported by the U+IOT platform device through the data subscription.

![Data subscription image][dataSubscription_type]

### Function is introduced
The third party can subscribe to the Haier device message function through the data push service, and realize the interworking between the Haier device data and the third-party platform. Device messages include the device's online and offline lines, device status attributes, device alarms, and device big data information. Pushing is currently supported based on the typeid of the device.Device data subscriptions include device infrastructure configuration information and real-time information about the device's operational status.  
1、	Basic device information, including device version, binding information, and model number.
2、	Operating status, including faults, wifi information, heartbeat, offline reasons, etc.


### Application scenarios

Applicable to third-party platforms to obtain device data (up and down lines, device status attributes, device alarms, and device big data information) on the Haier platform to implement certain business logic.  

For third-party partners who use or sell Haier platform equipment, the service supports the Haier U+ cloud platform to push the device's online and offline, attributes, alarms, and big data information functions to third-party partners. Realize the data push function of Haier cloud platform to serve third-party platforms.  


### Way of use
#### Opening process  
![Opening process][dataSubscription_liucheng]
For details of the opening method, please refer to the “Data Push Service Access Manual”.

### Documentation

[DataPushPlatformInterfaceSpecification][dataSubscription_document_url1]    
[DataPushServiceAccessManual][dataSubscription_document_url2]

### common problem


[^-^]:文本连接注释
[dataSubscription_document_url1]:_document/_dataSubscription/DataPushPlatformInterfaceSpecification.docx
[dataSubscription_document_url2]:_document/_dataSubscription/DataPushAccessManual.docx

[^-^]:常用图片注释
[dataSubscription_type]:_media/_dataSubscription/dataSubscription_type.png
[dataSubscription_liucheng]:_media/_dataSubscription/dataSubscription_liucheng.png


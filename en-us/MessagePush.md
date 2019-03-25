
>**Current version:** [UWS Message push service standard edition V 3.0](en-us/ChangeLog/MessagePush)   
**Updated time:** {docsify-updated} 

## Introduction 
Iot based message push service is provided for developers, which supports personalized push service mode. Developers can push relevant information to App users by configuring customized messages according to application features or specific scenarios.  


**Message channel registration**</br>
1. Device registration: before the user logs in the terminal, it allows the device to register the channel and has the ability to receive push;</br>
2. User registration: after the user logs in the terminal, the device which gives him/her the ability to log in (or authorize) has the ability to share and receive business messages and must register the service with UMS.</br>

**Gets the user registration device list**</br>
1. After the user logs in the device and successfully registers the device, get the list of user registered devices for pushable messages;</br>
2. APP server gets the list of registered devices without token clientId.</br>

**Client-to-client message push**</br>
The sharing process by which messages are sent from one device to another.  
For example, "xin chu" shares recipes with her mobile phone
**Cloud - client message push**</br>
APP server and others send messages to the terminal without logging in</br>

**Information status query** </br>
1. Legitimate users query the message status;</br>
2. Report the status of terminal messages;</br>
3. After the display of the message terminal, the status of message read/processed shall be reported to UMS;</br>  

### Application scenarios
A message push service applicable to the Internet of things includes sending messages from one device terminal to another, or from the application server to the device terminal.  

## Public structure description
### TerminalDto
Terminal information

Parameter names|Type|Instructions|Note  
:-|:-:|:-|:-
userId|String| |User Id, unique identity  
clientId|String|If you can get it directly from the uSDK you need to get it from the uSDK; If there is no uSDK, the device MAC address can be fetched  
devAlias|String|Terminal alias|Terminal alias
appId|String| |Apply ID, less than 40 characters
isOnline|Integer|1, Online; 2, offline|1 represents the inner line of 10min;</br>2, means not online within 10min
lastOnlineTime|Date|The last time the device was online|When isOnline bit 1, this field is not returned;</br> when isOnline is 2 and the field is not returned, it means that the last online time was two days ago  

### UpMsg
Transparent fields: data, androids, ioss, please strictly follow the definition of the message model and the third party channel for the content of the three fields.  

Parameter names|Type|Instructions|Note  
:-|:-:|:-|:-
notification|Map<String,Object>|Define the content of the Notification, as shown in the definition of the Notification object|
data|Map<String,Object>|Define the Data content of a custom message. See the Data object definition for details|
android|Map<String,Object>|To define the customized content of Android system messages, see the Android object definition for details|
ios|Map<String,Object>|To define the customized content of IOS system messages, see the definition of IOS objects for details
options|Options|Define the Option Settings for messages, as detailed in the Option object definition
version|String|Defines the version of the message, which is V1|

### msgClientHistoryDto

Parameter names|Type|Instructions|Note  
:-|:-:|:-|:-
taskId|String|Message task ID|The msgId received by the terminal is the taskId of ums  
msgId|String|Message ID|
userId|String|The user ID|
appId|String|Application ID|
clientId|String|Terminal ID|
busineeType|String|Business types|
message|UpMsg|The message model|
msgStatus|Integer|Message sending status|
readStatus|Integer|Message read state|
pushTime|DateTime|ums channel push time|


### MsgCloudHistoryDto

Parameter names|Type|Instructions|Note  
:-|:-:|:-|:-
msgId|String|Message ID|
userId|String|The user ID|
appId|String|Application ID|
clientId|String|Terminal ID |
busineeType|String|Business types|
messgae|UpMsg|A message model|
msgStatusStatus|Integer|Message sending status|
raadStatus|Integer|Message read state|
tag|String|The label|
pushTime|DateTime|Ums message push time|
retCode|String|Return code|

### DoNotDisturbDto

Parameter names|Type|Instructions|Note  
:-|:-:|:-|:-
dndId|String|Do not disturb sign|
beginTime|Integer|The start time|
endTime|Integer|The end of time|
businessType|Integer|Message business type|
priorities|Integer|Message priority|





## List of terminal function interfaces  

?>  Services are provided externally using the REST interface style, and only the HTTPS protocol is supported. </br>Access address:`https://uws.haier.net/ums/v3`


### Account module

#### Terminal registration

>Through this interface, the terminals receiving messages are registered in the system, and the relationship between appId, userId, clientId and pushId is established. This interface is a prerequisite for using ums and umse.</br>
>When registering, if you want to register the user information userId, you need to pass the accessToken in the header


##### 1、The interface definition
?> **Access address:** `/account/register`</br>
**HTTP Method：** POST

**Input parameters:**

Parameter names|Type|Location|Required or not|Note
:-|:-:|:-:|:-:|:-
channel|Integer|body|yes|Channel type.</br>0 is the aurora,</br>1 represents the m2m channel,</br>1 represents the m2m channel,</br>3 is mail,</br>4 means no use or no channel.
pushId|String|body|no|Terminal channel push identification.</br>The channel can be empty when it is 4 and not otherwise, and the length of the FCM channel should be 152.  
devAlias|String|body|no|Equipment alias  
msgVersion|String|body|yes|Message model version, which corresponds to version in the message model  


**Output parameters:**   

Standard output parameter  



#### Terminal logout
> Unregister registered terminal information</br>
> Generally, this interface is used when the user account is cancelled or the APP is uninstalled  


##### 1、The interface definition
?> **Access address:** `/account/logout`</br>
**HTTP Method：** POST

**Input parameters:**   

No-parameter input

**Output parameters:**  

Standard output parameter  


#### Get user terminal information  
> Query all active terminals under this user</br>
> According to the userId query, the terminal information of all active states registered by the userId  

##### 1、The interface definition
?> **Access address:** `/account/getTerminals`</br>
**HTTP Method：** POST

**Input parameters:** 

 No-parameter input

**Output parameters:** 

Parameter names|Type|Location|Required or not|Note
:-:|:-:|:-:|:-:|:-
retData|List<TerminalDto>|Body|yes|Terminal information list  


### Equipment modules

#### The device terminal is not disturbed  
> Set the terminal to be uninterrupted according to business type, priority and time period;</br>
> 1.Identify unique terminal with userId+appId+clientId;</br>
> 2.Multiple uninterrupted messages can be set at the same terminal;</br>
> 3.BusinessType (business now in business) you will need to set up the business now.</br>
> 4.The time is not allowed to cross when multiple uninterrupted devices are set under the same terminal;</br>
> 5.Each do-not-disturb configuration supports setting multiple priorities.</br>


##### 1、The interface definition
?> **Access address:** `/config/setNotDisturb`</br>
**HTTP Method：** POST </br>


**Input parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|yes|Message business type
priority|Integer|body|yes|Message priority is defined in the message model  
beginTime|String|body|yes|The start time
endTime|String|body|yes|The end of time  

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
dndId|String|body|yes|Do not disturb unique identification  


#### Cancel the device terminal to avoid interference
Users can turn off the do not disturb feature  


##### 1、The interface definition
?> **Access address:** `/config/cancelNotDisturb`</br>
**HTTP Method：** POST 


**Input parameters:**

Parameter names|Type|Location|Required or not|Note
:-:|:-:|:-:|:-:|:-
dndId|String|body|yes|Do not disturb to set a unique identity


**Output parameters:**
Standard output parameter


#### Query for don't disturb information

> Gets the set list of the do-not-disturb configurations, with userId+appId+clientId (that is, terminal) as the granularity query.

##### 1、The interface definition

?> **Access address:** `/config/getNotDisturbs`</br>
**HTTP Method：** POST 


**Input parameters:** 

 No-parameter input

**Output parameters:**

Parameter names|Type|Location|Required or not|Note
:-:|:-:|:-:|:-:|:-
retData|List<DoNotDisturbDto>|body|yes||


### Message module  

#### Push messages by device

> A message is pushed from one terminal of a user to multiple terminals of the user, and the sending and receiving terminals of the message belong to the same user (it can be a different APP)   

##### 1、The interface definition

?> **Access address:** `/msg/pushByClients`</br>
**HTTP Method：** POST 


**Input parameters:** 

Parameter names|Type|Location|Required or not|Note
:-:|:-:|:-:|:-:|:-
toClients|List<String>|body|yes|The clientId collection belonging to this user  
message|UpMsg|body|yes|Push message content definition  

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task identification of this transmission|


#### Reports the read status of a message  

> The read state of the update message is read</br>
> If the message burns after reading, the terminal will update the message to read state, and other terminals will no longer receive the same message.  

##### 1、The interface definition

?> **Access address:** `/msg/reprotStatus`</br>
**HTTP Method：** POST 

**Input parameters:** 

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task id received by the terminal  


**Output parameters:**   

Standard output parameter



#### Query history messages  

> 1、Users can query historical messages up to a year old </br>
> 2、Cross-app queries are not supported for historical messages </br>
> 3、Support for terminal query (appId+userId+clientId), businessType query (appId+userId+clientId+businessType), tag query (appId+userId+clientId+tag)</br>
> 4、Support for pagination in reverse chronological order</br>

##### 1、The interface definition

?> **Access address:** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 


**Input parameters:** 

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
businessType|Integer|body|no|Message business type
tag|String|body|no|Custom label
pageIndex|Integer|body||The current page
pageSize|Integer|body||Number per page  



**Output parameters:** 

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
retData|List<MsgClientHiustoryDto>|body|yes|Push record information

#### Delete the history messages in the application  


>1、The user submits an application to delete one or more in-app messages in bulk</br>
>2、Upon receiving the request, the system marks the relevant message as deleted(table MSG_DISPATCH  MSG_STATUS = 5)




##### 1、The interface definition

?> **Access address:** `/msg/delMsgHistory`</br>
**HTTP Method：** POST 

**Input parameters:** 

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|One or more message task ids, separated by commas

**Output parameters:** 
Standard output parameter

## Cloud function interface  

?> Services are provided externally using the REST interface style, and only the HTTPS protocol is supported.</br>
访问地址：`https://uws.haier.net/umse/v3`  </br>
**For access security, the cloud interface needs to set up the caller IP whitelist when it is called.**

**Cloud application request Header**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
appId|String|header|yes|For the identity ID of terminal application, appId and appKey are obtained by applying for cloud application through hgeek network
appVersion|String|header|no|Apply version id, up to 32 characters  
sequenceId|String|header|yes|Message stream number, 6-32 bits; Defined and generated by the client itself; Date + sequential numbering is recommended.  
sign|String|header|yes|Through this parameter, the caller is authenticated, and the algorithm is detailed in the public description
timetamp|long|header|yes|Unix time stamps, accurate to milliseconds  
content-type|String|header|yes|Must be applicationg/json;charset=UTF-8

### The message push

#### Push messages by user

> The list of users receiving messages must be registered from the specified APP.

##### 1、The interface definition

?> **Access address:** `/msg/pushByUsers`</br>
**HTTP Method：** POST 

**Input parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
toUsers|List<String>|body|yes|List of user ids to receive the message
toApps|List<String>|body|yes|The list of apps that receive messages
messages|Message|body|yes|Push message content definition
tag|String|body|no|The label. For example, when the family push can be credited to the family
isBurn|Integer|body|no|Is burn after reading  

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task identification of this transmission  


#### Push messages by app

> The cloud pushes messages to the terminal according to the application list  

##### 1、The interface definition

?> **Access address:** `/msg/pushByApps`</br>
**HTTP Method：** POST 

**Input parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
toApps|List<String>|body|yes|A list of appids that accept messages  
businesssType|Integer|body|yes|Message business type
message|Message|body|yes|Push message content definition
isBurn|Integer|body|no|Is burn after reading

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task identification of this transmission

#### Push messages by user (support template)  

> The list of users receiving messages must be registered from the specified APP.  

##### 1、The interface definition

?> **Access address:** `/msg/pushWithTmplByUsers`</br>
**HTTP Method：** POST 

**Input parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
toUsers|List<String>|body|yes|List of user ids to receive the message  
toApps|List<String>|body|yes|The list of apps that receive messages  
messages|Message|body|yes|Push message content definition  
tag|String|body|no|The label. For example, when the family push can be credited to the family
isBurn|Integer|body|no|Is burn after reading
templateId|String|body|yes|The template identifier
templateParams|Map<String,string>|body|yes|Map.Entry.key Must be the only

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task identification of this transmission  

#### Push message by application (support template)

> Support pushing messages to app lists.  

##### 1、The interface definition

?> **Access address:** `/msg/pushWithTmplByApps`</br>
**HTTP Method：** POST 

**Input parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
toApps|List<String>|body|yes|A list of appids that accept messages  
tag|String|body|no|Tags, such as home push, can be stored as home identifiers  
message|Message|body|yes|Push message content definition
isBurn|Integer|body|no|Is burn after reading
templateId|String|body|yes|The template identifier
templateParams|Map<String,string>|body|yes|Map.Entry.key Must be the only

**Output parameters:**

Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|String|body|yes|The task identification of this transmission  

### TaskId queries for history messages  


> View a push task, push the message list.  

##### 1、The interface definition

?> **Access address:** `/msg/getMsgHistory`</br>
**HTTP Method：** POST 

**Input parameters:**


Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
taskId|Sting|body|no|The message identifier  



**Output parameters:**


Parameter names|Type|Location|Required or not|Note  
:-:|:-:|:-:|:-:|:-
retData|List<MsgCloudHistoryDto>|body|yes|Push record information  





[^-^]:文本连接注释
[MessagePush_document_url]:#

[^-^]:常用图片注释
[MessagePush_type]:_media/_MessagePush/MessagePush_type.png
[MessagePush_liucheng]:_media/_MessagePush/MessagePush_liucheng.png


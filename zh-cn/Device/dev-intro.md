#  介绍

## 简介
	
>  为Haier U+云平台提供用户设备绑定、解绑设备、获取用户设备列表等相关设备管理标准版服务的指导说明，作为第三方应用程序开发的依据和输入。


## 设备类服务架构详情

![设备类服务架构详情][framework] 


## 公共结构说明

**AuthInfo**
权限内容，其中至少一项为ture

参数名|类型|说明|备注
:-|:-:|:-:|:-
view|Boolean|是否有查看权限|
set|Boolean|是否有配置权限|
control|Boolean|是否有控制权限|

**Permission**
权限信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
auth|AuthInfo|权限内容|
authType|String|权限类型|home：家庭分享</br>share：个人分享</br>owener：设备主人</br>server：给appserver的权限

**DeviceBriefInfo**
设备简明信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
wifiType|String|设备WiFitype|
deviceType|String|设备类别|
online|Boolean|是否在线|

**DeviceInfo**
绑定设备信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
wifiType|String|设备wifi|
deviceType|String|设备类型|
totalPermission|AuthInfo|权限和，权限信息的综合|
permissions|Permission[]|权限信息|
online|Boolean|是否在线|
productNameT|String|产品型号名称|
productCodeT|String|产品型号编码|
deviceRole|String|设备角色：1 普通设备;2 网关设备;3 附件设备;4 子设备;|仅返回数字，角色信息不返回
parentDeviceId|String|主设备Id：若该设备本身为主设备，该字段为null；若该设备为从设备，该字段为主设备Id|
deviceRoleType|String|三种设备类别：主设备；从设备；无|
slaveDeviceIds|List<String>|所属从设备Id的集合：若无从设备该字段为空|

**BaseProperty**
基础属性

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|设备品牌|
model|String|设备型号|
others|Map<String,Strng>|其他属性|

**DeviceVersion**
设备详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
modules|Set<Module>|模块信息|
wifiType|String|wifi类型|
deviceType|String|设备类型|
baseProperty|BaseProperty|品牌信息|
location|Location|位置信息|
productNameT|	String|	产品型号名称	
productCodeT|	String|	产品型号编码	
deviceRole|	String|	设备角色：1 普通设备;2 网关设备;3 附件设备;4 子设备;仅返回数字，角色信息不返回	
parentDeviceId|	String|	主设备Id：若该设备本身为主设备，该字段为null；若该设备为从设备，该字段为主设备Id	
deviceRoleType|	String|	设备类别：三种：主设备；从设备；无	
slaveDeviceIds|	List<String>|	所属从设备Id的集合：若无从设备该字段为空	


**Module**
模块信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
moduleId|String|模块ID|
moduleType|String|模块类型|
ModuleInfos|Map<String,String>|模块其他信息|

**Location**
定位信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
longitude|Double|经度|
latitude|Double|纬度|
cityCode|String|城市编码|

**DeviceNetQualityDto**
设备信号强度

参数名|类型|说明|备注
:-|:-:|:-:|:-
softwareType|String|软件类型，平台信息|
hardware|String|硬件版本类型|
hardwareVers|String|硬件版本号|
softdwareVers|String|软件版本号|
netType|String|网络类型|可取值：</br>unknown,位置网络或设备不支持挽留过质量上报；</br>Wifi：WIFI网络
strength|String|信号强度|

**DevNetQuality**
设备网络质量详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
machineId|String|设备id标识|
isOnLine|Boolean|在线状态|true：在线，false：离线
statusLastChangeTime|Long|设备状态变化时间|-1:无效的时间，>0正常值
netType|String|设备连接网络类型|如：“Wifi”，如不存在，则为空字符串“”
ssid|String|设备连接的网络名称|如不存在，则为空字符串“”
rssi|Integer|网络信号强度|-100：无效值；正常值范围：最大值4，最小值-96；
prssi|Integer|网络信号强度百分比|-1：无效的百分比；>0正常值
signalLevel|Integer|网络信号质量等级|0：未知；1：优；2：良；3：合格；4：差
ilostRatio|Integer|广域网丢包率|-1: 无效的丢包率值（不参与信号等级计算）；>0 正常值
its|Integer|广域网延时|单位：ms，-1：无效的延时（不参与信号等级计算）；>0 正常值
lanIP|String|内网ip|如不存在，则为空字符串“”
moduleVersion|String|模块版本描述|如不存在，则为空字符串“”，版本格式：软件版本号/软件类型/硬件版本号/硬件类型

**DeviceStatus**
设备状态

参数名|类型|说明|备注
:-|:-:|:-:|:-
timestamp|long|时间戳|
deviceId|String|设备Id|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
statuses|Map<String,String>|设备状态|

**RoomInfoLocation**
房间位置

参数名|类型|说明|备注
:-|:-:|:-:|:-
userId|String|用户ID|
deviceId|String|设备Id|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
room|String|设备房间位置信息| 

**DeviceRoomInfoDto**
设备房间位置信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceName|String|设备名称，等同于别名|
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
wifiType|String|设备wifitype|
deviceType|String|设备类别|
room|String|设备房间位置信息|
permission|Permission[]|权限信息|
online|Boolean|是否在线|

**BrandInfo**
品牌/型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
brand|String|品牌|
model|String|型号|
deviceId|String|设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符

**DevFWVersion**
设备整机固件版本信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
model|String|U+产品整机型号编码|
vers|String|设备当前固件版本号。字符串格式，不做其余格式校验|
firmware|Firmware|设备可升级的新版本固件信息|如果设备不支持升级或者当前没有更新的固件，则不存在firmware字段    

**Firmware**
设备可升级的新版本固件信息  

参数名|类型|说明|备注
:-|:-:|:-:|:-
vers|String|新版本固件版本号。字符串格式，不做其余格式校验|
desc|String|新版本固件描述信息|
timeout|Int|升级超时时间，单位：秒，取值范围：10~604800（7天）|升级超时时间平台仅用于记录，并且在升级进度及结果中返回给用户，具体超时判断由App进行  
digestAlg|String|设备固件文件摘要算法，可取值：sha256 -- SHA256计算摘要|   
digest|String|设备固件文件摘要信息，格式：全小写十六进制字符串|
url|String|设备固件文件URL地址|
keyAlg|String|设备固件文件加密算法。如果设备固件未加密，则值为空字符串。可取值：aes256 -- AES256加密|  
key|String|设备固件文件加密密钥，格式：全小写十六进制字符串。如果设备固件未加密，则值为空字符串|  
size|int|原始升级包大小|

**User**
用户信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
loginId|String|用户名（邮箱）|
userId|String|用户Id|
userProflie|Map|用户拓展属性|

**OpPropertyValue**
属性操作

参数名|类型|说明|备注
:-|:-:|:-:|:-
name|String|属性|
value|String|值|

**OpResult** 
操作结果

参数名|类型|说明|备注
:-|:-:|:-:|:-
usn|String|操作序列号|
deviceId|String|操作设备ID|长度范围：1~16 格式：大写字母和数字 不包含特殊字符  
result|String|操作应答结果|是一个base64码，标准模型设备解密后的结果为：`{"extData":{},"args":[]}`，其中[]中的数据为多个由name,value组成的键值对；</br>非标准模型设备解密后的结果为:`{"extData":{},"statuses":[]}`，其中[]中的数据为多个由name,value组成的键值对

**DeviceBaseInfo**
设备基本信息

参数名|类型|说明|备注
:-|:-:|:-:|:- 
deviceId|String|设备Id|
sern|String|机器编码|
productCodeT|String|产品型号编码|
productNameT|String|产品型号名称|
connectionStatus|String|设备连接状态|online：在线；offline：离线

**ProductInfo**
产品型号信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
ProdcutCodeT|String|产品型号编码|
ProcutNameT|String|产品型号名称|  

**DeviceInfomation**
设备详细信息

参数名|类型|说明|备注
:-|:-:|:-:|:-
productCode|String|成品编码|
modelCode|String|型号名称|  
productImg1|String|型号图片url|本图片为图片1
typeId|String|typeId|


**ServiceDeviceBindStatusRequestDto** 
查询设备绑定信息请求

参数名|类型|说明|备注
:-|:-:|:-:|:-
deviceId|String|设备身份标识|

**ServiceDeviceBindStatusResultDto**
查询设备绑定信息结果

参数名|类型|说明|备注
:-|:-:|:-:|:-
status|Boolean|绑定状态|true: 已经绑定 false: 未绑定
bts|Long|时间戳|云端生成的绑定时间戳



[^-^]:常用图片注释
[framework]:_media/framework.png
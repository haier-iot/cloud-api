1、嵌入统计分析uSDK
1.1.	安装和部署
欢迎使用海尔用户行为数据统计分析 SDK(简称 SDK)，您可以按照如下步骤开始 SDK 的统计分析功能。  

1.1.1.	获取 AppId 和 AppKey
登录海极网，注册用户并申请开发者，创建应用后即可获得 AppId 和AppKey，具体可参见海极网的“申请开发者流程”。  

海极网:[http://www.haigeek.com](http://www.haigeek.com)  

1.1.2.	下载 SDK 及资料
开发者在海极网注册成功，成为海尔 U+开发者后，可以到海极网的文档中心-APP 开发-统计分析 SDK 中下载 SDK、demo 及开发者手册，开始自己程序开发之旅。  

下载地址:[http://www.haigeek.com](http://www.haigeek.com)  

1.1.3.	导入统计分析 SDK

由于数据分析 SDK 升级需要，除了引入 uAnalytics.framework 之外，还需要引入如下系统库文件到应用工程中，方能保证数据分析 SDK 正常运行。系统库文件如下:libsqlite3.0.tbd 或者 libsqlite3.dylib、CoreTelephony.framework、SystemConfiguration.framework。
1.1.4.	iOS 网络访问新特性

在 WWDC16 中，Apple 表示将继续在 iOS10 和 macOS10.12 里收紧对普通 HTTP 的访问限制。从 2017 年 1 月 1 日起，所有的新提交 app 默认是不允许使用 NSAllowsArbitraryLoads 来绕过 ATS 限制的，开发者可以选择使用NSExceptionDomains 来针对特定的域名开放 HTTP 应该要相对容易过审核。  

如果你想 App 中的网址都通过 ATS 验证，只有少数一些网址例外的话， 你可以为少数的网址添加 NSExceptionDomains,并且在下面添加你需要添加的网址即可。然后对每个网址进行分别的设置，其中将NSIncludeSubdomains 和 NSExceptionAllowsInsecureHttpLoads 设置成YES，NSExceptionRequiresForwardSecrecy 设置为 NO 即可不通过 ATS 验证。  

开发者需要在项目 info.plist 进行如下设置:
 ![info-pllist][info-pllist]
1.1.5.	帐号申请

全网器生命周期平台不再使用海极网的账号，迭代为使用员工工号登录， 登录前需要进行权限申请，申请完成，相关业务线审后，会自动开放权限。具体操作，请参照权限开通手册，地址如下: [https://data.haier.net/showdoc/web/#/p/d056b942e1c0e0233e7a58b8dc0afa2f](https://data.haier.net/showdoc/web/#/p/d056b942e1c0e0233e7a58b8dc0afa2f)  

1.1.6.	验证数据

当您完成以上的 SDK 嵌入工作后，启动 app，触发 SDK 统计接口，权限审批完成后，您可以大数据 ES 系统中查看上报的数据。  


生产环境:http://10.199.127.141:8080/  

ES 系统的相关疑问，请联系杨强，邮箱:yangqiang.uh@haier.com  

导入并成功使用 SDK，实时日志查询可以看到统计的原始数据(保证使用的数据上报策略为实时上报 REALTIME)，对应的数据 cmd 含义如下表:

| CMD  | 含义                                          |
| ---- | --------------------------------------------- |
| 1001 | 首次启动                                      |
| 1002 | 启动                                          |
| 1003 | 时长类事件(具体事件见时长类 EventID 列表)     |
| 1004 | 非时长类事件(具体事件见非时长类 EventID 列表) |
| 1005 | 异常上报                                      |
| 1006 | 地理位置信息统计                              |

如果经过几分钟后，还未看到实时指标更新，请检查以下事项:    
1、设备的 wifi 是否打开，是否正常联网;  

2、APPID，APPKEY、权限等设置是否正确;  

3、确保已触发 SDK 统计接口;  

4、查看 LOGCAT 日志，是否有错误日志。   


2、统计分析 SDK3.x 版本重大变化

1、3.x 版本不兼容 2.x 版本，升级到 3.x 版本后，需要修改程序代码。  

2、删除启动事件 EventIdConst.APP_LAUNCHED_EVENT 定义，不需要开发者显示调用 SDK 启动事件，改为 MobEvent 的 onResume 和 onPause 方法  

中对启动状态进行判断，开发者必须强制调用一次。“e_app_start”为 SDK 保留字，不允许开发者使用或重复定义。  

3、删除数据上报策略方法，设置数据上报策略方法改为在。  

4、采用加密方式与云平台进行传输数据。  


3、统计分析 SDK 业务详解

3.1.	设置日志输出级别

为了方便开发期间查看行为统计信息，自统计分析 SDK3.1.01 版本开始， 提供了设置日志级别的功能方法，默认为 INFO 级别，该方法可在 SDK 启动前后调用。  

开发期间建议使用 UALogLevelDebug 级别，app 发布前建议使用UALogLevelNone 或 UALogLevelError 级别，避免重要信息泄露。    

方法:[uAnalytics setLogLevel:UALogLevelDebug];     

调用位置：SDK 启动前调用。

3.2 启动 SDK

SDK 启动是使用其他 API 功能的前提，该方法中需要开发者设置 APPID（必填，由海极网颁发）、APPKEY（必填，由海极网颁发）、渠道名称（选填）、广告标识符（选填）、数据中心（选填、默认为中国）、发送策略（选填,默认实时）等信息。   


方法：
+(void)startWithOptions:(uAnalyticsStartOptions *) startOptions;  
 参数：startOptionsuSDK 启动参数对象，包含如下信息： AppId:必填，海极网分配的。
Appkey:必填，海极网分配。  

policy:发送策略，选填,默认实时，可选为实时发送和启动时发送。  
  
Area：数据中心，选填、默认为中国。另支持东南亚数据中心。  

channel:渠道号,选填,为 nil 或@“”时,SDK 自动作为@"AppStore"渠道处理。  

idfa:广告标识符，选填,可以为 nil 或@“”。  

调用位置：应用启动时调用。  
示例代码：

 ![示例代码][pic7]


3.3.	地理位置信息统计功能

当开发者可以通过系统 API、百度地图、高德地图等方式获得地理位置信息，并通过如下方法上报到云平台，调用该方法一次上报一次地理位置信息。建议程序启动一次只调用一次该方法。  

方法:
+(void)setLatitude:(double)latitude longitude:(double)longitude;   
参数:  

latitud 纬度  
 
longitude 经度  

调用位置: 需要上报经纬度信息时调用。    

示例代码:[uAnalytics setLatitude:latitude longitude:longitude];


3.4.	uSDK 版本统计

APP 使用 uSDK 的 Framework 且 uSDK 启动成功后,可以调用如下接口设置 uSDK 版本信息，此版本信息将在发生异常上报和绑定设备时使用。如果不调用此方法设置 uSDK 版本信息，在发生异常上报和绑定设备的信息中 uSDK 版本将为””。  

如果 APP 没有嵌入 uSDK 的 Framework，不使用此接口。  
方法:+(void)setUSDKVersion:(NSString *)uSDKVersion;

参数:
uSDKVersion:uSDK 版本号，uSDK 启动成功后通过 API 获取.  

调用位置:uSDK 启动成功后调用此方法    

示例代码:[uAnalytics setUSDKVersion:uSDKVersion];


3.5.	用户统计功能

在获得用户唯一标识符后，APP 每次启动调用如下接口，该接口会记录用户唯一标识符并设置到 SDK 的请求消息头中，以确定某一用户在使用该应用。  

调用该方法后 SDK 不会立即上传用户唯一标识符，需要调用 SDK 其他业务方法(事件次数和时长类统计方法)后，才能触发该数据上传。  

方法:
+(void)setUserId:(NSString *)userId;
参数:
userId，用户标识符调用位置:
启动 SDK 方法，获得用户唯一标识符后调用。示例代码:
[uAnalytics setUserId:userId];

3.6.	异常信息统计功能

异常信息统计功能分为自动上报崩溃信息和主动上报异常信息两个方法。两个方法不互斥，建议都设置，可以帮助开发者收集应用程序的崩溃信息和异常信息，完善 APP 程序。  

3.6.1.	SDK 自动上报崩溃异常信息

SDK 能够自动捕获未知的崩溃信息并在下次启动时上传服务器,默认为自动上报，开发者可以不调用本方法。  

开发者可以通过将 value 设置为 No 关闭自动崩溃异常收集功能，但不建议关闭此功能。  

方法:
+(void)setExceptionCatchEnabled:(BOOL)value   
参数:  

value 是否打开异常捕获功能。   

调用位置:
在启动本 SDK 之后。  
示例代码:[uAnalytics setExceptionCatchEnabled:YES]

3.6.2.	APP 主动上报异常信息

此方法需要开发者主动调用，将需要关注的异常信息上报给云平台，帮助开发者改进自己的 APP 程序。  


方法一:  

+(void)postExceptionString:(NSString *)exception;  

参数:
exception 出错信息字符串.   
调用位置:需要上报错误信息的地方。  
示例代码:
@try{//代码省略
}@catch(NSException*exception){
[uAnalytics postException:exception.name];
}@finally{
}  

方法二:  

+(void)postException:(NSException *)exception;
  
参数:
exceptio:抛出的异常
  
调用位置:
需要上报错误信息的地方。  
示例代码:
@try{//代码省略
}@catch(NSException *exception){ [uAnalytics postException:exception];
  

}@finally{
}

3.7.	页面统计功能

使用时长类事件的统计功能，统计某个页面的使用情况，只有正确的使用页面统计方法才能获得准确的统计数据。  

3.7.1	标记一次页面访问的开始  

方法:
+(void)eventStart:(NSString *)eventide label:(NSString *)label;  

参数:  

eventId:时长类事件标识，固定为 APP_USE_DURATION
label:事件标签，建议使用英文，使用中文云平台统计会产生乱码。
  
调用位置:  

每个 UIViewController 的(void)viewDidAppear:(BOOL)animated  
 示例代码:

 ![示例代码][pic8]

(注意:每次调用，SDK 会检查是否产生新的会话(session 超时)，即生成启动次数。)  

3.7.2	标记一次页面访问的结束  

方法:  

+(void)eventEnd:(NSString *)eventide label:(NSString *)label;

  
参数:
eventId:时长类事件标识，固定为 APP_USE_DURATION
label:事件标签，建议使用英文，使用中文云平台统计会产生乱码。
  
调用位置:
UIViewController 的-(void)viewDidDisappear:(BOOL)animated   
示例代码:

 ![示例代码][pic9]


这两个调用不会阻塞应用程序的主线程，也不会影响应用程序的性能。注意如果您的 UIViewController 之间有继承或者控制关系，请不要同时在父和子 UIViewController 中重复添加 eventStart 和 eventEnd 方法，否则会造成统计数据混乱。
3.8.	事件次数和事件时长统计功能

可以统计某些用户行为的发生次数，时间，变化趋势等，例如设备控制，app 加载耗时等。通常 event_id 用于表示某种行为或功能的统计， event_id 用于唯一标识一个事件。  

用户行为事件分为 2 大类:    


1、统计次数:统计指定行为被触发的次数。   
 

2、统计时长:统计指定行为消耗的时间，单位为秒。  

需要 eventStart 接口与 eventEnd 接口成对使用才生效。事件参数类型:key-value 即 Objectve-c 语言中的 NSDictionary 数据类型，特殊的事件需要传入指定的 MAP 信息，如设备绑定/解绑和控制设备等，MAP 规范详见MAP 定义。    


目前只支持预定义的 event_id，详见 event_id 列表，填写正确的event_id 才能参与正常的数据统计。目前不支持数据分析网站上注册自定义的event_id 统计。  

3.8.1	普通事件次数统计

可以统计事件的发生次数，用于统计用户登录次数、Button 点击次数、用户行为触发次数等。EventId 的使用，见 4.7.2 非时长类 EventID 定义章节。  
方法一:
+(void)event:(NSString *)eventId;  

参数:
eventId:非时长类的无参数事件标识。  

调用位置:按 eventId 的功能描述在相应的位置调用    

方法二:、
+(void)event:(NSString *)eventide acc:(NSInteger)accumulation;   
  
参数:
eventId:非时长类的无参数事件标识
acc:事件发生次数，不能为负值，负值时按 0 处理。  
  
调用位置:按 eventId 的功能描述在相应的位置调用

方法三:
+(void)event:(NSString *)eventide attributes:(NSDictionary *)attributes;  

参数:
eventide:非时长类的有参数事件标识
attributes: key-value 参数对，key 和 value 都是 NSString 类型。   
 
调用位置:
按 eventId 的功能描述在相应的位置调用。  
示例代码:
NSDictionary*map_value=[NSDictionarydictionaryWithObjectsAndKeys:  

@"0007A88A522C",@"mac",  

@"eSDK_WIFI_AC",@"pf", @"e_1.0.09",@"sv",  

@"G_1.0.00",@"hv",  

@"smartlink",@"ct",  

@"0000",@"rc",  

@"40044",@"fi", @"00000000000000008080000000041410",@"tid",nil];  

[uAnalytics event:DEVICE_BIND_EVENT attributes:map_value];  
 方法四:  

+(void)event:(NSString *)eventid attributes:(NSDictionary *)attributes acc:(NSInteger)accumulation;  

参数:

eventId:非时长类的有参数事件标识。
attributes:key-value 参数对，key 和 value 都是 NSString 类型。  
acc:事件发生次数，不能为负值，负值时按 0 处理。    


调用位置:按 eventId 的功能描述在相应的位置调用。  
示例代码:
NSDictionary*map_value=[NSDictionarydictionary WithObjectsAndKeys:  
@"0007A88A522C",  
@"mac",   
@"eSDK_WIFI_AC",
@"pf",   
@"e_1.0.09",  
@"sv",  
@"G_1.0.00",  
@"hv",  
@"smartlink",   
@"ct",  
@"0000",  
@"rc",   
@"40044",  
@"fi",  

@"00000000000000008080000000041410",  
@"tid",nil];[uAnalytics event:DEVICE_BIND_EVENT attributes:map_valueacc:3];

3.8.2	打点(埋点)事件次数统计

打点事件又称埋点事件，是普通事件次数统计的一种特例，eventId 固定为USER_CLICK_EVENT 的事件。当现有普通事件次数统计功能不能满足开发者的需求时，可以参考打点事件。如果打点事件也不能满足开发者需求，需要自定义，请阅读 3.7.4 自定义事件章节。  

方法一:

+(void)event:(NSString*)eventide attributes:(NSDictionary*)attributes; 参数:
eventId 非时长类的有参数事件标识，固定值 USER_CLICK_EVENT attributeskey-value 参数对，key 和 value 都是 NSString 类型,key 的值固定为”actioncode”,value 值，需要和云平同事协商其具体意义。
调用位置:
按 eventId 的功能描述在相应的位置调用示例代码:
NSDictionary*attrs=[NSDictionary dictionaryWithObjectsAndKeys:@"value",@"actioncode",nil]; [uAnalytics event:USER_CLICK_EVENT attributes:attrs];

方法二:
+(void)event:(NSString *)eventide attributes:  
(NSDictionary*)attributesacc:(NSInteger)accumulation;  

参数:  

eventID:非时长类的有参数事件标识，固定值 USER_CLICK_EVENT attributes:key-value 参数对，key 和 value 都是 NSString 类型,key 的值固定为”actioncode”,value 值需要和云平同事协商其具体意义  

accumulation:事件发生次数，不能为负值，负值时按 0 处理。    

调用位置:按 eventId 的功能描述在相应的位置调用

示例代码: NSDictionary*attrs=[NSDictionarydictionaryWithObjectsAndKeys:  
@"val ue",  
@"actioncode",nil];  

[uAnalytics event:USER_CLICK_EVENT attributes:attrs];  

关于 param 中 value 值的具体定义需要和云平台同事进行商定，具体工作请联系杨强，邮箱:yangqiang.uh@haier.com

3.8.3	事件时长统计

可以对耗时类事件的时长进行统计，主要用于用户登陆耗时、APP 加载耗时、页面加载耗时、APP 使用时长等功能的统计。eventStart 和 eventEnd 方法必须成对出现，且参数列表完全相同，才能正常上报事件。  
EventId 的使用，见4.7.1 时长类 EventID 定义章节。  
如果现有时长类的 EventId 不能满足开发者的使用需求时，请阅读 3.7.4 自定义事件章节。  

方法:  

可以指定事件的开始和结束时间，来上报一个带有统计时长的事件。  

+(void)eventStart:(NSString*)eventIdlabel:(NSString*)label;   
参数:
eventId:时长类事件标识，见 4.7.1 时长类 EventID 定义章节。  

label:事件标签，建议使用英文，使用中文云平台统计会产生乱码。如:页面的名称、自定义的名称。  

调用位置:

按照时长类事件 eventId 功能描述调用。  
示例代码:  

[uAnalytics eventStart:USER_LOGIN_DURATION label:@"LoginViewController"];
//代码省略  

[uAnalytics eventEnd:USER_LOGIN_DURATION label:@"LoginViewController"];  

3.8.4 自定义事件

目前，由于云平台现有机制，暂不支持 EventID 用户自定义，也不支持统计分析网站上注册自定义的 eventID。如果现有 EventID 列表不能满足用户使用需求，与云平台同事进行沟通商定后，才能够对上报的统计数据进行统计。大数据生命周期平台同事，杨强，邮箱:yangqiang.uh@haier.com  


4、数据定义

.1.	设备绑定 MAP 定义

| 含义                       | KEY  | 示例 VALUE                               |
| -------------------------- | ---- | ---------------------------------------- |
| 设备的 mac(mac)            | mac  | 0007A88A522C                             |
| 设备平台信息(platform)     | pf   | UDISCOVERY_UWT                           |
| 模块软件版本(software_ver) | sv   | e_1.0.09                                 |
| 模块硬件版本(hardware_ver) | hv   | G_1.0.00                                 |
| 配置方式(config_type)      | ct   | Smartlink、softap                        |
| 失败原因(fail_info)        | fi   | 40044(函数 setDeviceConfigInfo 的返回值) |
| 绑定结果(return_code)      | rc   | 0000(见 4.6)                             |
| Typeid(type_id)            | tid  | 00000000000000008080000000041410         |
| uSDK 版本(usdk_ver)        | uv   | C2.0.02_2014062418_S2.1.00_2014070217    |

4.2.	设备解绑 MAP 定义

| 含义                  | KEY  | 示例 VALUE                                       |
| --------------------- | ---- | ------------------------------------------------ |
| 设备的 mac(mac)       | mac  | 0007A88A522C                                     |
| 解绑结果(return_code) | rc   | 云平台返回结果                                   |
|                       |      | 注意:只有 APP 调用解绑接口时，才需要调用该接口。 |

4.3.	设备控制 MAP 定义

| 含义               | Key  | Value                            |
| ------------------ | ---- | -------------------------------- |
| 设备 MAC 地址(mac) | mac  | 0007A88A522C                     |
| Typeid(type_id)    | tid  | 00000000000000008080000000041410 |
| 操作设备命令(cmd)  | cmd  | 205001                           |
|                    |      |                                  |

注意:目前只支持单命令，不支持组命令

4.4.	打点事件 MAP 定义

| 含义         | Key        | Value                  |
| ------------ | ---------- | ---------------------- |
| 打点事件代码 | actioncode | 和云平台同事商定具体值 |

4.5.	渠道列表

  ![][pic10]

  ![][pic11]

  ![][pic12] 

4.6.	设备绑定错误列表

   ![][pic13]

  ![][pic14]

4.7.	EventID 定义
4.7.1	时长类 EventID 定义

| EventID                          | 值           | 含义            | 参数 |
| -------------------------------- | ------------ | --------------- | ---- |
| EventIdConst.USER_LOGIN_DURATION | t_user_login | 用户登陆耗时 ID | 无参 |
| EventIdConst.APP_LOAD_DURATION   | t_app_load   | APP 加载耗时 ID | 无参 |
| EventIdConst.PAGE_LOAD_DURATION  | t_page_load  | 页面加载耗时 ID | 无参 |
| EventIdConst.APP_USE_DURATION    | t_app_use    | App 使用时长 ID | 无参 |

4.7.2	非时长类 EventID 定义

| EventID                           | 值           | 含义        | 参数   |
| --------------------------------- | ------------ | ----------- | ------ |
| EventIdConst.DEVICE_BIND_EVENT    | e_dev_bind   | 设备绑定 ID | 见 4.1 |
| EventIdConst.DEVICE_UNBIND_EVENT  | e_dev_unbind | 设备解绑 ID | 见 4.2 |
| EventIdConst.DEVICE_COMMAND_EVENT | e_dev_cmd    | 设备控制 ID | 见 4.3 |
| EventIdConst.USER_CLICK_EVENT     | t_user_click | 打点事件 ID | 见 4.4 |

5、注意事项

.1.	编码格式

SDK 编码格式默认 UTF-8，传递带中文参数请使用 UTF-8 编码，避免出现乱码。

5.2.	兼容问题

对于早期的开发者，若使用 3.0 之前的旧 SDK，使用现在的 SDK3.0 以上版本时报错，是由于新 SDK 不兼容旧版本导致的，需要开发者修改程序代码。





[^-^]: 常用图片注释

[info-pllist]:../App-dev/_media/info-pllist.jpg
[pic7]:../App-dev/_media/pic7.jpg
[pic8]:../App-dev/_media/pic8.jpg
[pic9]:../App-dev/_media/pic9.jpg
[pic11]:../App-dev/_media/pic11.jpg
[pic12]:../App-dev/_media/pic12.jpg
[pic13]:../App-dev/_media/pic13.jpg
[pic14]:../App-dev/_media/pic14.jpg
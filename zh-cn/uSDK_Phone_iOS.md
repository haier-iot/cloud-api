!>  **当前版本**：[uSDK_Phone_iOS V5.0]()  
 **更新时间**：{docsify-updated}

## 1 uSDK5.0简介
uSDK是一款移动应用开发套件，它能够支持目前主流的Android和iOS系统。它允许您运用Java、Object-C、Swift等创建App和智能硬件互动。
通过uSDK可以实现的功能包括智能设备配置入网、搜索、状态查询、控制、接收报警。


### uSDK4.x工程迁移到uSDK5.x
 如果您想从uSDK4x版本代码基础上升级使用uSDK 5.x版本，开发者只需要使用uSDK5.x版本的uSDK.framework和configFiles.bundle替换原工程中的uSDK.framework和configFiles.bundle即可。


## 2.实验开发环境搭建

### 2.1.	官网、QQ群
官网： 海极网 http://www.haigeek.com
官方QQ群：U+开发者技术讨论群457841032

### 2.2.	路由器环境设置
目前海尔智能设备只能匹配2.4g频段路由，才能正常工作和使用，暂不支持5g频段。为了使APP开发者能够顺利进行开发，建议APP开发者仔细阅读此章节后对路由器进行如下设置。

#### 2.2.1.	路由无线网络基本设置
由于当前市场上路由品牌和手机品牌丰富多样，手机和路由联合工作时容易存在兼容性问题（如11bgn mixed模式），导致手机、路由、海尔智能设备不能正常协同工作（始终无法配置成功、始终不能发现U+智能设备或者配置成功率时高时低），这时可以参考如下截图对路由设置进行更改。
注意：SSID号不建议使用中文，建议使用英文和数字。因为中文字符涉及到编解码问题，当APP、路由器编解码机制不同时，将导致设备无法配置成功。


截图一:
![路由无线设置图片1][router_setting1]
 
截图二：
![路由无线设置图片2][router_setting2]


#### 2.2.2.	路由转向设置
在海极网申请开发App后，网站会给出我们开发或测试环境信息，开发测试阶段使用开发者环境，发布时转入正式生产环境。
由于市场上U+智能设备默认连接生产环境服务器，APP开发时需要和U+智能设备处于同一环境（开发者环境），才能进行远程的开发测试，通过对路由进行转向设置，把智能设备和APP（uSDK）通过同一部路由连接到同一环境。如下是配置开发环境举例，配置完成后通过ping uhome.haier.net，查看配置是否成功.见截图：
![路由dns设置图片][router_dns_setting]


### 2.3.搭建实验测试环境
实验设备和步骤：
1.	一台2.4G无线路由器，参考2.2章节配置路由器，并连接互联网
2.	一台iOS 7.0及以上的iOS手机连接路由器，手机中安装uSDK相关App
3.	一台支持U+协议的智能设备或U+Dev开发板，接通电源i
4.	操作设备进入待入网模式
5.	使用uSDK功能的APP配置智能设备入网并查看智能设备是否上线

相关资料：
配置设备入网演示视频：QQ群文件
uSDK、uPlug、uGW 、DevKit、uPlug SDK、OPEN API等更多相关资料，见海极网下载中心。

### 2.4	搭建程序开发环境
####  2.4.1下载开发手册、uSDK.framework和uSDKDevDemo
下载地址：ht tp://www.haigeek.com/

![ios开包下载][iOS_uSDK_Pone_haigeek_down_address]


####  2.4.2.导入库文件
例如采用Xcode进行开发，首先将uSDK.framework和configFiles.bundle（文件存放在uSDK.framework包中）文件添加到工程，并在工程中添加系统库libc++.dylib或者libc++.tbd，uSDK引入就成功了。
####  2.4.3.iOS网络访问新特性
 在WWDC 16中，Apple表示将继续在iOS 10和macOS 10.12里收紧对普通 HTTP 的访问限制。从 2017年1月1日起，所有的新提交 app 默认是不允许使用NSAllowsArbitraryLoads来绕过 ATS 限制的，开发者可以选择使用NSExceptionDomains来针对特定的域名开放 HTTP 应该要相对容易过审核。
如果你想App中的网址都通过ATS验证，只有少数一些网址例外的话，你可以为少数的网址添加NSExceptionDomains,并且在下面添加你需要添加的网址即可。然后对每个网址进行分别的设置，其中将NSIncludeSubdomains和NSExceptionAllowsInsecureHttpLoads设置成YES，NSExceptionRequiresForwardSecrecy设置为NO，即可不通过ATS验证。
开发者需要在项目info.plist进行如下设置：
![infoplistsetting][ios_infoplist_setting]

### 2.5.uSDKDevDemo程序
U+开发平台网站提供一款操作电热水器的示例demo ，演示uSDK配置设备入网及同一无线网络与设备互动的情景。
uSDKDevDemo包含的功能
![iosdemo功能介绍][ios_demo_func_desc]

##  3.业务梳理及开发快速入门
本章总结使用uSDK的两个常见开发情景：配置设备入网、使用uSDK进行设备交互。下面给出各自流程图，使开发者大致了解业务运行流程。
###  3.1基本业务接入流程
本图简要介绍uSDK物联App主要业务运行流程，红色文字是uSDK交互，黑色文字是App与U+云交互：
![简单业务运行流程][devleop_step_simple]

注意：
启动uSDK、发现网络设备这两步与登录U+云平台、获取U+云账号设备列表可以互调位置，具体调用方法根据APP实际业务确定


###  3.2详细业务接入流程
本图介绍uSDK和物联App之间主要业务交互运行流程，红色文字是uSDK交互，黑色文字是App与U+云交互：
![简单业务运行流程][devleop_step_detail]


###3.3配置入网流程
参考下列流程开发配置设备入网功能，特别说明：成功启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。详细介绍请参考“章节4.2配置设备入网”。
![配置业务运行流程][public_config_step]


## 4.uSDK业务开发详解
### 4.1.uSDK启动
启动uSDK是调用各种功能性API，使用U+物联功能的前提，在主线程中启动一次即可。下面分别讲解启动uSDK及启动前后前后需要进行的必要设置：设置设备管理委托、设置过滤设备类型、启动uSDK、成功后设定uSDK日志级别、开启和关闭dns防劫持功能。

相关概念和术语
	AppID：在海极网为物联App申请的AppId，用于App校验
	AppKey：在海极网申请，用于与OPEN API交互时使用。APP开发中只有“开发测试” 的APP_KEY。开发完成且提交海极网审核完成后会发放“生产” 的APP_KEY，APP使用生产的APP_KEY进行发布。
	SecretKey：在海极网申请，uSDK使用

#### 4.1.1设置设备管理委托
uSDK启动前，APP开发者需要实现uSDKDeviceManagerDelegage委托并设置委托，才能在uSDK成功启动后，得到变化的设备集合，对设备列表集进行管理，详情见4.3章节 管理设备集变化中的管理设备列表集合。
[uSDKDeviceManager defaultDeviceManager].delegate = self。

#### 4.1.2设置过滤设备类型
uSDK启动前，设置开发者关心的设备类型，可以是一个，也可以是多个；若不设置，默认为全部类型。设置过滤类型后，只能收到或查询到自己关心类型的设备，其他类型的设备将被过滤掉。多次执行[uSDKDeviceManager defaultDeviceManager]. interestedDeviceTypes进行赋值，将覆盖已设定的设备过滤类型，以最后一次为准。
示例代码：
[uSDKDeviceManager defaultDeviceManager]. interestedDeviceTypes = ALL_TYPE

#### 4.1.3启动uSDK
启动uSDK是使用U+物联功能的前提，按如下方式启动uSDK。

	[[uSDKManager defaultManager] startSDKWithAppId:appDelegate.APPID appKey:appDelegate.APPKey secretKey:appDelegate.SERCERTKEY success:success failure:failure];

appId：海极网分配的appId，不能为空，不能随意填写。
appKey：海极网分配的appkey，不能为空，不能随意填写。
sercertKey：海极网分配的sercertKey，不能为空，不能随意填写。
代码块success启动成功时被触发，可以设置日志级别。
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。
注意
1.uSDK启动时需要到到海尔云端验证appId和version值的合法性，所以appId和Version的值一定要与海极网中的填写的数据一致，如果不一致，会导致appId和version值的合法性验证失败，uSDK不能启动
2.uSDK在主线程中成功启动一次即可，不要多次或在其他个线程中启动uSDK

#### 4.1.4设定uSDK日志级别
uSDK启动成功后，可以设置uSDK的日志级别，开发过程中建议使用USDK_LOG_DEBUG，上线产品建议使用USDK_LOG_NONE或USDK_LOG_ERROR，如不设置，默认为USDK_LOG_DEBUG输出所有日志。uSDK运行时会输出日志，其中包含与硬件交互及反馈给App的详细日志，uSDK的日志标签是uClient和uServer。
示例代码：

	[[uSDKManager defaultManager] setLogWithLevel:USDK_LOG_DEBUG isWriteToFile:NO success:^{
	 
  	} failure:^(NSError *error) {
  
 	 }];

代码块success方法执行成功时被触发。
代码块failure方法执行失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

#### 4.1.5 开启和关闭dns防劫持功能
为了保证用户能够正确访问到海尔云，uSDK中提供了dns防劫持功能， 默认此功能开启。
当开发人员需要使用非生产环境（开发者、联调、验证）进行开发测试时，在sdk启动后关闭uSDK提供的DNS防劫持功能，才能保证 uSDK正常访问对应的云环境。
当APP需要对外发布时，需要开启DNS防劫持功能，保证用户能够正确访问到海尔云。
API介绍如下：

1、开启全部特性

     	[[uSDKManager defaultManager] enableFeatures:uSDKFeatureDefault];

2、关闭全部特性

    	[[uSDKManager defaultManager] enableFeatures:uSDKFeatureNone];

### 4.2 退出uSDK
App需要退出或者不需要使用U+物联功能时需要停止uSDK

    [[uSDKManager defaultManager]stopSDKWithSuccess:^{
       
	} failure:^(NSError *error) {
       
	}];

##  4.3配置设备入网
配置设备入网就是使用uSDK将设备加入指定无线网络，或更改U+设备所在网络的一项操作。uSDK支持SmartLink、SoftAP两种方式入网，下面讲分别进行讲解。
uSDK4.X支持您使用加密方式发送配置信息，如果您的智能产品包含不支持加密入网的型号，则使用普通配置方式，以保证所有销售产品都能正常入网。如果产品全部支持加密入网，建议使用加密入网方式，提升整套产品安全性能。
  
预备知识
请先了解章节4.1启动uSDK。配置入网演示视频请至U+开发者QQ群下载。

相关术语和概念
	uPlug：负责智能设备或家电联网的WIFI物联集成电路板。

###  4.3.1.SmartLink配置方式
SmartLink方式是成熟稳定的设备入网方式，是U+平台物联设备的主流入网方案，通过将无线配置信息发送给待入网设备，完成配置入网动作。
uSDKDeviceManager.configDeviceBySmartLinkWithSSID有多个重载方法，可以指定MAC、TYPEID、超时时间、安全配置等参数，完成不同需求的配网功能。

#### 设备配置入网操作过程
1)	确认当前路由器频段是2.4G。按U+智能设备说明书指导，操作设备进入配置模式，此时uPlug指示灯有规律快闪，实体设备会有相应的指示灯闪烁，说明设备进入待入网模式。
2)	确认手机稳定连接目标路由器，设备安置位置WIFI信号良好，打开基于uSDK的App（通用测试App、uSDKDevDemo程序），将配置入网信息发送出去。设备收到配置信息后熄灭信号指示或发出蜂鸣声，手机不切换网络等待设备上线。
3)	App显示有新设备上线，说明配置成功。App提示配置失败，说明操作失败，重新按照1，2，3步骤进行。

#### 执行SmartLink配置方式
App把无线的名字和密码发给待入网的设备，示例代码如下：

    	void(^sucess)(uSDKDevice* device) = ^(uSDKDevice* device){
		};

    	void(^failure)(NSError* error) = ^(NSError* error){ 
		};

		[[uSDKDeviceManager defaultDeviceManager]configDeviceBySmartLinkWithSSID:ssid password:pwd deviceID:mac timeoutInterval:60 security:NO success:sucess failure:failure];

configDeviceBySmartLinkWithSSID：配置设备入网方法。

ssid：无线网络名称，不能为空，最大长度31。

pwd：无线网络密码，可以为空，最大长度63。

mac：设备mac地址。不知道的话，填写@”";

timeoutInterval：配置超时时间，单位为秒。范围5-120秒，推荐60秒。

security：安全配置方法标识。YES时，进行安全配置；NO时，普通配置。

代码块success配置成功时触发，device不为nil，设备配置入网成功，device携带设备信息。

代码块failure配置失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 判断设备入网是否成功及获得设备实例
配置成功，代码块success被触发，device不为nil说明设备入网成功，它就是入网设备对象实例，此时它携带设备网络及产品类型等基本信息。

##### 中断设备配置入网
在执行配置设备入网过程中可以调用API中断，中断动作结果将通过回调参数通知App。

    	[[uSDKDeviceManager defaultDeviceManager]stopSmartLinkConfig:^{

		} failure:^(NSError *error) {
           
		}];
代码块success中断配置成功时触发。
代码块failure中断配置失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

###  4.3.2SoftAP配置方式
SoftAP配置方式是将uPlug设置为WIFI热点，手机连接uPlug热点，然后把无线配置信息发送给uPlug的一种入网方式。

##### 步骤一 连接设备热点
 参考使用说明让设备进入待配置模式，此时物联模块工作于WIFI热点模式，打开手机无线局域网，可以看到设备热点（U-开头），将手机连接到设备的WIFI热点。

##### 步骤二 创建uSDKDeviceConfigInfo对象
执行此步骤的前提是将手机成功连接到智能设备的WIFI热点上。
创建uSDKDeviceConfigInfo对象并设置必要的基本信息，用于传递给设备热点。
开发者需要向configInfo对象中写入正确的wifi名称（不支持中文，最小长度1，最大长度31）和密码（不支持中文，可以为空，最大长度63）才能保证设备是可以配置成功的。

    uSDKSoftApConfigInfo * configInfo = [[uSDKSoftApConfigInfo  alloc]initWithSSID:self.SSITText.text password:self.passwordText.text]; configInfo.timeoutInterval = 80;
    	configInfo.security = NO;

##### 步骤三 执行SOFTAP配置

    	[[uSDKDeviceManager defaultDeviceManager]configDeviceBySoftapWithConfigInfo:configInfo
   			sendConfigInfoSuccess:^{
   			[AlertViewTools shouAlertViewWithTitle:@"提示"
  			 Msg:@"配置信息发送成功，请切换到目标网络"];
		} success:^(uSDKDevice *device) {
  		 	//配置成功的设备
   			[self showFindDevice:device]; 
		} failure:^(NSError *error) {
   			self.configResultLable.text = @"设备配置失败!";
		 }];

configInfo：装载配置信息的对象
sendConfigInfoSuccess： 配置信息发送成功时被触发，需要切网到目标网络
success：配置成功时被触发，device是配置成功的设备。
Failure：配置失败时被触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 步骤四 切换回配置目标网络
代码块sendConfigInfoSuccess执行成功后，需要切换回目标路由的网络，等待设备上线。

配置失败可能原因：网络原因、无线密码输入错误，或者写入无线密码并没有成功。

### 4.4管理设备集变化
uSDK启动完成后会不断扫描本网络里的U+设备，完成设备发现、设备离线，维护设备集合中设备数量及数据的变化，App通过实现设备列表变化回调实现设备集合变化的管理。本章分别讲解：管理变化设备列表集合、获得设备池全集的方法。

预备知识
请先了解章节3“快速入门” 
相关概念和术语
	小循环：uSDK和U+设备处于同一无线局域网完成设备交互的情形。
	TYPEID：TYPEID就是设备类型的标识字符串，用它可以识别U+平台上的各种硬件设备。
	设备连接状态
未连接：App还没连上智能设备，表示设备加入WIFI已发现。调用uSDKDevice的state 属性值是uSDKDeviceStateUnconnect。
离线：设备发现过并且App进行了连接，但此时无法收到设备的响应数据。uSDKDevice             的state属性值是uSDKDeviceStateOffline。
	设备的属性状态（属性）：开机、关机、运行等值。

##### 管理变化设备列表集合
uSDK会将网络里所有的智能设备都管理起来，形成一个设备列表的集合，并进行管理。设备列表集合的数量会根据实际设备的数量变化而发生变化，例如：配置一台新设备入网并上线，集合中会增加一台设备；设备切断电源然后下线，集合中会减少一台设备。
APP开发者需要实现uSDKDeviceManagerDelegage委托并设置委托，通过实现如下方法，可得到变化的设备列表；

###### 设置委托：

    	uSDKDeviceManager* deviceManager =  [uSDKDeviceManager defaultDeviceManager];
		deviceManager.delegate = self;//设置代理

###### 实现委托：

		-(void)deviceManager:(uSDKDeviceManager *)deviceManager didAddDevices:(NSArray<uSDKDevice *> *)devices{
   			//    devices中是收到的最新发现的设备集合，demo中使用的是设备全集
   			self.deviceList =  [[uSDKDeviceManager defaultDeviceManager].deviceDict.allValues;
   			[self.myTableView reloadData];
		}

		-(void)deviceManager:(uSDKDeviceManager *)deviceManager didRemoveDevices:(NSArray<uSDKDevice *> *)devices{
  			 //    devices中是下线的设备集合，demo中使用的是设备全集
   			self.deviceList = deviceManager.deviceDict.allValues;
  			[self.myTableView reloadData];
		}

devices：变化的设备列表集合。uSDK启动前设置委托，第一次接收到当前设备池里的所有设备集合，之后接收到的是状态发生变化的设备集合。uSDK启动成功后设置委托，第一次接收到的设备集合可能是所有设备集合，也可能是状态发生变化的设备集合。示例中使用uSDK的API获取所有设备的全集，使用变化的集合还是使用设备全集，需要根据实际业务需求确定。

##### 获得所有设备全集的方法

	[[uSDKDeviceManager defaultDeviceManager].deviceDict.allValues;


### 4.5管理设备连接状态变化
当设备状态（uSDK与设备连接状态）发生变化时，uPlug模块或云平台会及时的通知uSDK，uSDK会上报收到的设备状态给APP，用于更新UI界面或进行其他业务处理。设备连接状态分为：未连接、连接中、已连接（连接成功）、就绪、离线，五个状态值。关于设备状态值的具体定义，请参考7.1.16 uSDK与设备连接状态值定义
App开发者需要为每个设备实例实现uSDKDeviceDelegate委托并设置委托，然后实现如下方法，可以收到设备的上下线状态变化推送。

###### 设置委托：

	uSDKDeviceManager* deviceManager =  [uSDKDeviceManager defaultDeviceManager];
	deviceManager.delegate = self;//设置代理

###### 实现委托：

    	-(void)device:(uSDKDevice *)device didUpdateState:(uSDKDeviceState)state error:(NSError *)error{
 			self.deviceList = [[uSDKDeviceManager defaultDeviceManager]getDeviceList:ALL_TYPE];
   			[self.myTableView reloadData];
		}

当设备变为离线状态时可以通过error类获取错误码和错误描述。开发者可以根据错误码和错误描述，分析设备离线原因，自行进行处理。

### 4.6	与设备建立或断开连接
发现设备且设备处于未连接状态时，App连接或断开智能设备将触发连接状态的变化。本章分别讲解：执行连接设备、执行断开设备连接操作。

##### 设备的5种连接状态：
未连接状态：智能设备正常入网后被uSDK发现时的状态
连接中状态：执行连接智能设备，智能设备变为连接中
已连接状态:与智能设备连接成功后，变为已连接状态
就绪状态：调用连接查询属性的方法，与设备连接成功后收到上报属性后的状态
离线状态:连接失败或通信中断会变为离线

![connectstatus_change_step][connectstatus_change_step]


#### 执行连接设备
当需要获得某台设备属性数据或进行命令功能控制时，需要先执行设备连接方法。目前uSDK只支持连接单个设备，不支持同时连接多个设备，如有需要请逐个方法调用。

开发者执行设备连接操作前，可以通过设备的isSubscribed属性对设备的状态进行判断，为NO时进行连接操作，如果是YES，不需要再次对设备执行连接操作。

##### 1、连接设备但不获得设备属性方法：
使用uSDKDevice对象执行该方法，该方法执行成功后，需要等待一定时间App的设备连接状态将变为“连接成功/已连接”状态，不会出现“就绪” 状态。

	[self.currentDevice connectWithSuccess:success{
	
	} failure:{
	
	}];
代码块success执行成功时触发，等待一定时间后设备状态变为“已连接或连接成功”状态。
代码块failure执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述


##### 2、连接设备并获得设备属性方法：
执行连接设备并获得设备属性方法，该方法执行成功后，等待一定时间设备会变为“就绪” 状态，设备就绪状态时可以执行设备控制和获取的设备的属性集合。

	[self.currentDevice connectNeedPropertiesWithSuccess:^{
        
	} failure:^(NSError *error) {
        
	}];

代码块success执行成功时触发，等待一定时间后设备状态将变为“就绪” 状态。
代码块failure执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。


#### 执行断开设备连接
不关注某台设备属性数据时，执行断开连接设备方法，释放设备资源。只支持单个设备断开连接，不支持同时断开连接多个设备，如有需要请逐个方法调用。

    [self.currentDevice disconnectWithSuccess:^{
       
 	} failure:^(NSError *error) {
      
	}];

#### 获取设备的连接状态
通过uSDKDevice的state属性，获取设备的连接状态

注意
1．uSDK启动成功后，App可以在任意时刻执行连接设备操作（设备对象引用不为nil）。
2．开发中一台设备只执行一次设备连接操作即可，多次执行连接操作将使设备下线再上线。
3．断开某设备连接后，uSDK将不再上报此设备的任何属性消息给App。


### 4.7 管理设备属性状态变化
设备就绪后，App就能实时获得设备的最新属性值集合。本章主要讲解：实现获得设备的上下线状态接口方法，实现获得设备的属性状态接口方法，主动获得设备的所有属性状态等内容。

相关术语
六位码：六位码是一个键值对，在程序当中是对象uSDKDeviceAttribute，App使用它和设备进行沟通交流。举例：20w001代表开关机功能，App通过uSDK发送uSDKDeviceAttribute｛20w001:20w001｝就是开机，发送uSDKDeviceAttribute｛20w001:20w002｝关机，查询设备属性时uSDKDeviceAttribute｛20w001：20w001｝说明电视开机。
ID开发文档：六位码的集合文档，主要用途是明确设备操作指令及属性集等。

#### 实现获得设备的属性状态方法
App开发者成功连接设备操作并实现uSDKDeviceDelegate委托并设置委托，通过如下方法获得设备的属性值集合推送； 
##### 设置委托：
 self.currentDevice.delegate = self;

##### 实现委托：
    	-(void)device:(uSDKDevice *)device didUpdateValueForAttributes:(NSArray<uSDKDeviceAttribute *> *)attributes{
    	 	self.attrDict = self.currentDevice.attributeDict;
         	[self.myTableview reloadData];
		}

attributes：第一次收到attributes时，它包含该设备所有的属性值的全集，之后收到的是变化的属性集合。

#### App主动获得当前设备所有属性值
当设备就绪或已连接状态时，uSDKDevice对象的attributeDict属性中保存设备当前最新属性值合集，非就绪状态属性返回值无意义。
示例代码：
 self.currentDevice.attributeDict;

###	4.8设备故障和报警
设备就绪或已连接状态下，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载。报警信息六位码具体含义参考相关ID文档。本章讲解：获得报警消息、发送停止报警指令、获得报警解除消息、主动查询设备报警等内容。

预备知识
与设备建立连接，使设备就绪请参考章节4.4，单命令及如何向设备发送指令请参考章节4.7。

#### 获得报警消息
App开发者需要成功连接设备并实现uSDKDeviceDelegate委托并设置委托，通过如下方法，可以获得报警消息；
##### 设置委托：
 self.currentDevice.delegate = self;

##### 实现委托：

    -(void)device:(uSDKDevice *)device didReceiveAlarms:(NSArray<uSDKDeviceAlarm*>*)alarms{
     	 if (alarms.count<=0) {
       		return;
     	 }
  		NSString *alarmStr = @"";

  		for (uSDKDeviceAlarm *alarm in alarms){
   			if ([alarm.alarmMessage isEqualToString:@"506000"] ) {
  				 //506000 报警解除，不是报警
   				continue;
   			}else{ 
  			 //开发者收到报警时，需要根据开发文档对报警进行处理。如发送停止报警命令
  			 alarmStr = [alarmStr stringByAppendingFormat:@"%@ - %@,",alarm.alarmTimestamp,alarm.alarmMessage];
  			}
   		}
	}

#### 发送停止报警指令
当智能设备或家电报警时，其可能向uSDK规律性快速报警，App通知用户或记录信息之后，可以向设备发送停止报警指令，用于表示使用者已经了解设备已经发生故障。停止报警对于App来讲只是一条普通单命令，停止报警的指令参考设备的ID文档。

#### 获得报警解除消息
发生故障的设备修理正常后会向uSDK发送报警解除消息，对于App来讲报警解除就是没有报警，就是设备正常。报警解除消息与普通报警消息都在-(void)device:(uSDKDevice *)device didReceiveAlarms:(NSArray<uSDKDeviceAlarm*>*)alarms方法中处理，App注意分辨。报警解除的六位码请参考设备ID文档。

#### 主动查询设备报警
App可以调用API主动查询设备报警信息

    NSArray<uSDKDeviceAlarm*>* alarmList = self.currentDevice.alarmList;
self. currentDevice：uSDKDevice对象，App查询设备是否有报警信息
alarmList：设备当前的报警信息列表

###  4.9.执行设备控制
海尔智能设备至今大多使用ID文档进行开发，通过6位码的键值对方式对海尔智能设备控制，可以给设备发送单命令，也可以发送组命令，可以设定超时时间，命令执行后会有结果与设备反馈。

随着U+平台的日趋开放,为了兼容来自不同厂家的各类物联设备及终端应用软件，海尔制定了统一的物联设备建模标准,对各种物联设备的基本属性、功能等信息进行规范化的描述，最终形成标准设备模型文档，以下简称标准文档 。

标准文档是海尔的主推方向，需要和uSDK3.x或uSDK4.x、标准设备，三方面一起使用才能完成对设备的控制。标准文档的使用需要参考标准文档模型和下面的写属性、操作命令进行开发。uSDK2.x不支持标准文档。

设备处于就绪或连接成功状态，是控制设备的必要前提条件。当执行连接设备方法成功后，且设备处于就绪或已连接状态时，可以向设备发送控制指令。核心类uSDKDevice提供API用于执行设备控制。下面将以6位码的ID文档和标准设备模型文档为例，下面将分别讲解单命令和组命令控制。



##### 相关术语和概念
	TYPEID：TYPEID就是设备类型的标识字符串。uSDK的功能是通用的，它可以识别U+平台各种硬件设备，使用者可以使用TYPEID区分设备类型。

#### 4.9.1.	发送单命令（6位码）
单命令：App只发送一个指令给设备
![public_single_cmd][public_single_cmd]

 App开发者使用uSDKDevice对象发送单命令时，需要严格遵守ID文档规定，发送指定的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。不能使用发送组命令的方法发送单命令。
示例代码：

    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){
        void(^success)(void) =^{
            UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"命令执行成功" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
            [alertView show];
        };
        void(^failure)(NSError* error) = ^(NSError* error){
            NSString *str = [NSString stringWithFormat:@"错误原因:%@",[error description]];
            NSString *title = [NSString stringWithFormat:@"%@ 命令执行失败",name];
            UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:title
                    message:str delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
            [alertView show];
        };
        [self.currentDevice writeAttributeWithName:name value:value success:success failure:failure];
        
    }else{
        UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"设备非连接成功或就绪状态,不可交互" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
        [alertView show];
        return;
    }
name：属性名，NSString类型。如”204003”

value：属性值，NSString类型。如”204003”

代码块success命令执行成功时触发。

代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。 

#### 4.9.2.发送组命令（6位码）
组命令是由组命令号或组命令标识、单命令集合两部分组成的。
组命令：App发送多条指令的集合，设备接收并执行
![public_group_cmd_sixcode][public_group_cmd_sixcode]

“单命令/组命令”说明当前指令204001可以既作为单命令发送，也可以运用到组命令中。

ID文档中的组命令号或组命令标识： 分为10进制和16进制两种
![public_group_cmd_stand][public_group_cmd_stand]

组命令号/组命令标识是ID文档中已经预定义好的，开发者不允许变更。现有ID文档或者标准文档中的组命令号/组命令标识存在16进制和10进制两种，然而uSDK中执行组命令时要求传递的组命令号必须是16进制6为长度的字符串（字母大写）。开发者在使用10进制的组命令号时需要先将10进制的组命令号转化为16进制（字母大写），然后左端补“0”，形成6位的16进制字符串。

例如ID文档的组命令号19807，转换补位后是“004D5F”，“004D5F”就是uSDK要求的目标组命令号。

例如ID文档的组命令号0x6001，转换为“006001”，“006001” 就是uSDK要求的目标组命令号。

例如ID文档的组命令号4d5f，补位后是“004D5F”，“004D5F”就是uSDK要求的目标组命令号。

命令集合是按ID文档中组命令要求，组合成的单命令的集合。App开发者发送组命令时，需要严格遵守ID文档中的组命令规定，不能添加除ID文档中要求的组命令外的任何命令，且组命令中单命令的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。

示例代码：以组命令号19807为例。

    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){
    NSString* groupCmdName = @"004D5F";
    void(^success)(void) =^{
        UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"命令执行成功" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
        [alertView show];
    };
    void(^failure)(NSError* error) = ^(NSError* error){
        NSString *str = [NSString stringWithFormat:@"错误原因:%@",[error description]];
        UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"命令执行失败" message:str delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
        [alertView show];
    };
    //timeoutInterval值的单位是秒，默认为15s，取值范围5-120s，建议15s左右
    
       [self.currentDevice executeOperation:groupCmdName args:cmdList timeoutInterval:5 success:success failure:failure];
   }else{
       UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"设备非连接成功或就绪状态,不可交互" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
       [alertView show];
       return;
       
   }

groupName：组命令标识字，NSString类型。长度6位的16进制大写字符串。

cmdList：uSDKArgument对象实例的集合

5：超时时间，单位是秒

代码块success命令执行成功时触发。

代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

注意
1．此方法只能发送组命令，组命令要求的所有命令都需要放入cmdList，且没有顺序要求。
2．组命令标识字具体值参考设备的ID文档，需要传入长度6位的16进制大写的字符串。
3．App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。

#### 命令超时设定
uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。自定义超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。

#### 4.9.3.	写属性命令（标准文档）
海极网支持创建智能设备，创建完成后会产生《XX设备应用开发文档》，以下将说明如何使用此文档与设备进行交互。

操作属性
操作属性的含义是此项可以作为智能设备的属性，可写列为T时，此项可以作为命令发往设备；可写列为F时，表示此命令不可用，不能作为命令发往设备。
![public_op_attr_stand][public_op_attr_stand]




[^-^]:常用图片注释
[router_setting1]:_media/_usdk/router_setting1.png
[router_setting2]:_media/_usdk/router_setting2.png
[router_dns_setting]:_media/_usdk/router_dns_setting.jpg
[iOS_uSDK_Pone_haigeek_down_address]:_media/_usdk/iOS_uSDK_Pone_haigeek_down_address.png
[ios_infoplist_setting]:_media/_usdk/ios_infoplist_setting.png
[ios_demo_func_desc]:_media/_usdk/ios_demo_func_desc.png
[devleop_step_simple]:_media/_usdk/devleop_step_simple.png
[public_config_step]:_media/_usdk/public_config_step.png
[devleop_step_detail]:_media/_usdk/devleop_step_detail.png
[connectstatus_change_step]:_media/_usdk/connectstatus_change_step.png
[public_single_cmd]:_media/_usdk/public_single_cmd.png
[public_group_cmd_sixcode]:_media/_usdk/public_group_cmd_sixcode.png
[public_group_cmd_stand]:_media/_usdk/public_group_cmd_stand.png
[public_op_attr_stand]:_media/_usdk/public_op_attr_stand.png




[^-^]:文本连接注释
[usdk_document_url]:_document/_usdk/uSDK5.0_Phone_Android开发手册_20180717173928794.pdf


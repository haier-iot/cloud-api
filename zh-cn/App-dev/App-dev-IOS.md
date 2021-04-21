# 实验开发环境搭建

## 搭建实验测试环境

### 准备设备和软件

①.一台2.4G无线路由器，参考2.2 章节配置路由器，并连接互联网
②.一台iOS 9.0 及以上的iOS 手机连接路由器，手机中安装uSDK 相关App
③.一台支持U+协议的智能设备（安全版本智能设备）或U+Dev开发板，接通电源
④.操作设备进入待入网模式
⑤.使用uSDK 功能的APP 配置智能设备入网并查看智能设备是否上线

### 动手搭建环境

①将路由器加电并配置连接互联网，打开2.4G频段
②手机连接路由器2.4G频段
③智能设备接通电源
④操作设备进入待入网模式
⑤使用硬件测试工具App或uSDK5XDemo App配置智能设备入网
⑥使用硬件测试工具App或uSDK5XDemo App查看智能设备是否入网

## 搭建程序开发环境

### 导入库文件

例如采用Xcode 进行开发，首先将uSDK.framework 和configFiles.bundle
（文件存放在uSDK.framework 包中）文件添加到工程，并在工程中添加系统库libc++.dylib 或者libc++.tbd，uSDK 引入就成功了。

###  iOS网络访问新特性

在WWDC 16 中，Apple 表示将继续在iOS 10 和macOS 10.12 里收紧对普通 HTTP 的访问限制。从 2017 年1 月1 日起，所有的新提交 app 默认是不允许使用NSAllowsArbitraryLoads 来绕过 ATS 限制的，开发者可以选择使用NSExceptionDomains 来针对特定的域名开放 HTTP 应该要相对容易过审核。如果你想App 中的网址都通过ATS 验证，只有少数一些网址例外的话，你可以为少数的网址添加NSExceptionDomains,并且在下面添加你需要添加的网址即可。然后对每个网址进行分别的设置，其中将NSIncludeSubdomains 和NSExceptionAllowsInsecureHttpLoads 设置成YES，NSExceptionRequiresForwardSecrecy 设置为NO，即可不通过ATS 验证。
开发者需要在项目info.plist 进行如下设置：
![info.pllist](C:\Users\01503602\Desktop\GitHub\info.pllist.png)

### 在XCode 的Other linker flag 中设置-ObjC

集成uSDK5.4.0 及以后版本，需要在Build settings 中的Other linker flag
中添加-ObjC 属性。如截图：
![Objc](C:\Users\01503602\Desktop\GitHub\Objc.png)

### 开发测试环境配置及确认

我们在海极网申请开发App后，使用开发者环境开发测试，产品发布时转入生产环境，配置uSDK特性，使uSDK工作在恰当的环境里。
U+智能设备（uPlug）默认连接生产环境，App开发测试阶段，修改路由器DNS，智能设备配置到此路由，保证设备连接开发者环境。
AppId、Appkey统一通过海极网上申请，开发者中心-我的应用 ，移动应用、云应用。开发者环境使用的AppKey或systemKey为开发key值

### 配置开发者环境

设备联网使用的路由器DNS设置为39.97.52.209 
注意：wifi模块的版本需要支持，smartDevice设备需要关闭HttpDNS服务。

### 配置验收环境

DNS为：182.92.120.221
Ping gw.haier.net得到的IP为： 182.92.120.221
Ping uws.haier.net得到的IP为： 123.57.182.237 或 47.93.125.250
Ping uhome.haier.net得到的IP为：47.93.125.250 或 123.57.182.237

# 业务梳理及开发快速入门

## 基本业务流程

本图简要介绍uSDK物联App主要业务运行流程，红色文字：App与uSDK交互，黑色文字：App与U+云交互：

![基本业务流程](C:\Users\01503602\Desktop\GitHub\基本业务流程.png)

## 配置入网流程

参考下列流程开发配置设备入网功能，特别说明：启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。

![配置入网流程](C:\Users\01503602\Desktop\GitHub\配置入网流程.png)

## 使用uSDK控制设备流程说明图

以下流程图适用于使用uSDK进行设备配置入网，控制的App，OPENAPI请更换为UWS或对应服务。

![IOS控制流程图](C:\Users\01503602\Desktop\GitHub\IOS控制流程图.png)

# uSDK业务开发详解

启动uSDK是使用U+物联功能的前提。为App配置好海极网验证信息，启动uSDK。如果果需要，紧接着设定uSDK日志级别。

### 启动uSDK

首先执行初始化方法，然后执行具体启动方法：
//推荐在Application对象实现中执行,
//或者在所有uSDK操作之前，入参需要是ApplicationContext
uSDKManager.init(ApplicationContext);

### 初始化启动参数

uSDKStartOptions USDK_START_OPTION = new uSDKStartOptions.Builder()

### 设定uSDK日志级别

uSDK默认输出所有日志，日志标签是uSDK。uSDK提供API支持App设定日志输出级别，uSDK启动成功后，执行即可：
uSDKManager.initLog(uSDKLogLevelConst level, boolean isWriteToFile, IuSDKCallback)。 
开发测试阶段将日志级别设置为DEBUG

### 启动uSDK

uSDKMgr.startSDK(USDK_START_OPTION, new ICallback<Void>() 
注意：
①uSDK成功启动一次即可，不要多次、多个线程启动
②uSDK成功启动之后再调用uSDK提供的各类方法
③启动入参Context必须是ApplicationContext

## 配置设备入网和绑定

产品配置入网后就直接绑定，首先请完成账号登录拿到账号session/token，并使uSDK连接U+云。

### SmartLink入网并绑定

①App在智能设备入网准备阶段（例如引导操作设备和输入无线密码），构造SmartLink绑定信息类：
SmartLinkBindInfo smartLinkBindInfo = new SmartLinkBindInfo.Builder()
②入网和绑定，在onSucess回调中获得设备对象实例，此时设备已经被绑定，可以通过UWS接口查询到设备信息：
bindUtil.bindDeviceBySmartLink(smartLinkBindInfo, new IBindResultCallback<uSDKDevice>() 

### SoftAP入网并绑定

①询问目标WIFI时，构造SoftAP绑定信息类
SoftApBindInfo.Builder builder = new SoftApBindInfo.Builder();  
②手机成功连接设备热点U-XXXX时，执行入网和绑定
Binding.getSingleInstance().bindDeviceBySoftAp(softApBindInfo, new ISoftApResultCallback<uSDKDevice>()
此接口回调时说明配置信息已经成功发送，App继续执行务必让手机成功连接和设备相同的WIFI。

### 蓝牙绑定WIFI设备

支持通过蓝牙配置设备连接WIFI，并且绑定（硬件要支持优家的蓝牙技术或标准）。
①首先实现并设置蓝牙待入网设备回调，先注册再使用：
DeviceScanner.setListener(new IDeviceScannerListener()
②调用API开始扫描：
DeviceScanner.startScanConfigurableDevice(new IuSDKCallback()
③App准备绑定信息，向用户收集无线名称、密码拼装BLEBindInfo对象：
bleBindConfigInfo = new BLEBindInfo.Builder() 
④执行设备绑定：
Binding.getSingleInstance().bindDeviceByBLE( 		canciate, bleBindConfigInfo, new IBindResultCallback<uSDKDevice>() 
⑤停止扫描
设备绑定成功，停止扫描并询问关闭蓝牙
DeviceScanner.stopScanConfigurableDevice(new IuSDKCallback()
stopScanConfigurableDevice：停止搜索方法
⑥ConfigurableDevice对象的有用方法
ConfigurableDevice.getBleName()：返回蓝牙设备的名称
ConfigurableDevice.getDeviceId：返回设备在无线时的DeviceId，通常是设备MAC
ConfigurableDevice.getType()：获得设备大类

### 纯蓝牙智能设备绑定

支持绑定蓝牙设备。
①设置扫描目标类型：原生蓝牙
DeviceScanner.setFeature(DeviceScanner.BLE_SCAN);
DeviceScanner.BLE_SCAN：扫描目标设备是蓝牙设备，带蓝牙WIFI设备会被过滤掉，不会出现
②首先实现并设置扫描回调，先注册再使用：
DeviceScanner. setListener(new IDeviceScannerListener()
③调用API开始扫描：
DeviceScanner.startScanConfigurableDevice(60, new IuSDKCallback() 
startScanConfigurableDevice：启动扫描方法，需要启动成功，需要手机提前开启蓝牙功能。
④App准备绑定信息，向用户收集无线名称、密码拼装BLEBindInfo对象：
PureBLEBindInfo info = new PureBLEBindInfo.Builder() 
⑤执行设备绑定：
Binding.bindPureBLEDevice(
info, new IBindResultCallback<uSDKDevice>() 
⑥停止扫描：
DeviceScanner.getSingleInstance() .stopScanConfigurableDevice(new IuSDKCallback()

### 绑定SmartDevice担当网关外围设备

支持绑定SmartDevice实现网关外围设备。本功能uSDK实质上仅仅是发送“开始”指令给网关设备，然后监听网关与外围设备组网及绑定的相关消息。另本命令是通过用户网关发送给网关设备，所以功能实现时需要您提前绑定设备且网络良好，请一定注意。
①组织绑定信息
SlaveDeviceBindInfo friendInfo = new SlaveDeviceBindInfo.Builder()
②执行绑定
Binding.getInstance().bindSlaveDevice(friendInfo, new IBindResultCallback<uSDKDevice>()
自带屏幕或操作系统设备入网
对于一些高级、大型家电或智能设备，其自身可能携带操作系统并且带有显示装置，此时我们应将其视为普通计算机，由其自行引导使用者连接网络。

## 发现及管理网络设备

智能设备已经连上WIFI，App实现设备管理接口就能实现设备管理业务，接收uSDK管理设备集变化、设定过滤设备类型、获得可用设备全集。

### 接收uSDK管理设备集变化回调

App依靠uSDK管理智能设备。uSDK将网络里所有的智能设备都管理起来，形成设备池，设备池里设备的数量会发生变化，例如：配置一台新设备入网，配置过的设备上电加入网络等。uSDK将以上情况都汇报给App，App编程实现设备管理。
App实现并注册如下接口方法，发现及管理网络设备：
uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()
注意：
①本接口是即时接口，在uSDK启动前设置。
②如果延时设置或再次设定本接口，需要结合查询设备所有列表使用，确保智能设备列表数据齐全。

### 设定设备过滤类型

App只关心设备池里的某类设备（例如：酒柜设备）：
①App执行如下方法：
uSDKDeviceManager. setInterestType(uSDKDeviceTypeConst.WINE_CABINET)
②App实现管理设备集变化回调：
此时uSDK只向App上报uSDKDeviceTypeConst.WINE_CABINET类型的设备，实现设备过滤。
注意：
再次执行setInterestType方法，将覆盖已设定的设备过滤类型。

### 获得设备池全集

uSDKDeviceManager.getDeviceList()，本方法也支持设备类型过滤。

## 与设备建立或断开连接

现在我们已经发现设备，调用API就可以连接了，连接设备、断开设备连接。

### 设备的连接状态：

连接中(connecting)：App正在与设备建立连接。
已连接(connected)：App已连接智能设备，可以发送控制指令，查询属性状态等，是我们可以交互的设备。
离线(off_line)：App不断开与智能设备连接的情况下，智能设备发生异常。

![设备的连接状态](C:\Users\01503602\Desktop\GitHub\设备的连接状态.png)

未连接、连接中、已连接、离线都是设备（uSDKDevice）的连接状态。App连接或断开智能设备将触发连接状态的变化：
①智能设备正常入网后是未连接（被我们发现）
②连接智能设备后，智能设备变为连接中
③与智能设备连接成功后，变为已连接
④如果连接失败或通信中断会变为离线
⑤断开与智能设备的连接，则变为未连接

### 设备连接状态变化接口

App先注册监听，然后执行连接方法。智能设备的连接状态会通过回调上报：
uSDKDevice.setDeviceListener(new IuSDKDeviceListener(){

public void onDeviceOnlineStatusChange(uSDKDevice device, uSDKDeviceStatusConst status, int statusCode)}）

statusCode信息表

| statusCode值 | 含义                                     |
| ------------ | ---------------------------------------- |
| 50002        | App连接U+云失败                          |
| 50003        | 智能设备连接U+云失败（云端通知设备离线） |
| 50007        | 心跳超时                                 |
| 50008        | App连接设备失败（本地）                  |
| 50009        | App读取智能设备数据失败（本地）          |
| 50010        | App向智能设备写数据失败（本地）          |

### 连接设备

uSDKDevice.connectNeedProperty，使用本方法。

### 断开连接

不需要某台设备属性数据时，断开设备连接，释放设备资源：
uSDKDevice.disconnect。

### 主动查询连接状态

uSDKDevice.getStatus()。
注意：
①断开设备连接，uSDK将不再上报此设备的任何属性消息给App。
②当用户解绑定设备时，一定执行断开设备连接。
③Android设备熄屏或进入后台强制uSDK与设备保持连接请执行：uSDKDeviceManager.setKeepLocalOnline(true)，uSDK启动成功后执行本方法。

## 获得设备属性状态

现在已经连接成功了，我们就能实时获得设备的最新属性值集合与设备交互，获得设备的连接状态和实时属性值。

### 获得设备的属性状态

①App实现并注册如下接口，准备接收设备属性的实时值：
uSDKDevice.setDeviceListener(new uSDKDeviceListener(){

public void onDeviceAttributeChange(uSDKDevice device, List<uSDKDeviceAttribute> propertiesChanged) }）
②App连接智能设备
③App根据ID文档发送查询指令，指令执行成功后，就可以在接口里接收属性数据了。
uSDKDevice.writeAttribute("60w0ZZ", null, new IuSDKCallback()

### App主动获得当前设备所有属性值

当我们第一次通过接口获得设备的属性状态后，只要设备已连接或就绪，调用uSDKDevcie.getDevcieAttrMap返回设备最新属性值合集。

## 处理设备故障和报警

我们连接设备后，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载。
报警信息六位码具体含义参考相关ID文档。获得报警消息、发送停止报警指令、获得报警解除消息、主动查询设备报警等内容。

### 获得报警消息

实现uSDKDeviceListener获得报警消息，报警消息和设备属性状态变化是同一接口。
uSDKDevice .setDeviceListener(new uSDKDeviceListener(){

public void onDeviceAlarm(uSDKDevice myDevice, List<uSDKDeviceAlarm> alarmList)}）

### 发送停止报警指令

智能设备或家电可能向uSDK规律性快速报警，App处理完成后，可以向设备发送停止报警指令，用于表示使用者已经了解设备发生故障。停止报警是一条普通单命令，参考ID文档编写。

### 获得报警解除消息

发生故障的设备修理正常后会发送报警解除消息，报警解除就是没有报警，就是正常。报警解除消息与普通报警消息都在onDeviceAlarm()接口方法产生，App注意分辨。报警解除的具体值，请参考设备ID文档或设备模型文档。

### 主动查询设备报警

App可以调用API主动查询设备报警信息
ArrayList<uSDKDeviceAlarm> mAlarms = mDevice.getAlarmList();

## 执行设备控制

我们和设备已经连接了，可以向设备发送控制指令。uSDKDevice提供API用于执行设备控制。执行控制时，可以给设备发送单命令、组命令，设定超时时间，命令执行后会反馈结果。

### 发送单命令

selecteduSDKDevice.writeAttribute("204003", "214001", new IuSDKCallback()
注意：
App需要严格遵守ID文档规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。

### 发送组命令

①16进制组命令字处理，组命令字要求全部大写。

List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>();

mAttrs.add(new uSDKArgument("20600E", "01:02"));

mAttrs.add(new uSDKArgument("20600F", "02:03"));            

selecteduSDKDevice.execOperation(**"000007"**, mAttrs, 10, new IuSDKCallback() 

②10进制组命令字处理 

组命令标识字为整形时，先转为16进制，再按字符串下发，19807=0x4d5f，不足6个字符补0，得到”004d5f”，转为大写”004D5F”，组命令下发时填写”004D5F”作为组命令字。
List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>();
mAttrs.add(new uSDKArgument("202003", "202003"));				
selecteduSDKDevice.execOperation("004D5F", mAttrs, 10, new IuSDKCallback() {
});
注意：
①组命令说明中需要的指令放入mAttrs，mAttrs没有顺序要求。
②组命令标识字具体值参考设备的ID文档。
③App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。

### 与海极网创建智能设备交互（标准模型设备文档）

海极网支持创建智能设备，创建完成后会产生《XX设备应用开发文档》，以下将说明如何使用此文档与设备进行交互。
1）属性
属性的含义是此项可以作为智能设备的属性，可写列为T时，此项可以作为命令发往设备：
selecteduSDKDevice.writeAttribute("A", "True", new IuSDKCallback() {			
注意：
发送指令时Key和Value大小写敏感（例如：”True”）。
可写列为F时，此行不能作为指令发送。
2）操作
操作的含义是App端可以发送表格描述的操作实现对应的功能，这里包含您在海极网创建的高级命令。
①三种常用指令 
List mAttrs = new ArrayList();
selecteduSDKDevice.execOperation(“getAllProperty”, mAttrs, 10, new IuSDKCallback()
②自定义的高级指令，tempPwdLengthX及其它几项是您设备的属性之一。
List mAttrs = new ArrayList();
mAttrs.add(new uSDKArgument(“tempPwdLengthX”, “10”));
mAttrs.add(new uSDKArgument(“tempPwdPart1X”, “11”));
mAttrs.add(new uSDKArgument(“tempPwdPart2X”, “12”));
……
selecteduSDKDevice.execOperation("Tmpopen", mAttrs, 10, new IuSDKCallback() 
注意：
App需要严格遵守应用开发文档的规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。

### 命令超时设定

uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。建议超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。

### 命令执行结果与设备反馈

命令执行结果在IuSDKCallback回调中给出。uSDKErrorConst.RET_USDK_OK代表发送成功，uSDKErrorConst.RET_USDK_TIMEOUT_ERR代表超时。
设备执行成功有可能改变设备的属性值，App接收设备的最新属性上报即可。

## 远程控制及U+云平台推送消息

经过前面的开发，我们已经可以在本地和智能设备完美交互了，但我们的手机不能切换路由或者使用4G，这么做和智能设备的数据通路会立刻切断。现在我们让App同时连接远程服务器，手机更换WIFI或使用4G则直接通过服务器连接设备。

### 一般的U+物联App如何运行

App执行uSDKDeviceManager.connectToCloud连接用户接入网关，此方法需要用户信息，适时运行方法连接远程服务器。

### 连接用户接入网关

App在日常使用情景中，账号登录后按以下内容编程连接用户网关，获得远程和智能设备交互的能力。
deviceManager.connectToCloud(userInfo, new ICallback<Void>()
账号登录后执行一次即可。

### 远程服务器的连接状态

uSDKDeviceManager提供一个回调，可以了解我们当前与远程服务器的连接状态：
uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()

### 处理绑定一台新设备

当绑定一台新设备时，需要驱动uSDK刷新交互设备列表，uSDKDeviceManager.refreshDeviceList()方法。

### 用户解绑定时解除设备远程能力

用户解绑定设备成功时，已经失去对设备的控制权，断开设备连接，释放设备对象实例，调用uSDKDeviceManager.refreshDeviceList()驱动uSDK刷新设备列表。
注意：
如果不是用户主动解绑设备，但收到接入网关推送的设备解绑定消息（此时设备可能被他人解绑定），App需要编写逻辑处理此消息：
uSDKDeviceManager.setDeviceManagerListener(
new IuSDKDeviceManagerListener()

### 账号注销时的处理

当用户账号注销时，务必调用 uSDKDeviceManager.disconnectToCloud断开远程链接，清空账号数据。 

### 如何测试远程功能是否正常

手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、4G数据网络，查询设备状态、进行控制。
注意：
①当用户切网时，uSDK自动尝试连接用户接入网关，查询设备的最新状态。
②App在开发者环境绑定设备，智能设备也要连接开发者环境，开发环境始终保持一致。
③App使用UWS过程中，U+云平台要求App有自己的ClientId。ClientId和终端的身份验证是相关的，错误或固定的ClientId会产生异常行为，App可以使用uSDKManager. getClientId()方法产生ClientId。

### 接收U+云推送消息

App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。
实现如下方法接收云的推送消息：
uSDKManager.setManagerListener(new IuSDKManagerListener() {
	public void onBusinessMessage(String content)	});

### 接收U+云Session异常消息

当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，检查程序逻辑，查看用户登录地址与连接用户接入网关地址是否同一服务器环境。
uSDKManager.setManagerListener(new IuSDKManagerListener() {
	public void onSessionException(String session) });

## 获取设备WIFI信号强度

可以使用uSDKDevice实例获取设备的Wifi信号强度，注意必须是WIFI设备。
uSDKDevice.getDeviceNetQuality(new IuSDKGetDeviceNetQualityCallback() 

## 和带子机设备交互

带子机设备通常有商用空调、星盒等设备。一台主机可以有多台子机，子机从属于主机，主机拥有mac，子机没有。在程序中我们可以和主机、子机进行交互，主机需要连接，子机不需要连接。

### 获得子机实例

当主机已连接后，通过向主机发送查询指令（根据设备模型文档或ID文档），由主机动态上报子机实例（一会上报一台或连续上报子机实例信息），在以下回调中就可以得到子机实例。
parentuSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
	public void onSubDeviceListChange(
			uSDKDevice currentDevice, ArrayList<uSDKDevice> childArrayList) {
	}
});

### 子机连接状态、属性及报警

子机有三种连接状态：离线、已连接、就绪。子机的属性、报警将通过回调上报给App，实现以下接口并设定到子机对象实例上：
private class ChildDeviceCtrlListner implements IuSDKDeviceListener {}

### 子机控制

子机只要不是离线状态，我们就可以和子机进行交互。对子机的指令仅能影响子机本身。
子机的实例对象也是uSDKDevice。

### 主机控制

我们可以像普通设备一样和主机交互，其中一些指令将影响到附属的所有子机。

### 辅助方法

selectedChildDevice.getMainDevice()：获得主机
selectedChildDevice.isSubDevice()：子机？
selectedChildDevice.isMainDevice()：主机？
selectedChildDevice.getSubType()：子机类型
selectedChildDevice.getSubId()：子机Id
parentDevice.getSubDeviceList()：获得当前有哪些子机
parentDevice.getSubDeviceById()：根据子机Id获得子机实例

## 订阅SmartDevice设备资源

支持您获得SmartDevice设备图片资源（。

### 实现步骤

当需要获得设备资源时，需要提前为设备设置监听IuSDKDeviceListenerWithResource，实现onReceive方法。设备资源名称请您咨询设备实现方，当前设备是否可以存取资源请根据typeid判断。 

### 具体步骤：

①设置资源设备监听实现方法
②连接设备使其就绪
③订阅资源名称
④使用完毕释放资源
selecteduSDKDevice.setDeviceListener(new IuSDKDeviceListenerWithResource()
注意：
订阅资源需要设备就绪，使用完成需要时释放，设备下线再次就绪时继续获得数据。

## 退出uSDK

App不需要使用U+物联功能时可以停止uSDK。
uSDKManager.stopSDK();

## 有用的类和方法

### uSDKDevice有用的方法

uSDKDevice.getIP可以得到设备IP地址。
uSDKDevice.getStatus查询设备的连接状态。
uSDKDevice.getNetType可以得知设备是本地设备还是远程设备。
uSDKDevice.getDeviceType查询设备类型，例如冰箱、洗衣机。
uSDKDevice.getUplusId查询设备uplusId(typeId)。
uSDKManager.getClientId()得到终端ClientId。

### 调用API获得uSDKDevice

uSDKDeviceManager.getDevcieList获得当前可交互的所有设备。
uSDKDeviceManager.getDevice使用DeviceId获得设备实例。
uSDKDeviceManager.getDeviceList(uSDKDeviceTypeConst.XXXXXXX)按类型取。

### 获得uSDK版本号

uSDKManger.getuSDKVersion()

### 获得错误码和描述

uSDKError.getDescription()
uSDKError.getErrorCode()
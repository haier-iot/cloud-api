# **实验开发环境搭建**

## 准备设备和软件

①一台2.4G无线路由器
②一台Android4.4以上智能手机，支持蓝牙BLE
③一台支持U+协议的智能设备（安全版本智能设备）
④硬件测试工具App或uSDKDemoApp

## uSDK权限配置

在使用Android版uSDK进行开发时，需要为uSDK配置相关权限：
<uses-permission 
android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission 
android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission 
android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE"/>
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
以上权限uSDK仅用于和设备及接入网关通信。

## 开发测试环境配置及确认

我们在海极网申请开发App后，使用开发者环境开发测试，产品发布时转入生产环境。
U+智能设备（uPlug）默认连接生产环境，App开发测试阶段，修改路由器DNS，智能设备配置到此路由，保证设备连接开发者环境。
AppId、Appkey统一通过海极网上申请，开发者中心-我的应用 ，移动应用、云应用。开发者环境使用的AppKey或systemKey为开发key值

## 配置开发者环境

设备联网使用的路由器DNS设置为39.97.52.209 
注意：wifi模块的版本需要支持，smartDevice设备需要关闭HttpDNS服务。

# 业务梳理及开发快速入门

## 基本业务流程

本图简要介绍uSDK物联App主要业务运行流程，红色文字：App与uSDK交互，黑色文字：App与U+云交互。

![基本业务流程](C:\Users\01503602\Desktop\GitHub\基本业务流程.png)



## 配置入网流程

考下列流程开发配置设备入网功能，特别说明：启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。
![配置入网流程](C:\Users\01503602\Desktop\GitHub\配置入网流程.png)





## 使用uSDK控制设备流程说明图

以下流程图适用于使用uSDK进行设备配置入网，控制的App，OPENAPI请更换为UWS或对应服务。
![控制设备流程图](C:\Users\01503602\Desktop\GitHub\控制设备流程图.png)

# uSDK业务开发详解

## 启动uSDK

启动uSDK是使用U+物联功能的前提。为App配置好海极网验证信息，启动uSDK。

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
注意
①uSDK成功启动一次即可，不要多次、多个线程启动
②uSDK成功启动之后再调用uSDK提供的各类方法
③启动入参Context必须是ApplicationContext

## 配置设备入网和绑定

配置设备入网就是使用uSDK将智能设备加入指定WIFI的一项操作。

### SmartLink入网并绑定

①App在智能设备入网准备阶段（例如引导操作设备和输入无线密码），构造SmartLink绑定信息类：
SmartLinkBindInfo smartLinkBindInfo = new SmartLinkBindInfo.Builder()
②入网和绑定，在onSucess回调中获得设备对象实例，此时设备已经被绑定，可以通过UWS接口查询到设备信息：
bindUtil.bindDeviceBySmartLink(smartLinkBindInfo, 
				new IBindResultCallback<uSDKDevice>() 

### SoftAP入网并绑定	

①询问目标WIFI时，构造SoftAP绑定信息类
SoftApBindInfo.Builder builder = new SoftApBindInfo.Builder()
②手机成功连接设备热点U-XXXX时，执行入网和绑定
Binding.getSingleInstance().bindDeviceBySoftAp(softApBindInfo, 					
				new ISoftApResultCallback<uSDKDevice>()

### 蓝牙绑定WIFI设备

1.首先实现并设置蓝牙待入网设备回调，先注册再使用：

DeviceScanner.setListener(new IDeviceScannerListener() {

​    public void onDeviceAdd(ConfigurableDevice unJoinedDevice) {}

​    public void onDeviceRemove(ConfigurableDevice configurableDevice) {}}）

​    

2.调用API开始扫描：

DeviceScanner.startScanConfigurableDevice(new IuSDKCallback()

3. App准备绑定信息，向用户收集无线名称、密码拼装BLEBindInfo对象：

bleBindConfigInfo = new BLEBindInfo.Builder()

4.执行设备绑定：

Binding.getSingleInstance().bindDeviceByBLE(

​       canciate, bleBindConfigInfo, new IBindResultCallback<uSDKDevice>() 

5.停止扫描

设备绑定成功，停止扫描并询问关闭蓝牙

DeviceScanner.stopScanConfigurableDevice(new IuSDKCallback() 

6.ConfigurableDevice对象的有用方法

ConfigurableDevice.getBleName()：返回蓝牙设备的名称

ConfigurableDevice.getDeviceId：返回设备在无线时的DeviceId，通常是设备MAC

ConfigurableDevice.getType()：获得设备大类

### 纯蓝牙智能设备绑定

1.设置扫描目标类型：原生蓝牙

DeviceScanner.setFeature(DeviceScanner.BLE_SCAN);

2.首先实现并设置扫描回调，先注册再使用：

DeviceScanner. setListener(new IDeviceScannerListener() {

  public void onDeviceAdd(ConfigurableDevice configurableDevice) 

  public void onDeviceRemove(ConfigurableDevice inValidDevice) }）

3.调用API开始扫描：

DeviceScanner.startScanConfigurableDevice(60, new IuSDKCallback() {

4.App准备绑定信息，向用户收集无线名称、密码拼装BLEBindInfo对象：

PureBLEBindInfo info = new PureBLEBindInfo.Builder()

5.执行设备绑定：

Binding.bindPureBLEDevice(

6.停止扫描：

DeviceScanner.getSingleInstance().stopScanConfigurableDevice(new IuSDKCallback()

### 绑定SmartDevice担当网关外围设备

第一步**：**组织绑定信息

SlaveDeviceBindInfo friendInfo = new SlaveDeviceBindInfo.Builder()      

第二步：执行绑定

Binding.getInstance().bindSlaveDevice(friendInfo, new IBindResultCallback<uSDKDevice>() 

以下为API文档列举错误码

| -10001     | 无效参数错误                              | **14010**  | 请先调用connectToCloud  接口 | **-13023** | 进入组网模式超时         |
| ---------- | ----------------------------------------- | ---------- | ---------------------------- | ---------- | ------------------------ |
| **-13024** | 获取绑定结果超时                          | **-13025** | 等待组网结果超时             | **-25002** | 参数为空                 |
| **-25022** | 开启配对失败                              | **-25023** | 设备类型不支持               | **-25024** | 配对模式已开启，重复开启 |
| **-25026** | 配对超时                                  | **-25025** | 配对设备个数达上限           | **-25027** | 添加并绑定设备失败       |
| **-25028** | 退出配对模式（人为操作设备或APP取消配网） |            |                              |            |                          |

 

### 自带屏幕或操作系统设备入网

对于一些高级、大型家电或智能设备，其自身可能携带操作系统并且带有显示装置，此时我们应将其视为普通计算机，由其自行引导使用者连接网络。



## 发现及管理网络设备：

### 接收uSDK管理设备集变化回调

App依靠uSDK管理智能设备。uSDK将网络里所有的智能设备都管理起来，形成设备池，设备池里设备的数量会发生变化，例如：配置一台新设备入网，配置过的设备上电加入网络等。uSDK将以上情况都汇报给App，App编程实现设备管理。

uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener() {

public void onDevicesRemove(List<uSDKDevice> devicesChanged)

public void onDevicesAdd(List<uSDKDevice> devicesChanged) }
注意
①本接口是即时接口，在uSDK启动前设置。
②如果延时设置或再次设定本接口，需要结合查询设备所有列表使用，确保智能设备列表数据齐全。

### 设定设备过滤类型

App只关心设备池里的某类设备（例如：酒柜设备）：

uSDKDeviceManager. setInterestType(uSDKDeviceTypeConst.WINE_CABINET)

### App实现管理设备集变化回调：

此时uSDK只向App上报uSDKDeviceTypeConst.WINE_CABINET类型的设备，实现设备过滤。
注意
再次执行setInterestType方法，将覆盖已设定的设备过滤类型。
获得设备池全集
uSDKDeviceManager.getDeviceList()，本方法也支持设备类型过滤。

## 与设备建立或断开连接

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
uSDKDevice.setDeviceListener(new IuSDKDeviceListener() {

public void onDeviceOnlineStatusChange(uSDKDevice device,uSDKDeviceStatusConst status, int statusCode) }）

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

注意
①断开设备连接，uSDK将不再上报此设备的任何属性消息给App。
②当用户解绑定设备时，一定执行断开设备连接。
③Android设备熄屏或进入后台强制uSDK与设备保持连接请执行：
uSDKDeviceManager.setKeepLocalOnline(true)，uSDK启动成功后执行本方法。

## 获得设备属性状态

### 获得设备的属性状态

1)App实现并注册如下接口，准备接收设备属性的实时值：
uSDKDevice.setDeviceListener(new uSDKDeviceListener()
2)App连接智能设备
3)App根据ID文档发送查询指令,指令执行成功后，就可以在接口里接收属性数据了。

### App主动获得当前设备所有属性值

当我们第一次通过接口获得设备的属性状态后，只要设备已连接或就绪，调用uSDKDevcie.getDevcieAttrMap返回设备最新属性值合集。

## 处理设备故障和报警

连接设备后，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载

### 获得报警消息

实现uSDKDeviceListener获得报警消息，报警消息和设备属性状态变化是同一接口。
uSDKDevice .setDeviceListener(new uSDKDeviceListener() {

public void onDeviceAlarm(uSDKDevice myDevice, List<uSDKDeviceAlarm> alarmList) }

### 发送停止报警指令

智能设备或家电可能向uSDK规律性快速报警，App处理完成后，可以向设备发送停止报警指令，用于表示使用者已经了解设备发生故障。停止报警是一条普通单命令，参考ID文档编写。

### 获得报警解除消息

发生故障的设备修理正常后会发送报警解除消息，报警解除就是没有报警，就是正常。报警解除消息与普通报警消息都在onDeviceAlarm()接口方法产生，App注意分辨。报警解除的具体值，请参考设备ID文档或设备模型文档。

### 主动查询设备报警

App可以调用API主动查询设备报警信息
ArrayList<uSDKDeviceAlarm> mAlarms = mDevice.getAlarmList()

## 执行设备控制

uSDKDevice提供API用于执行设备控制。执行控制时，可以给设备发送单命令、组命令，设定超时时间，命令执行后会反馈结果。

### 发送单命令

selecteduSDKDevice.writeAttribute("name", "value", new IuSDKCallback()

### 发送组命令

List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>()
组命令字要求全部大写
注意
①组命令说明中需要的指令放入mAttrs，mAttrs没有顺序要求。
②组命令标识字具体值参考设备的ID文档。
③App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。

### 命令超时设定

uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。建议超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。

### 命令执行结果与设备反馈

命令执行结果在IuSDKCallback回调中给出。uSDKErrorConst.RET_USDK_OK代表发送成功，uSDKErrorConst.RET_USDK_TIMEOUT_ERR代表超时。
设备执行成功有可能改变设备的属性值，App接收设备的最新属性上报即可。

## 远程控制及U+云平台推送消息

App执行uSDKDeviceManager.connectToCloud连接用户接入网关，账号登录后按以下内容编程连接用户网关，获得远程和智能设备交互的能力

1）用户接入网关：U+云支持App实现远程功能的服务器软件系统。UWS：U+云为开发人员提供的REST API，主要和用户设备等业务系统交互。

2）accessToken(session)：App用户账号登录后分配，userId：用户注册并登录后获得。

3）绑定、设备绑定：由App发起将用户及所属设备数据上送到云并创建设备数据档案的过程，**设备能够被远程控制的前提是已经被用户绑定**。

4)切网：用户由设备所在A路由切换到B路由，或改用数据网络的行为。

5)小循环VS大循环：它们是两种情景的描述。小循环指的是App与智能设备在同一无线局域网。大循环是说App需要借助U+云用户接入网关才能和设备进行交互的情景，此时App与设备通常不在同一网络。

![大循环](C:\Users\01503602\Desktop\GitHub\大循环.png)

6) 由于加密需要，设备运行设备时间必须准确，当前时间需要超过2015年3月1日

### 连接用户接入网关

uSDKUserInfo userInfo = new uSDKUserInfo.Builder()
deviceManager.connectToCloud(userInfo, new ICallback<Void>() 
账号登录后执行一次即可。

### 远程服务器的连接状态

uSDKDeviceManager提供一个回调，可以了解我们当前与远程服务器的连接状态：
uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()

### 处理绑定一台新设备

当绑定一台新设备时，需要驱动uSDK刷新交互设备列表，uSDKDeviceManager.refreshDeviceList()方法。

### 用户解绑定时解除设备远程能力

用户解绑定设备成功时，已经失去对设备的控制权，断开设备连接，释放设备对象实例，调用uSDKDeviceManager.refreshDeviceList()驱动uSDK刷新设备列表。

注意
如果不是用户主动解绑设备，但收到接入网关推送的设备解绑定消息（此时设备可能被他人解绑定），App需要编写逻辑处理此消息：
uSDKDeviceManager.setDeviceManagerListener()

### 账号注销时的处理

当用户账号注销时，务必调用 uSDKDeviceManager.disconnectToCloud断开远程链接，清空账号数据。 

### 如何测试远程功能是否正常

手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、4G数据网络，查询设备状态、进行控制。

注意
1．当用户切网时，uSDK自动尝试连接用户接入网关，查询设备的最新状态。
2．App在开发者环境绑定设备，智能设备也要连接开发者环境，开发环境始终保持一致。
3．App使用UWS过程中，U+云平台要求App有自己的ClientId。ClientId和终端的身份验证是相关的，错误或固定的ClientId会产生异常行为，App可以使用uSDKManager. getClientId()方法产生ClientId。

### 接收U+云推送消息

App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。

实现如下方法接收云的推送消息：
uSDKManager.setManagerListener(new IuSDKManagerListener(){

public void onBusinessMessage(String content)})

### 接收U+云Session异常消息

当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，检查程序逻辑，查看用户登录地址与连接用户接入网关地址是否同一服务器环境。
uSDKManager.setManagerListener(new IuSDKManagerListener() {

public void onSessionException(String session)})

## 获取设备WIFI信号强度

可以使用uSDKDevice实例获取设备的Wifi信号强度，注意必须是WIFI设备
uSDKDevice.getDeviceNetQuality(new IuSDKGetDeviceNetQualityCallback() {
	public void onDeviceNetQualityGet(
			uSDKErrorConst result, uSDKDeviceNetQuality level, int detailValue)});

## 和带子机设备交互

带子机设备通常有商用空调、星盒等设备。一台主机可以有多台子机，子机从属于主机，主机拥有mac，子机没有。在程序中我们可以和主机、子机进行交互，主机需要连接，子机不需要连接。

### 获得子机实例

当主机已连接后，通过向主机发送查询指令（根据设备模型文档或ID文档），由主机动态上报子机实例（一会上报一台或连续上报子机实例信息），在以下回调中就可以得到子机实例。
parentuSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
	public void onSubDeviceListChange(
			uSDKDevice currentDevice, ArrayList<uSDKDevice> childArrayList) });

### 子机连接状态、属性及报警

子机有三种连接状态：离线、已连接、就绪。子机的属性、报警将通过回调上报给App，实现以下接口并设定到子机对象实例上：
private class ChildDeviceCtrlListner implements IuSDKDeviceListener {
		public void onDeviceAlarm(uSDKDevice uSDKDevice, List<uSDKDeviceAlarm> list) 
		public void onDeviceAttributeChange(uSDKDevice childDevice, List<uSDKDeviceAttribute> list) 
		public void onDeviceOnlineStatusChange(
	uSDKDevice uSDKDevice, uSDKDeviceStatusConst uSDKDeviceStatusConst, int i) }}

### 子机控制

子机只要不是离线状态，我们就可以和子机进行交互，对子机的指令仅能影响子机本身，子机的实例对象也是uSDKDevice。

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

uSDK已经支持您获得SmartDevice设备图片资源

当需要获得设备资源时，需要提前为设备设置监听IuSDKDeviceListenerWithResource，实现onReceive方法。设备资源名称请您咨询设备实现方，当前设备是否可以存取资源请根据typeid判断。 

具体步骤：
①设置资源设备监听实现方法
②连接设备使其就绪
③订阅资源名称
④使用完毕释放资源
selecteduSDKDevice.setDeviceListener(new IuSDKDeviceListenerWithResource() )

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
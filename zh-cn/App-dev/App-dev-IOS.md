# 移动端 SDK iOS #

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
 ![info.pllist][info-pllist]

### 在XCode 的Other linker flag 中设置-ObjC

集成uSDK5.4.0 及以后版本，需要在Build settings 中的Other linker flag  
中添加-ObjC 属性。如截图：       
 ![Objc][Objc]

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



## 基本业务流程

本图简要介绍uSDK物联App主要业务运行流程，红色文字：App与uSDK交互，黑色文字：App与U+云交互：

 ![基本业务流程][pic1]

## 配置入网流程

参考下列流程开发配置设备入网功能，特别说明：启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。

 ![配置入网流程][pic2]

## 使用uSDK控制设备流程说明图

以下流程图适用于使用uSDK进行设备配置入网，控制的App，OPENAPI请更换为UWS或对应服务。

 ![控制流程图][pic6]

## uSDK业务开发详解

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



### 配置设备入网和绑定 

产品配置入网后就直接绑定，首先请完成账号登录拿到账号session/token，并使uSDK连接U+云。 

**SmartLink入网并绑定**

①App在智能设备入网准备阶段（例如引导操作设备和输入无线密码），构造SmartLink绑定信息类：
SmartLinkBindInfo smartLinkBindInfo = new SmartLinkBindInfo.Builder()   
②入网和绑定，在onSucess回调中获得设备对象实例，此时设备已经被绑定，可以通过UWS接口查询到设备信息：   
bindUtil.bindDeviceBySmartLink(smartLinkBindInfo, new IBindResultCallback<uSDKDevice>() 

**SoftAP入网并绑定**   

①询问目标WIFI时，构造SoftAP绑定信息类    
SoftApBindInfo.Builder builder = new SoftApBindInfo.Builder();  
②手机成功连接设备热点U-XXXX时，执行入网和绑定     
Binding.getSingleInstance().bindDeviceBySoftAp(softApBindInfo, new ISoftApResultCallback<uSDKDevice>()      
此接口回调时说明配置信息已经成功发送，App继续执行务必让手机成功连接和设备相同的WIFI。  

**蓝牙绑定WIFI设备**

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

**纯蓝牙智能设备绑定**

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

**绑定SmartDevice担当网关外围设备**

支持绑定SmartDevice实现网关外围设备。本功能uSDK实质上仅仅是发送“开始”指令给网关设备，然后监听网关与外围设备组网及绑定的相关消息。另本命令是通过用户网关发送给网关设备，所以功能实现时需要您提前绑定设备且网络良好，请一定注意。     
①组织绑定信息           
SlaveDeviceBindInfo friendInfo = new SlaveDeviceBindInfo.Builder()
②执行绑定       
Binding.getInstance().bindSlaveDevice(friendInfo, new IBindResultCallback<uSDKDevice>()
自带屏幕或操作系统设备入网       
对于一些高级、大型家电或智能设备，其自身可能携带操作系统并且带有显示装置，此时我们应将其视为普通计算机，由其自行引导使用者连接网络。        

### 发现及管理网络设备

智能设备已经连上WIFI，App实现设备管理接口就能实现设备管理业务，接收uSDK管理设备集变化、设定过滤设备类型、获得可用设备全集。        

**接收uSDK管理设备集变化回调**

App依靠uSDK管理智能设备。uSDK将网络里所有的智能设备都管理起来，形成设备池，设备池里设备的数量会发生变化，例如：配置一台新设备入网，配置过的设备上电加入网络等。uSDK将以上情况都汇报给App，App编程实现设备管理。     
App实现并注册如下接口方法，发现及管理网络设备：    
uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()
注意：
①本接口是即时接口，在uSDK启动前设置。   
②如果延时设置或再次设定本接口，需要结合查询设备所有列表使用，确保智能设备列表数据齐全。  

**设定设备过滤类型** 

App只关心设备池里的某类设备（例如：酒柜设备）：        
①App执行如下方法：      
uSDKDeviceManager. setInterestType(uSDKDeviceTypeConst.WINE_CABINET)      
②App实现管理设备集变化回调：    
此时uSDK只向App上报uSDKDeviceTypeConst.WINE_CABINET类型的设备，实现设备过滤。     
注意：  
再次执行setInterestType方法，将覆盖已设定的设备过滤类型。

**获得设备池全集**

uSDKDeviceManager.getDeviceList()，本方法也支持设备类型过滤。    

### 与设备建立或断开连接

现在我们已经发现设备，调用API就可以连接了，连接设备、断开设备连接。 

**设备的连接状态**

连接中(connecting)：App正在与设备建立连接。     
已连接(connected)：App已连接智能设备，可以发送控制指令，查询属性状态等，是我们可以交互的设备。       
离线(off_line)：App不断开与智能设备连接的情况下，智能设备发生异常。     

 ![设备连接状态][pic4]

未连接、连接中、已连接、离线都是设备（uSDKDevice）的连接状态。App连接或断开智能设备将触发连接状态的变化： 
①智能设备正常入网后是未连接（被我们发现）  
②连接智能设备后，智能设备变为连接中    
③与智能设备连接成功后，变为已连接       
④如果连接失败或通信中断会变为离线         
⑤断开与智能设备的连接，则变为未连接           

**设备连接状态变化接口**

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

**连接设备**

uSDKDevice.connectNeedProperty，使用本方法。 

**断开连接**

不需要某台设备属性数据时，断开设备连接，释放设备资源：   
uSDKDevice.disconnect。

**主动查询连接状态**

uSDKDevice.getStatus()。   
注意：    
①断开设备连接，uSDK将不再上报此设备的任何属性消息给App。    
②当用户解绑定设备时，一定执行断开设备连接。   
③Android设备熄屏或进入后台强制uSDK与设备保持连接请执行： uSDKDeviceManager.setKeepLocalOnline(true)，uSDK启动成功后执行本方法。    

### 获得设备属性状态

现在已经连接成功了，我们就能实时获得设备的最新属性值集合与设备交互，获得设备的连接状态和实时属性值。

**获得设备的属性状态**

①App实现并注册如下接口，准备接收设备属性的实时值：    

    uSDKDevice.setDeviceListener(new uSDKDeviceListener(){
    
    public void onDeviceAttributeChange(uSDKDevice device, List<uSDKDeviceAttribute> propertiesChanged) }）
②App连接智能设备     
③App根据ID文档发送查询指令，指令执行成功后，就可以在接口里接收属性数据了。   
uSDKDevice.writeAttribute("60w0ZZ", null, new IuSDKCallback() 

**App主动获得当前设备所有属性值**

当我们第一次通过接口获得设备的属性状态后，只要设备已连接或就绪，调用uSDKDevcie.getDevcieAttrMap返回设备最新属性值合集。    

### 处理设备故障和报警

我们连接设备后，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载。    
报警信息六位码具体含义参考相关ID文档。获得报警消息、发送停止报警指令、获得报警解除消息、主动查询设备报警等内容。           

**获得报警消息**  

实现uSDKDeviceListener获得报警消息，报警消息和设备属性状态变化是同一接口。   

    uSDKDevice .setDeviceListener(new uSDKDeviceListener(){
    
    public void onDeviceAlarm(uSDKDevice myDevice, List<uSDKDeviceAlarm> alarmList)}）

**发送停止报警指令**   

智能设备或家电可能向uSDK规律性快速报警，App处理完成后，可以向设备发送停止报警指令，用于表示使用者已经了解设备发生故障。停止报警是一条普通单命令，参考ID文档编写。   

**获得报警解除消息**     
发生故障的设备修理正常后会发送报警解除消息，报警解除就是没有报警，就是正常。报警解除消息与普通报警消息都在onDeviceAlarm()接口方法产生，App注意分辨。报警解除的具体值，请参考设备ID文档或设备模型文档。  

**主动查询设备报警**

App可以调用API主动查询设备报警信息   
ArrayList<uSDKDeviceAlarm> mAlarms = mDevice.getAlarmList();   

### 执行设备控制

我们和设备已经连接了，可以向设备发送控制指令。uSDKDevice提供API用于执行设备控制。执行控制时，可以给设备发送单命令、组命令，设定超时时间，命令执行后会反馈结果。        

**发送单命令**

selecteduSDKDevice.writeAttribute("204003", "214001", new IuSDKCallback()   
注意：      
App需要严格遵守ID文档规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。    

**发送组命令**

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

**与海极网创建智能设备交互（标准模型设备文档）**

海极网支持创建智能设备，创建完成后会产生《XX设备应用开发文档》，以下将说明如何使用此文档与设备进行交互。   
1）属性   
属性的含义是此项可以作为智能设备的属性，可写列为T时，此项可以作为命令发往设备：    

`selecteduSDKDevice.writeAttribute("A", "True", new IuSDKCallback() {`			
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

**命令超时设定**  

uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。建议超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。   

**命令执行结果与设备反馈**

命令执行结果在IuSDKCallback回调中给出。uSDKErrorConst.RET_USDK_OK代表发送成功，uSDKErrorConst.RET_USDK_TIMEOUT_ERR代表超时。   
设备执行成功有可能改变设备的属性值，App接收设备的最新属性上报即可。     

### 远程控制及U+云平台推送消息

经过前面的开发，我们已经可以在本地和智能设备完美交互了，但我们的手机不能切换路由或者使用4G，这么做和智能设备的数据通路会立刻切断。现在我们让App同时连接远程服务器，手机更换WIFI或使用4G则直接通过服务器连接设备。  

**一般的U+物联App如何运行**  

App执行uSDKDeviceManager.connectToCloud连接用户接入网关，此方法需要用户信息，适时运行方法连接远程服务器。     

**连接用户接入网关**

App在日常使用情景中，账号登录后按以下内容编程连接用户网关，获得远程和智能设备交互的能力。   

    deviceManager.connectToCloud(userInfo, new ICallback<Void>()
账号登录后执行一次即可。   

**远程服务器的连接状态**

uSDKDeviceManager提供一个回调，可以了解我们当前与远程服务器的连接状态：   

    uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()

**处理绑定一台新设备**

当绑定一台新设备时，需要驱动uSDK刷新交互设备列表，uSDKDeviceManager.refreshDeviceList()方法。

**用户解绑定时解除设备远程能力**  

用户解绑定设备成功时，已经失去对设备的控制权，断开设备连接，释放设备对象实例，调用uSDKDeviceManager.refreshDeviceList()驱动uSDK刷新设备列表。    
注意：  
如果不是用户主动解绑设备，但收到接入网关推送的设备解绑定消息（此时设备可能被他人解绑定），App需要编写逻辑处理此消息：   

    uSDKDeviceManager.setDeviceManagerListener( 
    new IuSDKDeviceManagerListener()

**账号注销时的处理**

当用户账号注销时，务必调用 uSDKDeviceManager.disconnectToCloud断开远程链接，清空账号数据。 

**如何测试远程功能是否正常**

手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、4G数据网络，查询设备状态、进行控制。
注意：    
①当用户切网时，uSDK自动尝试连接用户接入网关，查询设备的最新状态。   
②App在开发者环境绑定设备，智能设备也要连接开发者环境，开发环境始终保持一致。   
③App使用UWS过程中，U+云平台要求App有自己的ClientId。ClientId和终端的身份验证是相关的，错误或固定的ClientId会产生异常行为，App可以使用uSDKManager. getClientId()方法产生ClientId。

**接收U+云推送消息**

App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。  
实现如下方法接收云的推送消息：       

    uSDKManager.setManagerListener(new IuSDKManagerListener() {
    public void onBusinessMessage(String content)	});

**接收U+云Session异常消息**

当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，检查程序逻辑，查看用户登录地址与连接用户接入网关地址是否同一服务器环境。    

    uSDKManager.setManagerListener(new IuSDKManagerListener() {
    public void onSessionException(String session) });

### 获取设备WIFI信号强度

可以使用uSDKDevice实例获取设备的Wifi信号强度，注意必须是WIFI设备。    

    uSDKDevice.getDeviceNetQuality(new IuSDKGetDeviceNetQualityCallback() 

### 和带子机设备交互

带子机设备通常有商用空调、星盒等设备。一台主机可以有多台子机，子机从属于主机，主机拥有mac，子机没有。在程序中我们可以和主机、子机进行交互，主机需要连接，子机不需要连接。    

**获得子机实例**

当主机已连接后，通过向主机发送查询指令（根据设备模型文档或ID文档），由主机动态上报子机实例（一会上报一台或连续上报子机实例信息），在以下回调中就可以得到子机实例。   

    parentuSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
    	public void onSubDeviceListChange(
    			uSDKDevice currentDevice, ArrayList<uSDKDevice> childArrayList) {
    	}
    });

**子机连接状态、属性及报警**

子机有三种连接状态：离线、已连接、就绪。子机的属性、报警将通过回调上报给App，实现以下接口并设定到子机对象实例上：    

      private class ChildDeviceCtrlListner implements IuSDKDeviceListener {}

**子机控制**  

子机只要不是离线状态，我们就可以和子机进行交互。对子机的指令仅能影响子机本身。    
子机的实例对象也是uSDKDevice。   

**主机控制**

我们可以像普通设备一样和主机交互，其中一些指令将影响到附属的所有子机。 

**辅助方法**

selectedChildDevice.getMainDevice()：获得主机   
selectedChildDevice.isSubDevice()：子机？    
selectedChildDevice.isMainDevice()：主机？           
selectedChildDevice.getSubType()：子机类型  
selectedChildDevice.getSubId()：子机Id         
parentDevice.getSubDeviceList()：获得当前有哪些子机         
parentDevice.getSubDeviceById()：根据子机Id获得子机实例            

### 订阅SmartDevice设备资源

支持您获得SmartDevice设备图片资源。     

**实现步骤**

当需要获得设备资源时，需要提前为设备设置监听IuSDKDeviceListenerWithResource，实现onReceive方法。设备资源名称请您咨询设备实现方，当前设备是否可以存取资源请根据typeid判断。    

**具体步骤：**

①设置资源设备监听实现方法       
②连接设备使其就绪         
③订阅资源名称       
④使用完毕释放资源      
selecteduSDKDevice.setDeviceListener(new IuSDKDeviceListenerWithResource()   
注意：    
订阅资源需要设备就绪，使用完成需要时释放，设备下线再次就绪时继续获得数据。     

### 退出uSDK

App不需要使用U+物联功能时可以停止uSDK。    
uSDKManager.stopSDK();       

### 有用的类和方法 

**uSDKDevice有用的方法**    

uSDKDevice.getIP可以得到设备IP地址。          
uSDKDevice.getStatus查询设备的连接状态。         
uSDKDevice.getNetType可以得知设备是本地设备还是远程设备。        
uSDKDevice.getDeviceType查询设备类型，例如冰箱、洗衣机。      
uSDKDevice.getUplusId查询设备uplusId(typeId)。        
uSDKManager.getClientId()得到终端ClientId。        

**调用API获得uSDKDevice**   

uSDKDeviceManager.getDevcieList获得当前可交互的所有设备。        
uSDKDeviceManager.getDevice使用DeviceId获得设备实例。       
uSDKDeviceManager.getDeviceList(uSDKDeviceTypeConst.XXXXXXX)按类型取。      

**获得uSDK版本号**

uSDKManger.getuSDKVersion()        

**获得错误码和描述** 

uSDKError.getDescription()   
uSDKError.getErrorCode()   



## iOS版资料 ##    

**1. uSDK开发手册**   

简介：uSDK开发手册的使用对象是使用uSDK开发APP的开发者。开发者可以通过此手册，可以了解uSDK的用法、关键流程以及常见问题。   

**2. uSDK Demo**

简介：uSDK示例工程的使用对象是使用uSDK的APP开发者。开发者通过此示例工程，可以了解uSDK的应用方法及流程。     
**3. iOS uSDK开发包下载**


支持有效期：新版本SDK发布起，APP新接入大版本SDK的支持有效期为6-12个月，APP新接入小版本SDK的支持有效期为3-6个月。    


----------
- ##### iOS uSDK_8.5.0

  版本号： v8.5.0
  发布日期：2021.04.30
  MD5值：1429A5C9956B2BACA5CAA5960FBDBD7B
  下载链接： [点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.5.0_Phone_iOS_20210430161621_20210511092500310.zip)
  更新日志：
  新增功能

  ##### 一、外部修改

  uSDKDevice.h新增接口

  ```
  /**
   订阅资源新接口，数据会进行加解密， 对应的delegate回调为新的:- (void)device:(uSDKDevice *)device didReceiveDecodeResource:(NSString *)resource data:(NSString *)JSONData;
  
  
   @param resourceName 资源名称
   @param success 订阅成功的回调
   @param failure 订阅失败的回调
   @since 8.5.0
   */
  
  - (void)subscribeResourceWithDecode:(NSString *)resourceName success:(void(^)(void))success failure:(void(^)(NSError *error))failure;
  
  /**
   新的接收资源数据
  
   @param device 设备对象
   @param resource 资源名称
   @param JSONData 资源数据
   @since 8.5.0
   */
  
  - (void)device:(uSDKDevice *)device didReceiveDecodeResource:(NSString *)resource data:(NSString *)JSONData;
    uSDKCameraScanQRCodeBindInfo 新增必填字段productCode
    /**
     成品编码，必填
     */
    @property (nonatomic, copy) NSString *productCode;
    新增本地视频回放、本地图片获取和获取云存视频m3u8地址
    uSDKPlaybackPlayer.h新增接口
    /// 回放文件
    @interface uSDKPlaybackItem: NSObject
    /// 回放文件起始时间（秒）
    @property (nonatomic, assign) NSTimeInterval startTime;
    /// 回放文件结束时间（秒）
    @property (nonatomic, assign) NSTimeInterval endTime;
    /// 回放文件持续时间（秒）
    @property (nonatomic, assign) NSTimeInterval duration;
  
  @end
  
  /// 回放文件分页
  @interface uSDKPlaybackPage<uSDKItem>: NSObject
  /// 当前页码索引
  @property (nonatomic, assign) uint32_t  pageIndex;
  /// 总页数
  @property (nonatomic, assign) uint32_t  totalPage;
  /// 回放文件数组
  @property (nonatomic, strong) NSArray<uSDKItem> *items;
  
  @end
  
  @interface uSDKPictureItem : NSObject
  
  /// 图片ID
  @property (nonatomic, assign) uint32_t id;
  /// 图片时间
  @property (nonatomic, assign) NSTimeInterval time;
  /// 图片类型
  @property (nonatomic, assign) uSDKPlayerImageType type;
  /// 图片名称
  @property (nonatomic, copy) NSString *name;
  /// 图片文本描述
  @property (nonatomic, copy) NSString *describe;
  /// 缩略图base64编码
  @property (nonatomic, copy) NSString *thumbnail;
  
  @end
  
  @interface uSDKPlaybackPlayer : uSDKPlayer
  
  /**
  
   * 创建播放器
     *
   * @param device 设备对象
   * @param completionHandler 结果回调
     */
  
  + (void)createPlayerWithDevice:(uSDKDevice* _Nonnull)device
        completionHandler:(void(^)(uSDKPlaybackPlayer *playbackPlayer, NSError *_Nullable error))completionHandler;
  
  /**
  
   * 获取有回放文件的日期列表
   * @param pageIndex 页码索引，获取指定页码的回放文件（ pageIndex从0开始递增）
   * @param countPerPage 在[startTime, endTime]时间范围内按每页`countPerPage`个文件查询，每次返回一页的数据，该值由APP设置，取值范围[1, 3200]
   * @param startTime 开始时间戳（秒）
   * @param endTime 结束时间戳（秒）
   * @param completionHandler 结果回调
     */
  
  - (void)getPlaybackDateListOfpageIndex:(uint32_t)pageIndex
        countPerPage:(uint32_t)countPerPage
           startTime:(NSTimeInterval)startTime
             endTime:(NSTimeInterval)endTime
    completionHandler:(void (^)(uSDKPlaybackPage<NSNumber *> *_Nullable page, NSError *_Nullable error))completionHandler;
  
  /**
  
   * 获取一页回放文件列表
   * @param pageIndex 页码索引，获取指定页码的回放文件（ pageIndex从0开始递增）
   * @param countPerPage 在[startTime, endTime]时间范围内按每页`countPerPage`个文件查询，每次返回一页的数据，该值由APP设置，取值范围[1, 3200]
   * @param startTime 开始时间戳（秒）
   * @param endTime 结束时间戳（秒）
   * @param completionHandler 结果回调
   * @note 请根据实际情况合理设置查询时间范围和分页，`时间跨度太长`或`每页数量过大`会增加设备查询时间， 建议[startTime, endTime]区间不超过24小时 或 countPerPage不超过1440个文件.
     */
  
  - (void)getPlaybackListV2OfpageIndex:(uint32_t)pageIndex
        countPerPage:(uint32_t)countPerPage
           startTime:(NSTimeInterval)startTime
             endTime:(NSTimeInterval)endTime
    completionHandler:(void (^)(uSDKPlaybackPage<uSDKPlaybackItem *> *_Nullable page, NSError *_Nullable error))completionHandler;
  
  /**
  
   * 获取可以查看的照片列表
   * @param startTime 起始时间
   * @param endTime 结束时间
   * @param photoID 图片ID  注：查询首页图片列表时photoID传参为0，查询下一页图片列表时photoID传参为上一页最后一张图片的photoID
   * @param count 要查询的照片个数，范围是0到10
   * @param completionHandler 结果回调
   * @note 1.查询首页：使用参数【startTime，endTime，count】 ，photoID 传参为0 ，查询首页前count张图片，并且记录查询到的最后一张图片id；
   * 2.查询下一页：使用参数【startTime，id，endTime，count】，photoID 传参为上一页最后一张图片的photoID，查询下一页的count张图片，并且并且记录查询到的最后一张图片id；
   * 3.同第2步，持续查询下一页，直到查询不到任何内容；
      */
  
  - (void)getPhotoListWithStartTime:(NSTimeInterval)startTime
        endTime:(NSTimeInterval)endTime
        photoID:(uint32_t)photoID
          count:(uint32_t)count
    completionHandler:(void(^)(NSArray<uSDKPictureItem*> *items, NSError *_Nullable error))completionHandler;
  
  /**
  
   * 获取设备的本地图片内容
   * @param photoID 图片ID
   * @param timeout 超时时间 范围建议5-10秒左右
   * @param completionHandler 结果回调
     */
  
  - (void)getLocalPhotoDataWithPhotoID:(uint32_t)photoID
        timeout:(NSTimeInterval)timeout
    completionHandler:(void(^)(NSData *data, NSError *_Nullable error))completionHandler;
  
  
  /**
  
   * 获取云存视频可播放日期信息
   * - 用于终端用户在云存页面中对云存服务时间内的日期进行标注，区分出是否有云存视频文件。
   * @param timezone 相对于0时区的秒数，例如东八区28800
   * @param completionHandler 回调结果
     */
  
  - (void)getCloudVideoDateListWithTimezone:(NSInteger)timezone
        completionHandler:(void(^)(NSArray<NSNumber *> *items, NSError *_Nullable error))completionHandler;
  
  /** 获取云存回放文件列表
  
   *  获取云存列表，用户对时间轴渲染
   *  @param startTime 开始UTC时间,单位秒
   *  @param endTime 结束UTC时间,单位秒 超过一天只返回一天
   *  @param completionHandler 回调结果
      */
  
  - (void)getCloudVideoPlayListWithStartTime:(NSTimeInterval)startTime
        endTime:(NSTimeInterval)endTime
    completionHandler:(void (^)(NSArray<uSDKPlaybackItem *> *items, NSError *_Nullable error))completionHandler;
  
  /** 获取云存回放 m3u8 播放地址
  
   * @param startTime 开始UTC时间,单位秒
   * @param endTime 结束UTC时间,单位秒 填 0 则默认播放到最新为止
   * @param completionHandler 回调结果
     */
  
  - (void)getCloudVideoPlayAddressWithStartTime:(NSTimeInterval)startTime
        endTiem:(NSTimeInterval)endTime
    completionHandler:(void (^)(NSString *url, NSError *_Nullable error))completionHandler;
  
  /**
  
   * 设置回放参数.
   * @param item 播放的文件(可跨文件).
   * @param time 指定播放起始时间点（秒），取值范围`playbackItem.startTime >= time <= playbackItem.endTime`.
     */
  
  - (void)setPlaybackItem:(uSDKPlaybackItem *)item
        seekToTime:(NSTimeInterval)time;
  
  /// 暂停
  
  - (void)pause;
  
  /// 恢复
  
  - (void)resume;
  
  @end
  uSDKPlayer.h新增播放器代理协议
  /**
  
   *  播放时间回调
   *  @param player 播放器实例
   *  @param PTS 时间戳
      */
  
  - (void)player:(uSDKPlayer *)player didUpdatePTS:(NSTimeInterval)PTS;
  
  /**
  
   *  SD卡回放文件播放结束
   *  @param player 播放器实例
   *  @param startTime 当前播放结束的文件的开始时间
      */
  
  - (void)player:(uSDKPlayer *)player didFinishPlaybackFile:(NSTimeInterval)startTime;
    uSDKConstantInfo.h新增音视频相关错误码
    /**
      *  视频功能内部错误
         */
          ERR_USDK_PLAYER_INTERNAL_FAILURE = -20001,
          /**
      *  无本地回放视频
         */
          ERR_USDK_PLAYER_PLAYBACKLIST_EMPTY = -20001,
          /**
      *  视频分辨率已改变
         */
          ERR_USDK_PLAYER_RESOLUTION_CHANGED =  -20003,
          /**
      *  超过设备可支持的最大P2P通道数
         */
          ERR_USDK_PLAYER_DEVICE_CALLING_ALL_CHN_BUSY = 20004,
          /**
      *  P2P通道消息发送失败
         */
          ERR_USEK_PLAYER_MESSAGE_SEND_FAILURE = -20005,
          /**
      *  P2P通道消息发送超时
         */
          ERR_USEK_PLAYER_MESSAGE_SEND_TIMEOUT = -20006,
          /**
      *  设备正在录制
         */
          ERR_USDK_PLAYER_RECORDER_IS_RUNNING = -20007,
          /**
      *  APP端通道连接数已达上限
         */
          ERR_USDK_PLAYER_EXCEEDS_MAX_NUMBER = -20008,
  ```

  ##### 二、内部优化

  1.配置文件下载地址变更
  2.uSDKMonitorPlayer和uSDKPlaybackPlayer中destroyPlayer接口统一定义在父类uSDKPlayer中
  3.创建播放器时向平台请求tid、accessid、accesstoken的逻辑统一放到分类uSDKPlayer+uSDKPrivatePlayer.h中实现
  4.内部新增接口getDeviceScanQRcodeWithQRCodeInfo，去网络请求判断是否支持新的扫码绑定，内部增加扫码绑定的逻辑，和安防绑定的逻辑分开走。

- **iOS uSDK_8.4.0**    

版本号： v8.4.0    
发布日期：2021.03.22        
MD5值：B0E8073E25A10B62AB776F2C82D63F50       
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.4.0_Phone_iOS_20210318174107_20210330132958513.zip)    

更新日志：      

**新增功能**    

1.uSDKDevice.h组控接口变更    

    /**
     组设备的成员列表
     @since 8.4.0
     */
    @property (nonatomic, strong, readonly) NSArray<uSDKDevice *> *groupMembers;
    
    /**
    获取可与当前设备分到同一组的设备列表, 当前设备要求有zigbee能力或BLEMesh能力
     
    @param completionHandler 接口执行完成时回调，error == nil表示接口执行成功，接口执行成功时，devices也可能为空，表示接口执行成功，但没有可分组设备
    @since 8.4.0
    */
    - (void)fetchGroupableDeviceListCompletionHandler:(void(^)(NSArray<uSDKDevice *> *devices, NSError *error))completionHandler;
     
    /**
    创建分组，返回组设备对象
     
    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
    @param completionHandler 接口执行完成时回调，error == nil表示组创建成功，注意组设备虽然创建成功，但没有任何设备被成功添加到该组中，
    需要主动调用addDevices:toGroupWithTimeoutInterval:progressNotify:completionHandler:函数去添加进组中。
    @since 8.4.0
    */
    - (void)createGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
    completionHandler:(void(^)(uSDKDevice *device, NSError *error))completionHandler;
     
    /**
    向组设备中添加设备，要求当前device对象为组设备
     
    @param devices 需要添加到组设备的设备列表，可以添加到组设备的设备列表需要通过接口``
    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
    @param progressNotify 进度通知，上报每个设备的添加结果，如果error == nil则表示添加成功
    @param completionHandler 接口执行完成时回调，参数校验失败时error有值，一旦开始添加，不管是否有设备成功添加到组，error都为nil
    @since 8.4.0
    */
    - (void)addDevices:(NSArray<uSDKDevice *> *)devices
    toGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
    progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
    completionHandler:(void(^)(NSError *error))completionHandler;
     
    /**
    从组设备中移除设备，要求当前device对象为组设备
     
    @param devices 需要从组设备中删除的设备列表
    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据删除设备的多少动态调整参数
    @param progressNotify 进度通知，上报每个设备的删除结果，如果error == nil则表示设备删除成功
    @param completionHandler 接口执行完成时回调，参数校验失败时error有值，一旦开始添加，不管是否有设备成功添加到组，error都为nil
    @since 8.4.0
    */
    - (void)removeDevices:(NSArray<uSDKDevice *> *)devices
    fromGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
    progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
    completionHandler:(void(^)(NSError *error))completionHandler;
     
    /**
    删除组设备，要求当前device对象为组设备
     
    @param completionHandler 接口执行完成时回调，error == nil表示组删除成功
    @since 8.4.0
    */
    - (void)deleteGroupCompletionHandler:(void(^)(NSError *error))completionHandler;


2.内部修改  
2.1 按照新方案更新了内部逻辑    
2.2 H2面板增加了zigbee连接方式的支持。    
2.3 新增uSDKMutableDevice+uSDKGroupOperation类，用于分发mesh和zigbee不同通道的组控相关的功能。    








----------
- **iOS uSDK_8.3.0**      

版本号： v8.3.0   
发布日期：2021.03.02    
MD5值：65EFA79B2C417A9558B254E9BF943E99    
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.3.0_Phone_iOS_20210302105710_20210315163827028.zip)     
更新日志：    
**新增功能**    
1.新增NFC数据解析和数据更新的功能        


    //uSDKUtil.h
    @interface uSDKNFCUtil : NSObject 
    /**
     * 解析NFC标签数据
     * @param NdefRecord   NFC标签原始数据
     * @param complete接口回调 成功返回uSDKNFCInfo类，失败返回error
     **/
    + (void)parseNFCTagDataWithNdefRecord:(NSString*)NdefRecord
     complete:(void(^)(uSDKNFCInfo *NFCInfo, NSError *error))complete;
    /**
     * 更新NFC设备信息
     *  @param NFCInfoNFC标签解析后数据
     *  @param complete接口回调 失败返回error
     */
    + (void)updateNFCDeviceInfoWithNFCInfo:(uSDKNFCInfo*)NFCInfo
      complete:(void(^)(NSError *error))complete;
    @end 

2.新增NFCInfo类     

    //uSDKInfo.h     
    //NFC标签数据解析结果
    @interface uSDKNFCInfo : NSObject     
    @property (nonatomic, copy) NSString *NFCSerialNumber;  //NFC标签序列号    
    @property (nonatomic, copy) NSString *MAC;  //设备MAC    
    @property (nonatomic, copy) NSString *deviceID; //设备ID
    @property (nonatomic, copy) NSString *productCode;  //设备成品编码
    @property (nonatomic, copy) NSString *hwProductID; //华为的PID
    @end

**内部优化**    
1.只订阅，不获取属性时，忽略属性上报    
2.解决fota升级时在子线程调UI导致崩溃的问题    
3.smartlink配置失败返601时进行重试      

**组件修改内容**     
1.在门锁通过手机进行LocalKey更新或者时间同步时，用户可以优先订阅该门锁，建立连接。    
2.修改的“蓝牙门锁升级时崩溃”问题。      

**CAE修改内容**    
1.增加蓝牙beacon v0的数据解析，以便支持云芯二代配置失败，可以通过蓝牙beacon上报配置失败原因    
2.解决蓝牙搜索内存泄露的问题    
3.解决友盟异常接管sigpipe，导致crash的问题（ios）    
4.ble ota超时时间和hal层超时时间（10秒）对齐，从2秒改成11秒     



----------
- **iOS uSDK_8.2.0**    

版本号： v8.2.0   
发布日期：2021.02.04    
MD5值：0B7D9309709F48E84EB281DB31A6D220     
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.2.0_Phone_iOS_20210205170244003.zip)       
注意事项1：不支持国海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用更新日志：    
1.新增功能   
1.1 新增事件上报自定义类 uSDKDeviceEvent   

    //uSDK设备事件信息类，用于描述设备的事件消息。   
    
    @interface uSDKDeviceEvent : NSObject
    
    //事件名称-标准定义
    @property (nonatomic, copy, readonly) NSString *name;
    
    // 事件类型[ message消息;alarm报警;fault故障;]
    @property (nonatomic, copy, readonly) NSString *type;
    
    // 该事件附带的属性列表
    @property (nonatomic, strong) NSArray<uSDKDeviceAttribute*> *attrs;
    @end  

1.2 新增事件上报代理回调（见uSDKDevice.h）    

    /**
     * 设备事件回调
     * @param device 事件上报的设备对象
     * @param events 事件
     */
    -(void)device:(uSDKDevice *)device didReceiveEvent:(NSArray<uSDKDeviceEvent*>*)events ;

2.接口变更  
 无    
3.内部优化    
3.1 新接入蓝牙门锁类设备的 搜索发现、配置绑定、控制、属性报警事件上报。 可配设备处于搜索发现阶段时，该类设备的配网方式为纯蓝牙配置方式（uSDKDeviceConfigTypePureBLE ）。蓝牙门锁类设备使用用纯蓝牙接口（bindPureBLEDevice）进行配置绑定。   
3.2 接入蓝牙门锁类设备的OTA功能，沿用之前FOTA升级接口。        
3.3 接入蓝牙门锁类设备具备查看历史数据功能。    
3.4 接入3款老营养秤（与8.0.0的广播称使用方式一致）。        


----------

- **iOS uSDK_8.1.1**    

版本号： v8.1.1    
发布日期：2021.1.8    
MD5值：059E057A2891E8872FD1A9CE9850B213    
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.1_Phone_iOS_20210114161003329.zip)    
注意事项1：不支持国海外环境。     
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用更新日志：     
1.新增功能   
 无   
2.接口变更    
 无   
3.内部优化及BUG修改   
3.1 修复蓝牙体脂秤数据更新没有给app上报的问题    



----------

- **iOS uSDK_8.1.0**    

版本号： v8.1.0    
发布日期：2020.12.23   
MD5值：4689EB4BE4490F024FF35D30D0135FC1   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.0_Phone_iOS_20201228103123939.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    
更新日志：   
1.新增功能   
1.1 根据自定义traceId创建链式跟踪对象（见uTraceNode.h）     

    /**
     根据自定义traceId创建链式跟踪对象 
     1.该方法针对App主动服务业务使用
     2.调用埋点方法时，bId的值：
    2.1对于DI点，bId = uTraceNodeDI.bId
    2.2对于CS/CR点，bId = businessID
      
     @param traceID 链式跟踪ID，不能为空，且要求为32个字符长度的uuid字符串
     @param businessID 业务ID，可以为空
     @return uTrace对象
     @since 8.1.0
     */
    + (uTrace *)createTraceWithTraceID:(NSString *)traceID businessID:(NSString *)businessID;

1.2 网关子设备绑定接口，绑定信息uSDKSlaveDeviceBindInfo中新增属性，用于RISCO设备接入
在uSDKSlaveDeviceBindInfo.h中新增加属性    

    /**
    通用扩展信息，可以为空，SDK不做校验，仅透传
     
    如：RISCO设备接入时，需传入扩展信息：ip/port/Identification Code，信息格式需App开发者与设备侧协商。
    @since 8.1.0
    */
    @property (nonatomic, copy) NSString *extendInfo;


2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 对于可配置设备，在获取到设备型号信息后再上报App    
3.2 根据云端配网策略，动态显示设备配网方式    

----------

- **iOS uSDK_8.0.0**   

版本号： v8.0.0  
发布日期：2020.12.07   
MD5值：EEBF44726B93E3519D79D7FF7D44A712  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.0.0_Phone_iOS_20201211175856730.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用   
更新日志：   
1.新增功能   
1.1增加BLE Mesh设备管理   
1.1.1 BLE Mesh设备搜索BLE Mesh设备进入配网模式后，可以通过自发现待入网设备接口   [uSDKDeviceScanner startScanConfigurableDevice]进行搜索，在返回的uSDK DeviceInfo中增加了uSDKDeviceConfigTypeBLEMesh的配网方式    


      typedef NS_OPTIONS(NSUInteger, uSDKDeviceConfigType) {
    	uSDKDeviceConfigTypeBLEMesh = (1UL << 3),
    };   

1.1.2 BLE Mesh设备配网对于支持BLE Mesh配网方式的设备，在配网时，需要调用新增的BLE Mesh配网接口   

    //uSDKBinding.h+ (void)bindBLEMeshDevice:(uSDKBLEMeshBindInfo *)bindInfo 
       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
      success:(void(^)(uSDKDevice *device))success
      failure:(void(^)(NSError *error))failure;   

1.1.3 BLE Mesh组设备对于BLE Mesh设备，可以将多个具有相同功能集的设备加入一个组，并生成一个新的组设备。 如:将房间内所有的灯加入一个组，会生成一个组设备，可以直接对组设备进行控制，即会对加入到这个组设备的所有设备生效    
1.1.3.1 获取可分组设备列表 可以根据某个device对象，获取可以与这个设备加入同一个组的设备列表    

    //uSDKDevice.h- (void)getBLEMeshGroupDeviceListSuccess:(void(^)(NSArray<uSDKDevice *> *devices))success failure:(void(^)(NSError *error))failure;   

1.1.3.2 创建设备分组   
调用获取可分组设备列表接口后，可以在返回的列表中选择一个或多个设备，与当前设备对象加入同一个组，创建成功后，会产生一个新的uSDKDevice设备 对象   
创建组的过程分为两步，第一步是创建组设备对象，第二步是将选择的设备和当前设备对象加入到这个新创建的组中     

    //uSDKDevice.h- (void)createBLEMeshGroupWithDevices:(NSArray<uSDKDevice *> *)devices 
    	timeoutInterval:(NSTimeInterval)timeoutInterval 
    	progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify 
    	success:(void(^)(uSDKDevice *device))success 
    	failure:(void(^)(NSError *error))failure;

1.3.3 向组设备中添加成员 组创建成功后，可以向组内继续添加新的成员，在调用该接口时，要求当前device对象为组设备    

    //uSDKDevice.h- (void)addDevices:(NSArray<uSDKDevice *> *)devices toBLEMeshGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval 
    progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
       success:(void(^)(void))success
       failure:(void(^)(NSError *error))failure;

1.3.4 从组内删除成员 组创建成功后，可以向组内继续添加新的成员，在调用该接口时，要求当前device对象为组设备    

    //uSDKDevice.h- (void)removeDevices:(NSArray<uSDKDevice *> *)devices fromBLEMeshGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval 
       progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
      success:(void(^)(void))success
      failure:(void(^)(NSError *error))failure;

1.3.5 删除组设备 组设备不能解绑，但可以删除，删除组设备后，会自动解绑    

    //uSDKDevice.h- (void)deleteBLEMeshGroupSuccess:(void(^)(void))success 
      failure:(void(^)(NSError *error))failure;

1.4 增加BLE Mesh OTA    
针对BLE Mesh设备OTA，依然沿用原FOTA接口，使用方式不变 。    
1.5是 增加BLE ADV设备管理    
1.5.1 BLE ADV设备搜索未绑定的BLE ADV设备可以通过自发现待入网设备接口[uSDKDeviceScanner startScanConfigurableDevice]进行搜索，在返回的uSDKDeviceIn fo中增加了uSDKDeviceConfigTypeBLEADV的配网方式     

    typedef NS_OPTIONS(NSUInteger, uSDKDeviceConfigType) {
    uSDKDeviceConfigTypeBLEADV = (1UL << 5),
    };

1.5.2 BLE ADV设备配网对于支持BLE ADV配网方式的设备，在配网时，需要调用新增的BLE ADV配网接口 
//uSDKBinding.h+ (void)bindBLEADVDevice:(uSDKBLEADVBindInfo *)bindInfo  

          progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
                 success:(void(^)(uSDKDevice *device))success
                 failure:(void(^)(NSError *error))failure;  

1.5.3 uSDK设备属性类新增设备属性的时间戳，精确到毫秒(目前该字段，只有BLE_ADV设备 才会有值， 开发者可以不用关心这个字段)     


    //uSDKBinding.h@property (nonatomic, assign) uint64_t timeStamp;   

1.6. 增加uSDK音视频P2P功能    
新增uSDKPlayer.h核心播放器， uSDKMonitorPlayer.h 监控播放器。 uSDKPlayer为抽象类，不要直接实例化，请使用其派生类:uSDKMonitorP layer使用监控播放器可以实现手机与设备之间 实时的音视频播放信息。    
1.7. 获取路由器信息    
当WiFi网络中存在已入网设备时，需要配置另外一台设备到该WiFi网络，可以通过此接口获取该WiFi网络的路由器SSID和密码      

    //uSDKBinding.h
    + (void)getConfigRouterInfo:(NSTimeInterval)timeoutInterval
    	success:(void(^)(uSDKConfigRouterInfo *routerInfo))success
    	failure:(void(^)(NSError *error))failure;

1.8 uSDKDeviceScanner增加wifi、ble开关通知    

    typedef NS_ENUM(NSUInteger, uSDKInvalidPermission) { 
    uSDKBleInvalid,
    uSDKNetWorkInvalid,
    };
    /** 
     @param scanner uSDKDeviceScanner
     @param invalidPermission
     */
    - (void)deviceScanner:(uSDKDeviceScanner*)scanner didPermissionInvalid:(uSDKInvalidPermission *) invalidPermission; 
2.接口变更  
 无  
3.内部优化及BUG修改   
3.1 针对smartLink配网“配置命令发送完成,但未找到上线设备”导致配网失败的问题优化   
3.2 针对softAp“用户侧绑定，配置命令发送完成，但未找到上线设备”导致配网失败的问题优化   
3.3 在前台时，WiFi或WWAN时会进行云连接   
3.4 修改配网ssid长度判断方式为字节长度，即如果ssid内容全部为中文，则最多支持10个中文字符的ssid    
3.5 解决uSDK配置文件被APP清除缓存功能删除后导致设备离线的问题   
3.6 增加绑定重试接口埋点    
3.7 切网优化   
3.8 wifi搜索改为调用组件的接口    
3.9. bindDeviceBySoftAp、bindDeviceByBLE、retryBindDeviceWithTimeoutInterval 三个接口中不支持bindCode的自绑定设 备，绑定结果从云端查询 。  
3.10 增加uSDKLanSearch类实现代理设备搜索 。uSDKDeviceScanner中实现搜索合并    
3.11 增加获取配网策略   
3.12. HAL层优化:OTA过程中蓝牙关闭时的一些事件。   
3.13 https请求中header中增加uTrace字段，包含绑定和查询绑定结果   
3.14 设备控制失败的时候，区分 是设备离线 还是手机离线   
3.15 不管httpDNS是否开启，cae和ucom都使用SDK的dns解析   
3.16 scanner中搜索到的可配网设备 获取deviceModelInfo后 才上报APP    
3.17 进入后台时，停止WiFi搜索，断开云连接，断开所有本地设备连接。以减少后台活动 。在前台时，只有在WiFi时才会开启WiFi搜索   


----------


- **iOS uSDK_6.1.1**     

版本号： v6.1.1  
 发布日期：2020.09.15  
 MD5值：1873681996C5DDB7BE42397D2C4C9C17  
 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.1_Phone_iOS_20200924135359763.zip)  
 注意事项1：此版本不支持海外环境    
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    
更新日志：    

1.新增功能  
 无   
2.接口变更   
 无   
3.内部优化及BUG修改     
3.1 组件修改编译脚本，使CAE wifi搜索中的锁生效   

----------

- **iOS uSDK_6.1.0**   

版本号： v6.1.0    
 发布日期：2020.09.04   
 MD5值：173A40442C4560EE5A106614497A6C32    
 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.0_Phone_iOS_20200904172640495.zip)   
 注意事项1：此版本不支持海外环境。   
 注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用    
 更新日志：  

1.新增功能    
1.1.  查询设备网络信号质量(见`uSDKDevice.h`)   

     @param success 成功的回调（uSDKNetworkQualityInfo：设备网络质量信息类，具体的网络质量指标 请进该类中查看）
     @param failure 失败的回调
     @since 6.1.0
     */
     - (void)getNetworkQualityV2Success:(void(^)(uSDKNetworkQualityInfoV2 *networkQuality))success
     failure:(void(^)(NSError *error))failure;

1.2 新增uSDK与设备的连接状态  

    typedef NS_ENUM(NSUInteger, uSDKDeviceConnectStatus) {  
     //大循环在线
    uSDKDeviceConnectStatusCloudConnected,
    
    // 大循环不在线、小循环在线
    uSDKDeviceConnectStatusLocalConnected,
    
    // 大小循环均不在线，BLE在线
    uSDKDeviceConnectStatusBleConnected,
       
    // 大小循环、BLE均不在线
    uSDKDeviceConnectStatusOffline
    };


1.3 新增设备网络质量信息类（见`uSDKNetworkQualityInfoV2.h`）   

    /**
     设备网络质量查询结果，其中connectStatus为uSDK与设备当前的连接状态，其他属性均为云平台返回的结果
     */
    @interface uSDKNetworkQualityInfoV2 : NSObject

1.4. 新增设备故障信息（见`uSDKFaultInformation`）    

//`uSDKDeviceInfo`类中新增了属性`faultInfo`，在设备广播故障期间，`app`同时可以从属性中获取当前设备的`故障信息`。

    @interface uSDKFaultInformation : NSObject
    
    @property (nonatomic, assign) NSInteger state;
    
    @property (nonatomic, assign) NSInteger stateCode;
    
    @end

1.5. 接收设备故障事件信息（见`uSDKDeviceDelegate`）   

//在设备配置绑定成功后，如果用户家里路由器断电或修改密码，会导致设备连不上路由器，此时设备(目前只有云芯二代3.2.00版本支持)通过蓝牙广播故障事件，手机侧在收到设备故障广播时会通过下面代理通知App.  

    @protocol uSDKDeviceDelegate;
    /// 设备故障信息上报
    /// @param device 设备对象
    /// @param faultInformation 故障信息类
    /// @since 6.1.0
    - (void)device:(uSDKDevice *)device didUpdateFaultInformation:(uSDKFaultInformation *)faultInformation;


1.6 通过蓝牙更新设备侧的路由器SSID和密码.    


 `app`在收到上面的代理回调后，可以调用下面的接口修改模块连接的`ssid`和`password`，修改成功后，会通过上面代理回调`App`，`state == 0, stateCode == 0`，并将属性`faultInfo`中的`state`和`stateCode`都置为0  

      目前只支持云芯二代3.2.00模块   
     @param ssid 路由器的ssid，必选
     @param password 路由器的密码，必选
     @param bssid 路由器的bssid，可选
     @param timeoutInterval 超时时间[30,120]
     @param progressNotify 更新进度通知
     @param success 成功回调
     @param failure 失败回调
     @since 6.1.0
      
    - (void)updateRouterSSID:(NSString *)ssid
    password:(NSString *)password
       bssid:(NSString *)bssid
     timeoutInterval:(NSTimeInterval)timeoutInterval
      progressNotify:(void(^)(uSDKRouterInfoUpdateProgress updateProgress))progressNotify
     success:(void(^)(void))success
     failure:(void(^)(NSError *error))failure;



2.接口变更   
 无     
3.内部优化及BUG修改    

3.1. `HTTPDNS`缓存机制优化, 本地进行`DNS`解析时，结果不进入缓存  
3.2. 设备列表使用安全的`NSDictionary`, 解决多线程访问可能导致的崩溃问题  
3.3. 离线分析埋点，增加云连接状态，设备连接状态，网络状态，前后台事件的埋点  
3.4. 配置绑定优化：SoftAP、BLE设备发起绑定带bindCode  
3.5. 技术优化：iOS和组件通信逻辑优化，解决组件返回消息时，SDK上层找不到对应请求的问题  
3.6. 基础连接优化：uSDK网络切换处理机制优化，在任何切网时都重启本地搜索，重启云连接，而不是根据网络状态进行连接或断开  
3.7. 配置绑定优化：softAp配网过程中同时监听BLE事件广播  






----------



- **iOS uSDK_6.0.1**    

版本号： v6.0.1   
发布日期：2020.07.27   
MD5值：D534E44EB36CF5C6887F0BEEE73E395D  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.1_Phone_iOS_20200727112723419.zip)  
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：   

1.新增功能   
 无      
2.接口变更  
 无   
3.内部优化及BUG修改   
3.1. 解决日志库命名冲突问题   
3.2. 获取设备列表方法加锁，防止设备特别多时，容易引起多线程访问导致的崩溃   


----------

- **iOS uSDK_6.0.0**

版本号： v6.0.0  
发布日期：2020.07.13  
MD5值：8BAEFC10DED08895D23C5D393CF37B77  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.0_Phone_iOS_20200713111814031.zip)   
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：  

1.新增功能  
1.1  启动待配置状态的新直连设备搜索  
`[uSDKDeviceScanner startScanConfigurableDevice]；`  

1.2 验证码方式绑定新直连设备  

1.2.1 获取新直连绑定验证码方式绑定信息（见uSDKNewDirectLinkVerificationCodeBindInfo）  

    /**
     通过uSDKDeviceScanner扫描到的新直连设备对象，device.configType & uSDKDeviceConfigTypeNewDirectLink == 1表示该设备支持新直连配网
     */
    @property (nonatomic, strong) uSDKDeviceInfo *deviceInfo;
    // 验证码  长度[6,63]
    @property (nonatomic, copy) NSString *verificationCode;

1.2.2 验证码方式绑定新直连设备   

    @interface uSDKbinding
    /**
     新直连设备手动确认方式绑定接口
    
     @param bindInfo 新直连绑定验证码方式绑定信息
     @param progressNotify 进度上报，共四个进度上报
     1.参数校验成功后上报uSDKBindProgressConnectDevice
     2.与设备建立连接成功时上报uSDKBindProgressSendConfigInfo
     3.设备开始校验时上报uSDKBindProgressVerification
     4.设备开始绑定时上报uSDKBindProgressBindDevice
     @param success 配网成功时的block回调
     @param failure 配网失败时的block回调
     @since 6.0.0
     */
     +(void)bindNewDirectLinkDeviceWithManualConfirmBindInfo:(uSDKNewDirectLinkManualConfirmBindInfo *)bindInfo
       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
      success:(void(^)(uSDKDevice *device))success
      failure:(void(^)(NSError *error))failure;


1.3 手动确认校验方式绑定新直连设备   

1.3.1 获取新直连绑定验证码方式绑定信息（见uSDKNewDirectLinkManualConfirmBindInfo）   

    /**
     通过uSDKDeviceScanner扫描到的新直连设备对象，device.configType & uSDKDeviceConfigTypeNewDirectLink == 1表示该设备支持新直连配网
     */
    @property (nonatomic, strong) uSDKDeviceInfo *deviceInfo;
    
    1.3.2 手动确认校验方式绑定新直连设备（见uSDKbinding）
    /**
     新直连设备手动确认方式绑定接口
     @param bindInfo 新直连绑定验证码方式绑定信息。
     @param progressNotify 进度上报，共四个进度上报
     1.参数校验成功后上报uSDKBindProgressConnectDevice
     2.与设备建立连接成功时上报uSDKBindProgressSendConfigInfo
     3.设备开始校验时上报uSDKBindProgressVerification
     4.设备开始绑定时上报uSDKBindProgressBindDevice
     @param success 配网成功时的block回调
     @param failure 配网失败时的block回调
     */
     +(void)bindNewDirectLinkDeviceWithManualConfirmBindInfo:(uSDKNewDirectLinkManualConfirmBindInfo *)bindInfo
       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
      success:(void(^)(uSDKDevice *device))success
      failure:(void(^)(NSError *error))failure;


1.4 自发现蓝牙设备可发现已配置的蓝牙设备  

通过`uSDKDeviceScanner`类发现的蓝牙设备列表中，如果***设备已配置，但未连接***，也依然可以通过该类发现，在`uSDKDeviceInfo`中新增属性用于标识设备的配置状态  

    //设备当前可配置状态
    typedef NS_ENUM(NSUInteger, uSDKDeviceConfigState) {
    //设备当前状态已配置
    uSDKDeviceConfigStateConfigured,
    //设备当前状态可配置
    uSDKDeviceConfigStateConfigurable,
    //设备当前状态可触发进配置
    uSDKDeviceConfigStateTriggerConfigurable,
    };
    
    @interface uSDKDeviceInfo
    //设备当前的可配置状态
    @property (nonatomic, assign, readonly) uSDKDeviceConfigState configState;


1.5 uSDK启动项里增加开启蓝牙搜索配置(见uSDKStartOptions)  

    //默认为YES，表示开启BLE可控设备搜索，如果不想开启，则将该属性置为NO
    @property (nonatomic, assign) BOOL enableBLEControllableSearch;

1.6 大循环下获取设备的模块信息(见uSDKDevice)   

    /**
     获取设备模块信息
     @param timeout 超时时间(s)，最小5s，最长120s，建议值15s
     @param success 成功的回调, 返回uSDKModuleInfo对象
    1. 接口只要执行成功，就会通过success block返回
    2. 但uSDKModuleInfo中具体数据依赖云平台，可能有的字段是空的，可能全都是空的
     @param failure 失败的回调, 返回NSError对象
     */
    - (void)deviceModuleInfoWithTimeout:(NSTimeInterval)timeout
    success:(void(^)(uSDKModuleInfo *info))success
    failure:(void(^)(NSError *error))failure;

1.7 无效命令  

对支持无效命令的设备，进行操作`(read/write/op)`，触发无效命令时，会携带无效命令标识上报给`App`，无效命令标识的值放到`NSError`的`NSLocalizedFailureReasonErrorKey`对应的值中  

1.8 softAp通知App切网  
无论设备侧发起绑定，还是用户侧发起绑定，在发送配置信息后都会通过`switchNetworkNotify`通知`App`进行切网  

1.9 配置绑定增加重试接口   
当配置绑定返回`ERR_USDK_BIND_TIMEOUT_NEED_RETRY_BIND（-16018）`时，可以通过该接口尝试进行重试绑定  

    /**
    绑定重试接口
    当错误码为ERR_USDK_BIND_TIMEOUT_NEED_RETRY_BIND（-16018）时需要调用重试接口试图重新绑定设备，
    会返回-16018的接口bindDeviceByBLE、bindPureBLEDevice、bindDeviceBySoftAp、bindDeviceBySmartLink、bindDeviceByQRCode、bindNewDirectLinkDevice
    
    @param timeoutInterval 绑定超时时间（单位是秒，范围为10秒-180秒）
    @param success 绑定成功时的block回调
    @param failure 绑定失败时的block回调
    @since 6.0.0
     */
     +(void)retryBindDeviceWithTimeoutInterval:(NSTimeInterval)timeoutInterval
       success:(void(^)(uSDKDevice *device))success
       failure:(void(^)(NSError *error))failure;

2.接口变更   

2.1针对`softap`配网，修改参数`deviceBSSID`为`deviceSSID`, softap配网不再校验`iotDevBssid`(见uSDKSoftApBindInfo)  

    /**
     设备SoftAp热点的SSID，必填
     如果在调用softap绑定接口时，手机连接的热点与该属性的值不匹配，则返回-13016(未连接到设备热点)错误
     */
    @property (nonatomic, copy) NSString* iotDeviceSSID;
    
    /**
     设备SoftAp热点的BSSID
     @deprecated 6.0.0 属性废弃后，不再校验该属性
     */
    @property (nonatomic, copy) NSString* iotDevBssid DEPRECATED_ATTRIBUTE;

3.内部优化及BUG修改   

3.1 历史数据，uSDK收到BLE模块发送的空包导致的异常  
3.2bundleID重名导致的崩溃  
3.3 并发授权时导致的崩溃  
3.4  uSDK就绪性能优化  
3.5 iOS下载配置文件使用httpdns  
3.6 长连接建立&重连机制优化  
3.7 用户侧主动绑定和查询绑定结果增加http通道  
3.8 -10003错误码细化  
3.9. 配置绑定优化：无效参数（10001）问题的解决  

----------


- **iOS uSDK_5.8.1**

版本号： v5.8.1  
发布日期：2020.05.20  
MD5值：96CDF4D39DD2B765AFEA6DB1118A1F46  
注意事项：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：  

1.新增功能  
1.1新增非安全设备获取bindInfo接口(见SDKBinding.h)  

         +(void)unSafeDeviceBindInfoWithSuccess:(void(^)(NSString *bindInfo))success
         failure:(void(^)(NSError *error))failure;

2.接口变更    
 无    
3.内部优化及BUG修改   
3.1 `uSDKDevice`中的`getBindInfo`方法中增加`self class`类型判断，防止自创建`uSDKDevice`类型崩溃   
3.2 修复`uSDKDeviceListManager`中方法`uSDKDidConnectToCloud`疑似导致crash问题  
3.3 打印日志功能，`os_log`打印全部使用`os_log_error`，使windows第三方工具也可以抓到实时日志  
3.4 打印日志功能，DEBUG模式下采用sync方式打印日志，方便crash时的问题追踪(async时，崩溃后最后的日志还是打印不出来)；RELEASE模式下采用async方式打印日志，防止sync可能导致的阻塞问题  



- **iOS uSDK_5.7.0**

版本号： v5.7.0  
发布日期：2020.02.27  
MD5值：5F449B0E1A1F9B5E4C7A0717927D700F   
注意事项：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  

更新日志：  

1.新增功能  

1.1 标记设备进入焦点（详情页）(见uSDKDevice类)  

    //进入焦点后，如果大循环控制超时，则提升蓝牙通道的优先级高于大循环且低于小循环通道。重复进入返回NO
    
    - (BOOL)inFocus;

 




1.2 标记设备退出焦点（详情页）(见uSDKDevice类)  

    // 重复退出返回NO
    
    - (BOOL)outFocus;   


1.3 新增zigbee绑定_失败错误码   

    // App进入后台导致的超时
    
    ERR_USDK_ENTER_BACKGROUND_TIMEOUT = -13026

 




1.4 新增蓝牙配置时设备需要触发进配置的错误码   

    // 设备需要触发进配置
    
    ERR_USDK_DEVICE_NEED_TRIGGER_CONFIG = -13027

 




2.接口变更  
  无    
3.内部优化及BUG修改  
3.1 优化账号下绑定设备非常多时,上线慢的问题。  
3.2 fix 控制设备时可能出现crash的问题。  
3.3 fix _蓝牙搜索中可能会因为deviceID==nil 造成的crash问题。  
3.4 zigbee配网内部优化。  
3.5 uSDK使用新的log功能。  
3.6 控制打点，args中增加个新字段：APP版本号 – pver。  
3.7 cloudoffline 打点， 去掉app前后台这个字段。  


----------

- **iOS uSDK_5.6.1**

版本号： v5.6.1  
发布日期：2019.12.13  
MD5值：121D2C1ED1D28C5DDD678FFE3887A3AF   
注意事项：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：  
1.新增功能  
1.1 新增网关子机绑定信息类(uSDKSlaveDeviceBindInfo)   

    //该对象必填,需要使用从uSDK中返回的设备对象，不能自己创建
    @property (nonatomic, strong) uSDKDevice *masterDevice;
    //设备的uplusID，必填
    @property (nonatomic, copy) NSString *uplusID;
    //设备的产品编码，必填
    @property (nonatomic, copy) NSString *productCode;
    //超时时间，30-120s，默认60s
    @property (nonatomic, assign) NSTimeInterval timeoutInterval;

1.2 新增开始进入组网模式阶段的错误码（uSDKBindProgresStartNetworkingMode）  

    //参数为空
    UGW_RSP_INVALID_PARAM -25001
    //设备类型不支持
    UGW_RSP_INVALID_PRODUCT_CODE  -25023
    //配对模式已开启，重复开启
    UGW_RSP_REPEAT_STARTPAIR  -25024
    //开启配对失败
    UGW_RSP_STARTPAIR_FAILED  -25022

1.3  新增开始组网阶段的错误码（uSDKBindProgresStartNetworking）  

    //配对超时
    UGW_RSP_PAIR_TIMEOUT -25026
    //配对设备个数达上限
    UGW_RSP_PAIR_DEV_MAX  -25025

1.4 新增开始绑定子机阶段的错误码（uSDKBindProgresStartBindSlaveDevice）   

    //添加并绑定设备失败  
    UGW_RSP_BIND_ERROR -25027
    //退出配对模式（人为操作设备或APP取消配网）
    UGW_RSP_EXIT_PAIR -25028

1.5 新增网关子机绑定功能（uSDKBinding）  

    /**
    网关子机设备与网关配对，并将网关子机设备绑定到云平台
    调用该接口前，需要成功调用uSDKDeviceManager中的connectToCloud接口
    @param bindInfo 网关子机绑定信息类
    @param progressNotify 进度回调，共三个进度回调：
    1. uSDKBindProgressStartEnterNetworkingMode
    2. uSDKBindProgressStartNetworking
    3. uSDKBindProgressStartBindSlaveDevice
    @param success 配置绑定成功时的block回调，只有配置和绑定都成功会通过success block进行回调
    @param failure 配置绑定失败时的block回调，任何一个过程失败都会通过failure block进行回调
    */  
    + (void)bindSlaveDeviceWithBindInfo:(uSDKSlaveDeviceBindInfo *)bindInfo
     progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
    success:(void(^)(uSDKDevice *device))success
    failure:(void(^)(NSError *error))failure;

2.接口变更  
 无   
3.内部优化   
 无  




[info.pllist]:../App-dev/_media/info-pllist.png
[控制流程图]:../App-dev/_media/pic6.png
[Objc]:../App-dev/_media/Objc.png
[基本业务流程]:../App-dev/_media/pic1.png
[配置入网流程]:../App-dev/_media/pic2.png
[设备的连接状态]:../App-dev/_media/pic4.png
# 移动端 SDK Android #

## 实验开发环境搭建 ##

### 准备设备和软件

①一台2.4G无线路由器
②一台Android4.4以上智能手机，支持蓝牙BLE
③一台支持U+协议的智能设备（安全版本智能设备）
④硬件测试工具App或uSDKDemoApp

### uSDK权限配置

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

### 开发测试环境配置及确认

我们在海极网申请开发App后，使用开发者环境开发测试，产品发布时转入生产环境。
U+智能设备（uPlug）默认连接生产环境，App开发测试阶段，修改路由器DNS，智能设备配置到此路由，保证设备连接开发者环境。
AppId、Appkey统一通过海极网上申请，开发者中心-我的应用 ，移动应用、云应用。开发者环境使用的AppKey或systemKey为开发key值

### 配置开发者环境

设备联网使用的路由器DNS设置为39.97.52.209  
注意：wifi模块的版本需要支持，smartDevice设备需要关闭Ht tpDNS服务。   

## 业务梳理及开发快速入门 ##

### 基本业务流程

本图简要介绍uSDK物联App主要业务运行流程，红色文字：App与uSDK交互，黑色文字：App与U+云交互。   

 ![基本业务流程][pic1]

### 配置入网流程

考下列流程开发配置设备入网功能，特别说明：启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。      
 ![配置入网流程][pic2]


### 使用uSDK控制设备流程说明图

以下流程图适用于使用uSDK进行设备配置入网，控制的App，OPENAPI请更换为UWS或对应服务。    
 ![控制流程图][pic3]

## uSDK业务开发详解 ##

### 启动uSDK

启动uSDK是使用U+物联功能的前提。为App配置好海极网验证信息，启动uSDK。    

**启动uSDK**  

首先执行初始化方法，然后执行具体启动方法：    

    //推荐在Application对象实现中执行,   
    //或者在所有uSDK操作之前，入参需要是ApplicationContext   
    uSDKManager.init(ApplicationContext);    

**初始化启动参数** 

`uSDKStartOptions USDK_START_OPTION = new uSDKStartOptions.Builder()`    

**设定uSDK日志级别**   

uSDK默认输出所有日志，日志标签是uSDK。uSDK提供API支持App设定日志输出级别，uSDK启动成功后，执行即可：  

    uSDKManager.initLog(uSDKLogLevelConst level, boolean isWriteToFile, IuSDKCallback)。  

开发测试阶段将日志级别设置为DEBUG

 **启动uSDK**

uSDKMgr.startSDK(USDK_START_OPTION, new ICallback<Void>() 
注意
①uSDK成功启动一次即可，不要多次、多个线程启动
②uSDK成功启动之后再调用uSDK提供的各类方法
③启动入参Context必须是ApplicationContext

### 配置设备入网和绑定

配置设备入网就是使用uSDK将智能设备加入指定WIFI的一项操作。   

**SmartLink入网并绑定**  

①App在智能设备入网准备阶段（例如引导操作设备和输入无线密码），构造SmartLink绑定信息类：   

    SmartLinkBindInfo smartLinkBindInfo = new SmartLinkBindInfo.Builder() 

②入网和绑定，在onSucess回调中获得设备对象实例，此时设备已经被绑定，可以通过UWS接口查询到设备信息：

    bindUtil.bindDeviceBySmartLink(smartLinkBindInfo, 
    		new IBindResultCallback<uSDKDevice>() 

**SoftAP入网并绑定**	   

①询问目标WIFI时，构造SoftAP绑定信息类:

    SoftApBindInfo.Builder builder = new SoftApBindInfo.Builder()   
②手机成功连接设备热点U-XXXX时，执行入网和绑定

    Binding.getSingleInstance().bindDeviceBySoftAp(softApBindInfo, 			
    			new ISoftApResultCallback<uSDKDevice>()

**蓝牙绑定WIFI设备**   

1.首先实现并设置蓝牙待入网设备回调，先注册再使用：    

    DeviceScanner.setListener(new IDeviceScannerListener() {
    
    ​public void onDeviceAdd(ConfigurableDevice unJoinedDevice) {}
    
    ​public void onDeviceRemove(ConfigurableDevice configurableDevice) {}}）

​    

2.调用API开始扫描：    

    DeviceScanner.startScanConfigurableDevice(new IuSDKCallback()

3.App准备绑定信息，向用户收集无线名称、密码拼装BLEBindInfo对象：    

    bleBindConfigInfo = new BLEBindInfo.Builder()

4.执行设备绑定：    

     Binding.getSingleInstance().bindDeviceByBLE(    
    canciate, bleBindConfigInfo, new IBindResultCallback<uSDKDevice>() 

5.停止扫描   

设备绑定成功，停止扫描并询问关闭蓝牙   

    DeviceScanner.stopScanConfigurableDevice(new IuSDKCallback() 

6.ConfigurableDevice对象的有用方法    

    ConfigurableDevice.getBleName()：返回蓝牙设备的名称    
    ConfigurableDevice.getDeviceId：返回设备在无线时的DeviceId，通常是设备MAC    
    ConfigurableDevice.getType()：获得设备大类    

 **纯蓝牙智能设备绑定** 

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

**绑定SmartDevice担当网关外围设备**

第一步**：**组织绑定信息

    SlaveDeviceBindInfo friendInfo = new SlaveDeviceBindInfo.Builder()      

第二步：执行绑定

    Binding.getInstance().bindSlaveDevice(friendInfo, new IBindResultCallback<uSDKDevice>() 

以下为API文档列举错误码:

| -10001     | 无效参数错误                              | **14010**| 请先调用connectToCloud  接口 | **-13023** | 进入组网模式超时         |
| ---------- | ----------------------------------------- | ---------- | ---------------------------- | ---------- | ------------------------ |
| **-13024** | 获取绑定结果超时                          | **-13025** | 等待组网结果超时             | **-25002** | 参数为空                 |
| **-25022** | 开启配对失败                              | **-25023** | 设备类型不支持               | **-25024** | 配对模式已开启，重复开启 |
| **-25026** | 配对超时                                  | **-25025** | 配对设备个数达上限           | **-25027** | 添加并绑定设备失败       |
| **-25028** | 退出配对模式（人为操作设备或APP取消配网） |            |                              |            |                          |

 

**自带屏幕或操作系统设备入网** 

对于一些高级、大型家电或智能设备，其自身可能携带操作系统并且带有显示装置，此时我们应将其视为普通计算机，由其自行引导使用者连接网络。   



### 发现及管理网络设备： 

**接收uSDK管理设备集变化回调** 

App依靠uSDK管理智能设备。uSDK将网络里所有的智能设备都管理起来，形成设备池，设备池里设备的数量会发生变化，例如：配置一台新设备入网，配置过的设备上电加入网络等。uSDK将以上情况都汇报给App，App编程实现设备管理。   

    uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener() {
    
    public void onDevicesRemove(List<uSDKDevice> devicesChanged)
    
    public void onDevicesAdd(List<uSDKDevice> devicesChanged) }
注意:    
①本接口是即时接口，在uSDK启动前设置。    
②如果延时设置或再次设定本接口，需要结合查询设备所有列表使用，确保智能设备列表数据齐全。   
 **设定设备过滤类型**  

App只关心设备池里的某类设备（例如：酒柜设备）：    

    uSDKDeviceManager. setInterestType(uSDKDeviceTypeConst.WINE_CABINET)

**App实现管理设备集变化回调**

此时uSDK只向App上报uSDKDeviceTypeConst.WINE_CABINET类型的设备，实现设备过滤。    
注意:再次执行setInterestType方法，将覆盖已设定的设备过滤类型。    

获得设备池全集  
    uSDKDeviceManager.getDeviceList()，本方法也支持设备类型过滤。

### 与设备建立或断开连接

 **设备的连接状态：** 

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

** 设备连接状态变化接口**  

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

**连接设备**

    uSDKDevice.connectNeedProperty，使用本方法。

**断开连接**

不需要某台设备属性数据时，断开设备连接，释放设备资源：  
    `uSDKDevice.disconnect。`

**主动查询连接状态**

`uSDKDevice.getStatus()`。

注意：   
①断开设备连接，uSDK将不再上报此设备的任何属性消息给App。    
②当用户解绑定设备时，一定执行断开设备连接。   
③Android设备熄屏或进入后台强制uSDK与设备保持连接请执行：   
`uSDKDeviceManager.setKeepLocalOnline(true)`，uSDK启动成功后执行本方法。    

### 获得设备属性状态

**获得设备的属性状态**

1)App实现并注册如下接口，准备接收设备属性的实时值：   
`uSDKDevice.setDeviceListener(new uSDKDeviceListener()`   
2)App连接智能设备    
3)App根据ID文档发送查询指令,指令执行成功后，就可以在接口里接收属性数据了。    

**App主动获得当前设备所有属性值**

当我们第一次通过接口获得设备的属性状态后，只要设备已连接或就绪，调用    `uSDKDevcie.getDevcieAttrMap`返回设备最新属性值合集。    

### 处理设备故障和报警

连接设备后，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载   

**获得报警消息**  

实现`uSDKDeviceListener`获得报警消息，报警消息和设备属性状态变化是同一接口。  

    uSDKDevice .setDeviceListener(new uSDKDeviceListener() {
    
    public void onDeviceAlarm(uSDKDevice myDevice, List<uSDKDeviceAlarm> alarmList) }

**发送停止报警指令** 

智能设备或家电可能向uSDK规律性快速报警，App处理完成后，可以向设备发送停止报警指令，用于表示使用者已经了解设备发生故障。停止报警是一条普通单命令，参考ID文档编写。    

**获得报警解除消息**

发生故障的设备修理正常后会发送报警解除消息，报警解除就是没有报警，就是正常。报警解除消息与普通报警消息都在onDeviceAlarm()接口方法产生，App注意分辨。报警解除的具体值，请参考设备ID文档或设备模型文档。    

**主动查询设备报警 **

App可以调用API主动查询设备报警信息   

    ArrayList<uSDKDeviceAlarm> mAlarms = mDevice.getAlarmList()

### 执行设备控制

uSDKDevice提供API用于执行设备控制。执行控制时，可以给设备发送单命令、组命令，设定超时时间，命令执行后会反馈结果。

**发送单命令** 


    selecteduSDKDevice.writeAttribute("name", "value", new IuSDKCallback()

**发送组命令**


    List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>()
组命令字要求全部大写    
注意：    
①组命令说明中需要的指令放入mAttrs，mAttrs没有顺序要求。   
②组命令标识字具体值参考设备的ID文档。   
③App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。    

**命令超时设定**

uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。建议超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。   

**命令执行结果与设备反馈** 

命令执行结果在IuSDKCallback回调中给出。uSDKErrorConst.RET_USDK_OK代表发送成功，uSDKErrorConst.RET_USDK_TIMEOUT_ERR代表超时。   
设备执行成功有可能改变设备的属性值，App接收设备的最新属性上报即可。    

### 远程控制及U+云平台推送消息

App执行uSDKDeviceManager.connectToCloud连接用户接入网关，账号登录后按以下内容编程连接用户网关，获得远程和智能设备交互的能力   

1）用户接入网关：U+云支持App实现远程功能的服务器软件系统。UWS：U+云为开发人员提供的REST API，主要和用户设备等业务系统交互。   

2）accessToken(session)：App用户账号登录后分配，userId：用户注册并登录后获得。

3）绑定、设备绑定：由App发起将用户及所属设备数据上送到云并创建设备数据档案的过程，设备能够被远程控制的前提是已经被用户绑定。

4)切网：用户由设备所在A路由切换到B路由，或改用数据网络的行为。

5)小循环VS大循环：它们是两种情景的描述。小循环指的是App与智能设备在同一无线局域网。大循环是说App需要借助U+云用户接入网关才能和设备进行交互的情景，此时App与设备通常不在同一网络。

 ![大循环][pic5]

6) 由于加密需要，设备运行设备时间必须准确，当前时间需要超过2015年3月1日

**连接用户接入网关**

    uSDKUserInfo userInfo = new uSDKUserInfo.Builder()
    deviceManager.connectToCloud(userInfo, new ICallback<Void>() 
账号登录后执行一次即可。   

**远程服务器的连接状态**

uSDKDeviceManager提供一个回调，可以了解我们当前与远程服务器的连接状态：
uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListener()  

**处理绑定一台新设备**

当绑定一台新设备时，需要驱动uSDK刷新交互设备列表，uSDKDeviceManager.refreshDeviceList()方法。   

**用户解绑定时解除设备远程能力**

用户解绑定设备成功时，已经失去对设备的控制权，断开设备连接，释放设备对象实例，调用uSDKDeviceManager.refreshDeviceList()驱动uSDK刷新设备列表。   

注意    
如果不是用户主动解绑设备，但收到接入网关推送的设备解绑定消息（此时设备可能被他人解绑定），App需要编写逻辑处理此消息：   
uSDKDeviceManager.setDeviceManagerListener()

**账号注销时的处理**

当用户账号注销时，务必调用 uSDKDeviceManager.disconnectToCloud断开远程链接，清空账号数据。 

**如何测试远程功能是否正常**

手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、4G数据网络，查询设备状态、进行控制。

注意    
1．当用户切网时，uSDK自动尝试连接用户接入网关，查询设备的最新状态。   
2．App在开发者环境绑定设备，智能设备也要连接开发者环境，开发环境始终保持一致。   
3．App使用UWS过程中，U+云平台要求App有自己的ClientId。ClientId和终端的身份验证是相关的，错误或固定的ClientId会产生异常行为，App可以使用uSDKManager. getClientId()方法产生ClientId。   

**接收U+云推送消息**

App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。  

实现如下方法接收云的推送消息：   

    uSDKManager.setManagerListener(new IuSDKManagerListener(){
    
    public void onBusinessMessage(String content)})

**接收U+云Session异常消息**

当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，检查程序逻辑，查看用户登录地址与连接用户接入网关地址是否同一服务器环境。

    uSDKManager.setManagerListener(new IuSDKManagerListener() {
    
    public void onSessionException(String session)})

### 获取设备WIFI信号强度

可以使用uSDKDevice实例获取设备的Wifi信号强度，注意必须是WIFI设备

    uSDKDevice.getDeviceNetQuality(new IuSDKGetDeviceNetQualityCallback() {
    public void onDeviceNetQualityGet(
    		uSDKErrorConst result, uSDKDeviceNetQuality level, int detailValue)});

### 和带子机设备交互

带子机设备通常有商用空调、星盒等设备。一台主机可以有多台子机，子机从属于主机，主机拥有mac，子机没有。在程序中我们可以和主机、子机进行交互，主机需要连接，子机不需要连接。  

**获得子机实例**

当主机已连接后，通过向主机发送查询指令（根据设备模型文档或ID文档），由主机动态上报子机实例（一会上报一台或连续上报子机实例信息），在以下回调中就可以得到子机实例。

    parentuSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
    public void onSubDeviceListChange(
    		uSDKDevice currentDevice, ArrayList<uSDKDevice> childArrayList) });

**子机连接状态、属性及报警**   

子机有三种连接状态：离线、已连接、就绪。子机的属性、报警将通过回调上报给App，实现以下接口并设定到子机对象实例上：   

    private class ChildDeviceCtrlListner implements IuSDKDeviceListener {
    	public void onDeviceAlarm(uSDKDevice uSDKDevice, List<uSDKDeviceAlarm> list) 
    	public void onDeviceAttributeChange(uSDKDevice childDevice, List<uSDKDeviceAttribute> list) 
    	public void onDeviceOnlineStatusChange(
    uSDKDevice uSDKDevice, uSDKDeviceStatusConst uSDKDeviceStatusConst, int i) }}

**子机控制**

子机只要不是离线状态，我们就可以和子机进行交互，对子机的指令仅能影响子机本身，子机的实例对象也是uSDKDevice。   

**主机控制**

我们可以像普通设备一样和主机交互，其中一些指令将影响到附属的所有子机。   

**辅助方法 **

selectedChildDevice.getMainDevice()：获得主机   
selectedChildDevice.isSubDevice()：子机？   
selectedChildDevice.isMainDevice()：主机？  
selectedChildDevice.getSubType()：子机类型   
selectedChildDevice.getSubId()：子机Id   
parentDevice.getSubDeviceList()：获得当前有哪些子机  
parentDevice.getSubDeviceById()：根据子机Id获得子机实例   

### 订阅SmartDevice设备资源

uSDK已经支持您获得SmartDevice设备图片资源  

当需要获得设备资源时，需要提前为设备设置监听IuSDKDeviceListenerWithResource，实现onReceive方法。设备资源名称请您咨询设备实现方，当前设备是否可以存取资源请根据typeid判断。   

具体步骤：   
①设置资源设备监听实现方法   
②连接设备使其就绪   
③订阅资源名称   
④使用完毕释放资源   
selecteduSDKDevice.setDeviceListener(new IuSDKDeviceListenerWithResource() )

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



## Android版资料 ##   

**1. uSDK开发手册**  

简介：uSDK开发手册的使用对象是使用uSDK开发APP的开发者。开发者可以通过此手册，可以了解uSDK的用法、关键流程以及常见问题。    

**2. uSDK Demo**

简介：uSDK示例工程的使用对象是使用uSDK的APP开发者。开发者通过此示例工程，可以了解uSDK的应用方法及流程。     


**3. Android uSDK开发包下载**   

支持有效期：新版本SDK发布起，APP新接入大版本SDK的支持有效期为6-12个月，APP新接入小版本SDK的支持有效期为3-6个月。   

----------

- #### Android uSDK_8.5.0

  版本号： v8.5.0
  发布日期：2021.04.30
  MD5值：A32B8E21261D917CE8BE775004FE83F3
  下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.5.0_Phone_Android_20210429155736_20210511092354389.zip) 
  更新日志：

  ##### 新增接口

  1.新增类
  MonitorPlaybackPlayer – 音视频回放核心类
  IMonitorPlayerListener – 音视频回放状态接口
  MonitorPlaybackListener – 获取音视频列表回调类
  MessagePlaybackNode – 音视频播放节点信息
  MonitorCloudVideoListener – 获取云端音视频回调类

  2.MonitorPlaybackPlayer.java接口说明

  ```
  
  
  ​```
  /**
  
   * 初始化视频回放
   * @param sdkDevice
   * @param iCallback
     */
     public static void createPlaybackPlayer(uSDKDevice sdkDevice, VideoView videoView, ICallback<MonitorPlaybackPlayer> iCallback)
  
  示例：
  MonitorPlaybackPlayer.createPlaybackPlayer(mDevice, sdkVideoView, new ICallback<MonitorPlaybackPlayer>() {
      @Override
      public void onSuccess(MonitorPlaybackPlayer monitorPlaybackPlayer) {
          mMonitorPlaybackPlayer = monitorPlaybackPlayer;
          addPlaybackListener();
      }
  
      @Override
      public void onFailure(uSDKError error) {
           
      }
  
  });
  
  /**
  
   * 播放器状态回调监听
   * @param monitorPlayerListener
     */
     public void addPlayerListener(IMonitorPlayerListener monitorPlayerListener)
  
  /**
  
   * 获取设备端sd卡中存在录像的日期列表
   * @param startTime (单位毫秒)
   * @param endTime (单位毫秒)
   * @param pageIndex 查询页
   * @param countPerPage 每页大小
   * @param monitorPlaybackListener
     */
     public void getPlaybackDateList(long startTime, long endTime, int pageIndex, int countPerPage, MonitorPlaybackListener<MonitorPlaybackExistDateMessage> monitorPlaybackListener)
  
  /**
  
   * 获取回放详情列表
   * 开始时间-结束时间的时间差不允许大于一个月
   * @param startTime (单位毫秒)
   * @param endTime (单位毫秒)
   * @param pageIndex 查询页
   * @param countPerPage 每页大小
   * @param recordType 筛选类型, null为不筛选
   * @param monitorPlayerListener 回调
     */
     public void getPlaybackList(long startTime, long endTime, int pageIndex, int countPerPage, String recordType, MonitorPlaybackListener<MonitorPlaybackMessage> monitorPlayerListener)
  
  /**
  
   * 指定时间播放
   * @param startTime 时间（毫秒）
   * @param messagePlaybackNode 播放节点信息
     */
     public void seekToTime(long startTime, MessagePlaybackNode messagePlaybackNode)
  
  /**
  
   * 播放
     */
     public void videoPlay()
  
  /**
  
   * 是否正在播放
   * @return
     */
     public boolean isPlaying()
  
  /**
  
   * 暂停
     */
     public void pause()
  
  /**
  
   * 继续
     */
     public void resume()
  
  /**
  
   * 停止
     */
     public void videoStop()
  
  /**
  
   * 设置是否静音
   * @param on true(静音) false(非静音)
     */
     public void mute(boolean on)
  
  /**
  
   * 设置播放数据信息
   * @param startTime （毫秒）
   * @param messagePlaybackNode 播放节点信息
     */
     public void setDataResource(long startTime, MessagePlaybackNode messagePlaybackNode)
  
  /**
  
   * 获取可以查看的照片列表
   * @param startTime (单位秒)
   * @param endTime (单位秒)
   * @param count 每页条数 0~10
   * @param photoId 图片ID,查询首页图片传0，查询下一页传上一个最后一个图片的ID
   * @param monitorPlaybackListener
     */
     public void getDevicePhotoList(long startTime, long endTime, int count, int timeout, MonitorPlaybackListener<List<MonitorPhotoListBean>> monitorPlaybackListener)
  
  /**
  
   * 获取本地图片详情
   * @param photoId 图片ID
   * @param timeout 超时时间  单位秒
     */
     public void getLocalPhotoDataByPhotoId(int photoId, int timeout, ICallback<byte[]> iCallback)
  
  /**
  
   * 获取有云存视频可播放日期信息
   * 用于终端用户在云存页面中对云存服务时间内的日期进行标注，区分出是否有云存视频文件
   * @param timezone 相对于0时区的秒数，例如东八区28800
   * @param monitorCloudVideoListener
     */
     public void getCloudVideoDateListByTimezone(int timezone, MonitorCloudVideoListener<DateBean> monitorCloudVideoListener)
  
  /**
  
   * 获取云回放存在视频数据的时间段列表
   * 由于查询的数据大小限制，起始到结束时间差需要小于等于一天
   * @param startTime 单位秒
   * @param endTime 单位秒
   * @param monitorCloudVideoListener 回调
     */
     public void getCloudVideoPlayListByDeviceId(long startTime, long endTime, MonitorCloudVideoListener<List<VideoPlayListBean.DataBean>> monitorCloudVideoListener)
  
  /**
  
   *  获取回放的 m3u8 列表
   *  @param startTime 单位秒
   *  @param endTime 单位秒
   *  @param monitorCloudVideoListener 回调
      */
      public void getVideoPlayAddressByDeviceId(long startTime, long endTime, MonitorCloudVideoListener<PlayAddressBean> monitorCloudVideoListener)
  
  /**
  
   * 释放资源
     */
     public void destroyPlayer()
  
  2. 新增回调接口
     public interface IMonitorPlayerListener {
  
      /**
  
       * 播放器状态回调
       * @param status
         */
          void onPlayerStatusUpdate(MonitorPlayerStatus status);
  
      /**
  
       * 播放错误回调
       * @param error
         */
          void onReceiveError(uSDKError error);
  
      /**
  
       * 音视频回放时间戳
       * @param time 单位毫秒
         */
          void onTime(long time);
  
      /**
  
       * 回放文件播放结束
       * @param time 单位毫秒
         */
          void onPlayFileFinished(long time);
         }
  
  3. 新增回调接口
     public interface MonitorPlaybackListener<T> {
  
      /**
  
       * 获取视频列表开始
         */
          void onStart();
  
      /**
  
       * 获取视频列表成功
       * @param t
         */
          void onSuccess(T t);
  
      /**
  
       * 获取视频列表失败
       * @param sdkError
         */
          void onError(uSDKError sdkError);
         }
  
  4. 播放节点信息类
     public class MessagePlaybackNode {
  
      public long startTime;
      public long endTime;
      public String recordType = "";
  
      public long getStartTime() {
          return startTime;
      }
  
      public void setStartTime(long startTime) {
          this.startTime = startTime;
      }
  
      public long getEndTime() {
          return endTime;
      }
  
      public void setEndTime(long endTime) {
          this.endTime = endTime;
      }
  
      public String getRecordType() {
          return recordType;
      }
  
      public void setRecordType(String recordType) {
          this.recordType = recordType;
      }
  
      public long getDuration(){
          return endTime - startTime;
      }
  
      @Override
      public String toString() {
          return "MessagePlaybackNode{" +
                  "startTime=" + startTime +
                  ", endTime=" + endTime +
                  ", recordType='" + recordType + '\'' +
                  '}';
      }
     }
  
  5. 获取音视频云播放列表类
     public interface MonitorCloudVideoListener<T> {
  
      /**
  
       * 获取云端视频开始
         */
          void onStart();
  
      /**
  
       * 获取云端视频成功
       * @param t
         */
          void onSuccess(T t);
  
      /**
  
       * 获取云端视频失败
       * @param sdkError
         */
          void onFail(uSDKError sdkError);
         }
  
  6. 图片类型枚举
     public enum PlayerImageType {
  
      /**
  
       * 报警图片
         */
  
      IMAGE_TYPE_ALARM,
  
      /**
  
       * 事件图片
         */
          IMAGE_TYPE_EVENT,
  
      /**
  
       * 普通图片
         */
          IMAGE_TYPE_NORAM,
         }
  
  7. ErrorConst类中新增枚举
     /**
  
   * 视频功能内部错误
     */
     ERR_USER_MONITOR_FUNCTION_INTERNAL(-20001, "视频功能内部错误"),
  
  /**
  
   * 无本地回放视频
     */
     ERR_USER_NO_LOCAL_PLAYBACK(-20002, "无本地回放视频"),
  
  /**
  
   * 视频分辨率已改变
     */
     ERR_USER_MONITOR_RESOLUTION_CHANGED(-20003, "视频分辨率已改变"),
  
  /**
  
   * 超过设备可支持的最大P2P通道数
     */
     ERR_USER_MAXIMUM_CHANNEL_EXCEEDED(-20004, "超过设备可支持的最大P2P通道数"),
  
  /**
  
   * P2P通道消息发送失败
     */
     ERR_USER_CHANNEL_MESSAGE_SEND_FAILED(-20005, "P2P通道消息发送失败"),
  
  /**
  
   * P2P通道消息发送超时
     */
     ERR_USER_CHANNEL_MESSAGE_SEND_TIMEOUT(-20006, "P2P通道消息发送超时"),
  
  /**
  
   * 设备正在录制
     */
     ERR_USER_DEVICE_RECORD(-20007, "设备正在录制"),
  
  /**
  
   * APP端通道连接数已达上限
     */
     ERR_USER_CHANNEL_NUMBER_FULL(-20008, "APP端通道连接数已达上限"),
  
  /**
  
   * 获取数据失败
     */
     ERR_USER_FAIL_GET_DATA(-20009, "获取数据失败");
  
  8. uSDKDevice.java接口变更
     /**
  
  * 订阅资源
  * 需要在{@link #setDeviceListener(IuSDKDeviceListener)}时传入接口{@link IuSDKDeviceListenerWithResource}的实现
    *
  * @param resName  资源名称
  * @param callback 回调接口
  * @since v8.5.0
    */
    public void subscribeResourceWithDecode(String resName, ICallback<Void> callback)
  ​```
  9. ```
     DeviceListener.java接口变更
     public void onReceiveDecodeResource(uSDKDevice device, String resource, String data)
  ```


  ```
  
  ##### 内部优化
  
  ota升级优化
  ```

- **Android uSDK_8.4.0**     

版本号： v8.4.0    
发布日期：2021.03.23    
MD5值：86F55528A8C6829669DCD4E00B196CF3    
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.4.0_Phone_Android_20210323100244_20210330132915768.zip)     
更新日志：    
**新增接口组相关接口变更**      

1. 新增回调接口         


    /**
     *进度回调接口    
     *@param <PT> 参数化类型之入参类型
     *@param <CT> 参数化类型之结果类型
     *@since v8.5.0   
     */
    public interface IProgressCallback<PT, CT>{
     
    /**
     *当处理进度
     *
     *@param pt正在处理的对象
     *@param error 错误码
    */
    @Keep
    void onProgress(PT pt, uSDKError error);
     
    /**
     *回调完成
     *
     *@param ct处理的结果对象
     *@param error 错误码
     */
    @Keep
    void onComplete(CT ct, uSDKError error);
    }

 






2. uSDKDevice.java接口变更    


    /**
     *获取可与当前设备分到同一组的设备列表, 当前设备要求有ZigBee能力或BLEMesh能力
     *
     *@param callback 接口执行完成时回调; 失败时，回调具体错误码; 接口执行成功时，devices也可能为空，表示接口执行成功，但没有可分组设备
     *@since v8.5.0
     */
    public void fetchGroupableDeviceList(ICallback<List<uSDKDevice>> callback);
     
    /**
     * 创建分组，返回组设备对象,
     * 创建完成后需要主动调用 {@link #addDevicesToGroup(List, int, IProgressCallback)}添加设备
     *
     * @param timeout  超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
     * @param callback 创建分组， 失败时，回调具体错误码; 接口执行成功时，回调创建好的组设备
     * @since v8.5.0
     */
    public void createGroup(int timeout, ICallback<uSDKDevice> callback);


​     
​    /**
​     * 向组设备中添加设备，要求当前device对象为组设备
​     *
​     * @param deviceList 需要添加到组设备的设备列表
​     * @param timeout超时时间，取值范围30-180秒
​     * @param callback   结果、进度回调（包括每个设备的添加情况，或未能添加的错误原因）
​     * @since v8.5.0
​     */
​    public void addDevicesToGroup(List<uSDKDevice> deviceList, int timeout, IProgressCallback<uSDKDevice, Void> callback);


​     
​    /**
​     * 从组设备中移除设备，要求当前device对象为组设备
​     *
​     * @param deviceList 待移除的mesh设备列表
​     * @param timeout超时时间
​     * @param callback   结果、进度回调（包括每个设备的移除情况，或未能移除的错误原因）
​     * @since v8.5.0
​     */
​    public void removeDevicesFromGroup(List<uSDKDevice> deviceList, int timeout, IProgressCallback<uSDKDevice, Void> callback);
​     
​    /**
​     * 删除组设备，要求当前device对象为组设备
​     *
​     * @param callback 删除结果回调
​     */
​    public void deleteGroup(ICallback<Void> callback);


**内部优化**     

uSDK Client：     
1.新增zigbee组设备支持；   
2.H2面板增加了zigbee连接方式的支持；   
3.网络异常的情况下，应用分类列表使用本地缓存；   
4.本机设备不依赖绑定   
5.连接中的状态同步调整；   
6.修复蓝牙OTA升级最后一包无法发现设备的问题；     


组件：    
1.新增组控设备的支持    
2.新增小夜灯的支持    
3.需求11000, usdk连云失败，门锁设备制造报警，app端报警信息无变化    
4.针对于重复订阅错误码优化，23003转23013    

CAE    
1.设置COAP透传通道最大数据长度为1024   
2.蓝牙配置，除了获取type    


----------
- **Android uSDK_8.3.0**      

版本号： v8.3.0   
发布日期：2021.03.03    
MD5值：7A466ACF6E977807AC1971DFF6D64AAA   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.3.0_Phone_Android_20210303124515_20210315163741095.zip)     
更新日志：     
**新增接口**    
新增NFC标签功能接口：    
1.新增类     
NFCUtil – NFC 标签解析、更新工具类   
NFCInfo – NFC bean 对象    
ICallback – 请求的回调     

2.NFCUtil.java接口说明     

    /**
     * 解析NFC标签数据，异步
     *
     * @param ndefRecord NFC标签原始数据，例如：
     *   zj.haier.net?untype=original&nsn=FFFF00000000&mac=C00000000000&model=CEAAJXX00&hwp=A0VF&s=abcdef
     *
     * @param completed  接口回调 成功返回 NFCInfo 类对象， 错误返回 uSDKError
     *   {@link NFCInfo}
     *   {@link ICallback#onSuccess(Object)}
     *   {@link ICallback#onFailure(uSDKError)}
     */
    public static void parseNFCTagData(String ndefRecord, ICallback<NFCInfo> completed);
    /**
     * 更新 NFC设备信息，异步，需在 NFCInfo 中设置 deviceID。
     *
     * @param info  NFCInfo对象， deviceId, nfcSerialNumber, mac 和 productCode 为必填项
     *  {@link NFCInfo#setDeviceID(String)}
     *  {@link NFCInfo#setNFCSerialNumber(String)}
     *  {@link NFCInfo#setMAC(String)}
     *  {@link NFCInfo#setProductCode(String)}
     *
     * @param completed 接口回调 成功返回 Void，失败返回 uSDKError
     *  {@link ICallback#onSuccess(Object)}
     *  {@link ICallback#onFailure(uSDKError)}
     *
     */
    public static void updateNFCDeviceInfo(NFCInfo info, ICallback<Void> completed);


3.NFCInfo.java字段列表    

| **名称**                | **类型** | **是否必须** | **说明**       |
| ----------------------- | -------- | ------------ | -------------- |
| deviceID                | String   | 必须         | 设备ID         |
| hwProductID             | String   | 可选         | 华为 productId |
| MAC                     | String   | 必须         | Mac地址        |
| NFCSerialNumber  String | String   | 必须         | 序列号         |
| productCode             | String   | 必须         | 产品编码       |

**内部优化**     
uSDK:    
1.ClientId埋点；    
2.Smartlink配置失败返601时进行重试    
3.修复蓝牙门锁离线问题；    
4.移除行为统计功能代码；    
5.修复蓝牙OTA升级过程中同一账号下其他账号进度重复的bug。     

组件：   
1.在门锁通过手机进行LocalKey更新或者时间同步时，用户可以优先订阅该门锁，建立连接。    
2.修改的“蓝牙门锁升级时崩溃”问题。      

CAE：    
1.解决蓝牙搜索内存泄露的问题；    
2.ble ota超时时间和hal层超时时间（10秒）对齐，从2秒改成11秒;   
3.增加蓝牙beacon v0的数据解析，以便支持云芯二代配置失败，可以通过蓝牙beacon上报配置失败原因。   


----------

- **Android uSDK_8.2.0**    

版本号： v8.2.0     
发布日期：2021.02.04     
MD5值：C92F76A66F7D4AC3C507A956E4198E6A   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.2.0_Phone_Android_20210205165559298.zip)    

注意事项1：不支持海外环境。     
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用。    
注意事项3：JDK版本最低版本要求：大于等于JDK8。   
Android Studio工程可以参考如下配置：     


    android {
    defaultConfig {
    //必须高于等于16
    minSdkVersion 16
    }  
    compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
    }
​    } 

更新日志：   
1.新增功能   
增加设备事件通知    
增加uSDKDeviceEvent实体类,包装收到的上报的设备事件，内部包含事件名称，事件类型和附带的属性信息    

    public class uSDKDeviceEvent {
    public String getName() {
    }
       
    public String getType() {
    }
       
    public List<DeviceAttribute> getAttrs() {
    }
     
    }
以上事件通过IuSDKDeviceListener接口上报。事件接口为Java8的默认实现，需要关注事件的可重写该接口    

    public interface IuSDKDeviceListener {
      void onDeviceEvent(uSDKDevice device, List<uSDKDeviceEvent> events);
      void onDeviceAlarm(uSDKDevice device, List<uSDKDeviceAlarm> alarms);
      void onDeviceAttributeChange(uSDKDevice device, List<uSDKDeviceAttribute> attrs);   
    }

2.接口变更   
 无    
3.内部优化及BUG修改   
3.1 支持蓝牙门锁历史数据获取   
3.2 支持蓝牙门锁OTA   
3.3 支持蓝牙门锁的绑定    



----------

- **Android uSDK_8.1.1**   

版本号： v8.1.1   
发布日期：2021.1.8   
MD5值：25E61F109E9C717722D051591E8695DA   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.1_Phone_Android_20210114160830269.zip)   
注意事项1：不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用     

更新日志：   
1.新增功能   
 无   
2.接口变更   
  无   
3.内部优化及BUG修改   
3.1 该版本修复蓝牙体脂秤数据更新没有给app上报的问题。    



----------

- **Android uSDK_8.1.0**   

版本号： v8.1.0  
发布日期：2020.12.23   
MD5值：0E81AEEADB547D43BE4424F140E8168A   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.0_Phone_Android_20201228102147320.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用   
更新日志：   
1.新增功能   
1.1 蓝牙广播设备配置绑定埋点；    
1.2 新直连绑定埋点；    
1.3 增加smartLink转SoftAp配置绑定的接口    
1.3.1 Binding类中增加接口bindDeviceBySmartLinkAuto接口    

    /**
     * SmartLink配置绑定接口，将指定的设备接入指定的WiFi，
     * 并将设备绑定到云平台
     * 调用该接口前，需要成功调用uSDKDeviceManager中的connectToCloud接口
     *
     * 对比于{@link Binding#bindDeviceBySmartLink(SmartLinkBindInfo, IBindResultCallback)}
     * 本接口会在内部计算是否满足转softAp的条件，如果满足，自动执行SoftAp配置绑定
     *
     * SmartLink绑定依次上报以下两个状态
     * <ol>
     *  <li>发送配置信息:{@link BindProgress#SEND_CONFIG_INFO}</li>
     *  <li>设备绑定:{@link BindProgress#BIND_DEVICE}</li>
     * </ol>
     * <p>
     *
     * 如果满足转SoftAp，绑定依次上报以下三个状态
     * <ol>
     *  <li>连接设备:{@link BindProgress#CONNECT_DEVICE}</li>
     *  <li>发送配置信息:{@link BindProgress#SEND_CONFIG_INFO}</li>
     *  <li>设备绑定:{@link BindProgress#BIND_DEVICE}</li>
     * </ol>
     * <p>
     *
     * @param bindInfo 配置信息
     * @param cb   绑定结果回调接口
     */
    @Keep
    public void bindDeviceBySmartLinkAuto(SmartLinkBindInfo bindInfo, IAutoBindCallback<uSDKDevice> cb)

1.3.2  SmartLinkBindInfo#Builder增加应用分类和成品编码的写入接口   

 		/**
 	     * 设置应用分类
 	     * @param appTypeCode
 	     */
 	    public Builder appTypeCode(String appTypeCode)
 	
 	    /**
 	     * 设置成品编码
 	     * @param productCode
 	     */
 	    public Builder productCode(String productCode)

1.3.3 增加IAutoBindCallback接口类    

    /**
    IBindCallback继承自ISoftApResultCallback，并且未增加接口
    */
    public interface IAutoBindCallback<R> extends IBindCallback<R> {
    /**
     * uSDK内部自动连接模块热点失败，请求APP协助热点切换
     * @param softApSsid
     */
    void switchToSoftApRequest(String softApSsid);
    }

1.4 增加子机配置绑定中RISCO设备配置绑定接口    

 SlaveDeviceBindInfo#Builder增加自定义扩展参数的接口   

    /**
     * RISCO设备绑定，自定义扩展参数
     * @param extendInfo
     * @return
     */
    public Builder extendInfo(String extendInfo)

1.5 uSDK支持主东服务链路跟踪埋点    
1.5.1 Trace类增加带traceId的构建Trace对象的接口    

    /**   
     * 根据businessId和自定义traceId创建一个新的跟踪链对象,
     * 如果传入的businessId重复,则会将之前创建的对象更新为一个全新的链式跟踪
     *
     * @param businessId 业务Id
     * @param traceId 长队为32位的字串
     * @return 跟踪链对象
     */
    public static Trace createTrace(String traceId, String businessId) 

1.5.2 Trace类增加带traceId的addDITraceNode接口   

     /**
     * 添加DI跟踪节点(DI) <br/>
     * <p>
     * 适配CAE打点，如果customTraceId为空，则使用uTrace内置traceId打点，不为空,则使用传入的traceId作为id标识
     *
     * @param customTraceId 自定义traceId
     * @param node          DI跟踪节点对象
     * @return 发送的结果
     */
    public int addDITraceNode(String customTraceId, DITraceNode node) 

2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 uSDK - Android手机配网策略默认取值ble - 配置绑定   
3.2 融合搜索优化 & 问题修改；   
3.3 配网策略默认取值ble；    


----------


- **Android uSDK_8.0.0**

版本号： v8.0.0   
发布日期：2020.12.07   
MD5值：78DF1B23D362F42755F1AE90DAC67018   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.0.0_Phone_Android_20201211175355894.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    
更新日志：   
1.新增功能   
1.1 新增支持蓝牙体脂秤，绑定和属性上报    
1.1.1. ConfigType新增`BLE_ADV`枚举，代表新支持的蓝牙广播设备    
1.1.2. Binding内部新增void bindBLEAdvDevice接口   
1.1.3. 增加BLEAdvBindInfo实体类，传递广播设备发现信息   

     Builder 设置搜索上来的可发现设备，setConfigurableDevice(ConfigurableDevice)   

1.1.4. 属性上报通过原有的设备属性上报通知给App   
1.2. 增加P2P音视频能力设备接入   
1.2.1 增加`VideoView`控件承载视频播放View   
1.2.2 增加`MonitorPlayer`来持有媒体播放的控制功能   
1.3 增加`IMonitorPlayerListener`播放监听器   
     //播放器状态回调
     void onPlayerStatusUpdate(MonitorPlayerStatus status);
     
     // 播放错误回调
     void onReceiveError(uSDKError error);    
1.4 新增`MonitorPlayerStatus`枚举   
1.5 新增获取路由器信息    
1.5.1 新增实体类 （ConfigRouterInfo.java）   

       /**
      * 获取ssid   
      *
      * @return ssid of wifi   
      */
      public String getSsid();   
      /**
      * 获取路由器的bssid   
      *
      * @return bssid of wifi   
      */
      public String getBssid();   
      /**
      * 获取路由器的密码   
      *
      * @return password of wifi   
      */
      public String getPassword();   
      /**
      * 是否切换了网络 
      *
      * @return true: 切网，false: 正常未切网   
      */
      public boolean isNeedSwitchNetwork();   

1.5.2 2. Binding类中新增获取路由器信息接口     

        /**
       * 获取路由器信息
       *
       * @param timeout  超时时间
       * @param callback 获取路由器信息回调接口
       */
    public void getConfigRouterInfo(int timeout, ICallback<uSDKConfigRouterInfo> callback);

1.6 设备搜索新增接口   
1.6.1 新增搜索特性枚举    

       public enum SearchState {
       //启用SoftAp热点搜索
       WIFI_ENABLE(1),  
       //启用蓝牙搜索
       BLE_ENABLE(1 << 1),
       //已入网代理搜索
       PROXY_ENABLE(1 << 2),   
       //新直连搜索
       NEW_DIRECT_LINK_ENABLE(1 << 3);
       }   
1.6.2 新增特性控制接口(DeviceScanner.java)    

        /**
     * 使能搜索特性
     * 示例代码
     * <pre>{@code
     *  // 启用新直连搜索
     *  // 启用Wifi搜索
     *  // 启用已入网代理设备搜索
     *  // 启用蓝牙搜索
     *  int features = SearchState.WIFI_ENABLE.mask
     * | SearchState.NEW_DIRECT_LINK_ENABLE.mask
     * | SearchState.PROXY_ENABLE.mask
     * | SearchState.BLE_ENABLE.mask;
     *
     * enableSearchFeature(features);
     * }</pre>
     *
     * @apiNote 目前仅支持对SoftAp搜索使能控制，其他暂无法控制，默认是开启的
     * @param features 搜索特性
     * @since v8.0.0
     */
    public void enableSearchFeature(int features);

1.7  新增权限相关接口   
1.7.1 新增扫描权限监听接口    

      public interface ScannerListener {
      // 当权限不合法时
       //@param permission uSDK 需要的系统权限枚举
      void onPermissionInvalid(Permission permission);
      }   

1.7.2 新增权限控制枚举    

      public enum Permission {
      //蓝牙未打开
      BLE_DISABLE(),
      // 需要蓝牙相关权限
      BLE_INVALID(Manifest.permission.ACCESS_FINE_LOCATION),
      //wifi没有打开
      WIFI_DISABLE(),
      /**  
       * 需要Wifi相关权限
       * Android 9：
       * 成功调用 WifiManager.startScan() 需要满足以下所有条件：
       *
       * 1. 应用拥有 ACCESS_FINE_LOCATION 或 ACCESS_COARSE_LOCATION 权限。
       * 2. 应用拥有 CHANGE_WIFI_STATE 权限。
       * 3. 设备已启用位置信息服务（位于设置 > 位置信息下）。
       * Android 10（API 级别 29）及更高版本：
       * 成功调用 WifiManager.startScan() 需要满足以下所有条件：
       *
       * 如果您的应用以 Android 10（API 级别 29）SDK 或更高版本为目标平台，应用需要拥有 ACCESS_FINE_LOCATION 权限。
       * 如果您的应用以低于 Android 10（API 级别 29）的 SDK 为目标平台，应用需要拥有 ACCESS_COARSE_LOCATION 或 ACCESS_FINE_LOCATION 权限。
       * 应用拥有 CHANGE_WIFI_STATE 权限。
       * 设备已启用位置信息服务（位于设置 > 位置信息下）。
       * 若要成功调用 WifiManager.getScanResults()，请确保满足以下所有条件：
       *
       * 如果您的应用以 Android 10（API 级别 29）SDK 或更高版本为目标平台，应用需要拥有 ACCESS_FINE_LOCATION 权限。
       * 如果您的应用以低于 Android 10（API 级别 29）的 SDK 为目标平台，应用需要拥有 ACCESS_COARSE_LOCATION 或 ACCESS_FINE_LOCATION 权限。
       * 应用拥有 ACCESS_WIFI_STATE 权限。
       * 设备已启用位置信息服务（位于设置 > 位置信息下）。
       */
      WIFI_INVALID(Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION);
      
      private String[] permissions;
      
      Permission(String... perms) {
      permissions = perms;
      }
      
      @Keep
      @Nullable
      public String[] getSystemPermissions() {
      return permissions;
      }
      }

1.7.3 新增设置权限回调接口（ DeviceScanner.java）     
      `public void setScannerListener(ScannerListener listener);`

1.8 新增错误码   
1.8.1. `-16021`: 不支持获取配置的路由器信息     
1.8.2. `-16022`: 获取路由器信息失败   
1.8.3. `-16023`: 获取路由器信息超时   

2.接口变更   
 无   
3.内部优化及BUG修改     
3.1. 新增热点搜索&已入网设备代理搜索功能，以及都说搜索功能融合上报   
3.2. 新增uSDK从云端获取Wifi&Ble模块的配网策略   
3.3. 不支持bindCode的自绑定设备查询绑定结果接口变更；   
3.4. uSDK线程池优化，改为"4+4"模式;   
3.5. 其他若干代码优化以及Demo优化等   
3.6. 新增BindCode埋点字段   





----------

- **Android uSDK_6.2.1**   

版本号： v6.2.1   
发布日期：2020.10.14  
MD5值：72EE689C195650C7FB54C156E6B2B845   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.2.1_Phone_Android_20201023102752356.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用   
更新日志：   
1.新增功能   
 无   
2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 修改了获取蓝牙历史数据崩溃的问题   
3.2 修改了Android 11 系统下 偶发崩溃的问题   



----------

- **Android uSDK_6.2.0**   


版本号： v6.2.0   
发布日期：2020.09.24   
MD5值：EB63AA7EB3639B710C231A0FE0CE30D3   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.2.0_Phone_Android_20200924135413217.zip)    
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用    
更新日志：  
1.新增功能    
 无     
2.接口变更      
 无    
3.内部优化及BUG修改   
3.1 SmartLink 配置仅配置接口不依赖本地上线包，收到ack后就认为配置成功   
3.2 SmartLink 配置绑定接口，不依赖本地上线，配置包发送完成后开始查询绑定结果。收到ack后，停止查询绑定结果，进行用户侧绑定。查询绑定结果和用户侧绑定均使用,从云平台获取到的bindCode。   
3.3 所有的绑定和查询绑定结果使用的bindCode类型变为无符号4字节整数。增加bindCodeFrom参数，1-云平台生成，0-sdk生成。绑定接口，如果组件返回190005认为绑定成功。   
3.4 重试绑定接口增加SR和SS埋点   
3.5 SmartLink cs/cr点携带平台返回的traceIdB，用户打通usdk和模块的链路日志。   
3.6 停止小循环对wifi模块的升级功能   



----------


- **Android uSDK_6.1.1**    

版本号： v6.1.1   
发布日期：2020.09.15  
MD5值：DBDE4625AB59881CA4DB3531826C8347  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.1_Phone_Android_20200924135317509.zip)  
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用  
更新日志：  
1.新增功能  
 无    
2.接口变更  
 无     
3.内部优化及BUG修改  
3.1 修复SoftAp配置过程NPE问题  
3.2 修复设备测绑定重复查询绑定结果问题  
3.3 修复了组件导致uSDK崩溃的问题  
3.4 修复SoftAp设备BindCode能力判断错误问题  



----------


- **Android uSDK_6.1.0**    

版本号： v6.1.0  
 发布日期：2020.09.04  
 MD5值：4A5C53F82B11C5F1DEF9EDE5C5D1614  
 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.0_Phone_Android_20200907155553790.zip)  
 注意事项1：此版本不支持海外环境。  
 注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用  
 更新日志：  

1.新增功能    
1.1.  查询设备网络信号质量(见`uSDKDevice`)   

    public void getNetworkQualityV2(final ICallback<uSDKNetworkQualityInfoV2> callback);

1.2 新增网络质量信息类（见 `uSDKNetworkQualityInfoV2`）   

    public uSDKDeviceConnectStatus getDeviceConnectStatus()//获取设备的链接状态
    public String getMachineId()//设备机器编号
    public boolean isOnLine()//获取设备是否远程在线
    public long getStatusLastChangeTime()//获取设备最后一次状态变化时间，格林威治时间
    public String getNetType()//获取设备的网络类型,例如 "Wifi"
    public String getSsid()//获取设备所连接的路由器名称
    public int getRssi()//获取设备所连接的路由器网络信号强度
    public int getPrssi()//获取设备所连接的路由器网络信号强度百分比
    public int getSignalLevel()//获取设备所连接的路由器网络信号质量等级,取值: 0 未知,1 优,2 良,3 合格,4 差
    public int getIlostRatio()//获取设备所连接的路由器 广域网丢包率
    public int getIts()//获取设备所连接的路由器 广域网延时
    public String getLanIP()//获取设备所连接的路由器的内网IP
    public String getModuleVersion()//获取设备的模块版本描述,版本格式; 软件版本号/软件类型/硬件版本号/硬件类型

1.3 新增 `uSDKDeviceConnectStatus` 枚举   

        CLOUD_CONNECTED("远程连接")
        LOCAL_CONNECTED("本地连接")
        LOCAL_BLE_CONNECTED("蓝牙连接")
        OFFLINE("离线")

1.4 新增获取故障信息的接口（见`uSDKDevice`）   

    public uSDKFaultInformation getSDKFaultInformation()

1.5 新增设备故障信息类（见uSDKFaultInformation）   

    public int getStateCode()
    public int getState()

1.6 新增故障信息回调方法（见`DeviceListener`）   


    public void onUpdateFaultInformation(uSDKFaultInformation faultInformation)



1.7 新增通过蓝牙修改设备侧SSID&PWD的接口（见uSDKDevice）   

    public void updateRouterSSID(String ssid, String password, String bSsid, int timeout, IuSDKUpdateRouterSSIDCallBack updateRouterSSIDCallBack)  


2.接口变更   
 无  
3 内部优化   

3.1. HTTPDNS缓存机制优化, 本地进行DNS解析时，结果不进入缓存   
3.2. 配置绑定优化：SoftAP、BLE设备发起绑定带bindCode    
3.3. 离线分析埋点，增加云连接状态，设备连接状态，网络状态，前后台事件的埋点   
3.4. 基础连接优化：uSDK网络切换处理机制优化，在任何切网时都重启本地搜索，重启云连接，而不是根据网络状态进行连接或断开   
3.5. 配置绑定优化：softAp配网过程中同时监听BLE事件广播  
3.6. 绑定HTTP超时时，内部进行重试，指导的成功或者超时  
3.7. 设备测绑定，解决绑定成功后设备离线的问题（result结果处理）   
3.8. 三不可（不可配/不可控/不可触发进配置）设备添加到搜索发现列表   



----------


- **Android uSDK_6.0.2**  

版本号：v6.0.2  
发布日期：2020.08.03  
MD5值：A62F7795D722829BDC3B843DED346510  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.1_Phone_Android_20200727112651916.zip)   
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用   
更新日志：   
1.新增功能   
 无  
2.接口变更  
 无  
3.内部优化及BUG修改  
3.1 组件修改gcc编译方式为NDK  
3.2 删除对arm64-v8a的CPU架构支持，APP可使用armeabi-v7a或armeabi方式在该CPU架构手机上运行uSDK。  

----------

- **Android uSDK_6.0.1**  

版本号：v6.0.1  
发布日期：2020.07.27  
MD5值：DFCC151551747FFC17CACAA646A5025C  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.1_Phone_Android_20200727112651916.zip)  
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用  
更新日志：   
1.新增功能  
 无  
2.接口变更   
 无  
3.内部优化及BUG修改  
3.1修改32位系统资源通道内存地址由无符号整型转为有符号整型（jlong）时造成内容意义改变的问题  
3.2修改和组件的接口，long型修改为json的形式，将错误码和指针地址物理区分，取消原理负值为错误码，正值为指针的设计。  
3.3优化网络不可达时断开本地和远程连接的逻辑，优化频繁刷新连接状态时导致的ANR。  

----------

- **Android uSDK_6.0.0**

版本号：v6.0.0  
发布日期：2020.07.13  
MD5值：A54DB129EAFF543301836C2D4E3C590B  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.0_Phone_Android_20200713111734073.zip)  
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用   

更新日志：  


1.新增功能  
1.1  启动待配置状态的新直连设备搜索  
`DeviceScanner.startScanConfigurableDevice(final IuSDKCallback callback);`

1.2 验证码方式绑定新直连设备  
1.2.1 获取新直连绑定验证码方式绑定信息（见NewDirectLinkVerificationCodeBindInfo）   

    NewDirectLinkVerificationCodeBindInfo bindInfo = new NewDirectLinkVerificationCodeBindInfo.Builder()   
    .setConfigurableDevice(currentDeviceInfo)// 传入scanner上报的可配置设备  
    .setVerificationCode(code) // 传入验证码  
    .csNode(csNode)  
    .timeout(timeout)  
    .build();  

1.2.2 验证码方式绑定新直连设备(见uSDKBinding)   

       /** 
     * 验证码方式新直连设备绑定
     * @param bindInfo 绑定信息，包含待配置设备信息和验证码
     * @param cb 绑定状态及结果回调
     */
    public void bindNewDirectLinkDevice(NewDirectLinkVerificationCodeBindInfo bindInfo, IBindResultCallback<uSDKDevice> cb)

1.3 手动确认校验方式绑定新直连设备   
1.3.1 获取新直连绑定验证码方式绑定信息（见uSDKNewDirectLinkManualConfirmBindInfo）  

       NewDirectLinkManualConfirmBindInfo bindInfo = new NewDirectLinkManualConfirmBindInfo.Builder()
    .setConfigurableDevice(configurableDevice) // 传入scanner上报的可配置设备
    .csNode(csNode)
    .timeout(timeout)
    .build();

1.3.2 手动确认校验方式绑定新直连设备（见uSDKbinding）  

    /**  
     * 手动方式（按键）新直连设备绑定
     * @param bindInfo 包含待配置设备信息
     * @param cb 绑定状态及结果回调
     */
    public void bindNewDirectLinkDevice(NewDirectLinkManualConfirmBindInfo bindInfo, IBindResultCallback<uSDKDevice> cb)

1.4 自发现蓝牙设备可发现已配置的蓝牙设备   

1.4.1 增加ConfigStatus枚举类定义   

    public enum ConfigStatus {
    CONFIG_ABLE("可配置"),
    TRIGGER_CONFIG_ABLE("触发可配置"),
    ALREADY_CONFIGURED("已经配置");
    }

1.4.2  ConfigurableDevice增加接口，获取可配置设备的配置状态   

    //获取配置状态
    public ConfigStatus getConfigStatus() {
    }

1.5 uSDK启动项里增加开启蓝牙搜索配置(见uSDKStartOptions)   
uSDKStartOptions.Builder增加方法，设置是否默认开启蓝牙可控制设备搜索   

    /**
     * 设置是否默认开启蓝牙可控制设备搜索，该值默认为true
     * @param isBleSearchControllableDevice
     * @return
     */
    
    public Builder isBleSearchControllableDevice(boolean isBleSearchControllableDevice) {
    this.isBleSearchControllableDevice = isBleSearchControllableDevice;
    return this;
    }

1.6 大循环下获取设备的模块信息(见uSDKDevice)   

    /**
     * 获取设备模块信息
     * @param timeout 执行命令超时时长，单位为秒.超时时长最小为5秒，最长为120秒，建议值15秒
     * @param callback 业务回调结果{@link ModuleInfo}对象
     */
    public void getModuleInfo(int timeout, ICallback<ModuleInfo> callback)

1.7 无效命令    

对支持无效命令的设备，进行操作`(read/write/op)`，触发无效命令时，会携带无效命令标识上报给`App`，无效命令标识的值放到`uSDKError`的`failureReason`对应的值中。   

1.7.1 read新增接口   

    //根据属性名读取设备的属性值，属性值会在回调函数中返回，并更新设备对应的属性值
      public void readAttribute(final String name, final ICallback<String> callback(){}
      public void readAttribute(final String name, final int timeout, final ICallback<String> callback)
      public void readAttribute(final String name, final int timeout, final Trace diTrace, final ICallback<String> callback) {}


1.7.2 write新增接口   

    //写入设备属性值,回调中只返回是否成功,如果写入成功，设备对应的属性值会在设备属性变化上报中更新(超时时间15s)
      public void writeAttribute(String name, String value, final ICallback<Void> callback) {}
      public void writeAttribute(String name, String value, int timeout, final ICallback<Void> callback)
      public void writeAttribute(final String name, final String value, final int timeout, final Trace diTrace, final ICallback<Void> callback)

1.7.3  op新增接口   

    //执行设备命令操作的方法.默认超时时长为15秒.每一种设备都有自己特定的命令集，详细的命令集描述请参看对应的设备ID文档
     public void execOperation(String operationName, List<uSDKArgument> args, final ICallback<Void> callback)
     public void execOperation(String operationName, List<uSDKArgument> args, int timeout, final ICallback<Void> callback)
      public void execOperation(String operationName, List<uSDKArgument> args, int timeout, final Trace diTrace, final ICallback<Void> callback)

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
    + (void)retryBindDeviceWithTimeoutInterval:(NSTimeInterval)timeoutInterval
       success:(void(^)(uSDKDevice *device))success
       failure:(void(^)(NSError *error))failure;


2.接口变更    

2.1配置绑定优化：无效参数（10001）问题的解决   

softap配网不再校验`iotDevBssid`, 而是校验`iotDevSSID`,修改SoftApBindInfo.Buidler, 如下：   

     /**
     * 设备 soft ap 热点的 bssid
     * @param bssid
     * @return
     * @deprecated 6.0.0
     */
    @Deprecated
    public Builder iotDevBssid(String bssid) {
    this.mIotDevBssid = NetUtil.correctBSSID(bssid);
    return this;
    
    }
    
    /**
     * 设备 soft ap 热点的 ssid
     * @param ssid
     * @return
     */
    public Builder iotDeviceSSID(String ssid) {
    this.mIotDeviceSSID = ssid;
    return this;
    }


3.内部优化及BUG修改   
3.1 uSDK就绪性能优化；   
3.2 纯蓝牙设备历史数据逻辑，uSDK收到BLE模块发送的空包导致的异常;   
3.3 uSDK清单文件增加权限要求`android.permission.BLUETOOTH`、`android.permission.BLUETOOTH_ADMIN`,解决APP未声明此权限时的崩溃问题;   

 

----------

- **Android uSDK_5.8.2**  

版本号：v5.8.2  
发布日期：2020.06.28  
MD5值：74713F56EB99EA522668E1E2FAAEA122    
注意事项：如需数据统计分析功能，请与统计分析SDK3.2.0版本搭配使用 
更新日志：  
1.新增功能  
 无   
2.接口变更    
 无    
3.内部优化及BUG修改   
3.1、ble业务service类型保护；  
3.2、远程登出接口代码健壮性微调整  

----------

- **Android uSDK_5.7.0**   

版本号：v5.7.0  
发布日期：2020.02.27   
MD5值：64198BB25BCCC188E4C1AD14C4D8A66D   
注意事项：如需数据统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：  
1.新增功能  
1.1 新增标记设备进入焦点（详情页）(见uSDKDevice类)  

    // 进入焦点后，退出焦点前，如果大循环控制超时，则提升蓝牙通道的优先级高于大循环且低于小循环通道  
    public boolean inFocus() 

1.2. 新增标记设备退出焦点（详情页）  
`public boolean outFocus()`

1.3 新增蓝牙配置时设备需要触发进配置的错误码（见uSDKErrorConst）  

    // 设备需要触发进配置
    ERR_USDK_DEVICE_NEED_TRIGGER_CONFIG = -13027
 2.接口变更  
 无   
3.内部优化及BUG修改  
 无   



[^-^]:常用图片注释
[pic1]:../App-dev/_media/pic1.png
[pic2]:../App-dev/_media/pic2.png
[pic3]:../App-dev/_media/pic3.png
[pic4]:../App-dev/_media/pic4.png
[pic5]:../App-dev/_media/pic5.png
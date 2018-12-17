!>  **当前版本**：[uSDK_Phone_Android V5.0]()  
 **更新时间**：{docsify-updated}

## 1 uSDK5.0简介
uSDK是一款移动应用开发套件，它能够支持目前主流的Android和iOS系统。它允许您运用Java、Object-C、Swift等创建App和智能硬件互动。
通过uSDK可以实现的功能包括智能设备配置入网、搜索、状态查询、控制、接收报警。

###1.1.	uSDK5.X变化与新增功能
<li>uSDK5.X与4.X API基本保持一致，精简了2.X历史API</li>
<li>修改了设备属性和报警的方法名</li>
<li>提升了高性能路由SmartLink配置成功率</li>


###uSDK4.x工程迁移到uSDK5.x
 修改获得设备属性和报警API。

## 2.实验开发环境搭建

### 2.1.	官网、QQ群
官网： 海极网 http://www.haigeek.com，注册账号成为硬、软件开发者，使用U+物联开放平台的各项服务及下载资料。官方QQ群：U+开发者技术讨论群457841032。


### 2.2.	路由器环境设置
目前海尔智能设备只能匹配2.4g频段路由，才能正常工作和使用，暂不支持5g频段。为了使APP开发者能够顺利进行开发，建议APP开发者仔细阅读此章节后对路由器进行如下设置。

#### 2.2.1.	路由无线网络基本设置
当前市场上路由品牌和手机品牌丰富多样，手机和路由联合工作时容易存在兼容性问题（如11bgn mixed模式），导致手机、路由、海尔智能设备不能正常协同工作（始终无法配置成功、始终不能发现U+智能设备或者配置成功率时高时低），这时可以参考如下截图对路由设置进行更改。
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
准备设备和软件
<li>1.一台2.4G无线路由器</li>
<li>2.一台Android4.0以上智能手机</li>
<li>3.一台支持U+协议的智能设备（安全版本智能设备）</li>
<li>4.硬件测试工具App或uSDK4XDemoApp</li>
路由器需要连接互联网，它负责接入U+设备，提供数据通路。Android手机主要承载基于uSDK的App程序。支持U+协议的智能设备可以是智能空调、洗衣机整机、空气盒子或者U+Dev开发板。

实验设备全家福

U+Dev开发板可以申请进入官方开发群说明您的开发背景，向群主申请。

相关资料：
配置设备入网演示视频：QQ群文件
uSDK等更多相关资料，见海极网下载中心。

### 2.4	搭建程序开发环境
####  2.4.2.导入库文件
配置开发者环境举例
配置完成后ping developer.haier.net，查看配置是否成功：

导入库文件
配置uSDK5.X aar到gradle配置文件里。
配置工程包名
建立应用工程时工程包名需要和U+开发者网站上申请应用填写的应用包名一致。例如：
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.partner.uplus.app"
    android:versionCode="2"
    android:versionName="2.1">

配置App校验信息
将U+开发者网站申请的APP_ID,APP_KEY,APP_SECRET配置到文件中，举例如下：
<application>
<meta-data 
  android:name="APP_ID"
  android:value="XX-XXXX-XXXX" />
<meta-data 
  android:name="SECRET_KEY"    
  android:value="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" />
<meta-data   
  android:name="APP_KEY"
  android:value="xxxxxxxxxxxxxxxxxxxxxxxxxxxxx" />
</application>

注意
APP_KEY分为“测试”和“生产”。在开发调试阶段用测试的APP_KEY，在“提交应用”和发布到商店时用生产的APP_KEY。
无论是开发者还是用户，在第一次启动APP时手机需要联网。

避免uSDK包被混淆
请添加如下配置避免uSDK二次混淆：-keep class com.haier.uhome.** {*;}。

uSDK权限配置
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

### 2.5.范例Demo程序
海极网提供一款示例demo 程序，演示uSDK配置设备入网及局域网与设备互动的情景。
uSDKDevDemo包含的功能
<table width="100%">
<tr>
	<th>功能</th><th>文件</th>
</tr>
<tr>
	<td width="50%">SmartLink配置设备入网</td>
	<td width="50%">MakeDevice2NetSmartLink.java</td>
</tr>
<tr>
	<td>SoftAP配置设备入网</td><td>MakeDevice2NetSoftAP.java</td>
</tr>
<tr>
	<td>搜索设备功能，并展示设备列表</td><td>MainActivity.java</td>
</tr>
<tr>
	<td>与设备建立连接，显示状态、报警</td><td>DeviceStatusActivity.java</td>
</tr>
<tr>
<td width="50%">设备控制</td>
<td width="50%">DeviceControlUDevKitDialog.java
DeviceControlElecHeaterDialog.java</td>
</tr>
<tr>
<td>ElecHeater_Dev_DOC_ID_111c120024000810060500418001840000000000000000000000000000000000.pdf
UDevKit_Dev_DOC_ID_110c10841050850814ee00000000000000000000000000000000000000000000.pdf 
</td>
<td>asset文件夹
电热水器ID开发文档
U+开发板ID开发文档</td>
</tr>
</table>

##  3.业务梳理及开发快速入门
搭建实验环境及App开发环境后，本章总结使用uSDK的两个常见业务流程：基本业务流程和配置设备入网流程。
### 3.1基本业务接入流程
本图简要介绍uSDK物联App主要业务运行流程，红色文字是uSDK交互，黑色文字是App与U+云交互：
![简单业务运行流程][devleop_step_simple]

注意：
启动uSDK、发现网络设备这两步与登录U+云平台、获取U+云账号设备列表可以互调位置，具体调用方法根据APP实际业务确定

###3.2配置入网流程
参考下列流程开发配置设备入网功能，特别说明：成功启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。详细介绍请参考“章节4.2配置设备入网”。
![配置业务运行流程][public_config_step]


## 4.uSDK业务开发详解
### 4.1.uSDK启动
启动uSDK是调用各种功能性API，使用U+物联功能的前提，在主线程中启动一次即可。下面分别讲解启动uSDK,及启动前后需进行的必要设置：设置设备管理、设置过滤设备类型、uSDK启动成功后设定日志级别、开启和关闭DNS防劫持功能等。

<h5>预备知识</h5>
请先了解章节2.3熟悉App开发环境的配置。

<h5>相关概念和术语</h5>
- AppID：在海极网为物联App申请的AppId，用于App校验
- AppKey：在海极网申请，用于与OPEN API交互时使用。APP开发中只有“开发测试” 的APP_KEY。开发完成且提交海极网审核完成后会发放“生产” 的APP_KEY，APP使用生产的APP_KEY进行发布。
- SecretKey：在海极网申请，uSDK使用

<h5>启动uSDK</h5>
首先执行初始化方法，然后执行具体启动方法：

	//推荐在Application对象实现中执行,
    //或者在所有uSDK操作之前，入参需要是ApplicationContext
    uSDKManager.init(ApplicationContext);
    //result为uSDKErrorConst.RET_USDK_OK时启动成功。
    uSDKManager.startSDK(new IuSDKCallback() {
    	@Override
    	public void onCallback(uSDKErrorConst result) {
    		if(result == uSDKErrorConst.RET_USDK_OK) {
    		//在此设置uSDK特性，例如：关闭httpDNS，或者设置为NONE
    		}
    	}
    });

<h5>注意</h5>
1. uSDK成功启动一次即可，不要多次、多个线程启动
2. uSDK成功启动之后再调用uSDK提供的各类方法
3. 启动入参Context必须是ApplicationContext

<h5>设定uSDK日志级别</h5>
uSDK默认输出所有日志，日志标签是uSDK。uSDK提供API支持App设定日志输出级别，uSDK启动成功后，执行即可：
`uSDKManager.initLog(uSDKLogLevelConst level, boolean isWriteToFile, IuSDKCallback)`

<h5>启用uSDK特定功能</h5>
1. uSDK启动前，设置特定功能：
- uSDKManager.SDK_FEATURE_TRACE开启链式跟踪，
- uSDKManager.SDK_FEATURE_SAFTE_DNS：开启httpDNS功能
- uSDKManager.SDK_FEATURE_DEFAULT：开启所有额外功能，uSDKManager.SDK_FEATURE_NONE：关闭所有额外功能。
2. 集成uSDK的App在生产以外环境以外测试或使用时，在uSDK启动成功回调里添加以下语句：
    
	int fea = uSDKManager.SDK_FEATURE_NONE; <br/>
    //根据需要开启需要特性，注意不开启uSDKManager. SDK_FEATURE_SAFTE_DNS
    fea |= uSDKManager.SDK_FEATURE_TRACE;
    uSDKManger.enableFeatures(fea);

App上线时，修改代码允许httpDNS特性，例如设置成：uSDKManager.SDK_FEATURE_DEFAULT允许全特性，或者根据实际需要设置。

##  4.3配置设备入网
配置设备入网就是使用uSDK将智能设备加入指定WIFI的一项操作。uSDK支持SmartLink、SoftAP两种方式入网。对于每种配置方式，本章将讲解：设备入网的操作过程、程序收集目标无线网络配置信息、普通或安全方式发送配置入网信息、获得配置入网设备实例、中断设备配置入网动作等，最后讲解自带屏幕或操作系统设备入网。
uSDK5.X支持您使用加密方式发送配置信息，如果您的智能产品包含不支持加密入网的型号，则使用普通配置方式，以保证所有销售产品都能正常入网。如果产品全部支持加密入网，则使用加密入网方式，提升整套产品安全性能。

<h5>预备知识</h5>
请先了解章节4.1启动uSDK。配置入网演示视频请至U+开发者QQ群下载。

相关术语和概念
- uPlug：为智能设备连接WIFI的集成电路板。

<h5>SmartLink入网方式</h5>

----------

U+平台SmartLink方式是成熟、稳定、快速的设备入网方式。运行时将无线配置信息发送给待入网设备，完成配置入网动作。

<h5>了解设备入网操作过程</h5>
1)	确认当前路由器频段是2.4G。按说明书指导，操作设备进入配置模式（一般是长按某按键或组合按键），智能设备面板WIFI信号灯闪烁（此时uPlug指示灯规律快闪），说明设备进入待入网模式。
2)	确认手机稳定连接路由器，设备安装位置WIFI信号良好，打开基于uSDK的App（我们要开发的产品），将配置入网信息发送出去。设备收到配置信息后连接路由器，手机在目标网络里等待设备出现。
3)	App显示有新设备出现。
4)	App端设备长时间（2分钟）不上线，说明操作失败，重新按照1，2，3步骤进行。

<h5>App收集配置入网信息</h5>
配置入网信息指的就是WIFI的SSID和密码，App编程获取WIFI名称并显示，密码由使用者输入。

<h5>App发送配置入网信息</h5>
App把无线的名字和密码发给待入网的设备：

    uSDKDeviceManager.configDeviceBySmartLink(apName, apPass, null, 100, false, 
		new IuSDKSmartLinkCallback() { 
    		public void onCallback(uSDKDevice deviceJustJoined , uSDKErrorConst result) {
    			if(uSDKErrorConst.RET_USDK_OK == result) {
    				System.out.println("Success: " + deviceJustJoined.getDevivceId());
    			} else {
    				System.out.println("配置设备入网失败了： " + 
						result .getErrorId() + " :" + result .getValue());
    			}
    });

uSDKDeviceManager.configDeviceBySmartLink：配置设备入网方法。
apName：无线网络名称，不能为空，apPass：无线网络密码，不能为空。
1) apName（即SSID）最大长度为31个字符，最小长度为1个字符，内容为可见英文字符
2) apPass（即路由密码）要求8至63个字符，内容为可见英文字符
3)两项均不支持中文
null：设备Id（设备MAC），null：不指定目标设备。
100：配置入网超时时间，单位是秒，推荐100s。
IuSDKSmartLinkCallback：回调接口。
true：仅能支持配置安全设备入网，false：同时支持安全设备和普通设备时使用，请根据产品实际情况配置本参数。

<h5>注意</h5>
1.	uSDKDeviceManager.configDeviceBySmartLink方法有多个可以指定MAC、超时时间、是否安全，开发功能时根据业务场景需要选择相应的实现。
2.	重载实现可以指定TypeId，被配置成功且TypeId是指定值的设备将被返回。
3.	重载实现可以指定uPlusId数组（TypeId数组）进行过滤，满足过滤条件且最先返回的设备将被认为是成功入网的设备返回给App。
4.	重载实现有配合uTrace的方法
5.	当配置动作动作尚未完成，再次执行配置API时，将返回配置进行中的信息。
6.	-901错误码：1.您可以提示使用者重新进行设备配置入网操作；2.如果此时您能得知设备的MAC值（例如：二维码或一维码），可以调用API查看设备是否存在（章节4.12），如果能找到，也可以确认为配置入网成功。 

<h5>判断设备入网是否成功</h5>
result==uSDKErrorConst.RET_USDK_OK时，设备配置入网成功。配置失败时，result携带配置错误信息。

<h5>获得配置入网设备实例</h5>
deviceJustJoined不为null时，它就是入网设备对象实例，此时它携带设备网络及产品类型等基本静态信息。

<h5>中断设备配置入网</h5>
在执行配置设备入网过程中可以调用API中断，中断动作结果将通过回调参数通知App，中断结果返回后，才能启动下一次配置动作。
    uSDKDeviceManager.stopSmartLinkConfig(new IuSDKCallback() {
    	@Override
    	public void onCallback(uSDKErrorConst result) {
    		System.out.println("取消配置结果：" + result);
    	}
    );

<h5>发现待入网智能设备&极路由发现待入网设备</h5>
----------
安装有第三代uPlug的智能设备第一次接通电源或进入配置模式等待入网时，uSDK3.X-uSDK4.4.01能够发现它们，我们可以提示使用者，或者引导用户把设备配置入网。
当您使用uSDK4.2.02及以上，并连接极路由时，可以接收极路由发现的待入网设备。从uSDK4.4.02开始在普通路由（这里指非极路由），不能发现待入网的三代uPlug，此功能被移除。
普通路由发现待入网智能设备和极路由发现待入网设备共用一个接口。
 
第三代uPlug

1.注册接口

    uSDKDeviceManager.setDeviceScanListener(new IDeviceScanListener() {
    	@Override
    	public void onDeviceScanned(ConfigurableDevice candidater) {
    		System.out.println("uSDK3.x Demo find:" + candidater.getDevIdSubfix());
    	}
    	@Override
    	public void onDeviceRemoved(ConfigurableDevice candidater) {
      	System.out.println(candidater.getDevIdSubfix() +
      			" has been disappeared!");				
       }
    });
	onDeviceScanned`：设备发现时
	onDeviceRemoved：设备被配置入网或其它原因不能被发现时

2.启动扫描
`uSDKDeviceMgr.startScanConfigurableDevice`，如果wifi没开则会直接报错。

3.停止扫描
`uSDKDeviceMgr.stopScanConfigurableDevice`，不需要此功能时务必停止扫描。

4.配置设备入网
一般普通路由使用SmartLink组合mac地址方式配置入网：MAC结尾4个字符：candidater.getDevIdSuffix()，极路由使用免密配置入网，请阅读常见问题回答5。


<h5>注意</h5>
1.	其它应用场景不使用带mac的配置方式入网
2.	安装三代uplug的智能设备同时支持不带mac参数的 uSDKDeviceManager.configDeviceBySmartLink方法。

<h5>SoftAP入网模式</h5>
----------
SoftAP是将智能设备变身为WIFI热点，手机连接热点，然后把无线配置信息发送给设备的一种入网方式。
SoftAP操作或编程时全程务必关闭数据流量，如果用户未关闭App需要关闭和提示。

<h5>了解SoftAP设备入网操作过程</h5>
1. 参考使用说明书让设备进入配置模式，此时智能设备工作于WIFI热点模式，打开手机WIFI列表，就能看见智能设备。
2. 打开通用测试App（硬件测试工具App） SoftAP配置选项卡，显示uplus-、haier、U-MAC地址后4位组成的WIFI热点列表。
3. 点击一项，让手机连接到热点。连接成功后，显示手机附近的WIFI列表，使用者选一个，例如：A。输入A的密码，发送给设备。
4. 操作手机连接到A，打开App等待该设备上线。App端设备长时间不上线（1分钟），说明操作失败，重新按照步骤1至6进行。

<h5>App实现SoftAP配置入网</h5>
App实现SoftAP配置入网需要完成的工作：<br/>

1. 把路由的SSID和密码发送给智能设备；
2. 在目标路由中发现在设备；

**实现步骤**<br/>
<ul>
<li>第一步：<b>App提示并关闭手机流量，SoftAP流程必须关闭手机流量。</b>App显示当前连接无线并提供无线选择功能，询问用户智能设备要加入哪个路由。默认为选择手机当前连接WIFI。
</li>
<li>第二步：App收集无线密码，校验路由名称和密码是否符合要求。</li>
<li>第三步：App编程或手动连接到设备WIFI热点U-XX。</li>
<li>第四步：App准备SoftAP配置信息</li>
	<pre>
	//App准备SoftAP配置信息
	 uSDKSoftApConfigInfo softApConfigInfo = new uSDKSoftApConfigInfo.Builder()
      .routeInfo("ssid", "password") 
      .security(false)
      .configTimeout(90)
      .timerValidInBackground(false)
      [.uplusIDList(typeIdList)] 
      .builder();
	
	参数说明：
	“ssid”和”password”是无线名称和密码
	secuirty：false非安全配置
	configTimeout：SoftAP配置全过程超时间90s
	uplusIDList：typeId过滤，过滤TypeId时使用，选择性使用
	timerValidInBackground：当App切换出当前界面时uSDK是否继续消耗超时时间
	无线名称和密码校验要求：SSID最大长度为31个字符，最小长度为1个字符，内容建议为可见英文字符，passwrod设置时要求8至63个字符，内容为可见英文字符。
</pre>
<li>第五步：App进行SoftAP配置&检查配置结果</li>

	final String targetSSID = softApConfigInfo.getSsid();
	uSDKDeviceMgr.configDeviceBySoftAp(softApConfigInfo, 
		new IuSDKSoftApCallback() {	
			@Override
			public void onSoftApConfigCallback(uSDKDevice targetDevice, 	uSDKErrorConst result) {
				if(result == uSDKErrorConst.RET_USDK_OK) {
					//App在此处等待配置最终结果
					//targetDevice：配置入网设备
					Log.d("uSDK4.XDemo", targetDevice.getDeviceId() + “已成功入网”);
				}
			}
			@Override
			public void sendConfigInfoSuccess() {
				//App编程或手动将Android连接至targetSSID等待设备
				Log.d("uSDK4.XDemo", "请立即将手机连接至<" + targetSSID + “>等待设备上线”);
			}
	});
	
	softApConfigInfo：uSDKSoftApConfigInfo对象
	重载方法：uSDKDeviceManger.configDeviceBySoftAp(uSDKSoftApConfigInfo configInfo,TraceNode csNode,IuSDKSoftApCallback callback) 
	注意：上一次SoftAP API执行完成后，才能二次调用。
</ul>
<h5>自带屏幕或操作系统设备入网</h5>
对于一些高级、大型家电或智能设备，其自身可能携带操作系统并且带有显示装置，此时我们应将其视为普通计算机，由其自行引导使用者连接网络。

### 4.4管理设备集变化
uSDK启动完成后会不断扫描本网络里的U+设备，完成设备发现、设备离线，维护设备集合中设备数量及数据的变化，App通过实现设备列表变化回调实现设备集合变化的管理。本章分别讲解：管理变化设备列表集合、获得设备池全集的方法。

<h5>预备知识</h5>
请先了解章节3“快速入门” 

<h5>相关概念和术语</h5>
- 小循环：uSDK和U+设备处于同一无线局域网完成设备交互的情形。
- TYPEID：TYPEID就是设备类型的标识字符串，用它可以识别U+平台上的各种硬件设备。
- 设备连接状态
未连接：App还没连上智能设备，表示设备加入WIFI已发现。调用uSDKDevice的state 属性值是uSDKDeviceStateUnconnect。
离线：设备发现过并且App进行了连接，但此时无法收到设备的响应数据。uSDKDevice             的state属性值是uSDKDeviceStateOffline。
- 设备的属性状态（属性）：开机、关机、运行等值。

<h5>接收uSDK管理设备集变化回调</h5>
App依靠uSDK管理智能设备。uSDK将网络里所有的智能设备都管理起来，形成设备池，设备池里设备的数量会发生变化，例如：配置一台新设备入网，配置过的设备上电加入网络等。uSDK将以上情况都汇报给App，App编程实现设备管理。

App实现并注册如下接口方法，发现及管理网络设备：

    uSDKDeviceManager.setDeviceManagerListener(new IuSDKDeviceManagerListen() {
       @Override
       public void onDevicesRemove(List<uSDKDevicedevicesChanged) {
       		refreshSightedDeviceList(devicesChanged);
       }			
       @Override
       public void onDevicesAdd(List<uSDKDevicedevicesChanged) {
       		refreshSightedDeviceList(devicesChanged);
       }
       ……
    });
	devicesChanged：变化的设备集合
	onDevicesAdd：在网络里发现新设备
	onDevicesRemove：曾经发现的设备现在扫描不到了，它们不可用了

<h5>注意</h5>
本接口是即时接口，在uSDK启动前设置。
如果延时设置或再次设定本接口，需要结合查询设备所有列表使用，确保智能设备列表数据齐全。

<h5>设定设备过滤类型</h5>
App只关心设备池里的某类设备（例如：酒柜设备）：
App执行如下方法：
`uSDKDeviceManager.setInterestType(uSDKDeviceTypeConst.WINE_CABINET)`

App实现管理设备集变化回调：
此时uSDK只向App上报`uSDKDeviceTypeConst.WINE_CABINET`类型的设备，实现设备过滤。

<h5>注意</h5>
再次执行setInterestType方法，将覆盖已设定的设备过滤类型。

<h5>获得设备池全集</h5>
`uSDKDeviceManager.getDeviceList()`本方法也支持设备类型过滤。

### 4.5	与设备建立或断开连接
现在我们已经发现设备，调用API就可以连接了。本章分别讲解：连接设备、断开设备连接。

<h5>预备知识</h5>
“启动uSDK”请参考章节4.1，“发现及管理网络设备”请参考章节4.3。

<h5>相关概念或术语</h5> 
- 连接中(connecting)：App正在与设备建立连接。
- 已连接(connected)：App已连接智能设备，可以发送控制指令，查询属性状态等，是我们可以交互的设备。
- 离线(off_line)：App不断开与智能设备连接的情况下，智能设备发生异常。

未连接、连接中、已连接、离线都是设备（uSDKDevice）的连接状态。App连接或断开智能设备将触发连接状态的变化：
<ol>
<li>智能设备正常入网后是未连接（被我们发现</li>
<li>连接智能设备后，智能设备变为连接中</li>
<li>与智能设备连接成功后，变为已连接</li>
<li>如果连接失败或通信中断会变为离线</li>
<li>断开与智能设备的连接，则变为未连接</li>
</ol>

<h5>设备连接状态变化接口</h5>
App先注册监听，然后执行连接方法。智能设备的连接状态会通过回调上报：

	uSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
			@Override
			public void onDeviceOnlineStatusChange(uSDKDevice device,
				uSDKDeviceStatusConst status, int statusCode) {
			}
		}
		……
	});
	status：设备连接结果。
	statusCode：设备连接状态变化码，设备连接状态异常时提示用户或帮助分析问题。
	statusCode信息表
	statusCode值	含义
	50002：App连接U+云失败
	50003：智能设备连接U+云失败（云端通知设备离线）
	50007：心跳超时
	50008：App连接设备失败（本地）
	50009：App读取智能设备数据失败（本地）
	50010：App向智能设备写数据失败（本地）
<h5>连接设备</h5>
`uSDKDevice.connectNeedProperty`<br/>
`uSDKDevice.connect`

<h5>断开连接</h5>
不需要某台设备属性数据时，断开设备连接，释放设备资源：<br/>
`uSDKDevice.disconnect`

<h5>主动查询连接状态</h5>
`uSDKDevice.getStatus()`

<h5>注意</h5>
<ul>
<li>断开设备连接，uSDK将不再上报此设备的任何属性消息给App。</li>
<li>当用户解绑定设备时，一定执行断开设备连接。</li>
<li>
Android设备熄屏或进入后台强制uSDK与设备保持连接请执行：uSDKDeviceManager.setKeepLocalOnline(true)，uSDK启动成功后执行本方法。
</li>
</ul>

###4.5.	获得设备属性状态
现在已经连接成功了，我们就能实时获得设备的最新属性值集合与设备交互了。本章主要讲解：获得设备的连接状态和实时属性值。

<h5>预备知识</h5>
与设备建立连接请参考章节4.4。

<h5>相关术语</h5>
- 六位码：六位码是一个键值对，在程序中是uSDKDeviceAttribute对象，是App和设备沟通交流的语言。举例：20w001代表开关机功能，App通过uSDK发送uSDKDeviceAttribute｛20w001:20w001｝就是开机，发送uSDKDeviceAttribute｛20w001:20w002｝关机，查询设备属性时uSDKDeviceAttribute｛20w001：20w001｝说明电视开机。
- ID文档：六位码的集合文档，主要用途是明确设备操作指令及属性集等。

<h5>获得设备的属性状态</h5>
第一步：App实现并注册如下接口，准备接收设备属性的实时值：

	uSDKDevice.setDeviceListener(new uSDKDeviceListener() {
	……
	@Override
		public void onDeviceAttributeChange(uSDKDevice device,
						List<uSDKDeviceAttribute> propertiesChanged) {		
		}
	……		
	});
	propertiesChanged：变化的属性集合。
第二步：App连接智能设备<br/>
`uSDKDevice.connectNeedProperty(),uSDKDevice.connect()。`<br/>
第三步：App根据ID文档发送查询指令（发送单命令请参考4.7章节）。指令执行成功后，就可以在接口里接收属性数据了。
查询指令举例如下：
 
	uSDKDevice.writeAttribute("60w0ZZ", null, new IuSDKCallback() {	
		@Override
		public void onCallback(uSDKErrorConst result) {
			if(result == uSDKErrorConst.RET_USDK_OK) {
				System.out.println("command succeed！");
			}				
		}
	});

<h5>App主动获得当前设备所有属性值</h5>
当我们第一次通过接口获得设备的属性状态后，只要设备已连接或就绪，调用`uSDKDevcie.getDevcieAttrMap`返回设备最新属性值合集。
### 4.6.处理设备故障和报警
我们连接设备后，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载。报警信息六位码具体含义参考相关ID文档。本章讲解：获得报警消息、发送停止报警指令、获得报警解除消息、主动查询设备报警等内容。

<h5>预备知识</h5>
与设备建立连接请参考章节4.4。
单命令及如何向设备发送指令请参考章节4.7。

<h5>获得报警消息</h5>
实现uSDKDeviceListener获得报警消息，报警消息和设备属性状态变化是同一接口。

	uSDKDevice .setDeviceListener(new uSDKDeviceListener() {
		……
		@Override
		public void onDeviceAlarm(uSDKDevice myDevice, List<uSDKDeviceAlarm> 	alarmList) {
			
			StringBuffer alarmListStr = new StringBuffer();
			for(uSDKDeviceAlarm  deviceAlarm : alarmList) {
				alarmListStr.append(myDevice.getIp() 
				+ "报警信息：" + deviceAlarm.getAlarmMessage() );
			}
		}
		……
	});

<h5>发送停止报警指令</h5>
智能设备或家电可能向uSDK规律性快速报警，App处理完成后，可以向设备发送停止报警指令，用于表示使用者已经了解设备发生故障。停止报警是一条普通单命令，参考ID文档编写。

<h5>获得报警解除消息</h5>
发生故障的设备修理正常后会发送报警解除消息，报警解除就是没有报警，就是正常。报警解除消息与普通报警消息都在onDeviceAlarm()接口方法产生，App注意分辨。报警解除的具体值，请参考设备ID文档或设备模型文档。

<h5>主动查询设备报警</h5>
App可以调用API主动查询设备报警信息

	ArrayList<uSDKDeviceAlarm> mAlarms = mDevice.getAlarmList();
	if (mAlarms != null) {
		for (uSDKDeviceAlarm alarm : mAlarms) {
			alarm.getAlarmMessage();
		}
	}
	mDevice：uSDKDevice对象，App查询mDevice是否有报警信息。
	mAlarms：设备当前的报警信息。

###4.7.执行设备控制
我们和设备已经连接了，可以向设备发送控制指令。uSDKDevice提供API用于执行设备控制。执行控制时，可以给设备发送单命令、组命令，设定超时时间，命令执行后会反馈结果，下面将分别讲解。

<h5>预备知识</h5>
与设备建立连接请参考章节4.4
获得设备属性状态上报请参考章节4.5

<h5>相关术语和概念</h5>
<ul>
<li>TYPEID：TYPEID就是设备类型的标识字符串，可以使用TYPEID区分设备类型</li>
<li>ID文档：智能设备命令属性集合描述文档</li>
<li>标准模型设备文档：智能设备命令属性交互描述文档，海极网创建智能设备时生成
<li>单命令：App只发送一个指令给设备，例如ID文档片段如下：
 
发送｛204003:204003｝代表启动指令，发送｛204004:204004｝代表暂停指令。
设备状态返回时｛204003:｝+｛204004:204004｝，代表当前智能设备是暂停。</li>
<li>组命令：App发送指令集合给设备，例如ID文档版本如下：
 
单命令/组命令说明当前指令204001可以作为单命令发送，同时也可以运用到组命令中。</li>
</ul>

<h5>与六位码智能设备交互</h5>
<h5>发送单命令</h5>

	selecteduSDKDevice.writeAttribute("204003", "214001", new IuSDKCallback() {		
		@Override
		public void onCallback(uSDKErrorConst result) {
			if(result == uSDKErrorConst.RET_USDK_OK) {
				System.out.println(“Command succeed!");
			}
						
		}
	});
	selecteduSDKDevice：uSDKDevice对象实例，代表智能设备。
	selecteduSDKDevice.writeAttribute：命令发送方法。
	IuSDKCallback：控制结果反馈回调。
	214001：214001：ID文档描述的控制指令。

<h5>注意</h5>
App需要严格遵守ID文档规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。

<h5>发送组命令</h5>
16进制组命令字处理
 
想执行低谷时间设置功能，就需要执行图片显示的组命令，其中000007是组命令字。发送时组命令字使用”000007”，长度为6个字符（不足6位的从左补0），另外组命令字要求全部大写。

	List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>();
	mAttrs.add(new uSDKArgument("20600E", "01:02"));
	mAttrs.add(new uSDKArgument("20600F", "02:03"));
				
	selecteduSDKDevice.execOperation("000007", mAttrs, 10, new IuSDKCallback() {

		@Override
		public void onCallback(uSDKErrorConst result) {
			if (result != uSDKErrorConst.RET_USDK_OK) {
				System.err.println(result);
			}
		}
	});
	mAttrs：组命令集合
	selectedDevice：uSDKDevice对象实例，发指令给selectedDevice
	selectedDevice.execOperation：命令发送方法
	“000007”：组命令标识字
	10：超时时间，单位是秒
	IuSDKCallback：命令执行结果回调

10进制组命令字处理
组命令标识字为整形时，先转为16进制，再按字符串下发，<b>19807=0x4d5f</b>，不足6个字符补0，得到”004d5f”，转为大写”004D5F”，组命令下发时填写<b>”004D5F”</b>作为组命令字。

	List<uSDKArgument> mAttrs = new ArrayList<uSDKArgument>();
	mAttrs.add(new uSDKArgument("202003", "202003"));
	……
				
	selecteduSDKDevice.execOperation("004D5F", mAttrs, 10, new IuSDKCallback() {

		@Override
		public void onCallback(uSDKErrorConst result) {
			if (result != uSDKErrorConst.RET_USDK_OK) {
				System.err.println(result);
			}
		}
	});

	注意
	1．组命令说明中需要的指令放入mAttrs，mAttrs没有顺序要求。
	2．组命令标识字具体值参考设备的ID文档。
	3．App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。

<h5>与海极网创建智能设备交互（标准模型设备文档）</h5>
海极网支持创建智能设备，创建完成后会产生《XX设备应用开发文档》，以下将说明如何使用此文档与设备进行交互。

**属性**的含义是此项可以作为智能设备的属性，可写列为T时，此项可以作为命令发往设备：
 

	selecteduSDKDevice.writeAttribute("A", "True", new IuSDKCallback() {		
	
	@Override
	public void onCallback(uSDKErrorConst result) {
		if(result == uSDKErrorConst.RET_USDK_OK) {
			System.out.println(“Command succeed!");
		}
						
	}
	});
	selecteduSDKDevice：uSDKDevice对象实例，代表智能设备。
	selecteduSDKDevice.writeAttribute：命令发送方法。
	IuSDKCallback：控制结果反馈回调。
	A：True：发送A功能开。

	注意
	发送指令时Key和Value大小写敏感（例如：”True”）。
	可写列为F时，此行不能作为指令发送。

<h5>操作</h5>
操作的含义是App端可以发送表格描述的操作实现对应的功能，这里包含您在海极网创建的高级命令。
示例一：三种常用指令
 
	List mAttrs = new ArrayList();
	selecteduSDKDevice.execOperation(“getAllProperty”, mAttrs, 10, 
			new IuSDKCallback() {
			@Override
			public void onCallback(uSDKErrorConst result) {
			if (result != uSDKErrorConst.RET_USDK_OK) {
				System.err.println(result);
			}
		}
	}

示例二：自定义的高级指令，tempPwdLengthX及其它几项是您设备的属性之一。
 
		List mAttrs = new ArrayList();
		mAttrs.add(new uSDKArgument(“tempPwdLengthX”, “10”));
		mAttrs.add(new uSDKArgument(“tempPwdPart1X”, “11”));
		mAttrs.add(new uSDKArgument(“tempPwdPart2X”, “12”));
		……
		selecteduSDKDevice.execOperation("Tmpopen", mAttrs, 10, 
				new IuSDKCallback() {

			@Override
			public void onCallback(uSDKErrorConst result) {
				if (result != uSDKErrorConst.RET_USDK_OK) {
					System.err.println(result);
				}
			}
		});

	selecteduSDKDevice：uSDKDevice对象实例，代表智能设备。
	selecteduSDKDevice.execOperation：命令发送方法。
	IuSDKCallback：控制结果反馈回调。
	10：控制命令超时时间。

<h5>注意</h5>
App需要严格遵守应用开发文档的规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。

<h5>命令超时设定</h5>
uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。建议超时时间范围：T（单位：秒）5< T <120，App根据自身业务设定超时间。

<h5>命令执行结果与设备反馈</h5>
命令执行结果在IuSDKCallback回调中给出。uSDKErrorConst.RET_USDK_OK代表发送成功，uSDKErrorConst.RET_USDK_TIMEOUT_ERR代表超时。
设备执行成功有可能改变设备的属性值，App接收设备的最新属性上报即可。


### 4.8.远程控制及U+云平台推送消息
经过前面的开发，我们已经可以在本地和智能设备完美交互了，但我们的手机不能切换路由或者使用4G，这么做和智能设备的数据通路会立刻切断。现在我们让App同时连接远程服务器，手机更换WIFI或使用4G则直接通过服务器连接设备。

实现远程控制，本章将讲解以下内容：
<ul>
<li>一般的U+物联App如何运行</li>
<li>连接用户接入网关</li>
<li>连接用户接入关后新绑定设备添加远程控制能力</li>
<li>设备解绑时解除设备远程能力</li>
<li>账号注销时如何处理</li>
<li>如何测试远程功能是否正常</li>
</ul>

<h5>实现设备远程控制预备知识</h5>
请参考4.7以前所有章节实现小循环与设备交互。
请参考章节3.1“基本业务流程”或章节5，确定编程连接用户接入网关时机。

<h5>相关概念及术语</h5>
<ul>
<li>
用户接入网关：U+云支持App实现远程功能的服务器软件系统。App编程使用如下服务器地址和端口：
服务器描述	地址	端口
<table>
<tr>
<td>服务器描述</td>
<td>地址</td>
<td>端口</td>
</tr>
<tr>
<td>开发者环境（中国）</td>
<td>usermg.uopendev.haier.net</td>
<td>56821</td>
</tr>
<tr>
<td>生产环境（中国）</td>
<td>gw.haier.net</td>
<td>56811</td>
</tr>
<tr>
<td>中国以外服务器环境</td>
<td colspan="2">请自行确定</td>
</tr>
</table>
<li>U+云OPEN API：U+云为开发人员提供的网络API，主要和用户账号等业务系统交互（OPEN API开发文档请从海极网下载）。</li>
<li>accessToken(session)：App用户账号登录后OPEN API分配。</li>
<li>绑定、设备绑定：由App发起将用户及所属设备数据上送到云并创建设备数据档案的过程，设备能够被远程控制的前提是已经被用户绑定。</li>
<li>切网：用户由设备所在A路由切换到B路由，或改用数据网络的行为。</li>
<li>小循环VS大循环：它们是两种情景的描述。小循环指的是App与智能设备在同一无线局域网。大循环是说App需要借助U+云用户接入网关才能和设备进行交互的情景，此时App与设备通常不在同一网络。</li>
<li>由于加密需要，设备运行设备时间必须准确，当前时间需要超过2015年3月1日</li>

 
图 大循环

<h5>一般的U+物联App如何运行</h5>
章节5描述了一般U+物联App的运行过程，本章讲解连接用户接入网关这个步骤。App执行uSDKDeviceManager.connectToGateway连接用户接入网关，此方法需要几个参数：session是U+云账号登录后的accessToken，域名和port就是用户接入网关相关的域名和port；设备信息集合就是用户拥有哪些设备，我们需要做的就是把这些参数凑齐然，参考章节5的图示，适时运行方法连接远程服务器。

<h5>连接用户接入网关</h5>
App在日常使用情景中，连接用户接入网关时，用户可能已经拥有一台智能设备，举例：
1.使用OPEN API拉取用户设备列表json（一台燃气热水器），我们将要使用其中三个红色加粗字段：
	<pre>/*
{"id":"0007A88A527B",
"status":{"online":true},
"location":{"cityCode":null,"longitude":"0.0","latitude":"0.0"},
"attrs":{"brand":null,"model":null},"name":"燃气热水器_527B",
"mac":"0007A88A527B","
type":{"type":"18",
"typeIdentifier":"111c120024000810180400418002480000000000000000000000000000000000",
"specialCode":"0041800248","subType":"04"},
"version":{
	"eProtocolVer":"2.15",
	"smartlink":{"smartLinkSoftwareVersion":"e_0.1.36",
	"smartLinkHardwareVersion":"G_1.0.00",
	"smartLinkPlatform":"UDISCOVERY_UWT",
	"smartLinkDevfileVersion":"0.0.0"}}}*/
</pre>

2.执行连接用户接入网关
使用步骤1的五个标红字段，为用户账号下的每台设备都生成一个uSDKDeviceInfo，每个字段都不能为空和随意填写，如果用户账号无设备使用size为0的设备集合。
	
	uSDKDeviceInfo remoteCtrledDeviceInfo = new uSDKDeviceInfo(
		"0007A88A527B", 
		"111c120024000810180400418002480000000000000000000000000000000000",
		true);
	
	//"0007A88A527B"：deviceId（设备MAC）
	//"111c120024000810180400418002480000000000000000000000000000000000"：设备uplusId
	//true：”online”时选true，其它选false

	List<uSDKDeviceInfo> remotedCtrledDeviceCollection = 
				new ArrayList<uSDKDeviceInfo>();
	remotedCtrledDeviceCollection.add(remoteCtrledDeviceInfo);
	/*
		* 连接用户接入网关
		*  App开发测试时设置域名和端口连接开发者环境，发布时设置生产环境
		*  uSDKDeviceManager.connectToGateway("token”, “usermg.uopendev.haier.net”, 56821
		*				, remotedCtrledDeviceCollection, new IuSDKCallback() 	{
		*   开发者环境：“usermg.uopendev.haier.net”, 56821
		*   生产环境（App发布）：“gw.haier.net”, 56811
		*/
	uSDKDeviceManager.connectToGateway("abcdefghijklmnopqrstuvwxyz123456",
	“gw.haier.net”, 56811,remotedCtrledDeviceCollection, new IuSDKCallback() {
		@Override
		public void onCallback(uSDKErrorConst result) {
			if(uSDKErrorConst.RET_USDK_OK == result) {
					
			} else {
				//方法执行错误原因
				//uSDKErrorConst.ERR_USDK_INVALID_PARAM错误值需要检查某台设备的deviceId、//uPlusId是否异常
				System.err.println(result.getValue());
			}	
		}
	});
	abcdefghijklmnopqrstuvwxyz123456：accessToken/session。
	remotedCtrledDeviceCollection：允许远程控制设备信息集合，账号无设备时使用空集合。

<h5>远程服务器的连接状态</h5>

uSDKDeviceManager提供一个回调，可以了解我们当前与远程服务器的连接状态：
	
	uSDKDeviceManager.setDeviceManagerListener(
		new IuSDKDeviceManagerListener() {
			
		……
		@Override
		public void onCloudConnectionStateChange(
				uSDKCloudConnectionState status) {	
			//已连接：uSDKCloudConnectionState.CLOUD_CONNECTION_STATE_CONNECTED
		}
	});

<h5>处理绑定一台新设备</h5>
绑定一台新设备后，按“连接用户接入网关”小章节，将新绑定的设备加入集合，再次连接用户接入网关，确保新设备远程可用，并按需连接设备以实现控制或查询状态。

<h5>用户解绑定时解除设备远程能力</h5>
用户解绑定设备成功时，已经失去对设备的控制权，断开设备连接，释放设备对象实例，按“连接用户接入网关”小章节再次连接用户接入网关。

<h5>注意</h5>
如果不是用户主动解绑设备，但收到接入网关推送的设备解绑定消息（此时设备可能被他人解绑定），App需要编写逻辑处理此消息：
	

	uSDKDeviceManager.setDeviceManagerListener(
		new IuSDKDeviceManagerListener() {
		……
				
		@Override
		public void onDeviceUnBind(String mac) {								
		//建议提示用户设备被解绑
		}
	});

<h5>账号注销时的处理</h5>
当用户账号注销时，我们需要调用 uSDKDeviceManager.disconnectToGateway断开远程链接，清空账号数据。 

<h5>如何测试远程功能是否正常</h5>
手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、4G数据网络，查询设备状态、进行控制。

<h5>注意</h5>
<ol>
<li>当用户切网时，uSDK自动尝试连接用户接入网关，查询设备的最新状态。</li>
<li>App使用OPEN API在开发者环境绑定设备，智能设备也要连接开发者境。</li>
<li>App使用OPEN API过程中，U+云平台要求App有自己的ClientId。ClientId和终端的身份验证是相关的，错误或固定的ClientId会产生异常行为，App可以使用uSDKManager. getClientId()方法产生ClientId。</li>
</ol>

<h5>接收U+云推送消息</h5>
App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。

实现如下方法接收云的推送消息：

	uSDKManager.setManagerListener(new IuSDKManagerListener() {
		……
		@Override
		public void onBusinessMessage(String content) {

	}}
	);
	setManagerListener：uSDK事件回调
	onBusinessMessage：用户接入网关消息推送回调方法
	content：消息内容

<h5>接收Token异常消息</h5>
当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，检查程序逻辑，查看用户登录地址与连接用户接入网关地址是否同一服务器环境。
	
	uSDKManager.setManagerListener(new IuSDKManagerListener() {
		//session：失效session	
		@Override
		public void onSessionException(String session) {					
		}
	});
	session:异常session串
### 4.9.和获取设备WIFI信号强度
uSDK4.2.02及以上开发者按照章节4.8开发完成后，就可以使用uSDKDevice实例获取设备的Wifi信号强度。

	uSDKDevice.getDeviceNetQuality(new IuSDKGetDeviceNetQualityCallback() {
		@Override
		public void onDeviceNetQualityGet(
			uSDKErrorConst result, 
			uSDKDeviceNetQuality level, int detailValue) {
			if(result == uSDKErrorConst.RET_USDK_OK) {
				//uSDKDeviceNetQuality.GOOD 信号强度良好
									
			}
		}
	});

	IuSDKGetDeviceNetQualityCallback：信号强度方法回调
	level：信号强度等级
	detailValue：具体数值
###4.10.和带子机设备交互
带子机设备通常有商用空调、星盒等设备，在程序中我们可以和主机和子机进行交互，该类型设备有主机和子机，子机从属于主机，一台主机可以有多台子机。主机拥有mac，子机没有。

<h5>获得子机实例</h5>
当主机已连接后，通过向主机发送查询指令（根据设备开发文档），由设备上报子机的动态，在回调中就可以得到子机实例，子机的数量会动态变化。
	
	parentuSDKDevice.setDeviceListener(new IuSDKDeviceListener() {
		……
		@Override
		public void onSubDeviceListChange(
			uSDKDevice currentDevice, ArrayList<uSDKDevice> childArrayList) {
			……
		}
		});
		parentuSDKDevice：主机对象
		childArrayList：附属子机实例集合

<h5>子机的连接状态及控制</h5>
子机只有两种连接状态：离线、已连接。子机只要不是离线状态，我们就可以和子机进行交互。对子机的指令仅能影响子机本身。子机的实例对象也是uSDKDevice，参考章节4.8执行设备控制。

<h5>子机属性及报警</h5>
子机的属性和报警将通过回调上报给App，实现以下接口并设定到子机对象实例上：
	
	private class ChildDeviceCtrlListner implements IuSDKDeviceListener {

		@Override
		public void onDeviceAlarm(uSDKDevice uSDKDevice, List<uSDKDeviceAlarm> list) {
			System.out.println("我有子机报警信息……");
		}

		@Override
		public void onDeviceAttributeChange(uSDKDevice childDevice, List<uSDKDeviceAttribute> list) {
			System.out.println("我有子机属性状态信息……");
		}

		@Override
		public void onDeviceOnlineStatusChange(
			uSDKDevice uSDKDevice, 
			uSDKDeviceStatusConst uSDKDeviceStatusConst, int i) {
				System.out.println("子机上下线消息：" + uSDKDevice.getDeviceId()
					+ "-" + uSDKDevice.getSubId()
					+ "-" + uSDKDevice.getSubType());
			}

			……
		}

<h5>主机控制</h5>
我们可以像普通设备一样和主机交互，其中一些指令将影响到附属的所有子机。

<h5>辅助方法</h5>
<pre>
selectedChildDevice.getMainDevice()：获得主机
selectedChildDevice.isSubDevice()：子机？
selectedChildDevice.isMainDevice()：主机？
selectedChildDevice.getSubType()：子机类型
selectedChildDevice.getSubId()：子机Id
parentDevice.getSubDeviceList()：获得当前有哪些子机
parentDevice.getSubDeviceById()：根据子机Id获得子机实例
</pre>

###4.11.退出uSDK
App不需要使用U+物联功能时可以停止uSDK。
<pre>uSDKManager.stopSDK();</pre>

###4.12.有用的类和方法
<h5>uSDKDevice有用的方法</h5>
<pre>
uSDKDevice.getIP可以得到设备IP地址。
uSDKDevice.getStatus查询设备的连接状态。
uSDKDevice.getNetType可以得知设备是本地设备还是远程设备。
uSDKDevice.getDeviceType查询设备类型，例如冰箱、洗衣机。
uSDKDevice.getUplusId查询设备uplusId(typeId)。
uSDKManager.getClientId()得到终端ClientId。
</pre>


<h5>调用API获得uSDKDevice</h5>
<pre>
uSDKDeviceManager.getDevcieList获得当前可交互的所有设备。
uSDKDeviceManager.getDevice使用DeviceId获得设备实例。
uSDKDeviceManager.getDeviceList(uSDKDeviceTypeConst.XXXXXXX)按类型取。
</pre>

<h5>获得uSDK版本号</h5>
<pre>
uSDKManger.getuSDKVersion()
</pre>

##5.使用uSDK绑定
从4.0开始 uSDK开始提供绑定功能，同时 uSDKDevice提供获取绑定信息 API（对应uws接口加密信息字段），方便App自行绑定用。
uSDKDevice的security属性：0为非安全设备，非0为安全类设备。安全类设备：设备配置入网成功后，10分钟内获得绑定key，安全设备必须绑定才能在uSDK中就绪。

###5.1.使用uSDK绑定步骤
<ol>
<li>帐号登录U+云成功</li>
<li>执行连接用户接入网关，参考4.8章节</li>
<li>设备配置入网成功</li>
<li>使用uSDK绑定</li>
</ol>

调用本方法将入网设备绑定设备至UWS。

	uSDKDeviceManager.bindDevice(uSDKDevice, "device name", 90, 
			new IuSDKCallback() {
		@Override
		public void onCallback(uSDKErrorConst result) {
			//绑定成功
			if(result == uSDKErrorConst.RET_USDK_OK) {
			}				
		}
	});
	uSDKDevice：配置成功入网的设备
	“device name”：UWS接口定义的设备名称
	90：90秒，推荐时间90s，范围20-120（秒）
	result：信息反馈，result值为uSDKErrorConst.RET_USDK_OK时绑定成功。
	本方法包含拿绑定信息和绑定两个步骤，本方法自带重试。
###5.2.App获取绑定信息
如果App自行编程绑定设备，请参考下列API获得绑定信息，示例方法：

	uSDKDevice.getDeviceBindInfo(userToken, 60, new IuSDKGetDeviceBindInfoCallback() {
		@Override
		public void onDeviceBindInfoGet(uSDKErrorConst result, String info) 	{
			if(result == uSDKErrorConst.RET_OK) {
				Log.d(“uSDK4.XDemo”, “绑定Info ” + info);
			}					
		}
	});
	userToken：用户登录U+云成功的token值。
	60：超时时间秒。
	key：“设备绑定信息”
	重载方法：getDeviceBindInfo(
	String token,int timeout,TraceNode csNode,IuSDKGetDeviceBindInfoCallback callback)

	注意
	1．本方法自带重试机制，在超时时间内自动重试。
	2．安全类设备：设备绑定是App使用uSDK与设备本地、远程交互的前提条件。
	3．拿绑定key有效时间为10分钟，失效后，需要重新进行设备配置，再次获取绑定key。
###5.3.异常处理
绑定Info是设备从U+云请求的，所以我们依赖于设备正常连接外网，连接U+云。获取绑定Info或绑定失败时参考下表: 
<table>
<tr>
<th>错误码</th><th>信息</th><th>解决建议</th>
</tr>
<tr>
<td>27002</td>	<td>账号未登录</td>	<td>账号登录成功</td>
</tr>
<tr>
<td>27004</td>	<td>coap错误</td>	 <td>程序重试</td>
</tr>
<tr>
<td>-27116</td>	<td>设备本身还没有拿到Key</td>	<td>等待一会儿</td>
</tr>
<tr>
<td>-27117</td>	<td>设备未连接至U+云	<td>检查外网，等待设备一会儿</td>
<tr>
<td>-10003</td>	<td>接口超时	<td>程序重试</td>
</tr>
<tr>
<td>G20910</td>	<td>云平台返错</td>	<td>重新配置绑定</td>
</tr>
<tr>
<td>G20908</td>	<td>云平台返错</td>	<td>重新配置绑定</td>
</tr>
</table>

##6.海外uSDK业务指引
<h5>设定配置文件下载地址</h5>
如果您的App主要在中国以外地区使用，请先调用下面准备方法再启动uSDK，本方法的作用是使您的App可以在实际使用区域获得相关数据文件，使uSDK正常工作。

<h5>预备知识</h5>
参考章节4.1了解uSDK启动内容。

<h5>相关API</h5>
<pre>
uSDKErrorConst  uSDKManager. setProfileServiceUrl (String profileServerUrl)
</pre>

<h5>注意</h5>
<ol>
<li>profileServerUrl是以http或https开头，端口号结尾的url，例如：http://xxxxxx:9090</li>
<li>方法返回值为uSDKErrorConst.RET_USDK_OK说明准备工作成功，不成功终止</li>
<li>profileServerUrl值请根据区域确定</li>
</ol>

<h5>设定uPlug模块主网关地址及端口</h5>
如果您的目标智能设备不能连接本区域的设备接入网关（例如美国销售的智能设备安装中国区域uPlug模块），执行下述API方法使uPlug模块区域适配。

<h5>预备知识</h5>
配置入网请参考章节4.2，与设备建立连接请参考章节4.5。

<h5>本方法执行时机与步骤</h5>
<ol>
<li>智能设备每次必须进入配置模式，10分钟内完成操作。</li>
<li>请连接设备，使设备已连接或就绪。</li>
<li>执行本方法修改主网关地址及端口。</li>
<li>超过10分钟没有成功，需重新进行步骤1至3。</li>
</ol>

<h5>设定uPlug主网关地址及端口</h5>
	
	//以下方法入参均为示例值
	public void uSDKDevice.setDeviceGatewayAndPort(“gw.haieroet.com”, 8569, 	new IuSDKCallback() {
		@Override
		public void onCallback(uSDKErrorConst result) {
			if(uSDKErrorConst.RET_USDK_OK != result) {
				System.err.println("method exec failed!");
			}
		}
	});
	“gw.haieroet.com”：以字母开始的主网关域名字符串
	8569：端口值
	IuSDKCallback：结果回调

第二种设定uPlug主网关地址及端口方法
在执行设备SoftAP配置时，同时设定uPlug主网关地址及端口，示例如下：
	
	uSDKDeviceConfigInfo.setMainGatewayDomain();
	uSDKDeviceConfigInfo.setMainGatewayPort();

<h5>注意</h5>
<ol><li>uSDKDevice.setDeviceGatewayAndPort方法执行失败请尝试重新执行，建议重试三次。</li>
<li>设定uPlug主网关地址及端口方法两种只选其一。</li>
</ol>

<h5>设定产品无线区域</h5>
根据产品要求的需要为无线模块设定国家或地区，此方法在SoftAP写配置信息时执行：

<h5>预备知识</h5>
配置入网请参考章节4.2

	设定无线模块国家地区
	//”JP”:日本，”CN”:中国，”US”:美国，”EU”:欧洲，”WW”:世界
	//举例：设定为中国区
	uSDKDeviceConfigInfo.setApPassword(“password”);
	uSDKDeviceConfigInfo.setCountry("CN");

##9.常见情况问答
<h5>1.为什么我的设备配置不到路由器上？</h5>
答：请首先确认产品说明书要求的路由器频段。市售路由器除了2.4g频段还有5g频段。目前智能设备只能连接2.4g频段；其次登录路由器查看负载数是否太多，网络条件是否较差；最后尝试修改路由工作模式为bg模式，20M带宽。
<h5>2.远程功能开发时不能获得设备属性是怎么回事？</h5>
1.检查App账号功能当前在哪个服务器环境，例如：开发者环境、联调环境。
2.检查智能设备所在路由的DNS，是否和App账号功能在同一服务器环境。
3.检查连接用户接入网关时的各项参数是否正确输入，方法是否正常执行。
<h5>3.为什么我连接设备成功之后总会弹报警解除？</h5>
报警解除消息代表设备当前没有报警。
<h5>4.为什么我的控制指令失败了？</h5>
1.检查发送的指令是否按设备开发文档发送指令，命令key和value是否填写正确。
2.某些智能设备本身有自己的逻辑，例如，空调送风模式时不能设定温度。
3.更换无线网络或更换一台智能设备。
<h5>5.与极路由结合开发设备配置入网功能</h5>
极路由可以安装U+的插件程序，发现待入网设备并实现无密码配置入网。
前提条件
极路由升级固件至1.4版本以上，极路由安装自身市场插件：海尔智能家居，U+设备WIFI模块是瑞昱模组（空调：2.5.10及以上；通用：2.3.30及以上），uSDK4.2.02及以上，极路由连接外网。

功能实现
	发现待入网设备
为了简化开发，发现待入网设备使用已有接口，参考章节4.2配置设备入网内容”发现待入网智能设备&极路由发现待入网设备“，得到待入网设备对象后，您需要先调用如下方法，决定采用何种方式将设备配置入网。
boolean flag = ConfigurableDevice.isSupportNoPwdConfig();当flag为true时，说明当前连接极路由，可以调用免密配置方法。flag为false，您需要参考4.2章节进行SmartLink或SoftAP方式配置入网。

	无密码配置设备入网

	String deviceId = ConfigurableDevice.getDevId();
	//deviceId：MAC值要求大写
	uSDKDeviceManager.configDeviceByNoPassword(deviceId, 60, new IuSDKConfigCallback() {
		@Override
		public void onConfigCallback(
	uSDKDevice deviceJustCatched, uSDKErrorConst result｛	
	//result值为RET_USDK_OK时，deviceJustCatched就是入网成功的设备
	if(uSDKErrorConst.RET_USDK_OK == result) {
		
	}
		}
	}); 
	deviceId：设备Id（Mac值）。
	timeout：建议60，范围是30s-120s。
	IuSDKConfigCallback：结果回调。
	重载方法：
	uSDKDeviceManager.configDeviceByNoPassword(deviceId, typeId, timeout, IuSDKConfigCallback)
	uSDKDeviceManager.configDeviceByNoPassword(
	deviceId, typeId, timeout, TraceNode csNode, IuSDKConfigCallback)
	
	ConfigurableDevice对象有用的方法：
	ConfigurableDevice.isSafe() 判断是否安全模块
	ConfigurableDevice.getTag()  得到类似：uplus-haier-sh-2041-v3-sapz这样的SSID
	ConfigurableDevice.getDevId() 得到全MAC
	ConfigurableDevice.getVersion() 得到uplus-haier-sh-2041-v3-sapz中的“v3”

##10.附录
####10.1.物联开发术语集
<h5>10.1.1.物联模块uPlug</h5>
按照U+通信协议制造的物联模块，实现智能设备或智能家电联网。使用uSDK实现配置智能家电或设备入网时，智能家电或设备必须连接uPlug。
 
图 uPlug物联模块
<h5>10.1.2.智能家电、智能硬件或设备底板</h5>
智能家电：智能空调、智能酒柜、智能热水器、空气净化器等。
智能硬件：U+平台出品的空气盒子、醛知道等。
设备底板：以上产品的核心电子控制电路板（携带uPlug无线物联模块）。
 
图 智能硬件

<h5>10.1.3.配置、配置设备入网</h5>
当用户购买新的智能设备或家电后，智能设备或家电当前并不能自动识别用户家里的无线网络，需要用户进行一些操作，将设备加入网络当中。这一过程就像手机加入某一无线网络，先选择无线，再输入密码。在实际操作过程当中，用户对于智能设备或家电的操作往往是按一个组合按键等，让智能设备进入待命状态，能够监听我们将要发送的无线配置信息。
如果智能设备或家电安装的是uPlug无线物联模块，那么配置设备入网时手机或平板电脑需要连接无线路由器。目前uPlug无线物联模块并不能很好的支持5G频段，所以手机或平板电脑务必连接2.4G无线网络。

<h5>10.1.4.TYPEID、uPlusId</h5>
TYPEID/uPlusId是设备类型标识字符串，海极网创建智能设备后生成此值，一般存储在智能设备硬件上。

<h5>10.1.5.U+云平台UWS</h5>
U+云平台为开发人员提供了一套网络API，此套API主要用于和用户账号系统或U+云家庭设备模型功能交互。UWS开发文档请从海极网下载。

<h5>10.1.6.U+云平台用户接入网关</h5>
U+云平台有一套设备接入网关系统，此系统专职服务于设备连接功能，设备配置入网之后，只要智能设备所在网络可以连接互联网，uPlug就会自动连接到U+云平台设备接入网关。当用户应用进行远程连接或控制时，U+云平台设备接入网关也将成为uSDK的代理，中转uSDK各种设备相关指令。

<h5>10.1.7.绑定、设备绑定</h5>
我们需要为用户和他（她）们的设备，在U+云平台创建一个档案，收集用户的一些基本信息和智能设备的信息，保证他们的对应关系，以便在日后为用户进行增值服务。而由App发起将用户数据和设备数据上送到云的过程就叫做设备绑定，在技术层面上讲就是App发http请求到云，云接受数据并返回一个结果，说明设备绑定到用户账号是否成功。

<h5>10.1.8.六位码</h5>
举例：20w001代表开关机功能，执行开机20w001，执行关机20w002。此时应用程序通过uSDK发送｛20w001:20w001｝执行开机，发送｛20w001:20w002｝执行关机，而当查询属性时，发现20w001对应的值是20w001，说明电视开机。六位码就是一个键值对，表示设备的属性状态或者承载App的控制指令。

<h5>10.1.9.ID文档、设备模型文档</h5>
ID文档就是六位码的集合文档，主要用途是明确设备操作指令集等。设备模型文档是海极网创建硬件同时生成的文档，主要用途是设备属性和操作指令。

<h5>10.1.10.设备就绪</h5>
和智能设备或家电的所有交互都是网络操作，当前智能硬件或智能家电的计算通信能力都是有限的，所以uSDK需要移动应用明确下达指令才会和设备建立数据通信链接，链接成功后，uSDK将会查询指定设备的最新属性状态，并将此状态上报给移动应用，这样的一种状态就是设备就绪，就是设备具备可交互性，设备就绪以后才能通过uSDK发送指令。

<h5>10.1.11.控制、状态反馈、设备报警</h5>
控制就是App通过uSDK发送控制指令操作设备。智能设备或家电通过uPlug物联模块传送当前自己的最新状态到uSDK，uSDK推送消息到移动应用叫做状态反馈。智能设备或家电通过uPlug物联模块传送当前自己的报警状态到uSDK，uSDK推送设备报警给移动应用是设备报警。

<h5>10.1.12.单命令、组命令</h5>
ID文档定义的一个指令属性。单命令发送时只发送命令本身，组命令发送时按ID文档要求发送指令组合。

<h5>10.1.13.切网</h5>
切网就是用户操作移动设备切换当前设备使用其它网络，例如：用户使用无线网络，关闭无线网络，打开3G数据网络。用户使用无线网络A，又切换到无线网络B。

<h5>10.1.14.小循环VS大循环</h5>
在使用uSDK及相关系统进行开发过程当中，可能会听到或看到两个词小循环、大循环，它们是两种情景的描述。小循环指的是移动应用程序与智能设备在同一无线局域网。大循环是说移动应用程序与智能设备不在同一无线局网，数据通路需要完全借助于U+云平台设备接入网关的情景。


[^-^]:常用图片注释  
[router_setting1]:../_media/_usdk/router_setting1.png
[router_setting2]:../_media/_usdk/router_setting2.png
[router_dns_setting]:../_media/_usdk/router_dns_setting.jpg
[iOS_uSDK_Pone_haigeek_down_address]:../_media/_usdk/iOS_uSDK_Pone_haigeek_down_address.png
[ios_infoplist_setting]:../_media/_usdk/ios_infoplist_setting.png
[ios_demo_func_desc]:../_media/_usdk/ios_demo_func_desc.png
[devleop_step_simple]:../_media/_usdk/devleop_step_simple.png
[public_config_step]:../_media/_usdk/public_config_step.png
[devleop_step_detail]:../_media/_usdk/devleop_step_detail.png
[connectstatus_change_step]:../_media/_usdk/connectstatus_change_step.png
[public_single_cmd]:../_media_usdk/public_single_cmd.png
[public_group_cmd_sixcode]:../_media/_usdk/public_group_cmd_sixcode.png
[public_group_cmd_stand]:../_media/_usdk/public_group_cmd_stand.png
[public_op_attr_stand]:../_media/_usdk/public_op_attr_stand.png




[^-^]:文本连接注释
[usdk_document_url]:_document/_usdk/uSDK5.0_Phone_Android开发手册_20180717173928794.pdf


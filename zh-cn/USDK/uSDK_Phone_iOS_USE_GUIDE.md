 **当前版本**：[uSDK_Phone_iOS V5.0]
 **更新时间**：{docsify-updated}

## 1 uSDK5.0简介
uSDK是一款移动应用开发套件，它能够支持目前主流的Android和iOS系统。它允许您运用Java、Object-C、Swift等创建App和智能硬件互动。<br>
通过uSDK可以实现的功能包括智能设备配置入网、搜索、状态查询、控制、接收报警。


### uSDK4.x工程迁移到uSDK5.x
 如果您想从uSDK4x版本代码基础上升级使用uSDK 5.x版本，开发者只需要使用uSDK5.x版本的uSDK.framework和configFiles.bundle替换原工程中的uSDK.framework和configFiles.bundle即可。


## 2.实验开发环境搭建

### 2.1.	官网、QQ群
官网： 海极网 http://www.haigeek.com<br>
官方QQ群：U+开发者技术讨论群457841032

### 2.2.	路由器环境设置
目前海尔智能设备只能匹配2.4g频段路由，才能正常工作和使用，暂不支持5g频段。为了使APP开发者能够顺利进行开发，建议APP开发者仔细阅读此章节后对路由器进行如下设置。

#### 2.2.1.	路由无线网络基本设置
由于当前市场上路由品牌和手机品牌丰富多样，手机和路由联合工作时容易存在兼容性问题（如11bgn mixed模式），导致手机、路由、海尔智能设备不能正常协同工作（始终无法配置成功、始终不能发现U+智能设备或者配置成功率时高时低），这时可以参考如下截图对路由设置进行更改。<br>
注意：SSID号不建议使用中文，建议使用英文和数字。因为中文字符涉及到编解码问题，当APP、路由器编解码机制不同时，将导致设备无法配置成功。


截图一:<br>
![路由无线设置图片1][router_setting1]
 
截图二：<br>
![路由无线设置图片2][router_setting2]


#### 2.2.2.	路由转向设置
在海极网申请开发App后，网站会给出我们开发或测试环境信息，开发测试阶段使用开发者环境，发布时转入正式生产环境。<br>
由于市场上U+智能设备默认连接生产环境服务器，APP开发时需要和U+智能设备处于同一环境（开发者环境），才能进行远程的开发测试，通过对路由进行转向设置，把智能设备和APP（uSDK）通过同一部路由连接到同一环境。如下是配置开发环境举例，配置完成后通过ping uhome.haier.net，查看配置是否成功.见截图：<br>
![路由dns设置图片][router_dns_setting]


### 2.3.搭建实验测试环境
实验设备和步骤：<br>
1.	一台2.4G无线路由器，参考2.2章节配置路由器，并连接互联网<br>
2.	一台iOS 7.0及以上的iOS手机连接路由器，手机中安装uSDK相关App<br>
3.	一台支持U+协议的智能设备或U+Dev开发板，接通电源<br>
4.	操作设备进入待入网模式<br>
5.	使用uSDK功能的APP配置智能设备入网并查看智能设备是否上线

相关资料：<br>
配置设备入网演示视频：QQ群文件<br>
uSDK、uPlug、uGW 、DevKit、uPlug SDK、OPEN API等更多相关资料，见海极网下载中心。

### 2.4	搭建程序开发环境
####  2.4.1下载开发手册、uSDK.framework和uSDKDevDemo
下载地址：ht tp://www.haigeek.com/

![ios开包下载][iOS_uSDK_Pone_haigeek_down_address]


####  2.4.2.导入库文件
例如采用Xcode进行开发，首先将uSDK.framework和configFiles.bundle（文件存放在uSDK.framework包中）文件添加到工程，并在工程中添加系统库libc++.dylib或者libc++.tbd，uSDK引入就成功了。
####  2.4.3.iOS网络访问新特性
 在WWDC 16中，Apple表示将继续在iOS 10和macOS 10.12里收紧对普通 HTTP 的访问限制。从 2017年1月1日起，所有的新提交 app 默认是不允许使用NSAllowsArbitraryLoads来绕过 ATS 限制的，开发者可以选择使用NSExceptionDomains来针对特定的域名开放 HTTP 应该要相对容易过审核。<br>
如果你想App中的网址都通过ATS验证，只有少数一些网址例外的话，你可以为少数的网址添加NSExceptionDomains,并且在下面添加你需要添加的网址即可。然后对每个网址进行分别的设置，其中将NSIncludeSubdomains和NSExceptionAllowsInsecureHttpLoads设置成YES，NSExceptionRequiresForwardSecrecy设置为NO，即可不通过ATS验证。
开发者需要在项目info.plist进行如下设置：
![infoplistsetting][ios_infoplist_setting]

### 2.5.uSDKDevDemo程序
U+开发平台网站提供一款操作电热水器的示例demo ，演示uSDK配置设备入网及同一无线网络与设备互动的情景。
uSDKDevDemo包含的功能<br>
![iosdemo功能介绍][ios_demo_func_desc]

##  3.业务梳理及开发快速入门
本章总结使用uSDK的两个常见开发情景：配置设备入网、使用uSDK进行设备交互。下面给出各自流程图，使开发者大致了解业务运行流程。
###  3.1基本业务接入流程
本图简要介绍uSDK物联App主要业务运行流程，红色文字是uSDK交互，黑色文字是App与U+云交互：<br>
![简单业务运行流程][devleop_step_simple]

注意：
启动uSDK、发现网络设备这两步与登录U+云平台、获取U+云账号设备列表可以互调位置，具体调用方法根据APP实际业务确定


###  3.2详细业务接入流程
本图介绍uSDK和物联App之间主要业务交互运行流程，红色文字是uSDK交互，黑色文字是App与U+云交互：<br>
![简单业务运行流程][devleop_step_detail]


###  3.3配置入网流程
参考下列流程开发配置设备入网功能，特别说明：成功启动uSDK是配置入网功能的前提；只使用uSDK配置入网功能的App，完成设备入网后，可执行退出uSDK。详细介绍请参考“章节4.2配置设备入网”。<br>
![配置业务运行流程][public_config_step]




## 4	uSDK联合调试常见问题
### 4.1  为什么我的设备配置到路由器上很耗时？
答：SmartLink配置后设备上线一般耗时20秒左右。在实际环境中有时耗时30多秒，造成这种情况，有下面几个原因：<br>
一是，路由器连接负载多。不同路由器，价格高低，负载能力都是不一样的。如果一个路由器的设备连接数，接近上限的时候，此时把设备配置到这个路由器上，就会出现很慢的情况，就跟手机连接到路由一样。<br>
二是，测试环境路由器多，各个路由器信号接收之间相互影响，导致配置慢甚至不成功。<br>

### 4.2  	为什么我的设备配置不到路由器上？
答：请首先确认产品说明书要求的路由器频段。市售路由器除了2.4g频段还有5g频段。目前智能设备只能连接2.4g频段。请确认当前设备支持频段和路由工作频段是否一致。

### 4.3. 为何设备已经进入待配置模式，手机发送完配置信息后，手机依然处于待配置模式？也许换一个手机就可以正常配置。
答：是由于手机和路由的兼容性不好引起的，打开路由无线网络基本设置界面，修改为如下数据：无线模式：11bg mixed   频段带宽：20MHz 。

### 4.4.	App在等待设备就绪或已连接状态时如何处理 
答：智能设备或家电在uSDK达到就绪或已连接状态时，对于开发者来讲才是可以交互的。我们所有的操作都是网络的，所以等待智能设备或家电进入就绪或已连接状态需要一定时间，如果连接设备时，设备状态在线，UI界面需要有适当的处理，例如可以显示连接中。

### 4.5.	为什么我的智能设备会频繁上下线？
答：智能设备或家电配置入网之后，使用uSDK时发现设备经常自己上下线，表现就是重新上电之后大约3～4分钟可以上线可以正常使用，超过时间后，设备自己下线或变得不可用。
处理方法：<br>
更换网络负载较小的无线网络<br>
将物联模块重新上电<br>
重新启动路由器<br>
更换物联模块<br>

### 4.6.	为什么移动应用业务数据通路和uSDK数据通路需要在同一个环境？
答：开发uSDK应用与普通应用不同，除了进行普通的数据请求，还可以通过uSDK操作设备，有两条数据通路。我们需要保证两条数据通路在同一服务器环境，如果环境不一致，U+云平台设备接入网关会认为移动应用账号身份不合法，无法和智能家电或硬件进行正常交互。

### 4.7.	为什么我设备成功之后总会弹报警解除？
答：报警解除消息代表设备当前没有报警。

### 4.7.	为什么我设备已连接不就绪？
问题描述：连接且查询设备属性方法后，设备已连接状态，始终无法变成就绪状态。<br>
答：此问题大多是设备底板问题，需要底板开发人员排查，存在如下可能性：<br>
1.设备已远程离线<br>
2.设备底板未正确回复查询设备属性指令<br>
3.设备底板上报的属性状态E++数据非法<br>

### 4.4.模块软件版本大于等于	2.5.00，设备连接后始终离线？
答：模块软件版本大于等于2.5.00的设备是安全类设备，需要完成绑定后才能就绪。

### 4.9	为何绑定失败，提示“非安全设备校验失败”
答：原因1.设备所在网络无法连接外网。解决方法：设备所在网络必须连接外网
   原因2.配置完成10分钟外，执行绑定操作。解决方法：配置完成10分钟内完成绑定操作。
   原因3.app和设备所在环境不同。例如App连接生产环境，设备连接开发者环境。解决方法：app和设备连接同一环境。
   


## 5	物联开发术语集

### 5.1	物联模块uPlug、智能家电、智能硬件或设备底板
联模块uPlug:按照U+通信协议制造的物联模块，实现智能设备或智能家电联网。使用uSDK实现配置智能家电或设备入网时，智能家电或设备必须连接uPlug。<br>
智能家电：智能空调、智能酒柜、智能热水器、空气净化器等；<br>
智能硬件：U+平台出品的空气盒子、醛知道等。<br>
设备底板：以上产品的核心电子控制电路板（携带uPlug无线物联模块）。


### 5.2.	配置、配置设备入网
当用户购买新的智能设备或家电后，智能设备或家电当前并不能自动识别用户家里的无线网络，需要用户进行一些操作，将设备加入网络当中。这一过程就像手机加入某一无线网络，先选择无线，再输入密码。在实际操作过程当中，用户对于智能设备或家电的操作往往是按一个组合按键等，让智能设备进入待命状态，能够监听我们将要发送的无线配置信息。<br>

如果智能设备或家电安装的是uPlug无线物联模块，那么配置设备入网时手机或平板电脑需要连接无线路由器。目前uPlug无线物联模块并不能很好的支持5G频段，所以手机或平板电脑务必连接2.4G无线网络。

### 5.3.	TYPEID
TYPEID就是设备类型的标识字符串。uSDK的功能是通用的，它可以识别U+平台上的各种硬件设备并汇报给使用者，使用者可以使用TYPEID区分设备类型。

### 5.4.	U+云平台OPEN API
U+云平台为开发人员提供了一套网络API，此套API主要用于和用户账号系统或U+云的其它功能进行交互。OPEN API开发文档请从海极网下载。

### 5.5.	APP开发时开发环境设置
APP开发者使用uSDK开发APP时需要在不同的开发环境中进行，主要分为开发者环境、联调环境、生产环境、验收环境、验证环境等。开发过程中，设备和APP应该处于同一环境中，否则会造成获取不到设备、获取不到属性、无法控制等情况。
### 5.6.	U+云平台用户接入网关
U+云平台有一套设备接入网关系统，此系统专职服务于设备连接功能，设备配置入网之后，只要智能设备所在网络可以连接互联网，uPlug就会自动连接到U+云平台设备接入网关。当用户应用进行远程连接或控制时，U+云平台设备接入网关也将成为uSDK的代理，中转uSDK各种设备相关指令。

### 5.7.	绑定、设备绑定
我们需要为用户和他（她）们的设备，在U+云平台创建一个档案，收集用户的一些基本信息和智能设备的信息，保证他们的对应关系，以便在日后为用户进行增值服务。而由App发起将用户数据和设备数据上送到云的过程就叫做设备绑定，在技术层面上讲就是App发http请求到云，云接受数据并返回一个结果，说明设备绑定到用户账号是否成功。

### 5.8.	ID文档、六位码
ID文档就是六位码的集合文档，主要用途是明确设备操作指令集等。
举例：20w001代表开关机功能，执行开机20w001，执行关机20w002。此时应用程序通过uSDK发送｛20w001:20w001｝执行开机，发送｛20w001:20w002｝执行关机，而当查询属性时，发现20w001对应的值是20w001，说明电视开机。六位码就是一个键值对，表示设备的属性状态或者承载App的控制指令。

### 5.9.	标准文档
随着U+平台的日趋开放,为了兼容来自不同厂家的各类物联设备及终端应用软件，海尔制定了统一的物联设备建模标准,对各种物联设备的基本属性、功能等信息进行规范化的描述 。
### 5.10.	设备就绪或已连接状态
和智能设备或家电的所有交互都是网络操作，当前智能硬件或智能家电的计算通信能力都是有限的，所以uSDK需要移动应用明确下达指令才会和设备建立数据通信链接，链接成功后，uSDK将会查询指定设备的最新属性状态，并将此状态上报给移动应用，这样的一种状态就是设备就绪或已连接，设备具备可交互性，设备就绪或已连接状态以后才能通过uSDK发送指令。

### 5.11.	控制、状态反馈、设备报警
控制就是App通过uSDK发送控制指令操作设备。智能设备或家电通过uPlug物联模块传送当前自己的最新状态到uSDK，uSDK推送消息到移动应用叫做状态反馈。智能设备或家电通过uPlug物联模块传送当前自己的报警状态到uSDK，uSDK推送设备报警给移动应用是设备报警。

### 5.12.	单命令、组命令
ID文档定义的一个指令属性。单命令发送时只发送命令本身，组命令发送时按ID文档要求发送指令组合。

### 5.13.	切网
切网就是用户操作移动设备切换当前设备使用其它网络，例如：用户使用无线网络，关闭无线网络，打开3G数据网络。用户使用无线网络A，又切换到无线网络B。

### 5.14.	小循环VS大循环
在使用uSDK及相关系统进行开发过程当中，可能会听到或看到两个词小循环、大循环，它们是两种情景的描述。小循环指的是移动应用程序与智能设备在同一无线局域网。大循环是说移动应用程序与智能设备不在同一无线局网，数据通路需要完全借助于U+云平台设备接入网关的情景。

### 5.15.	uSDK与设备连接状态值定义
未连接（uSDKDeviceStateUnconnect）：设备上电后被uSDK发现，但没有执行设备连接，操作的状态，等同于2.0版本的在线状态。<br>

连接中（uSDKDeviceStateConnecting）：调用连接设备方法，但尚未连接成功或就绪前的状态<br>

连接成功（uSDKDeviceStateConnected）：调用连接设备但不获取设备属性方法成功后的状态，能够查询设备属性状态和执行控制的状态。<br>

就绪（uSDKDeviceStateReady）：调用连接设备并获取设备属性方法成功后的状态，能够查询设备属性状态和执行控制的状态，等同于2.0版本的就绪状态。<br>

下/离线（uSDKDeviceStateOffline）：连接设备后设备断电或切网，设备不可被操作的状态

### 5.16.	uSDK与云平台连接状态值定义
未连接（uSDKCloudConnectionStateUnconnect）：未调用连接云平台方法时的状态<br>
连接中（uSDKCloudConnectionStateConnecting）：调用连接云平台方法，未返回连接结果前的状态<br>
连接成功（uSDKCloudConnectionStateConnected）：调用连接云平台方法，连接成功的状态<br>
连接失败（uSDKCloudConnectionStateConnectFailed）：调用连接云平台方法，连接失败的状态<br>

















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
[public_single_cmd]:../_media/_usdk/public_single_cmd.png
[public_group_cmd_sixcode]:../_media/_usdk/public_group_cmd_sixcode.png
[public_group_cmd_stand]:../_media/_usdk/public_group_cmd_stand.png
[public_op_attr_stand]:../_media/_usdk/public_op_attr_stand.png
[public_stand_cmd_getAllProperty]:../_media/_usdk/public_stand_cmd_getAllProperty.png
[public_stand_op_cmd_2]:../_media/_usdk/public_stand_op_cmd_2.png
[public_user_gateway_dev_online]:../_media/_usdk/public_user_gateway_dev_online.png
[public_get_bindinfo_error_code]:../_media/_usdk/public_get_bindinfo_error_code.png




[^-^]:文本连接注释
[usdk_document_url]:_document/_usdk/uSDK5.0_Phone_Android开发手册_20180717173928794.pdf


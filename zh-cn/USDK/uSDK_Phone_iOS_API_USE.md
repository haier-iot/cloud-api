
**当前版本**：[uSDK_Phone_iOS V5.0]
**更新时间**：{docsify-updated}

## 1.帐号登录
SDK需要登陆后才能正常使用其他API功能，所以APP开发必须在海极网上申请APPID和APPKEY，申请完成后，调用uAccount提供的初始化方法和登录方法帐号。<br>

相关概念和术语<br>
AppID：在海极网为物联App申请的AppId，用于App校验<br>
AppKey：在海极网申请，用于与UWS云服务交互时使用。APP开发中只有“开发测试” 的APP_KEY。开发完成且提交海极网审核完成后会发放“生产” 的APP_KEY，APP使用生产的APP_KEY进行发布。<br>
SecretKey：在海极网申请，uSDK使用

##### 初始化：

	 self.baseStr = @"https://uhome.haier.net:6503";
    [[uAccount defaultUAccount] startWithBaseUrl:self.baseStr appId:self.APPID appKey:self.APPKey];
    
self.baseStr：域名，可以使用不同的域名访问不同环境的服务器。<br>
self.APPID：app的唯一标识<br>
self.APPKey：海极网申请appkey，用于标识该app的使用环境。
    
##### 帐号登录：

	[[uAccount defaultUAccount] loginWithLoginId:userName password:pwd deviceToken:@"" success:^(NSString *result) {
         performSelector:@selector(GetDeviceList) withObject:nil afterDelay:1];    
    } failure:^(RespCommonModel *failure) {
        [AlertViewTools shouAlertViewWithTitle:@"提示" Msg:failure.retInfo];
    } httpError:^(RespCommonModel *httpError) {
        [AlertViewTools shouAlertViewWithTitle:@"提示" Msg:httpError.retInfo];
    }];
userName：申请的U+帐号<br>
pwd:密码<br>
deviceToken：登录时为@""<br>
success:登录成功时触发。<br>
failure：登录失败时触发。<br>
httpError：发生http异常时触发<br>
    
    


## 2.SDK初始化配置及启动
启动uSDK是调用各种功能性API，使用U+物联功能的前提，在主线程中启动一次即可。下面分别讲解启动uSDK及启动前后前后需要进行的必要设置：设置设备管理委托、设置过滤设备类型、启动uSDK、成功后设定uSDK日志级别、开启和/关闭dns防劫持功能。


##### 2.1设置过滤设备类型
uSDK启动前，设置开发者关心的设备类型，可以是一个，也可以是多个；若不设置，默认为全部类型。设置过滤类型后，只能收到或查询到自己关心类型的设备，其他类型的设备将被过滤掉。多次执行[uSDKDeviceManager defaultDeviceManager].interestedDeviceTypes进行赋值，将覆盖已设定的设备过滤类型，以最后一次为准。<br>
示例代码：

    [uSDKDeviceManager defaultDeviceManager].interestedDeviceTypes = ALL_TYPE

##### 2.2 启动uSDK
启动uSDK是使用U+物联功能的前提，按如下方式启动uSDK。

    [[uSDKManager defaultManager] startSDKWithAppId:appDelegate.APPID appKey:appDelegate.APPKey secretKey:appDelegate.SERCERTKEY success:success failure:failure];

appId：海极网分配的appId，不能为空，不能随意填写。<br>
appKey：海极网分配的appkey，不能为空，不能随意填写。<br>
sercertKey：海极网分配的sercertKey，不能为空，不能随意填写。<br>
代码块success启动成功时被触发，可以设置日志级别。<br>
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>
注意<br>
1.uSDK启动时需要到到海尔云端验证appId和version值的合法性，所以appId和Version的值一定要与海极网中的填写的数据一致，如果不一致，会导致appId和version值的合法性验证失败，uSDK不能启动<br>
2.uSDK在主线程中成功启动一次即可，不要多次或在其他个线程中启动uSDK

##### 2.3 设置日志输出级别
uSDK启动成功后，可以设置uSDK的日志级别，开发过程中建议使用USDK_LOG_DEBUG，上线产品建议使用USDK_LOG_NONE或USDK_LOG_ERROR，如不设置，默认为USDK_LOG_DEBUG输出所有日志。uSDK运行时会输出日志，其中包含与硬件交互及反馈给App的详细日志，uSDK的日志标签是uClient和uServer。<br>
示例代码：

    [[uSDKManager defaultManager] setLogWithLevel:USDK_LOG_DEBUG isWriteToFile:NO success:^{

    } failure:^(NSError *error) {

    }];

代码块success方法执行成功时被触发。<br>
代码块failure方法执行失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 2.4 开启/关闭dns防劫持功能
为了保证用户能够正确访问到海尔U+云，uSDK中提供了dns防劫持功能， 默认此功能开启。
当开发人员需要使用非生产环境（开发者、联调、验证）进行开发测试时，在sdk启动后关闭DNS防劫持功能，才能保证 uSDK正常访问对应的云环境。<br>
当APP需要对外发布时，需要开启DNS防劫持功能，保证用户能够正确访问到海尔云。<br>

API介绍如下：

1、开启全部特性

    [[uSDKManager defaultManager] enableFeatures:uSDKFeatureDefault];

2、关闭全部特性

    [[uSDKManager defaultManager] enableFeatures:uSDKFeatureNone];





## 3.搜索设备
uSDK启动前，APP开发者需要实现uSDKDeviceManagerDelegage委托并设置委托，启动uSDK完成后会不断扫描本网络里的U+设备并上报给app，App通过实现设备列表变化回调方法得到实时更新的设备集合。本章分别讲解：管理变化设备列表集合、获得设备列表全集的方法。

### 3.1 拉取云端设备列表
通过uAccount类提供的获取设备列表方法获取用户绑定的设备列表。

    [[uAccount defaultUAccount] getDeviceListWithMainType:nil  typeId:nil success:^(NSArray<UacDevice *> *failedIds) {
        [AlertViewTools shouAlertViewWithTitle:@"提示" Msg:@"登录成功"];
    } failure:^(RespCommonModel *failure) {
        [AlertViewTools shouAlertViewWithTitle:@"提示" Msg:failure.retInfo];
    } httpError:^(RespCommonModel *httpError) {
        [AlertViewTools shouAlertViewWithTitle:@"提示" Msg:httpError.retInfo];
    }];


代码块success，执行成功时触发，failedIds：用户绑定的设备列表。<br>
代码块failure，执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>
代码块httpError， 网络异常时触发<br>

### 3.2 连接用户接入网关
通过执行uSDKDeviceManager的connectToCloudWithDevices方法连接用户接入网关，使SDK缓存的设备对象具备远程交互的能力。<br>
示例代码：

	[[uSDKDeviceManager defaultDeviceManager]connectToCloudWithDevices:devList token:appDelegate.remoteSession gatewayDomain:appDelegate.gatewayDomain gatewayPort:[appDelegate.gatewayPort integerValue] success:^{
                
            } failure:^(NSError *error) {
                UIAlertView *view = [[UIAlertView alloc] initWithTitle:@"connectToCloudWithDevices" message:error.localizedDescription delegate:self cancelButtonTitle:@"ok" otherButtonTitles:nil, nil];
                [view show];
            }];
connectToCloudWithDevices：连接用户接入网关，使设备具备远程控制能力<br>
deviceList：3.1章节中获取的用户绑定的设备列表。<br>
remoteSession，U+云账号登录后的accessToken。<br>
gatewayDomain和gatewayPort：用户接入网关的域名和端口。开发者需要注意智能设备和手机APP是否使用了相同的环境。<br>
代码块success执行成功时被触发。<br>
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

   
### 3.3 获得新入网设备
uSDK会将网络里所有的智能设备都管理起来，形成一个设备列表的集合，并进行管理。设备列表集合的数量会根据实际设备的数量变化而发生变化，例如：配置一台新设备入网并上线，集合中会增加一台设备；设备切断电源然后下线，集合中会减少一台设备。<br>
APP开发者需要实现uSDKDeviceManagerDelegage委托并设置委托，通过实现如下方法，可得到新入网的设备列表；

##### 设置委托：

     [uSDKDeviceManager defaultDeviceManager].delegate = self。

##### 实现委托：

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

### 3.4获得全部设备

    [[uSDKDeviceManager defaultDeviceManager].deviceDict.allValues;

## 4.设备配置绑定
设备配置绑定就是使用uSDK将设备加入指定无线网络且把设备和用户帐号建立关联关系的一项操作。uSDK支持SmartLink、SoftAP、蓝牙配置绑定三种方式，下面讲分别进行讲解。<br>
如果您的产品全部支持加密入网，建议使用加密入网方式，提升整套产品安全性能，否则使用普通配置入网方式。<br>

##### 设备配置绑定前提条件：
1、用户登录且成功调用连接用户接入网关方法（uSDKDeviceManager的connectToCloudWithDevices方法）。<br>
2、设备和SDK在同一网络内，且连接U+云的环境相同（生产、开发）<br>
##### 获取设备配置方式
1、查看设备使用说明书，查看设备配置方式<br>
2、向设备提供人员进行咨询<br>
3.技术支持QQ群：457841032<br>
4.商务经理：wangzheng@haier.com<br>


###  4.1.SmartLink配置绑定
SmartLink方式是成熟稳定的设备入网方式，是U+平台物联设备的主流入网方案，通过将无线配置信息发送给待入网设备，完成配置、并和用户帐号进行绑定的操作。<br>
	此方法使用简单，但易与路由协同工作易存在兼容性问题，导致配置失败，需要对路由参数进行调整。<br>

##### 设备配置入网操作过程
1.确认当前路由器是2.4G频段、11bg模式、20MB。 <br>
2.设备安放在WIFI信号良好的位置，按照设备说明书操作设备进入配置模式。 <br>
3.手机连接到目标路由器，打开基于uSDK的App（通用测试App、uSDKDevDemo程序），按 APP提示进行设备配置绑定操作<br>
4.APP提示配置绑定成功，则设备完成配置绑定操作。App提示配置绑定失败，重新按照1、2、3步骤进行操作。


##### 执行SmartLink配置绑定方法
示例代码如下：

    uSDKSmartLinkBindInfo *info = [[uSDKSmartLinkBindInfo alloc]init];
    info.ssid = ssid;
    info.password = pwd;
    info.timeoutInterval = 120;
    info.security = NO;
    [uSDKBinding bindDeviceBySmartLink:info progressNotify:^(uSDKBindProgressInfo *bindProgressInfo) {
        if(uSDKBindProgressSendConfigInfo==bindProgressInfo.bindProgress){
            [[[ToastView alloc]init]showToastWithMessage:@"配置信息发送中."];
        }if(uSDKBindProgressBindDevice==bindProgressInfo.bindProgress){
            [[[ToastView alloc]init]showToastWithMessage:@"配置绑定中."];
        }
    } success:^(uSDKDevice *device) {
        self.labelDesc.text = @"配置绑定成功";
        [[[ToastView alloc]init]showToastWithMessage:@"配置绑定成功."];
        [self.activityIndicator stopAnimating];
        [self connectToCloud];
    } failure:^(NSError *error) {
         self.labelDesc.text = @"配置绑定失败";
        [self.activityIndicator stopAnimating];
        if (error.code==-21205)return;
        NSString *strMsg = [NSString stringWithFormat:@"result error:%@",[error description]];
        UIAlertView* alert=[[UIAlertView alloc]initWithTitle:@"配置失败" message:strMsg delegate:nil cancelButtonTitle:@"关闭" otherButtonTitles:nil, nil];
        [alert show];
    }];

ssid：无线网络名称，不能为空，最大长度31。<br>
pwd：无线网络密码，可以为空，最大长度63。<br>
timeoutInterval：配置超时时间，单位为秒。范围5-120秒，推荐60秒。<br>
security：安全配置方法标识。YES时，进行安全配置；NO时，普通配置。<br>
info:用于装载ssid、密码、超时时间、安全配置表示等信息。<br>
代码块progressNotify，设备配网进度通知。<br>
代码块success触发时，设备配置绑定成功。<br>
代码块failure配置失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 配置失败可能原因
1.网络原因，网络不稳定，丢包率很高，或者ping延时很长<br>
2.无线密码输入错误<br>
3.手机开启wlan+功能,配置期间手机网络和wifi间切换<br>
4.设备未进入配置模式<br>
5.路由器处于5G频段或11bgn模式


###  4.2 SoftAP配置绑定
SoftAP配置方式是将设备设置为WIFI热点，手机连接设备热点（U-开头），然后把无线配置信息发送给设备，并和用户帐号进行绑定的操作。<br>
此方法配置成功率高，但是相对smartlink操作步骤稍多。<br>

##### 设备配置绑定前提条件：
用户登录且成功调用连接用户接入网关方法（uSDKDeviceManager的connectToCloudWithDevices方法）。<br>

##### 步骤一 连接设备热点
参考使用说明让设备进入待配置模式，此时物联模块工作于WIFI热点模式，打开手机无线局域网，可以看到设备热点（U-开头），将手机连接到设备的WIFI热点。


##### 步骤二 执行SOFTAP配置绑定

示例代码：

    uSDKSoftApBindInfo *info = [[uSDKSoftApBindInfo alloc]init];
    info.ssid = self.SSIDText.text;;
    info.iotDevBssid = [DemoUtils fecthBSSID];
    info.password = self.passwordText.text;
    info.timeoutInterval = 120;
    info.isTimerValidInBackground = YES;
    info.security = NO;
    [uSDKBinding bindDeviceBySoftAp:info progressNotify:^(uSDKBindProgressInfo *bindProgressInfo) {
        if(uSDKBindProgressConnectDevice==bindProgressInfo.bindProgress){
            [[[ToastView alloc]init]showToastWithMessage:@"配置连接中."];
        }else if(uSDKBindProgressSendConfigInfo==bindProgressInfo.bindProgress){
            [[[ToastView alloc]init]showToastWithMessage:@"配置信息发送中."];
        }if(uSDKBindProgressBindDevice==bindProgressInfo.bindProgress){
             [[[ToastView alloc]init]showToastWithMessage:@"配置绑定中."];
        }
    } switchNetworkNotify:^{
        [AlertViewTools shouAlertViewWithTitle:@"绑定提示"
                                           Msg:@"请立即连接目标网络路由"];
    } success:^(uSDKDevice *device) {
        self.configResultLable.text = @"设备配置成功!";
        [[[ToastView alloc]init]showToastWithMessage:@"配置绑定成功."];
        [self connectToCloud];
    } failure:^(NSError *error) {
        self.configResultLable.text = @"设备配置失败!";
        NSString *info = [NSString stringWithFormat:@"%ld", (long)error.code] ;
        [AlertViewTools shouAlertViewWithTitle:@"设备配置失败" Msg:info];
    }];

ssid：无线网络名称，不能为空，最大长度31。<br>
pwd：无线网络密码，可以为空，最大长度63。<br>
info.iotDevBssid：设备的bssid，不能缺少。
info.iotDevBssid：设备热点的bssid，要求调用者必须传入该参数，SDK将用该参数校验是否还在设备热点上，而不是通过设备热点名称。<br>
timeoutInterval：配置超时时间，单位为秒。范围5-120秒。<br>
security：安全配置方法标识。YES时，进行安全配置；NO时，普通配置。<br>
info:用于装载ssid、密码、超时时间、安全配置表示等信息。<br>
sendConfigInfoSuccess： 配置信息发送成功时被触发，需要切网到目标wifi网络，等待设备上线。<br>
success：配置成功时被触发，device是配置成功的设备。<br>
Failure：配置失败时被触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 配置失败可能原因
1.网络原因，丢包较多或延时较长<br>
2.无线密码输入错误<br>
3.设备热点不稳定，配置过成功热点消失<br>
4.配置信息发送成功后，未切换到配置的wifi上<br>
5.设备未进入配置模式

###  4.3 蓝牙BLE配置绑定
蓝牙配置绑定方式是将具备蓝牙配网能力的海尔设备配置入网并完成用户帐号绑定的一项操作。
##### 配置绑定指导
######  步骤一、操作设备进入配置模式
参考设备使用说明书，操作设备进入配置模式。<br>
###### 步骤二、获取待配置蓝牙设备
APP开发者设置并实现uSDKDeviceScannerDelegate委托，开启扫描功能后，通过实现如下方法，可搜索到待配置的蓝牙设备列表。<br>
    示例代码：
 	
	[uSDKDeviceScanner sharedScanner].delegate = self;
    [uSDKDeviceScanner startScanConfigurableDevice];
    //发现待配网设备
    - (void)deviceScanner:(uSDKDeviceScanner *)scanner didFindNewDevice:(uSDKDeviceInfo *)device{
    self.deviceList =[uSDKDeviceScanner sharedScanner].discoverdDevices;
    [self.myTableView reloadData];
    }
    
###### 步骤三、执行配置绑定操作
通过上述步骤得到待配置蓝牙设备后，开发者可以发起蓝牙配置绑定，示例代码如下

	-(void)bleConfigWithDeviceInfo:(uSDKDeviceInfo *) devInfo{
   
    uSDKBLEBindInfo *bleInfo = [[uSDKBLEBindInfo alloc]init];
    bleInfo.ssid = self.SSidText.text;
    bleInfo.password = self.pwdText.text;
    bleInfo.timeoutInterval = 120;
    [uSDKBinding bindDeviceByBLE:devInfo bindInfo:bleInfo progressNotify:^(uSDKBindProgressInfo *bindProgressInfo) {
        if(uSDKBindProgressConnectDevice==bindProgressInfo.bindProgress){
            [[[ToastView alloc]init]showToastWithMessage:@"设备连接中"];
        }else if(uSDKBindProgressSendConfigInfo==bindProgressInfo.bindProgress){
             [[[ToastView alloc]init]showToastWithMessage:@"配置信息发送中."];
        }if(uSDKBindProgressBindDevice==bindProgressInfo.bindProgress){
             [[[ToastView alloc]init]showToastWithMessage:@"备绑定中."];
          
        }
    } success:^(uSDKDevice *device) {
       [[[ToastView alloc]init]showToastWithMessage:@"备绑定成功"];
        [self connectToCloud];
    } failure:^(NSError *error) {
        NSString *strMsg = [NSString stringWithFormat:@"result error:%@",[error description]];
        UIAlertView* alert=[[UIAlertView alloc]initWithTitle:@"配置失败" message:strMsg delegate:nil cancelButtonTitle:@"关闭" otherButtonTitles:nil, nil];
        [alert show];
    }];
    }
devInfo,发现的待配置蓝牙设备。<br>
bleInfo：装载配置信息的对象，包括ssid、密码、超时时间等信息。<br>
代码块progressNotify，设备配网进度通知。<br>
代码块success触发时，设备配置绑定成功。<br>
代码块failure配置绑定失败时触发，error中有需要关注的错误信息，error.code为错误码，开发者需要关注。<br>

##### 步骤四、关闭蓝牙扫描功能
配置绑定完成后，或需要离开此界面时，建议停止蓝牙搜索功能，降低资源消耗。

	 [uSDKDeviceScanner stopScanConfigurableDevice];
	 
<!--###  4.4 二维码绑定方式
通过Smartdevice SDK接入的设备，可以在屏幕上方生成二维码图片，扫描获取二维码信息后通过二维码绑定方式将设备绑定有云平台。扫描二维码绑定方式支持两种二维码生成方式，详见下文。<br>
1、老冰箱二维码信息生成方案,生成规则：typeid(定长64位)+deviceID(12~32位)<br>
2、标准二维码信息生成方案（推荐）, USmartDeviceManager的getBindQRCode(）方法获取二维码信息。


二维码绑定操作示例代码如下：

	uSDKQRCodeBindInfo *qrCodeInfo = [[uSDKQRCodeBindInfo alloc]init];
    qrCodeInfo.timeoutInterval = 60;
    qrCodeInfo.qrCode = @"{“UPID”:“201010463cf0823001211dc4deecd080c48491895ec2ad0556564965ee9de8c0”,“DID”:“0259218ABCD”,“BDK”:“3643663683635313532393469316038303466326033603666616665666566316”,“VER”:“1.0”}";
   
    [uSDKBinding bindDeviceByQRCode:qrCodeInfo success:^(uSDKDevice *device) {
        //绑定成功
    } failure:^(NSError *error) {
         //绑定失败
    }];
代码块success触发时，设备绑定成功。<br>
代码块failure绑定失败时触发，error中有需要关注的错误信息，error.code为错误码，开发者需要关注。<br>
-->


## 5.控制设备
控制设备就是通过SDK将命令发送给智能设备，完成设备功能改变的一项操作，控制命令包含单命令、组命令，具体命令参见设备的功能模型文档或ID开发文档。<br>

使设备能够被成功控制，按如下步骤的开发：<br>
1、获取设备列表<br>
2、连接用户接入网关<br>
3、连接设备<br>
4、执行设备控制方法<br>




### 5.1 建立设备连接
当需要获得某台设备属性数据或进行命令功能控制时，需要先执行设备连接方法，该方法执行成功后，等待一定时间设备会变为“已连接“/”就绪” 状态，设备就绪状态时可以执行设备控制和获取的设备的属性集合。<br>

开发者执行设备连接操作前，可以通过设备的isSubscribed属性对设备的状态进行判断，为NO时进行连接操作，如果是YES，不需要再次对设备执行连接操作。<br>

目前uSDK只支持连接单个设备，不支持同时连接多个设备，如有需要请逐个方法调用。<br>

执行连接设备并获得设备属性方法：

    [self.currentDevice connectNeedPropertiesWithSuccess:^{

    } failure:^(NSError *error) {

    }];

代码块success执行成功时触发，等待一定时间后设备状态将变为“就绪” 状态。<br>
代码块failure执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>




### 5.2 控制设备
完成连接设备操作，设备变为就绪状态后，开发者可以通过uSDKDevice对象发送控制命令给设备，完成设备功能的改变。<br>
核心类uSDKDevice提供API用于执行设备控制。下面将以6位码的ID文档和标准设备模型文档为例，下面将分别讲解单命令和组命令控制。

6位码ID文档和标准设备模型文档获取方式<br>
1、向对接的商务人员索取相关文档<br>
2、设备创建者登录海极网，获取相关文档<br>


#### 5.2.1 发送单命令（6位码）
单命令：App只发送一个指令给设备<br>
![单命令][SingleCmd]

使用uSDKDevice对象发送单命令时，需要严格遵守ID文档规定，发送指定的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。不能使用发送组命令的方法发送单命令。<br>
示例代码：

    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){ 
      void(^success)(void) =^{
       //命令执行成功
      };
      void(^failure)(NSError* error) = ^(NSError* error){
      //命令执行失败
      };
      [self.currentDevice writeAttributeWithName:name   value:value success:success failure:failure];

    }else{
    	//连接状态不可用
       return;
    }
name：属性名，NSString类型。如”201003”<br>
value：属性值，NSString类型。如”201003”<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。 

#### 5.2.2.发送组命令（6位码）
组命令是由组命令号/组命令标识、单命令集合两部分组成的。由组命令号/组命令标识,分为10进制和10进制两种<br>
ID文档中的组命令号或组命令标识<br>
![public_group_cmd_stand][public_group_cmd_stand]

“单命令/组命令”说明当前指令201001可以既作为单命令发送，也可以运用到组命令中。<br>
![public_group_cmd_sixcode][public_group_cmd_sixcode]


现有ID文档或者标准文档中的组命令号/组命令标识存在16进制和10进制两种，uSDK中执行组命令时要求传递的组命令号必须是10进制6为长度的字符串（字母大写）。开发者在使用10进制的组命令号时需要先将16进制的组命令号转化为10进制（字母大写），然后左端补“0”，形成6位的10进制字符串。<br>

例如ID文档的组命令号19807，转换补位后是“004D5F”，“004D5F”就是uSDK要求的目标组命令号。<br>

例如ID文档的组命令号0x6001，转换为“006001”，“006001” 就是uSDK要求的目标组命令号。<br>

例如ID文档的组命令号4D5F，补位后是“004D5F”，“004D5F”就是uSDK要求的目标组命令号。<br>

命令集合是按ID文档中组命令要求，组合成的单命令的集合。App开发者发送组命令时，需要严格遵守ID文档中的组命令规定，不能添加除ID文档中要求的组命令外的任何命令，且组命令中单命令的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。<br>

示例代码：以组命令号19807为例。

	 uSDKArgument *arg = [[uSDKArgument alloc]init];
    arg.name =@"201000";
    arg.value = @"201000";
    
    uSDKArgument *arg2 = [[uSDKArgument alloc]init];
    arg2.name =@"201002";
    arg2.value = @"201002";
    
    uSDKArgument *arg3 = [[uSDKArgument alloc]init];
    arg3.name =@"201005";
    arg3.value = @"201005";
    
    NSArray* cmdList = [NSArray arrayWithObjects:arg,arg2,arg3 nil];	
    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){
       NSString* groupCmdName = @"004D5F";
       void(^success)(void) =^{
          //命令执行成功
       };
       void(^failure)(NSError* error) = ^(NSError* error){
         //命令执行失败
       };
       [self.currentDevice executeOperation:groupCmdName args:cmdList timeoutInterval:5 success:success failure:failure];
    }else{
       //设备非连接成功或就绪状态,不可交互
        return;
    }

groupName：组命令标识字，NSString类型。长度6位的10进制大写字符串。<br>
cmdList：uSDKArgument对象实例的集合<br>
5：超时时间，单位是秒<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 注意
1．此方法只能发送组命令，组命令要求的所有命令都需要放入cmdList，且没有顺序要求。<br>
2．组命令标识字具体值参考设备的ID文档，需要传入长度6位的10进制大写的字符串。<br>
3．App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。

#### 5.2.3 发送写属性命令（标准文档）
海极网支持创建智能设备，创建完成后会产生《XX设备应用开发文档》，以下将说明如何使用此文档与设备进行交互。

操作属性<br>
操作属性的含义是此项可以作为智能设备的属性，可写列为T时，此项可以作为命令发往设备；可写列为F时，表示此命令不可用，不能作为命令发往设备。
![public_op_attr_stand][public_op_attr_stand]

示例代码：

    [self.currentDevice writeAttributeWithName:name value:value timeoutInterval:5 success:^{
        // do cmd success
    } failure:^(NSError *error) {
        // do cmd failure
    }];

name：属性名称，NSString类型，必须和文档中一致。<br>
value：属性名称对应的可取值，NSString类型，必须和文档中一致。<br>
5：超时时间，单位是秒<br>
代码块success：命令执行成功时触发。<br>
代码块failure：命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，  error.localizedDescription为错误码的文字描述。

注意<br>
1、发送指令时Key和Value大小写敏感（例如：”True”）。<br>
2、可写列为F时，此行不能作为指令发送。

#### 5.2.4.发送操作命令/组命令（标准文档）
操作，是一组属性的集合，即是您在海极网创建的高级命令， 需要使用发送组命令的形式发送。
##### 操作形式一：
![public_stand_cmd_getAllProperty][public_stand_cmd_getAllProperty]

示例代码：

    NSArray* cmdList = [[NSArray alloc]init];
    NSString *groupCmdName =  @"getAllProperty";
    [self.currentDevice executeOperation:groupCmdName args:cmdList timeoutInterval:5 success:^{
        //do group cmd success
    } failure:^(NSError *error) {
        //do group cmd failure
    }];
groupName：操作命令名称，NSString类型。<br>
cmdList：uSDKArgument对象实例的集合。<br>
5：超时时间，单位是秒<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 操作形式二：
![public_stand_op_cmd_2][public_stand_op_cmd_2]

示例代码：

    uSDKArgument *arg = [[uSDKArgument alloc]init];
    arg.name =@"tempPwdLengthX";
    arg.value = @"10";
    
    uSDKArgument *arg2 = [[uSDKArgument alloc]init];
    arg2.name =@"tempPwdPart1X";
    arg2.value = @"3";
    
    uSDKArgument *arg3 = [[uSDKArgument alloc]init];
    arg3.name =@"tempPwdPart2X";
    arg3.value = @"3";
    
    NSArray* cmdList = [NSArray arrayWithObjects:arg,arg2,arg3 nil];
    NSString *groupCmdName =  @"Tmpopen";
    [self.currentDevice executeOperation:groupCmdName args:cmdList timeoutInterval:5 success:^{
        //do group cmd success
    } failure:^(NSError *error) {
        //do group cmd failure
    }];
groupName：操作名称，NSString类型。<br>
cmdList：uSDKArgument对象实例的集合。不能为nil<br>
5：超时时间，单位是秒<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。




## 6 获取设备属性/报警
完成连接设备操作且变为就绪状态后，App就能实时获得设备的最新属性和报警信息。本章主要讲解：实现获得设备变化的属性状态接口方法，获得设备的所有属性状态、获得设备设备报警信息等内容。

### 6.1获得设备属性
App开发者成功连接设备并完成uSDKDeviceDelegate设置及实现委托，通过如下方法获得设备的属性值集合推送； 
#### 6.1.1 获得设备变化的属性

##### 设置委托：

    self.currentDevice.delegate = self;

##### 实现委托：

    -(void)device:(uSDKDevice *)device didUpdateValueForAttributes:(NSArray<uSDKDeviceAttribute *> *)attributes{
      self.attrDict = self.currentDevice.attributeDict;
      [self.myTableview reloadData];
    }

attributes：第一次收到attributes时，它包含该设备所有的属性值的全集，之后收到的是变化的属性集合。

#### 6.1.2 获得设备所有属性
当设备就绪或已连接状态时，uSDKDevice对象的attributeDict属性中保存设备当前最新属性值合集，非就绪状态属性返回值无意义。<br>
示例代码：

    self.currentDevice.attributeDict;

### 6.2 获得设备报警及报警处理
设备就绪或已连接状态下，智能设备或家电发生障或警告时，uSDK会将收到的报警信息即时推送给App。<br>

#### 6.2.1 获取设备报警信息
App开发者需要成功连接设备并实现uSDKDeviceDelegate委托并设置委托，开发者可以通过uSDKDeviceAlarm类查看设备报警信息。<br>


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
          
        }
      }
    }
alarms：设备当前的报警信息列表，建议将重要的报警信息呈现给使用者。


##### 获取设备所有报警
App可以调用uSDKDevice的API获取设备缓存的所有报警信息

    NSArray<uSDKDeviceAlarm*>* alarmList = self.currentDevice.alarmList;
self.currentDevice：uSDKDevice对象，App查询设备是否有报警信息<br>
alarmList：设备当前的报警信息列表

#### 6.2.2 收到报警后的处理方式
当智能设备或家电报警时，会向uSDK规律性连续发送报警信息，开发者需要将报警信息展示给APP使用者，由使用者决定是否可以向设备发送停止报警指令，若使用者已经了解设备已经发生故障，需要发送停止报警命令给设备，停止报警的指令参考设备的ID文档。




## 7 接收U+平台消息推送
经过上述开发步骤后，SDK已经与U+平台建立了数据通路，APP开发者需要实现uSDKManagerDelegage委托并设置委托，可以接受U+平台推送的各种消息，包括：业务消息、Session异常消、绑定/解除绑定消息。

##### 设置委托
开发者需要首先设置委托，然后分别实现不同的委托，就可以获得各自的推送消息。
	
	uSDKManager *uSDKMgr = [uSDKManager    defaultManager]; uSDKMgr.delegate = self;
	
### 7.1接收业务消息推送

设置uSDKManagerDelegage委托后，通过实现如下方法，获得业务消息推送。<br>
实现委托：

    -(void)uSDKManager:(uSDKManager *)sdkManager businessMessage:(NSString *)businessMessage{

    }

sdkManager 当前uSDKManager对象<br>
businessMessage 当前推送的业务消息

### 7.2接收Session异常消息推送
设置uSDKManagerDelegage委托后，通过实现如下方法，获得Session异常消息推送。

实现委托：

    -(void)uSDKManager:(uSDKManager*)sdkManager sessionException:(NSString*)token{

    }

sdkManager：当前uSDKManager对象<br>
token：当前会话失效的token

### 7.3 接收绑定/解除绑定消息
同一帐号、在手机A和B上绑定同一台设备的时，在手机上A绑定/解绑设备后，手机B将会收到云平台推送的绑定/解除绑定消息。

##### 7.3.1 接收绑定消息推送
设置uSDKManagerDelegage委托后，通过实现如下方法，获得绑定消息推送。<br>
实现委托：

    -(void)deviceManager:(uSDKDeviceManager *)deviceManager didBindDevice:(NSString *)deviceID{

    }
deviceManager： 设备管理器对象<br>
deviceID :  绑定设备的ID

##### 7.3.2 接收解除绑定消息推送
设置uSDKManagerDelegage委托后，通过实现如下方法，获得解除绑定消息推送。<br>
实现委托：

    -(void)deviceManager:(uSDKDeviceManager *)deviceManager didBindDevice:(NSString *)deviceID{

    }
deviceManager： 设备管理器对象<br>
deviceID :  解除绑定设备的ID

注意<br>
同一token不能在多个APP上使用，否则会造成消息推送丢失、属性信息推送丢失的情况。


## 8 授权设备
移动端SDK能够对SmartDevice SDK接入设备进行授权的一种能力，该设备被授权后，具备控制其他设备的能力。

##### 授权设备前提条件：
1.用户登录并连接接入网关<br>
2.被授权设备已被绑定<br>
3.被授权设备为已连接或就绪<br>
4.所有涉及设备均绑定在帐号下<br>

### 8.1 判断设备是否具备被授权的能力

    NSString *t =@"mqttUGWAuth";
    if(device.softwareType==t){
    	//判断设备是否被授权过
    }

### 8.2 设备是否被授权

    [device getDeviceAuthStateSuccess:^(BOOL authState) {
         if(authState==NO){
            //调用授权方法
         }
     } failure:^(NSError *error) {
            //分析失败原因
     }];
success 获取成功的回调authState为YES时 代表已经授权，为NO时 代表未授权
failure 失败的回调

### 8.3 执行授权操作

    [device authToDeviceSuccess:^{
       //设备授权成功
    } failure:^(NSError *error) {
        //设备授权失败
    }];
success，授权成功时触发
failure，失败时触发

###  8.4 撤消授权

    [device authToDeviceInvalidSuccess:^{
        //取消授权成功
    } failure:^(NSError *error) {
        //取消授权失败，可以重试
    }]; 


##  9 断开设备连接
当开发者不关注某台设备属性数据时，退出APP时、切换帐号登录时，需要执行断开连接设备方法，释放设备资源。
只支持单个设备断开连接，不支持同时断开连接多个设备，如有需要请逐个方法调用。

    [self.currentDevice disconnectWithSuccess:^{

    } failure:^(NSError *error) {

    }]; 


## 10 退出uSDK
App需要退出或者不需要使用U+物联功能时需要停止uSDK，减少系统资源消耗。

    [[uSDKManager defaultManager]stopSDKWithSuccess:^{

    } failure:^(NSError *error) {

    }];

##  11 解除设备绑定关系

当该用用户与已绑定的设备不需要这种关联关系时，用户需要对该设备进行解绑操作，解除关联关系。解除绑定后的设备，将不再具备远程的控制能力。<br>

解除绑定关系前，需要断开和设备连接。

使用uAccount解除绑定方法内部具有重试机制，为异步方法，不会阻塞线程，执行结果会在回调函数中以参数形式返回。

示例代码：

    [[uAccount defaultUAccount]unbindDeviceWithDeviceId:deviceID success:^(RespCommonModel *successModel) {
        //解除绑定成功
    } failure:^(RespCommonModel *failureModel) {
       //解除绑定失败
    } httpError:^(RespCommonModel *httpErrorModel) {
       //Http请求发生异常
    }];
代码块success，执行成功时触发，successModel返回的具体信息。<br>
代码块failure，执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>
代码块httpError 网络异常时触发<br>

<!--### 5.2 使用UWS云服务解除设备绑定
UWS服务是U+平台对外提供的服务，关于解除设备绑定的详情请见[海极网.UWS服务](https://www.haigeek.com/developercenter/static/develop.html#/devloperTools/)-->

<!--### 5.2  异常处理
设备进行绑定时的加密信息来源与 U+云，app调用bindDevice接口时，由于时间或网络因素，智能设备可能还没有成功连接到 U+云，开发者可以参考下列信息处理异常: 
![public_get_bindinfo_error_code][public_get_bindinfo_error_code]-->


## 12 海外业务开发指引
到目前为止国际版uSDK中能够支撑欧洲和美国两个环境，通过配置不同的参数，使SDK连接到不同的服务器。<br>

### 12.1 设置配置文件下载地址
uSDK启动前，需设置配置文件下载地址，使uSDK能够从指定的海外数据中心下载配置文件使设备连接成功或就绪、可控。

##### 配置文件地址

    1、美国：https://standardcfm-gea-us.haieriot.net:443/hardwareconfig/config/getDownUrlByFormat
    2、欧洲：https://standardcfm-gea-euro.haieriot.net:443/hardwareconfig/config/getDownUrlByFormat

示例代码：

    uSDKErrorConst result =   [[uSDKManager defaultManager]setProfileServiceUrl:url];
      if(result!=RET_USDK_OK){
      //重新设置配置文件服务器的地址
    }
url：下载配置文件服务器的地址，参数不能为@""或nil，长度不超过128，且必须以http或者https开头.<br>
result：执行结果返回值，如果不等于RET_USDK_OK，程序不能向下执行

### 12.2 连接海外用户网关
开发海外远程功能时，需要连接不同地域使用的用户接入网关地址和端口不同，如在美国需要使用的美国数据中心，用户接入网关地址和端口应该是美国数据中心的地址和端口。

##### 海外用户网关地址和端口：
1、美国：

    网关地址：gw-gea-us.haieriot.net
    网关端口：56815

2、欧洲：

    网关地址：gw-gea-euro.haieriot.net
    网关端口：56815
#####  执行连接海外用户网关  
示例代码：

    [[uSDKDeviceManager defaultDeviceManager]connectToCloudWithDevices:devList token:remoteSession gatewayDomain:gatewayDomain gatewayPort:gatewayPort success:^{

    } failure:^(NSError *error) {

    }];
connectToCloudWithDevices：连接用户接入网关，使设备具备远程控制能力<br>
deviceList：远程设备列表。<br>
remoteSession +云账号登录后的accessToken。<br>
gatewayDomain和gatewayPort：用户接入网关的域名和端口。<br>
代码块success执行成功时被触发。<br>
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>


### 12.3 SoftAP配置设备
已出厂的海外模块中已经烧写了指定海外数据中心地址,大多情况下不需要设置主网关地址来更改模块连接的数据中心，如需更改，通过softap时设置模块的主网关地址和端口的方法实现。<br>


SoftAp配置接口，该接口可在softApConfigInfo参数中配置超时时间，在APP进入后台时是否计时。 uSDK 4.5.01新增。<br>
示例代码：

    [self.view endEditing:YES]; //实现该方法是需要注意view需要是继承UIControl而来的
    uSDKSoftApConfigInfo *cfgInfo = [uSDKSoftApConfigInfo init];
    cfgInfo.mainGatewayDomain = usMainGatewayDomain;
    cfgInfo.mainGatewayPort = usMainGatewayPort;
    cfgInfo.security = YES;
    cfgInfo.timeoutInterval = 60;
    cfgInfo.ssid = ssid;
    cfgInfo.password =pwd;
    
    [[uSDKDeviceManager defaultDeviceManager] configDeviceBySoftapWithConfigInfo:cfgInfo sendConfigInfoSuccess:^{
        [AlertViewTools shouAlertViewWithTitle:@"提示"
                                           Msg:@"配置信息发送成功，请切换到目标网络"];
    } success:^(uSDKDevice *device) {
        //配置成功的设备
        [self showFindDevice:device];
        if(delegate.isLogin){
            [DemoUtils bindDevice:device];
        }
    } failure:^(NSError *error) {
        self.configResultLable.text = @"设备配置失败!";
        NSString *info = [NSString stringWithFormat:@"%ld", (long)error.code] ;
        [AlertViewTools shouAlertViewWithTitle:@"设备配置失败" Msg:info];
    }];
usMainGatewayDomain：模块主网关地址<br>
usMainGatewayPort：网关端口<br>
oftApConfigInfo : uSDKSoftApConfigInfo对象，在此对象中设置SSID、密码、超时时间等参数<br>
代码块sendConfigInfoSuccess，向设备发送配置信息成功时触发，开发者需要连接到配置的目标网络，然后等待设备上线。<br>
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。





[^-^]:常用图片注释
[SingleCmd]:img/SingleCmd.png
[public_group_cmd_sixcode]:img/public_group_cmd_sixcode.png
[public_group_cmd_stand]:img/public_group_cmd_stand.png
[public_op_attr_stand]:img/public_op_attr_stand.png
[public_stand_cmd_getAllProperty]:img/public_stand_cmd_getAllProperty.png
[public_stand_op_cmd_2]:img/public_stand_op_cmd_2.png
[public_user_gateway_dev_online]:img/public_user_gateway_dev_online.png
[public_get_bindinfo_error_code]:img/public_get_bindinfo_error_code.png
[connectstatus_change_step]:img/connectstatus_change_step.png

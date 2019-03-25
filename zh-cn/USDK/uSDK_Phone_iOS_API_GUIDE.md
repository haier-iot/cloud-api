

**当前版本**：[uSDK_Phone_iOS V5.0]
**更新时间**：{docsify-updated}

## 1.uSDK启动
启动uSDK是调用各种功能性API，使用U+物联功能的前提，在主线程中启动一次即可。下面分别讲解启动uSDK及启动前后前后需要进行的必要设置：设置设备管理委托、设置过滤设备类型、启动uSDK、成功后设定uSDK日志级别、开启和关闭dns防劫持功能。

相关概念和术语<br>
AppID：在海极网为物联App申请的AppId，用于App校验<br>
AppKey：在海极网申请，用于与UWS或OPEN API交互时使用。APP开发中只有“开发测试” 的APP_KEY。开发完成且提交海极网审核完成后会发放“生产” 的APP_KEY，APP使用生产的APP_KEY进行发布。<br>
SecretKey：在海极网申请，uSDK使用

### 1.1设置设备管理委托
uSDK启动前，APP开发者需要实现uSDKDeviceManagerDelegage委托并设置委托，才能在uSDK成功启动后，得到变化的设备集合，对设备列表集进行管理，详情见1.3章节 管理设备集变化中的管理设备列表集合。

    [uSDKDeviceManager defaultDeviceManager].delegate = self。

### 1.2设置过滤设备类型
uSDK启动前，设置开发者关心的设备类型，可以是一个，也可以是多个；若不设置，默认为全部类型。设置过滤类型后，只能收到或查询到自己关心类型的设备，其他类型的设备将被过滤掉。多次执行[uSDKDeviceManager defaultDeviceManager]. interestedDeviceTypes进行赋值，将覆盖已设定的设备过滤类型，以最后一次为准。<br>
示例代码：

    [uSDKDeviceManager defaultDeviceManager].interestedDeviceTypes = ALL_TYPE

### 1.3启动uSDK
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

### 1.4.设置uSDK日志级别
uSDK启动成功后，可以设置uSDK的日志级别，开发过程中建议使用USDK_LOG_DEBUG，上线产品建议使用USDK_LOG_NONE或USDK_LOG_ERROR，如不设置，默认为USDK_LOG_DEBUG输出所有日志。uSDK运行时会输出日志，其中包含与硬件交互及反馈给App的详细日志，uSDK的日志标签是uClient和uServer。<br>
示例代码：

    [[uSDKManager defaultManager] setLogWithLevel:USDK_LOG_DEBUG isWriteToFile:NO success:^{

    } failure:^(NSError *error) {

    }];

代码块success方法执行成功时被触发。<br>
代码块failure方法执行失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

### 1.5 开启/关闭dns防劫持功能
为了保证用户能够正确访问到海尔云，uSDK中提供了dns防劫持功能， 默认此功能开启。
当开发人员需要使用非生产环境（开发者、联调、验证）进行开发测试时，在sdk启动后关闭uSDK提供的DNS防劫持功能，才能保证 uSDK正常访问对应的云环境。<br>
当APP需要对外发布时，需要开启DNS防劫持功能，保证用户能够正确访问到海尔云。
API介绍如下：

1、开启全部特性

    [[uSDKManager defaultManager] enableFeatures:uSDKFeatureDefault];

2、关闭全部特性

    [[uSDKManager defaultManager] enableFeatures:uSDKFeatureNone];



## 2 退出uSDK
App需要退出或者不需要使用U+物联功能时需要停止uSDK

    [[uSDKManager defaultManager]stopSDKWithSuccess:^{

    } failure:^(NSError *error) {

    }];

##  3配置设备入网
配置设备入网就是使用uSDK将设备加入指定无线网络，或更改U+设备所在网络的一项操作。uSDK支持SmartLink、SoftAP两种方式入网，下面讲分别进行讲解。<br>
uSDK4.X支持您使用加密方式发送配置信息，如果您的智能产品包含不支持加密入网的型号，则使用普通配置方式，以保证所有销售产品都能正常入网。如果产品全部支持加密入网，建议使用加密入网方式，提升整套产品安全性能。

预备知识<br>
请先了解章节1.动uSDK。配置入网演示视频请至U+开发者QQ群下载。

相关术语和概念<br>
uPlug：负责智能设备或家电联网的WIFI物联集成电路板。

###  3.1.SmartLink配置方式
SmartLink方式是成熟稳定的设备入网方式，是U+平台物联设备的主流入网方案，通过将无线配置信息发送给待入网设备，完成配置入网动作。<br>
uSDKDeviceManager.configDeviceBySmartLinkWithSSID有多个重载方法，可以指定MAC、TYPEID、超时时间、安全配置等参数，完成不同需求的配网功能。

#### 设备配置入网操作过程
1)    确认当前路由器频段是2.4g。按U+智能设备说明书指导，操作设备进入配置模式，此时uPlug指示灯有规律快闪，实体设备会有相应的指示灯闪烁，说明设备进入待入网模式。<br>
2)    确认手机稳定连接目标路由器，设备安置位置WIFI信号良好，打开基于uSDK的App（通用测试App、uSDKDevDemo程序），将配置入网信息发送出去。设备收到配置信息后熄灭信号指示或发出蜂鸣声，手机不切换网络等待设备上线。<br>
3)    App显示有新设备上线，说明配置成功。App提示配置失败，说明操作失败，重新按照1，2，3步骤进行。

#### 执行SmartLink配置方式
App把无线的名字和密码发给待入网的设备，示例代码如下：

    void(^sucess)(uSDKDevice* device) = ^(uSDKDevice* device){
    }
    void(^failure)(NSError* error) = ^(NSError* error){ 
    };
    [[uSDKDeviceManager defaultDeviceManager]configDeviceBySmartLinkWithSSID:ssid password:pwd deviceID:mac timeoutInterval:60 security:NO success:sucess failure:failure];

configDeviceBySmartLinkWithSSID：配置设备入网方法。<br>
ssid：无线网络名称，不能为空，最大长度31。<br>
pwd：无线网络密码，可以为空，最大长度63。<br>
mac：设备mac地址。不知道的话，填写@”";<br>
timeoutInterval：配置超时时间，单位为秒。范围5-120秒，推荐60秒。<br>
security：安全配置方法标识。YES时，进行安全配置；NO时，普通配置。<br>
代码块success配置成功时触发，device不为nil，设备配置入网成功，device携带设备信息。<br>
代码块failure配置失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 判断设备入网是否成功及获得设备实例
配置成功，代码块success被触发，device不为nil说明设备入网成功，它就是入网设备对象实例，此时它携带设备网络及产品类型等基本信息。

##### 中断设备配置入网
在执行配置设备入网过程中可以调用API中断，中断动作结果将通过回调参数通知App。

    [[uSDKDeviceManager defaultDeviceManager]stopSmartLinkConfig:^{

    } failure:^(NSError *error) {

    }];
代码块success中断配置成功时触发。<br>
代码块failure中断配置失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

###  3.2 SoftAP配置方式
SoftAP配置方式是将uPlug设置为WIFI热点，手机连接uPlug热点，然后把无线配置信息发送给uPlug的一种入网方式。

##### 步骤一 连接设备热点
参考使用说明让设备进入待配置模式，此时物联模块工作于WIFI热点模式，打开手机无线局域网，可以看到设备热点（U-开头），将手机连接到设备的WIFI热点。

##### 步骤二 创建uSDKSoftApConfigInfo对象
创建uSDKSoftApConfigInfo对象并设置必要的基本信息，用于传递给设备热点。
开发者需要向configInfo对象中写入正确的wifi名称（不支持中文，最小长度1，最大长度31）和密码（不支持中文，可以为空，最大长度63）才能保证设备是可以配置成功的。

    uSDKSoftApConfigInfo * cfgInfo = [uSDKSoftApConfigInfo init];
    cfgInfo.security = NO;
    cfgInfo.timeoutInterval = 60;
    cfgInfo.ssid = ssid;
    cfgInfo.password =pwd;

##### 步骤三 执行SOFTAP配置
SoftAp配置接口，开发者无需再调用getSoftapDeviceConfigInfo接口。该接口整合了获取配置信息(getSoftapDeviceConfigInfo)和发送配置命令两个步骤，并且两个步骤均提供了重试功能，方便开发者使用。该接口可在softApConfigInfo参数中配置超时时间在APP进入后台时是否计时。 uSDK 5.01新增<br>
示例代码：

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

cfgInfo：装载配置信息的对象<br>
sendConfigInfoSuccess： 配置信息发送成功时被触发，需要切网到目标网络<br>
success：配置成功时被触发，device是配置成功的设备。<br>
Failure：配置失败时被触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

##### 步骤四 切换回配置目标网络
代码块sendConfigInfoSuccess执行成功后，需要切换回目标路由的网络，等待设备上线。<br>

配置失败可能原因：网络原因、无线密码输入错误，或者写入无线密码并没有成功。

##  4.管理设备集变化
uSDK启动完成后会不断扫描本网络里的U+设备，完成设备发现、设备离线，维护设备集合中设备数量及数据的变化，App通过实现设备列表变化回调实现设备集合变化的管理。本章分别讲解：管理变化设备列表集合、获得设备池全集的方法。

相关概念和术语<br>
小循环：uSDK和U+设备处于同一无线局域网完成设备交互的情形。<br>
TYPEID：TYPEID就是设备类型的标识字符串，用它可以识别U+平台上的各种硬件设备。
设备连接状态<br>
未连接：App还没连上智能设备，表示设备加入WIFI已发现。调用uSDKDevice的state 属性值是uSDKDeviceStateUnconnect。<br>
离线：设备发现过并且App进行了连接，但此时无法收到设备的响应数据。uSDKDevice的state属性值是uSDKDeviceStateOffline。<br>
设备的属性状态（属性）：开机、关机、运行等值。

##### 管理变化设备列表集合
uSDK会将网络里所有的智能设备都管理起来，形成一个设备列表的集合，并进行管理。设备列表集合的数量会根据实际设备的数量变化而发生变化，例如：配置一台新设备入网并上线，集合中会增加一台设备；设备切断电源然后下线，集合中会减少一台设备。<br>
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


##  5.管理设备连接状态变化
当设备状态（uSDK与设备连接状态）发生变化时，uPlug模块或云平台会及时的通知uSDK，uSDK会上报收到的设备状态给APP，用于更新UI界面或进行其他业务处理。设备连接状态分为：未连接、连接中、已连接（连接成功）、就绪、离线，五个状态值。关于设备状态值的具体定义，请参考 uSDK与设备连接状态值定义<br>
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

## 6    与设备建立或断开连接
发现设备且设备处于未连接状态时，App连接或断开智能设备将触发连接状态的变化。本章分别讲解：执行连接设备、执行断开设备连接操作。

### 设备的5种连接状态：
未连接状态：智能设备正常入网后被uSDK发现时的状态<br>
连接中状态：执行连接智能设备，智能设备变为连接中<br>
已连接状态:与智能设备连接成功后，变为已连接状态<br>
就绪状态：调用连接查询属性的方法，与设备连接成功后收到上报属性后的状态<br>
离线状态:连接失败或通信中断会变为离线

![connectstatus_change_step][connectstatus_change_step]


### 6.1 执行连接设备
当需要获得某台设备属性数据或进行命令功能控制时，需要先执行设备连接方法。目前uSDK只支持连接单个设备，不支持同时连接多个设备，如有需要请逐个方法调用。<br>

开发者执行设备连接操作前，可以通过设备的isSubscribed属性对设备的状态进行判断，为NO时进行连接操作，如果是YES，不需要再次对设备执行连接操作。

#### 6.1.1、连接设备但不获得设备属性方法：
使用uSDKDevice对象执行该方法，该方法执行成功后，需要等待一定时间App的设备连接状态将变为“连接成功/已连接”状态，不会出现“就绪” 状态。

    [self.currentDevice connectWithSuccess:success{

    } failure:{

    }];
代码块success执行成功时触发，等待一定时间后设备状态变为“已连接或连接成功”状态。<br>
代码块failure执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述


#### 6.1.2、连接设备并获得设备属性方法：
执行连接设备并获得设备属性方法，该方法执行成功后，等待一定时间设备会变为“就绪” 状态，设备就绪状态时可以执行设备控制和获取的设备的属性集合。

    [self.currentDevice connectNeedPropertiesWithSuccess:^{

    } failure:^(NSError *error) {

    }];

代码块success执行成功时触发，等待一定时间后设备状态将变为“就绪” 状态。<br>
代码块failure执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>


### 6.2 断开设备连接
不关注某台设备属性数据时，执行断开连接设备方法，释放设备资源。只支持单个设备断开连接，不支持同时断开连接多个设备，如有需要请逐个方法调用。

    [self.currentDevice disconnectWithSuccess:^{

    } failure:^(NSError *error) {

    }];

### 6.3 获取设备的连接状态
通过uSDKDevice的state属性，获取设备的连接状态

注意<br>
1．uSDK启动成功后，App可以在任意时刻执行连接设备操作（设备对象引用不为nil）。<br>
2．开发中一台设备只执行一次设备连接操作即可，多次执行连接操作将使设备下线再上线。<br>
3．断开某设备连接后，uSDK将不再上报此设备的任何属性消息给App。<br>


## 7 管理设备属性状态变化
设备就绪后，App就能实时获得设备的最新属性值集合。本章主要讲解：实现获得设备的上下线状态接口方法，实现获得设备的属性状态接口方法，主动获得设备的所有属性状态等内容。

相关术语<br>
六位码：六位码是一个键值对，在程序当中是对象uSDKDeviceAttribute，App使用它和设备进行沟通交流。举例：20w001代表开关机功能，App通过uSDK发送<br>uSDKDeviceAttribute｛20w001:20w001｝就是开机，发送<br>uSDKDeviceAttribute｛20w001:20w002｝关机，查询设备属性时<br>uSDKDeviceAttribute｛20w001：20w001｝说明电视开机。<br>
ID开发文档：六位码的集合文档，主要用途是明确设备操作指令及属性集等。

### 实现获得设备的属性状态方法
App开发者成功连接设备操作并实现uSDKDeviceDelegate委托并设置委托，通过如下方法获得设备的属性值集合推送； 
### 设置委托：

    self.currentDevice.delegate = self;

### 实现委托：

    -(void)device:(uSDKDevice *)device didUpdateValueForAttributes:(NSArray<uSDKDeviceAttribute *> *)attributes{
      self.attrDict = self.currentDevice.attributeDict;
      [self.myTableview reloadData];
    }

attributes：第一次收到attributes时，它包含该设备所有的属性值的全集，之后收到的是变化的属性集合。

### App主动获得当前设备所有属性值
当设备就绪或已连接状态时，uSDKDevice对象的attributeDict属性中保存设备当前最新属性值合集，非就绪状态属性返回值无意义。<br>
示例代码：

    self.currentDevice.attributeDict;

##    8.设备故障和报警
设备就绪或已连接状态下，智能设备或家电如果存在故障或警告，uSDK会即时将消息推送给App。设备报警信息用uSDKDeviceAlarm承载。报警信息六位码具体含义参考相关ID文档。本章讲解：获得报警消息、发送停止报警指令、获得报警解除消息、主动查询设备报警等内容。

预备知识<br>
与设备建立连接，使设备就绪请参考章节单命令及如何向设备发送指令。

### 获得报警消息
App开发者需要成功连接设备并实现uSDKDeviceDelegate委托并设置委托，通过如下方法，可以获得报警消息；
### 设置委托：

    self.currentDevice.delegate = self;

### 实现委托：

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

### 发送停止报警指令
当智能设备或家电报警时，其可能向uSDK规律性快速报警，App通知用户或记录信息之后，可以向设备发送停止报警指令，用于表示使用者已经了解设备已经发生故障。停止报警对于App来讲只是一条普通单命令，停止报警的指令参考设备的ID文档。

### 获得报警解除消息
发生故障的设备修理正常后会向uSDK发送报警解除消息，对于App来讲报警解除就是没有报警，就是设备正常。报警解除消息与普通报警消息都在-(void)device:(uSDKDevice *)device didReceiveAlarms:(NSArray<uSDKDeviceAlarm*>*)alarms方法中处理，App注意分辨。报警解除的六位码请参考设备ID文档。

### 主动查询设备报警
App可以调用API主动查询设备报警信息

    NSArray<uSDKDeviceAlarm*>* alarmList = self.currentDevice.alarmList;
self.currentDevice：uSDKDevice对象，App查询设备是否有报警信息<br>
alarmList：设备当前的报警信息列表

##  9.执行设备控制
海尔智能设备至今大多使用ID文档进行开发，通过6位码的键值对方式对海尔智能设备控制，可以给设备发送单命令，也可以发送组命令，可以设定超时时间，命令执行后会有结果与设备反馈。<br>

随着U+平台的日趋开放,为了兼容来自不同厂家的各类物联设备及终端应用软件，海尔制定了统一的物联设备建模标准,对各种物联设备的基本属性、功能等信息进行规范化的描述，最终形成标准设备模型文档，以下简称标准文档 。<br>

标准文档是海尔的主推方向，需要和uSDK3.x或uSDK1.x、标准设备，三方面一起使用才能完成对设备的控制。标准文档的使用需要参考标准文档模型和下面的写属性、操作命令进行开发。uSDK2.x不支持标准文档。<br>

设备处于就绪或连接成功状态，是控制设备的必要前提条件。当执行连接设备方法成功后，且设备处于就绪或已连接状态时，可以向设备发送控制指令。核心类uSDKDevice提供API用于执行设备控制。下面将以6位码的ID文档和标准设备模型文档为例，下面将分别讲解单命令和组命令控制。



### 相关术语和概念
TYPEID：TYPEID就是设备类型的标识字符串。uSDK的功能是通用的，它可以识别U+平台各种硬件设备，使用者可以使用TYPEID区分设备类型。

### 控制命令超时设定
uSDK提供的默认控制方法超时时间为15秒，网络及设备良好的情况下瞬间返回。自定义超时时间范围：T（单位：秒）5<= T <=120，App根据自身业务设定超时间。

### 9.1.    发送单命令（6位码）
单命令：App只发送一个指令给设备
![public_single_cmd][public_single_cmd]

App开发者使用uSDKDevice对象发送单命令时，需要严格遵守ID文档规定，发送指定的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。不能使用发送组命令的方法发送单命令。<br>
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
      [self.currentDevice writeAttributeWithName:name   value:value success:success failure:failure];

    }else{
      UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"设备非连接成功或就绪状态,不可交互" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];
      [alertView show];
      return;
    }
name：属性名，NSString类型。如”201.03”<br>
value：属性值，NSString类型。如”201.03”<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。 

### 9.2.发送组命令（6位码）
组命令是由组命令号或组命令标识、单命令集合两部分组成的。<br>
组命令：App发送多条指令的集合，设备接收并执行<br>
![public_group_cmd_sixcode][public_group_cmd_sixcode]

“单命令/组命令”说明当前指令201001可以既作为单命令发送，也可以运用到组命令中。<br>

ID文档中的组命令号或组命令标识： 分为10进制和16进制两种
![public_group_cmd_stand][public_group_cmd_stand]

组命令号/组命令标识是ID文档中已经预定义好的，开发者不允许变更。现有ID文档或者标准文档中的组命令号/组命令标识存在16进制和10进制两种，然而uSDK中执行组命令时要求传递的组命令号必须是16进制6为长度的字符串（字母大写）。开发者在使用10进制的组命令号时需要先将10进制的组命令号转化为16进制（字母大写），然后左端补“0”，形成6位的16进制字符串。<br>

例如ID文档的组命令号19807，转换补位后是“001.5F”，“001.5F”就是uSDK要求的目标组命令号。<br>

例如ID文档的组命令号0x6001，转换为“006001”，“006001” 就是uSDK要求的目标组命令号。<br>

例如ID文档的组命令号1.5f，补位后是“001.5F”，“001.5F”就是uSDK要求的目标组命令号。<br>

命令集合是按ID文档中组命令要求，组合成的单命令的集合。App开发者发送组命令时，需要严格遵守ID文档中的组命令规定，不能添加除ID文档中要求的组命令外的任何命令，且组命令中单命令的key和value，不能随意填写或填空，值不能超过ID文档规定的范围。<br>

示例代码：以组命令号19807为例。

    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){
        NSString* groupCmdName = @"001.5F";
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

groupName：组命令标识字，NSString类型。长度6位的16进制大写字符串。<br>
cmdList：uSDKArgument对象实例的集合<br>
5：超时时间，单位是秒<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

注意
1．此方法只能发送组命令，组命令要求的所有命令都需要放入cmdList，且没有顺序要求。<br>
2．组命令标识字具体值参考设备的ID文档，需要传入长度6位的16进制大写的字符串。<br>
3．App需要严格遵守ID文档组命令的具体规定，不能随意添加或者减少指令，命令格式中要求的指令key和value不能随意填空。



### 9.3    写属性命令（标准文档）
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

### 9.4.操作命令/组命令（标准文档）
操作，是一组属性的集合，即是您在海极网创建的高级命令， 需要使用发送组命令的形式发送。
操作形式一：
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
cmdList：uSDKArgument对象实例的集合。不能为nil<br>
5：超时时间，单位是秒<br>
代码块success命令执行成功时触发。<br>
代码块failure命令执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

操作形式二：
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

注意<br>
App需要严格遵守ID文档或应用开发文档的规定，命令格式中要求的指令key、value不能随意填空，值不能超过文档规定的范围。


## 10 实现设备远程控制
经过前面的开发，我们已经可以在本地和智能设备完美交互了，但是我们的手机不能切换路由或者使用1.，如果这么做和智能设备的数据通路会立刻切断。现在我们来介绍手机更换WIFI或使用1.时，如何让App连接远程服务器，进行设备设备远程控制、获取设备状态等。

### 本章将讲解以下内容
1.连接用户接入网关时机<br>
2.用户账号有绑定设备时连接用户接入网关<br>
3.连接用户接入关后新绑定设备添加远程控制能力<br>
1.设备解绑时解除设备远程能力<br>
5.如何测试远程功能是否正常<br>

### 相关概念及术语
1.用户接入网关：U+云支持App实现远程功能的服务器软件系统。App编程使用如下服务器地址和端口：
![public_user_gateway_dev_online][public_user_gateway_dev_online]

2.U+云UWS或OPEN API：U+云为开发人员提供的网络API，主要和用户账号等业务系统交互（UWS或OPEN API开发文档请从海极网下载）。<br>

3.accessToken(session)：App用户账号登录后UWS或OPEN API分配。
绑定、设备绑定：由App发起将用户及所属设备数据上送到云并创建设备数据档案的过程，设备能够被远程控制的前提是已经被用户绑定。<br>

1.切网：用户由设备所在A路由切换到B路由，或改用数据网络的行为。<br>

5.小循环VS大循环：它们是两种情景的描述。小循环指的是App与智能设备在同一无线局域网。大循环是说App需要借助U+云用户接入网关才能和设备进行交互的情景，此时App与设备通常不在同一网络。

### 10.1v连接用户接入网关调用时机
连接用户接入网关操作是使设备具备远程能力的必要步骤，APP开发者在开发过程中需要在三个地方需要调用连接用户接入网关操作：1、登录成功后；2.绑定设备成功后；3.解绑定设备成功后。

#### 1.新绑定设备的处理：
通过UWS或OPEN API绑定一台新设备后，开发者需要在现有用户帐号下的设备列表中增加已绑定的新设备或者重新获取用户帐号下的设备列表，将新的设备列表作为参数重新执行连接用户接入网关方法，确保已添加的新设备远程可用。

####   2.解绑设备的处理：
通过UWS或OPEN API解绑定一台设备后，需要调用断开连接用户接入网关方法将该设备断开连接，开发者需要从现有用户帐号下的设备列表中删除已解绑的设备或者重新获取用户帐号下的设备列表，将新的设备列表作为参数重新执行连接用户接入网关方法，确保已解绑的设备远程不再具备远程能力。

###  10.2 连接用户接入网关调用步骤
通过执行uSDKDeviceManager的connectToCloudWithDevices方法连接用户接入网关，此方法需要几个参数：session就是U+云账号登录后的accessToken；设备信息集合就是用户拥有哪些设备，我们需要做的就是把这些参数凑齐然，参考章节5的图示适时运行方法即可。

#### 步骤一、获取用户名下的用户设备列表
通过U+平台的UWS或OPEN API 提供的方法获取用户名下的用户设备列表json：以燃气热水器为例，我们要使用三个红色加粗字段，
{"id":"0007A88A527B","status":{"online":true},"location":{"cityCode":null,"longitude":"0.0","latitude":"0.0"},"attrs":{"brand":null,"model":null},"name":"燃气热水器_527B","mac":"0007A88A527B","type":{"type":"18","typeIdentifier"
:"111c120021.008101801.01.80021.0000000000000000000000000000000000","specialCode":"001.80021.","subType":"01.},"version":{"eProtocolVer":"2.15","smartlink":{"smartLinkSoftwareVersion":"e_0.1.36","smartLinkHardwareVersion":"G_1.0.00","smartLinkPlatform":"UDISCOVERY_UWT","smartLinkDevfileVersion":"0.0.0"}}}

#### 步骤二、构建需要上传的数据
使用步骤1的三个标红字段，为用户账号下的每台设备都生成一个uSDKDeviceInfo，每个字段都不能为空和随意填写。<br>
示例代码：

    NSMutableArray* remoteDevices = [[NSMutableArray alloc] init];
    uSDKDevice* dev = [[uSDKDevice alloc]initWithDeviceID:item.id uplusID:item.typeInfo.typeId isOnline:item.status];
    [remoteDevices addObject:dev];

#### 步骤三、执行连接用户接入网关方法
开发过程中的项目使用开发环境，上线产品需要使用生产环境。如果智能设备和手机APP使用了不同环境，将导致远程设备离线、不可控制。<br>
示例代码：

    NSArray* devList = [NSArray arrayWithArray:appDelegate.remoteDevicesList];
       
    [[uSDKDeviceManager defaultDeviceManager]connectToCloudWithDevices:devList token:remoteSession gatewayDomain:gatewayDomain gatewayPort:gatewayPort success:^{
        
    } failure:^(NSError *error) {
        
    }];

connectToCloudWithDevices：连接用户接入网关，使设备具备远程控制能力<br>
deviceList：远程设备列表。<br>
remoteSession +云账号登录后的accessToken。<br>
gatewayDomain和gatewayPort：用户接入网关的域名和端口。开发者需要注意智能设备和手机APP是否使用了相同的环境。<br>
代码块success执行成功时被触发。<br>
代码块failure启动失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

注意：
1、用户登录后，例如一位新用户，从云拉取用户设备列表为空时，deviceList不能为nil，可以是长度为0的NSArray对象.<br>
2、如果当用户只关心业务消息推送，不关心设备远程控制时，deviceList不能为nil, 可以是长度为0的NSArray对象.<br>


### 10.3获取连接用户接入网关的状态
站在APP开发者使用的角度来看，可以分为主动获取用户接入网关连接状态和被动接收用户接入网关连接状态两种方式。

#### 1、主动获取用户接入网关连接状态
uSDK启动成功后，uSDK会对用户接入网关连接状态进行维护，可以在任意时刻通过如下方法获取，具体状态值见7.1.17章节 uSDK与云平台连接状态值定义。

    uSDKCloudConnectionState *cloudState =   [uSDKDeviceManager defaultDeviceManager].cloudConnectionState;
 

#### 2、被动接收用户接入网关连接状态
APP开发者调用连接用户接入网关方法后，需要实现uSDKDeviceManagerDelegage委托并设置委托，通过实现如下方法，方可获得用户接入网关连接状态推送。具体状态值见7.1.17章节 uSDK与云平台连接状态值定义

设置委托：

    [uSDKDeviceManager defaultDeviceManager].delegate = self;


实现委托：    

    -(void)deviceManager:(uSDKDeviceManager*)deviceManager didUpdateCloudState:(uSDKCloudConnectionState)state error:(NSError*)offlineReason{
       if(state==uSDKCloudConnectionStateConnectFailed){
            NSLog(@"云连接失败，错误码：@%",offlineReason.code);
        }
    }
  

### 10.4 测试远程功能是否正常
手机在配置设备到A路由，绑定设备成功。手机或平板电脑切换连接B路由或直接使用2G、3G、1.数据网络，查询设备状态、进行控制。<br>

#### 注意：
1、当用户切网时，移动设备切网成功后uSDK会自动尝试连接用户接入网关，当uSDK成功读取到智能设备信息后，再次向移动应用程序推送设备就绪消息和设备状态变化消息。<br>

2、连接用户接入网关地址，需要和uPlug的目标连接环境一致。举例：App申请北京联调环境用户接入网关地址，uPlug也应该连接北京联调环境设备。<br>

3、App使用UWS或OPEN API过程中，U+云平台会要求移动应用客户端要有自己的ClientId。ClientId和移动应用的身份验证是相关的，验证合法的ClientId会分配 Session值（Token），而Session值会应用于uSDK远程登录，所以错误或固定的ClientId会产生异常行为。

### 10.5 断开用户接入网关连接，解除设备远程功能
当用户切换帐号、注销、退出程序时，开发者需要调用uSDK的API方法断开用户接入网关连接，解除设备的远程功能，具体代码如下

    [[uSDKDeviceManager defaultDeviceManager]disconnectToCloudWithToken:deletate.remoteSession success:^{
        
    } failure:^(NSError *error) {
        
    }];
代码块success，执行成功时触发，断开云的连接成功<br>
代码块failure，执行失败触发，断开云的连接失败，建议重新执行该方法。

## 11 接收U+云平台推送消息
App获得消息推送需要先连接用户接入网关。在U+云消息推送功能中，uSDK仅扮演Client角色，透传用户接入网关发送来的消息。<br>
下面主要分为业务消息推送和Session异常消息推送

###  11.1 接收U+云平台推送业务消息

APP开发者需要实现uSDKManagerDelegage委托并设置委托，通过实现如下方法，获得业务消息推送。

设置委托：

uSDKManager *uSDKMgr = [uSDKManager defaultManager];
uSDKMgr.delegate = self;

实现委托：

    -(void)uSDKManager:(uSDKManager *)sdkManager businessMessage:(NSString *)businessMessage{

    }

sdkManager 当前uSDKManager对象<br>
businessMessage 当前推送的业务消息


### 11.2  接收U+云推送的Session异常消息
当App得到Session异常消息时，可以提示用户登录信息已失效，提示用户重新登录，或者检查程序逻辑，查看用户登录地址与连接用户接入网管地址是否同一服务器环境。
APP开发者需要实现uSDKManagerDelegage委托并设置委托，通过实现如下方法，获得Session异常消息推送。<br>

设置委托：

    uSDKManager *uSDKMgr = [uSDKManager defaultManager];
    uSDKMgr.delegate = self;

实现委托：

    -(void)uSDKManager:(uSDKManager*)sdkManager sessionException:(NSString*)token{

    }

sdkManager：     当前uSDKManager对象<br>
token：          当前会话失效的token


## 12 接收云平台推送的绑定和解除绑定消息
当uSDK已经和云平台建立连接，调用UWS或OPEN API中的绑定和解绑定方法成功时，uSDK会收到云平台推送的绑定和解除绑定消息。如果存在同一帐号、在手机A和B上绑定同一台设备的情况，开发者需要区别处里。<br>
APP开发者需要实现uSDKDeviceManagerDelegate委托并设置委托，通过实现如下2个方法，获得绑定和解除绑定消息推送。

### 12.1 接收绑定消息推送

设置委托：

    [uSDKDeviceManager defaultDeviceManager].delegate = self;

实现委托：

    -(void)deviceManager:(uSDKDeviceManager *)deviceManager didBindDevice:(NSString *)deviceID{

    }
deviceManager： 设备管理器对象<br>
deviceID :  绑定设备的ID

### 12.2 接收解除绑定消息推送。

设置委托：

    [uSDKDeviceManager defaultDeviceManager].delegate = self;

实现委托：

    -(void)deviceManager:(uSDKDeviceManager *)deviceManager didUnbindDevice:(NSString *)deviceID{

    }  
deviceManager: 设备管理器对象<br>
deviceID:   解绑定设备的ID


##  13.获取设备Wifi信号强度
设备就绪后，使用uSDKDevice实例对象的方法获取设备的Wifi信号强度。
信号强度等级，包含 差、中、良、优，等级与具体数值对应关系如下：<br>
差：number <= 20，<br>
中：21 <= number <= 30，<br>
良：31 <= number <= 1.，<br>
优：number => 1.<br>

示例代码：

    [device getDeviceNetQualitySuccess:^(NSUInteger number, DeviceNetQuality quality) {

    } failure:^(NSError *error) {

    }];
代码块success，执行成功时触发，其中number表示信号强度的具体数值，quality 代表强度的等级，包含 差、中、良、优 <br>
码块failure，执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。

## 14. 设备绑定和解绑操作
当需要使用设备具备远程控制能力时、需要将设备和用户建立关联关系，使该设备为用户所有，就是设备绑定操作。<br>

当该用用户与已绑定的设备不需要这种关联关系时，用户需要对该设备进行解绑操作，解除关联关系。解除绑定后的设备，将不再具备远程的控制能力。<br>

设备绑定操作和解绑操作都需要访问外部网络，所以必须保证外部网络良好可用。

### 14.1 设备绑定
绑定设备操作是将设备和用户帐号建立关联关系的一项操作，需要依赖外部网络。本章主要介绍两种绑定方式，开发者可以根据实际情况选择。绑定方式：1、使用uSDK提供的绑定方法绑定。2、使用云平台提供的绑定接口进行绑定（open api或者uws）。

#### 绑定设备的基本条件
1.设备和app必须连接同一服务器的环境，否则将会绑定失败。例如app连接开发者环境，设备连接到生产环境。<br>

2.绑定设备操作是将设备和用户帐号建立关联关系的一项操作，需要访问外部网络，所以必须保证外网可以正常访问。

#### 设备绑定的时机 
1.海尔模块设备：配置完成10分钟内，完成设备绑定操作，超过10分钟将绑定失败，需要重新进行配置。<br>

2.SmartDevice SDK接入设备：成功开启绑定时间窗10分钟内，完成设备绑定操作，超过10分钟将绑定失败，需要重新开启绑定时间窗   <br>

####   14.1.1使用uSDK的绑定方法
SDK的绑定方法中包含了获取设备绑定信息和绑定设备到云平台的操作，方法简单易用，降低了开发人员的开发成本。

##### 前提条件
使用SDK的绑定方法进行设备绑定，是uSDK提供的uAccount类提供的功能，所以需要使用uAccount类提供的登录、获取设备列表等全功能。


##### 操作步骤：
1.使用uAccount类提供的帐号登录功能登录U+云成功<br>
2.设备配置入网成功<br>
3.使用uSDK的绑定设备方法绑定设备<br>
4.获取用户设备列表<br>
5.执行连接用户接入网关<br>

##### 执行绑定设备方法
示例代码：

    [[uSDKDeviceManager defaultDeviceManager]bindDevice:device deviceName:@"device1" timeoutInterval:90 success:^{
      //新设备，需要连接用户接入网关。
    } failure:^(NSError *error) {

    }];
device：配置成功的设备<br>
deviceName：设备名称，可自定义<br>
timeoutInterval:超时时间，范围20-120秒，建议90，可以根据实际情况调节。<br>
success代码块：方法执行成功时触发，说明设备成功绑定到U+云时。<br>
failure代码块：方法执行失败时触发，error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。



####    14.1.2使用U+云提供的绑定方法（uws或open api）
当开发人员使用U+云提供uws或open api接口文档进行开发时，就需要使用文档中提供的绑定设备方法完成绑定操作。此时不能与uAccount提供的绑定方法混用。<br>

本章节重点介绍uSDK中uSDKDevice类提供的获取设备绑定信息方法,关于U+云提供uws或open api接口文档提供绑定方法见详细接口文档。

##### 获取绑定信息的时机 
1.海尔模块设备：配置完成10分钟内，完成设备绑定操作，超过10分钟将绑定失败，需要重新进行配置。<br>
2.SmartDevice SDK接入设备：成功开启绑定时间窗10分钟内，完成设备绑定操作，超过10分钟将绑定失败，需要重新开启绑定时间窗  <br>

##### 操作步骤：
1.使用U+云提供uws或open api接口文档提供的帐号登录功能登录U+云成功<br>
2.设备配置入网成功<br>
3.使用uSDK中uSDKDevice类提供的获取设备绑定信息方法获取绑定信息<br>
4.使用U+云提供uws或open api接口文档提供绑定方法完成设备绑定<br>
5.获取用户设备列表<br>
6.执行连接用户接入网关<br>

##### 执行获取设备绑定信息

示例代码

    [device getDeviceBindInfoWithToken:@"token" timeoutInterval:30 success:^(NSString *info) {
       //调用云平台提供的设备绑定方
    } failure:^(NSError *error) {

    }];  

"token"：用户登录的真实token<br>
30：获取绑定信息的超时时间，单位秒。<br>
代码块success，执行成功时触发，info就是设备绑定信息。<br>
代码块failure，执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。


### 14.2 设备解除绑定
当该用用户与已绑定的设备不需要这种关联关系时，用户需要对该设备进行解绑操作，解除关联关系。解除绑定后的设备，将不再具备远程的控制能力。<br>

解绑操作都需要访问外部网络，所以必须保证外部网络良好可用。<br>

####    14.2.1使用uSDK的解除绑定方法

##### 前提条件
使用SDK的绑定方法进行解除设备绑定，是uSDK提供的uAccount类提供的功能，所以需要使用uAccount类提供的登录、获取设备列表等全功能。


##### 操作步骤：
1.使用uAccount类提供的帐号登录功能登录U+云成功<br>
2.使用uSDK的解除绑定设备方法<br>
3.获取用户设备列表<br>
4.执行连接用户接入网关<br>


##### 执行设备解除绑定方法
解除绑定方法内部具有重试机制，为异步方法，不会阻塞线程，执行结果会在回调函数中以参数形式返回

示例代码：

    [[uAccount defaultUAccount]unbindDeviceWithDeviceId:deviceID success:^(RespCommonModel *successModel) {
        
    } failure:^(RespCommonModel *failureModel) {
        
    } httpError:^(RespCommonModel *httpErrorModel) {
        
    }];
代码块success，执行成功时触发，successModel返回的具体信息。<br>
代码块failure，执行失败时触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。<br>
代码块httpError 网络异常时触发<br>


####   14.2.2使用U+云提供的解除绑定方法
当开发人员使用U+云提供uws或open api接口文档进行开发时，就需要使用文档中提供的绑定设备方法完成绑定操作。此时不能与uAccount提供的绑定方法混用。

##### 操作步骤：
1.使用uAccount类提供的帐号登录功能登录U+云成功<br>
2.使用U+云提供的解除绑定方法<br>
3.获取用户设备列表<br>
4.执行连接用户接入网关<br>

##### 执行设备解除绑定方法
具体方法详见uws或open api接口文档。

### 14.3  异常处理
设备进行绑定时的加密信息来源与 U+云，app调用bindDevice接口时，由于时间或网络因素，智能设备可能还没有成功连接到 U+云，开发者可以参考下列信息处理异常: 
![public_get_bindinfo_error_code][public_get_bindinfo_error_code]

## 15.海外uSDK业务开发指引

到目前为止uSDK分为国内版、欧洲版和美国版，三个版本，各地区需要使用对应的版本。<br>

通读第四章节，并熟练掌握相关知识是开发海外智能设备功能的前提条件， 开发者需要重点阅读本章节。

### 15..1 设置配置文件下载地址
uSDK启动前，需设置配置文件下载地址，使uSDK能够从指定的海外数据中心下载配置文件使设备连接成功或就绪、可控。

#### 配置文件地址：

    1、美国：https://standardcfm-gea-us.haieriot.net:443/hardwareconfig/config/getDownUrlByFormat
    2、欧洲：https://standardcfm-gea-euro.haieriot.net:443/hardwareconfig/config/getDownUrlByFormat

示例代码：

    uSDKErrorConst result =   [[uSDKManager defaultManager]setProfileServiceUrl:url];
      if(result!=RET_USDK_OK){
      //重新设置配置文件服务器的地址
    }
url：下载配置文件服务器的地址，参数不能为@""或nil，长度不超过128，且必须以http或者https开头.<br>
result：执行结果返回值，如果不等于RET_USDK_OK，程序不能向下执行

### 15.2 连接海外用户网关
开发海外远程功能时，需要连接不同地域使用的用户接入网关地址和端口不同，如在美国需要使用的美国数据中心，用户接入网关地址和端口应该是美国数据中心的地址和端口。

#### 海外用户网关地址和端口：
1、美国：

    网关地址：gw-gea-us.haieriot.net
    网关端口：56815

2、欧洲：

    网关地址：gw-gea-euro.haieriot.net
    网关端口：56815
####  执行连接海外用户网关  
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

注意：<br>
1、用户登录后，例如一位新用户，从云拉取用户设备列表为空时，deviceList不能为nil，可以是长度为0的NSArray对象.<br>
2、如果当用户只关心业务消息推送，不关心设备远程控制时，deviceList不能为nil, 可以是长度为0的NSArray对象.    <br> 


### 15.3 设置uPlug模块主网关地址
已出厂的海外模块中已经烧写了指定海外数据中心地址,大多情况下不需要设置主网关地址，来更改模块连接的数据中心。<br>

目前有两种方法设置模块的主网关地址；1、softap时设置模块的主网关地址。2、使用uSDKDevice对象设置模块的主网关地址

#### 15.3.1 softap时设置模块的主网关地址
SoftAp配置接口，开发者无需再调用getSoftapDeviceConfigInfo接口。该接口整合了获取配置信息(getSoftapDeviceConfigInfo)和发送配置命令两个步骤，并且两个步骤均提供了重试功能，方便开发者使用。该接口可在softApConfigInfo参数中配置超时时间在APP进入后台时是否计时。 uSDK 4.5.01新增。<br>
示例代码：

    [self.view endEditing:YES]; //实现该方法是需要注意view需要是继承UIControl而来的
    uSDKSoftApConfigInfo *cfgInfo = [uSDKSoftApConfigInfo init];
    cfgInfo.mainGatewayDomain = usMainGatewayDomain;
    cfgInfo.mainGatewayPort = usMainGatewayPort;
    cfgInfo.security = NO;
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

#### 15.3.2 通过SDKDevice对象设置模块的主网关地址
当App端配置设备成功后10分钟内，且使设备处于连接成功或就绪状态时，开发者才可以调用SDKDevice对象设置模块主网关地址和端口方法，发送主网关域名和端口到设备模块中，使设备能够连接到指定的海外数据中心。配置设备成功10分钟内后，该方法调用返回失败结果。<br>
示例代码：

    if(self.currentDevice.state == uSDKDeviceStateConnected ||self.currentDevice.state == uSDKDeviceStateReady){
        [device setDeviceGatewayWithDomain:mainGatewayDomain port:mainGatewayPort success:^{
            
        } failure:^(NSError *error) {
            
        }];
    }
mainGatewayDomain：模块主网关地址<br>
mainGatewayPort：网关端口<br>
代码块success，执行成功时触发<br>
代码块failure执行失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。


##  16.   极路由相关业务指引
极路由作为海尔智能设备的深度合作伙伴，为了提高用户的体验效果和产品的满意度，针对海尔智能设备提供了待配置设备的发现功能和免密码配置功能。

### 16.1    极路由的待配置设备发现功能
#### 环境搭建
极路由版本要求：升级固件至1.4及以上版本，<br>
uSDK版本要求：uSDK需要升级到4.2.02及以上版本

####  模块版本要求：
空调：瑞昱模块 2.5.10及以上；通用：瑞昱模块2.3.30及以上，其他版本的模块不支持。
上述环境搭建完成后，开发人员就可以根据代码开发指导的流程进行开发和测试了。

####  代码开发指导
APP开发者需要实现uSDKDeviceScannerDelegate委托并设置委托，通过实现如下方法，才能发现待配置设备。
##### 设置委托

    [[uSDKDeviceScanner alloc]init].delegate = self;

##### 启动设备扫描功能

     [uSDKDeviceScanner startScanConfigurableDevice];

##### 实现发现待配置设备方法
发现新增的待入网设备时触发该方法

    - (void)deviceScanner:(uSDKDeviceScanner*)scanner didFindNewDevice:(uSDKDeviceInfo *)device{

    }
scanner： uSDKDeviceScanner 对象<br>
device：  发现的设备对象


### 16.2  免密码配置功能
极路由发现的待配置设备，可以通过免密码配置方式进行配置，也可以通过正常的SmartLink配置方式进行配置，开发者需要通过uSDKDeviceInfo.supportNoPwdConfig属性进行判断，当为YES时，说明被发现的设备支持免密配置方式； 当为NO时，您需要参考4.2章节进行SmartLink方式配置入网。

#### 执行无密码配置设备入网

    - (void)deviceScanner:(uSDKDeviceScanner*)scanner didFindNewDevice:(uSDKDeviceInfo *)device{
      if(device.supportNoPwdConfig==YES){
          [[uSDKDeviceManager defaultDeviceManager]configDeviceByNoPasswordWithDeviceID:device.deviceID timeoutInterval:60 success:^(uSDKDevice *device) {
        
            } failure:^(NSError *error) {
            
          }];
      }
    }  
device.deviceID：设备id或者设备mac<br>
代码块success，执行成功时触发<br>
代码块failure执行失败时被触发, error中有需要关注的错误信息，error.code为错误码，error.localizedDescription为错误码的文字描述。


### 16.3 实现移除待配置设备方法
SDK连续三次扫描不到原来已发现的待配置设备，则会通过该方法将已移除的设备上报给开发者。

    - (void)deviceScanner:(uSDKDeviceScanner*)scanner didRemoveDevice:(uSDKDeviceInfo *)device{

     }

scanner： uSDKDeviceScanner 对象<br>
device：  减少的设备对象


### 16.4 停止设备扫描功能

    [uSDKDeviceScanner stopScanConfigurableDevice];



[^-^]:常用图片注释
[public_single_cmd]:../_media/_usdk/public_single_cmd.png
[public_group_cmd_sixcode]:../_media/_usdk/public_group_cmd_sixcode.png
[public_group_cmd_stand]:../_media/_usdk/public_group_cmd_stand.png
[public_op_attr_stand]:../_media/_usdk/public_op_attr_stand.png
[public_stand_cmd_getAllProperty]:../_media/_usdk/public_stand_cmd_getAllProperty.png
[public_stand_op_cmd_2]:../_media/_usdk/public_stand_op_cmd_2.png
[public_user_gateway_dev_online]:../_media/_usdk/public_user_gateway_dev_online.png
[public_get_bindinfo_error_code]:../_media/_usdk/public_get_bindinfo_error_code.png
[connectstatus_change_step]:../_media/_usdk/connectstatus_change_step.png
 1.	嵌入统计分析 SDK
1.1.	安装和部署
欢迎使用海尔用户行为数据统计分析SDK(简称 SDK)，您可以按照如下步骤开始SDK的统计分析功能。
1.1.1.	获取 AppId 和 AppKey
登录海极网，注册用户并申请开发者，创建应用后即可获得AppId 和 AppKey，具体可参见海极网的“申请开发者流程”。
海极网:[http://www.haigeek.com](http://www.haigeek.com)
1.1.2.	下载 SDK 及资料
开发者在海极网注册成功，成为海尔 U+开发者后，可以到海极网的开发者中心下载 SDK、demo 及开发者手册，开始自己程序开发之旅。
下载地址:[http://www.haigeek.com](http://www.haigeek.com)
**图片**
1.1.3.	导入统计分析 SDK
统计分析 SDK 导入方式
下载 SDK 压缩包，解压至本地目录，将其中 lib 目录下的
uAnalytics*.*.*_Android.aar 复制到 Androidstudio 工程 libs 目录中。在 build.gradle 文件的 dependencies 中添加如下语句: implementation (name:'3.5.0_uAnalytics_Android_*', ext:'aar')
。具体可参考 uAnalyticsDemo3.x 中的代码。

1.1.4.	配置 AndroidManifest.xml 文件
配置权限:
<uses- permissionandroid:name="android.permission.ACCESS_COARSE_LO CATION"/>
 
<uses- permissionandroid:name="android.permission.READ_PHONE_STATE "/>
<uses- permissionandroid:name="android.permission.ACCESS_FINE_LOCAT ION"/>
<uses- permissionandroid:name="android.permission.ACCESS_WIFI_STATE "/>
<uses-permissionandroid:name="android.permission.GET_TASKS"/>

<uses- permissionandroid:name="android.permission.WRITE_EXTERNAL_S TORAGE"/>
<uses- permissionandroid:name="android.permission.READ_LOGS"/>
<uses- permissionandroid:name="android.permission.ACCESS_NETWORK_S TATE"/>
<uses- permissionandroid:name="android.permission.INTERNET"/><uses- permissionandroid:name="android.permission.WAKE_LOCK"/>
 
<uses- permissionandroid:name="android.permission.CHANGE_NETWORK_ STATE"/>
<uses- permissionandroid:name="android.permission.CHANGE_WIFI_STAT E"/><uses- permissionandroid:name="android.permission.CHANGE_WIFI_MULT ICAST_STATE"/>
<uses- permissionandroid:name="android.permission.WRITE_SETTINGS"/>
配置 META-DATA:	
**表格**
(注意:CHANNEL 需按照定义好的渠道号填写，见渠道列表)

配置 META-DATA 示例:
<meta- dataandroid:name="U_ANALYTICS_APPID"android:value="MB- TESTAPP1-0000"/>
<meta-dataandroid:name="U_ANALYTICS_APPKEY" android:value="89t61cde07y69b6599a42c8d1c020fce"/>
<meta- dataandroid:name="U_ANALYTICS_CHANNEL"android:value="gfan"/
>
<!--policy_realtime 实时发送策略 send_restart 启动时发送策略
-->

<meta-dataandroid:name="U_ANALYTICS_POLICY" android:value="policy_realtime"/>
1.1.5.	帐号申请
全网器生命周期平台不再使用海极网的账号，迭代为使用员工工 号登录，登录前需要进行权限申请，申请完成，相关业务线审后， 会自动开放权限。具体操作，请参照权限开通手册，地址如下:

https://data.haier.net/showdoc/web/#/p/d056b942e1c0e0233e7a 58b8dc0afa2f
 
1.1.6.	验证数据

当您完成以上的 SDK 嵌入工作后，启动 app，触发 SDK 统计接口，权限审批完成后，您可以登录全网器生命周期平台查看上报的 数据。
实时查询的链接:生产环
境:https://data.haier.net/lifecyclemp/build/login.html

全网器生命周期平台上的相关疑问，请联系杨强，邮 箱:yangqiang.uh@haier.com
导入并成功使用  SDK，实时日志查询可以看到统计的原始数据(保证使用的数据上报策略为实时上报 REALTIME)，对应的数据cmd 含义如下表:

CMD 含义
1001	首次启动
1002	启动
1003	时长类事件(具体事件见时长类 EventID 列表)
1004	非时长类事件(具体事件见非时长类 EventID 列表)
1005	异常上报
1006	地理位置信息统计
 
如果经过几分钟后，还未看到实时指标更新，请检查以下事
项:
1、设备的 wifi 是否打开，是否正常联网;
2、APPID，APPKEY、权限等设置是否正确;
3、确保已触发 SDK 统计接口;
4、查看 LOGCAT 日志，是否有错误日志。

1.2.	混淆注意事项
统计分析 SDK 开发者不能在的程序中再次混淆，否则可能出现异常问题。开发者可以参考 demo 程序或如下代码：
-keep public class com.haier.uhome.uAnalytics.networkfactory.protobuffer.Mess ageAnalytics$*{*;}
-keep public class com.haier.uhome.usdk.encode.NativeUtil{*;}

2.	统计分析 SDK3.x 版本重大变化
1、3.x 版本不兼容 2.x 版本，升级到 3.x 版本后，需要修改程序代码
2、删除启动事件 EventIdConst.APP_LAUNCHED_EVENT 定义，不需要开发者显示调用 SDK 启动事件，改为 MobEvent 的 onResume 和onPause 方法中对启动状态进行判断，开发者必须强制调用一次。“e_app_start”为 SDK 保留字，不允许开发者使用或重复定义
 
3、删除数据上报策略方法，设置数据上报策略方法改为在AndroidManifest.xml 中进行设置
4、采用加密方式与云平台进行传输数据
5、统计分析 SDK3.0.01 使用 jar 包，而统计分析 SDK3.0.02 开始使用 aar 形式。

3.	统计分析 SDK 业务详解
3.1.	渠道统计功能
渠道统计功能可以帮助用户统计产品渠道，APP 开发者通过配置AndroidManifest.xml 文件和调用设置渠道接口两种方式进行设置。建议开发者默认使用配置 AndroidManifest 方式配置。
开发者调用过程中要注意，填写的是渠道号，而不是渠道名称， 填写中文、特殊字符时，云平台统计到的将是乱码。
3.1.1	AndroidManifest 方式配置

配置 META-DATA 示例:
<meta- dataandroid:name="U_ANALYTICS_CHANNEL"android:value="gfan"/
>
 
3.1.2	设置渠道接口方法配置

若开发者已经使用 AndroidManifest 进行配置，则不需要调用本方法。
若 AndroidManifest 配置方式不能满足开发需求，可以调用使用本方法设置渠道，建议放在 SDKAPI 调用的最初始位置，即 SDKAPI 首次调用位置，此时 SDK 将不再向云平台发送 AndroidManifest 中的渠道号。若不是在 SDKAPI 首次调用位置，云平台将会以本方法上传的渠道号为准。
调用该方法后 SDK 会将渠道信息设置到 SDK 的请求消息头中， 不会立即上传，需要调用 SDK 其他业务方法后，才能上传该渠道信息。
示例代码:

MobEvent.setChannel(this,"gfan");

3.2 SDK 初始化
统计分析 SDK3.5.0 版本提供了显示初始化 SDK 的方法，该方法中允许开发设置日志级别、置历史数据轮询上报时间间隔、设置历史数据有效期等参数，建议在首个启动页面的 OnCreate 方法中调用。
 
如果不显示调用 SDK 初始化方法，APP 开发者需要强制调用onResume 和 onPause 方法一次，才能正常统计到设备的基本信息和 app 的使用次数。

1、设置历史数据轮询上报时间间隔。取值范围 1-60 分钟，若不调
用此接口，默认 5 分钟。
2、设置历史数据有效期。取值范围 1-60 天。如开发者不调用该接
口，有效期默认为 7 天。超过有效期的数据，不再上报，从本地数据库清除。
3、设置后台数据清除任务的时间间隔。取值范围 1-60 分钟，若不
调用此接口，默认 30 分钟。
4、设置日志级别。开发期间建议使用 DEBUG 级别，app 发布前建
议使用 None 或 Error 级别，避免重要信息泄露

示例代码：

AnalysisConfig config = new AnalysisConfig.Builder(this)

.reportIntervalTime(5)

.cacheExpireTime(10)

.cleanIntervalTime(30)

.logLevel(LogLevel.DEBUG)

.build(); MobEvent.init(config);
 


3.3.	地理位置信息统计功能
获取地理位置章节分为 SDK 自动上报地理位置和 APP 主动上报地理位置信息两个方法，两个方法不互斥。程序每次启动时，开发 者只调用任意一个方法上报地理位置信息即可，不推荐多次调用。
3.3.1.	SDK 自动上报地理位置
设置自动上报地理位置信息方法，统计分析 SDK 通过 Android
系统 API 获取地理位置信息并上报到云平台。
1、调用该方法一次上报一次地理位置信息，建议程序启动时调用一 次该方法。
2、不调用此方法，不上报地理位置信息。
3、地理位置信息发生改变时不会上报地理位置信息。
方法:
MobEvent.setAutoLocation(Context contex)
参数:
context 页面的设备上下文
调用位置:
建议在 app 入口 activity 的 onCreate 方法中。示例代码: MobEvent.setAutoLocation(MainActivity.this);
 
3.3.2.	APP 主动上报地理位置
当开发者可以自己主动上报地理位置信息(如百度地图、高德地图等)，并通过如下方法上报到云平台，调用该方法一次上报一次地 理位置信息。建议程序启动一次只调用一次该方法。
方法:
MobEvent.setLocation(Context context,double latitude,double
longitude)。
参数:
context:页面的设备上下文latitud:纬度。
longitud:经度。
调用位置:
建议在 app 入口 activity 的 onCreate 方法中。
示例代码:

MobEvent.setLocation(MainActivity.this,latitude,longitude);

3.4.	异常信息统计功能
异常信息统计功能分为自动上报崩溃信息和主动上报异常信息两 个方法。两个方法不互斥，建议都设置，可以帮助开发者收集应用
程序的崩溃信息和异常信息，完善 APP 程序。
 
3.4.1	SDK 自动上报崩溃异常信息

默认情况下，SDK   会自动捕获未知的崩溃信息并在下次启动时上
传服务器,，开发者可以不调用。

将 flag 设置为 false 时，会关闭自动崩溃异常收集功能，不建议关闭此功能。
App 有需求使用系统的 CrashHandler 时，先将 flag 设置为false，取消 SDK 对 CrashHandler 进行注册，通过 posException 方法上传崩溃信息。
方法:
MobEvent.setExceptionCatchEnabled(Context context,Boolean flag)
参数:
context:页面的设备上下文flag:开关标识
调用位置:
建议在 app 入口 activity 的 onCreate 方法中。
示例代码:
MobEvent.setExceptionCatchEnabled(MainActivity.this,true);
3.4.2	APP 主动上报异常信息
此方法需要开发者主动调用，将需要关注的异常信息上报给云平 台，帮助开发者改进自己的 APP 程序。
 
方法一:
MobEvent.postException(Context context,String exception)。
参数:
context:页面的设备上下文exception:出错信息字符串
调用位置:需要上报错误信息的地方。示例代码:
try{//代码省略
}catch(Exception e){ MobEvent.postException(getActivity(),e.toString()); e.printStackTrace();
}
方法二:
MobEvent.postException(Context context,Throwable exception)
参数:
context:页面的设备上下文
exception:抛出的异常信息
调用位置:需要上报错误信息的地方。示例代码:
try{
//代码省略
}catch(Exception e){
 
MobEvent.postException(getActivity(),e);
}

3.5.	uSDK 版本统计功能
APP 使用 uSDK 的 JAR 或 aar 包且 uSDK 启动成功后,可以调用如下接口设置 uSDK 版本信息，此方法不会即时上传 uSDK 版本信息，会在发生异常上报和绑定设备时使用并上传。如果不调用此方法设置 uSDK 版本信息，在发生异常和绑定设备上报的信息中uSDK 版本将为””。如果 APP 没有嵌入 uSDK，不使用此接口。方法:
MobEvent.bindUSDK(Contextcontext,StringuSDKVersion)
参数:
context:页面的设备上下文
uSDKVersion:uSDK 版本号，uSDK 启动成功后通过 API 获取
调用位置:建议在入口 Acitivity 的 onCreate 方法中，uSDK 启动成功后调用此方法。
示例代码: Stringver=uSDKManager.getSingleInstance().getuSDKVersion();Mob Event.bindUSDK(MainActivity.this,ver);
 
3.7.绑定用户统计功能
在获得用户唯一标识符后，APP  每次启动调用如下接口，该接口会记录用户唯一标识符并设置到 SDK 的请求消息头中，以确定某一用户在使用该应用。调用该方法后 SDK 不会立即上传用户唯一标识符，需要调用 SDK 其他业务方法(事件次数和时长类统计方法)后， 才能触发该数据上传。
方法:
MobEvent.bindUserId(Contextcontext,StringuserId);
参数:
context:页面的设备上下文。userId:用户标识符。
调用位置:获得用户唯一标识符后，APP  每次启动调用。
示例代码:
Private void userLogin(){
//用户登录成功，获取 userId MobEvent.bindUserId(MainActivity.this,userId);
}

注意:调用该方法后 SDK 不会立即上传用户唯一标识符，需要调用SDK 其他业务方法(事件次数和时长类统计方法)后，才能触发该数据上传。
 
3.7.	页面统计功能
使用时长类事件的统计功能，统计某个页面的使用情况，应该在 每个 Activity 中调用 MobEvent.onResume(this)和MobEvent.onPause(this)方法，只有正确的使用页面统计方法才能获 得准确的统计数据。具体调用方法如下:
3.7.1	Activity/FragmentActivity 的页面统计
1.	标记一次页面访问的开始
方法:
MobEvent.onResume(Context context);
参数:
context 页面的设备上下文。
调用位置:每个 activity 的 onResume()。
示例代码:
@Override
public void onResume(){ super.onResume();
//页面统计开始MobEvent.onResume(this);
}
注意:每次调用，SDK 会检查是否产生新的会话(session 超时)，即生成启动次数。
 
2.	标记一次页面访问的结束
方法:
MobEvent.onPause(Context context);
参数:
context 页面的设备上下文。
调用位置:每个 activity 的 onPause()。
示例代码:
@Override
public void onPause(){ super.onPause();
//页面统计结束MobEvent.onPause(this)
}
所有的 Activity 中都调用 MobEvent.onResume(this)和
MobEvent.onPause(this)方法，这两个调用将不会阻塞应用程序的 主线程，也不会影响应用程序的性能。注意如果您的 activity 之间有继承或者控制关系，请不要同时在父和子 activity 中重复添加onResume 和 onPause 方法，否则会造成统计数据混乱。例如使用 TabHost、TabActivity、ActivityGroup 时。
注意:
 
onResume 和 onPause 需要成对使用才能正常统计 activity，不能多个页面交叉调用，如果出现调用顺序 onResume—onResum 或者 onPause—onPause，统计将会出问题。必须的顺序是onResume—onPause—onResume—onPause。
为了统计准确性，建议在每个 activity 中都调用以上接口，否则可能会导致 SDK 不能对使用时间和使用次数数据正常统计。
3.7.2	FragmentActivity+Fragment 或 View 中的页面统计

一、FragmentActivity+Fragment 的页面统计步骤:步骤一、FragmentActivity 中完成 3.7.1 中的设置，步骤二、通过使用事件时长统计方法完成 Fragment 的页面统计功能。
二、View 的页面统计步骤:

通过使用事件时长统计方法完成 view 的页面统计功能。事件时长统计方法在 Fragment 或 View 中的用法，如下: 1.标记一次页面访问的开始
方法:

MobEvent.onEventStart(Context context,String eventId,String label);
参数:
context:页面的设备上下文。
 
eventID:事件标识(本章节中固定 EventIdConst.APP_USE_DURATION) label:事件英文标签，使用中文云平台统计会产生乱码。如:页面的名 称。
调用位置:在 Fragment 或 View 的 onResume 方法中调用。

示例代码:

@Override

public void onResume(){ super.onResume(); MobEvent.onEventStart(getActivity(),
EventIdConst.APP_USE_DURATION,getClass().getSimpleName());

}
2、标记一次页面访问的结束方法:
MobEvent.onEventEnd(Context context,String eventId,String label);
参数:
context:页面的设备上下文。
eventID:事件标识(本章节中固定 EventIdConst.APP_USE_DURATION) label:事件英文标签，使用中文云平台统计会产生乱码。
调用位置:在 Fragment 或 View 的 onPause 方法中调用。

示例代码:
 
@Override

public void onPause(){ super.onPause();
MobEvent.onEventEnd(getActivity(), EventIdConst.APP_USE_DURATION,getClass().getSimpleName());
}
如果您的 Fragment 或 View 之间有继承或者控制关系，请不要

同时在父和子 Fragment 或 View 中重复添加 onResume 和 onPause
方法，否则会造成统计数据混乱。注意:
1、onEventStart 和 onEventEnd 必须成对出现，且参数完全相同， 才能正常统计 Fragment 或 View，不能多个页面交叉调用，如果出现调用顺序 onEventStart—onEventStart 或者 onEventEnd— onEventEnd，统计将会出问题。必须的顺序是 onEventStart— onEventEnd—onEventStart—onEventEnd。
2、label 值为 null 时，不向服务器发送 label 值;label 值为””时，发送值为””;
 
3.8.	事件次数和事件时长统计功能
可以统计某些用户行为事件的发生次数，时间，变化趋势等，例 如设备控制，app 加载耗时等，通常 eventId 用于表示某种行为或功能的统计，eventId 唯一标识一个事件。
自定义事件分为 2 大类:
	统计次数:统计指定行为被触发的次数。
	统计时长:统计指定行为消耗的时间，单位为秒。

需要 eventStart 接口与 eventEnd 接口成对使用才生效。事件参数类型:key-value 即 JAVA 语言中的 MAP 数据类型，特殊的事件需要传入指定的 MAP 信息，如设备绑定/解绑和控制设备等，MAP 规范详见 4.数据定义。
目前只支持预定义的 eventId，详见 4.7eventId 列表，填写正确的 eventId 才能参与正常的数据分析。目前不支持数据分析
网站上注册自定义的 eventId。

3.8.1	普通事件次数统计
可以统计事件的发生次数，用于统计用户登次数、Button 点击次数、用户行为触发次数等。EventId 的使用，见 4.7.2 非时长类EventID 定义章节。
方法一:
MobEvent.onEvent(Context context,String eventId)。
 
参数:
context:页面的设备上下文。
eventId:非时长类的无参数事件标识。
调用位置:按照非时长类 eventId 功能描述调用。方法二:
MobEvent.onEvent(Context context,String,eventId,int acc)
参数:
context:页面的设备上下文
eventId:非时长类的无参数事件标识,
acc:事件发生次数，不能为负值，负值时按 0 处理。
调用位置:按照非时长类 eventId 功能描述调用。方法三:
MobEvent.onEvent(Contextcontext,StringeventId,Map<String,String> param)
参数:
context:页面的设备上下文。
eventId:非时长类的有参数事件标识。
Param:key-value 参数对，key 和 value 都是 string 类型
调用位置:按照非时长类 eventId 功能描述调用。
示例代码:
Map<String,String> param = new HashMap<String,String>(); param.put("mac","00:07:A8:8A:52:2C");
 
param.put("pf","eSDK_WIFI_AC"); param.put("sv","e_1.0.09");
param.put("hv","G_1.0.00");

param.put("fi","4004");

param.put("ct","smartlink");

param.put("rc","0000"); param.put("tid","00000000000000008080000000041410");
MobEvent.onEvent(getActivity(),EventIdConst.DEVICE_BIND_EVENT, param);
方法四:
MobEvent.onEvent(Contextcontext,StringeventId,Map<String,String> param,intacc)
参数:
context:页面的设备上下文。
eventId:非时长类的有参数事件标识。
param:key-value 参数对，key 和 value 都是 string 类型 acc:事件发生次数，不能为负值，负值时按 0 处理。
调用位置:按照 eventId 功能描述调用。
示例代码:
Map<String,String> param =n ew HashMap<String,String>(); param.put("mac","00:07:A8:8A:52:2C");
 
param.put("pf","eSDK_WIFI_AC"); param.put("sv","e_1.0.09"); param.put("hv","G_1.0.00");param.put("fi","4004"); param.put("ct","smartlink");
param.put("rc","0000"); param.put("tid","00000000000000008080000000041410");
MobEvent.onEvent(getActivity(),EventIdConst.DEVICE_BIND_EVENT, param,5);;
3.8.2	打点(埋点)事件次数统计

打点事件又称埋点事件，是普通事件次数统计的一种特例， eventId 固定为 EventIdConst.USER_CLICK_EVENT 的事件。当现有普通事件次数统计功能不能满足开发者的需求时，可以参考打点事 件。如果打点事件也不能满足开发者需求，需要自定义，请阅读
3.7.4 自定义事件章节。

方法一: MobEvent.onEvent(Contextcontext,StringeventId,Map<String,String> param)
参数:
 
context:页面的设备上下文eventId:事件标识，固定值
EventIdConst.USER_CLICK_EVENTparam:key-value 参数对，key 和value 都是 string 类型,key 的值固定为“actioncode”,value 值需要和云平同事协商其具体意义。
调用位置:按 eventId 的功能描述在相应的位置调用。

示例代码:

Map<String,String>param=newHashMap<String,String>(); paramput("actioncode","any_click_event");
MobEvent.onEvent(getActivity(),EventIdConst.USER_CLICK_EVENT,p aram);
方法二: MobEvent.onEvent(Contextcontext,StringeventId,Map<String,String> param,
intacc)

参数:

context:页面的设备上下文eventId:事件标识，固定值
 
EventIdConst.USER_CLICK_EVENTparam:key-value 参数对，key 和
value 都是 string 类型,key

的值固定为“actioncode”,value   值需要和云平同事协商其具体意义。

accumulation:事件发生次数，不能为负值，负值时按 0 处理。

调用位置:按 eventId 的功能描述在相应的位置调用。
示例代码:

Map<String,String>param=newHashMap<String,String>();

param.put("actioncode","any_click_event");

MobEvent.onEvent(getActivity(),EventIdConst.USER_CLICK_EVENT,p aram,5);
关于 param 中 value 值的具体定义需要和云平台同事进行商定， 具体工作请联系杨强，邮箱:yangqiang.uh@haier.com
3.8.3	事件时长统计

可以对耗时类事件的时长进行统计，主要用于用户登陆耗时、APP 加载耗时、页面加载耗时、APP 使用时长等功能的统计。eventStart 和 eventEnd 方法必须成对出现，且参数列表完全相同， 才能正常上报事件。EventId 的使用，见 4.7.1 时长类 EventID 定义章节。
 
如果现有时长类的 EventId 不能满足开发者的使用需求时，请阅读 3.7.4 自定义事件章节。
方法:

可以指定事件的开始和结束时间，来上报一个带有统计时长的事 件。

MobEvent.onEventStart(Contextcontext,StringeventId,Stringlabel) MobEvent.onEventEnd(Contextcontext,StringeventId,Stringlabel)
参数:

context:页面的设备上下文
eventID:时长类事件标识，见 4.7.1 时长类 EventID 定义章节 label:
事件英文标签，使用中文云平台统计会产生乱码。如:页 面的名称、自定义的名称。
调用位置:按照事件时长类 eventId 功能描述调用。

示例代码:

privatevoidappLoad(){MobEvent.onEventStart(getActivity(), EventIdConst.APP_LOAD_DURATION,getClass().getSimpleName());
//.	代码部分省略
 
MobEvent.onEventEnd(getActivity(),EventIdConst.APP_USE_DURATI ON,getClass().getSimpleName());
}

注意:

1、onEventStart 和 onEventEnd 必须成对出现，且参数列表完全相同，才能正常上报事件。
2、label 值为 null 时，不向服务器发送 label 值;label 值为””时，发送值为””;
3.8.4	自定义事件

目前，由于云平台现有机制，暂不支持 EventID 用户自定义，也不支持统计分析网站上注册自定义的 eventID。如果现有 EventID 列表不能满足用户使用需求，与云平台同事进行沟通商定后，才能够对上报的统计数据进行统计。大数据生命周期平台同事，杨强，邮箱:yangqiang.uh@haier.com
3.9.	进程被杀时的处理
如果开发者调用 Process.kill 或者 System.exit 等类似的方法杀死
进程时，需在此之前调用此方法，用来保存统计数据。
方法:
 
MobEvent.onKillProcess(Contextcontext)

参数:context 页面的设备上下文
调用位置:调用 Process.kill 或者 System.exit 等类似的方法杀死进程之前。
示例代码:

MobEvent.onKillProcess(getActivity());System.exit(0);

4.	数据定义
4.1.	设备绑定 MAP 定义

含义	KEY	示例 VALUE
设备的 mac(mac)	mac	0007A88A522C
设备平台信息
(platform)	
pf	
UDISCOVERY_UWT
模块软件版本	sv	e_1.0.09
(software_ver)		
模块硬件版本
(hardware_ver)	
hv	
G_1.0.00
配置方式
(config_type)	
ct	
Smartlink、softap
失败原因
(fail_info)	
fi	40044(函数 setDeviceConfigInfo 的返回
值)
 
绑定结果
(return_code)	
rc	
0000(见 4.6)
Typeid(type_id)	tid	00000000000000008080000000041410
uSDK 版本
(usdk_ver)	
uv	
C2.0.02_2014062418_S2.1.00_2014070217



4.2.	设备解绑 MAP 定义

含义 KEY 示例 VALUE
设备的 mac(mac)	mac	0007A88A522C
解绑结果(return_code)	rc	云平台返回结果
注意:只有 APP 调用解绑接口时，才需要调用该接口。


4.3.	设备控制 MAP 定义

含义	Key	Value
设备 MAC 地址
(mac)	
mac	
0007A88A522C
Typeid(type_id)	tid	00000000000000008080000000041410
操作设备命令(cmd)	cmd	205001
 
注意:目前只支持单命令，不支持组命令

4.4.	打点事件 MAP 定义

含义	Key	Value
打点事件代码	actioncode	和云平台同事商定具体值

4.5.	渠道列表

 
 
 

 

4.6.	设备绑定错误列表

 

 

4.7.	EventID 定义
4.7.1	时长类 EventID 定义

EventID	值	含义	参数

EventIdConst.USER_LOGIN_DURATION	
t_user_login	用户登陆耗
时 ID	
无参

EventIdConst.APP_LOAD_DURATION	
t_app_load	APP 加载耗
时 ID	
无参

EventIdConst.PAGE_LOAD_DURATION	
t_page_load	页面加载耗
时 ID	
无参
 

EventIdConst.APP_USE_DURATION	
t_app_use	App 使用时
长 ID	
无参

4.7.2	非时长类 EventID 定义

EventID	值	含义	参数

EventIdConst.DEVICE_BIND_EVENT	
e_dev_bind	设备绑
定 ID	见 4.1

EventIdConst.DEVICE_UNBIND_EVENT	
e_dev_unbind	设备解
绑 ID	见 4.2

EventIdConst.DEVICE_COMMAND_EVENT	
e_dev_cmd	设备控
制 ID	见 4.3

EventIdConst.USER_CLICK_EVENT	
t_user_click	打点事
件 ID	见 4.4

5.	注意事项
5.1.	编码格式
SDK 编码格式默认 UTF-8，传递带中文参数请使用 UTF-8 编码，避免出现乱码。
 
5.2.	兼容问题
对于早期的开发者，若使用 3.0 之前的旧 SDK，使用现在的 SDK3.0 以上版本时报错，是由于新 SDK 不兼容旧版本导致的，需要开发者修改程序代码。

## Android uAnalytics SDK_3.6.0   ##

版本号： v3.6.0  

发布日期：2021.02.19  

MD5值：34403EDC5F1BC090748F89045E6D5A6F  

下载链接：点击下载  

更新日志：  

1.新增功能  

1.1 支持链式跟踪数据上传到东南亚环境。  

1.2 国内和东南亚版本合并为一个版本。  

1.3 新增数据中心枚举类（见AnalyticsIDCArea类）  

1.4 新增设置数据中心和获取数据中心功能（见AnalysisConfig.Builder`类）。  


   /**  

* 设置数据中心  
* 
*/  

   public Builder area(AnalyticsIDCArea area)  

   /** 

* 获取数据中心  
* 
*/  

   public AnalyticsIDCArea getArea()  


2.接口变更  

无   

3.内部优化及BUG修改  

3.1 AES加解密改为纯Java方式实现;  

3.2 调整Ohttp客户端为单例方式，避免过多的网络开销;  

3.3 Trace数据提交方式调整为单线程队列方式提交。  


## Android uAnalytics SDK_3.5.0   ##

版本号： v3.5.0  

发布日期：2020.11.04  

MD5值：E808DA6E3780A3B98C6927FB2471ED06  

下载链接：点击下载  
 
更新日志：  

1.新增功能  

1.1 允许开发者设置历史数据轮询上报时间间隔的方法，取值范围1-60分钟，若不调用此接口，默认5分钟；  

1.2 允许开发者设置历史数据上报有效期的方法,取值范围1-60天,若不调用此接口，默认为7天；  

1.3 提供了统计分析SDK显示调用的初始化方法；  


AnalysisConfig config = new AnalysisConfig.Builder(this)  

 .reportIntervalTime(5)  

 .cacheExpireTime(10)  

 .cleanIntervalTime(cleanInterval)  

 .logLevel(LogLevel.DEBUG)  

 .build();  


 MobEvent.init(config);  

2.接口变更  
 
无  

3.内部优化及BUG修改  

3.1. 提供了统计分析SDK兼容的初始化方法;  

3.2. 修正uAnalytics SDK 3.1.7 Android 无效参数异常导致crash;   

3.3. 优化查询，添加过期时间和已传标识的条件过滤，添加定期删除历史数据任务；  

3.4. 修复encode.so解析异常或者文件符描述错误的bug；  

3.5. 修复过期时间删除无效事务的bug;  

3.6. 内部线程池优化&其他若干代码优化；  

3.7. Android11及以上系统，extendid字段使用clientid填充；  


## Android uAnalytics SDK_3.4.0   ##

版本号： v3.4.0  

发布日期：2020.05.29  

MD5值：4AB10B235B8D4AD1E2EBC14D1377219E  

下载链接：点击下载  

更新日志：  

1.新增功能  

无  

2.接口变更   

无  

3.内部优化及BUG修改  

3.1 页面数据采集协议修改为https ;  

3.2 修复多线程下数据库事务提交崩溃的bug;  

3.3 修复Android10自动获取位置信息时provider时状态不置位的bug;  

3.4 修改app不申请SD卡读写权限时SDK申请作为补充；  

3.5 修复Android6.0以上获取Mac地址为默认值的bug;  

3.6 修复获取SIM卡为空的bug；  

3.7 其他若干优化以及日志输出敏感数据过滤。  


## Android uAnalytics SDK_3.3.0   ##

版本号： v3.3.0  

发布日期：2020.01.20  

MD5值：EC2137EBDCC775BAA3E0C3342BC94C13  

下载链接：点击下载  


更新日志：  


1 新增功能  

1.1 新增ProtoBufHttpHead类，用于开发者添加扩展字段  

1.2 新增自定义事件上报重载方法  

public static void onEvent(Context context, String eventId, ProtoBufHttpHead header);  

1.3 新增自定义事件次数统计重载方法  

public static void onEvent(Context context, String eventId, int acc, ProtoBufHttpHead header);  

1.4 新增自定义事件统计重载方法  

public static void onEvent(Context context, String eventId, Map<String, String> param, ProtoBufHttpHead header);  

1.5 新增自定义事件次数统计重载方法 public static void onEvent(Context context, String eventId, Map<String, String> param, int acc, ProtoBufHttpHead header);  

1.6 新增异常上报接口添加重载方法  

public static void postException(Context context, Throwable exception, ProtoBufHttpHead header);
 
2.接口变更  

无  

3.内部优化及BUG修改  

3.1. 修复普通事件传递签名错误的bug;  

3.2 修复获取手机IMEI号高版本为null的bug;  


## Android uAnalytics SDK_3.2.0   ##

版本号： v3.2.0  

发布日期：2019.11.28  

MD5值：52C3E8F860EFC99327047467E161C739  

下载链接：点击下载  


更新日志：   

1 新增功能  

无  

2.接口变更  

无  

3.内部优化及BUG修改  

3.1 链式跟踪域名修改为utrace.haier.net  

3.2 链式日志特殊错误码处理，防止CDN流量消耗  

3.3 修改行为统计分析域名为国内环境  

3.4 增加https安全证书校验  

3.5 数据库健壮性优化  

3.6 sonar扫描问题修改  

3.7 新增对协议‘fun’的支持  


## Android uAnalytics SDK_3.1.7   ##

版本号： v3.1.7  

发布日期：2019.08.23  
  
MD5值：84D996AB3CC8DA235F05EE8C97416A37  

下载链接：点击下载  


更新日志：  

1 新增功能  

无  

2.接口变更  

无
3.内部优化及BUG修改  

3.1 优化trace数据的序列化方式；  

3.2 修复多线程下导致op和opack点不一致的bug;  

3.3 优化异步方式提交减少队列堆积数据。  


Android uAnalytics SDK_3.1.6  

版本号： v3.1.6  

发布日期：2018.7.18  

MD5值：4325E567C5D87D9C9989A7C34B3A15D1  

下载链接：点击下载  

更新日志：  

1 新增功能  

1.1 新增APP访问云平台API的打点功能  

1.2 新增网络环境质量采集功能  

1.3 新增本地和远程控制设备打点功能  

1.4 新增配置绑定中模块的打点功能  

2.接口变更  

无  

3.内部优化及BUG修改  

无
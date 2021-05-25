iOS uAnalytics SDK_3.6.0
版本号： v3.6.0
发布日期：2021.02.19
MD5值：E4F30E0A0FF02B3806B3B0FAA979AC09
下载链接：点击下载
更新日志：
1.新增功能
1.1 支持链式跟踪数据上传到东南亚环境。
1.2 国内和东南亚版本合并为一个版本。
1.3 新增数据中心枚举类（见uAnalyticsIDCArea类）。
1.4 新增设置数据中心功能（见uAnalyticsStartOptions类）。
1.5 新增SDK启动接口

/**
SDK启动接口
@param startOptions SDK启动参数对象
@since 3.6.0
*/
+ (void)startWithOptions:(uAnalyticsStartOptions *)startOptions;
2.接口变更
无
3.内部优化及BUG修改
3.1 原启动接口startWithAppId调用新接口，启动参数默认设置国内数据中心;
3.2 用户行为统计数据批量发送修复递归死锁的问题;

iOS uAnalytics SDK_3.5.0
版本号： v3.5.0
发布日期：2020.9.14
MD5值：50D74E572B248A0C1609A229DA6CE02C
下载链接：点击下载
 更新日志：
1.新增功能
无
2.接口变更
无
3.内部优化及BUG修改
3.1 protobuffer库重命名
3.2 修改SDK版本号格式
3.3 国内域名改为https

iOS uAnalytics SDK_3.2.0
版本号： v3.2.0
发布日期：2019.11.28
MD5值：04367DBE016CDC72F55F100DA61934D8
下载链接：点击下载
更新日志：
1.新增功能
无
2.接口变更
无
3.内部优化及BUG修改
3.1 链式跟踪域名修改为utrace.haier.net
3.2 链式日志特殊错误码处理，防止CDN流量消耗
3.3 修改行为统计分析域名为国内环境
3.4 增加https安全证书校验

iOS uAnalytics SDK_3.1.7
版本号： v3.1.7
发布日期：2019.08.23
MD5值：825D2375BC3656EBD9E9A3F92581A65D
下载链接：点击下载
更新日志：
1.新增功能
无
2.接口变更
无
3.内部优化及BUG修改
3.1 优化trace数据的序列化方式；
3.2 修复多线程下导致op和opack点不一致的bug;
3.3 优化异步方式提交减少队列堆积数据。

iOS uAnalytics SDK_3.1.6
版本号： v3.1.6
发布日期：2018.7.18
MD5值：16D73C2009D3068A26100A87ADA0D6B8
下载链接：点击下载

更新日志：

1.新增功能
1.1 新增APP访问云平台API的打点功能
1.2 新增网络环境质量采集功能
1.3 新增本地和远程控制设备打点功能
1.4 新增配置绑定中模块的打点功能

2.接口变更
无
3.内部优化及BUG修改
无
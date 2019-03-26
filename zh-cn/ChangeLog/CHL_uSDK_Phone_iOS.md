 **当前版本**：[uSDK_Phone_iOS V5.3.0]()  
 **更新时间**：{docsify-updated}
 
 

## uSDK_Phone_iOS V5.3.0
 1 新增功能<br>
1.1 新增蓝牙相关功能<br>
1.1.1  新增开启蓝牙搜索（uSDKDeviceManager类）<br>
 (void)startBleSearch:(void(^)(void))success failure:(void(^)(NSError *error)) failure;<br>
1.1.2 新增停止蓝牙搜索(uSDKDeviceManager类)<br>
 (void)stopBleSearch:(void(^)(void))success failure:(void(^)(NSError *error)) failure;<br>
1.1.3 新增蓝牙类型(uSDKDeviceInfo)<br>
NET_TYPE_BLE = 2<br>
1.1.4 新增uSDKBleSearch类，用于蓝牙控制的搜索，当前SDK默认关闭<br>
1.1.5 增加蓝牙状态合并，重写状态合并方法(uSDKDeviceListManager类)<br>
1.2 新增设备热点BSSID属性(uSDKSoftApBindInfo类)<br>
@property (nonatomic, copy) NSString* iotDevBssid;<br>
 新增属性，设备热点的bssid，要求调用者必须传入该参数，SDK将用该参数校验是否还在设备热点上，而不是通过设备热点名称<br>
1.3 新增连通性测试v
当uSDKNetReachabilityChecking类中的cloud_state_notify上报130时，开始ping，直到cloud_state == offline或ping成功时停止<br>
1.4 新增加后台延时运行功能( uSDKManager类)<br>
 2 接口变更<br>
2.1修改uSDKBLEHALInterface类中register方法名为`registerBLEInterface`，原方法名疑似关键字<br>
3 内部优化及BUG修改<br>
3.1 优化uSDKQRCodeModel类，扫描得到白名单中的老型号冰箱二维码中DID小写的问题， SDK将小写DID转成大写<br>
3.2 优化BLE配置（uSDKBLEManager），当出现939(配置失败)时，返回模块最后的错误码，使错误结果更加准确<br>
3.3 优化uSDKBLEHALInterface类中BLE进入后台再回到前台时搜索慢的问题。BLE在进入后台时会放慢搜索频率，监听回前台消息，当回到前台时，重新调用一遍搜索，以恢复高频率搜索状态<br>
3.4 优化uSDKBLEHALInterface类中`centralManagerDidUpdateState`状态分发方式，改为通知方式<br>
3.5 对uSDKBLEHALInterface类中 bleMac和bleDevId进行区分，bleMac表示iOS从系统获取到的设备的UUID，bleDevId表示设备的唯一ID<br>
3.6 优化uSDKTools类transformBSSID`方法<br>
3.7 SmartLink内部优化<br>
3.7.1 模块ack中seed == 0 时也认为配置成功<br>
3.7.2 模块上线宣告中先根据ack的结果，再通过deviceID进行判断<br>
3.8 softAp内部优化<br>
3.8.1 单配置接口中仍然依赖ssid进行判断是否在设备热点上<br>
3.8.2 配置绑定接口依赖新添加的devBssid判断是否在设备热点上<br>
3.9 uSDKDevice类内部代码优化，埋点封装，将op,opack合并<br>

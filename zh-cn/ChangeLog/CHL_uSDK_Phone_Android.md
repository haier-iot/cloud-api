 **当前版本**：[uSDK_Phone_Android V5.3.0]()  
 **更新时间**：{docsify-updated}

## uSDK_Phone_Android V5.3.0
1.新增功能<br>

1.1.新增接口，支持控制蓝牙搜索开关<br>

startBleSearch(ICallback<Void> callback)<br>

stopBleSearch(ICallback<Void> callback)<br>

1.2.Soft配置设备入网优化，支持通过bsid或ssid配置设备入网,uSDKSoftApBindInfo必须传入设备热点BSSID，SDK将用该参数校验是否还在设备热点上，而不是通过设备热点名称public Builder iotDevBssid(String bssid)<br>

1.3.uSDKDeviceNetTypeConst新增NET_BLE<br>

1.4.uSDKErrorConst新增手机离线、域名解析错误的错误码埋点<br>

1.5.新增扫码绑定功能，支持直连设备通过扫码绑定接口进行绑定<br>

2.接口变更<br>
无<br>
 3.内部修改及Bug优化<br>

3.1.uSDKLogger增加net包，把ping相关业务移送到net包下，包含PingClient,PingResult, SinglePingResult三个类；<br>

3.2.NetUtil增加correctBSSID整理bssid的方法；etBSSID方法增加使用correctBSSID对结果进行处理后返回；<br>

3.3 BaseDeviceInfo删除带有ip & port参数的构造函数，增加getWifiMac() getDevProtocolType()接口；<br>

3.4DeviceInfo增加private String module; wifiMac;bleDevId等定义;<br>

3.5. setProtocol优化，增加对蓝牙设备的兼容；<br>

3.6.DevProtocolType增加DEV_PROT_BT_EPP定义，ErrorConst增加如下定义，ISystemListener增加蓝牙开关的回调public void onBtAdapterStateChanged(boolean isOn)，ProtocolType整理定义<br>

3.7SDKManager增加对蓝牙连接状态的检测<br>

3.8.BaseNative取消某些定义，ProfileNativeService增加同步设计，防止重复init或者重复uninit；SDKRuntime注册监时增加重入保护，MessageCommunication修改，utils包修改，增加BtAdapterMonitor类，config模块#### ConfigNative修改，CloudConnectionState取消英文说明，CloudDevice实现execOperationSdk内部接口<br>

3.9.IUserListener删除onCloudDeviceList回调，增加notifyCloudDeviceAdd回调<br>

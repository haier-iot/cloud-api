 **当前版本**：[SDK_SmartDevice_Android V5.1.0]()  
 **更新时间**：{docsify-updated}


## SDK_SmartDevice_Android V5.1.0
1.新增功能<br>

1.1.添加主机功能<br>

public void addDevice(final USmartDevice device, final ICallback<String> callback)<br>

1.2.添加子机接口<br>

public void addSlaveDevice(final USmartDevice device, final ICallback<String> callback)<br>

1.3.设备自绑定功能<br>

public void bindDevice(String token, final USmartDevice device, double timeout, final ICallback<USmartDevice> iCallback)<br>

1.4.获取绑定二维码功能<br>

public void getBindQRCode(USmartDevice device, double timeout, final ICallback iCallback)<br>

2. 接口变更<br>

2.1.addDevice addAuthDevice接口合并为 addDeviceByRandomPrefix，不对外公开，用户使用需要通过反射调用。<br>

private void addDeviceByRandomPrefix(final USmartDevice device, boolean isAuthDevice, final ICallback<String> callback)<br>

2.2.用户输入部分id变更为固定12位<br>

3.内部优化及BUG修改<br>

无

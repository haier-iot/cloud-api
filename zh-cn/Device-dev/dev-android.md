# Android SmartDevice SDK

Android SmartDevice SDK 是一款移动应用开发套件，包含设备接入和控制功能，能够使用在有屏的 Android 系统的智能设备上，将设备接入到 U+ 平台，并与 U+ 设备交互。


![图片][p1]


## 基本功能

**设备接入功能**  
 
&emsp;&emsp;启动/停止 SDK  
&emsp;&emsp;添加/删除设备  
&emsp;&emsp;属性和报警上报  
&emsp;&emsp;大数据上报  
&emsp;&emsp;开启绑定时间窗  
&emsp;&emsp;设备自绑定  
&emsp;&emsp;P2P 音视频功能，包含语音对讲和视频录制  
&emsp;&emsp;FOTA 升级

**设备控制功能**
 
&emsp;&emsp;设备入网  
&emsp;&emsp;设备搜索  
&emsp;&emsp;设备控制  
&emsp;&emsp;状态变化主动上报  
&emsp;&emsp;报警信息上报  
&emsp;&emsp;结合 uSDK 为设备实现授权，从而和帐号下的其他设备交互


## 开发文档


### 设备接入

**创建功能集时接入方式选择** 设备SDK(Android)

![图片][p2]


**记录生成的设备唯一标识** typeID **和** DeviceKey  

![图片][p3]


**记录生成的** 成品编码  

![图片][p4]


### API

**1. 设置日志级别**

默认不输出日志，需要通过添加设置日志级别接口，才能输出不同级别的日志。  
> 开发过程中建议使用 USDK_LOG_DEBUG，上线产品建议使用 USDK_LOG_ERROR。


```
uSDKManager.getSingleInstance().initLog(uSDKLogLevelConst.USDK_LOG_DEBUG, false, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst uSDKErrorConst) {
    
    }
});
```  

**2. 启动、停止服务**
> 推荐在 Application 对象实现中执行

- 启动服务
  
  启动 SDK 服务是调用各种功能性 API 的前置条件，在主线程中启动一次即可  

```
/**
 * domain       域名  gw.haier.net
 * port         端口  56817
 * rootCA       证书  0:"国内", 1:"东南亚"
 * authPath     存放认证文件的路径
 * netCardName  网卡名,如无线 wlan0
 */
StartOption startOption = new StartOption.Builder()
                                         .domain(domain)
                                         .port(port)
                                         .rootCA(mRootCa)
                                         .authPath(authPath)
                                         .netCardName(netCardName)
                                         .build();
                                         
mSmartDeviceManager.startService(startOption, new ICallback() {
    @Override
    public void onSuccess(Object result) {
        mSmartDeviceManager.addListener(SmartDeviceActivity.this);
        HaierToast.makeText(this, "SmartDevice 启动成功", Toast.LENGTH_SHORT).show();
        updateTitle();
        dialog.dismiss();
    }
    
    @Override
    public void onFailure(uSDKError error) {
        HaierToast.makeText(this, "SmartDevice 启动失败：" + error.toString(), Toast.LENGTH_SHORT).show();
        updateTitle();
        dialog.dismiss();
    }
});
```

  
- 停止服务  

  App 退出前或者不需要使用 SmartDevice SDK 功能时需要停止 SDK，主线程调用即可  
  停止 SDK 前需删除已注册上线的设备


```
mSmartDeviceManager.stopService
```



**3. 注册、上线、删除设备**  

开发者需要根据在海极网创建的硬件产品信息创建 AbsSmartDevice  设备实例，然后调用注册上线设备方法将设备实例接入到 U+平台中。

注册上线成功的设备是可授权设备，通过移动端 SDK  对此设备进行授权后，具备控制其他设备的能力。

> 若网络不可用情况下，调用此接口可以执行成功，但设备无法与 U+云进行网络连接及数据通信。程序内部会不断尝试与 U+云建立连接。

  
    
    
<span id="3.1"></span>
## <a id="jump">目录</a>
**3.1 注册、上线网关设备**  

- 网关设备注册


```
/**
 * mac        设备联网后的 mac 地址
 * upCodeT    海极网设备成品编码
 * uplusId    海极网设备 typeid
 * deviceKey  海极网设备 devicekey
 */
RegisterGatewayDevice registerGatewayDevice = new RegisterGatewayDevice.Builder()
                                                                       .mac()
                                                                       .upCodeT()
                                                                       .uplusId()
                                                                       .deviceKey()
                                                                       .build();
SmartDeviceManager.getInstance().registerGatewayDevice(registerGatewayDevice, new ICallback<RegisterResult>() {
    /*注册设备成功*/
    @Override
    public void onSuccess(RegisterResult result) {
        registerResult = result
    }
    
    /*注册设备失败，需根据错误码分析失败原因*/
    @Override
    public void onFailure(uSDKError error) {
        registerResult = null;
    }
})
```
  
- 网关设备上线  

```
/**
 * deviceId        注册设备返回的设备 id
 * upCodeT         海极网设备成品编码
 * uplusId         海极网设备 typeid
 * deviceKey       海极网设备 devicekey
 * upgradeVersion  版本号自定义
 * isUpgrade       是否支持 fota 升级
 * deviceRole      设备角色，选择网关设备
 */
GatewayDevice gatewayDevice = new GatewayDevice.Builder()
                                               .deviceId(registerResult.getDevId())
                                               .upCodeT()
                                               .uplusId()
                                               .deviceKey()
                                               .upgradeVersion()
                                               .isUpgrade()
                                               .deviceRole(DeviceRole.GATEWAY_DEVICE)
                                               .builder();
                                  
SmartDeviceManager.getInstance().gatewayDeviceOnline(gatewayDevice, new ICallback<String>() {
    
    /*设备上线成功*/
    @Override
    public void onSuccess(String result) {
        String msg = "gatewayDevice online: "+ result;
    }
    
    /*设备上线失败，需根据错误码分析失败原因*/
    @Override
    public void onFailure(uSDKError error) {
        String msg = "gatewayDevice 添加失败：" + error.toString();
    }
});
```
  
**3.2 注册、上线子设备** 

- 子设备注册  

```
/**
 * mac        设备联网后的 mac 地址
 * upCodeT    海极网设备成品编码
 * uplusId    海极网设备 typeid
 * deviceKey  海极网设备 devicekey
 */
RegisterSlaveDevice registerSlaveDevice = new RegisterSlaveDevice.Builder()
                                                                 .mac()
                                                                 .upCodeT()
                                                                 .uplusId()
                                                                 .deviceKey()
                                                                 .build();
SmartDeviceManager.getInstance().registerSlaveDevice(registerSlaveDevice, new ICallback<RegisterResult>() {
    /*注册设备成功*/
    @Override
    public void onSuccess(RegisterResult result) {
        String msg = "slaveDevice online: "+ result;
    }
    
    /*注册设备失败，需根据错误码分析失败原因*/
    @Override
    public void onFailure(uSDKError error) {
        String msg = "slaveDevice 添加失败：" + error.toString();
    }
}
```

- 子设备上线  

```
/**
 * parentId        子设备 mac
 * deviceId        注册设备返回的设备 id
 * upCodeT         海极网设备成品编码
 * uplusId         海极网设备 typeid
 * deviceKey       海极网设备 devicekey
 * deviceRole      设备角色，选择子设备
 * isUpgrade       是否支持 fota 升级
 * upgradeVersion  版本号自定义
 */
SlaveDevice slaveDevice = new SlaveDevice.Builder()
                                         .parentId(device.getDeviceId())
                                         .deviceId(result.getDevId())
                                         .upCodeT()
                                         .uplusId()
                                         .deviceKey()
                                         .deviceRole(DeviceRole.SLAVE_DEVICE)
                                         .isUpgrade(true)
                                         .upgradeVersion()
                                         .builder();
SmartDeviceManager.getInstance().gatewayDeviceOnline(gatewayDevice, new ICallback<String>() {
    /*设备上线成功*/
    @Override
    public void onSuccess(String result) {
        String msg = "gatewayDevice online: "+ result;
    }
    
    /*设备上线失败，需根据错误码分析失败原因*/
    @Override
    public void onFailure(uSDKError error) {
        String msg = "gatewayDevice 添加失败：" + error.toString();
    }
});
```

**3.3 注册、上线附件设备**

接口流程及参数参考 [3.2 注册、上线子设备](#jump)
```
SmartDeviceManager.getInstance().registerAnnexDevice(registerAnnexDevice, new ICallback<RegisterResult>())
        
SmartDeviceManager.getInstance().annexDeviceOnline(annexDevice, new ICallback<String>())
```

**3.4 注册、上线普通设备**


```
SmartDeviceManager.getInstance().registerGeneralDevice(registerGeneralDevice, new ICallback<RegisterResult>())

SmartDeviceManager.getInstance().generalDeviceOnline(generalDevice, new ICallback<String>())
```

接口流程及参数可参考 [3.1 注册、上线网关设备](#3.1)

**3.5 删除设备**

开发者不需要使用设备接入功能或需要退出 APP 之前，需要将之前添加的设备实例从 SDK  中移除，可以有效减少资源消耗。

```
USmartDeviceManager.getInstance().delDevice(deviceID, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "smart device  删除成功";
        } else {
            msg = "smart device  删除失败：" + errorConst;
        }
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }
});
```




### 常见问题

1.问题1

2.问题2



## 历史版本


[p1]:_media/_android/p1.png
[p2]:_media/_android/p2.png
[p3]:_media/_android/p3.png
[p4]:_media/_android/p4.png
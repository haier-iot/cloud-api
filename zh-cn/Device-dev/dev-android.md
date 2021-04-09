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
&emsp;&emsp;支持 FOTA 升级

**设备控制功能**
 
&emsp;&emsp;设备入网功能  
&emsp;&emsp;设备搜索功能  
&emsp;&emsp;设备控制功能  
&emsp;&emsp;状态变化主动上报  
&emsp;&emsp;报警信息上报  
&emsp;&emsp;结合 uSDK 为设备实现授权，从而和帐号下的其他设备交互


## 开发文档


### 设备接入

**创建功能集时接入方式选择** 设备 SDK(Android)

![图片][p2]


**记录生成的设备唯一标识** typeID **和** DeviceKey  

![图片][p3]


**记录生成的** 成品编码  

![图片][p4]


### API

**1. 设置日志级别**

默认不输出日志，开发人员需要通过添加设置日志级别接口，才能输出不同级别的日志。  
> 开发过程中建议使用 USDK_LOG_DEBUG，上线产品建议使用 USDK_LOG_ERROR。


```
uSDKManager.getSingleInstance().initLog(uSDKLogLevelConst.USDK_LOG_DEBUG, false, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst uSDKErrorConst) {
    
    }
});
```  

**2. 启动/停止服务**
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




**3. 注册、上线、删除设备**  
- 注册设备
- 上线设备
- 删除设备


### 常见问题

1.问题1

2.问题2



## 历史版本


[p1]:_media/_android/p1.png
[p2]:_media/_android/p2.png
[p3]:_media/_android/p3.png
[p4]:_media/_android/p4.png
# Android SmartDevice SDK

Android SmartDevice SDK 是一款移动应用开发套件，包含设备接入和控制功能，能够使用在有屏的 Android 系统的智能设备上，将设备接入到 U+ 平台，并与 U+ 设备交互。


![图片][p1]


## 基本功能

**设备接入**  
 
&emsp;&emsp;启动/停止 SDK  
&emsp;&emsp;添加/删除设备  
&emsp;&emsp;属性和报警上报  
&emsp;&emsp;大数据上报  
&emsp;&emsp;开启绑定时间窗  
&emsp;&emsp;设备自绑定  
&emsp;&emsp;P2P 音视频功能，包含语音对讲和视频录制  
&emsp;&emsp;FOTA 升级

**设备控制**
 
&emsp;&emsp;设备入网  
&emsp;&emsp;设备搜索  
&emsp;&emsp;设备控制  
&emsp;&emsp;状态变化主动上报  
&emsp;&emsp;报警信息上报  
&emsp;&emsp;结合 uSDK 为设备实现授权，从而和帐号下的其他设备交互

**场景控制**  

&emsp;&emsp;离线控制 

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

<a id="jump1"> </a>
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
 
<a id="jump2"> </a> 
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

接口流程及参数参考 [3.2](#jump2)
```
SmartDeviceManager.getInstance().registerAnnexDevice(registerAnnexDevice, new ICallback<RegisterResult>())
        
SmartDeviceManager.getInstance().annexDeviceOnline(annexDevice, new ICallback<String>())
```

**3.4 注册、上线普通设备**


```
SmartDeviceManager.getInstance().registerGeneralDevice(registerGeneralDevice, new ICallback<RegisterResult>())

SmartDeviceManager.getInstance().generalDeviceOnline(generalDevice, new ICallback<String>())
```

接口流程及参数可参考 [3.1](#jump1)

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

**4. U+ 云连接状态**

当注册上线设备成功后，SDK  会与云平台建立数据通路进行数据交互，连接状态发生变化时会通过回调通知 App。
开启绑定时间窗、属性状态上报、大数据上报、报警上报等各项功能都依靠云通信，所以需要重点关注和云连接状态。

> SDK和云的连接是免维护的，自带重连机制。

实现IUSmartDeviceManagerListener  接口并注册该接口得到连接状态信息。
  
```
/**
 * state  ：251 和云连接成功
 * state  ：252 和云建立连接失败
 */
public void onCloudState(int state) {
    String msg = "onCloudState state : " + state;
}
```



**5. 设备数据上报**

> SDK 与 U+ 云平台成功建立数据通路后，连接状态变为 251，此时可以通过 SDK  上报设备的属性状态、大数据、报警等信息。

**5.1 设备状态数据上报**

> 每次上报的都是属性全集，不能单个属性上报。

具体的属性名和属性值参考在[海极网](https://www.haigeek.com/)中创建的硬件设备的属性集合。  

此操作的数据上报成功与否依赖 **网络连接** 及 **SDK和云的连接状态**，执行前应进行相应的状态判断。

```
/**
 * pairName   海极网中创建的硬件设备的属性名
 * pairValue  海极网中创建的硬件设备的属性值
 */
ArrayList<USmartDevicePair> pairs = new ArrayList<>(4);
pairs.add(new USmartDevicePair("model", model));
pairs.add(new USmartDevicePair("humidity", humidity));
pairs.add(new USmartDevicePair("timedTurnOn", timedTurnOn));
pairs.add(new USmartDevicePair("onOffStatus", onOffStatus));
mSmartDevice.reportStatus(pairList, new IuSDKCallback() {
     @Override
     public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "reportStatus 成功";
        } else {
            msg = "reportStatus 失败：" + errorConst;
        }
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }
});
```

**5.2 设备报警数据上报**

> 根据业务需求可以一次上报单个报警，也可以上报多个报警。

具体的报警属性名和属性值需要参考在[海极网](https://www.haigeek.com/)中创建的硬件设备的报警属性集合。

此操作的数据上报成功与否依赖 **网络连接** 及 **SDK和云的连接状态**，执行前应进行相应的状态判断。


```
String pairName = "Alarm";
String pairName = "Alarm1";
USmartDevicePair smartDevicePair = new USmartDevicePair(pairName, pairValue);
List<USmartDevicePair> pairList = new ArrayList<>();
pairList.add(smartDevicePair);
mSmartDevice.reportAlarm(pairList, new IuSDKCallback() {
     @Override
     public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "reportAlarm 成功";
        } else {
            msg = "reportAlarm 失败：" + errorConst;
        }
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }
});
```

**5.3 设备大数据上报**

具体的大数据内容需要参考在[海极网](https://www.haigeek.com/)中创建的硬件设备的大数据。

此操作的数据上报成功与否依赖 **网络连接** 及 **SDK和云的连接状态**，执行前应进行相应的状态判断。


```
String type = "String";
String bigData = null;
try{
    JSONArray array = new JSONArray();
    JSONObject attr1 = new JSONObject();
    attr1.put("laundryCycle","10");
    JSONObject attr2 = new JSONObject();
    attr2.put("dryMode","0");
    JSONObject attr3 = new JSONObject();
    attr3.put("autoDetergentStatus","true");
    JSONObject attr4 = new JSONObject();
    attr4.put("autoDisinfectantStatus","true");
    array.put(attr1);
    array.put(attr2);
    array.put(attr3);
    array.put(attr4);
    bigData = array.toString();
} catch (JSONException e) {
    e.printStackTrace();
}

mSmartDevice.reportBigData(type, Base64.encodeToString(bigData.getBytes(), Base64.NO_WRAP), new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            String msg;
            msg = "reportBigData 成功";
        } else {
            msg = "reportBigData 失败: " + errorConst;
        }
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }
});
```

**6. 开启绑定时间窗口**

对 SmartDevice 接入设备进行控制或获取设备属性时，首先需要开启 SmartDevice 接入设备的绑定时间窗，然后使用移动端 APP  对接入设备进行安全绑定。
移动端 App 绑定成功后，才能和 SmartDevice  接入设备进行交互。

设备端开启绑定时间窗成功后，移动端 APP 可以开始设备安全绑定  


> 开启绑定时间窗功能需要访问 U+云，所以调用此方法前需要保证网络畅通，能够正常访问外网。  
> 
> **和 U+ 云的连接状态变为 251  时，才能调用开启绑定时间窗方法**。
> 
> 每次开启有效时间为 20 分钟。  
移动端 APP 需要在 20 分钟内完成安全绑定功能，超过 20 分钟设备端需要重新开启时间窗。
> 
> 建议开启绑定时间窗时，增加重试策略，直到开启成功为止。
  
  

```
/**
 * timeOut  方法请求的超时时间，建议 5-10 秒
 */
int timeOut = 3;
int retryTimes = 10;
if（cloudConnectState != 251）{
    return;
}

while(flag&&retryTimes > 0){
    retryTimes--;
    mSmartDevice.bindWindow(timeOut, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "开启绑定时间窗成功";
            retryTimes=-1;
            flag =false;
        } else {
            msg = "开启绑定时间窗失败：" + errorConst;
        }
    }
});
```

**7. 处理接收到控制命令**

**7.1 接收读属性命令**

SmartDevice 接入设备接收到读属性命令时，需要实现 IUSmartDeviceListener  类的该接口方法，根据业务需要处理该读属性命令并完成读属性应答，并调用设备属性上报方法上报设备所有属性。

> 例：在读属性回调中接收到读取开机命令，需要将该命令经过处理后发送给设备底板，  
将底板执行结果赋值给 USmartReadRsp 对象，并调用设备属性上报方法上报设备所有属性。


```
/**
 * 读属性回调：通过该接口可以获取指定设备的属性
 * 读属性应答：执行读属性回调后需调用此接口将结果返回给 uGW server
 */
public USmartReadRsp onDeviceReadCallback(String devId, int reqSn, String name) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        USmartReadRsp readRsp = new USmartReadRsp(reqSn);
        readRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return readRsp;
    }
    return device.read(reqSn, name);
}
```

**7.2 接收写属性命令**

SmartDevice 接入设备接收到单命令或写属性命令时，需要实现 IUSmartDeviceListener  类的该接口方法，根据业务需要处理该写属性命令，并返回写属性应答结果和最新的设备所有属性。

> 例：在写属性回调中接收到开机命令，需要将该命令经过处理后发送给设备底板，  
将底板执行结果赋值给 UBaseSmartRsp  对象并返回，然后调用属性上报方法上报设备所有属性。

```
/**
 * 写属性回调：通过该接口可以更新接入设备的可写属性
 * 写属性应答：执行写属性回调后需调用此接口将结果返回给 uGW server
 */
public UBaseSmartRsp onDeviceWriteCallback(String devId, int reqSn, String name, String value) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        UBaseSmartRsp smartRsp = new UBaseSmartRsp(reqSn);
        smartRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return smartRsp;
    }
    return device.write(reqSn, name, value);
}
```

**7.3 接收操作/组命令**

SmartDevice 接入设备接收到组命令或操作命令时，需要实现 IUSmartDeviceListener  类的该接口方法，根据业务需要处理该操作命令，并返回操作应答结果和最新的设备所有属性。

> 例：在操作或组命令回调中接收到获取所有属性命令，需要将该命令经过处理后发送给设备底板，  
> 将底板执行结果和设备所有属性赋值给  USmartOpRsp 对象并返回，完成控制结果和所有属性的上报。

```
/**
 * 操作性回调：通过该接口可以更新接入设备的可操作属性
 * 操作性应答：执行操作回调后需调用此接口将结果返回给 uGW server
 */
public USmartOpRsp onDeviceOpCallback(String devId, int reqSn, 
                                String opName, List<USmartDevicePair> pairList) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        USmartOpRsp opRsp = new USmartOpRsp(reqSn);
        opRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return opRsp;
    } 
    return device.op(reqSn, opName, pairList);
}
```

**8. 设备自绑定功能**

从 5.1.0 版本开始，通过本 SDK 接入的设备可以实现设备自绑定功能。
> 即**通过 AbsSmartDevice 设备对象的 bindDevice  方法将设备自己和帐号建立关联关系，绑定到云平台的操作**。
>   
>此方法大多使用在有屏幕的智能设备上。

设备自绑定业务请求发起的方式：

```
AbsSmartDevice mOnlineDevice = SmartDeviceManager.getInstance().getOnlineDeviceById(deviceId)
```

可以通过这个方法获取到之前注册上线的对象赋给 AbsSmartDevice mOnlineDevice  
后续使用这个 mOnlineDevice 对象做相关业务。


```
/**
 * token  登录 U+帐号系统后返回的令牌
 * 60     绑定超时时间，单位:秒
 */
mOnlineDevice.bindDevice(java.lang.String token, 60, new ICallback<Void>() {
    
    /*为SDK方法执行成功的回调，不是绑定业务的执行结果*/
     @Override
     public void onSuccess(Void result) {
        String msg = "绑定  device: " + mOnlineDevice.getDeviceId() + " sucess";
    }
    
    /*为SDK方法执行失败的回调，需根据返回错误码进行处理*/
    @Override
    public void onFailure(uSDKError error) {
        String msg = "绑定 device: " + mOnlineDevice.getDeviceId() + " 失败: " + error.getCode();
   }
}
```
  
  
> **这里的成功回调为 SDK 方法的执行结果，不是绑定业务的执行结果**   
> 
> **绑定业务的执行结果，在 SmartDeviceListener 类的 onDeviceBindCallback 回调方法中**

```
/**
* 设备自绑定的执行结果回调
* @param devId    设备  mac
* @param sn       请求  sn
* @param result   0 成功，非 0 失败
*/
@Override
public void onDeviceBindCallback(String devId, int sn, int result) {
    String msg = "绑定结果: " + result + " devid= " + devId + " sn= " + sn;
    Toast.makeText(this, msg, Toast.LENGTH_LONG).show();
    UplusDevice.addLog(msg);
}
```

> **此 SmartDeviceListener 的设置需要在注册上线设备方法前调用，否则 SmartDeviceListener 中的回调方法不会被触发**

**9. 获取绑定二维码信息**

通过 SmartDevice SDK 接入的设备，具备生成二维码信息的功能。

```
if(MainActivity.cloudStateFlag != 251){
    Toast.makeText(this, "未连接到 U+云，不能获取二维码信息", 1).show();
    return;
}

/**
 * 超时时间 20-120  建议 30 秒
 * mSmartDevice: 添加成功的主机或子机设备对象
 */
USmartDeviceManager.getInstance().getBindQRCode(mSmartDevice, 30, new ICallback() {
     
    /*成功获取二维码信息，包含 typeid、设备 mac 及加密信息内容*/
    @Override
    public void onSuccess(Object o) {
        /*info：手机端 SDK 端绑定设备需要使用的信息*/
        String info = o.toString();
    }
    
    /*获取二维码信息失败，需根据返回错误码进行处理和分析*/
    @Override
    public void onFailure(uSDKError uSDKError) {
        
    }
});
```

**10. P2P 音视频功能**

Smartdevice6.0.0 版本支持音视频功能,即通过设备端 sdk 上报视频,配合
usdk8.0.0 版本在 app 端实时展示视频内容,并且支持在视频播放过程中语音对讲
及上报报警内容。
示例代码：
通过 getOnlineDeviceById  获取到的上线对象来做为音视频对象。

**10.1 注册上线设备对象音视频监听**


```
/**
 * 连接音视频云状态
 * 0  未连接成功
 * 1  连接成功
 */
public void addSmartDeviceVideoListener(ISmartDeviceVideoListener listener) {
    onIotVideoCloudState(int state) {
    
    }
}

```




**10.2 对象音视频初始化**

```
/**
 * path 此路径需为此应用进程中的目录
 * 回调成功即初始化成功
 */
public void initIotVideo(java.lang.String path, <any> cb)
```




### 注意事项

1.

2.



## 历史版本


[p1]:_media/_android/p1.png
[p2]:_media/_android/p2.png
[p3]:_media/_android/p3.png
[p4]:_media/_android/p4.png
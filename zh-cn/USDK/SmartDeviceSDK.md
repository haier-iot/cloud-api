
### 简介

设备端SDK（SmartDevice SDK），是一款基于Android系统或Linux系统的应用开发套件，能够使用在有屏、无屏的Android系统或Linux系统的智能设备上，将设备接入到U+平台，并与U+设备交互。主要包括设备接入功能和设备控制功能。

设备接入功能，主要的业务功能包括启动/停止SDK、添加/删除设备、属性和报警上报、大数据上报、开启绑定时间窗等功能。

设备控制功能，主要的业务包括设备入网功能、设备搜索功能、设备控制功能、接收状态变化上报功能、接收报警信息上报功能等。

SDK具备如下特点：

![SmartDeviceSDK图片][SmartDeviceSDK_1]

### 设备端SDK能力介绍

1.能力图集

![SmartDeviceSDK图片][SmartDeviceSDK_2]


1.1 上线宣告能力

当设备端SDK成功启动并添加设备后，该接入设备会在当前WIFI网络中宣告上线信息，使自己能够被该网络中的APP（移动端SDK）发现，这种能力被称作上线宣告能力。

1.2 设备被控能力

作为接入设备，能够被手机APP或云服务进行设备控制，更改设备状态的一种能力。

1.3 属性上报能力

作为接入设备，当设备属性状态发生变化时、或接受到设备控制命令后，需要及时上报设备的实际属性状态，这种能力被成为属性上报能力。

1.4 设备控制设备能力

通过设备端SDK添加授权设备后，使用移动端SDK对该设备进行授权，使该设备具备控制其他设备的一种能力。

### 应用场景

可以把设备端SDK理解为一套标准的中间连接件，能够完成控制命令的传输，设备报警、设备属性状态、大数据等数据转发。用于和设备底板、U+云、移动端SDK进行数据通信，完成大小循环的建立，功能与海尔uPlug模块功能基本相同，使用在有屏、无屏的Android系统或Linux系统的智能设备上。开发者只需要集中精力在底板和设备端SDK之间的数据转换上，就可以快速完成设备的接入。

![SmartDeviceSDK图片][SmartDeviceSDK_3]

### 使用方式

![SmartDeviceSDK图片][SmartDeviceSDK_4]


### uSDK SmartDevice Android 5.0 开发手册

#### 1.SmartDevice SDK 5.0简介

	SmartDevice SDK是一款Android移动应用开发套件，包含设备接入和设备控制功能，能够使用在有屏的Android系统的智能设备上，将设备接入到U+平台，并与U+设备交互。

	设备接入功能，主要的业务功能包括启动/停止SDK、添加/删除设备、属性和报警上报、大数据上报、开启绑定时间窗等功能。

	设备控制功能，主要的业务包括设备入网功能、设备搜索功能、设备控制功能、状态变化主动上报功能、报警信息上报功能等，结合uSDK还能为设备实现授权从而和帐号下的设备交互。

	本文档主要介绍设备接入功能部分，设备控制功能部分不在本文内，详情见uSDK4.0_Phone开发手册。

#### 2.SmartDevice SDK整体交互图

	开发者可以把SmartDevice SDK理解为一套标准的中间连接件，用于连接底板设备和云、手机uSDK，完成大小循环的建立，功能与海尔uPlug模块功能基本相同。开发者只需要集中精力在底板和smartdevice之间的数据转换上，完成设备的快速接入。

![SmartDeviceSDK图片][SmartDeviceSDK_3]


#### 3.SmartDevice SDK接入流程简图


![SmartDeviceSDK图片][SmartDeviceSDK_5]



#### 4.SmartDevice设备接入业务详解

	设备接入是指使用SmartDevice SDK的接入功能，在有屏android设备上，通过软件程序实现一台安全类的智能设备并接入U+云的功能，其作用与uplug模块功能类似。

	设备接入功能需要实时和U+云的进行数据通信，良好的网络是完成接入功能的重要前提，否则会出现开启绑定时间窗失败、App端设备下线等问题。

准备工作如下：

	1、创建硬件设备

	目前SmartDevice SDK只支持安全类设备接入，即在海极网上创建的硬件设备时，对接方式应选择“设备SDK Android”对接方式。参考下图：

![SmartDeviceSDK图片][SmartDeviceSDK_6]

	2、配置typeId/uplusId

	在海极网中申请、创建硬件设备的typeId/uplusId后， 需要联系云平台张磊（zhanglei.uh@haier.com），对typeId/uplusId在系统中进行配置，否则无法完成接入功能。

	3、搭建环境

	经过上一个步骤的配置后，针对此设备U+云会颁发一个deviceKey用与远程访问，以及该deviceKey适用U+云环境，具体接入环境已张磊张工提供为准。例如：开发者或生产环境。开发者环境用于进行接入设备的开发和调试，生产环境用于审核完成后的上线产品。
	路由访问开发者环境的DNS配置样例：

![SmartDeviceSDK图片][SmartDeviceSDK_7]




##### 4.1.	设置日志级别


	默认不输出日志，开发人员需要通过设置日志级别接口，才能输出不同级别的日志。开发过程中建议使用USDK_LOG_DEBUG，上线产品建议使用USDK_LOG_NONE或USDK_LOG_ERROR。

	uSDKManager.getSingleInstance().initLog(uSDKLogLevelConst.USDK_LOG_DEBUG,  false, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst uSDKErrorConst) {
    }
	});


##### 4.2.	启动服务

	启动SDK服务是调用各种功能性API的前置条件，在主线程中启动一次即可。

	相关概念和术语
	devId：自定义的设备id或mac,由大写字母和数字组成，长度不超过16位。
	typeId：在海极网中创建硬件设备生成的typeid，需要联系云平台张磊，对typeId在系统中进行配置，否则无法完成接入功能。
	deviceKey：用于设备的合法性验证，目前需要联系张磊进行配置，开发者不能随意填写。邮箱：zhanglei.uh@haier.com

	示例代码：
	
	smartDeviceManager.startService(
        getApplicationContext(), "gw.haier.net", 56810, new IuSDKCallback() {
             @Override
             public void onCallback(uSDKErrorConst result) {
                 String msg;

                 if (uSDKErrorConst.RET_USDK_OK == result) {
                     msg = "SmartDevice启动成功";
                     isStarted = true;
                     deviceSDKMgrListener = new SmartListener();
                     smartDeviceManager.setListener(deviceSDKMgrListener);

                 } else {
                     msg = "SmartDevice启动失败：" + result;

                 }  
             }
         });
    }


##### 4.3.	停止服务

	App退出前或者不需要使用SmartDevice SDK功能时需要停止SDK，主线程调用即可，停止SDK前需删除已添加的设备。

	示例代码：

	USmartDeviceManager.getInstance().stopService(new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "Smart停止成功";
            isStarted = false;
            deviceInfoText.setText("");
        } else {
            msg = "Smart停止失败：" + errorConst;
        }
        startResultText.setText(msg);
    }
	});


##### 4.4.	添加设备

	开发者需要根据在海极网创建的硬件产品信息创建USmartDevice设备实例，然后调用添加方法将设备实例接入到U+平台中，此操作需要依赖外部网络。若网络不可用情况下，调用此接口可以执行成功，但设备无法与U+云进行网络连接及数据通信，程序内部会不断尝试与U+云建立连接。

	注意：
	1、	devId中不能出现小写字母，否则会导致设备添加失败。
	2、	设备重复添加会返回错误码

	相关概念和术语
		devId：自定义的设备id或mac,由大写字母和数字组成，长度不超过16位。
		typeId：开发者在海极网中创建硬件设备的typeid
		swVersion：创建的硬件设备的软件版本号，纯数字组成，格式遵循：xxxx.xxxx.xxxx。举例：1.0.01、1.01.001、0001.1001.1001
		hwVersion：创建的硬件设备的硬件件版本号,纯数字组成，格式遵循：xxxx.xxxx.xxxx。举例：1.0.01、1.01.001、0001.1001.1001
		deviceKey：用于设备的合法性验证，目前需要联系张磊进行配置，开发者不能随意填写。邮箱：zhanglei.uh@haier.com


	示例代码：

	final USmartDevice device = new USmartDevice(deviceID, typeID,
          "2.0.0","2.0.0", deviceKey);
	device.setDeviceListener(SmartListener.getInstance());
	USmartDeviceManager.getInstance().addDevice(device, new IuSDKResultCallback<String>() {    
    @Override     
  	 public void onSuccess(String mac) {          
	//mac 为添加成功后的设备mac，此mac已经添加了16位的前缀
	Log.d("uSDK", "mac : "+mac );         
   	openDeviceBtn.setVisibility(View.VISIBLE);         
	String msg = "smart device 添加成功";              
	Toast.makeText(getApplicationContext(), msg,1).show();         deviceInfoText.setText("deviceID:" + mac + "\n" + "typeID:" + typeID);     
    }      
	 @Override     
 	public void onFail(uSDKErrorConst result) {         
	Log.d("uSDK", "失败的错误码 : "+result.getErrorId() );  
	String msg = "smart device 添加失败";    
	Toast.makeText(getApplicationContext(), msg,1).show(); 
 	} 
	});


##### 4.5.	添加授权设备

	授权设备也是SmartDevice实现的设备，此设备添加成功绑定后，再经过uSDK端为其进行授权操作后，可以编程实现和帐号下的其它设备交互。

	示例代码如下：

	SmartDeviceManager.addAuthDevice(
	leaderDevice, new IuSDKResultCallback<String>() {
    String msg;

    @Override
     public void onSuccess(String s) {
         msg = "设备添加成功：" + s;
         ……
     }

     @Override
     public void onFail(uSDKErrorConst result) {
       msg = "设备添加失败：" + result;
                Toast.makeText(
                        getApplicationContext(), msg, Toast.LENGTH_SHORT).show();
         ……
      }
	});

##### 4.6.	删除设备

 	开发者不需要使用设备接入功能，或者需要退出APP之前，需要将之前添加的设备实例从SDK中移除。API可以删除普通设备和授权设备。

	示例代码：

	USmartDeviceManager.getInstance().delDevice(deviceID, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "smart device 删除成功";
        } else {
            msg = "smart device 删除失败：" + errorConst;
        }
        Toast.makeText(MainActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
});


##### 4.7.	连云状态回调

	当设备与云平台交互时，SDK会触发云平台状态回调通知App。实现IUSmartDeviceManagerListener接口并注册该接口得到信息。另外大数据上报，开启绑定时间窗，属性状态、报警上报等各项功能都依靠云通信，所以需要重点关注和云连接状态。SmartDevice和云的连接是免维护的，自带重连机制。

	示例代码：

	//state ：251 和云连接成功
	//state ：252 和云建立连接失败
	public void onCloudState(int state) {
    String msg = "onCloudState state : " + state;
	//实现自己的业务
	}


##### 4.8.	设备状态上报

 	开发者可以通过接入的USmartDevice设备实例实现设备属性状态上报，每次上报的都是属性值全集，不能单个属性上报。具体的属性名和属性值需要参考在海极网中创建的硬件设备的属性集合。

	此操作的数据上报成功与否依赖网络连接及SDK和云的连接状态，执行前应进行相应的状态判断。

	相关概念和术语
		pairName：属性名，海极网中创建的硬件设备的属性名。
		pairValue：属性值，海极网中创建的硬件设备的属性值。

	示例代码：

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
        Toast.makeText(SmartDeviceDetailActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
	});

##### 4.9.	设备报警上报

	开发者可以通过接入的USmartDevice设备实例实现设备报警上报，具体的报警属性名和报警属性值需要参考在海极网中创建的硬件设备的报警属性集合。

	开发者根据业务需求可以一次上报单个报警，也可以上报多个报警。此操作的数据上报成功与否依赖网络连接及SDK和云的连接状态，执行前应进行相应的状态判断。

	相关概念和术语
		pairName：属性名，海极网中创建的硬件设备的报警属性名。
		pairValue：属性值，海极网中创建的硬件设备的报警属性值。

	示例代码：
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
        Toast.makeText(SmartDeviceDetailActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
	});


##### 4.10.	设备大数据上报

	开发者可以通过接入的USmartDevice设备实例实现设备大数据上报，具体的大数据内容需要参考在海极网中创建的硬件设备的大数据。

	此操作的数据上报成功与否依赖网络连接及SDK和云的连接状态，执行前应进行相应的状态判断。


	相关概念和术语
		type：大数据类型，海极网中创建的硬件设备的大数据类型。
		data：大数据内容，海极网中创建的硬件设备的大数据内容。

	示例代码：
	String type = "String";
	String data = "This is a BigData data! Now is " + System.currentTimeMillis();
	mSmartDevice.reportBigData(type, Base64.encodeToString(data.getBytes(), Base64.NO_WRAP), new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "reportBigData 成功";
        } else {
            msg = "reportBigData 失败：" + errorConst;
        }
        Toast.makeText(SmartDeviceDetailActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
	});


##### 4.11.	开启绑定时间窗口

	开发者需要对SmartDevice接入设备进行控制或获取设备属性时，首先需要开启SmartDevice接入设备的绑定时间窗，然后使用APP对接入设备进行安全绑定，App端绑定成功后，才能和SmartDevice接入设备进行交互。

 	开启绑定时间窗成功后，APP端可以开始设备安全绑定，每次开启有效时间为10分钟，APP端需要在10分钟内完成安全绑定功能，超过10分钟需要重新开启时间窗。

	注意：
	1、开启绑定时间窗功能需要访问U+云，所以调用此方法前需要保证网络畅通，能够正常访问外网。
	2、建议开启绑定时间窗时，增加重试策略，直到开启成功为止。

	相关概念和术语
		timeOut：方法请求的超时时间，建议5-10秒

	示例代码：
	int timeOut = 5；//单位：秒
	mSmartDevice.bindWindow(timeOut, new IuSDKCallback() {
    @Override
    public void onCallback(uSDKErrorConst errorConst) {
        String msg;
        if (uSDKErrorConst.RET_USDK_OK == errorConst) {
            msg = "开启绑定时间窗成功";
        } else {
            msg = "开启绑定时间窗失败：" + errorConst;
        }
        Toast.makeText(SmartDeviceDetailActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
	});


##### 4.12.	读属性回调

 	SmartDevice接入设备接收到读属性命令（如uSDK命令）时，SmartDevice SDK开发者需要实现IUSmartDeviceListener类的该接口方法，根据业务需要处理该读属性命令并完成读属性应答，并调用设备属性上报方法上报设备所有属性。

	例如：在读属性回调中接收到读取开机命令，需要将该命令经过处理后发送给设备底板，将底板执行结果赋值给USmartReadRsp对象，并调用设备属性上报方法上报设备所有属性。具体实现见return device.read(reqSn, name);

	相关概念和术语
		读属性回调：通过该接口可以获取指定设备的属性
		读属性应答：执行读属性回调后需调用此接口将结果返回给uGW server

	示例代码：
	public USmartReadRsp onDeviceReadCallback(String devId, int reqSn, String name) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        USmartReadRsp readRsp = new USmartReadRsp(reqSn);
        readRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return readRsp;
    }
    return device.read(reqSn, name);
	}


##### 4.13.	写属性回调

	SmartDevice接入设备接收到单命令或写属性命令（如uSDK命令）时，SmartDevice SDK开发者需要实现IUSmartDeviceListener类的该接口方法，根据业务需要处理该写属性命令，并返回写属性应答结果和最新的设备所有属性。

 	例如：在写属性回调中接收到开机命令，需要将该命令经过处理后发送给设备底板，将底板执行结果赋值给UBaseSmartRsp对象并返回，然后调用属性上报方法上报设备所有属性。具体实现见device.write(reqSn, name, value)；

	相关概念和术语
		写属性回调：通过该接口可以更新接入设备的可写属性
		写属性应答：执行写属性回调后需调用此接口将结果返回给uGW server

	示例代码：
	public UBaseSmartRsp onDeviceWriteCallback(String devId, int reqSn, String name, String value) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        UBaseSmartRsp smartRsp = new UBaseSmartRsp(reqSn);
        smartRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return smartRsp;
    }
    return device.write(reqSn, name, value);
	}


##### 4.14.	操作/组命令回调

	SmartDevice接入设备接收到组命令或操作命令（如uSDK发送）时，SmartDevice SDK开发者需要实现IUSmartDeviceListener类的该接口方法，根据业务需要处理该操作命令，并返回操作应答结果和最新的设备所有属性。

 	例如：在操作或组命令回调中接收到获取所有属性命令，需要将该命令经过处理后发送给设备底板，将底板执行结果和设备所有属性赋值给USmartOpRsp对象并返回，完成控制结果和所有属性的上报。具体实现见device.op(reqSn, opName, pairList);

	相关概念和术语
		操作性回调：通过该接口可以更新接入设备的可操作属性
		操作性应答：执行操作回调后需调用此接口将结果返回给uGW server

	示例代码：
	public USmartOpRsp onDeviceOpCallback(String devId, int reqSn, String opName, List<USmartDevicePair> pairList) {
    UplusDevice device = MyApplication.getUplusDevice(devId);
    if (null == device) {
        USmartOpRsp opRsp = new USmartOpRsp(reqSn);
        opRsp.setResult(USmartDeviceManager.RESULT_ERR);
        return opRsp;
    } return device.op(reqSn, opName, pairList);
	}




[^-^]:常用图片注释  
[SmartDeviceSDK_1]:../_media/_SmartDeviceSDK/SmartDeviceSDK_1.png
[SmartDeviceSDK_2]:../_media/_SmartDeviceSDK/SmartDeviceSDK_2.png
[SmartDeviceSDK_3]:../_media/_SmartDeviceSDK/SmartDeviceSDK_3.png
[SmartDeviceSDK_4]:../_media/_SmartDeviceSDK/SmartDeviceSDK_4.png
[SmartDeviceSDK_5]:../_media/_SmartDeviceSDK/SmartDeviceSDK_5.png
[SmartDeviceSDK_6]:../_media/_SmartDeviceSDK/SmartDeviceSDK_6.png
[SmartDeviceSDK_7]:../_media/_SmartDeviceSDK/SmartDeviceSDK_7.png

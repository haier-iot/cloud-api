# 移动SDK相关问题


**1.ios usdk接入无法初始化**

?> 答：使用过程中，usdk无法初始化，提报如下错误

```
2020-06-30 15:33:01.587593+0800 uSDKDevDemo_3.x[746:98133] [uSDK] [uSDK][Phone 5.8.1] [ERROR] [uSDKCommunication.m:163]-[uSDKCommunication receiveJson:] [model from JsonData error: Error Domain=JsonModelUtil Code=-11105 "无法反射创建对象:PMModuleStartResp" UserInfo={NSLocalizedDescription=无法反射创建对象:PMModuleStartResp}]

```
?> 这个问题是由于configFiles.bundle 资源包，没有放到工程里，需要开发者在配置时候，必须将资源放在工程里。

**2.usdk日志级别如何设置**

?> 答：uSDK 默认输出所有日志，日志标签是 uSDK。uSDK 提供 API 支持 App 设定日志输出 级别，uSDK 启动成功后，执行即可：`uSDKManager.initLog(uSDKLogLevelConst level, boolean isWriteToFile, IuSDKCallback)`

?>  日志级别：
```
public static final int DEBUG = 0x01; 
public static final int INFO = 0x02; 
public static final int WARNING = 0x04; 
public static final int ERROR = 0x08; 
public static final int NONE = 0x10;

```

##### 3、Android 音视频接入libc++_shared.so库冲突

?> 答：如果libc++_shared.so有冲突：

```
packagingOptions {
    pickFirst 'lib/arm64-v8a/libc++_shared.so'
    pickFirst 'lib/armeabi/libc++_shared.so'
    pickFirst 'lib/armeabi-v7a/libc++_shared.so'
}
```
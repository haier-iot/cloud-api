# uSDK-Android资料下载 

## 支持有效期   

支持有效期：新版本uSDK发布起，APP新接入大版本SDK的支持有效期为6-12个月，APP新接入小版本SDK的支持有效期为3-6个月。  

## 版本资料 

### Android uSDK_8.6.2          

本号： v8.6.2

发布日期：2021.06.16

MD5值：B41F88E95DB2010CE4C9013473ADD45C

下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.6.2_Phone_Android_20210615191055_20210617154440790.zip)  



更新日志：

MonitorPlayer、MonitorPlaybackPlayer类 新增接口

1、新增接口说明

/**

 \* 设置分辨率

 \* @param videoDefinition

 \* @param iCallback

 */

public void changeDefinition(VideoDefinition videoDefinition, ICallback<Void> iCallback)

 

/**

 \* 获取分辨率

 \* @return

 */

public VideoDefinition getDefinition()

 

/**

 \* 截图

 \* @param path

 \* @param iScreenshotCallback

 */

public void takeScreenshot(String path, IScreenshotCallback iScreenshotCallback)

 

/**

 \* 开始录像

 \* @param path

 \* @param iRecordingCallback

 \* @return

 */

public boolean startRecording(String path, IRecordingCallback iRecordingCallback)

 

/**

 \* 停止录像

 */

public void stopRecording()

 

/**

 \* 是否正在录像

 \* @return

 */

public boolean isRecording()

2、MonitorPlayerListener.java新增速率回调接口

/**

 \* 视频接收速率（轮询去查，每两秒查询一次）

 \* @param speed 字节/秒

 */

void updateSpeed(int speed)

3、新增VideoDefinition枚举

/**

 \* 音视频分辨率枚举

 */

public enum VideoDefinition {

 

  /**

   \* 高清

   */

  HD,

 

  /**

   \* 标清

   */

  SD,

 

  /**

   \* 流畅

   */

  FL

}

4、新增IScreenshotCallback回调

/**

 \* 截图回掉接口

 */

public interface IScreenshotCallback {

 

  /**

   \* 截图回掉

   \* @param path

   \* @param error

   */

  void onResult(String path, uSDKError error);

}

5、新增IRecordingCallback回调

/**

 \* 录像功能回掉

 */

public interface IRecordingCallback {

 

  /**

   \* 录像回掉

   \* @param path

   \* @param error

   */

  void onResult(String path, uSDKError error);

 

 

  /**

   \* 开始录像回调

   */

  void onStartRecord();

}

6、NFCInfo增加custom字段，下面当前的NFCInfo字段说明

/**

 \* 序列号 (required)

 */

private String NFCSerialNumber;

 

/**

 \* 华为 productId (options)

 */

private String hwProductID;

 

/**

 \* mac地址 (required)

 */

private String MAC;

 

/**

 \* 产品编码 (required)

 */

private String productCode;

 

/**

 \* 设备ID (required)

 */

private String deviceID;

 

/**

\* 自定义字段

*/

private String custom;

 

/** 其它字段均为uSDK内部使用，APP不可使用 */

### Android uSDK_8.6.1          



版本号： v8.6.1  

发布日期：2021.06.03  

MD5值：6DA4F528B6A8383FB23ADE3BB4EDDAEC 

下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.6.1_Phone_Android_20210603155147_20210609094916846.zip)  

更新日志：  

新增接口  

1、新增搜索发现扩展信息Bean

/**
 * 扩展信息
 *
 * @author 18009912 on 2021-04-12
 * @version 8.6.0
 */
public class ExtendInfo {

    /**
     * 扩展协议办办号
     */
    private final String protoVer;

    /**
     * 秘钥版本
     */
    private final String keyVer;

    /**
     * 支持的配网方式
     */
    private final String configVer;

    /**
     * 硬件版本号
     */
    private final String hwVer;

    /**
     * 硬件平台
     */
    private final String hwPlatform;

    public ExtendInfo(String protoVer, String configVer, String keyVer,
                  String hwVer, String hwPlatform) {
        this.protoVer = protoVer;
        this.configVer = configVer;
        this.keyVer = keyVer;
        this.hwVer = hwVer;
        this.hwPlatform = hwPlatform;
    }

    public String getProtoVer() {
        return protoVer;
    }

    public String getKeyVer() {
        return keyVer;
    }

    public String getHwVer() {
        return hwVer;
    }

    public String getHwPlatform() {
        return hwPlatform;
    }

    public String getConfigVer() {
        return configVer;
    }
}  

2、新增配置类型

ConfigType.java新增枚举

/**
 * SmartLink配置方式
 *
 * 1 << 7
 * @since v8.6.0
 */
SMARTLINK(ConfigurableDeviceInfo.CFG_SMARTLINK, "SmartLink"),

/**
 * 支持先绑后配
 *
 * 1 << 8
 * @since v8.6.0
 */
BIND_WITHOUT_WIFI(ConfigurableDeviceInfo.CFG_BIND_WITHOUT_WIFI, "先绑后配");  

3、配置绑定新增无网绑定接口

Binding.java新增bindDeviceWithoutWifi接口

无网绑定

<p>BLE绑定依次上报以下三个状态</p>
<li>连接设备:{@link BindProgress#CONNECT_DEVICE}</li>
<li>设备绑定:{@link BindProgress#BIND_DEVICE}</li>   

 <p>相关错误码说明：</p>  

 *   <li><strong><a>0:</a></strong> 接口执行成功</li>
 *   <li><strong><a>-10001:</a></strong> 无效参数错误</li>
 *   <li><strong><a>-10002:</a></strong> SDK未启动</li>
 *   <li><strong><a>-10003:</a></strong> 接口执行超时，建议30-180s，默认60s</li>
 *   <li><strong><a>-10006:</a></strong> 内部错误</li>
 *   <li><strong><a>-13020:</a></strong> 密码长度不符合要求</li>
 *   <li><strong><a>-14037:</a></strong> 蓝牙开关被关闭</li>
 *   <li><strong><a>-13031:</a></strong> 发送配置请求超时</li>
 *   <li><strong><a>-16013:</a></strong> 小循环没搜到且mqtt或https消息超时</li>
 *   <li><strong><a>-16018:</a></strong> 绑定超时需要重试绑定</li>
 *   <li><strong><a>-16012:</a></strong> 小循环搜到了且mqtt或https消息超时</li>
 *   <li><strong><a>-14010:</a></strong> 请先调用connectToGateway接口</li>
 *   <li><strong><a>-13006:</a></strong> 设备正在配置中</li>
 * </ul>
 *
 * @param bindInfo 蓝牙绑定的路由器信息
 * @param callback 绑定结果回调接口
 * @since v8.6.0 2021-4-26 13:48:00
 */
@Keep
public void bindDeviceWithoutWifi(WithoutWifiBindInfo bindInfo, IBindResultCallback<uSDKDevice> callback);  

4、可发现设备信息ConfigurableDevice变更

移除获取设备型号信息方法（getDeviceModelInfo()）

新增其他属性获取

/**
 * 获取扩展字段
 *
 * @return 扩展字段
 * @since v8.6.0
 */
public ExtendInfo getExtendInfo();

/**
 * 获取型号端ID
 *
 * @return 型号端ID
 * @since v8.6.0
 */
public String getModelShortID();

/**
 * 获取可支持的配置方式
 *
 * @return 可支持的配置方式
 * @since v8.6.0
 */
public int getAllConfigTypes();

/**
 * 获取设备分类名称
 *
 * @return 设备分类名称
 * @since v8.6.0
 */
public String getAppTypeName();  

5、绑定信息新增设置和获取NFCInfo

(1) 以下类新增获取NFCInfo和通过Builder模式设置NFCInfo，并且支持在已有信息基础上重新构建。

BLEAdvBindInfo.java
BLEBindInfo.java
BLEMeshBindInfo.java
BaseBleBindInfo.java
CameraQRCodeBindInfo.java
NewDirectLinkManualConfirmBindInfo.java
NewDirectLinkVerificationCodeBindInfo.java
PureBLEBindInfo.java
QRCodeBindInfo.java
SlaveDeviceBindInfo.java
SmartLinkBindInfo.java
SoftApBindInfo.java
WithoutWifiBindInfo.java

/**
 * 获取NFC信息
 */
public NFCInfo getNFCInfo();

/**
 * 支持重新构建
 */
public Builder rebuild();
(2) 以下构建信息中的设置可配置设备方法标记为废弃

BLEAdvBindInfo.java
BLEBindInfo.java
BLEMeshBindInfo.java
PureBLEBindInfo.java
WithoutWifiBindInfo.java

/**
 * 设置可配置的设备对象
 *
 * @param configurableDevice 可配置的设备对象
 * @see Builder#setConfigurableDevice(com.haier.uhome.usdk.scanner.ConfigurableDevice)
 * @deprecated Moved to builder
 */
@Deprecated
public void setConfigurableDevice(ConfigurableDevice configurableDevice);  

内部优化  

uSDK Client  

热点规则升级2.0功能，蓝牙、热点搜索和代理搜索优化;  

搜索设备融合逻辑优化增加对设备状态变化查询成品编码的处理；    

修复平台没有记录成品编码的老设备 不能可配置发现的问题；

针对于v5蓝牙广播协议Wif&BLE设备支持新配置绑定和先绑后配；  

更新路由器密码优化支持先绑后配设置模块配置信息；  

蓝牙OTA优化，支持复用连接；  

ClientID逻辑优化；  

SoftAp仅绑定模式下，bindCode强制设置为0；  

添加NFC配置绑定功能。  

ucom  

老版本模块更新密码时，不支持复用链接；  

蓝牙OTA优化，支持复用连接；  

支持先绑后配新增协议； 

其他若干优化  

CAE  

事件广播解析器支持V3协议，usdk兼容性将事件广播版本与通用广播版本统一  

云芯二代v5协议扩展字段解析。  


### Android uSDK_8.5.0  

- 版本号： v8.5.0

- 发布日期：2021.04.30

- MD5值：A32B8E21261D917CE8BE775004FE83F3

- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.5.0_Phone_Android_20210429155736_20210511092354389.zip) 

- 版本日志 

	**新增接口**

  1.新增类
  MonitorPlaybackPlayer – 音视频回放核心类
  IMonitorPlayerListener – 音视频回放状态接口
  MonitorPlaybackListener – 获取音视频列表回调类
  MessagePlaybackNode – 音视频播放节点信息
  MonitorCloudVideoListener – 获取云端音视频回调类

  2.MonitorPlaybackPlayer.java接口说明
  /** 
   * 初始化视频回放
   * @param sdkDevice
   * @param iCallback
     */
     public static void createPlaybackPlayer(uSDKDevice sdkDevice, VideoView videoView, ICallback<MonitorPlaybackPlayer> iCallback)
  
  示例：
  MonitorPlaybackPlayer.createPlaybackPlayer(mDevice, sdkVideoView, new ICallback<MonitorPlaybackPlayer>() {
      @Override
      public void onSuccess(MonitorPlaybackPlayer monitorPlaybackPlayer) {
          mMonitorPlaybackPlayer = monitorPlaybackPlayer;
          addPlaybackListener();
      }
  
      @Override
      public void onFailure(uSDKError error) {
           
      }
  
  });
  
  /**
  
   * 播放器状态回调监听
   * @param monitorPlayerListener
     */
     public void addPlayerListener(IMonitorPlayerListener monitorPlayerListener)
  
  /**
  
   * 获取设备端sd卡中存在录像的日期列表
   * @param startTime (单位毫秒)
   * @param endTime (单位毫秒)
   * @param pageIndex 查询页
   * @param countPerPage 每页大小
   * @param monitorPlaybackListener
     */
     public void getPlaybackDateList(long startTime, long endTime, int pageIndex, int countPerPage, MonitorPlaybackListener<MonitorPlaybackExistDateMessage> monitorPlaybackListener)

  /**
  
   * 获取回放详情列表
   * 开始时间-结束时间的时间差不允许大于一个月
   * @param startTime (单位毫秒)
   * @param endTime (单位毫秒)
   * @param pageIndex 查询页
   * @param countPerPage 每页大小
   * @param recordType 筛选类型, null为不筛选
   * @param monitorPlayerListener 回调
     */
     public void getPlaybackList(long startTime, long endTime, int pageIndex, int countPerPage, String recordType, MonitorPlaybackListener<MonitorPlaybackMessage> monitorPlayerListener)
  /** 
   * 指定时间播放
   * @param startTime 时间（毫秒）
   * @param messagePlaybackNode 播放节点信息
     */
     public void seekToTime(long startTime, MessagePlaybackNode messagePlaybackNode)
  /**
   * 播放
     */
     public void videoPlay()
  /**
   * 是否正在播放
   * @return
     */
     public boolean isPlaying()
  /** 
   * 暂停
     */
     public void pause() 
  /** 
   * 继续
     */
     public void resume()  
  /** 
   * 停止
     */
     public void videoStop()  
  /** 
   * 设置是否静音
   * @param on true(静音) false(非静音)
     */
     public void mute(boolean on) 
  /** 
   * 设置播放数据信息
   * @param startTime （毫秒）
   * @param messagePlaybackNode 播放节点信息
     */
     public void setDataResource(long startTime, MessagePlaybackNode messagePlaybackNode)
  /** 
   * 获取可以查看的照片列表
   * @param startTime (单位秒)
   * @param endTime (单位秒)
   * @param count 每页条数 0~10
   * @param photoId 图片ID,查询首页图片传0，查询下一页传上一个最后一个图片的ID
   * @param monitorPlaybackListener
     */
     public void getDevicePhotoList(long startTime, long endTime, int count, int timeout, MonitorPlaybackListener<List<MonitorPhotoListBean>> monitorPlaybackListener) 
  /**
  
   * 获取本地图片详情
   * @param photoId 图片ID
   * @param timeout 超时时间  单位秒
     */
     public void getLocalPhotoDataByPhotoId(int photoId, int timeout, ICallback<byte[]> iCallback) 
  /**  
   * 获取有云存视频可播放日期信息
   * 用于终端用户在云存页面中对云存服务时间内的日期进行标注，区分出是否有云存视频文件
   * @param timezone 相对于0时区的秒数，例如东八区28800
   * @param monitorCloudVideoListener
     */
     public void getCloudVideoDateListByTimezone(int timezone, MonitorCloudVideoListener<DateBean> monitorCloudVideoListener)
  /**  
   * 获取云回放存在视频数据的时间段列表
   * 由于查询的数据大小限制，起始到结束时间差需要小于等于一天
   * @param startTime 单位秒
   * @param endTime 单位秒
   * @param monitorCloudVideoListener 回调
     */
     public void getCloudVideoPlayListByDeviceId(long startTime, long endTime, MonitorCloudVideoListener<List<VideoPlayListBean.DataBean>> monitorCloudVideoListener)  
  /** 
   *  获取回放的 m3u8 列表
   *  @param startTime 单位秒
   *  @param endTime 单位秒
   *  @param monitorCloudVideoListener 回调
      */
      public void getVideoPlayAddressByDeviceId(long startTime, long endTime, MonitorCloudVideoListener<PlayAddressBean> monitorCloudVideoListener) 
  /** 
   * 释放资源
     */
     public void destroyPlayer()
  
  2. 新增回调接口
     public interface IMonitorPlayerListener {  
      /** 
       * 播放器状态回调
       * @param status
         */
          void onPlayerStatusUpdate(MonitorPlayerStatus status);  
      /**  
       * 播放错误回调
       * @param error
         */
          void onReceiveError(uSDKError error);  
      /**  
       * 音视频回放时间戳
       * @param time 单位毫秒
         */
          void onTime(long time);  
      /** 
       * 回放文件播放结束
       * @param time 单位毫秒
         */
          void onPlayFileFinished(long time);
         }
  
  3. 新增回调接口
     public interface MonitorPlaybackListener<T> {  
      /**  
       * 获取视频列表开始
         */
          void onStart();  
      /**  
       * 获取视频列表成功
       * @param t
         */
          void onSuccess(T t); 
      /**  
       * 获取视频列表失败
       * @param sdkError
         */
          void onError(uSDKError sdkError);
         }
  
  4. 播放节点信息类
     public class MessagePlaybackNode {
      public long startTime;
      public long endTime;
      public String recordType = "";
  
      public long getStartTime() {
          return startTime;
      }
  
      public void setStartTime(long startTime) {
          this.startTime = startTime;
      }
  
      public long getEndTime() {
          return endTime;
      }
  
      public void setEndTime(long endTime) {
          this.endTime = endTime;
      }
  
      public String getRecordType() {
          return recordType;
      }
  
      public void setRecordType(String recordType) {
          this.recordType = recordType;
      }
  
      public long getDuration(){
          return endTime - startTime;
      }
  
      @Override
      public String toString() {
          return "MessagePlaybackNode{" +
                  "startTime=" + startTime +
                  ", endTime=" + endTime +
                  ", recordType='" + recordType + '\'' +
                  '}';
      }
     }
  
  5. 获取音视频云播放列表类
     public interface MonitorCloudVideoListener<T> {
  
      /**
  
       * 获取云端视频开始
         */
          void onStart();
  
      /**
  
       * 获取云端视频成功
       * @param t
         */
          void onSuccess(T t);
  
      /**
  
       * 获取云端视频失败
       * @param sdkError
         */
          void onFail(uSDKError sdkError);
         }
  
  6. 图片类型枚举
     public enum PlayerImageType {
  
      /**
  
       * 报警图片
         */
  
      IMAGE_TYPE_ALARM,
  
      /**
  
       * 事件图片
         */
          IMAGE_TYPE_EVENT,
  
      /**
  
       * 普通图片
         */
          IMAGE_TYPE_NORAM,
         }
  
  7. ErrorConst类中新增枚举
     /**
  
   * 视频功能内部错误
     */
     ERR_USER_MONITOR_FUNCTION_INTERNAL(-20001, "视频功能内部错误"),
  
  /**
  
   * 无本地回放视频
     */
     ERR_USER_NO_LOCAL_PLAYBACK(-20002, "无本地回放视频"),
  
  /**
  
   * 视频分辨率已改变
     */
     ERR_USER_MONITOR_RESOLUTION_CHANGED(-20003, "视频分辨率已改变"),
  
  /**
  
   * 超过设备可支持的最大P2P通道数
     */
     ERR_USER_MAXIMUM_CHANNEL_EXCEEDED(-20004, "超过设备可支持的最大P2P通道数"),
  
  /**
  
   * P2P通道消息发送失败
     */
     ERR_USER_CHANNEL_MESSAGE_SEND_FAILED(-20005, "P2P通道消息发送失败"),
  
  /**
  
   * P2P通道消息发送超时
     */
     ERR_USER_CHANNEL_MESSAGE_SEND_TIMEOUT(-20006, "P2P通道消息发送超时"),
  
  /**
  
   * 设备正在录制
     */
     ERR_USER_DEVICE_RECORD(-20007, "设备正在录制"),
  
  /**
  
   * APP端通道连接数已达上限
     */
     ERR_USER_CHANNEL_NUMBER_FULL(-20008, "APP端通道连接数已达上限"),
  
  /**
  
   * 获取数据失败
     */
     ERR_USER_FAIL_GET_DATA(-20009, "获取数据失败");
  
  8. uSDKDevice.java接口变更
     /**
  
  * 订阅资源
  * 需要在{@link #setDeviceListener(IuSDKDeviceListener)}时传入接口{@link IuSDKDeviceListenerWithResource}的实现
    *
  * @param resName  资源名称
  * @param callback 回调接口
  * @since v8.5.0
    */
    public void subscribeResourceWithDecode(String resName, ICallback<Void> callback)
  ​```
  9. ```
     DeviceListener.java接口变更
     public void onReceiveDecodeResource(uSDKDevice device, String resource, String data)
     ```
  ```
  
  ```





### Android uSDK_8.4.0

- 版本号： v8.4.0 
- 发布日期：2021.03.23
- MD5值：86F55528A8C6829669DCD4E00B196CF3
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.4.0_Phone_Android_20210323100244_20210330132915768.zip)  
- 版本日志 

	**新增接口组相关接口变更**  

1. 新增回调接口         


    /**
     *进度回调接口    
     *@param <PT> 参数化类型之入参类型
     *@param <CT> 参数化类型之结果类型
     *@since v8.5.0   
     */
    public interface IProgressCallback<PT, CT>{
     
    /**
     *当处理进度
     *
     *@param pt正在处理的对象
     *@param error 错误码
    */
    @Keep
    void onProgress(PT pt, uSDKError error);
     
    /**
     *回调完成
     *
     *@param ct处理的结果对象
     *@param error 错误码
     */
    @Keep
    void onComplete(CT ct, uSDKError error);
    }

 











2. uSDKDevice.java接口变更    


    /**
     *获取可与当前设备分到同一组的设备列表, 当前设备要求有ZigBee能力或BLEMesh能力
     *
     *@param callback 接口执行完成时回调; 失败时，回调具体错误码; 接口执行成功时，devices也可能为空，表示接口执行成功，但没有可分组设备
     *@since v8.5.0
     */
    public void fetchGroupableDeviceList(ICallback<List<uSDKDevice>> callback);
     
    /**
     * 创建分组，返回组设备对象,
     * 创建完成后需要主动调用 {@link #addDevicesToGroup(List, int, IProgressCallback)}添加设备
     *
     * @param timeout  超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
     * @param callback 创建分组， 失败时，回调具体错误码; 接口执行成功时，回调创建好的组设备
     * @since v8.5.0
     */
    public void createGroup(int timeout, ICallback<uSDKDevice> callback);


​     
​    /**
​     * 向组设备中添加设备，要求当前device对象为组设备
​     *
​     * @param deviceList 需要添加到组设备的设备列表
​     * @param timeout超时时间，取值范围30-180秒
​     * @param callback   结果、进度回调（包括每个设备的添加情况，或未能添加的错误原因）
​     * @since v8.5.0
​     */
​    public void addDevicesToGroup(List<uSDKDevice> deviceList, int timeout, IProgressCallback<uSDKDevice, Void> callback);


​     
​    /**
​     * 从组设备中移除设备，要求当前device对象为组设备
​     *
​     * @param deviceList 待移除的mesh设备列表
​     * @param timeout超时时间
​     * @param callback   结果、进度回调（包括每个设备的移除情况，或未能移除的错误原因）
​     * @since v8.5.0
​     */
​    public void removeDevicesFromGroup(List<uSDKDevice> deviceList, int timeout, IProgressCallback<uSDKDevice, Void> callback);
​     
​    /**
​     * 删除组设备，要求当前device对象为组设备
​     *
​     * @param callback 删除结果回调
​     */
​    public void deleteGroup(ICallback<Void> callback);


	**内部优化**     

uSDK Client：     
1.新增zigbee组设备支持；   
2.H2面板增加了zigbee连接方式的支持；   
3.网络异常的情况下，应用分类列表使用本地缓存；   
4.本机设备不依赖绑定   
5.连接中的状态同步调整；   
6.修复蓝牙OTA升级最后一包无法发现设备的问题；     


组件：    
1.新增组控设备的支持    
2.新增小夜灯的支持    
3.需求11000, usdk连云失败，门锁设备制造报警，app端报警信息无变化    
4.针对于重复订阅错误码优化，23003转23013    

CAE    
1.设置COAP透传通道最大数据长度为1024   
2.蓝牙配置，除了获取type    



### Android uSDK_8.3.0

- 版本号： v8.3.0
- 发布日期：2021.03.03
- MD5值：7A466ACF6E977807AC1971DFF6D64AAA
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.3.0_Phone_Android_20210303124515_20210315163741095.zip)     
- 版本日志 

  **新增接口**  

新增NFC标签功能接口：    
1.新增类     
NFCUtil – NFC 标签解析、更新工具类   
NFCInfo – NFC bean 对象    
ICallback – 请求的回调     

2.NFCUtil.java接口说明     

    /**
     * 解析NFC标签数据，异步
     *
     * @param ndefRecord NFC标签原始数据，例如：
     *   zj.haier.net?untype=original&nsn=FFFF00000000&mac=C00000000000&model=CEAAJXX00&hwp=A0VF&s=abcdef
     *
     * @param completed  接口回调 成功返回 NFCInfo 类对象， 错误返回 uSDKError
     *   {@link NFCInfo}
     *   {@link ICallback#onSuccess(Object)}
     *   {@link ICallback#onFailure(uSDKError)}
     */
    public static void parseNFCTagData(String ndefRecord, ICallback<NFCInfo> completed);
    /**
     * 更新 NFC设备信息，异步，需在 NFCInfo 中设置 deviceID。
     *
     * @param info  NFCInfo对象， deviceId, nfcSerialNumber, mac 和 productCode 为必填项
     *  {@link NFCInfo#setDeviceID(String)}
     *  {@link NFCInfo#setNFCSerialNumber(String)}
     *  {@link NFCInfo#setMAC(String)}
     *  {@link NFCInfo#setProductCode(String)}
     *
     * @param completed 接口回调 成功返回 Void，失败返回 uSDKError
     *  {@link ICallback#onSuccess(Object)}
     *  {@link ICallback#onFailure(uSDKError)}
     *
     */
    public static void updateNFCDeviceInfo(NFCInfo info, ICallback<Void> completed);


3.NFCInfo.java字段列表    

| **名称**                | **类型** | **是否必须** | **说明**       |
| ----------------------- | -------- | ------------ | -------------- |
| deviceID                | String   | 必须         | 设备ID         |
| hwProductID             | String   | 可选         | 华为 productId |
| MAC                     | String   | 必须         | Mac地址        |
| NFCSerialNumber  String | String   | 必须         | 序列号         |
| productCode             | String   | 必须         | 产品编码       |

**内部优化**     
uSDK:    
1.ClientId埋点；    
2.Smartlink配置失败返601时进行重试    
3.修复蓝牙门锁离线问题；    
4.移除行为统计功能代码；    
5.修复蓝牙OTA升级过程中同一账号下其他账号进度重复的bug。     

组件：   
1.在门锁通过手机进行LocalKey更新或者时间同步时，用户可以优先订阅该门锁，建立连接。    
2.修改的“蓝牙门锁升级时崩溃”问题。      

CAE：    
1.解决蓝牙搜索内存泄露的问题；    
2.ble ota超时时间和hal层超时时间（10秒）对齐，从2秒改成11秒;   
3.增加蓝牙beacon v0的数据解析，以便支持云芯二代配置失败，可以通过蓝牙beacon上报配置失败原因。   




### Android uSDK_8.2.0

- 版本号： v8.2.0
- 发布日期：2021.02.04
- MD5值：C92F76A66F7D4AC3C507A956E4198E6A
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.2.0_Phone_Android_20210205165559298.zip)     
- 版本日志 

	**注意事项**

注意事项1：不支持海外环境。     
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用。    
注意事项3：JDK版本最低版本要求：大于等于JDK8。   
Android Studio工程可以参考如下配置：     


    android {
    defaultConfig {
    //必须高于等于16
    minSdkVersion 16
    }  
    compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
    }
​    } 

	**新增功能**   
增加设备事件通知    
增加uSDKDeviceEvent实体类,包装收到的上报的设备事件，内部包含事件名称，事件类型和附带的属性信息    

    public class uSDKDeviceEvent {
    public String getName() {
    }
       
    public String getType() {
    }
       
    public List<DeviceAttribute> getAttrs() {
    }
     
    }
以上事件通过IuSDKDeviceListener接口上报。事件接口为Java8的默认实现，需要关注事件的可重写该接口    

    public interface IuSDKDeviceListener {
      void onDeviceEvent(uSDKDevice device, List<uSDKDeviceEvent> events);
      void onDeviceAlarm(uSDKDevice device, List<uSDKDeviceAlarm> alarms);
      void onDeviceAttributeChange(uSDKDevice device, List<uSDKDeviceAttribute> attrs);   
    }

2.接口变更   
 无    
3.内部优化及BUG修改   
3.1 支持蓝牙门锁历史数据获取   
3.2 支持蓝牙门锁OTA   
3.3 支持蓝牙门锁的绑定    





### Android uSDK_8.1.1

- 版本号： v8.1.1
- 发布日期：2021.1.8
- MD5值：25E61F109E9C717722D051591E8695DA
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.1_Phone_Android_20210114160830269.zip)    
- 版本日志 

	**注意事项**

注意事项1：不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用     

	**更新日志**  
1.新增功能   
 无   
2.接口变更   
  无   
3.内部优化及BUG修改   
3.1 该版本修复蓝牙体脂秤数据更新没有给app上报的问题。    



###  Android uSDK_8.1.0

- 版本号： v8.1.0
- 发布日期：2020.12.23
- MD5值：0E81AEEADB547D43BE4424F140E8168A
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.0_Phone_Android_20201228102147320.zip)    
- 版本日志 

	**注意事项**

注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用   

	**更新日志**   
1.新增功能   
1.1 蓝牙广播设备配置绑定埋点；    
1.2 新直连绑定埋点；    
1.3 增加smartLink转SoftAp配置绑定的接口    
1.3.1 Binding类中增加接口bindDeviceBySmartLinkAuto接口    

    /**
     * SmartLink配置绑定接口，将指定的设备接入指定的WiFi，
     * 并将设备绑定到云平台
     * 调用该接口前，需要成功调用uSDKDeviceManager中的connectToCloud接口
     *
     * 对比于{@link Binding#bindDeviceBySmartLink(SmartLinkBindInfo, IBindResultCallback)}
     * 本接口会在内部计算是否满足转softAp的条件，如果满足，自动执行SoftAp配置绑定
     *
     * SmartLink绑定依次上报以下两个状态
     * <ol>
     *  <li>发送配置信息:{@link BindProgress#SEND_CONFIG_INFO}</li>
     *  <li>设备绑定:{@link BindProgress#BIND_DEVICE}</li>
     * </ol>
     * <p>
     *
     * 如果满足转SoftAp，绑定依次上报以下三个状态
     * <ol>
     *  <li>连接设备:{@link BindProgress#CONNECT_DEVICE}</li>
     *  <li>发送配置信息:{@link BindProgress#SEND_CONFIG_INFO}</li>
     *  <li>设备绑定:{@link BindProgress#BIND_DEVICE}</li>
     * </ol>
     * <p>
     *
     * @param bindInfo 配置信息
     * @param cb   绑定结果回调接口
     */
    @Keep
    public void bindDeviceBySmartLinkAuto(SmartLinkBindInfo bindInfo, IAutoBindCallback<uSDKDevice> cb)

1.3.2  SmartLinkBindInfo#Builder增加应用分类和成品编码的写入接口   

 		/**
 	     * 设置应用分类
 	     * @param appTypeCode
 	     */
 	    public Builder appTypeCode(String appTypeCode)
 	
 	    /**
 	     * 设置成品编码
 	     * @param productCode
 	     */
 	    public Builder productCode(String productCode)

1.3.3 增加IAutoBindCallback接口类    

    /**
    IBindCallback继承自ISoftApResultCallback，并且未增加接口
    */
    public interface IAutoBindCallback<R> extends IBindCallback<R> {
    /**
     * uSDK内部自动连接模块热点失败，请求APP协助热点切换
     * @param softApSsid
     */
    void switchToSoftApRequest(String softApSsid);
    }

1.4 增加子机配置绑定中RISCO设备配置绑定接口    

 SlaveDeviceBindInfo#Builder增加自定义扩展参数的接口   

    /**
     * RISCO设备绑定，自定义扩展参数
     * @param extendInfo
     * @return
     */
    public Builder extendInfo(String extendInfo)

1.5 uSDK支持主东服务链路跟踪埋点    
1.5.1 Trace类增加带traceId的构建Trace对象的接口    

    /**   
     * 根据businessId和自定义traceId创建一个新的跟踪链对象,
     * 如果传入的businessId重复,则会将之前创建的对象更新为一个全新的链式跟踪
     *
     * @param businessId 业务Id
     * @param traceId 长队为32位的字串
     * @return 跟踪链对象
     */
    public static Trace createTrace(String traceId, String businessId) 

1.5.2 Trace类增加带traceId的addDITraceNode接口   

     /**
     * 添加DI跟踪节点(DI) <br/>
     * <p>
     * 适配CAE打点，如果customTraceId为空，则使用uTrace内置traceId打点，不为空,则使用传入的traceId作为id标识
     *
     * @param customTraceId 自定义traceId
     * @param node          DI跟踪节点对象
     * @return 发送的结果
     */
    public int addDITraceNode(String customTraceId, DITraceNode node) 

2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 uSDK - Android手机配网策略默认取值ble - 配置绑定   
3.2 融合搜索优化 & 问题修改；   
3.3 配网策略默认取值ble；    



###  Android uSDK_8.0.0

- 版本号： v8.0.0
- 发布日期：2020.12.23
- MD5值：0E81AEEADB547D43BE4424F140E8168A
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.0.0_Phone_Android_20201211175355894.zip)    
- 版本日志 

 **注意事项**  

注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    

 **更新日志**  

1.新增功能   
1.1 新增支持蓝牙体脂秤，绑定和属性上报    
1.1.1. ConfigType新增`BLE_ADV`枚举，代表新支持的蓝牙广播设备    
1.1.2. Binding内部新增void bindBLEAdvDevice接口   
1.1.3. 增加BLEAdvBindInfo实体类，传递广播设备发现信息   

     Builder 设置搜索上来的可发现设备，setConfigurableDevice(ConfigurableDevice)   

1.1.4. 属性上报通过原有的设备属性上报通知给App   
1.2. 增加P2P音视频能力设备接入   
1.2.1 增加`VideoView`控件承载视频播放View   
1.2.2 增加`MonitorPlayer`来持有媒体播放的控制功能   
1.3 增加`IMonitorPlayerListener`播放监听器   
     //播放器状态回调
     void onPlayerStatusUpdate(MonitorPlayerStatus status);
     
     // 播放错误回调
     void onReceiveError(uSDKError error);    
1.4 新增`MonitorPlayerStatus`枚举   
1.5 新增获取路由器信息    
1.5.1 新增实体类 （ConfigRouterInfo.java）   

       /**
      * 获取ssid   
      *
      * @return ssid of wifi   
      */
      public String getSsid();   
      /**
      * 获取路由器的bssid   
      *
      * @return bssid of wifi   
      */
      public String getBssid();   
      /**
      * 获取路由器的密码   
      *
      * @return password of wifi   
      */
      public String getPassword();   
      /**
      * 是否切换了网络 
      *
      * @return true: 切网，false: 正常未切网   
      */
      public boolean isNeedSwitchNetwork();   

1.5.2 2. Binding类中新增获取路由器信息接口     

        /**
       * 获取路由器信息
       *
       * @param timeout  超时时间
       * @param callback 获取路由器信息回调接口
       */
    public void getConfigRouterInfo(int timeout, ICallback<uSDKConfigRouterInfo> callback);

1.6 设备搜索新增接口   
1.6.1 新增搜索特性枚举    

       public enum SearchState {
       //启用SoftAp热点搜索
       WIFI_ENABLE(1),  
       //启用蓝牙搜索
       BLE_ENABLE(1 << 1),
       //已入网代理搜索
       PROXY_ENABLE(1 << 2),   
       //新直连搜索
       NEW_DIRECT_LINK_ENABLE(1 << 3);
       }   
1.6.2 新增特性控制接口(DeviceScanner.java)    

        /**
     * 使能搜索特性
     * 示例代码
     * <pre>{@code
     *  // 启用新直连搜索
     *  // 启用Wifi搜索
     *  // 启用已入网代理设备搜索
     *  // 启用蓝牙搜索
     *  int features = SearchState.WIFI_ENABLE.mask
     * | SearchState.NEW_DIRECT_LINK_ENABLE.mask
     * | SearchState.PROXY_ENABLE.mask
     * | SearchState.BLE_ENABLE.mask;
     *
     * enableSearchFeature(features);
     * }</pre>
     *
     * @apiNote 目前仅支持对SoftAp搜索使能控制，其他暂无法控制，默认是开启的
     * @param features 搜索特性
     * @since v8.0.0
     */
    public void enableSearchFeature(int features);

1.7  新增权限相关接口   
1.7.1 新增扫描权限监听接口    

      public interface ScannerListener {
      // 当权限不合法时
       //@param permission uSDK 需要的系统权限枚举
      void onPermissionInvalid(Permission permission);
      }   

1.7.2 新增权限控制枚举    

      public enum Permission {
      //蓝牙未打开
      BLE_DISABLE(),
      // 需要蓝牙相关权限
      BLE_INVALID(Manifest.permission.ACCESS_FINE_LOCATION),
      //wifi没有打开
      WIFI_DISABLE(),
      /**  
       * 需要Wifi相关权限
       * Android 9：
       * 成功调用 WifiManager.startScan() 需要满足以下所有条件：
       *
       * 1. 应用拥有 ACCESS_FINE_LOCATION 或 ACCESS_COARSE_LOCATION 权限。
       * 2. 应用拥有 CHANGE_WIFI_STATE 权限。
       * 3. 设备已启用位置信息服务（位于设置 > 位置信息下）。
       * Android 10（API 级别 29）及更高版本：
       * 成功调用 WifiManager.startScan() 需要满足以下所有条件：
       *
       * 如果您的应用以 Android 10（API 级别 29）SDK 或更高版本为目标平台，应用需要拥有 ACCESS_FINE_LOCATION 权限。
       * 如果您的应用以低于 Android 10（API 级别 29）的 SDK 为目标平台，应用需要拥有 ACCESS_COARSE_LOCATION 或 ACCESS_FINE_LOCATION 权限。
       * 应用拥有 CHANGE_WIFI_STATE 权限。
       * 设备已启用位置信息服务（位于设置 > 位置信息下）。
       * 若要成功调用 WifiManager.getScanResults()，请确保满足以下所有条件：
       *
       * 如果您的应用以 Android 10（API 级别 29）SDK 或更高版本为目标平台，应用需要拥有 ACCESS_FINE_LOCATION 权限。
       * 如果您的应用以低于 Android 10（API 级别 29）的 SDK 为目标平台，应用需要拥有 ACCESS_COARSE_LOCATION 或 ACCESS_FINE_LOCATION 权限。
       * 应用拥有 ACCESS_WIFI_STATE 权限。
       * 设备已启用位置信息服务（位于设置 > 位置信息下）。
       */
      WIFI_INVALID(Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION);
      
      private String[] permissions;
      
      Permission(String... perms) {
      permissions = perms;
      }
      
      @Keep
      @Nullable
      public String[] getSystemPermissions() {
      return permissions;
      }
      }

1.7.3 新增设置权限回调接口（ DeviceScanner.java）     
      `public void setScannerListener(ScannerListener listener);`

1.8 新增错误码   
1.8.1. `-16021`: 不支持获取配置的路由器信息     
1.8.2. `-16022`: 获取路由器信息失败   
1.8.3. `-16023`: 获取路由器信息超时   

2.接口变更   
 无   
3.内部优化及BUG修改     
3.1. 新增热点搜索&已入网设备代理搜索功能，以及都说搜索功能融合上报   
3.2. 新增uSDK从云端获取Wifi&Ble模块的配网策略   
3.3. 不支持bindCode的自绑定设备查询绑定结果接口变更；   
3.4. uSDK线程池优化，改为"4+4"模式;   
3.5. 其他若干代码优化以及Demo优化等   
3.6. 新增BindCode埋点字段   




###  Android uSDK_6.2.1

- 版本号： v6.2.1
- 发布日期：2020.10.14
- MD5值：72EE689C195650C7FB54C156E6B2B845
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.2.1_Phone_Android_20201023102752356.zip)      
- 版本日志 

 **注意事项**

注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用   
更新日志：   
1.新增功能   
 无   
2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 修改了获取蓝牙历史数据崩溃的问题   
3.2 修改了Android 11 系统下 偶发崩溃的问题   



###  Android uSDK_6.2.0

- 版本号： v6.2.0 
- 发布日期：2020.09.24
- MD5值：EB63AA7EB3639B710C231A0FE0CE30D3
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.2.0_Phone_Android_20200924135413217.zip)       
- 版本日志 

 **注意事项**
    
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用    
更新日志：  
1.新增功能    
 无     
2.接口变更      
 无    
3.内部优化及BUG修改   
3.1 SmartLink 配置仅配置接口不依赖本地上线包，收到ack后就认为配置成功   
3.2 SmartLink 配置绑定接口，不依赖本地上线，配置包发送完成后开始查询绑定结果。收到ack后，停止查询绑定结果，进行用户侧绑定。查询绑定结果和用户侧绑定均使用,从云平台获取到的bindCode。   
3.3 所有的绑定和查询绑定结果使用的bindCode类型变为无符号4字节整数。增加bindCodeFrom参数，1-云平台生成，0-sdk生成。绑定接口，如果组件返回190005认为绑定成功。   
3.4 重试绑定接口增加SR和SS埋点   
3.5 SmartLink cs/cr点携带平台返回的traceIdB，用户打通usdk和模块的链路日志。   
3.6 停止小循环对wifi模块的升级功能   



###  Android uSDK_6.1.1

- 版本号： v6.1.1 
- 发布日期：2020.09.15
- MD5值：DBDE4625AB59881CA4DB3531826C8347
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.1_Phone_Android_20200924135317509.zip)       
- 版本日志 

 **注意事项**

注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用  
更新日志：  
1.新增功能  
 无    
2.接口变更  
 无     
3.内部优化及BUG修改  
3.1 修复SoftAp配置过程NPE问题  
3.2 修复设备测绑定重复查询绑定结果问题  
3.3 修复了组件导致uSDK崩溃的问题  
3.4 修复SoftAp设备BindCode能力判断错误问题  

###  Android uSDK_6.1.0

- 版本号： v6.1.0 
- 发布日期：2020.09.04
- MD5值：4A5C53F82B11C5F1DEF9EDE5C5D1614
- 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.0_Phone_Android_20200907155553790.zip)        
- 版本日志 

 **注意事项**
 注意事项1：此版本不支持海外环境。  
 注意事项2：如需统计分析功能，请与统计分析SDK3.4.0版本搭配使用  
 更新日志：  

1.新增功能    
1.1.  查询设备网络信号质量(见`uSDKDevice`)   

    public void getNetworkQualityV2(final ICallback<uSDKNetworkQualityInfoV2> callback);

1.2 新增网络质量信息类（见 `uSDKNetworkQualityInfoV2`）   

    public uSDKDeviceConnectStatus getDeviceConnectStatus()//获取设备的链接状态
    public String getMachineId()//设备机器编号
    public boolean isOnLine()//获取设备是否远程在线
    public long getStatusLastChangeTime()//获取设备最后一次状态变化时间，格林威治时间
    public String getNetType()//获取设备的网络类型,例如 "Wifi"
    public String getSsid()//获取设备所连接的路由器名称
    public int getRssi()//获取设备所连接的路由器网络信号强度
    public int getPrssi()//获取设备所连接的路由器网络信号强度百分比
    public int getSignalLevel()//获取设备所连接的路由器网络信号质量等级,取值: 0 未知,1 优,2 良,3 合格,4 差
    public int getIlostRatio()//获取设备所连接的路由器 广域网丢包率
    public int getIts()//获取设备所连接的路由器 广域网延时
    public String getLanIP()//获取设备所连接的路由器的内网IP
    public String getModuleVersion()//获取设备的模块版本描述,版本格式; 软件版本号/软件类型/硬件版本号/硬件类型

1.3 新增 `uSDKDeviceConnectStatus` 枚举   

        CLOUD_CONNECTED("远程连接")
        LOCAL_CONNECTED("本地连接")
        LOCAL_BLE_CONNECTED("蓝牙连接")
        OFFLINE("离线")

1.4 新增获取故障信息的接口（见`uSDKDevice`）   

    public uSDKFaultInformation getSDKFaultInformation()

1.5 新增设备故障信息类（见uSDKFaultInformation）   

    public int getStateCode()
    public int getState()

1.6 新增故障信息回调方法（见`DeviceListener`）   


    public void onUpdateFaultInformation(uSDKFaultInformation faultInformation)



1.7 新增通过蓝牙修改设备侧SSID&PWD的接口（见uSDKDevice）   

    public void updateRouterSSID(String ssid, String password, String bSsid, int timeout, IuSDKUpdateRouterSSIDCallBack updateRouterSSIDCallBack)  


2.接口变更   
 无  
3 内部优化   

3.1. HTTPDNS缓存机制优化, 本地进行DNS解析时，结果不进入缓存   
3.2. 配置绑定优化：SoftAP、BLE设备发起绑定带bindCode    
3.3. 离线分析埋点，增加云连接状态，设备连接状态，网络状态，前后台事件的埋点   
3.4. 基础连接优化：uSDK网络切换处理机制优化，在任何切网时都重启本地搜索，重启云连接，而不是根据网络状态进行连接或断开   
3.5. 配置绑定优化：softAp配网过程中同时监听BLE事件广播  
3.6. 绑定HTTP超时时，内部进行重试，指导的成功或者超时  
3.7. 设备测绑定，解决绑定成功后设备离线的问题（result结果处理）   
3.8. 三不可（不可配/不可控/不可触发进配置）设备添加到搜索发现列表   




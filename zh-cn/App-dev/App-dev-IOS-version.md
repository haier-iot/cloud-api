# uSDK-IOS资料下载  #  

## 支持有效期   

支持有效期：新版本uSDK发布起，APP新接入大版本SDK的支持有效期为6-12个月，APP新接入小版本SDK的支持有效期为3-6个月。  

## 版本资料 


### IOS uSDK_8.6.0     

版本号： v8.6.0  

发布日期：2021.05.31   

MD5值：523E828FA8C020EE873C373C4D0A93D7  

下载链接：[点击下载]()  

更新日志：  

一、外部修改    


1.新增uSDKBaseBindInfo类,所有的bindinfo类都继承于此类，用于增加一个NFCInfo的公共属性，选填。  

@interface uSDKBaseBindInfo : NSObject
 
/**  

 选填参数  

 */  

@property(nonatomic, strong) uSDKNFCInfo *NFCInfo;
 
@end  

绑定之前给bindinfo上的NFCInfo赋值就用基类的属性。

2.音视频  

uSDKMonitorPlayer   
废弃createPlayerWithDevice:success:failure:方法,  
新增createPlayerWithDeviceId:success:failure:方法，请尽快更换为新方法创建，  如下：

@interface uSDKMonitorPlayer : uSDKPlayer<uSDKPlayerTalkable>
 
/**
 * 创建播放器
 *
 * @param device  device设备对象
 * @param success 成功，返回uSDKMonitorPlayer对象
 * @param failure 失败
 * @deprecated 8.6.0
 */
+ (void)createPlayerWithDevice:(uSDKDevice* _Nonnull)device
                       success:(void(^)(uSDKMonitorPlayer *monitorPlayer))success
                       failure:(void(^)(NSError *error))failure DEPRECATED_MSG_ATTRIBUTE("Please use [uSDKMonitorPlayer createPlayerWithDeviceId:success:failure]");
 
 
/**
 * 创建播放器
 *
 * @param deviceId 设备唯一标识
 * @param success 成功，返回uSDKMonitorPlayer对象
 * @param failure 失败
 */
+ (void)createPlayerWithDeviceId:(NSString* _Nonnull)deviceId
                       success:(void(^)(uSDKMonitorPlayer *monitorPlayer))success
                       failure:(void(^)(NSError *error))failure;
 
 
@end
uSDKPlaybackPlayer 废弃createPlayerWithDevice:completionHandler:方法,新增createPlayerWithDeviceId:completionHandler:方法，请尽快更换为新方法创建，如下：

/**
 * 创建播放器
 *
 * @param device 对象
 * @param completionHandler 结果回调
 * @deprecated 8.6.0
 */
+ (void)createPlayerWithDevice:(uSDKDevice* _Nonnull)device
             completionHandler:(void(^)(uSDKPlaybackPlayer *playbackPlayer, NSError *_Nullable error))completionHandler DEPRECATED_MSG_ATTRIBUTE("Please use [uSDKPlaybackPlayer createPlayerWithDeviceId:success:failure]");
 
/**
 * 创建播放器
 *
 * @param deviceId 设备ID
 * @param completionHandler 结果回调
 */
+ (void)createPlayerWithDeviceId:(NSString* _Nonnull)deviceId
             completionHandler:(void(^)(uSDKPlaybackPlayer *playbackPlayer, NSError *_Nullable error))completionHandler;
 
/**
 * 获取有回放文件的日期列表
 * @param pageIndex 页码索引，获取指定页码的回放文件（ pageIndex从0开始递增）
 * @param countPerPage 在[startTime, endTime]时间范围内按每页`countPerPage`个文件查询，每次返回一页的数据，该值由APP设置，取值范围[1, 3200]
 * @param startTime 开始时间戳（秒）
 * @param endTime 结束时间戳（秒）
 * @param completionHandler 结果回调
 */  

3.无路由先绑接口  

//uSDKBinding.h  

/**  

 无路由先绑定接口  

 @param bindInfo 配置信息  

 @param progressNotify 进度回调，共三个进度回调：  

    1. uSDKBindProgressConnectDevice  
    
    2. uSDKBindProgressSendConfigInfo  
   
    3. uSDKBindProgressBindDevice  
   
 @param completionHandler 配网完成时的block回调，如果成功，则error == nil 

 
  
获取sessionKe失败。ERR_USDK_CLOUD_COMMON_ERROR = -10008,  

获取sessionKey请求超时。ERR_USDK_HTTP_COMMON_ERROR = -10010，  

绑定过程中 可能出现的错误码：  

   @(1001):@"路由器断电等导致找不到路由器",  

   @(1002):@"用户修改路由信息导致无法连上路由器",  

   @(1003):@"疑似密码错误",  

   @(1004):@"设备未配置WiFi信息",  

 
   @(2001):@"收到配置信息但是找不到路由",  

   @(2002):@"密码错误",  

   @(2003):@"疑似密码错误",  

   @(2004):@"配置信息不完整",  

    
   @(4001):@"连接主网关失败",  

   @(4002):@"连接接入网关失败",  

 
   @(5001):@"设备被解绑",  

   @(5002):@"绑定达到上限",  

   @(5003):@"Token失效"
 
 @since 8.6.0  

 */  

+ (void)bindDeviceWithoutWifi:(uSDKWithoutWifiBindInfo *)bindInfo  
+ 
         progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify  

                completionHandler:(void(^)(uSDKDevice *device, NSError *error))  

			  completionHandler;  

4.新增属性  

在uSDKDeviceInfo.h中新增加属性  


/**  

 应用类型名称  

 @since 8.6.0  

*/  

@property (nonatomic, copy, readonly) NSString* apptypeName;  

 
/**  

 设备支持的所有配置方式  

 @since 8.6.0  

 */  

@property (nonatomic, assign, readonly) uSDKDeviceConfigType allConfigTypes;  



/**  

 设备支持的配置方式  

 */  

typedef NS_OPTIONS(NSUInteger, uSDKDeviceConfigType) {  

    /**  

     BLE配置方式，调用uSDKBinding.bindDeviceByBLE接口进行配置绑定  

     */  

    uSDKDeviceConfigTypeBLE = (1UL << 0),  

    /**  

     纯BLE配置方式，调用uSDKBinding.bindPureBLEDevice接口进行配置绑定  

     @since 5.4.0  

     */  

    uSDKDeviceConfigTypePureBLE = (1UL << 1) ,  

    /**  
 
     极路由免密配置  

     @deprecated 5.4.0  

     */  

    uSDKDeviceConfigTypeHiRouter = (1UL << 2),  

    /**  

     BLEMesh配置方式，调用uSDKBinding.bindBLEMeshDevice接口进行配置绑定  

     @since 7.0.0  

     */  

    uSDKDeviceConfigTypeBLEMesh = (1UL << 3),  

    /**  

     新直连配置方式，调用uSDKBinding.bindNewDirectLinkDeviceWithVerificationCodeBindInfo接口进行配置绑定  

     @since 6.0.0  

     */  

    uSDKDeviceConfigTypeNewDirectLink = (1UL << 4),  

    /**  

     蓝牙广播绑定方式，调用uSDKBinding.bindBLEADVDevice接口进行配置绑定  

     @since 8.0.0  

     */  

    uSDKDeviceConfigTypeBLEADV = (1UL << 5),  

    /**  

     softAp配置方式，调用uSDKBinding.bindDeviceBySoftAp接口进行配置绑定  

     @since 8.0.0  

     */  

    uSDKDeviceConfigTypeSoftAP = (1UL << 6),  

    /**  

     smartlink配置方式，调用uSDKBinding.bindDeviceBySmartLink接口进行配置绑定  

     @since 8.6.0  

     */  

    uSDKDeviceConfigTypeSmartlink = (1UL << 7),  

    /**  

     无网先配，调用uSDKBinding.bindDeviceBySmartLink接口进行配置绑定  

     @since 8.6.0  

     */  

    uSDKDeviceConfigTypeBindWithoutWifi = (1UL << 8)  

    
};  


二、内部优化  

uSDKBleSearch、uSDKBleSearch中增加hotSpotName、apptypeName、configType、allConfigTypes的解析。  

uSDKDeviceScanner中搜索合并优化。  

bindDeviceByBLE接口增加对新协议设备的支持。  

设备下载配置文件失败时，新增DI打点。  

蓝牙类设备上报版本信息时，增加machineID字段。  

BLE_OTA中复用安全连接，超时时间从云端获取。  

组件修复蓝牙重复数据不上报的bug  

 

### IOS uSDK_8.5.0    
   

  版本号： v8.5.0    

 
  发布日期：2021.04.30    


  MD5值：1429A5C9956B2BACA5CAA5960FBDBD7B    


  下载链接： [点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.5.0_Phone_iOS_20210430161621_20210511092500310.zip)    


  更新日志：  
  

  新增功能    


 一、外部修改  
	

  uSDKDevice.h新增接口  

  /**
   订阅资源新接口，数据会进行加解密， 对应的delegate回调为新的:  
- (void)device:(uSDKDevice *)  device didReceiveDecodeResource:    
- 
- (NSString *)resourcedata:(NSString *)JSONData;  
- 
  
   @param resourceName 资源名称    


   @param success 订阅成功的回调   
  

   @param failure 订阅失败的回调    


   @since 8.5.0    


   */
  
- (void)subscribeResourceWithDecode:(NSString *)resourceName     
- 
- success:(void(^)(void))success     
- 
- failure:(void(^)(NSError *error))failure;   
- 
/**
 新的接收资源数据    

 @param device 设备对象  

 @param resource 资源名称  

 @param JSONData 资源数据  

 @since 8.5.0  

 */  
- (void)device:(uSDKDevice *)device didReceiveDecodeResource:(NSString *)resource data:(NSString *)JSONData;  
- 
  uSDKCameraScanQRCodeBindInfo 新增必填字段  
productCode  

  /**  

   成品编码，必填  

   */  

  @property (nonatomic, copy) NSString *productCode;    


  新增本地视频回放、本地图片获取和获取云存视频m3u8地址    


  uSDKPlaybackPlayer.h新增接口    


  /// 回放文件   
 

  @interface uSDKPlaybackItem: NSObject   
 

  /// 回放文件起始时间（秒）    


  @property (nonatomic, assign) NSTimeInterval startTime;    


  /// 回放文件结束时间（秒）  
  
  
  @property (nonatomic, assign) NSTimeInterval endTime;    


  /// 回放文件持续时间（秒）   
 

  @property (nonatomic, assign) NSTimeInterval duration;  
  

  @end     


  /// 回放文件分页  

  @interface uSDKPlaybackPage<uSDKItem>: NSObject  

  /// 当前页码索引  

  @property (nonatomic, assign) uint32_t  pageIndex;  

  /// 总页数  

  @property (nonatomic, assign) uint32_t  totalPage;  
  
  /// 回放文件数组  

  @property (nonatomic, strong) NSArray<uSDKItem> *items;

  @end

  @interface uSDKPictureItem : NSObject

  /// 图片ID   

  @property (nonatomic, assign) uint32_t id;  

  /// 图片时间  

  @property (nonatomic, assign) NSTimeInterval time;  

  /// 图片类型  

  @property (nonatomic, assign) uSDKPlayerImageType type;  

  /// 图片名称  

  @property (nonatomic, copy) NSString *name;  

  /// 图片文本描述  

  @property (nonatomic, copy) NSString *describe;  

  /// 缩略图base64编码  

  @property (nonatomic, copy) NSString *thumbnail;

  @end

  @interface uSDKPlaybackPlayer : uSDKPlayer

/**

 * 创建播放器
   *
 * @param device 设备对象
 * @param completionHandler 结果回调
   */
* (void)createPlayerWithDevice:(uSDKDevice* _Nonnull)device
      completionHandler:(void(^)(uSDKPlaybackPlayer *playbackPlayer, NSError *_Nullable error))completionHandler;

/**

 * 获取有回放文件的日期列表  
 *  
 * @param pageIndex 页码索引，获取指定页码的回放文件（ pageIndex从0开始递增）  
 * 
 * @param countPerPage 在[startTime, endTime]时间范围内按每页`countPerPage`个文件查询，每次返回一页的数据，该值由APP设置，取值范围[1, 3200]  
 * 
 * @param startTime 开始时间戳（秒）  
 * 
 * @param endTime 结束时间戳（秒）  
 * 
 * @param completionHandler 结果回调  
 * 
   */

- (void)getPlaybackDateListOfpageIndex:(uint32_t)pageIndex  
- 
      countPerPage:(uint32_t)countPerPage  

         startTime:(NSTimeInterval)startTime  

           endTime:(NSTimeInterval)endTime  

  completionHandler:(void (^)(uSDKPlaybackPage<NSNumber *> *_Nullable page, NSError *_Nullable error))completionHandler;

/**

 * 获取一页回放文件列表  
 * 
 * @param pageIndex 页码索引，获取指定页码的回放文件（ pageIndex从0开始递增）  
 * 
 * @param countPerPage 在[startTime, endTime]时间范围内按每页`countPerPage`个文件查询，每次返回一页的数据，该值由APP设置，取值范围[1, 3200]  
 * 
 * @param startTime 开始时间戳（秒）  
 * 
 * @param endTime 结束时间戳（秒）  
 * 
 * @param completionHandler 结果回调  
 * 
 * @note 请根据实际情况合理设置查询时间范围和分页，`时间跨度太长`或`每页数量过大`会增加设备查询时间， 建议[startTime, endTime]区间不超过24小时 或 countPerPage不超过1440个文件.  
 * 
   */

- (void)getPlaybackListV2OfpageIndex:(uint32_t)pageIndex
      countPerPage:(uint32_t)countPerPage
         startTime:(NSTimeInterval)startTime
           endTime:(NSTimeInterval)endTime
  completionHandler:(void (^)(uSDKPlaybackPage<uSDKPlaybackItem *> *_Nullable page, NSError *_Nullable error))completionHandler;

/**

 * 获取可以查看的照片列表
 * @param startTime 起始时间
 * @param endTime 结束时间
 * @param photoID 图片ID  注：查询首页图片列表时photoID传参为0，查询下一页图片列表时photoID传参为上一页最后一张图片的photoID
 * @param count 要查询的照片个数，范围是0到10
 * @param completionHandler 结果回调
 * @note 1.查询首页：使用参数【startTime，endTime，count】 ，photoID 传参为0 ，查询首页前count张图片，并且记录查询到的最后一张图片id；
 * 2.查询下一页：使用参数【startTime，id，endTime，count】，photoID 传参为上一页最后一张图片的photoID，查询下一页的count张图片，并且并且记录查询到的最后一张图片id；
 * 3.同第2步，持续查询下一页，直到查询不到任何内容；
    */

- (void)getPhotoListWithStartTime:(NSTimeInterval)startTime
      endTime:(NSTimeInterval)endTime
      photoID:(uint32_t)photoID
        count:(uint32_t)count
  completionHandler:(void(^)(NSArray<uSDKPictureItem*> *items, NSError *_Nullable error))completionHandler;

/**

 * 获取设备的本地图片内容
 * @param photoID 图片ID
 * @param timeout 超时时间 范围建议5-10秒左右
 * @param completionHandler 结果回调
   */

- (void)getLocalPhotoDataWithPhotoID:(uint32_t)photoID
      timeout:(NSTimeInterval)timeout
  completionHandler:(void(^)(NSData *data, NSError *_Nullable error))completionHandler;


/**

 * 获取云存视频可播放日期信息
 * - 用于终端用户在云存页面中对云存服务时间内的日期进行标注，区分出是否有云存视频文件。
 * @param timezone 相对于0时区的秒数，例如东八区28800
 * @param completionHandler 回调结果
   */

- (void)getCloudVideoDateListWithTimezone:(NSInteger)timezone
      completionHandler:(void(^)(NSArray<NSNumber *> *items, NSError *_Nullable error))completionHandler;

/** 获取云存回放文件列表

 *  获取云存列表，用户对时间轴渲染
 *  @param startTime 开始UTC时间,单位秒
 *  @param endTime 结束UTC时间,单位秒 超过一天只返回一天
 *  @param completionHandler 回调结果
    */

- (void)getCloudVideoPlayListWithStartTime:(NSTimeInterval)startTime
      endTime:(NSTimeInterval)endTime
  completionHandler:(void (^)(NSArray<uSDKPlaybackItem *> *items, NSError *_Nullable error))completionHandler;

/** 获取云存回放 m3u8 播放地址

 * @param startTime 开始UTC时间,单位秒
 * @param endTime 结束UTC时间,单位秒 填 0 则默认播放到最新为止
 * @param completionHandler 回调结果
   */

- (void)getCloudVideoPlayAddressWithStartTime:(NSTimeInterval)startTime
      endTiem:(NSTimeInterval)endTime
  completionHandler:(void (^)(NSString *url, NSError *_Nullable error))completionHandler;

/**

 * 设置回放参数.
 * @param item 播放的文件(可跨文件).
 * @param time 指定播放起始时间点（秒），取值范围`playbackItem.startTime >= time <= playbackItem.endTime`.
   */

- (void)setPlaybackItem:(uSDKPlaybackItem *)item
      seekToTime:(NSTimeInterval)time;

/// 暂停
- (void)pause;

/// 恢复

- (void)resume;

@end
uSDKPlayer.h新增播放器代理协议
/**

 *  播放时间回调
 *  @param player 播放器实例
 *  @param PTS 时间戳
    */

- (void)player:(uSDKPlayer *)player didUpdatePTS:(NSTimeInterval)PTS;

/**

 *  SD卡回放文件播放结束
 *  @param player 播放器实例
 *  @param startTime 当前播放结束的文件的开始时间
    */
*  (void)player:(uSDKPlayer *)player didFinishPlaybackFile:(NSTimeInterval)startTime;
  uSDKConstantInfo.h新增音视频相关错误码
  /**
  *  视频功能内部错误
     */
      ERR_USDK_PLAYER_INTERNAL_FAILURE = -20001,
      /**
  *  无本地回放视频
     */
      ERR_USDK_PLAYER_PLAYBACKLIST_EMPTY = -20001,
      /**
  *  视频分辨率已改变
     */
      ERR_USDK_PLAYER_RESOLUTION_CHANGED =  -20003,
      /**
  *  超过设备可支持的最大P2P通道数
     */
      ERR_USDK_PLAYER_DEVICE_CALLING_ALL_CHN_BUSY = 20004,
      /**
  *  P2P通道消息发送失败
     */
      ERR_USEK_PLAYER_MESSAGE_SEND_FAILURE = -20005,
      /**
  *  P2P通道消息发送超时
     */
      ERR_USEK_PLAYER_MESSAGE_SEND_TIMEOUT = -20006,
      /**
  *  设备正在录制
     */
      ERR_USDK_PLAYER_RECORDER_IS_RUNNING = -20007,
      /**
  *  APP端通道连接数已达上限
     */
      ERR_USDK_PLAYER_EXCEEDS_MAX_NUMBER = -20008,

二、内部优化

1.配置文件下载地址变更
2.uSDKMonitorPlayer和uSDKPlaybackPlayer中destroyPlayer接口统一定义在父类uSDKPlayer中
3.创建播放器时向平台请求tid、accessid、accesstoken的逻辑统一放到分类uSDKPlayer+uSDKPrivatePlayer.h中实现
4.内部新增接口getDeviceScanQRcodeWithQRCodeInfo，去网络请求判断是否支持新的扫码绑定，内部增加扫码绑定的逻辑，和安防绑定的逻辑分开走。



### **iOS uSDK_8.4.0**  ###

版本号： v8.4.0    

发布日期：2021.03.22        

MD5值：B0E8073E25A10B62AB776F2C82D63F50       

下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.4.0_Phone_iOS_20210318174107_20210330132958513.zip)    

更新日志：      

**新增功能**    

1.uSDKDevice.h组控接口变更    

​    /**
​     组设备的成员列表
​     @since 8.4.0
​     */
​    @property (nonatomic, strong, readonly) NSArray<uSDKDevice *> *groupMembers;
​    
​    /**
​    获取可与当前设备分到同一组的设备列表, 当前设备要求有zigbee能力或BLEMesh能力
​     
​    @param completionHandler 接口执行完成时回调，error == nil表示接口执行成功，接口执行成功时，devices也可能为空，表示接口执行成功，但没有可分组设备
​    @since 8.4.0
​    */
    - (void)fetchGroupableDeviceListCompletionHandler:(void(^)(NSArray<uSDKDevice *> *devices, NSError *error))completionHandler;

​    /**
​    创建分组，返回组设备对象
​     
​    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
​    @param completionHandler 接口执行完成时回调，error == nil表示组创建成功，注意组设备虽然创建成功，但没有任何设备被成功添加到该组中，
​    需要主动调用addDevices:toGroupWithTimeoutInterval:progressNotify:completionHandler:函数去添加进组中。
​    @since 8.4.0
​    */
    - (void)createGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
        completionHandler:(void(^)(uSDKDevice *device, NSError *error))completionHandler;

​    /**
​    向组设备中添加设备，要求当前device对象为组设备
​     
​    @param devices 需要添加到组设备的设备列表，可以添加到组设备的设备列表需要通过接口``
​    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据添加设备的多少动态调整参数
​    @param progressNotify 进度通知，上报每个设备的添加结果，如果error == nil则表示添加成功
​    @param completionHandler 接口执行完成时回调，参数校验失败时error有值，一旦开始添加，不管是否有设备成功添加到组，error都为nil
​    @since 8.4.0
​    */
    - (void)addDevices:(NSArray<uSDKDevice *> *)devices
        toGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
        progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
        completionHandler:(void(^)(NSError *error))completionHandler;

​    /**
​    从组设备中移除设备，要求当前device对象为组设备
​     
​    @param devices 需要从组设备中删除的设备列表
​    @param timeoutInterval 超时时间，取值范围30-180秒，App可根据删除设备的多少动态调整参数
​    @param progressNotify 进度通知，上报每个设备的删除结果，如果error == nil则表示设备删除成功
​    @param completionHandler 接口执行完成时回调，参数校验失败时error有值，一旦开始添加，不管是否有设备成功添加到组，error都为nil
​    @since 8.4.0
​    */
    - (void)removeDevices:(NSArray<uSDKDevice *> *)devices
        fromGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval
        progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
        completionHandler:(void(^)(NSError *error))completionHandler;

​    /**
​    删除组设备，要求当前device对象为组设备
​     
​    @param completionHandler 接口执行完成时回调，error == nil表示组删除成功
​    @since 8.4.0
​    */
    - (void)deleteGroupCompletionHandler:(void(^)(NSError *error))completionHandler;

2.内部修改  

2.1 按照新方案更新了内部逻辑    

2.2 H2面板增加了zigbee连接方式的支持。    

2.3 新增uSDKMutableDevice+uSDKGroupOperation类，用于分发mesh和zigbee不同通道的组控相关的功能。    


----------

### **iOS uSDK_8.3.0**   ###    

版本号： v8.3.0   

发布日期：2021.03.02    

MD5值：65EFA79B2C417A9558B254E9BF943E99    

下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.3.0_Phone_iOS_20210302105710_20210315163827028.zip)     

更新日志：    

**新增功能**    
1.新增NFC数据解析和数据更新的功能        


​    //uSDKUtil.h
​    @interface uSDKNFCUtil : NSObject 
​    /**
     * 解析NFC标签数据
          * @param NdefRecord   NFC标签原始数据
          * @param complete接口回调 成功返回uSDKNFCInfo类，失败返回error
               **/
    + (void)parseNFCTagDataWithNdefRecord:(NSString*)NdefRecord
          complete:(void(^)(uSDKNFCInfo *NFCInfo, NSError *error))complete;
        /**
     * 更新NFC设备信息
          *  @param NFCInfoNFC标签解析后数据
          *  @param complete接口回调 失败返回error
               */
    + (void)updateNFCDeviceInfoWithNFCInfo:(uSDKNFCInfo*)NFCInfo
      complete:(void(^)(NSError *error))complete;
        @end 

2.新增NFCInfo类     

​    //uSDKInfo.h     
​    //NFC标签数据解析结果
​    @interface uSDKNFCInfo : NSObject     
​    @property (nonatomic, copy) NSString *NFCSerialNumber;  //NFC标签序列号    
​    @property (nonatomic, copy) NSString *MAC;  //设备MAC    
​    @property (nonatomic, copy) NSString *deviceID; //设备ID
​    @property (nonatomic, copy) NSString *productCode;  //设备成品编码
​    @property (nonatomic, copy) NSString *hwProductID; //华为的PID
​    @end

**内部优化**    
1.只订阅，不获取属性时，忽略属性上报    
2.解决fota升级时在子线程调UI导致崩溃的问题    
3.smartlink配置失败返601时进行重试      

**组件修改内容**     
1.在门锁通过手机进行LocalKey更新或者时间同步时，用户可以优先订阅该门锁，建立连接。    
2.修改的“蓝牙门锁升级时崩溃”问题。      

**CAE修改内容**    
1.增加蓝牙beacon v0的数据解析，以便支持云芯二代配置失败，可以通过蓝牙beacon上报配置失败原因    
2.解决蓝牙搜索内存泄露的问题    
3.解决友盟异常接管sigpipe，导致crash的问题（ios）    
4.ble ota超时时间和hal层超时时间（10秒）对齐，从2秒改成11秒     



----------

### **iOS uSDK_8.2.0**   ###  

版本号： v8.2.0   

发布日期：2021.02.04    

MD5值：0B7D9309709F48E84EB281DB31A6D220    


下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.2.0_Phone_iOS_20210205170244003.zip)       

注意事项1：不支持国海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用更新日志：    
1.新增功能   
1.1 新增事件上报自定义类 uSDKDeviceEvent   

​    //uSDK设备事件信息类，用于描述设备的事件消息。   
​    
​    @interface uSDKDeviceEvent : NSObject
​    
​    //事件名称-标准定义
​    @property (nonatomic, copy, readonly) NSString *name;
​    
​    // 事件类型[ message消息;alarm报警;fault故障;]
​    @property (nonatomic, copy, readonly) NSString *type;
​    
​    // 该事件附带的属性列表
​    @property (nonatomic, strong) NSArray<uSDKDeviceAttribute*> *attrs;
​    @end  

1.2 新增事件上报代理回调（见uSDKDevice.h）    

​    /**
     * 设备事件回调
          * @param device 事件上报的设备对象
          * @param events 事件
               */
            -(void)device:(uSDKDevice *)device didReceiveEvent:(NSArray<uSDKDeviceEvent*>*)events ;

2.接口变更  
 无    
3.内部优化    
3.1 新接入蓝牙门锁类设备的 搜索发现、配置绑定、控制、属性报警事件上报。 可配设备处于搜索发现阶段时，该类设备的配网方式为纯蓝牙配置方式（uSDKDeviceConfigTypePureBLE ）。蓝牙门锁类设备使用用纯蓝牙接口（bindPureBLEDevice）进行配置绑定。   
3.2 接入蓝牙门锁类设备的OTA功能，沿用之前FOTA升级接口。        
3.3 接入蓝牙门锁类设备具备查看历史数据功能。    
3.4 接入3款老营养秤（与8.0.0的广播称使用方式一致）。        


----------

###  **iOS uSDK_8.1.1**     ###

版本号： v8.1.1    
发布日期：2021.1.8    
MD5值：059E057A2891E8872FD1A9CE9850B213    
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.1_Phone_iOS_20210114161003329.zip)    
注意事项1：不支持国海外环境。     
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用更新日志：     
1.新增功能   
 无   
2.接口变更    
 无   
3.内部优化及BUG修改   
3.1 修复蓝牙体脂秤数据更新没有给app上报的问题    



----------

### **iOS uSDK_8.1.0**     ###

版本号： v8.1.0    
发布日期：2020.12.23   
MD5值：4689EB4BE4490F024FF35D30D0135FC1   
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.1.0_Phone_iOS_20201228103123939.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    
更新日志：   
1.新增功能   
1.1 根据自定义traceId创建链式跟踪对象（见uTraceNode.h）     

​    /**
​     根据自定义traceId创建链式跟踪对象 
​     1.该方法针对App主动服务业务使用
​     2.调用埋点方法时，bId的值：
​    2.1对于DI点，bId = uTraceNodeDI.bId
​    2.2对于CS/CR点，bId = businessID
​      
​     @param traceID 链式跟踪ID，不能为空，且要求为32个字符长度的uuid字符串
​     @param businessID 业务ID，可以为空
​     @return uTrace对象
​     @since 8.1.0
​     */
    + (uTrace *)createTraceWithTraceID:(NSString *)traceID businessID:(NSString *)businessID;

1.2 网关子设备绑定接口，绑定信息uSDKSlaveDeviceBindInfo中新增属性，用于RISCO设备接入
在uSDKSlaveDeviceBindInfo.h中新增加属性    

​    /**
​    通用扩展信息，可以为空，SDK不做校验，仅透传
​     
​    如：RISCO设备接入时，需传入扩展信息：ip/port/Identification Code，信息格式需App开发者与设备侧协商。
​    @since 8.1.0
​    */
​    @property (nonatomic, copy) NSString *extendInfo;


2.接口变更   
 无   
3.内部优化及BUG修改   
3.1 对于可配置设备，在获取到设备型号信息后再上报App    
3.2 根据云端配网策略，动态显示设备配网方式    

----------

### **iOS uSDK_8.0.0**    ###

版本号： v8.0.0  
发布日期：2020.12.07   
MD5值：EEBF44726B93E3519D79D7FF7D44A712  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK8.0.0_Phone_iOS_20201211175856730.zip)   
注意事项1：此版本不支持海外环境。   
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用   
更新日志：   
1.新增功能   
1.1增加BLE Mesh设备管理   
1.1.1 BLE Mesh设备搜索BLE Mesh设备进入配网模式后，可以通过自发现待入网设备接口   [uSDKDeviceScanner startScanConfigurableDevice]进行搜索，在返回的uSDK DeviceInfo中增加了uSDKDeviceConfigTypeBLEMesh的配网方式    


​      typedef NS_OPTIONS(NSUInteger, uSDKDeviceConfigType) {
​    	uSDKDeviceConfigTypeBLEMesh = (1UL << 3),
​    };   

1.1.2 BLE Mesh设备配网对于支持BLE Mesh配网方式的设备，在配网时，需要调用新增的BLE Mesh配网接口   

​    //uSDKBinding.h+ (void)bindBLEMeshDevice:(uSDKBLEMeshBindInfo *)bindInfo 
​       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
​      success:(void(^)(uSDKDevice *device))success
​      failure:(void(^)(NSError *error))failure;   

1.1.3 BLE Mesh组设备对于BLE Mesh设备，可以将多个具有相同功能集的设备加入一个组，并生成一个新的组设备。 如:将房间内所有的灯加入一个组，会生成一个组设备，可以直接对组设备进行控制，即会对加入到这个组设备的所有设备生效    
1.1.3.1 获取可分组设备列表 可以根据某个device对象，获取可以与这个设备加入同一个组的设备列表    

​    //uSDKDevice.h- (void)getBLEMeshGroupDeviceListSuccess:(void(^)(NSArray<uSDKDevice *> *devices))success failure:(void(^)(NSError *error))failure;   

1.1.3.2 创建设备分组   
调用获取可分组设备列表接口后，可以在返回的列表中选择一个或多个设备，与当前设备对象加入同一个组，创建成功后，会产生一个新的uSDKDevice设备 对象   
创建组的过程分为两步，第一步是创建组设备对象，第二步是将选择的设备和当前设备对象加入到这个新创建的组中     

​    //uSDKDevice.h- (void)createBLEMeshGroupWithDevices:(NSArray<uSDKDevice *> *)devices 
​    	timeoutInterval:(NSTimeInterval)timeoutInterval 
​    	progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify 
​    	success:(void(^)(uSDKDevice *device))success 
​    	failure:(void(^)(NSError *error))failure;

1.3.3 向组设备中添加成员 组创建成功后，可以向组内继续添加新的成员，在调用该接口时，要求当前device对象为组设备    

​    //uSDKDevice.h- (void)addDevices:(NSArray<uSDKDevice *> *)devices toBLEMeshGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval 
​    progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
​       success:(void(^)(void))success
​       failure:(void(^)(NSError *error))failure;

1.3.4 从组内删除成员 组创建成功后，可以向组内继续添加新的成员，在调用该接口时，要求当前device对象为组设备    

​    //uSDKDevice.h- (void)removeDevices:(NSArray<uSDKDevice *> *)devices fromBLEMeshGroupWithTimeoutInterval:(NSTimeInterval)timeoutInterval 
​       progressNotify:(void(^)(uSDKDevice *device, NSError *error))progressNotify
​      success:(void(^)(void))success
​      failure:(void(^)(NSError *error))failure;

1.3.5 删除组设备 组设备不能解绑，但可以删除，删除组设备后，会自动解绑    

​    //uSDKDevice.h- (void)deleteBLEMeshGroupSuccess:(void(^)(void))success 
​      failure:(void(^)(NSError *error))failure;

1.4 增加BLE Mesh OTA    
针对BLE Mesh设备OTA，依然沿用原FOTA接口，使用方式不变 。    
1.5是 增加BLE ADV设备管理    
1.5.1 BLE ADV设备搜索未绑定的BLE ADV设备可以通过自发现待入网设备接口[uSDKDeviceScanner startScanConfigurableDevice]进行搜索，在返回的uSDKDeviceIn fo中增加了uSDKDeviceConfigTypeBLEADV的配网方式     

​    typedef NS_OPTIONS(NSUInteger, uSDKDeviceConfigType) {
​    uSDKDeviceConfigTypeBLEADV = (1UL << 5),
​    };

1.5.2 BLE ADV设备配网对于支持BLE ADV配网方式的设备，在配网时，需要调用新增的BLE ADV配网接口 
//uSDKBinding.h+ (void)bindBLEADVDevice:(uSDKBLEADVBindInfo *)bindInfo  

​          progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
​                 success:(void(^)(uSDKDevice *device))success
​                 failure:(void(^)(NSError *error))failure;  

1.5.3 uSDK设备属性类新增设备属性的时间戳，精确到毫秒(目前该字段，只有BLE_ADV设备 才会有值， 开发者可以不用关心这个字段)     


​    //uSDKBinding.h@property (nonatomic, assign) uint64_t timeStamp;   

1.6. 增加uSDK音视频P2P功能    
新增uSDKPlayer.h核心播放器， uSDKMonitorPlayer.h 监控播放器。 uSDKPlayer为抽象类，不要直接实例化，请使用其派生类:uSDKMonitorP layer使用监控播放器可以实现手机与设备之间 实时的音视频播放信息。    
1.7. 获取路由器信息    
当WiFi网络中存在已入网设备时，需要配置另外一台设备到该WiFi网络，可以通过此接口获取该WiFi网络的路由器SSID和密码      

​    //uSDKBinding.h
    + (void)getConfigRouterInfo:(NSTimeInterval)timeoutInterval
        	success:(void(^)(uSDKConfigRouterInfo *routerInfo))success
        	failure:(void(^)(NSError *error))failure;

1.8 uSDKDeviceScanner增加wifi、ble开关通知    

​    typedef NS_ENUM(NSUInteger, uSDKInvalidPermission) { 
​    uSDKBleInvalid,
​    uSDKNetWorkInvalid,
​    };
​    /** 
​     @param scanner uSDKDeviceScanner
​     @param invalidPermission
​     */
    - (void)deviceScanner:(uSDKDeviceScanner*)scanner didPermissionInvalid:(uSDKInvalidPermission *) invalidPermission; 

2.接口变更  
 无  
3.内部优化及BUG修改   
3.1 针对smartLink配网“配置命令发送完成,但未找到上线设备”导致配网失败的问题优化   
3.2 针对softAp“用户侧绑定，配置命令发送完成，但未找到上线设备”导致配网失败的问题优化   
3.3 在前台时，WiFi或WWAN时会进行云连接   
3.4 修改配网ssid长度判断方式为字节长度，即如果ssid内容全部为中文，则最多支持10个中文字符的ssid    
3.5 解决uSDK配置文件被APP清除缓存功能删除后导致设备离线的问题   
3.6 增加绑定重试接口埋点    
3.7 切网优化   
3.8 wifi搜索改为调用组件的接口    
3.9. bindDeviceBySoftAp、bindDeviceByBLE、retryBindDeviceWithTimeoutInterval 三个接口中不支持bindCode的自绑定设 备，绑定结果从云端查询 。  
3.10 增加uSDKLanSearch类实现代理设备搜索 。uSDKDeviceScanner中实现搜索合并    
3.11 增加获取配网策略   
3.12. HAL层优化:OTA过程中蓝牙关闭时的一些事件。   
3.13 https请求中header中增加uTrace字段，包含绑定和查询绑定结果   
3.14 设备控制失败的时候，区分 是设备离线 还是手机离线   
3.15 不管httpDNS是否开启，cae和ucom都使用SDK的dns解析   
3.16 scanner中搜索到的可配网设备 获取deviceModelInfo后 才上报APP    
3.17 进入后台时，停止WiFi搜索，断开云连接，断开所有本地设备连接。以减少后台活动 。在前台时，只有在WiFi时才会开启WiFi搜索   


----------

### **iOS uSDK_6.1.1**    ###  

版本号： v6.1.1  
 发布日期：2020.09.15  
 MD5值：1873681996C5DDB7BE42397D2C4C9C17  
 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.1_Phone_iOS_20200924135359763.zip)  
 注意事项1：此版本不支持海外环境    
注意事项2：如需统计分析功能，请与统计分析SDK3.5.0版本搭配使用    
更新日志：    

1.新增功能  
 无   
2.接口变更   
 无   
3.内部优化及BUG修改     
3.1 组件修改编译脚本，使CAE wifi搜索中的锁生效   

----------

### **iOS uSDK_6.1.0**   ### 

版本号： v6.1.0    
 发布日期：2020.09.04   
 MD5值：173A40442C4560EE5A106614497A6C32    
 下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.1.0_Phone_iOS_20200904172640495.zip)   
 注意事项1：此版本不支持海外环境。   
 注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用    
 更新日志：  

1.新增功能    
1.1.  查询设备网络信号质量(见`uSDKDevice.h`)   

​     @param success 成功的回调（uSDKNetworkQualityInfo：设备网络质量信息类，具体的网络质量指标 请进该类中查看）
​     @param failure 失败的回调
​     @since 6.1.0
​     */
     - (void)getNetworkQualityV2Success:(void(^)(uSDKNetworkQualityInfoV2 *networkQuality))success
          failure:(void(^)(NSError *error))failure;

1.2 新增uSDK与设备的连接状态  

​    typedef NS_ENUM(NSUInteger, uSDKDeviceConnectStatus) {  
​     //大循环在线
​    uSDKDeviceConnectStatusCloudConnected,
​    
​    // 大循环不在线、小循环在线
​    uSDKDeviceConnectStatusLocalConnected,
​    
​    // 大小循环均不在线，BLE在线
​    uSDKDeviceConnectStatusBleConnected,
​       
​    // 大小循环、BLE均不在线
​    uSDKDeviceConnectStatusOffline
​    };


1.3 新增设备网络质量信息类（见`uSDKNetworkQualityInfoV2.h`）   

​    /**
​     设备网络质量查询结果，其中connectStatus为uSDK与设备当前的连接状态，其他属性均为云平台返回的结果
​     */
​    @interface uSDKNetworkQualityInfoV2 : NSObject

1.4. 新增设备故障信息（见`uSDKFaultInformation`）    

//`uSDKDeviceInfo`类中新增了属性`faultInfo`，在设备广播故障期间，`app`同时可以从属性中获取当前设备的`故障信息`。

​    @interface uSDKFaultInformation : NSObject
​    
​    @property (nonatomic, assign) NSInteger state;
​    
​    @property (nonatomic, assign) NSInteger stateCode;
​    
​    @end

1.5. 接收设备故障事件信息（见`uSDKDeviceDelegate`）   

//在设备配置绑定成功后，如果用户家里路由器断电或修改密码，会导致设备连不上路由器，此时设备(目前只有云芯二代3.2.00版本支持)通过蓝牙广播故障事件，手机侧在收到设备故障广播时会通过下面代理通知App.  

​    @protocol uSDKDeviceDelegate;
​    /// 设备故障信息上报
​    /// @param device 设备对象
​    /// @param faultInformation 故障信息类
​    /// @since 6.1.0
    - (void)device:(uSDKDevice *)device didUpdateFaultInformation:(uSDKFaultInformation *)faultInformation;


1.6 通过蓝牙更新设备侧的路由器SSID和密码.    


 `app`在收到上面的代理回调后，可以调用下面的接口修改模块连接的`ssid`和`password`，修改成功后，会通过上面代理回调`App`，`state == 0, stateCode == 0`，并将属性`faultInfo`中的`state`和`stateCode`都置为0  

​      目前只支持云芯二代3.2.00模块   
​     @param ssid 路由器的ssid，必选
​     @param password 路由器的密码，必选
​     @param bssid 路由器的bssid，可选
​     @param timeoutInterval 超时时间[30,120]
​     @param progressNotify 更新进度通知
​     @param success 成功回调
​     @param failure 失败回调
​     @since 6.1.0
​      
    - (void)updateRouterSSID:(NSString *)ssid
        password:(NSString *)password
       bssid:(NSString *)bssid
          timeoutInterval:(NSTimeInterval)timeoutInterval
      progressNotify:(void(^)(uSDKRouterInfoUpdateProgress updateProgress))progressNotify
          success:(void(^)(void))success
          failure:(void(^)(NSError *error))failure;



2.接口变更   
 无     
3.内部优化及BUG修改    

3.1. `HTTPDNS`缓存机制优化, 本地进行`DNS`解析时，结果不进入缓存  
3.2. 设备列表使用安全的`NSDictionary`, 解决多线程访问可能导致的崩溃问题  
3.3. 离线分析埋点，增加云连接状态，设备连接状态，网络状态，前后台事件的埋点  
3.4. 配置绑定优化：SoftAP、BLE设备发起绑定带bindCode  
3.5. 技术优化：iOS和组件通信逻辑优化，解决组件返回消息时，SDK上层找不到对应请求的问题  
3.6. 基础连接优化：uSDK网络切换处理机制优化，在任何切网时都重启本地搜索，重启云连接，而不是根据网络状态进行连接或断开  
3.7. 配置绑定优化：softAp配网过程中同时监听BLE事件广播  






----------



### **iOS uSDK_6.0.1**   ###  

版本号： v6.0.1   
发布日期：2020.07.27   
MD5值：D534E44EB36CF5C6887F0BEEE73E395D  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.1_Phone_iOS_20200727112723419.zip)  
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：   

1.新增功能   
 无      
2.接口变更  
 无   
3.内部优化及BUG修改   
3.1. 解决日志库命名冲突问题   
3.2. 获取设备列表方法加锁，防止设备特别多时，容易引起多线程访问导致的崩溃   


----------

### **iOS uSDK_6.0.0** ###

版本号： v6.0.0  
发布日期：2020.07.13  
MD5值：8BAEFC10DED08895D23C5D393CF37B77  
下载链接：[点击下载](https://resource.haigeek.com/download/resource/selfService/admin/uSDK6.0.0_Phone_iOS_20200713111814031.zip)   
注意事项1：此版本不支持海外环境。  
注意事项2：如需统计分析功能，请与统计分析SDK3.2.0版本搭配使用  
更新日志：  

1.新增功能  
1.1  启动待配置状态的新直连设备搜索  
`[uSDKDeviceScanner startScanConfigurableDevice]；`  

1.2 验证码方式绑定新直连设备  

1.2.1 获取新直连绑定验证码方式绑定信息（见uSDKNewDirectLinkVerificationCodeBindInfo）  

​    /**
​     通过uSDKDeviceScanner扫描到的新直连设备对象，device.configType & uSDKDeviceConfigTypeNewDirectLink == 1表示该设备支持新直连配网
​     */
​    @property (nonatomic, strong) uSDKDeviceInfo *deviceInfo;
​    // 验证码  长度[6,63]
​    @property (nonatomic, copy) NSString *verificationCode;

1.2.2 验证码方式绑定新直连设备   

​    @interface uSDKbinding
​    /**
​     新直连设备手动确认方式绑定接口
​    
​     @param bindInfo 新直连绑定验证码方式绑定信息
​     @param progressNotify 进度上报，共四个进度上报
​     1.参数校验成功后上报uSDKBindProgressConnectDevice
​     2.与设备建立连接成功时上报uSDKBindProgressSendConfigInfo
​     3.设备开始校验时上报uSDKBindProgressVerification
​     4.设备开始绑定时上报uSDKBindProgressBindDevice
​     @param success 配网成功时的block回调
​     @param failure 配网失败时的block回调
​     @since 6.0.0
​     */
​     +(void)bindNewDirectLinkDeviceWithManualConfirmBindInfo:(uSDKNewDirectLinkManualConfirmBindInfo *)bindInfo
​       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
​      success:(void(^)(uSDKDevice *device))success
​      failure:(void(^)(NSError *error))failure;


1.3 手动确认校验方式绑定新直连设备   

1.3.1 获取新直连绑定验证码方式绑定信息（见uSDKNewDirectLinkManualConfirmBindInfo）   

​    /**
​     通过uSDKDeviceScanner扫描到的新直连设备对象，device.configType & uSDKDeviceConfigTypeNewDirectLink == 1表示该设备支持新直连配网
​     */
​    @property (nonatomic, strong) uSDKDeviceInfo *deviceInfo;
​    
​    1.3.2 手动确认校验方式绑定新直连设备（见uSDKbinding）
​    /**
​     新直连设备手动确认方式绑定接口
​     @param bindInfo 新直连绑定验证码方式绑定信息。
​     @param progressNotify 进度上报，共四个进度上报
​     1.参数校验成功后上报uSDKBindProgressConnectDevice
​     2.与设备建立连接成功时上报uSDKBindProgressSendConfigInfo
​     3.设备开始校验时上报uSDKBindProgressVerification
​     4.设备开始绑定时上报uSDKBindProgressBindDevice
​     @param success 配网成功时的block回调
​     @param failure 配网失败时的block回调
​     */
​     +(void)bindNewDirectLinkDeviceWithManualConfirmBindInfo:(uSDKNewDirectLinkManualConfirmBindInfo *)bindInfo
​       progressNotify:(void(^)(uSDKBindProgressInfo *bindProgressInfo))progressNotify
​      success:(void(^)(uSDKDevice *device))success
​      failure:(void(^)(NSError *error))failure;


1.4 自发现蓝牙设备可发现已配置的蓝牙设备  

通过`uSDKDeviceScanner`类发现的蓝牙设备列表中，如果***设备已配置，但未连接***，也依然可以通过该类发现，在`uSDKDeviceInfo`中新增属性用于标识设备的配置状态  

​    //设备当前可配置状态
​    typedef NS_ENUM(NSUInteger, uSDKDeviceConfigState) {
​    //设备当前状态已配置
​    uSDKDeviceConfigStateConfigured,
​    //设备当前状态可配置
​    uSDKDeviceConfigStateConfigurable,
​    //设备当前状态可触发进配置
​    uSDKDeviceConfigStateTriggerConfigurable,
​    };
​    
​    @interface uSDKDeviceInfo
​    //设备当前的可配置状态
​    @property (nonatomic, assign, readonly) uSDKDeviceConfigState configState;


1.5 uSDK启动项里增加开启蓝牙搜索配置(见uSDKStartOptions)  

​    //默认为YES，表示开启BLE可控设备搜索，如果不想开启，则将该属性置为NO
​    @property (nonatomic, assign) BOOL enableBLEControllableSearch;

1.6 大循环下获取设备的模块信息(见uSDKDevice)   

​    /**
​     获取设备模块信息
​     @param timeout 超时时间(s)，最小5s，最长120s，建议值15s
​     @param success 成功的回调, 返回uSDKModuleInfo对象
    1. 接口只要执行成功，就会通过success block返回
        2. 但uSDKModuleInfo中具体数据依赖云平台，可能有的字段是空的，可能全都是空的
          @param failure 失败的回调, 返回NSError对象
          */
    - (void)deviceModuleInfoWithTimeout:(NSTimeInterval)timeout
        success:(void(^)(uSDKModuleInfo *info))success
        failure:(void(^)(NSError *error))failure;

1.7 无效命令  

对支持无效命令的设备，进行操作`(read/write/op)`，触发无效命令时，会携带无效命令标识上报给`App`，无效命令标识的值放到`NSError`的`NSLocalizedFailureReasonErrorKey`对应的值中  

1.8 softAp通知App切网  
无论设备侧发起绑定，还是用户侧发起绑定，在发送配置信息后都会通过`switchNetworkNotify`通知`App`进行切网  

1.9 配置绑定增加重试接口   
当配置绑定返回`ERR_USDK_BIND_TIMEOUT_NEED_RETRY_BIND（-16018）`时，可以通过该接口尝试进行重试绑定  

​    /**
​    绑定重试接口
​    当错误码为ERR_USDK_BIND_TIMEOUT_NEED_RETRY_BIND（-16018）时需要调用重试接口试图重新绑定设备，
​    会返回-16018的接口bindDeviceByBLE、bindPureBLEDevice、bindDeviceBySoftAp、bindDeviceBySmartLink、bindDeviceByQRCode、bindNewDirectLinkDevice
​    
​    @param timeoutInterval 绑定超时时间（单位是秒，范围为10秒-180秒）
​    @param success 绑定成功时的block回调
​    @param failure 绑定失败时的block回调
​    @since 6.0.0
​     */
​     +(void)retryBindDeviceWithTimeoutInterval:(NSTimeInterval)timeoutInterval
​       success:(void(^)(uSDKDevice *device))success
​       failure:(void(^)(NSError *error))failure;

2.接口变更   

2.1针对`softap`配网，修改参数`deviceBSSID`为`deviceSSID`, softap配网不再校验`iotDevBssid`(见uSDKSoftApBindInfo)  

​    /**
​     设备SoftAp热点的SSID，必填
​     如果在调用softap绑定接口时，手机连接的热点与该属性的值不匹配，则返回-13016(未连接到设备热点)错误
​     */
​    @property (nonatomic, copy) NSString* iotDeviceSSID;
​    
​    /**
​     设备SoftAp热点的BSSID
​     @deprecated 6.0.0 属性废弃后，不再校验该属性
​     */
​    @property (nonatomic, copy) NSString* iotDevBssid DEPRECATED_ATTRIBUTE;

3.内部优化及BUG修改   

3.1 历史数据，uSDK收到BLE模块发送的空包导致的异常  
3.2bundleID重名导致的崩溃  
3.3 并发授权时导致的崩溃  
3.4  uSDK就绪性能优化  
3.5 iOS下载配置文件使用httpdns  
3.6 长连接建立&重连机制优化  
3.7 用户侧主动绑定和查询绑定结果增加http通道  
3.8 -10003错误码细化  
3.9. 配置绑定优化：无效参数（10001）问题的解决  

----------



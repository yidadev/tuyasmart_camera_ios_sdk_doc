#  报警消息

### 图片消息

摄像头设备通常具有侦测报警的功能，可以通过功能点打开侦测开关。侦测告警主要分两种，声音检测和移动检测。当设备检测到声音或者物体移动时，会上报一个警告消息，消息会附带一帧当前视频的截图。

### 视频消息

直供电门铃设备，提供视频消息的能力。当有人按下门铃时，设备可以上传一段留言视频，这个消息也会通过报警消息获取到，消息体会附带一段6秒的加密视频。

## 获取消息列表

报警消息的相关接口封装在 ```TuyaSmartCameraKit/TuyaSmartCameraMessage.h``` 中。

### 初始化

获取消息列表前，需要使用设备 ID 和时区初始化消息管理器。

```objective-c
- (instancetype)initWithDeviceId:(NSString *)devId timeZone:(NSTimeZone *)timeZone;
```

### 消息日历

可以通过下面接口获取某年某月有报警消息的日期，以便于在日历上直观展示。

```objective-c
/**
 get day list which has messages
 获取有消息的日期

 @param year year
 @param month month
 @param success callback
 @param failure callback
 */
- (void)messageDaysForYear:(NSInteger)year
                     month:(NSInteger)month
                   success:(void (^)(NSArray<NSString *> *result))success
                   failure:(void (^)(NSError *error))failure;
```

```result``` 为字符串数组，元素为日期字符串，如：“01”、“11”、“30”。

### 消息分类

通过下面的接口获取默认的消息分类列表，可以通过分类查询消息。

```objective-c
/**
 get message schemes list
 获取消息分类列表

 @param success callback
 @param failure callback
 */
- (void)getMessageSchemes:(void (^)(NSArray<TuyaSmartCameraMessageSchemeModel *> *result))success
                  failure:(void (^)(NSError *error))failure;
```

#### 消息分类模型

```TuyaSmartCameraMessageSchemeModel``` 为消息类型的数据模型，其中 ```msgCodes``` 属性表示此分类包含哪些类型的消息。在获取消息列表时，可以传入此属性的值，以获取指定类型的消息，其他属性可以查看 ```TuyaSmartCameraMessageModel.h``` 文件中，``` TuyaSmartCameraMessageSchemeModel``` 类的定义。

### 消息列表

获取消息列表的接口定义如下：

```objective-c
/**
 get message list
 获取消息列表

 @param msgCodes msgCodes
 @param offset offset
 @param limit page size
 @param startTime pass 0 for no time limit
 @param endTime end time
 @param success callback
 @param failure callback
 */
- (void)messagesWithMessageCodes:(NSArray *)msgCodes
                          Offset:(NSInteger)offset
                           limit:(NSInteger)limit
                       startTime:(NSInteger)startTime
                         endTime:(NSInteger)endTime
                         success:(void (^)(NSArray<TuyaSmartCameraMessageModel *> *result))success
                         failure:(void (^)(NSError *error))failure;
```

参数定义：

* **msgCodes** : 获取的消息类型列表，可以通过 ```TuyaSmartCameraMessageSchemeModel``` 中的 ```msgCodes``` 属性获取。
* **offset** : 偏移量，指从 offset 个消息开始。
* **limit** : 数量限制，一次获取多少个消息。如果需要一次性获取全部消息，可以传入 ```NSIntegerMax``` 。
* **startTime** : 消息列表的最早时间限制，获取不早于 startTime 上报的消息。传入 0 表示无最早时间限制。
* **endTime** : 消息列表的最晚时间显示，获取不晚于 endTime 上报的消息。
* **success** : 成功回调，返回消息模型的数组。
* **failure** : 失败回调。

### 消息模型

消息模型的具体定义可查看在 ```TuyaSmartCameraMessageModel.h``` 文件中的 ```TuyaSmartCameraMessageModel``` 类。

根据消息类型不同，可能会有不同的附件。通过 ```attachPic``` 属性获取图片附件的地址，```attachVideos``` 属性获取视频附件的地址，通常情况下，这个属性只有一个元素。

### 视频消息

视频消息中的视频附件，是加密后的视频，需要通过 ```TuyaSmartCloudManager``` 提供的接口播放。 ```attachVideos``` 中的元素格式为："视频地址@密钥"，播放视频时，需要同时传入视频地址和密钥。

视频播放相关的接口定义如下，返回 0 表示成功：

```objective-c
/**
 play encrypt video in detect message
 播放报警消息中的加密视频
 
 @param url video path
 @param nStartTime Start time to play
 @param encryptKey encrypt key
 @param callback start callback
 @param finihedCallBack finish callback

 @return error code
*/
- (int)playVideoMessageWithUrl:(NSString *)url startTime:(int)nStartTime encryptKey:(NSString *)encryptKey onResponse:(void (^)(int errCode))callback onFinish:(void (^)(int errCode))finihedCallBack;

/**
 pause play
 暂停播放
 
 @return error code
*/
- (int)pausePlayVideoMessage;

/**
 resume play
 恢复播放
 
 @return error code
*/
- (int)resumePlayVideoMessage;

/**
 stop play
 停止播放
 
 @return error code
*/
- (int)stopPlayVideoMessage;
```

视频消息的播放同云存储视频的播放类似，在接收到视频帧时，会回调下面的代理方法：

```objective-c
- (void)cloudManager:(TuyaSmartCloudManager *)cloudManager didReceivedFrame:(CMSampleBufferRef)frameBuffer videoFrameInfo:(TuyaSmartVideoFrameInfo)frameInfo;
```

结构体```TuyaSmartVideoFrameInfo```中的下面两个属性描述视频的总时长和进度，单位是毫秒。

* **nDuration** : 视频总时长
* **nProgress** : 当前视频帧的进度

视频声音开关同云存储一样，使用下面的接口：

```objective-c
/**
 set mute.
 @param mute mute sound.
 @param success success call back.
 @param failure failed call back.
 */
- (void)enableMute:(BOOL)mute success:(void(^)(void))success failure:(void (^)(NSError * error))failure;
```

示例代码可参考 Demo 工程中的 ```TYCameraMessageViewController``` 。


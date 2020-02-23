## 侦测报警

涂鸦智能摄像机通常具有侦测报警的功能，可以通过设备功能点打开侦测开关。侦测告警主要分两种，声音检测和移动检测。当设备检测到声音或者物体移动时，会上报一个警告消息，如果你的 App 集成了推送功能，App 还会收到一个推送通知。集成推送请参考[集成 Push](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Push.html)。

报警消息通常会附带一张当前视频的截图。

直供电门铃设备，提供视频消息的能力。当有人按下门铃时，设备可以上传一段留言视频，这个消息也会通过报警消息获取到，消息体会附带一段 6 秒的加密视频。

### 消息列表

报警消息的相关接口封装在 ```TuyaSmartCameraKit/TuyaSmartCameraMessage.h``` 中。

#### 初始化

获取消息列表前，需要使用设备 ID 和时区初始化消息管理器。

```objective-c
- (instancetype)initWithDeviceId:(NSString *)devId timeZone:(NSTimeZone *)timeZone;
```

#### 消息日历

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

#### 消息分类

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

```TuyaSmartCameraMessageSchemeModel``` 为消息分类的数据模型，其中 ```msgCodes``` 属性表示此分类包含哪些类型的消息。在获取消息列表时，可以传入此属性的值，以获取指定类型的消息，其他属性可以查看 ```TuyaSmartCameraMessageModel.h``` 文件中，``` TuyaSmartCameraMessageSchemeModel``` 类的定义。

#### 消息类型

消息类型表示报警消息的触发形式，代码中体现为消息模型的`msgCode`属性，取值范围如下：

* **ipc_motion**：移动侦测
* **ipc_doorbell**：门铃呼叫
* **ipc_dev_link**：设备联动
* **ipc_passby**：有人经过
* **ipc_linger**：有人徘徊
* **ipc_leave_msg**：门铃消息留言
* **ipc_connected**：门铃已接听
* **ipc_unconnected**：门铃未接听
* **ipc_refuse**：门铃拒接
* **ipc_human**：人形检测
* **ipc_cat**：宠物检测
* **ipc_car**：车辆检测
* **ipc_baby_cry**：婴儿哭声
* **ipc_bang**：异响
* **ipc_face**：检测到人脸
* **ipc_antibreak**：强拆报警
* **ipc_low_battery**：低电量告警

由于设备能力的不同，能触发的消息类型会有差别。消息分类和消息类型不同，消息类型表示报警消息的触发方式，消息分类是将一个或多个类型的消息组合成一个大类，比如 `ipc_passby`、`ipc_linger`、`ipc_motion` 可以组合成一个分类为移动侦测。

#### 消息列表

获取消息列表的接口定义如下：

```objective-c
/**
 获取消息列表

 @param msgCodes 消息类型
 @param offset 偏移量
 @param limit 分页大小
 @param startTime 传 0 表示无开始时间限制
 @param endTime 结束时间
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

/**
批量删除消息

 @param msgIds 需要删除的消息的id数组，TuyaSmartCameraMessageModel.msgId
 @param success callback
 @param failure callback
*/
- (void)removeMessagesWithMessageIds:(NSArray *)msgIds
                             success:(void (^)(void))success
                             failure:(void (^)(NSError *))failure;
```

获取消息列表的参数定义：

* **msgCodes** : 获取的消息类型列表，可以通过 ```TuyaSmartCameraMessageSchemeModel``` 中的 ```msgCodes``` 属性获取。
* **offset** : 偏移量，指从 offset 个消息开始。
* **limit** : 数量限制，一次获取多少个消息。最大为 200。
* **startTime** : 消息列表的最早时间限制，获取不早于 startTime 上报的消息。传入 0 表示无最早时间限制。
* **endTime** : 消息列表的最晚时间显示，获取不晚于 endTime 上报的消息。
* **success** : 成功回调，返回消息模型的数组。
* **failure** : 失败回调。

#### 消息模型

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

### 报警消息与存储卡回放

报警消息和存储卡回放没有直接联系，唯一的关联是在存储卡事件录制模式的情况下，报警消息和存储卡视频录制的触发原因和时间点是一样的。

报警消息保存在涂鸦云端，存储卡视频录像保存在摄像机的存储卡中，且存储卡中的视频在容量不足时，可能会被覆盖。存储卡录制的开关和侦测报警的开关也没有关联，所以即使在存储卡事件录制的模式下，报警消息和存储卡中的视频录像也不是一一对应的。

但是存在报警消息发生的时间点有视频录像的情况，IPC SDK 并不提供这种关联查找的接口，开发者可以通过报警消息的触发时间，在当天的存储卡录像视频片段中查找是否有对应的视频录像来建立这种关联。

### 示例代码

完整的示例代码请参考 demo 程序中的`TYCameraMessageViewController.m`文件。

__Objective-C__

```objective-c
- (void)enableDetect {
		if ([self.dpManager isSupportDP:TuyaSmartCameraMotionDetectDPName]) {
        bool motionDetectOn = [[self.dpManager valueForDP:TuyaSmartCameraMotionDetectDPName] tysdk_toBool];
        if (!motionDetectOn) {
            [self.dpManager setValue:@(YES) 
             									 forDP:TuyaSmartCameraMotionDetectDPName 
             								 success:^(id result) {
                // 开启移动侦测成功
            } failure:^(NSError *error) {
								// 网络错误
            }];
        }
      	[self.dpManager setValue:TuyaSmartCameraMotionHigh
                         	 forDP:TuyaSmartCameraMotionSensitivityDPName
                         success:^(id result) {
            // 成功设置移动侦测灵敏度为高灵敏度
        } failure:^(NSError *error) {
            // 网络错误.
        }];
    }
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.cameraMessage = [[TuyaSmartCameraMessage alloc] initWithDeviceId:self.devId timeZone:[NSTimeZone defaultTimeZone]];
    [self.cameraMessage getMessageSchemes:^(NSArray<TuyaSmartCameraMessageSchemeModel *> *result) {
	      // 消息分类模型
        self.schemeModels = result;
				// 获取第一个分类的消息
        [self reloadMessageListWithScheme:result.firstObject];
    } failure:^(NSError *error) {
        // 请求失败
    }];
}

- (void)reloadMessageListWithScheme:(TuyaSmartCameraMessageSchemeModel *)schemeModel {
    NSDateFormatter *formatter = [NSDateFormatter new];
    formatter.dateFormat = @"yyyy-MM-dd";
    NSDate *date = [formatter dateFromString:@"2020-02-17"];
	  // 获取从2020年2月17日零点到现在的报警消息中的前20条
    [self.cameraMessage messagesWithMessageCodes:schemeModel.msgCodes Offset:0 limit:20 startTime:[date timeIntervalSince1970] endTime:[[NSDate new] timeIntervalSince1970] success:^(NSArray<TuyaSmartCameraMessageModel *> *result) {
        self.messageModelList = result;
    } failure:^(NSError *error) {
        NSLog(@"error: %@", error);
    }];
}

```

__Swift__

```swift
func enableDectect() {
    guard self.dpManager.isSupportDP(.motionDetectDPName) else {
        return;
    }
    if let isMontionDetectOn = self.dpManager.value(forDP: .motionDetectDPName) as? Bool, !isMontionDetectOn {
        self.dpManager.setValue(true, forDP: .motionDetectDPName, success: { _ in
            // 开启移动侦测成功
        }) { _ in
            // 网络错误
        }
    }
    self.dpManager.setValue(TuyaSmartCameraMotion.high, forDP: .motionSensitivityDPName, success: { _ in
        // 成功设置移动侦测灵敏度为高灵敏度
    }) { _ in
        // 网络错误
    }
}

override func viewDidLoad() {
    super.viewDidLoad()
    self.cameraMessage = TuyaSmartCameraMessage(deviceId: self.devId, timeZone: NSTimeZone.default)
    self.cameraMessage.getSchemes({ result in
        // 获取第一个分类的消息
        self.schemeModels = result
        if let schemeModel = result?.first {
            reloadMessage(schemeModel: schemeModel)
        }
    }) { _ in
        // 请求失败
    }
}
    
func reloadMessage(schemeModel: TuyaSmartCameraMessageSchemeModel) {
    let formatter = DateFormatter()
    formatter.dateFormat = "yyyy-MM-dd"
    let date = formatter.date(from: "2020-02-17")
    // 获取从2020年2月17日零点到现在的报警消息中的前20条
    self.cameraMessage.messages(withMessageCodes: schemeModel.msgCodes, offset: 0, limit: 20, startTime: Int(date!.timeIntervalSince1970), endTime: Int(Date().timeIntervalSince1970), success: { result in
        self.messageModelList = result;
    }) { _ in
        // 请求失败
    }
}
```




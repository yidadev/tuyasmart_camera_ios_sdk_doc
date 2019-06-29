## 摄像头控制

摄像头控制相关的所有功能对应于 ```TuyaSmartCameraType``` 协议。在需要使用到摄像头的页面添加头文件：

```objective-c
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```

### 回调代理

摄像头相关操作都是异步，操作的结果通过 ``` TuyaSmartCameraDelegate ``` 的代理方法返回。下面所述中的 “成功回调” 皆是指操作成功时调用的代理方法。

#### p2p 连接成功回调

当 p2p 连接成功的时候，会调用下面的代理方法，可以在此代理方法中开启预览或者回放。

```objective-c
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
    // start preview
}
```

#### p2p 连接断开回调

当 p2p 断开的时候，回调用下面的代理方法。可以在此代理方法中重新连接。

```objective-c
- (void)cameraDisconnected:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 预览成功回调

开启预览成功时会回调下面的代理方法。可以此代理方法中获取视频清晰度等操作。

```objective-c
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 停止预览回调

```objective-c
- (void)cameraDidStopPreview:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 回放日期查询回调

调用查询回放日期，收到设备返回的数据后，会调用下面的代理方法，日期以整数数组返回，如 @[ @(1), @(2), @(3)]，表示这个月 1，2，3有回放视频数据。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveRecordDayQueryData:(NSArray<NSNumber *> *)days {
    
}
```

#### 回放视频时间片段查询回调

调用查询某一天的回放时间片段，收到设备返回的数据后，会调用下面的代理方法。时间片段数据以字段数组返回。一个字典表示一段回放视频的起始时间，其中的 key 有定义常量，具体如下：

* kTuyaSmartPlaybackPeriodStartDate ：开始时间，NSDate，时区为手机时间时区
* kTuyaSmartPlaybackPeriodStopDate： 结束时间，NSDate，时区为手机时间时区
* kTuyaSmartPlaybackPeriodStartTime： 开始时间，Unix 时间戳，播放时，可传入此字段的值
* kTuyaSmartPlaybackPeriodStopTime： 结束时间，Unix 时间戳，播放时，可传入此字段的值

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveTimeSliceQueryData:(NSArray<NSDictionary *> *)timeSlices {
    
}
```

#### 开始回放回调

开始回放成功后，会调用下面的代理方法。

```objective-c
- (void)cameraDidBeginPlayback:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 回放暂停回调

```objective-c
- (void)cameraDidPausePlayback:(id<TuyaSmartCameraType>)camera {
    	
}
```

#### 回放恢复回调

```objective-c
- (void)cameraDidResumePlayback:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 回放停止回调

```objective-c
- (void)cameraDidStopPlayback:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 回放结束回调

当某一个回放片段或者某一天的回放视频播放完毕时，会调用下面的代理方法。

```objective-c
- (void)cameraPlaybackDidFinished:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 第一帧回调

当开启预览或者开始回放成功后，收到了第一帧视频图像的时候，会调用下面的代理方法，并返回第一帧图像。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveFirstFrame:(UIImage *)image {
    
}
```

#### 视频帧数据回调（必须实现）

当收到视频帧数据的时候，会回调下面的代理方法，返回视频帧的 YUV 数据和帧信息。如果你需要自己实现视频渲染的话，可以在这里接收视频帧数据。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didReceiveVideoFrame:(CMSampleBufferRef)sampleBuffer frameInfo:(TuyaSmartVideoFrameInfo)frameInfo {
    
}
```



#### 清晰度状态回调

当调用获取清晰度后，或者设备端发送的视频数据分辨率改变时，会调用下面的代理方法，目前只有高清/标清两种状态，所以回调的状态是布尔值，YES 表示高清，NO 表示标清。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveDefinitionState:(BOOL)isHd {
    
}
```

#### 声音状态回调

当调用开关声音，或者设备到手机的声音开关状态改变时，会回调下面的代理方法。isMute 为 YES 表示静音状态，为NO表示声音开启。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveMuteState:(BOOL)isMute playMode:(TuyaSmartCameraPlayMode)playMode {
    
}
```

#### 对讲开始回调

调用开始对讲成功后，会调用下面的代理方法。

```objective-c
- (void)cameraDidBeginTalk:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 对讲停止回调

调用停止对讲成功后，会调用下面的代理方法。

```objective-c
- (void)cameraDidStopTalk:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 截图保存成功回调

调用截图会立即返回当前视频帧图像，并自动将图像保存到系统相册，如果保存成功，会调用下面的代理方法。

```objective-c
- (void)cameraSnapShootSuccess:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 录制开始回调

调用开始录制成功后，会调用下面的代理方法。

```objective-c
- (void)cameraDidStartRecord:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 录制保存成功回调

调用停止录制后，录制的视频会自动保存到系统相册，保存成功后，会调用下面的代理方法。

```objective-c
- (void)cameraDidStartRecord:(id<TuyaSmartCameraType>)camera {
    
}
```

#### 操作失败回调

当操作失败时，会统一由以下方法回调。

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didOccurredError:(TYCameraErrorCode)errCode;
```

errCode 表示哪个操作失败。具体errCode的定义如下：

```objective-c
typedef enum {
TY_ERROR_NONE,                      // 0
TY_ERROR_CONNECT_FAILED,            // 1    连接失败（包含超时，密码错误，不支持的设备等）
TY_ERROR_CONNECT_DISCONNECT,        // 2    连接断开
TY_ERROR_ENTER_PLAYBACK_FAILED,     // 3    连接回放通道失败
TY_ERROR_START_PREVIEW_FAILED,      // 4    预览失败（如果在预览过程中，连接断开，也会回调这个错误）
TY_ERROR_START_PLAYBACK_FAILED,     // 5    回放失败（如果在回放过程中，回放通道的连接断开，也会回调这个错误）
TY_ERROR_PAUSE_PLAYBACK_FAILED,     // 6    暂停回放失败
TY_ERROR_RESUME_PLAYBACK_FAILED,    // 7    恢复回放失败
TY_ERROR_ENABLE_MUTE_FAILED,        // 8    设置静音状态失败（通常在关闭静音，也就是开启声音监听的时候才会发生错误）
TY_ERROR_START_TALK_FAILED,         // 9    开启对讲失败
TY_ERROR_SNAPSHOOT_FAILED,          // 10   截图失败
TY_ERROR_RECORD_FAILED,             // 11   录制失败（开启录制和停止录制的操作都可能会触发这个错误）
TY_ERROR_ENABLE_HD_FAILED,          // 12   设置高清标清状态失败
TY_ERROR_GET_HD_FAILED,             // 13   获取高清标清状态失败
TY_ERROR_QUERY_RECORD_DAY_FAILED,   // 14   查询有视频记录的日期失败
TY_ERROR_QUERY_TIMESLICE_FAILED,    // 15   查询某天的所有视频记录失败
} TYCameraErrorCode;
```

### 创建摄像头对象

创建摄像头之前，需要先根据设备 id 获取对应摄像头的配置参数，使用的类是 ```TuyaSmartRequest```。

创建摄像头配置类 ```TuyaSmartCamearConfig```对象，还需要用到```TuyaSmartDeviceModel```类中的 ```localKey``` 属性。设备对应的```TuyaSmartDeviceModel```模型可以通过```TuyaSmartDevice```来获取，这样可以保证获取到的数据是最新，SDK内部会自动同步设备的信息。

可以通过 ``` TuyaSmartCameraFactory``` 工具类来创建配置对象和摄像头对象，具体代码如下：

```objective-c
#define kTuyaSmartIPCConfigAPI @"tuya.m.ipc.config.get"
#define kTuyaSmartIPCConfigAPIVersion @"2.0"

- (void)createCamera {
	TuyaSmartDevice *device = [TuyaSmartDevice deviceWithDeviceId:@"your_device_id"];
	NSString *localKey = device.deviceModel.localKey;
    
	__weak typeof(self) weakSelf = self;
	[[TuyaSmartRequest new] requestWithApiName:kTuyaSmartIPCConfigAPI postData:@{@"devId": self.devId} version:kTuyaSmartIPCConfigAPIVersion success:^(id result) {
    	__strong typeof(weakself) self = weakself;
	    // SDK 初始化的时候，可能会阻塞线程，建议在子线程创建Camera对象
    	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        	// 根据接口返回的数据创建Config对象
	        TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:nil localKey:localKey configData:result];
        	id<TuyaSmartCameraType> camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType config:config delegate:self];
    	});
	} failure:^(NSError *error) {
    	NSLog(@"error: %@", error);
	}];
}
```

### 视频渲染视图

设置```TuyaSmartCameraType``` 对象的```autoRender```属性为 YES（默认就是YES），SDK 内部会自动渲染视频图像，渲染视频图像的视图实现 ```TuyaSmartVideoViewType```协议。通过```[camera videoView]```可以获取到默认的视频渲染视图。

* 获取视频渲染视图

```objective-c
- (UIView<TuyaSmartVideoViewType> *)videoView;
```

示例：

```objective-c
- (void)viewDidLoad {
    // 前提是 TuyaSmartCameraType 对象创建成功
	CGFloat ScreenWidth = [UIScreen mainScreen].bounds.size.width;
	CGFloat ScreenHeight = [UIScreen mainScreen].bounds.size.height;
	CGFloat VideoWidth = ScreenWidth;
	CGFloat VideoHeight = VideoWidth * 9 / 16;

	UIView<TuyaSmartVideoViewType> *videoView = [self.camera videoView];
	videoView.frame = CGRectMake(0, 64, VideoWidth, VideoHeight);
	[self.view addSubview:videoView];
}
```

* 视频缩放、移动、清除操作

```objective-c
// 将视频图像放大1.2倍
[videoView tuya_setScaled:1.2];
// 将视频图像向右移动10.0个pt点
[videoView tuya_setOffset:CGPointMake(10.0, 0.0)];
// 清除图像，并将缩放倍数，和图像位移重置
[videoView tuya_clear]; 
// 图像渲染会拉伸铺满整个视图
videoView.scaleToFill = YES;
```
### 摄像头功能

摄像头功能接口封装为 ```TuyaSmartCameraType``` 协议。

#### 连接p2p通道

在开始获取视频流之前，需要先连接 p2p 通道。

```objective-c
- (void)connect;
```

示例：

```objective-c
// 连接命令
- (void)viewDidLoad {
	// other code...
	[self.camera connect];
}

// 成功回调
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	self.isConnected = YES;
}

// 连接断开回调
- (void)cameraDisconnected:(id<TuyaSmartCameraType>)camera {
	self.isConnected = NO;
	NSLog(@"---disconnected");
}
```
#### 播放模式

定义预览还是回放，在设置静音状态时，需要传入此参数，以指定设置哪个模式是否静音。

```objective-c
typedef NS_ENUM(NSUInteger, TuyaSmartCameraPlayMode) {
	TuyaSmartCameraPlayModeNone,        // 无
	TuyaSmartCameraPlayModePreview,     // 预览
	TuyaSmartCameraPlayModePlayback     // 回放
};
```
#### 开启预览

在 p2p 连接成功后，就可以开启预览了。

```objective-c
- (void)startPreview;
```

示例：

```objective-c
- (void)startPreview {
	// 判断是否在录制视频，如果正在录制视频，停止录制。(是否在录制中的状态由开发者自己维护)
	if (self.isRecording) {
		[self.camera stopRecord];
	}
	// 判断是否正在预览，如果正在预览，停止预览。(是否在预览中的状态由开发者自己维护)
	if (self.isPlaybacking) {
		[self.camera stopPlayback];
	}
	// 判断p2p通道是否已经连接，如果未连接或者连接已经断开，连接通道。(p2p通道是否连接的状态由开发者自己维护)
	if (self.isConnected) {
        // 开启预览
		[self.camera startPreview];
    }else {
        [self.camera connect];
    }
}
// 成功回调
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
	// 当前的播放模式
	self.playMode = TuyaSmartCameraPlayModePreview;
	// 预览状态
	self.isPreviewing = YES;
}
```
* 注：当前摄像头的状态，如正在预览，录制，回放，p2p通道已连接，连接断开等，由开发者自己维护，SDK中不会保留这些状态值，只负责命令的下发与回调。下面不再赘述。


#### 停止预览

```objective-c
- (void)stopPreview;
```

示例：

```objective-c
- (void)stopPreview {
	// 若正在录制视频，停止录制
	if (self.isRecording) {
		[self.camera stopRecord];
	}
	// 如果正在对讲，则关闭对讲
	if (self.isTalking) {
		[self.camera startTalk];
	}
	// 停止预览，此操作不会失败
	[self.camera stopPreview];
	self.isPreviewing = NO;
	self.playMode = TuyaSmartCameraPlayModeNone;
}

// 成功回调
- (void)cameraDidStopPreview:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---stop preview");
}
```
#### 获取有回放视频记录的日期

在开始回放前，需要获取到回放视频记录的信息。首先获取有回放视频记录的日期

```objective-c
- (void)queryRecordDaysWithYear:(NSUInteger)year month:(NSUInteger)month;
```

示例：

```objective-c
- (void)queryPlaybackDays {
	// 根据年份和月份查询某一月中有回放视频记录的日期
	[self.camera queryRecordDaysWithYear:2018 month:7];
}

// 成功回调
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveRecordDayQueryData:(NSArray<NSNumber *> *)days {
// days是一个 NSNumber 类型的日期数组，包含有回放视频记录的日期，日期的值从 1 到 31 之间
	self.playbackDays = days;
	self.curentDay = days.lastObject.integerValue;
}
```
#### 获取某日的视频回放信息

获取到有用回放记录的日期后，根据日期获取当日的视频回放记录

```objective-c
- (void)queryRecordTimeSliceWithYear:(NSUInteger)year month:(NSUInteger)month day:(NSUInteger)day;
```

示例：

```objective-c
- (void)queryPlaybackRecords {
	// 根据日期查询回放视频信息
	[self.camera queryRecordTimeSliceWithYear:2018 month:7 day:self.curentDay];
}

// 成功回调
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveTimeSliceQueryData:(NSArray<NSDictionary *> *)timeSlices {
// timeSlices 是一个字典数组，包含当日所有的回放视频片段信息。
// 每个字典对应一个回放视频片段，字典包含四个字段：
/*
/// 回放片段开始时间的key, 值类型是 NSDate，基本手机系统本地时间的时区
FOUNDATION_EXTERN NSString * const kTuyaSmartTimeSliceStartDate;

/// 回放片段结束时间的key, 值类型是 NSDate，基本手机系统本地时间的时区
FOUNDATION_EXTERN NSString * const kTuyaSmartTimeSliceStopDate;

/// 回放片段开始时间的key, 值类型是 NSNumber，Unix时间戳，基于格林时间
FOUNDATION_EXTERN NSString * const kTuyaSmartTimeSliceStartTime;

/// 回放片段结束时间的key, 值类型是 NSNumber，Unix时间戳，基于格林时间
FOUNDATION_EXTERN NSString * const kTuyaSmartTimeSliceStopTime;
*/
	self.timeSlices = timeSlices;
}
```
#### 开启回放

开始播放一段回放视频需要传入开始播放的时间戳 playTime，回放视频片段的开始时间 startTime, 回放视频片段的结束时间：stopTime。playTime 必须要在 starTime 和 stopTime 之间。如果正在播放某一段回放视频，想要播放更改播放点，或者播放其他视频片段，可以再次调用此方法。

```objective-c
- (void)startPlayback:(NSInteger)playTime startTime:(NSInteger)startTime stopTime:(NSInteger)stopTime;
```

示例：

```objective-c
- (void)startPlayBack:(NSDictionary)timeSlice {
	// 如果正在预览，则停止预览。包括停止录制，对讲等操作。详见上述的停止预览方法。
	if (self.isPreviewing) {
		[self stopPreview];
	}
	// timeSlice 取自上面视频片段回放信息数组里的一个元素
	NSInteger startTime = [[timeSlice objectForKey:kTuyaSmartTimeSliceStartTime] integerValue];
	NSInteger stopTime = [[timeSlice objectForKey:kTuyaSmartTimeSliceStopTime] integerValue];
	// 开始回放视频片段，传入开始播放的时间戳，视频片段的开始时间戳，结束时间戳
	// 开始播放的时间戳需要在视频片段的开始时间与结束时间之间，这里默认从开始时间开始播放
	[self.camera startPlayback:startTime startTime:startTime stopTime:stopTime];
}

// 成功回调
- (void)cameraDidBeginPlayback:(id<TuyaSmartCameraType>)camera {
	self.playMode = TuyaSmartCameraPlayModePlayback;
	self.isPlaybacking = YES;
}
```
#### 暂停回放

```objective-c
- (void)pausePlayback {
	// 如果正在录制，停止录制
	if (self.isRecording) {
		[self.camera stopRecord];
	}
	// 暂停回放
	[self.camera pausePlayback];
}

// 成功回调
- (void)cameraDidPausePlayback:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---pause playback");
}
```
#### 恢复回放

```objective-c
- (void)resumePlayback {
	// 恢复回放
	[self.camera resumePlayback];
}

// 成功回调
- (void)cameraDidResumePlayback:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---resume playback");
}
```
#### 停止回放

```objective-c
- (void)stopPlayback {
	// 如果正在录制，停止录制
	if (self.isRecording) {
		[self.camera stopRecord];
}
	// 停止回放
	[self.camera stopPlayback];
}

// 成功回调
- (void)cameraDidStopPlayback:(id<TuyaSmartCameraType>)camera {
	self.playMode = TuyaSmartCameraPlayModeNone;
	self.isPlaybacking = NO;
}
```
#### 开启视频录制

```objective-c
- (void)startRecord {
	// 如果没有播放任何视频，或者已经在录制中，不做任何操作
	if (self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording) {
		return;
	}

	// 如果正在对讲，则关闭对讲
	if (self.isTalking) {
		[self.camera startTalk];
	}

	// 开启录制
	[self.camera startRecord];
}

// 成功回调
- (void)cameraDidStartRecord:(id<TuyaSmartCameraType>)camera {
	self.isRecording = YES;
}
```
#### 停止视频录制

```objective-c
- (void)stopRecord {
	// 如果正在录制视频，停止录制
	if (self.isRecording) {
		[self.camera stopRecord];
	}
}

// 成功回调
- (void)cameraDidStopRecord:(id<TuyaSmartCameraType>)camera {
	self.isRecording = NO;
}
```
* 注：如果视频录制成功，会直接保存在手机相册。同时，会在手机相册中创建一个与APP同名的自定义相册，视频将会出现在这个自定义相册中。


#### 视频截图

```objective-c
- (void)snapShoot {
	// 如果没有播放任何视频，不做任何操作
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// 截图操作
	[self.camera snapShoot];
}

// 成功回调
- (void)camearaSnapShootSuccess:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---snap shoot success");
}
```
* 注：如果截图成功，图片会直接保存在手机相册。同时，会在手机相册中创建一个与APP同名的自定义相册，截图将会出现在这个自定义相册中。


#### 设置静音

```objective-c
- (void)enabelMute:(BOOL)isMuted {
	// 如果当前没有播放视频，则不做任何操作
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// 给当前的播放模式设置静音状态
	// isMuted: YES-关闭视频声音；NO-开启视频声音
	// 视频的声音默认是关闭的
	[self.camera enableMute:isMuted forPlayMode:self.playMode];
}

// 静音状态改变回调
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveMuteState:(BOOL)isMute playMode:(TuyaSmartCameraPlayMode)playMode {
	self.isMuted = isMute;
}
```
#### 清晰度获取与更改

```objective-c
// 预览开启成功的回调
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
	// other code ...
	// 发送查询清晰度的命令
	[self.camera getHD];

	// 更改清晰度
	// [self.camera enableHD:YES];
}

// 清晰度状态回调，查询或设置清晰度的结果，统一由这个代理方法回调
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveDefinitionStatus:(BOOL)isHd {
	// isHd: 是否高清；YES-高清；NO-标清
	self.isHd = isHd;
}
```
#### 开启对讲

```objective-c
- (void)startTalk {
	// 如果正在录制，停止录制
	if (self.isRecording) {
		[self.camera stopRecord];
	}

	// 发送开启对讲的命令
	[self.camera startTalk];
}

// 成功回调
- (void)cameraDidBeginTalk:(id<TuyaSmartCameraType>)camera {
	self.isTalking = YES;
}
```
#### 停止对讲

```objective-c
- (void)stopTalk {
	// 如果正在对讲，发送停止对讲的命令
	if (self.isTalking) {
		[self.camera stopTalk];
	}
}

// 成功回调
- (void)cameraDidStopTalk:(id<TuyaSmartCameraType>)camera {
	self.isTalking = NO;
}
```
* 注：对讲和录制是互斥的，而且只有在预览时可以开启对讲。


#### 分辨率改变回调

```objective-c
// 视频图像分辨率更改的回调
- (void)camera:(id<TuyaSmartCameraType>)camera resolutionDidChangeWidth:(NSInteger)width height:(NSInteger)height {
	NSLog(@"---resolution changed: %ld x %ld", width, height);
}
```
#### 开启裸流。

如果需要自己渲染视频流，可以开启获取裸流属性。开启此属性后，p2p 1.0 会返回未解码的原始数据，p2p 2.0 会返回解码后的 YUV 数据。

```objective-c
- (void)viewDidLoad {
	// other init code
	// 当camera.isRecvFrame 为YES时，将不会自动渲染视频画面，视频帧数据将通过下面的代理方法给出。
	self.camera.isRecvFrame = YES;
}

/**
p2p 1.0 获取未解码原始视频帧数据的代理方法。
@param camera      摄像头对象
@param frameData   原始视频帧数据
@param size        视频帧数据大小
@param frameInfo   视频帧信息，包含编码方式和时间戳
*/
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didReceiveFrameData:(const char *)frameData dataSize:(unsigned int)size frameInfo:(TuyaSmartVideoStreamInfo)frameInfo;

/**
p2p 2.0 获取解码后的YUV数据的代理方法。
@param camera      		摄像头对象
@param sampleBuffer   	YUV视频帧数据
@param frameInfo   		视频帧信息，包含分辨率，帧率和时间戳
*/
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didReceiveVideoFrame:(CMSampleBufferRef)sampleBuffer frameInfo:(TuyaSmartVideoFrameInfo)frameInfo;

```
* 注:  开启获取裸流后,原始的渲染视图(videoVIew)就不会渲染图像,需要开发者自行通过该接口的数据进行解析渲染。


#### 销毁资源

```objective-c
// 摄像头面板销毁时，销毁摄像头资源。
- (void)dealloc {
	[self.camera destory];
}
```


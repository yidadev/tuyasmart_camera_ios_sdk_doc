# sdk	摄像头功能

### 操作失败回调

摄像头相关操作都是异步，操作的结果通过 ``` TuyaSmartCameraDelegate ``` 的代理方法返回。下面所述中的 “成功回调” 皆是指操作成功时调用的代理方法。当操作失败时，会统一由以下方法回调。

__Objective-C__

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didOccurredError:(TYCameraErrorCode)errCode;
```

__Swift__

``` swift
func camera(_ camera: TuyaSmartCameraType!, didOccurredError errCode: TYCameraErrorCode)
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


### 视频播放视图

* 获取视频播放视图

__Objective-C__

```objective-c
- (void)viewDidLoad {
CGFloat ScreenWidth = [UIScreen mainScreen].bounds.size.width;
CGFloat ScreenHeight = [UIScreen mainScreen].bounds.size.height;
CGFloat VideoWidth = ScreenWidth;
CGFloat VideoHeight = VideoWidth * 9 / 16;

UIView<TuyaSmartVideoViewType> *videoView = [self.camera videoView];
videoView.frame = CGRectMake(0, 64, VideoWidth, VideoHeight);
[self.view addSubview:videoView];
}
```

__Swift__

``` swift
let screenWidth = UIScreen.main.bounds.size.width
let screenHeight = UIScreen.main.bounds.size.height
let videoWidth = screenWidth
let videoHeight = screenWidth * 9 / 16
        
let videoView = self.camera.videoView as TuyaSmartVideoViewType
        videoView.frame = CGRect(x: 0, y: 64, width: videoWidth, height: videoHeight)
self.view.addSubview(videoView)
```

* 视频缩放、移动、清除操作

__Objective-C__

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

__Swift__

``` swift
videoView?.tuya_setScaled(1.2)
videoView?.tuya_setOffset(CGPoint(10.0, 0.0))
videoView?.tuya_clear()
videoView?.scaleToFill = true
```

### 连接p2p通道

__Objective-C__

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

__Swift__

``` swift
override func viewDidLoad() {
    super.viewDidLoad()
    self.camera?.connect()
}

func cameraDidConnected(_ camera: TuyaSmartCameraType!) {
    self.isConnected = true
}
    
func cameraDisconnected(_ camera: TuyaSmartCameraType!) {
    self.isConnected = false
}

```

### 连接回放通道

p2p连接需要建立两个通道，默认通道用于命令下发与预览的音视频数据传输，若要回放功能，需要在默认通道连接后，再建立一个回放通道，用于回放的音视频传输。

__Objective-C__

```objective-c
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	self.isConnected = YES;
	// 建立回放通道
	[self.camera enterPlayback];
}

// 成功回调
- (void)cameraDidConnectPlaybackChannel:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---didconnectedplayback");
}
```

__Swift__

``` swift
func cameraDidConnected(_ camera: TuyaSmartCameraType!) {
    self.isConnected = true
    self.camera.enterPlayback()
}
    
func cameraDidConnectPlaybackChannel(_ camera: TuyaSmartCameraType!) {
    print("---didconnectedplayback")
}
```

### 播放模式

定义预览还是回放，在设置静音状态时，需要传入此参数，以指定设置哪个模式是否静音。

```objective-c
typedef NS_ENUM(NSUInteger, TuyaSmartCameraPlayMode) {
	TuyaSmartCameraPlayModeNone,        // 无
	TuyaSmartCameraPlayModePreview,     // 预览
	TuyaSmartCameraPlayModePlayback     // 回放
};
```
### 开启预览

__Objective-C__

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
	if (!self.isConnected) {
		[self.camera connect];
	}
		// 开启预览
		[self.camera startPreview];
	}
// 成功回调
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
	// 当前的播放模式
	self.playMode = TuyaSmartCameraPlayModePreview;
	// 预览状态
	self.isPreviewing = YES;
}
```

__Swift__

``` swift
func startPreview() {
    // 判断是否在录制视频，如果正在录制视频，停止录制。(是否在录制中的状态由开发者自己维护)
    if self.isRecording {
        self.camera.stopRecord()
    }
    // 判断是否正在预览，如果正在预览，停止预览。(是否在预览中的状态由开发者自己维护)
    if self.isPlaybacking {
        self.camera.stopPlayback()
    }
    // 判断p2p通道是否已经连接，如果未连接或者连接已经断开，连接通道。(p2p通道是否连接的状态由开发者自己维护)
    if self.isConnected == false {
        self.camera.connect()
    }
    // 开启预览
    self.camera.startPreview()
}

 // 成功回调
func cameraDidBeginPreview(_ camera: TuyaSmartCameraType!) {
    // 当前的播放模式
    self.playMode = TuyaSmartCameraPlayModePreview
    // 预览状态
    self.isPreviewing = true
}
```

* 注：当前摄像头的状态，如正在预览，录制，回放，p2p通道已连接，连接断开等，由开发者自己维护，SDK中不会保留这些状态值，只负责命令的下发与回调。下面不再赘述。


### 停止预览

__Objective-C__

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

__Swift__

``` swift
func stopPreview() {
    // 若正在录制视频，停止录制
    if self.isRecording {
        self.camera.stopRecord()
    }
    // 如果正在对讲，则关闭对讲
	if self.isTalking {
		self.camera.startTalk()
	}
	// 停止预览，此操作不会失败
	self.camera.stopPreview()
	self.isPreviewing = false
	self.playMode = TuyaSmartCameraPlayModeNone
}

func cameraDidStopPreview(_ camera: TuyaSmartCameraType!) {
    print("---stop preview")
}
```

### 获取有回放视频记录的日期

在开始回放前，需要获取到回放视频记录的信息。首先获取有回放视频记录的日期

__Objective-C__

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

__Swift__

``` swift
func queryPlaybackDays() {
    self.camera.queryRecordDays(withYear: 2018, month: 7)
}

func camera(_ camera: TuyaSmartCameraType!, didReceiveRecordDayQueryData days: [NSNumber]!) {
    // days是一个 NSNumber 类型的日期数组，包含有回放视频记录的日期，日期的值从 1 到 31 之间
    self.playbackDays = days
    self.curentDay = days.last?.intValue
}
```

### 获取某日的视频回放信息

获取到有用回放记录的日期后，根据日期获取当日的视频回放记录

__Objective-C__

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

__Swift__

``` swift

func queryPlaybackRecords() {
    // 根据日期查询回放视频信息
    self.camera.queryRecordTimeSlice(withYear: 2018, month: 7, day: self.currentDay)
}

// 成功回调
func camera(_ camera: TuyaSmartCameraType!, didReceiveTimeSliceQueryData timeSlices: [[AnyHashable : Any]]!) {
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
	self.timeSlices = timeSlices
}
```

### 开启回放

__Objective-C__

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

__Swift__

``` swift

func startPlayBack(timeSlice: Dictionary) {
    // 如果正在预览，则停止预览。包括停止录制，对讲等操作。详见上述的停止预览方法。
	if self.isPreviewing {
		self.stopPreview()
	}
	// timeSlice 取自上面视频片段回放信息数组里的一个元素
	let startTime = timeSlice[kTuyaSmartTimeSliceStartTime] as? Int
	let stopTime = timeSlice[kTuyaSmartTimeSliceStopTime] as? Int
	// 开始回放视频片段，传入开始播放的时间戳，视频片段的开始时间戳，结束时间戳
	// 开始播放的时间戳需要在视频片段的开始时间与结束时间之间，这里默认从开始时间开始播放
	self.camera.startPlayback(startTime, startTime: startTime, stopTime: stopTime)
}

func cameraDidBeginPlayback(_ camera: TuyaSmartCameraType!) {
    self.playMode = TuyaSmartCameraPlayModePlayback
	self.isPlaybacking = true
}
```

### 暂停回放

__Objective-C__

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

__Swift__

``` swift
func pausePlayback() {
    if self.isRecording {
        self.camera.stopRecord()
    }
    self.camera.pausePlayback()
}

func cameraDidPausePlayback(_ camera: TuyaSmartCameraType!) {
    print("---pause playback")
}
```

### 恢复回放

__Objective-C__

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

__Swift__

``` swift
func resumePlayback() {
    self.camera.resumePlayback()
}

func cameraDidResumePlayback(_ camera: TuyaSmartCameraType!) {
    print("---resume playback")
}
```

### 停止回放

__Objective-C__

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

__Swift__

``` swift
func stopPlayback() {
    if self.isRecording {
        self.camera.stopRecord()
    }
    self.camera.stopPlayback()
}
    
func cameraDidStopPlayback(_ camera: TuyaSmartCameraType!) {
    self.playMode = TuyaSmartCameraPlayModeNone
	self.isPlaybacking = true
}
```

### 回放结束回调

__Objective-C__

```objective-c
// 视频片段回放结束时，会触发此回调。有些设备会自动播放当天的下一个视频片段，直到当天的所有视频片段播放完成后，才会触发这个回调。什么时候触发，取决设备固件的实现。
- (void)cameraPlaybackDidFinished:(id<TuyaSmartCameraType>)camera {
	self.playMode = TuyaSmartCameraPlayModeNone;
	self.isPlaybacking = NO;
}
```

__Swift__

``` swift
// 视频片段回放结束时，会触发此回调。有些设备会自动播放当天的下一个视频片段，直到当天的所有视频片段播放完成后，才会触发这个回调。什么时候触发，取决设备固件的实现。
 func cameraPlaybackDidFinished(_ camera: TuyaSmartCameraType!) {
    self.playMode = TuyaSmartCameraPlayModeNone
	self.isPlaybacking = false   
}
```

### 接收到第一帧视频回调

__Objective-C__

```objective-c
// 预览或者回放开始后，首次接收到视频帧的时候，会触发这个回调。此时表示视频开始正常播放了。
- (void)cameraDidReceiveFirstFrame:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---receive first frame");
}
```

__Swift__

``` swift
// 预览或者回放开始后，首次接收到视频帧的时候，会触发这个回调。此时表示视频开始正常播放了。
func camera(_ camera: TuyaSmartCameraType!, didReceiveFirstFrame image: UIImage!) {
      print("---receive first frame")  
}
```

### 开启视频录制

__Objective-C__

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

__Swift__

``` swift
func startRecord() {
    // 如果没有播放任何视频，或者已经在录制中，不做任何操作
	if self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording {
		return
	}

	// 如果正在对讲，则关闭对讲
	if self.isTalking {
		self.camera.startTalk()
	}

	// 开启录制
	self.camera.startRecord()
}

func cameraDidStartRecord(_ camera: TuyaSmartCameraType!) {
    self.isRecording = true
}
```

### 录制视频到指定文件

默认的视频录制，是保存到手机相册，从 TuyaSmartCameraBase-4.2.5，TuyaSmartCameraM-4.2.5，TuyaSmartCameraKit-4.3.2 开始，提供录制视频到指定文件的接口，此接口没有成功回调，不会失败。P2P 1.0 设备不支持此功能。

__Objective-C__

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
  NSString *videoPath = @"folder/filename.mp4";
	[self.camera startRecordWithFilePath:videoPath];
}
```

__Swift__

```swift
func startRecord() {
    // 如果没有播放任何视频，或者已经在录制中，不做任何操作
	if self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording {
		return
	}

	// 如果正在对讲，则关闭对讲
	if self.isTalking {
		self.camera.startTalk()
	}

	// 开启录制
  let videoPath = "folder/filename.mp4"
	self.camera.startRecord(videoPath)
}
```

### 停止视频录制

__Objective-C__

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

__Swift__

``` swift
func stopRecord() {
    if self.isRecording {
        self.camera.stopRecord()
    }
}

func cameraDidStopRecord(_ camera: TuyaSmartCameraType!) {
    self.isRecording = false
}
```

* 注：如果视频录制成功，会直接保存在手机相册。同时，会在手机相册中创建一个与APP同名的自定义相册，视频将会出现在这个自定义相册中。


### 视频截图

__Objective-C__

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

__Swift__

``` swift
func snapShoot() {
    // 如果没有播放任何视频，不做任何操作
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// 截图操作
	self.camera.snapShoot()
}

func cameraSnapShootSuccess(_ camera: TuyaSmartCameraType!) {
    print("---snap shoot success")
    
}
```

* 注：如果截图成功，图片会直接保存在手机相册。同时，会在手机相册中创建一个与APP同名的自定义相册，截图将会出现在这个自定义相册中。

### 截图保存到指定文件

默认的视频截图是保存到手机系统相册，从 TuyaSmartCameraBase-4.2.5，TuyaSmartCameraM-4.2.5，TuyaSmartCameraKit-4.3.2 开始，提供截图保存到指定文件的接口，同时返回截取到的图片对象，返回 nil 表示截图失败。P2P 1.0 设备不支持此功能。

__Objective-C__

```objective-c
- (void)snapShoot {
	// 如果没有播放任何视频，不做任何操作
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// 截图操作, thumbnilPath 为缩略图保存地址，可以为nil
	NSString *filePath = "folder/filename.png";
	[self.camera snapShootSavedAtPath:filePath thumbnilPath:nil];
}
```

__Swift__

```swift
func snapShoot() {
    // 如果没有播放任何视频，不做任何操作
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// 截图操作
	self.camera.snapShootSaved(_ filePath, thumbnilPath: nil);
}
```


### 设置静音

__Objective-C__

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

__Swift__

``` swift
func enableMute(isMuted: Bool) {
    // 如果当前没有播放视频，则不做任何操作
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// 给当前的播放模式设置静音状态
	// isMuted: YES-关闭视频声音；NO-开启视频声音
	// 视频的声音默认是关闭的
    self.camera.enableMute(isMuted, for: self.playMode)
}

// 静音状态改变回调
func camera(_ camera: TuyaSmartCameraType!, didReceiveMuteState isMute: Bool, playMode: TuyaSmartCameraPlayMode) {
    self.isMuted = isMute
}
```

### 清晰度获取与更改

__Objective-C__

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

__Swift__

``` swift
// 预览开启成功的回调
func cameraDidBeginPreview(_ camera: TuyaSmartCameraType!) {
    // other code ...
	// 发送查询清晰度的命令
	self.camera.getHD()

	// 更改清晰度
	self.camera.enableHD(true)
}

// 清晰度状态回调，查询或设置清晰度的结果，统一由这个代理方法回调
func camera(_ camera: TuyaSmartCameraType!, didReceiveDefinitionState isHd: Bool) {
    // isHd: 是否高清；YES-高清；NO-标清
	self.isHd = isHd
}
```

### 开启对讲

__Objective-C__

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

__Swift__

``` swift
func startTalk() {
    // 如果正在录制，停止录制
    if self.isRecording {
        self.camera.stopRecord()
    }
    // 发送开启对讲的命令
    self.camera.startTalk()
}

// 成功回调
func cameraDidBeginTalk(_ camera: TuyaSmartCameraType!) {
    self.isTalking = true
}
```

### 停止对讲

__Objective-C__

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

__Swift__

``` swift
func stopTalk() {
    // 如果正在对讲，发送停止对讲的命令
	if self.isTalking {
		self.camera.stopTalk()
	}
}

func cameraDidStopTalk(_ camera: TuyaSmartCameraType!) {
    self.isTalking = false
}
```

* 注：对讲和录制是互斥的，而且只有在预览时可以开启对讲。


### 分辨率改变回调

__Objective-C__

```objective-c
// 视频图像分辨率更改的回调
- (void)camera:(id<TuyaSmartCameraType>)camera resolutionDidChangeWidth:(NSInteger)width height:(NSInteger)height {
	NSLog(@"---resolution changed: %ld x %ld", width, height);
}
```

__Swift__

``` swift
// 视频图像分辨率更改的回调
func camera(_ camera: TuyaSmartCameraType!, resolutionDidChangeWidth width: Int, height: Int) {
    print("---resolution changed: %ld x %ld", width, height)
}
```

### 开启裸流。

如果需要自己渲染视频流，可以开启获取裸流属性。开启此属性后，p2p 1.0 会返回未解码的原始数据，p2p 2.0 会返回解码后的 YUV 数据。

__Objective-C__

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

__Swift__

``` swift
func viewDidLoad() {
    self.camera.isRecvFrame = true
}

/**
p2p 1.0 获取未解码原始视频帧数据的代理方法。
@param camera      摄像头对象
@param frameData   原始视频帧数据
@param size        视频帧数据大小
@param frameInfo   视频帧信息，包含编码方式和时间戳
*/  
func camera(_ camera: TuyaSmartCameraType!, ty_didReceiveFrameData frameData: UnsafePointer<Int8>!, dataSize size: UInt32, frameInfo: TuyaSmartVideoStreamInfo)

/**
p2p 2.0 获取解码后的YUV数据的代理方法。
@param camera      		摄像头对象
@param sampleBuffer   	YUV视频帧数据
@param frameInfo   		视频帧信息，包含分辨率，帧率和时间戳
*/
func camera(_ camera: TuyaSmartCameraType!, ty_didReceiveVideoFrame sampleBuffer: CMSampleBuffer!, frameInfo: TuyaSmartVideoFrameInfo)
```

* 注:  开启获取裸流后,原始的渲染视图(videoVIew)就不会渲染图像,需要开发者自行通过该接口的数据进行解析渲染。

### 对讲音频处理

如果你需要处理对讲时，APP 采集的音频数据，可以通过下面的方法。

__Objective-C__

```objective-c
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	// 开始音频接收
  [camer enableAudioProcess:YES];
}
// 实现对讲音频数据接收的代理方法
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didRecieveAudioRecordDataWithPCM:(const unsigned char*)pcm length:(int)length sampleRate:(int)sampleRate {
  // 在这个方法中同步处理 pcm 数据，但不要改变数据的长度
}
```

__Swift__

```swift
func cameraDidConnected(_ camera: TuyaSmartCameraType!) {
	camera.enableAudioProcess(true);
}

func camera(_ camera: TuyaSmartCameraType!, ty_didRecieveAudioRecordData pcm: const unsigned char*, length:int sampleRate: int) {
  // 在这个方法中同步处理 pcm 数据，但不要改变数据的长度
}
```



### 销毁资源

__Objective-C__

```objective-c
// 摄像头面板销毁时，销毁摄像头资源。
- (void)dealloc {
	[self.camera destory];
}
```

__Swift__

``` swift
deinit {
    self.camera.destory()
}
```

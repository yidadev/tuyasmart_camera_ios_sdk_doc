## 云存储

涂鸦平台为智能摄像机提供云存储的服务，可以将设备录制的视频上传到涂鸦云端。

### 云存储服务购买

#### SDK集成

云存储购买需要引入云存储服务购买页面的 SDK，在 podfile 中增加下面代码

```ruby
# 涂鸦平台的开放库
source 'https://github.com/TuyaInc/TYPublicSpecs.git'
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '9.0'
target 'Your_Target' do
  pod 'TYCameraCloudServicePanelSDK'
end
```

然后在工程主目录下执行```pod update```。

#### 初始化

云存储购买服务 SDK 的初始化需要传入涂鸦平台上注册 APP 的渠道标识符。接口如下：

```objc
/**
 初始化SDK，需要在 TuyaSmartSDK 初始化后调用

 @param scheme iot平台上的渠道标识符
 */
+ (void)setupWithAppScheme:(NSString *)scheme;
```

初始化方法需要在 APP 启动时，`TuyaSmartHomeKit` 初始化之后调用。示例代码如下：

__Objective-C__

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    [[TuyaSmartSDK sharedInstance] startWithAppKey:@"your_appKey" secretKey:@"your_appSecret"];
    [TYCameraCloudServicePanelSDK setupWithAppScheme:@"your_scheme"];
}
```

__Swift__

``` swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    TuyaSmartSDK.sharedInstance().start(withAppKey: "your_appKey", secretKey: "your_appSecret")
    TYCameraCloudServicePanelSDK.setup(withAppScheme: "your_scheme")
    return true   
}
```

#### 用户状态同步

在用户登录/登出的时候，需要调用一下接口同步用户登录状态：

```objc
/**
 在用户登录登出的时候，需要调用此方法，同步用户状态
 */
+ (void)userStateChanged;
```

示例代码：

__Objective-C__

```objc
- (void)login {
    [[TuyaSmartUser sharedInstance] loginByPhone:@"countryCode" phoneNumber:@"phoneNumber" password:@"password" success:^{
				[TYCameraCloudServicePanelSDK userStateChanged];
    } failure:^(NSError *error) {
		// failed
    }];
}
```

__Swift__

``` swift
func login() {
        TuyaSmartUser.sharedInstance().login("countryCode", phoneNumber: "phoneNumber", password: "password", success: {
            TYCameraCloudServicePanelSDK.userStateChanged()
        }) { (error) in
            //failed
        }
    }
```

#### 获取云存储购买页面

云存储购买页面是 H5 页面，由于需要从云端请求对应的页面地址，所以获取云存储页面的接口是异步的，并需要传入对应设备的 ```TuyaSmartDeviceModel```对象。接口如下：

```objc
/**
 获取云存储购买页面

 @param deviceModel 设备
 @param success 成功回调
 @param failure 失败回调
 */
+ (void)cloudServicePanelWithDevice:(TuyaSmartDeviceModel *)deviceModel
                            success:(void(^)(UIViewController *vc))success
                            failure:(void(^)(NSError *error))failure;
```

示例代码：

__Objective-C__

```objc
- (void)gotoCloudServicePanelWithDevice:(TuyaSmartDeviceModel *)deviceModel {
    [TYCameraCloudServicePanelSDK cloudServicePanelWithDevice:deviceModel success:^(UIViewController *vc) {
      // 这里返回的 vc 是一个 UINavigationController 的子类
		[self presentViewController:vc animated:YES completion:nil];
	} failure:^(NSError *error) {
		NSLog(@"Error: %@", error);
	}];
}
```

__Swift__

``` swift
TYCameraCloudServicePanelSDK.cloudServicePanel(withDevice: deviceModel, success: { (vc) in
     self.presentViewController(vc, animated: true, completion:nil)
}) { (error) in
            //failed
}
```

### 云视频

购买过云存储服务后，智能摄像机会把录制的视频上传涂鸦云端。云存储相关功能通过 `TuyaSmartCameraKit` 中的 `TuyaSmartCloudManager`类操作。

#### 初始化

`TuyaSmartCloudManager`初始化时，需要传入设备 id 。云存储默认是静音开始播放的，如果需要播放时开启声音，可在初始化时，设置静音状态为 NO。云存储播放时，视频帧数据和帧头信息都将用过代理方法回调。

```objc
/**
	初始化方法
	
	@param devId 设备id
 */
- (instancetype)initWithDeviceId:(NSString *)devId;
```

#### 代理

云存储代理接口为`TuyaSmartCloudManagerDelegate`，只有一个代理方法，会返回每一帧视频的 YUV 数据和帧信息，如果你想要自己渲染视频，可以将 `TuyaSmartCloudManager`的`autoRender`属性设置为“NO”（默认为“YES”），并在代理方法中获取视频帧的YUV数据加以渲染。

```objc
/**
 视频帧回调方法

 @param cloudManager cloudManager
 @param frameBuffer 视频帧YUV数据
 @param frameInfo 视频帧信息
 */
- (void)cloudManager:(TuyaSmartCloudManager *)cloudManager didReceivedFrame:(CMSampleBufferRef)frameBuffer videoFrameInfo:(TuyaSmartVideoFrameInfo)frameInfo;
```

结构体`TuyaSmartVideoFrameInfo`的定义如下：

```objc
typedef struct {
    int nWidth;		// 视频图像宽度
    int nHeight;	// 视频图像高度
    
    int nFrameRate;	// 帧率
    unsigned long long nTimeStamp;	// 时间戳
    unsigned long long nDuration;	// 播放视频消息时，表示视频总长度
    unsigned long long nProgress;   // 播放视频消息时，表示播放进度
} TuyaSmartVideoFrameInfo;
```

#### 加载云存储数据

在使用云存储播放功能前，还需要先加载云存储的相关数据，这个接口会返回云存储服务当前的状态，以及加载对应的加密秘钥，鉴权信息等。

```objc
/**
 加载云存储数据

 @param complete callback
 */
- (void)loadCloudData:(void(^)(TuyaSmartCloudState state))complete
```

#### 云存储服务状态

```objc
typedef NS_ENUM(NSUInteger, TuyaSmartCloudState) {
    TuyaSmartCloudStateNoService,       // 未开通云存储服务
    TuyaSmartCloudStateNoData,          // 已开通云存储服务，但是没有回放视频
    TuyaSmartCloudStateValidData,       // 已开通云存储服务，且有回放视频
    TuyaSmartCloudStateExpiredNoData,   // 云存储服务已过期，且无回放视频
    TuyaSmartCloudStateExpiredData,     // 云存储服务已过期，但是还有可以查看的回放视频
    TuyaSmartCloudStateLoadFailed       // 加载失败
};
```

云存储服务过期后，已上传的云存储视频还会预留一段时间（通常是7天，具体看云存储服务协议），如果在此期间没有续费，到期后，云存储视频将会删除。

#### 云存储录像日期

在加载云存储数据成功返回后，如果云端有视频回放数据，可以通过```cloudDays``` 属性获取有视频回放数据的日期。

```objc
@property (nonatomic, strong, readonly) NSArray<TuyaSmartCloudDayModel *> *cloudDays;
```

`TuyaSmartCloudDayModel`的定义如下：

```objc
@interface TuyaSmartCloudDayModel : NSObject

// 视频总长度，单位秒
@property (nonatomic, assign) NSInteger sumDuration;

// 日期, yyyy-MM-dd
@property (nonatomic, strong) NSString *uploadDay;

// 当天 00:00:00 的 Unix 时间戳
@property (nonatomic, assign) NSInteger startTime;

// 当天 23:59:59 的 Unix 时间戳
@property (nonatomic, assign) NSInteger endTime;

@end
```

#### 时间片段数据

在播放云存储时间前，需要获取到当天的视频片段数据。

```objc
/**
 获取某天的视频片段时间
 
 @param cloudDay 日期
 @param success success callback
 @param failure failure callback
 */
- (void)timeLineWithCloudDay:(TuyaSmartCloudDayModel *)cloudDay
                     success:(void(^)(NSArray<TuyaSmartCloudTimePieceModel *> * timePieces))success
                     failure:(void(^)(NSError * error))failure;
```

`TuyaSmartCloudTimePieceModel`类定义视频片段开始结束时间相关的信息。

```objc
@interface TuyaSmartCloudTimePieceModel : NSObject

// 视频开始时间 Unix 时间戳
@property (nonatomic, assign) NSInteger startTime;

// 视频开始时间 NSDate
@property (nonatomic, strong) NSDate *startDate;

// 视频结束时间 Unix 时间戳
@property (nonatomic, assign) NSInteger endTime;

// 视频结束时间 NSDate
@property (nonatomic, strong) NSDate *endDate;

@end
```

#### 云存储事件

可以通过以下接口获取触发云视频录制的报警事件列表。云存储的报警事件和侦测报警消息略有不同。他们的触发原因可能是一样的，但是侦测报警消息的删除不会影响到云存储事件，也不是所有的侦测报警消息都会触发云视频录制，比如电量警告等。而且云存储事件和云视频相关联，正常情况下，每一个云存储事件都会有一段对应的云视频。

```objc
/**
 获取某天的报警事件
 
 @param 日期
 @param offset 偏移，0 表示从第一个事件开始
 @param limit  数量限制，-1 表示获取所有事件
 @param success success callback
 @param failure failure callback
 */
- (void)timeEventsWithCloudDay:(TuyaSmartCloudDayModel *)cloudDay
                        offset:(int)offset
                         limit:(int)limit
                       success:(void(^)(NSArray<TuyaSmartCloudTimeEventModel *> * timeEvents))success
                       failure:(void(^)(NSError * error))failure;
```

`TuyaSmartCloudTimeEventModel`定义事件相关信息。

```objc
@interface TuyaSmartCloudTimeEventModel : NSObject

// 事件描述
@property (nonatomic, strong) NSString *describe;

// 事件开始时间 Unix 时间戳
@property (nonatomic, assign) NSInteger startTime;

// 事件结束时间 Unix 时间戳
@property (nonatomic, assign) NSInteger endTime;

// 缩略图
@property (nonatomic, strong) NSString *snapshotUrl;

@end
```

#### 云视频播放

播放云存储视频时，需要指定开始播放的时间，结束时间，和是否是播放事件。接口定义：

```objc
/**
 播放云存储视频

 @param startTime 开始播放时间
 @param endTime 结束时间，云存储视频播放会自动连续播放到当天所有视频结束
 @param isEvent 是否是事件
 @param responseCallback errCode 0 表示成功，负数表示失败
 @param finishedCallback 视频播放结束
 */
- (void)playCloudVideoWithStartTime:(long)startTime
                            endTime:(long)endTime
                            isEvent:(BOOL)isEvent
                         onResponse:(void(^)(int errCode))responseCallback
                         onFinished:(void(^)(int errCode))finishedCallback;

/**
 暂停播放
 
 @return errCode 错误码，0 表示成功
 */
- (int)pausePlayCloudVideo;

/**
 恢复播放

 @return errCode 错误码，0 表示成功
 */
- (int)resumePlayCloudVideo;

/**
 停止播放

 @return errCode 错误码，0 表示成功
 */
- (int)stopPlayCloudVideo;

/**
 视频帧渲染视图
 
 @return render view
 */
- (UIView<TuyaSmartVideoViewType> *)videoView;

```

如果是直接播放某个视频片段（`TuyaSmartCloudTimePieceModel`），开始时间传入 `TuyaSmartCloudTimePieceModel` 中介于 `startTime` 和 `endTime` 之间的一个时间戳， isEvent 传入 NO。如果是想要播放某个事件（`TuyaSmartCloudTimeEventModel`），开始时间传入 `TuyaSmartCloudTimeEventModel` 中的`startTime` ，`isEvent` 传入 `YES`，结束时间可以传入当天的结束时间，也就是 `TuyaSmartCloudDayModel` 的 `endTime`。

`TuyaSmartCloudManager`会自动渲染视频，通过`videoView`方法获取视频渲染视图，并添加到屏幕上。

#### 其他功能

云存储视频播放也提供有声音开关，本地视频录制，截图等功能。

```objc
/**
 设置静音状态.
 @param mute 静音
 @param success success call back.
 @param failure failed call back.
 */
- (void)enableMute:(BOOL)mute success:(void(^)(void))success failure:(void (^)(NSError * error))failure;

/**
 获取静音状态
 @return BOOL 是否静音
 */
- (BOOL)isMuted;

/**
 开始录制，视频自动保存到系统相册中
 */
- (void)startRecord;

/**
 开始录制，并且视频将会存到 filepath

 @param filePath 视频保存路径
 */
- (void)startRecordAtPath:(NSString *)filePath;

/**
 停止录制
 
 @return 错误码. 0 表示成功
 */
- (int)stopRecord;

/**
	视频截图，保存到系统相册中，截图成功返回 UIImage 对象，失败返回 nil
 */
- (UIImage *)snapShoot;

/**
 获取视频的截图并存储到指定的目录，如果不需要缩略图，设置 thumbnilPath 为 nil
 截图成功返回 UIImage 对象，失败返回 nil
 */
- (UIImage *)snapShootAtPath:(NSString *)filePath thumbnilPath:(NSString *)thumbnilPath;

```

同样的，如果你只需要获取当前视频截图 `UIImage`对象，不需要自动保存，可以通过 `videoView`的 `- (UIImage *)screenshot;`方法获取截图。

### 示例代码

__Objective-C__

```objc
// self.devId = @"xxxxx";
- (void)viewDidLoad {
	_cloudManager = [[TuyaSmartCloudManager alloc] initWithDeviceId:self.devId];
    [_cloudManager enableMute:NO success:nil failure:nil];
	_cloudManager.delegate = self;
}

- (void)loadData {
	__weak typeof(self) weakSelf = self;
	[self.cloudManager loadCloudData:^(TuyaSmartCloudState state) {
      weak_self.cloudStorageDays = weak_self.cloudManager.cloudDays;
      weak_self.selectedDay = weak_self.cloudManager.cloudDays.lastObject;
      [weakSelf checkCloudState:state];
  }];
}

- (void)requestTimelineData {
    [self.cloudManager timeLineWithCloudDay:self.selectedDay success:^(NSArray<TuyaSmartCloudTimePieceModel *> *timePieces) {
		// success
    } failure:^(NSError *error) {
		// failed
    }];
}

-(void)playVideo:(TuyaSmartCloudTimePieceModel *)timePiece {
    [self.cloudManager playCloudVideoWithStartTime:timePiece.startTime endTime:self.selectedDay.endTime isEvent:NO onResponse:^(int errCode) {
        if (errCode == 0) {
            // success
        }else {
				    // failed
        }
    } onFinished:^(int errCode) {
	  		// finished
        if (errCode != 0) {
				    // some error
        }
    }];
}

- (void)playEvent:(TuyaSmartCloudTimeEventModel *)event {
    [self.cloudManager playCloudVideoWithStartTime:event.startTime endTime:self.selectedDay.endTime isEvent:YES onResponse:^(int errCode) {
        if (errCode == 0) {
            // success
        }else {
    // failed
        }
    } onFinished:^(int errCode) {
  // finished
        if (errCode != 0) {
    // some error
        }
    }];
}

- (void)pause {
    if ([self.cloudManager pausePlayCloudVideo] != 0) {
        // 暂停失败
    }
}

- (void)resume {
    if ([self.cloudManager resumePlayCloudVideo] != 0) {
        // 恢复播放失败
    }
}

- (void)stop {
		[self.cloudManager stopPlayCloudVideo];
}

- (void)muteAction {
    BOOL isMuted = [self.cloudManger isMuted];
    [self.cloudManager enableMute:!isMuted success:^{
		// success
    } failure:^(NSError *error) {
		// failed
    }];
}

- (void)recordAction {
    if (self.isRecording) {
        if ([self.cloudManager stopRecord] != 0) {
            // 录制失败
        }else {
            // 录制成功，视频已保存到系统相册
        }
      	self.isRecording = NO;
    }else {
        [self.cloudManager startRecord];
    		self.isRecording = YES;
    }
}

- (void)snapShoot {
    if ([self.cloudManager snapShoot]) {
        // 图片已保存到系统相册
    }else {
        // 截图失败
    }
}

```

__Swift__

```swift
func viewDidLoad() {
    let cloudManager = TuyaSmartCloudManager(deviceId: devId)
    cloudManager?.enableMute(false, success: nil, failure: nil)
    cloudManager?.delegate = self
}

func loadData() {
    self.cloudManager.loadCloudData { [weak self] (state) in
    		self.cloudStorageDays = self.cloudManager.cloudDays
		    self.selectedDay =  self.cloudManager.cloudDays.last
    		self.checkCloudState(state)            
    }
}

func requestTimelineData() {
		self.cloudManager.timeLine(withCloudDay: self.selectedDay, success: { (timePieces) in
        //success
    }) { (error) in
        //failed
    }
}

func playVideo(_ timePiece: TuyaSmartCloudTimePieceModel) {
    self.cloudManager.playCloudVideo(withStartTime: timePiece.startTime, endTime:self.selectedDay.endTime , isEvent: false, onResponse: { (code) in
        if code == 0 {
            //success
        } else {
            //failed
        }
    }) { (errCode) in
        //finished
        if codeCode != 0 {
            //some error
        }
    }
}
    
func playEvent(_ event: TuyaSmartCloudTimeEventModel) {
    cloudManager.playCloudVideo(withStartTime: event.startTime, endTime: self.selectedDay.endTime, isEvent: true, onResponse: { (errCode) in
        if code == 0 {
            //success
        } else {
            //failed
        }
    }) { (errCode) in
        //finished
        if errCode != 0 {
            // some error
        }
    }
}

func pause() {
    if self.cloudManager.pausePlayCloudVideo() != 0 {
        //暂停失败
    }
}

func resume() {
    if self.cloudManager.resumePlayCloudVideo() != 0 {
        // 恢复播放失败
    }
}

func stop() {
		self.cloudManager.stopPlayCloudVideo()
}

func muteAction() {
    let isMuted = self.cloudManager.isMuted()
    self.cloudManager.enableMute(!isMuted, success: {
				// success
    }) { (error) in
        // failed
    }
}

func recordAction() {
    if self.isRecording {
        if self.cloudManager.stopRecord() != 0 {
            //录制失败
        } else {
            //录制成功，视频已保存系统相册
        }
        self.isRecording = false
    } else {
        self.cloudManager.startRecord()
        self.isRecording = true
    }
}

func snapShoot() {
    if self.cloudManager.snapShoot() {
        // 图片已保存到系统相册
    } else {
        // 截图失败
    }
}
```










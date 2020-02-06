# 云存储

## 云存储购买

### SDK集成

云存储购买通道如果要走涂鸦平台，需要引入云存储购买页面的 SDK，在 podfile 中增加下面代码

```ruby
source 'https://github.com/TuyaInc/TYPublicSpecs.git'
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '9.0'
target 'Your_Target' do
  pod 'TYCameraCloudServicePanelSDK'
end
```

然后在工程主目录下执行```pod update```。

### 初始化

云存储购买服务 SDK 的初始化需要传入涂鸦平台上注册 APP 的渠道标识符。接口如下：

__Objective-C__

```objective-c
/**
 初始化SDK，需要在 TuyaSmartSDK 初始化后调用

 @param scheme iot平台上的渠道标识符
 */
+ (void)setupWithAppScheme:(NSString *)scheme;
```

__Swift__

``` swift
func setup(withAppScheme scheme: String)
```

初始化方法需要在 APP 启动时，TuyaSmartHomeKit 初始化之后调用。示例代码如下：

__Objective-C__

```objective-c
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

### 用户状态同步

在用户登录/登出的时候，需要调用一下接口同步用户登录状态：

__Objective-C__

```objective-c
/**
 在用户登录登出的时候，需要调用此方法，同步用户状态
 */
+ (void)userStateChanged;
```

__Swift__

``` swift
/**
 在用户登录登出的时候，需要调用此方法，同步用户状态
 */
static func userStateChanged()
```

示例代码：

__Objective-C__

```objective-c
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

### 获取云存储购买页面

云存储购买页面是 H5 页面，由于需要从云端请求对应的页面地址，所以获取云存储页面的接口是异步的，并需要传入对应设备的 ```TuyaSmartDeviceModel```对象。接口如下：

```objective-c
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

```objective-c
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

## 云存储功能

云存储相关功能通过 ```TuyaSmartCameraKit中``` 的 ```TuyaSmartCloudManager```类操作。

### 初始化

云存储默认是静音开始播放的，如果需要播放时开启声音，可在初始化时，设置静音状态为 NO。云存储播放时，视频帧数据和帧头信息都将用过代理方法回调。

__Objective-C__

```objective-c
// self.devId = @"xxxxx";
- (void)viewDidLoad {
	_cloudManager = [[TuyaSmartCloudManager alloc] initWithDeviceId:self.devId];
    [_cloudManager enableMute:NO success:nil failure:nil];
	_cloudManager.delegate = self;
}
```

__Swift__

``` swift
func viewDidLoad() {
    let cloudManager = TuyaSmartCloudManager(deviceId: devId)
    cloudManager?.enableMute(false, success: nil, failure: nil)
    cloudManager?.delegate = self
}
```

### 代理

云存储代理接口为```TuyaSmartCloudManagerDelegate```，有一个代理方法，会返回每一帧视频的YUV数据和帧信息，如果你想要自己渲染视频，可以将 ```TuyaSmartCloudManager```的```autoRender```属性设置为“NO”（默认为“YES”），并在代理方法中获取视频帧的YUV数据加以渲染。

__Objective-C__

```objective-c
/**
 视频帧回调方法

 @param cloudManager cloudManager
 @param frameBuffer 视频帧YUV数据
 @param frameInfo 视频帧信息
 */
- (void)cloudManager:(TuyaSmartCloudManager *)cloudManager didReceivedFrame:(CMSampleBufferRef)frameBuffer videoFrameInfo:(TuyaSmartVideoFrameInfo)frameInfo;
```

__Swift__

``` swift
/**
 视频帧回调方法

 @param cloudManager cloudManager
 @param frameBuffer 视频帧YUV数据
 @param frameInfo 视频帧信息
 */
func cloudManager(_ cloudManager: TuyaSmartCloudManager!, didReceivedFrame frameBuffer: CMSampleBuffer!, videoFrameInfo frameInfo: TuyaSmartVideoFrameInfo)
```

结构体```TuyaSmartVideoFrameInfo```的定义如下：

```objective-c
typedef struct {
    int nWidth;		// 视频图像宽度
    int nHeight;	// 视频图像高度
    
    int nFrameRate;	// 帧率
    unsigned long long nTimeStamp;	// 时间戳
    unsigned long long nDuration;	// 与云存储无关，可忽略
    unsigned long long nProgress;   // 与云存储无关，可忽略
} TuyaSmartVideoFrameInfo;
```

### 加载云存储数据

__Objective-C__

```objective-c
/**
加载云存储数据
 @param complete callback
 */
- (void)loadCloudData:(void(^)(TuyaSmartCloudState state))complete;
```

__Swift__

``` swift
/**
加载云存储数据
 @param complete callback
 */
func loadCloudData(complete: ((TuyaSmartCloudState) -> Void)!)
```

在使用云存储播放功能前，还需要先加载云存储的相关数据，这个接口会返回云存储服务当前的状态，以及加载对应的加密秘钥，鉴权信息等。示例代码如下：

__Objective-C__

```objective-c
- (void)loadData {
	__weak typeof(self) weakSelf = self;
	[self.cloudManager loadCloudData:^(TuyaSmartCloudState state) {
        weak_self.cloudStorageDays = weak_self.cloudManager.cloudDays;
        weak_self.selectedDay = weak_self.cloudManager.cloudDays.lastObject;
		[weakSelf checkCloudState:state];
    }];
}
```

__Swift__

``` swift
func loadData() {
    self.cloudManager.loadCloudData { [weak self] (state) in
        self.cloudStorageDays = self.cloudManager.cloudDays
        self.selectedDay =  self.cloudManager.cloudDays.last
        self.checkCloudState(state)            
    }
}
```

### 云存储服务状态

```
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

### 云存储录像日期

在加载云存储数据成功返回后，如果云端有视频回放数据，可以通过```cloudDays``` 属性获取有视频回放数据的日期。

```objective-c
@property (nonatomic, strong, readonly) NSArray<TuyaSmartCloudDayModel *> *cloudDays;
```

```TuyaSmartCloudDayModel```的定义如下：

```objective-c
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

### 时间片段数据

在播放云存储时间前，需要获取到当天的视频片段数据。

```objective-c
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

示例代码：

__Objective-C__

```objective-c
- (void)requestTimelineData {
    [self.cloudManager timeLineWithCloudDay:self.selectedDay success:^(NSArray<TuyaSmartCloudTimePieceModel *> *timePieces) {
		// success
    } failure:^(NSError *error) {
		// failed
    }];
}
```

__Swift__

``` swift
 func requestTimelineData() {
        self.cloudManager.timeLine(withCloudDay: self.selectedDay, success: { (timePieces) in
            //success
        }) { (error) in
            //failed
        }
    }
```

```TuyaSmartCloudTimePieceModel```类定义视频片段开始结束时间相关的信息。

```objective-c
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

### 报警事件

可以通过以下接口获取触发视频录制的报警事件列表。

```objective-c
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

示例代码：

__Objective-C__

```objective-c
- (void)requestEventList {
    [self.cloudManager timeEventsWithCloudDay:self.selectedDay offset:0 limit:-1 success:^(NSArray<TuyaSmartCloudTimeEventModel *> *timeEvents) {
		// success
    } failure:^(NSError *error) {
		// failed
    }];
}
```

__Swift__

``` swift
func requestEventList() {
        self.cloudManager.timeEvents(withCloudDay: self.selectedDay, offset: 0, limit: -1, success: { (timeEvents) in
            //success
        }) { (error) in
            //failed
        }
    }
```

```TuyaSmartCloudTimeEventModel```定义时间相关信息。

```
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

### 云存储视频播放

播放云存储视频时，需要指定开始播放的时间，结束时间，和是否是播放事件。接口定义：

```objective-c
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
```

如果是直接播放某个视频片段（TuyaSmartCloudTimePieceModel），开始时间传入 TuyaSmartCloudTimePieceModel 中介于 startTime 和 endTime 之间的一个时间戳， isEvent 传入 NO。如果是想要播放某个事件（TuyaSmartCloudTimeEventModel），开始时间传入 TuyaSmartCloudTimeEventModel 中的 startTime ，isEvent 传入YES，结束时间可以传入当天的结束时间，也就是 TuyaSmartCloudDayModel 的 endTime,。示例代码如下：

__Objective-C__

```objective-c
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
```

__Swift__

``` swift
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
```

### 暂停

```objective-c
/**
 暂停

 @return errCode 错误码，0 表示成功
 */
- (int)pausePlayCloudVideo;
```

示例代码：

__Objective-C__

```objective-c
- (void)pause {
    if ([self.cloudManager pausePlayCloudVideo] != 0) {
        // 暂停失败
    }
}
```

__Swift__

``` swift
func pause() {
    if self.cloudManager.pausePlayCloudVideo() != 0 {
        //暂停失败
    }
}
```

### 继续播放

```objective-c
/**
 恢复播放

 @return errCode 错误码，0 表示成功
 */
- (int)resumePlayCloudVideo;
```

示例代码：

__Objective-C__

```objective-c
- (void)resume {
    if ([self.cloudManager resumePlayCloudVideo] != 0) {
        // 恢复播放失败
    }
}
```

__Swift__

``` swift
func resume() {
    if self.cloudManager.resumePlayCloudVideo() != 0 {
        // 恢复播放失败
    }
}
```

### 停止播放

```objective-c
/**
 停止播放

 @return errCode 错误码，0 表示成功
 */
- (int)stopPlayCloudVideo;
```

示例代码：

__Objective-C__

```objective-c
- (void)stop {
    if ([self.cloudManager stopPlayCloudVideo] != 0) {
        // 停止播放失败
    }
}
```

__Swift__

``` swift
func stop() {
    if self.cloudManager.stopPlayCloudVideo() != 0 {
        // 恢复播放失败
    }
}
```

### 声音开关

```objective-c
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
```

示例代码：

__Objective-C__

```objective-c
- (void)muteAction {
    BOOL isMuted = [self.cloudManger isMuted];
    [self.cloudManager enableMute:!isMuted success:^{
		// success
    } failure:^(NSError *error) {
		// failed
    }];
}
```

__Swift__

``` swift
func muteAction() {
    let isMuted = self.cloudManager.isMuted()
    self.cloudManager.enableMute(!isMuted, success: {
            // success
    }) { (error) in
            // failed
    }
}
```

### 录制视频

```objective-c
/**
 start record.
 */
- (void)startRecord;

/**
 stop record. video will save to photo album. photo asset collection name is bundle name, "[NSBundle mainBundle].infoDictionary[(__bridge NSString *)kCFBundleNameKey]".
 @return error code. return 0 as record success.
 */
- (int)stopRecord;
```

示例代码：

__Objective-C__

```objective-c
// self.isRecording = NO;

- (void)recordAction {
    if (self.isRecording) {
        if ([self.cloudManager stopRecord] != 0) {
            // 录制失败
        }else {
            // 录制成功，视频已保存到系统相册
        }
    }else {
        [self.cloudManager startRecord];
    	self.isRecording = YES;
    }
}
```

__Swift__

``` swift
func recordAction() {
    if self.isRecording {
        if self.cloudManager.stopRecord() != 0 {
            //录制失败
        } else {
            //录制成功，视频已保存系统相册
        }
    } else {
        self.cloudManager.startRecord()
        self.isRecording = true
    }
}
```

### 截图

```objective-c
/**
 get a screenshot of th video and save it to photo album. photo asset collection name is bundle name, "[NSBundle mainBundle].infoDictionary[(__bridge NSString *)kCFBundleNameKey]".
 */
- (UIImage *)snapShoot;
```

示例代码：

__Objective-C__

```objective-c
- (void)snapShoot {
    if ([self.cloudManager snapShoot]) {
        // 图片已保存到系统相册
    }else {
        // 截图失败
    }
}
```

__Swift__

``` swift
func snapShoot() {
    if self.cloudManager.snapShoot() {
        // 图片已保存到系统相册
    } else {
        // 截图失败
    }
}
```










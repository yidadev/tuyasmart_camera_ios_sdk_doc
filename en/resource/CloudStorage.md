# Cloud Storage

## Services Panel

### SDK Integration

TYCameraCloudServicePanelSDK provides the ability to purchase cloud storage services from Tuya platform. Add the code in your podfile:

```ruby
source 'https://github.com/TuyaInc/TYPublicSpecs.git'
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '9.0'
target 'Your_Target' do
  pod 'TYCameraCloudServicePanelSDK'
end
```

Execute command ```pod update ``` in the project's root directory.

### Initialize SDK

TYCameraCloudServicePanelSDK need the scheme identifier of your app which resign up from Tuya.

__Objective-C__

```objective-c
/**
 initialize SDK，call this after TuyaSmartSDK has initialized.

 @param scheme scheme identifier for tuya iot
 */
+ (void)setupWithAppScheme:(NSString *)scheme;
```

__Swift__

``` swift
func setup(withAppScheme scheme: String)
```

Example:

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

###  User Status Synchronization

When the user logs in/out, you need to call the interface to synchronize the user login status.

__Objective-C__

```objective-c
/**
 When the user logs in/out，synchronize the user login status.
 */
+ (void)userStateChanged;
```

__Swift__

``` swift
/**
 When the user logs in/out，synchronize the user login status.
 */
static func userStateChanged()
```

Example:

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

### Cloud Service Panel

Use this method to get cloud service panel:

```objective-c
/**
 get cloud service view controller for device

 @param deviceModel deviceMdoel
 @param success 
 @param failure 
 */
+ (void)cloudServicePanelWithDevice:(TuyaSmartDeviceModel *)deviceModel
                            success:(void(^)(UIViewController *vc))success
                            failure:(void(^)(NSError *error))failure;
```

Example：

__Objective-C__

```objective-c
- (void)gotoCloudServicePanelWithDevice:(TuyaSmartDeviceModel *)deviceModel {
    [TYCameraCloudServicePanelSDK cloudServicePanelWithDevice:deviceModel success:^(UIViewController *vc) {
      // vc is a subclass of UINavigationController
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

## Cloud Video

Use ```TuyaSmartCloudManager``` in ```TuyaSmartCameraKit```  to play cloud video.

### Initialize

Cloud video default is muted, if you want turn on the sound when video start, you can set mute to NO when initialize cloud manager.  

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

### Delegate

```TuyaSmartCloudManagerDelegate ``` has a method to respond data of video frame. If  your want render the video by yourself, you can set  ```TuyaSmartCloudManager.autoRender``` to NO ( default is YES), and get the YUV data of video frame from this method.

__Objective-C__

```objective-c
/**
call when received video frame

 @param cloudManager cloudManager
 @param frameBuffer YUV data of vdieo frame
 @param frameInfo frame head info
 */
- (void)cloudManager:(TuyaSmartCloudManager *)cloudManager didReceivedFrame:(CMSampleBufferRef)frameBuffer videoFrameInfo:(TuyaSmartVideoFrameInfo)frameInfo;
```

__Swift__

``` swift
/**
 call when received video frame

 @param cloudManager cloudManager
 @param frameBuffer YUV data of vdieo frame
 @param frameInfo frame head info
 */
func cloudManager(_ cloudManager: TuyaSmartCloudManager!, didReceivedFrame frameBuffer: CMSampleBuffer!, videoFrameInfo frameInfo: TuyaSmartVideoFrameInfo)
```

 ```TuyaSmartVideoFrameInfo``` declared like this：

```objective-c
typedef struct {
    int nWidth;		// width of video frame
    int nHeight;	// height of video frame
    
    int nFrameRate;
    unsigned long long nTimeStamp;
    unsigned long long nDuration;
    unsigned long long nProgress;
} TuyaSmartVideoFrameInfo;
```

### Load  Cloud Data

__Objective-C__

```objective-c
/**
load cloud data

 @param complete callback
 */
- (void)loadCloudData:(void(^)(TuyaSmartCloudState state))complete;
```

__Swift__

``` swift
/**
load cloud data
 @param complete callback
 */
func loadCloudData(complete: ((TuyaSmartCloudState) -> Void)!)
```

Before using the cloud storage playback function, you also need to load the relevant data of the cloud storage. This interface will return the current state of the cloud storage service, and load the corresponding encryption key, authentication information, etc.

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

### Cloud Service State

```
typedef NS_ENUM(NSUInteger, TuyaSmartCloudState) {
    TuyaSmartCloudStateNoService,       // Cloud storage service not activated
    TuyaSmartCloudStateNoData,          // activated but no video
    TuyaSmartCloudStateValidData,       // activated and has video
    TuyaSmartCloudStateExpiredNoData,   // service expired and no video
    TuyaSmartCloudStateExpiredData,     // service expired but has video could play
    TuyaSmartCloudStateLoadFailed       // load failed
};
```

After the cloud storage service expires, the uploaded cloud storage video will be reserved for a period of time (usually 7 days, depending on the cloud storage service agreement). If there is no renewal during this period, the cloud storage video will be deleted after expiration.

###  Cloud Storage Recording Date

After the cloud storage data is successfully returned, if there is video playback data in the cloud, the date of the video playback data can be obtained through the ```cloudDays``` attribute.

```objective-c
@property (nonatomic, strong, readonly) NSArray<TuyaSmartCloudDayModel *> *cloudDays;
```

```TuyaSmartCloudDayModel``` declared like this:

```objective-c
@interface TuyaSmartCloudDayModel : NSObject

// cloud video total time seconds
@property (nonatomic, assign) NSInteger sumDuration;

// date, yyyy-MM-dd
@property (nonatomic, strong) NSString *uploadDay;

// Unix timestamp of 00:00:00 at the day
@property (nonatomic, assign) NSInteger startTime;

// Unix timestamp of 23:59:59 at the day
@property (nonatomic, assign) NSInteger endTime;

@end
```

###  Time segment data

Before playing the cloud storage time, you need to get the video clip data of the day.

```objective-c
/**
 get the video clip data of the day
 @param cloudDay date
 @param success success callback
 @param failure failure callback
 */
- (void)timeLineWithCloudDay:(TuyaSmartCloudDayModel *)cloudDay
                     success:(void(^)(NSArray<TuyaSmartCloudTimePieceModel *> * timePieces))success
                     failure:(void(^)(NSError * error))failure;
```

Example：

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

```TuyaSmartCloudTimePieceMode``` class defines information about the start and end time of a video clip.

```objective-c
@interface TuyaSmartCloudTimePieceModel : NSObject

// Unix timestamp of start
@property (nonatomic, assign) NSInteger startTime;

// NSdate of start
@property (nonatomic, strong) NSDate *startDate;

// Unix timestamp of end
@property (nonatomic, assign) NSInteger endTime;

// NSDate of end
@property (nonatomic, strong) NSDate *endDate;

@end
```

### Alarm Event

A list of alarm events that trigger video recording can be obtained through the following interfaces.

```objective-c
/**
 get alarm event list
 @param date
 @param offset 0 indicates that the first event starts
 @param limit  -1 indicates get all
 @param success success callback
 @param failure failure callback
 */
- (void)timeEventsWithCloudDay:(TuyaSmartCloudDayModel *)cloudDay
                        offset:(int)offset
                         limit:(int)limit
                       success:(void(^)(NSArray<TuyaSmartCloudTimeEventModel *> * timeEvents))success
                       failure:(void(^)(NSError * error))failure;
```

Example：

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

```TuyaSmartCloudTimeEventModel``` class defines information about the alarm event.

```objective-c
@interface TuyaSmartCloudTimeEventModel : NSObject

// description of event
@property (nonatomic, strong) NSString *describe;

// Unix timestamp of start
@property (nonatomic, assign) NSInteger startTime;

// Unix timestamp of end
@property (nonatomic, assign) NSInteger endTime;

// Thumbnail
@property (nonatomic, strong) NSString *snapshotUrl;

@end
```

### Cloud Video Play

When playing a cloud storage video, you need to specify the time to start playback, the end time, and whether it is a playback event. Interface definition:

```objective-c
/**
start play cloud video

 @param startTime start play time
 @param endTime the end time, cloud storage video playback will automatically play continuously until the end of all videos on the day.
 @param isEvent whether it is a event
 @param responseCallback errCode 0 is success， negative numbers indicate failure
 @param finishedCallback cloud video play finished
 */
- (void)playCloudVideoWithStartTime:(long)startTime
                            endTime:(long)endTime
                            isEvent:(BOOL)isEvent
                         onResponse:(void(^)(int errCode))responseCallback
                         onFinished:(void(^)(int errCode))finishedCallback;
```

If you are playing a video clip directly (TuyaSmartCloudTimePieceModel), the start time is passed to a timestamp between startTime and endTime in TuyaSmartCloudTimePieceModel, and isEvent is passed in NO. If you want to play an event (TuyaSmartCloudTimeEventModel), the start time is passed to startTime in TuyaSmartCloudTimeEventModel , isEvent is passed YES, and the end time can be passed to the end time of the day, which is the endTime of TuyaSmartCloudDayModel. The sample code is as follows:

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

### Pause

```objective-c
/**
 Pause play

 @return errCode 0 is success
 */
- (int)pausePlayCloudVideo;
```

Example：

__Objective-C__

```objective-c
- (void)pause {
    if ([self.cloudManager pausePlayCloudVideo] != 0) {
        // pause play failed
    }
}
```

__Swift__

``` swift
func pause() {
    if self.cloudManager.pausePlayCloudVideo() != 0 {
        // pause play failed
    }
}
```

### Resume

```objective-c
/**
resume play

 @return errCode 0 is success
 */
- (int)resumePlayCloudVideo;
```

Example：

__Objective-C__

```objective-c
- (void)resume {
    if ([self.cloudManager resumePlayCloudVideo] != 0) {
        // resume play failed
    }
}
```

__Swift__

``` swift
func resume() {
    if self.cloudManager.resumePlayCloudVideo() != 0 {
        // resume play failed
    }
}
```

### Stop

```objective-c
/**
 stop play

 @return errCode 0 is success
 */
- (int)stopPlayCloudVideo;
```

Example：

__Objective-C__

```objective-c
- (void)stop {
    if ([self.cloudManager stopPlayCloudVideo] != 0) {
        // stop play failed
    }
}
```

__Swift__

``` swift
func stop() {
    if self.cloudManager.stopPlayCloudVideo() != 0 {
        // stop play failed
    }
}
```

### Mute

```objective-c
/**
set mute state.
 @param mute mute state
 @param success success call back.
 @param failure failed call back.
 */
- (void)enableMute:(BOOL)mute success:(void(^)(void))success failure:(void (^)(NSError * error))failure;

/**
 get mute state
 @return BOOL is muted
 */
- (BOOL)isMuted;
```

Example：

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

### Record

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

Example：

__Objective-C__

```objective-c
// self.isRecording = NO;

- (void)recordAction {
    if (self.isRecording) {
        if ([self.cloudManager stopRecord] != 0) {
            // record failed
        }else {
            // record video has saved in photo library
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
            // record failed
        } else {
            // record video has saved in photo library
        }
    } else {
        self.cloudManager.startRecord()
        self.isRecording = true
    }
}
```

### SnapShoot

```objective-c
/**
 get a screenshot of th video and save it to photo album. photo asset collection name is bundle name, "[NSBundle mainBundle].infoDictionary[(__bridge NSString *)kCFBundleNameKey]".
 */
- (UIImage *)snapShoot;
```

Example：

__Objective-C__

```objective-c
- (void)snapShoot {
    if ([self.cloudManager snapShoot]) {
        // picture has saved in photo library
    }else {
        // failed
    }
}
```

__Swift__

``` swift
func snapShoot() {
    if self.cloudManager.snapShoot() {
        // picture has saved in photo library
    } else {
        // failed
    }
}
```










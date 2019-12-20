# Camera Function

### Failed call back

The result of all camera's operation is called back through `TuyaSmartCameraDelegate`. if the operation failed, call back will happen through below methord.

__Objective-C__

```objective-c
- (void)camera:(id<TuyaSmartCameraType>)camera didOccurredError:(TYCameraErrorCode)errCode;
```

__Swift__

``` swift
func camera(_ camera: TuyaSmartCameraType!, didOccurredError errCode: TYCameraErrorCode)
```

errcode means certain operation failed, the definition of errcode is below:

```objective-c
typedef enum {
TY_ERROR_NONE,                      // 0
TY_ERROR_CONNECT_FAILED,            // 1    connect failed
TY_ERROR_CONNECT_DISCONNECT,        // 2    connect did disconnected
TY_ERROR_ENTER_PLAYBACK_FAILED,     // 3    connect playback channel failed
TY_ERROR_START_PREVIEW_FAILED,      // 4    preview failed. if the connection is broken during hte preview process, it will alse call back this error.
TY_ERROR_START_PLAYBACK_FAILED,     // 5    playback failed. if the connection is broken during the playback process, it will alse call back this error.
TY_ERROR_PAUSE_PLAYBACK_FAILED,     // 6    pause playabck failed
TY_ERROR_RESUME_PLAYBACK_FAILED,    // 7    resume playabck failed
TY_ERROR_ENABLE_MUTE_FAILED,        // 8    mute failed.
TY_ERROR_START_TALK_FAILED,         // 9    start talk to device failed
TY_ERROR_SNAPSHOOT_FAILED,          // 10   get screenshot failed
TY_ERROR_RECORD_FAILED,             // 11   record video failed
TY_ERROR_ENABLE_HD_FAILED,          // 12   set definition state failed
TY_ERROR_GET_HD_FAILED,             // 13   get definition state failed
TY_ERROR_QUERY_RECORD_DAY_FAILED,   // 14   query video record date failed
TY_ERROR_QUERY_TIMESLICE_FAILED,    // 15   query video record slice failed
} TYCameraErrorCode;
```


### Video render view

* Get the video render view

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

* Video frame zoom in, move and clear

__Objective-C__

```objective-c
// Enlarge the video image by 1.2 rate
[videoView tuya_setScaled:1.2];
// Move the video image to the right by 10.0 pt points
[videoView tuya_setOffset:CGPointMake(10.0, 0.0)];
// Clear the image and reset the zoom factor and offset
[videoView tuya_clear]; 
// the video will scaled to fill the view
videoView.scaleToFill = YES;
```

__Swift__

``` swift
videoView?.tuya_setScaled(1.2)
videoView?.tuya_setOffset(CGPoint(10.0, 0.0))
videoView?.tuya_clear()
videoView?.scaleToFill = true
```

### Connect p2p channel

__Objective-C__

```objective-c
// connect
- (void)viewDidLoad {
	// other code...
	[self.camera connect];
}

// callback for connected success
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	self.isConnected = YES;
}

// callback for connected faild or disconnected
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

### Connect playback channel

P2p connection need to build two channel, the original channel is used for give command and transmit preview audio and video data. if you want to have playback functionm, you should build another playback channel to transmit the audio and video data.

__Objective-C__

```objective-c
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	self.isConnected = YES;
    // connect playback channel
	[self.camera enterPlayback];
}

// callback for playback channel connected success
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

### Play mode

Define playback or preview, when set the status of mute, need to put the parameter inside, to define which mode is mute.

```objective-c
typedef NS_ENUM(NSUInteger, TuyaSmartCameraPlayMode) {
	TuyaSmartCameraPlayModeNone,        // none
	TuyaSmartCameraPlayModePreview,     // live video
	TuyaSmartCameraPlayModePlayback     // playback
};
```
### Start preview

__Objective-C__

```objective-c
- (void)startPreview {
	//  Determine if the video is being recorded, and if you are recording a video, stop recording. (whether the status in the recording is maintained by the developer himself)
	if (self.isRecording) {
		[self.camera stopRecord];
	}
	// Determine if you are previewing, if you are previewing, stop previewing. (whether the state in the preview is maintained by the developer itself)
	if (self.isPlaybacking) {
		[self.camera stopPlayback];
	}
	 // Determine if the p2p channel is connected. If it is not connected or the connection has been disconnected, connect the channel. (The state of whether the p2p channel is connected is maintained by the developer itself)
	if (!self.isConnected) {
		[self.camera connect];
	}
// start preview
		[self.camera startPreview];
	}
	
// callback for start preview success
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
    // current play mode
	self.playMode = TuyaSmartCameraPlayModePreview;
    // status for preview
	self.isPreviewing = YES;
}
```

__Swift__

``` swift
func startPreview() {
    //  Determine if the video is being recorded, and if you are recording a video, stop recording. (whether the status in the recording is maintained by the developer himself)
    if self.isRecording {
        self.camera.stopRecord()
    }
    // Determine if you are previewing, if you are previewing, stop previewing. (whether the state in the preview is maintained by the developer itself)
    if self.isPlaybacking {
        self.camera.stopPlayback()
    }
    // Determine if the p2p channel is connected. If it is not connected or the connection has been disconnected, connect the channel. (The state of whether the p2p channel is connected is maintained by the developer itself)
    if self.isConnected == false {
        self.camera.connect()
    }
    // start preview
    self.camera.startPreview()
}

// callback for start preview success
func cameraDidBeginPreview(_ camera: TuyaSmartCameraType!) {
    // current play mode
    self.playMode = TuyaSmartCameraPlayModePreview
    // status for preview
    self.isPreviewing = true
}
```

* Attention, current camera status, such as:preview, record, playback, p2p channel, disconnected, is maintained by developer, sdk will not store the status information, sdk only responsible for give command and call back.

### Stop preview

__Objective-C__

```objective-c
- (void)stopPreview {
    // if in recording, stop recording
	if (self.isRecording) {
		[self.camera stopRecord];
	}
    // if in talking, stop talking
	if (self.isTalking) {
		[self.camera startTalk];
	}
    // stop preview, this operation will not fail
	[self.camera stopPreview];
	self.isPreviewing = NO;
	self.playMode = TuyaSmartCameraPlayModeNone;
}

// callback for stop preview success
- (void)cameraDidStopPreview:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---stop preview");
}
```

__Swift__

``` swift
func stopPreview() {
    // if in recording, stop recording
    if self.isRecording {
        self.camera.stopRecord()
    }
    // if in talking, stop talking
	if self.isTalking {
		self.camera.startTalk()
	}
    // stop preview, this operation will not fail
	self.camera.stopPreview()
	self.isPreviewing = false
	self.playMode = TuyaSmartCameraPlayModeNone
}

// callback for stop preview success
func cameraDidStopPreview(_ camera: TuyaSmartCameraType!) {
    print("---stop preview")
}
```

### Get the date which have playback record

Before start playback, need to get the record the playback history, firstly get the date in which has the playback record.

__Objective-C__

```objective-c
- (void)queryPlaybackDays {
    // query the date of video recording in a month.
	[self.camera queryRecordDaysWithYear:2018 month:7];
}

// call back for query success
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveRecordDayQueryData:(NSArray<NSNumber *> *)days {
    // days the array of days，ex: [@(1), @(2), @(5), @(6), @(31)]; express in this month, 1，2，5，6，31  has video record.
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
        // days the array of days，ex: [@(1), @(2), @(5), @(6), @(31)]; express in this month, 1，2，5，6，31  has video record.
    self.playbackDays = days
    self.curentDay = days.last?.intValue
}
```

### Get the record of play back record.

Gfter getting effective date of play back record, get the playback record according to the date.

__Objective-C__

```objective-c
- (void)queryPlaybackRecords {
// query all video record slices for a particular day.
	[self.camera queryRecordTimeSliceWithYear:2018 month:7 day:self.curentDay];
}

// callback for query success
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveTimeSliceQueryData:(NSArray<NSDictionary *> *)timeSlices {
    /*
    timeSlices the array of playback video record information. the element is a NSDictionary, content like this:
    kTuyaSmartPlaybackPeriodStartDate  ： startTime(NSDate)
    kTuyaSmartPlaybackPeriodStopDate   ： stopTime(NSDate)
    kTuyaSmartPlaybackPeriodStartTime  ： startTime(NSNumer, unix timestamp)
    kTuyaSmartPlaybackPeriodStopTime   ： stopTime(NSNumer, unix timestamp)
    */

	self.timeSlices = timeSlices;
}
```

__Swift__

``` swift

func queryPlaybackRecords() {
     // query all video record slices for a particular day.
    self.camera.queryRecordTimeSlice(withYear: 2018, month: 7, day: self.currentDay)
}

// callback for query success
func camera(_ camera: TuyaSmartCameraType!, didReceiveTimeSliceQueryData timeSlices: [[AnyHashable : Any]]!) {
       /*
    timeSlices the array of playback video record information. the element is a NSDictionary, content like this:
    kTuyaSmartPlaybackPeriodStartDate  ： startTime(NSDate)
    kTuyaSmartPlaybackPeriodStopDate   ： stopTime(NSDate)
    kTuyaSmartPlaybackPeriodStartTime  ： startTime(NSNumer, unix timestamp)
    kTuyaSmartPlaybackPeriodStopTime   ： stopTime(NSNumer, unix timestamp)
    */
	self.timeSlices = timeSlices
}
```

### Start play back

__Objective-C__

```objective-c
- (void)startPlayBack:(NSDictionary)timeSlice {
	// If in previewing, stop previewing. This includes stopping recording, intercoming, etc. See the stop preview method above for details.
	if (self.isPreviewing) {
		[self stopPreview];
	}
	// timeSlice: Taken from an element in the video clip playback information array above
	NSInteger startTime = [[timeSlice objectForKey:kTuyaSmartTimeSliceStartTime] integerValue];
	NSInteger stopTime = [[timeSlice objectForKey:kTuyaSmartTimeSliceStopTime] integerValue];
	
	// Start playback of the video clip, pass in the timestamp of the start of playback, start timestamp of the video clip, end timestamp
	    // The timestamp to start playing needs to be between the start time and the end time of the video clip, where the default starts from the start time.
	[self.camera startPlayback:startTime startTime:startTime stopTime:stopTime];
}

// callback for start playback success
- (void)cameraDidBeginPlayback:(id<TuyaSmartCameraType>)camera {
	self.playMode = TuyaSmartCameraPlayModePlayback;
	self.isPlaybacking = YES;
}
```

__Swift__

``` swift

func startPlayBack(timeSlice: Dictionary) {
    // If in previewing, stop previewing. This includes stopping recording, intercoming, etc. See the stop preview method above for details.
	if self.isPreviewing {
		self.stopPreview()
	}
    // timeSlice: Taken from an element in the video clip playback information array above
	let startTime = timeSlice[kTuyaSmartTimeSliceStartTime] as? Int
	let stopTime = timeSlice[kTuyaSmartTimeSliceStopTime] as? Int
	    // Start playback of the video clip, pass in the timestamp of the start of playback, start timestamp of the video clip, end timestamp
	    // The timestamp to start playing needs to be between the start time and the end time of the video clip, where the default starts from the start time.
	self.camera.startPlayback(startTime, startTime: startTime, stopTime: stopTime)
}

// callback for start playback success
func cameraDidBeginPlayback(_ camera: TuyaSmartCameraType!) {
    self.playMode = TuyaSmartCameraPlayModePlayback
	self.isPlaybacking = true
}
```

### Pause play back

__Objective-C__

```objective-c
- (void)pausePlayback {
    // if in recording, stop recording
	if (self.isRecording) {
		[self.camera stopRecord];
	}
    // pause playback
	[self.camera pausePlayback];
}

// callback for pause playback
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

### Resume play back

__Objective-C__

```objective-c
- (void)resumePlayback {
// callback for resume playback success
	[self.camera resumePlayback];
}

// callback for resume playback success
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

### Stop play back

__Objective-C__

```objective-c
- (void)stopPlayback {
  // if in recording, stop recording
	if (self.isRecording) {
		[self.camera stopRecord];
}
    // stop playback
	[self.camera stopPlayback];
}

// callback for stop playback success
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


### Playback finish call back

__Objective-C__

```objective-c
// This callback is triggered when the video clip playback ends. Some devices automatically play the next video clip of the day, and the callback will not be triggered until all video clips of the day have finished playing. When to trigger, depends on the implementation of the device firmware.
- (void)cameraPlaybackDidFinished:(id<TuyaSmartCameraType>)camera {
	self.playMode = TuyaSmartCameraPlayModeNone;
	self.isPlaybacking = NO;
}
```

__Swift__

``` swift
// This callback is triggered when the video clip playback ends. Some devices automatically play the next video clip of the day, and the callback will not be triggered until all video clips of the day have finished playing. When to trigger, depends on the implementation of the device firmware.
 func cameraPlaybackDidFinished(_ camera: TuyaSmartCameraType!) {
    self.playMode = TuyaSmartCameraPlayModeNone
	self.isPlaybacking = false   
}
```

### Get the first frame call back

__Objective-C__

```objective-c
// This callback is triggered when the video frame is received for the first time after preview or playback begins. This indicates that the video has started playing normally.
- (void)cameraDidReceiveFirstFrame:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---receive first frame");
}
```

__Swift__

``` swift
// This callback is triggered when the video frame is received for the first time after preview or playback begins. This indicates that the video has started playing normally.
func camera(_ camera: TuyaSmartCameraType!, didReceiveFirstFrame image: UIImage!) {
      print("---receive first frame")  
}
```

### Start video recording

__Objective-C__

```objective-c
- (void)startRecord {
    // If no video is playing, or it is already in recording, do nothing.
    if (self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording) {
	    return;
    }

    // if in talking, stop talking
    if (self.isTalking) {
    	[self.camera startTalk];
    }

    // start recording
    [self.camera startRecord];
}

// callback for start record success
- (void)cameraDidStartRecord:(id<TuyaSmartCameraType>)camera {
	self.isRecording = YES;
}
```

__Swift__

``` swift
func startRecord() {
    // If no video is playing, or it is already in recording, do nothing.
	if self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording {
		return
	}

	// if in talking, stop talking
	if self.isTalking {
		self.camera.startTalk()
	}

	// start recording
	self.camera.startRecord()
}

// callback for start record success
func cameraDidStartRecord(_ camera: TuyaSmartCameraType!) {
    self.isRecording = true
}
```

### Recording to File

Recording video is saved in system photo library by default, we provide this interface to save video to specified file path since TuyaSmartCameraBase-4.2.5，TuyaSmartCameraM-4.2.5，TuyaSmartCameraKit-4.3.2. This method will not failed, so it has no callback function, and P2P 1.0 dose not support.

__Objective-C__

```objective-c
- (void)startRecord {
    // If no video is playing, or it is already in recording, do nothing.
	if (self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording) {
		return;
	}

	// if in talking, stop talking
	if (self.isTalking) {
		[self.camera startTalk];
	}

	// start recording
  NSString *videoPath = @"folder/filename.mp4";
	[self.camera startRecordWithFilePath:videoPath];
}
```

__Swift__

```swift
func startRecord() {
    // If no video is playing, or it is already in recording, do nothing.
	if self.playMode == TuyaSmartCameraPlayModeNone || self.isRecording {
		return
	}

	// if in talking, stop talking
	if self.isTalking {
		self.camera.startTalk()
	}

	// start recording
  let videoPath = "folder/filename.mp4"
	self.camera.startRecord(videoPath)
}
```

### Stop video recording

__Objective-C__

```objective-c
- (void)stopRecord {
	// if in recording, stop recording
	if (self.isRecording) {
		[self.camera stopRecord];
	}
}

// callback for stop record success
- (void)cameraDidStopRecord:(id<TuyaSmartCameraType>)camera {
	self.isRecording = NO;
}
```

__Swift__

``` swift
func stopRecord() {
// if in recording, stop recording
    if self.isRecording {
        self.camera.stopRecord()
    }
}

// callback for stop record success
func cameraDidStopRecord(_ camera: TuyaSmartCameraType!) {
    self.isRecording = false
}
```

* if video recorded successfully, the video will be stored in the phone photo album. the photo album name is same as App name.

### Video screenshot

__Objective-C__

```objective-c
- (void)snapShoot {
	// If no video is played, no action is taken
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// snapshoot
	[self.camera snapShoot];
}

// callback for snapshoot success
- (void)camearaSnapShootSuccess:(id<TuyaSmartCameraType>)camera {
	NSLog(@"---snap shoot success");
}
```

__Swift__

``` swift
func snapShoot() {
    // If no video is played, no action is taken
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// snapshoot
	self.camera.snapShoot()
}

// callback for snapshoot success
func cameraSnapShootSuccess(_ camera: TuyaSmartCameraType!) {
    print("---snap shoot success")
    
}
```

* if screen shot successfully, the picture will be stored in the phone photo album.

### Screenshot in file

The screenshot picture is saved in photo library by default, we provide this interface to save picture to specified file path since TuyaSmartCameraBase-4.2.5，TuyaSmartCameraM-4.2.5，TuyaSmartCameraKit-4.3.2, and return the image object, if return nil, it is failed. P2P 1.0 dose not support.

__Object-C__

```objective-c
- (void)snapShoot {
    // If no video is played, no action is taken
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// snapshoot, thumbnilPath could take nil
	NSString *filePath = "folder/filename.png";
	[self.camera snapShootSavedAtPath:filePath thumbnilPath:nil];
}
```

__Swift__

```swift
func snapShoot() {
  // If no video is played, no action is taken
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// snapshoot, thumbnilPath could take nil
	self.camera.snapShootSaved(_ filePath, thumbnilPath: nil);
}
```

### Mute

__Objective-C__

```objective-c
- (void)enabelMute:(BOOL)isMuted {
	// If no video is played, no action is taken
	if (self.playMode == TuyaSmartCameraPlayModeNone) {
		return;
	}
	// isMuted: YES-close sound of video；NO-open sound of video
	// default is YES
	[self.camera enableMute:isMuted forPlayMode:self.playMode];
}

// callback for mute state did changed
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveMuteState:(BOOL)isMute playMode:(TuyaSmartCameraPlayMode)playMode {
	self.isMuted = isMute;
}
```

__Swift__

``` swift
func enableMute(isMuted: Bool) {
    // If no video is played, no action is taken
	if self.playMode == TuyaSmartCameraPlayModeNone {
		return
	}
	// isMuted: YES-close sound of video；NO-open sound of video
	// default is YES
    self.camera.enableMute(isMuted, for: self.playMode)
}

// callback for mute state did changed
func camera(_ camera: TuyaSmartCameraType!, didReceiveMuteState isMute: Bool, playMode: TuyaSmartCameraPlayMode) {
    self.isMuted = isMute
}
```

### Definition information access and change

__Objective-C__

```objective-c
// start preview did success
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
	// other code ...
	// access definition
	[self.camera getHD];

	// change definition
	// [self.camera enableHD:YES];
}

// callback for definition state did changed
- (void)camera:(id<TuyaSmartCameraType>)camera didReceiveDefinitionStatus:(BOOL)isHd {
	// isHd: is high definiton；YES-HD；NO-SD
	self.isHd = isHd;
}
```

__Swift__

``` swift
// start preview did success
func cameraDidBeginPreview(_ camera: TuyaSmartCameraType!) {
    // other code ...
	// access definition
	self.camera.getHD()

	// change definition
	self.camera.enableHD(true)
}

// callback for definition state did changed
func camera(_ camera: TuyaSmartCameraType!, didReceiveDefinitionState isHd: Bool) {
    // isHd: is high definiton；YES-HD；NO-SD
	self.isHd = isHd
}
```

### Start talking

__Objective-C__

```objective-c
- (void)startTalk {
	// if in recording, stop recording
	if (self.isRecording) {
		[self.camera stopRecord];
	}

	// start talk to device
	[self.camera startTalk];
}

// call back for start talk success
- (void)cameraDidBeginTalk:(id<TuyaSmartCameraType>)camera {
	self.isTalking = YES;
}
```
__Swift__

``` swift
func startTalk() {
	// if in recording, stop recording
    if self.isRecording {
        self.camera.stopRecord()
    }
	// start talk to device
    self.camera.startTalk()
}

// call back for start talk success
func cameraDidBeginTalk(_ camera: TuyaSmartCameraType!) {
    self.isTalking = true
}
```

### Stop talking

__Objective-C__

```objective-c
- (void)stopTalk {
	// if in talking, stop talking
	if (self.isTalking) {
		[self.camera stopTalk];
	}
}

// callback for stop talking success
- (void)cameraDidStopTalk:(id<TuyaSmartCameraType>)camera {
	self.isTalking = NO;
}
```

__Swift__

``` swift
func stopTalk() {
	// if in talking, stop talking
	if self.isTalking {
		self.camera.stopTalk()
	}
}

// callback for stop talking success
func cameraDidStopTalk(_ camera: TuyaSmartCameraType!) {
    self.isTalking = false
}
```

* Talking and recording are mutually exclusive, and talking can be turned on only when previewing.

### Callback for  resolution did changed

__Objective-C__

```objective-c
// call back for video resolution did changed
- (void)camera:(id<TuyaSmartCameraType>)camera resolutionDidChangeWidth:(NSInteger)width height:(NSInteger)height {
	NSLog(@"---resolution changed: %ld x %ld", width, height);
}
```

__Swift__

``` swift
// call back for video resolution did changed
func camera(_ camera: TuyaSmartCameraType!, resolutionDidChangeWidth width: Int, height: Int) {
    print("---resolution changed: %ld x %ld", width, height)
}
```

### Turn on the bare stream.

If you need to get the original video frame data, you can enable  attribute.

__Objective-C__

```objective-c
- (void)viewDidLoad {
	// other init code
	// if camera.isRecvFrame is YES，the video will not be automatically rendered and the video frame data will be given by the proxy method below.
	self.camera.isRecvFrame = YES;
}

/**
if 'isRecvFrame' is true, and p2pType is "1", the video data will not decode in the SDK, and could get the orginal video frame data through this method.
@param camera      camera
@param frameData   original video frame data
@param size        video frame data size
@param frameInfo   frame header info
*/
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didReceiveFrameData:(const char *)frameData dataSize:(unsigned int)size frameInfo:(TuyaSmartVideoStreamInfo)frameInfo;

/**
 if 'isRecvFrame' is true, and p2pType is "2", could get the decoded YUV frame data through this method.
 @param camera          camera
 @param sampleBuffer    video frame YUV data
 @param frameInfo       frame header info
 */
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didReceiveVideoFrame:(CMSampleBufferRef)sampleBuffer frameInfo:(TuyaSmartVideoFrameInfo)frameInfo;
```

__Swift__

``` swift
func viewDidLoad() {
// other init code
	// if camera.isRecvFrame is YES，the video will not be automatically rendered and the video frame data will be given by the proxy method below.
    self.camera.isRecvFrame = true
}

/**
if 'isRecvFrame' is true, and p2pType is "1", the video data will not decode in the SDK, and could get the orginal video frame data through this method.
@param camera      camera
@param frameData   original video frame data
@param size        video frame data size
@param frameInfo   frame header info
*/
func camera(_ camera: TuyaSmartCameraType!, ty_didReceiveFrameData frameData: UnsafePointer<Int8>!, dataSize size: UInt32, frameInfo: TuyaSmartVideoStreamInfo)

/**
 if 'isRecvFrame' is true, and p2pType is "2", could get the decoded YUV frame data through this method.
 @param camera          camera
 @param sampleBuffer    video frame YUV data
 @param frameInfo       frame header info
 */
func camera(_ camera: TuyaSmartCameraType!, ty_didReceiveVideoFrame sampleBuffer: CMSampleBuffer!, frameInfo: TuyaSmartVideoFrameInfo)
```

* After the raw stream is opened, the original rendered view (videoVIew) will not render the image, and the developer needs to parse and render the data through the interface.

### Audio data of talk

You could do secondary processing of the collected audio data when talking.

__Objective-C__

```objective-c
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
	// enable auido data receive
  [camer enableAudioProcess:YES];
}
// implement the delegate method
- (void)camera:(id<TuyaSmartCameraType>)camera ty_didRecieveAudioRecordDataWithPCM:(const unsigned char*)pcm length:(int)length sampleRate:(int)sampleRate {
  // process the pcm data synchronized, but not change the pcm length.
}
```

__Swift__

```swift
func cameraDidConnected(_ camera: TuyaSmartCameraType!) {
	// enable auido data receive
	camera.enableAudioProcess(true);
}
// implement the delegate method
func camera(_ camera: TuyaSmartCameraType!, ty_didRecieveAudioRecordData pcm: const unsigned char*, length:int sampleRate: int) {
  // process the pcm data synchronized, but not change the pcm length.
}
```



### Destory resource

__Objective-C__

```objective-c
// when the view controller dealloc, destory the camera
- (void)dealloc {
	[self.camera destory];
}
```

__Swift__

``` swift
// when the view controller dealloc, destory the camera
deinit {
    self.camera.destory()
}
```

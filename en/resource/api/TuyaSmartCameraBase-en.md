## TYCameraUtil.h
### Class TYCameraUtil : NSObject

#### Method

- \+ dataTransfromBigOrSmall:

  - Return : NSData \*

  - data : NSData \*


- \+ activeAudioSession


- \+ unactiveAudioSession


- \+ localDateWithUnixTimeStamp:

  - Return : NSDate \*

  - timeStamp : NSInteger


- \+ sharedTimeZone

  - Return : NSTimeZone \*


- \+ libCachePath

  - Return : NSString \*


- \+ getAtCachePath:

  - Return : NSString \*

  - fileName : NSString \*


- \+ md5WithString:

  - Return : NSString \*

  - string : NSString \*


- \+ saveImageToPhotoLibrary:

  - Return : BOOL

  - image : UIImage \*


- \+ saveVideoToPhotoLibrary:

  - Return : BOOL

  - videoPath : NSString \*


- \+ recordCacheDirectory

  - Return : NSString \*


- \+ removeAllRecordCacheFiles





## TuyaSmartCameraFactory.h
### Protocol TuyaSmartCameraMaker : NSObject

#### Method

- \+ resetSocket


- \+ handleP2PType:

  - Return : BOOL

  - type : id


- \+ createCameraWithConfig:delegate:

  - Return : id\<TuyaSmartCameraType\>

  - ipcConfig : TuyaSmartCameraConfig \*

  - delegate : id\<TuyaSmartCameraDelegate\>

### Class TuyaSmartCameraConfig : NSObject

#### Property

- devId : NSString \*

  > device id of the camera


- p2pId : NSString \*

  > Get the configuration data through the "tuya.m.ipc.config.get" api, the value of the "p2pId" field in the data.


- password : NSString \*

  > Get the configuration data through the "tuya.m.ipc.config.get" api, the value of the "password" field in the data.


- p2pConfig : NSDictionary \*

  > Get the configuration data through the "tuya.m.ipc.config.get" api, the value of the "p2pConfig" field in the data.if there is no "p2pConfig" field, set nil.


- p2pSpecifiedType : NSInteger

  > Get the configuration data through the "tuya.m.ipc.config.get" api, the value of the "p2pSpecifiedType" field in the data.


- localKey : NSString \*

  > The "localKey" property in the TuyaSmartDeviceModel object corresponding to the camera device.


- uid : NSString \*

  > TuyaSmartUser.uid


- subDic : NSDictionary \*

  > camera station sub device info
NSDictionary \*dic = @{@"did" : stationDid,
@"subDid" : listDevicDid};


- traceId : NSString \*


### Class TuyaSmartCameraFactory : NSObject

#### Method

- ~~\+ initSDKs~~

  > Deprecated : SDK will auto init when create camera instance


- \+ resetSocket


- \+ tuyaSmartCameraVersion

  > get the camera sdk version

  - Return : NSString \*


- \+ cameraWithP2PType:config:delegate:

  > create a camera instance

  - Return : id\<TuyaSmartCameraType\>

    > camera instance

  - type : id

    > p2p type，pass the value of the "p2pType" field in the "skills" attribute of the "TuyaSmartDeviceModel" object corresponding to the camera device. 

  - ipcConfig : TuyaSmartCameraConfig \*

    > camera config 

  - delegate : id\<TuyaSmartCameraDelegate\>

    > delgate 


- \+ ipcConfigWithUid:localKey:configData:

  > create a camera config instance

  - Return : TuyaSmartCameraConfig \*

    > camera config

  - uid : NSString \*

    > User identifier from \[TuyaSmartUser sharedInstance\].uid 

  - localKey : NSString \*

    > from TuyaSmartDeviceModel.localKey 

  - data : NSDictionary \*

    > the respond data of API  "tuya.m.ipc.config.get" 


- \+ cameraMakers

  - Return : NSMutableArray\<Class\<TuyaSmartCameraMaker\>\> \*


- \+ registerCameraMaker:

  - maker : Class\<TuyaSmartCameraMaker\>





## TuyaSmartVideoViewType.h
### Protocol TuyaSmartVideoViewType : NSObject

#### Property

- scaleToFill : BOOL

  > set it YES, video image will stretch to fill the video view.


#### Method

- \- tuya_setScaled:

  > video scale factory

  - scaled : float

    > scale factory 


- \- tuya_setOffset:

  > video offset

  - offset : CGPoint

    > offset point 


- \- tuya_clear

  > clear video content, and reset scale and offset


- \- screenshot

  > get a screenshot from current video frame

  - Return : UIImage \*

    > screenshot image




## TuyaSmartCameraDefines.h
### Enum TUYA_ENUM_CODECID

- TUYA_MEDIA_CODEC_UNKNOWN

- TUYA_MEDIA_CODEC_VIDEO_MPEG4

- TUYA_MEDIA_CODEC_VIDEO_H263

- TUYA_MEDIA_CODEC_VIDEO_H264

- TUYA_MEDIA_CODEC_VIDEO_MJPEG

- TUYA_MEDIA_CODEC_VIDEO_H265

### Enum TYCameraErrorCode

- TY_ERROR_NONE

- TY_ERROR_CONNECT_FAILED

- TY_ERROR_CONNECT_DISCONNECT

- TY_ERROR_ENTER_PLAYBACK_FAILED

- TY_ERROR_START_PREVIEW_FAILED

- TY_ERROR_START_PLAYBACK_FAILED

- TY_ERROR_PAUSE_PLAYBACK_FAILED

- TY_ERROR_RESUME_PLAYBACK_FAILED

- TY_ERROR_ENABLE_MUTE_FAILED

- TY_ERROR_START_TALK_FAILED

- TY_ERROR_SNAPSHOOT_FAILED

- TY_ERROR_RECORD_FAILED

- TY_ERROR_ENABLE_HD_FAILED

- TY_ERROR_GET_HD_FAILED

- TY_ERROR_QUERY_RECORD_DAY_FAILED

- TY_ERROR_QUERY_TIMESLICE_FAILED




## TuyaSmartCameraType.h
### Protocol TuyaSmartCameraDelegate : NSObject

#### Method

- \- cameraDidConnected:

  > the p2p channel did connected.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDisconnected:

  > the p2p channel did disconnected.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidConnectPlaybackChannel:

  > the playback channel did connected.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidBeginPreview:

  > the camera did began play live video.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopPreview:

  > the camera did stop live video.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidBeginPlayback:

  > the camera did began playback record video in the SD card.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidPausePlayback:

  > the camera did pause playback record video in the SD card.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidResumePlayback:

  > the camera did resume playback record video in the SD card.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopPlayback:

  > the camera did stop playback record video in the SD card.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraPlaybackDidFinished:

  > the record video in the SD card playback finished.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- camera:playbackDidFinishedWithStatus:

  > the record video in the SD card playback finished

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - status : NSInteger

    > finished status 


- \- camera:didReceiveFirstFrame:

  > receive first video frame
this method will call when every 'startPreview/startPlayback/resumePlayback' sucess.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - image : UIImage \*

    > fisrt frame image 


- \- cameraDidBeginTalk:

  > begin talk to the device. will call when 'startTalk' success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopTalk:

  > talk to the device did stop. will call when 'stopTalk' success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraSnapShootSuccess:

  > the video screenshot has saved in the photo album.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStartRecord:

  > video recording did start success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopRecord:

  > video recording did stop sucess, and the video has saved in photo album success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- camera:didReceiveDefinitionState:

  > did receive definition state. will call when 'getHD' success or the definition has changed.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - isHd : BOOL

    > is high definition 


- \- camera:didReceiveRecordDayQueryData:

  > called when query date of the playback record success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - days : NSArray\<NSNumber \*\> \*

    > the array of days，ex: \[@(1), @(2), @(5), @(6), @(31)\]; express in this month, 1，2，5，6，31  has video record. 


- \- camera:didReceiveTimeSliceQueryData:

  > called when query video record slice of one day success.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - timeSlices : NSArray\<NSDictionary \*\> \*

    > the array of playback video record information. the element is a NSDictionary, content like this:
kTuyaSmartPlaybackPeriodStartDate  ： startTime(NSDate)
kTuyaSmartPlaybackPeriodStopDate   ： stopTime(NSDate)
kTuyaSmartPlaybackPeriodStartTime  ： startTime(NSNumer, unix timestamp)
kTuyaSmartPlaybackPeriodStopTime   ： stopTime(NSNumer, unix timestamp) 


- \- camera:didReceiveMuteState:playMode:

  > did receive mute state. will call when 'enableMute:' success. default is YES.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - isMute : BOOL

    > is muted 

  - playMode : TuyaSmartCameraPlayMode


- ~~\- camera:didOccurredError:~~

  > Deprecated : Use camera:didOccurredErrorAtStep:specificErrorCode: instead

  > the control of camera has occurred an error


- \- camera:didOccurredErrorAtStep:specificErrorCode:

  > the control of camera has occurred an error with specific reason code

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - errStepCode : TYCameraErrorCode

    > reference the TYCameraErrorCode 

  - errorCode : NSInteger

    > errorCode reference TuyaCameraSDK.framework/TYDefines 


- \- camera:resolutionDidChangeWidth:height:

  > the definition of the video did chagned

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - width : NSInteger

    > video width 

  - height : NSInteger

    > video height 


- \- camera:ty_didReceiveFrameData:dataSize:frameInfo:

  > if 'isRecvFrame' is true, and p2pType is "1", the video data will not decode in the SDK, and could get the orginal video frame data through this method.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - frameData : const char \*

    > original video frame data 

  - size : unsigned int

    > video frame data size 

  - frameInfo : TuyaSmartVideoStreamInfo

    > frame header info 


- \- camera:ty_didReceiveVideoFrame:frameInfo:

  > if 'isRecvFrame' is true, and p2pType is greater than 2, could get the decoded YUV frame data through this method.

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - sampleBuffer : CMSampleBufferRef

    > video frame YUV data 

  - frameInfo : TuyaSmartVideoFrameInfo

    > frame header info 


- \- camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:

  > if p2pType is greater than 2, could get audio record data when talking through this method. if yout want change the audio data, must keep the audio data length same，and synchronize。

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - pcm : const unsigned char\*

    > audio data 

  - length : int

    > date length 

  - sampleRate : int

    > audio sample rate 

### Protocol TuyaSmartCameraType : NSObject

#### Property

- delegate : id\<TuyaSmartCameraDelegate\>


- isRecvFrame : BOOL


- autoRender : BOOL

  > if you want get the original video frame data, set isRecvFrame YES.


#### Method

- \- camera:ty_didReceiveVideoFrame:frameInfo:

  - camera : id\<TuyaSmartCameraType\>

  - sampleBuffer : CMSampleBufferRef

  - frameInfo : TuyaSmartVideoFrameInfo


- \- camera:ty_didReceiveVideoFrame:frameInfo:

  - camera : id\<TuyaSmartCameraType\>

  - sampleBuffer : CMSampleBufferRef

  - frameInfo : TuyaSmartVideoFrameInfo


- \- destory

  > destory resources, should call this method when exit the camera panel.


- \- connect

  > start connect p2p channel


- \- enterPlayback

  > connnect playback channel, call this method before start playback video record.


- \- disConnect

  > disconnect p2p channel


- \- getCurViewWidth

  > get current video width

  - Return : CGFloat

    > video width


- \- getCurViewHeight

  > get current video height

  - Return : CGFloat

    > video height


- \- startPreview

  > start live video


- \- stopPreview

  > stop live video


- \- videoView

  > video frame render view

  - Return : UIView\<TuyaSmartVideoViewType\> \*

    > render view


- \- enableMute:forPlayMode:

  > enable mute state, the default is YES

  - mute : BOOL

    > is mute 

  - playMode : TuyaSmartCameraPlayMode


- \- startTalk

  > start talk to device


- \- stopTalk

  > stop talk to device.


- \- startRecord

  > start record video, the video record will saved in photo album.


- \- stopRecord

  > stop record video.


- \- snapShoot

  > get a screenshot of the video and save it to photo album. photo asset collection name is bundle name, "\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]".

  - Return : UIImage \*


- \- enableHD:

  > enbale high definition

  - hd : BOOL

    > is high definition 


- \- get

  > get the definition state


- \- queryRecordDaysWithYear:month:

  > query the date of video recording in a month.

  - year : NSUInteger

    > ex: 2019 

  - month : NSUInteger

    > ex: 1 


- \- queryRecordTimeSliceWithYear:month:day:

  > query all video record slices for a particular day.

  - year : NSUInteger

    > ex: 2019 

  - month : NSUInteger

    > ex: 1 

  - day : NSUInteger

    > ex: 3 


- \- startPlayback:startTime:stopTime:

  > start playback from a point in time, using unix timestamp

  - playTime : NSInteger

    > play time 

  - startTime : NSInteger

    > start time of a video slice 

  - stopTime : NSInteger

    > end time of a video slice 


- \- pausePlayback

  > pause playback


- \- resumePlayback

  > resume play back


- \- stopPlayback

  > stop play back


- \- camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:enableAudioProcess:

  - enable : BOOL


- \- startRecordWithFilePath:

  > start record video, the video record will saved in filepath

  - filePath : NSString \*


- \- snapShootSavedAtPath:thumbnilPath:

  > get a screenshot of the video and save it to filepah. if you do not need a thumbnil, set thumbnilPath nil.

  - Return : UIImage \*

  - filePath : NSString \*

  - thumbnilPath : NSString \*


- \- getSupportedDefinitions:failure:

  - success : void(^)(NSArray \*definitions)

  - failure : void(^)(NSError \*error)


- \- startPreviewWithDefinition:

  - definition : TuyaSmartCameraDefinition


- \- getDefinition:failure:

  - success : void(^)(TuyaSmartCameraDefinition definitions)

  - failure : void(^)(NSError \*error)


- \- setDefinition:success:failure:

  - definition : TuyaSmartCameraDefinition

  - success : void(^)(void)

  - failure : void(^)(NSError \*error)


- \- setOutLineEnable:

  - enable : BOOL






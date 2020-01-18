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

  > 设备id


- p2pId : NSString \*

  > 请求 "tuya.m.ipc.config.get" api得到的配置数据中的"p2pId" 字段


- password : NSString \*

  > 请求 "tuya.m.ipc.config.get" api得到的配置数据中的"password" 字段


- p2pConfig : NSDictionary \*

  > 请求 "tuya.m.ipc.config.get" api得到的配置数据中的"p2pConfig" 字段, 如果不存在 "p2pConfig" 字段，则置为nil


- p2pSpecifiedType : NSInteger

  > 请求 "tuya.m.ipc.config.get" api得到的配置数据中的"p2pSpecifiedType" 字段


- localKey : NSString \*

  > camera 的 TuyaSmartDeviceModel 对象中的 "localKey" 属性


- uid : NSString \*

  > TuyaSmartUser 的 uid 属性


- subDic : NSDictionary \*

  > camera 基站子设备信息
NSDictionary \*dic = @{@"did" : stationDid,
@"subDid" : listDevicDid};


- traceId : NSString \*


### Class TuyaSmartCameraFactory : NSObject

#### Method

- ~~\+ initSDKs~~

  > Deprecated : SDK will auto init when create camera instance


- \+ resetSocket


- \+ tuyaSmartCameraVersion

  > 获取 camera sdk 版本

  - Return : NSString \*


- \+ cameraWithP2PType:config:delegate:

  > 创建一个camera实例

  - Return : id\<TuyaSmartCameraType\>

    > camera instance

  - type : id

    > p2p 类型，传递camera设备的"TuyaSmartDeviceModel"对象的"skills"属性中的"p2pType"字段 

  - ipcConfig : TuyaSmartCameraConfig \*

    > camera config 

  - delegate : id\<TuyaSmartCameraDelegate\>

    > delgate 


- \+ ipcConfigWithUid:localKey:configData:

  > 创建一个camera配置实例

  - Return : TuyaSmartCameraConfig \*

    > camera config对象

  - uid : NSString \*

    > 使用 \[TuyaSmartUser sharedInstance\].uid 

  - localKey : NSString \*

    > 使用 TuyaSmartDeviceModel.localKey 

  - data : NSDictionary \*

    > 请求"tuya.m.ipc.config.get" API 返回的数据 


- \+ cameraMakers

  - Return : NSMutableArray\<Class\<TuyaSmartCameraMaker\>\> \*


- \+ registerCameraMaker:

  - maker : Class\<TuyaSmartCameraMaker\>





## TuyaSmartVideoViewType.h
### Protocol TuyaSmartVideoViewType : NSObject

#### Property

- scaleToFill : BOOL

  > 设为 YES，视频画面将会拉伸填充整个video view


#### Method

- \- tuya_setScaled:

  > 视频缩放

  - scaled : float

    > 视频缩放 


- \- tuya_setOffset:

  > 视频偏移量

  - offset : CGPoint

    > 视频偏移量 


- \- tuya_clear

  > 清空视频内容，重置 scale 和 offset


- \- screenshot

  > 从当前视频帧获取截图

  - Return : UIImage \*

    > 视频截图




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

  > p2p 通道已连接

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDisconnected:

  > p2p 通道已断开

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidConnectPlaybackChannel:

  > 回放通道已经连接

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidBeginPreview:

  > 摄像头已经开始播放实时视频

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopPreview:

  > 摄像头实时视频已停止

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidBeginPlayback:

  > 摄像头SD卡视频回放已开始

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidPausePlayback:

  > 摄像头SD卡视频回放已暂停

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidResumePlayback:

  > 摄像头SD卡视频回放已恢复播放

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopPlayback:

  > 摄像头SD卡视频回放已中止

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraPlaybackDidFinished:

  > 摄像头SD卡视频回放已结束

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- camera:playbackDidFinishedWithStatus:

  > 摄像头SD卡视频回放结束时状态

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - status : NSInteger

    > finished status 


- \- camera:didReceiveFirstFrame:

  > 

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - image : UIImage \*

    > 第一帧图片 


- \- cameraDidBeginTalk:

  > 开始与设备进行对讲，方法会在 'startTalk' 成时被调用

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopTalk:

  > 与设备对讲已经结束，方法会在 'stopTalk' 成功时被调用

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraSnapShootSuccess:

  > 视频截图已成功保存到相册

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStartRecord:

  > 视频录制已成功开始

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- cameraDidStopRecord:

  > 视频录制已经成功停止，视频已成功保存到相册

  - camera : id\<TuyaSmartCameraType\>

    > camera 


- \- camera:didReceiveDefinitionState:

  > 收到视频清晰度状态，方法会在 'getHD' 成功 或者清晰度改变的时候被调用

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - isHd : BOOL

    > 是否为高清 


- \- camera:didReceiveRecordDayQueryData:

  > 方法会在按日期查询回放视频数据成功后被调用

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - days : NSArray\<NSNumber \*\> \*

    > 日期的数组， ex: \[@(1), @(2), @(5), @(6), @(31)\]; 代表这个月中的 1，2，5，6，31 号有视频录制数据 


- \- camera:didReceiveTimeSliceQueryData:

  > 方法将会在查询一天内视频回放片段数据成功后被调用

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - timeSlices : NSArray\<NSDictionary \*\> \*

    > 回放视频数据信息数组，数组内元素为NSDictionary类型，如下:
kTuyaSmartPlaybackPeriodStartDate  ： startTime(NSDate)
kTuyaSmartPlaybackPeriodStopDate   ： stopTime(NSDate)
kTuyaSmartPlaybackPeriodStartTime  ： startTime(NSNumer, unix timestamp)
kTuyaSmartPlaybackPeriodStopTime   ： stopTime(NSNumer, unix timestamp) 


- \- camera:didReceiveMuteState:playMode:

  > 收到静音状态，方法会在 'enableMute:' 成功之后被调用，默认为 YES

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - isMute : BOOL

    > 是否为静音 

  - playMode : TuyaSmartCameraPlayMode


- ~~\- camera:didOccurredError:~~

  > Deprecated : Use camera:didOccurredErrorAtStep:specificErrorCode: instead

  > camera控制出现了错误


- \- camera:didOccurredErrorAtStep:specificErrorCode:

  > camera 控制出现了一个错误，附带错误码

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - errStepCode : TYCameraErrorCode

    > 具体参考 TYCameraErrorCode 

  - errorCode : NSInteger

    > 具体参考TuyaCameraSDK.framework/TYDefines 


- \- camera:resolutionDidChangeWidth:height:

  > 视频清晰度已经修改

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - width : NSInteger

    > video width 

  - height : NSInteger

    > video height 


- \- camera:ty_didReceiveFrameData:dataSize:frameInfo:

  > 如果 'isRecvFrame' 是true，并且 'p2pType' 是 1， 视频数据将不会在SDK中解码，通过此方法可以获取到原始视频帧数据

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - frameData : const char \*

    > 原始视频帧数据 

  - size : unsigned int

    > 视频帧数尺寸 

  - frameInfo : TuyaSmartVideoStreamInfo

    > 视频帧头信息 


- \- camera:ty_didReceiveVideoFrame:frameInfo:

  > 如果 'isRecvFrame' 为true，并且 'p2pType' 大于 2，可以通过此方法j获得解码后的 YUV 帧数据

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - sampleBuffer : CMSampleBufferRef

    > YUV 视频帧数据 

  - frameInfo : TuyaSmartVideoFrameInfo

    > 数据帧头信息 


- \- camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:

  > p2p2 以上的设备，如果 p2pType 大于 2， 此方法会返回录制的音频数据。如果你需要修改音频数据，务必不要改变音频数据的长度，并在修改操作需要在代理方法中同步进行。

  - camera : id\<TuyaSmartCameraType\>

    > camera 

  - pcm : const unsigned char\*

    > 音频数据 

  - length : int

    > 数据长度 

  - sampleRate : int

    > 音频样本比率 

### Protocol TuyaSmartCameraType : NSObject

#### Property

- delegate : id\<TuyaSmartCameraDelegate\>


- isRecvFrame : BOOL


- autoRender : BOOL

  > 如果需要获取原始视频帧数据，设置 isRecvFrame 为 YES


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

  > 销毁资源，退出摄像头面板的时候应该调用此方法


- \- connect

  > 开始连接 p2p 通道


- \- enterPlayback

  > 连接回放通道，在开始回放视频之前调用此方法


- \- disConnect

  > 断开 p2p 通道


- \- getCurViewWidth

  > 获取当前视频宽度

  - Return : CGFloat

    > video width


- \- getCurViewHeight

  > 获取当前视频高度

  - Return : CGFloat

    > video height


- \- startPreview

  > 开始视频实时预览


- \- stopPreview

  > 停止视频实时预览


- \- videoView

  > 视频帧渲染视图

  - Return : UIView\<TuyaSmartVideoViewType\> \*

    > render view


- \- enableMute:forPlayMode:

  > 开启静音状态，默认为YES

  - mute : BOOL

    > is mute 

  - playMode : TuyaSmartCameraPlayMode


- \- startTalk

  > 开始与设备对讲


- \- stopTalk

  > 停止与设备对讲


- \- startRecord

  > 开始录制视频，视频将会存储在系统相册


- \- stopRecord

  > 停止录制视频


- \- snapShoot

  > 获取视频的截图，图片将存储在系统相册。photo asset collection 名字为 bundle 名。"\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]"

  - Return : UIImage \*


- \- enableHD:

  > 开启高清分辨率

  - hd : BOOL

    > is high definition 


- \- get

  > 获取当前分辨率


- \- queryRecordDaysWithYear:month:

  > 获取当月的存在录制视频数据的日期

  - year : NSUInteger

    > ex: 2019 

  - month : NSUInteger

    > ex: 1 


- \- queryRecordTimeSliceWithYear:month:day:

  > 查询某一天所有的录制视频片段

  - year : NSUInteger

    > ex: 2019 

  - month : NSUInteger

    > ex: 1 

  - day : NSUInteger

    > ex: 3 


- \- startPlayback:startTime:stopTime:

  > 从某个时间开始回放，使用 unix 时间戳

  - playTime : NSInteger

    > 开始播放的时间 

  - startTime : NSInteger

    > 视频片段的开始时间 

  - stopTime : NSInteger

    > 视频片段的结束时间 


- \- pausePlayback

  > 暂停回放


- \- resumePlayback

  > 恢复回放


- \- stopPlayback

  > 停止回放


- \- camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:camera:ty_didRecieveAudioRecordDataWithPCM:length:sampleRate:enableAudioProcess:

  - enable : BOOL


- \- startRecordWithFilePath:

  > 开始录制视频，视频将会存储在指定目录

  - filePath : NSString \*


- \- snapShootSavedAtPath:thumbnilPath:

  > 获取视频截图并存储到指定目录。如果你不需要缩略图，请将thumbnilPath置为nil

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






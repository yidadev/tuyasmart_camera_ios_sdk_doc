## TuyaSmartCloudVideoPlayer.h
### Protocol TuyaSmartCloudVideoPlayerDelegate : NSObject

#### Method

- \- cloudVideoPlayer:didReceivedFrame:videoFrameInfo:

  - player : TuyaSmartCloudVideoPlayer \*

  - frameBuffer : CMSampleBufferRef

  - frameInfo : TuyaSmartVideoFrameInfo


- \- cloudVideoPlayer:didReceivedAudioFrameInfo:

  - player : TuyaSmartCloudVideoPlayer \*

  - frameInfo : TuyaSmartAudioFrameInfo

### Class TuyaSmartCloudVideoPlayer : NSObject

#### Property

- devId : NSString \*


- autoPlay : BOOL


- delegate : id\<TuyaSmartCloudVideoPlayerDelegate\>


#### Method

- \- initWithDeviceId:

  - Return : instancetype

  - devId : NSString \*


- \- configCloudDataTags:

  > 配置云端数据标签

  - jsonCloudDataTags : NSString \*


- \- configCloudDataTagsV2:

  - Return : NSString \*

  - jsonCloudDataTags : NSString \*


- \- playCloudDataWithStartTime:endTime:isEvent:authParams:encryptKey:onResponse:onFinish:

  > 开始播放云视频

  - nStartTime : int

  - nEndTime : int

  - isEvent : BOOL

  - jsonAuthParams : NSString \*

  - encryptKey : NSString \*

  - responseCallback : void (^)(int errCode)

  - finishCallBack : void (^)(int errCode)


- \- pausePlayCloudVideo

  > 暂停播放云视频

  - Return : int

    > 错误码，0代表成功


- \- resumePlayCloudVideo

  > 恢复播放云视频

  - Return : int

    > 错误码，0代表成功


- \- stopPlayCloudVideo

  > 停止播放云视频

  - Return : int

    > 错误码，0代表成功


- \- videoView

  > 视频帧数据渲染视图

  - Return : UIView\<TuyaSmartVideoViewType\> \*

    > render view


- \- enableMute:success:failure:

  > 设置静音

  - mute : BOOL

    > 禁用声音 

  - success : void(^)(void)

    > success call back. 

  - failure : void (^)(NSError \* error)

    > failed call back. 


- \- isMuted

  > 获取设备静音状态

  - Return : BOOL

    > 是否静音BOOL值


- \- startRecord

  > 开始录制。视频将会存储到系统相册，相片 asset collection 名称为bundle name，"\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]"


- \- startRecordAtPath:

  > 开始录制并且视频将会存储到指定 filepath

  - filePath : NSString \*

    > 保存路径 


- \- stopRecord

  > 停止录制

  - Return : int

    > 错误码。返回0代表录制成功


- \- snapShoot

  > 获取视频的截图并且将它存储到系统相册，相片 asset collection 名称为 bundle name，"\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]"

  - Return : UIImage \*


- \- snapShootAtPath:thumbnilPath:

  > 获取视频的截图并保存到指定的路径，如果您不需要缩略图，将thumbnilPath置为nil

  - Return : UIImage \*

  - filePath : NSString \*

  - thumbnilPath : NSString \*


- \- destroy

  > 释放资源


- \- playVideoMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > 在侦测消息中播放加密视频

  - Return : int

    > 错误码

  - url : NSString \*

    > 视频地址 

  - nStartTime : int

    > 开始播放的时间 

  - encryptKey : NSString \*

    > 加密key 

  - callback : void (^)(int errCode)

    > start callback 

  - finihedCallBack : void (^)(int errCode)

    > finish callback 


- \- pausePlayVideoMessage

  > 暂停播放视频

  - Return : int

    > 错误码


- \- resumePlayVideoMessage

  > 恢复播放视频

  - Return : int

    > 错误码


- \- stopPlayVideoMessage

  > 停止播放视频

  - Return : int

    > 错误码


- \- playAudioMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > 在侦测消息中播放加密音频

  - Return : int

    > 错误码

  - url : NSString \*

    > 音频地址 

  - nStartTime : int

    > 开始播放时间 

  - encryptKey : NSString \*

    > 加密key 

  - callback : void (^)(int errCode)

    > start callback 

  - finihedCallBack : void (^)(int errCode)

    > finish callback 


- \- pausePlayAudioMessage

  > 暂停播放s音频

  - Return : int

    > 错误码


- \- resumePlayAudioMessage

  > 恢复播放音频

  - Return : int

    > 错误码


- \- stopPlayAudioMessage

  > 停止播放音频

  - Return : int

    > 错误码







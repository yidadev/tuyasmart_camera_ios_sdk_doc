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

  > config cloud data tags.

  - jsonCloudDataTags : NSString \*


- \- configCloudDataTagsV2:

  - Return : NSString \*

  - jsonCloudDataTags : NSString \*


- \- playCloudDataWithStartTime:endTime:isEvent:authParams:encryptKey:onResponse:onFinish:

  > start play cloud video.

  - nStartTime : int

  - nEndTime : int

  - isEvent : BOOL

  - jsonAuthParams : NSString \*

  - encryptKey : NSString \*

  - responseCallback : void (^)(int errCode)

  - finishCallBack : void (^)(int errCode)


- \- pausePlayCloudVideo

  > pause play cloud video.

  - Return : int

    > error Code, 0 is success.


- \- resumePlayCloudVideo

  > resume play cloud video.

  - Return : int

    > error Code, 0 is success.


- \- stopPlayCloudVideo

  > stop play cloud video.

  - Return : int

    > error Code, 0 is success.


- \- videoView

  > video frame render view

  - Return : UIView\<TuyaSmartVideoViewType\> \*

    > render view


- \- enableMute:success:failure:

  > set mute.

  - mute : BOOL

    > mute sound. 

  - success : void(^)(void)

    > success call back. 

  - failure : void (^)(NSError \* error)

    > failed call back. 


- \- isMuted

  > get mute state.

  - Return : BOOL

    > BOOL


- \- startRecord

  > start record. video will save to photo album. photo asset collection name is bundle name, "\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]".


- \- startRecordAtPath:

  > start record and the video will save to the file path.

  - filePath : NSString \*

    > save path 


- \- stopRecord

  > stop record.

  - Return : int

    > error code. return 0 as record success.


- \- snapShoot

  > get a screenshot of the video and save it to photo album. photo asset collection name is bundle name, "\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]".

  - Return : UIImage \*


- \- snapShootAtPath:thumbnilPath:

  > get a screenshot of the video and save it to file path. if you do not need a thumbnil, set thumbnilPath nil.

  - Return : UIImage \*

  - filePath : NSString \*

  - thumbnilPath : NSString \*


- \- destroy

  > release resources.


- \- playVideoMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > play encrypt video in detect message

  - Return : int

    > error code

  - url : NSString \*

    > video path 

  - nStartTime : int

    > Start time to play 

  - encryptKey : NSString \*

    > encrypt key 

  - callback : void (^)(int errCode)

    > start callback 

  - finihedCallBack : void (^)(int errCode)

    > finish callback 


- \- pausePlayVideoMessage

  > pause play video

  - Return : int

    > error code


- \- resumePlayVideoMessage

  > resume play video

  - Return : int

    > error code


- \- stopPlayVideoMessage

  > stop play video

  - Return : int

    > error code


- \- playAudioMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > play encrypt audio in detect message

  - Return : int

    > error code

  - url : NSString \*

    > audio path 

  - nStartTime : int

    > Start time to play 

  - encryptKey : NSString \*

    > encrypt key 

  - callback : void (^)(int errCode)

    > start callback 

  - finihedCallBack : void (^)(int errCode)

    > finish callback 


- \- pausePlayAudioMessage

  > pause play audio

  - Return : int

    > error code


- \- resumePlayAudioMessage

  > resume play audio

  - Return : int

    > error code


- \- stopPlayAudioMessage

  > stop play audio

  - Return : int

    > error code







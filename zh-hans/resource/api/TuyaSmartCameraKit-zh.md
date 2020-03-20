## TuyaSmartCloudManager.h
### Protocol TuyaSmartCloudManagerDelegate : NSObject

#### Method

- \- cloudManager:didReceivedFrame:videoFrameInfo:

  - cloudManager : TuyaSmartCloudManager \*

  - frameBuffer : CMSampleBufferRef

  - frameInfo : TuyaSmartVideoFrameInfo


- \- cloudManager:didReceivedAudioFrameInfo:

  - player : TuyaSmartCloudManager \*

  - frameInfo : TuyaSmartAudioFrameInfo

### Class TuyaSmartCloudManager : NSObject

#### Property

- devId : NSString \*


- autoRender : BOOL


- cloudDays : NSArray\<TuyaSmartCloudDayModel \*\> \*


- delegate : id\<TuyaSmartCloudManagerDelegate\>


- encryptKey : NSString \*


- authJson : NSString \*


#### Method

- \- initWithDeviceId:

  - Return : instancetype

  - devId : NSString \*


- \- configCloudDataTags:

  > 配置云端数据标签。如果您想要自己调用api代替使用“loadCloudData”请求云端数据，您应该再播放云端视频之前调用此方法。

  - jsonCloudDataTags : NSString \*

    > 云数据标签 


- \- configCloudDataTagsV2:

  - Return : NSString \*

  - jsonCloudDataTags : NSString \*


- \- loadCloudData:

  > 加载云端数据

  - complete : void(^)(TuyaSmartCloudState state)

    > callback 


- \- reLoadCloudDays:

  > 重新加载云端日期数据

  - complete : void(^)(NSArray\<TuyaSmartCloudDayModel \*\> \*cloudDays)

    > callback 


- \- timeEventsWithCloudDay:offset:limit:success:failure:

  > 获取某天的时间片数据

  - cloudDay : TuyaSmartCloudDayModel \*

    > 目标日期 

  - offset : int

    > 事件索引偏移量 

  - limit : int

    > 事件数量限制，\-1代表所有 

  - success : void(^)(NSArray\<TuyaSmartCloudTimeEventModel \*\> \* timeEvents)

    > success callback 

  - failure : void(^)(NSError \* error)

    > failure callback 


- \- timeLineWithCloudDay:success:failure:

  > 获取某天的时间轴数据

  - cloudDay : TuyaSmartCloudDayModel \*

    > 目标日期 

  - success : void(^)(NSArray\<TuyaSmartCloudTimePieceModel \*\> \* timePieces)

    > success callback 

  - failure : void(^)(NSError \* error)

    > failure callback 


- \- playCloudVideoWithStartTime:endTime:isEvent:onResponse:onFinished:

  > 播放云端视频

  - startTime : long

    > 开始时间 

  - endTime : long

    > 结束时间，云视频将会自动继续播放直到当天最后一个视频 

  - isEvent : BOOL

    > YES 是时间片事件，NO是时间轴视频 

  - responseCallback : void(^)(int errCode)

    > errorCode 0 是成功，负数为失败 

  - finishedCallback : void(^)(int errCode)

    > 视频已经结束 


- \- pausePlayCloudVideo

  > 暂停播放

  - Return : int

    > errCode 0 为成功


- \- resumePlayCloudVideo

  > 恢复播放

  - Return : int

    > errCode 0 为成功


- \- stopPlayCloudVideo

  > 停止播放

  - Return : int

    > errCode 0 为成功


- \- videoView

  > 视频帧渲染视图

  - Return : UIView\<TuyaSmartVideoViewType\> \*

    > render view


- \- enableMute:success:failure:

  > 设置静音

  - mute : BOOL

    > 是否静音 

  - success : void(^)(void)

    > success call back. 

  - failure : void (^)(NSError \* error)

    > failed call back. 


- \- isMuted

  > 获取静音状态

  - Return : BOOL

    > BOOL


- \- startRecord

  > 开始录制


- \- startRecordAtPath:

  > 开始录制，并且视频将会存到filepath

  - filePath : NSString \*

    > 保存路径 


- \- stopRecord

  > 停止录制，视频将会存在系统相册。相片asset collection名称为 bundle name，"\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]"

  - Return : int

    > 错误码，返回0代表记录成功


- \- snapShoot

  > 获取视频的截图并存储到系统相册。照片 asset collection 名称为 bundle name，"\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]"

  - Return : UIImage \*


- \- snapShootAtPath:thumbnilPath:

  > 获取视频的截图并存储到指定的目录，如果不需要缩略图，设置 thumbnilPath 为nil

  - Return : UIImage \*

  - filePath : NSString \*

  - thumbnilPath : NSString \*


- \- destroy

  > 销毁 资源


- \- playVideoMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > 在侦测消息追踪播放加密视频

  - Return : int

    > error code

  - url : NSString \*

    > 视频路径 

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

    > error code


- \- resumePlayVideoMessage

  > 恢复播放视频

  - Return : int

    > error code


- \- stopPlayVideoMessage

  > 停止播放视频

  - Return : int

    > error code


- \- playAudioMessageWithUrl:startTime:encryptKey:onResponse:onFinish:

  > 在侦测消息中播放加密音频

  - Return : int

    > error code

  - url : NSString \*

    > 音频路径 

  - nStartTime : int

    > 开始播放时间 

  - encryptKey : NSString \*

    > 加密key 

  - callback : void (^)(int errCode)

    > start callback 

  - finihedCallBack : void (^)(int errCode)

    > finish callback 


- \- pausePlayAudioMessage

  > 暂停播放音频

  - Return : int

    > error code


- \- resumePlayAudioMessage

  > 恢复播放音频

  - Return : int

    > error code


- \- stopPlayAudioMessage

  > 停止播放音频

  - Return : int

    > error code





## TuyaSmartCloudService.h
### Class TuyaSmartCloudService : TuyaSmartRequest

#### Method

- \- requestCloudStorageStateWithParams:success:failure:

  - params : NSDictionary \*

  - success : TYSuccessString

  - failure : TYFailureError


- \- requestCloudStorageCountWithDeviceId:success:failure:

  - devId : NSString \*

  - success : TYSuccessList

  - failure : TYFailureError


- \- requestCloudStorageSecretWithDeviceId:success:failure:

  - devId : NSString \*

  - success : TYSuccessString

  - failure : TYFailureError


- \- requestCloudStoragePrefixsWithParams:success:failure:

  - params : NSDictionary \*

  - success : TYSuccessID

  - failure : TYFailureError


- \- requestCloudStorageTimelineWithParams:success:failure:

  - params : NSDictionary \*

  - success : TYSuccessList

  - failure : TYFailureError


- \- requestCloudStorageEventWithParams:success:failure:

  - params : NSDictionary \*

  - success : TYSuccessList

  - failure : TYFailureError


- \- requestCloudStorageAuthorityWithDeviceId:success:failure:

  - devId : NSString \*

  - success : TYSuccessID

  - failure : TYFailureError


- \- requestCloudStorageAuthorityWithDeviceId:bukets:success:failure:

  - devId : NSString \*

  - bukets : NSString\*

  - success : TYSuccessID

  - failure : TYFailureError





## TuyaSmartCameraDPManager.h
### Protocol TuyaSmartCameraDPObserver : NSObject

#### Method

- \- cameraDPDidUpdate:dps:

  > 设备 dps 数据已经更新

  - manager : TuyaSmartCameraDPManager \*

    > dpManger 

  - dpsData : NSDictionary \*

    > @{dpName: dpValue} 

### Class TuyaSmartCameraDPManager : NSObject

#### Property

- deviceId : NSString \*


- observers : NSArray \*


- device : TuyaSmartDevice \*


#### Method

- ~~\- initWithDevice:~~

  > Deprecated : Use initWithDeviceId: instead


- \- initWithDeviceId:

  - Return : instancetype

  - devId : NSString \*


- \- addObserver:

  - observer : id\<TuyaSmartCameraDPObserver\>


- \- removeObserver:

  - observer : id\<TuyaSmartCameraDPObserver\>


- \- isSupportDP:

  > 当前设备是否支持此dp点

  - Return : BOOL

    > BOOL

  - dpName : TuyaSmartCameraDPKey

    > dpName 


- \- valueForDP:

  > 获取dp的值，如果支持则返回dp值，否则返回nil

  - Return : id

    > dp value

  - dpName : TuyaSmartCameraDPKey

    > dpName 


- \- valueForDP:success:failure:

  > 将会 publish 查询命令到该设备的dp点

  - dpName : TuyaSmartCameraDPKey

    > dpName 

  - success : TYSuccessID

    > success callback 

  - failure : TYFailureError

    > failure callback 


- \- setValue:forDP:success:failure:

  > 设置dp点的值，如果设备支持该dp点，否则返回nil

  - value : id

    > value 

  - dpName : TuyaSmartCameraDPKey

    > dpName 

  - success : TYSuccessID

    > success callback 

  - failure : TYFailureError

    > failure callback 


- ~~\- isSurpportDP:~~

  > Deprecated : Use isSupportDP instead





## TuyaSmartCloudModels.h
### Class TuyaSmartCloudDayModel : NSObject

#### Property

- sumDuration : NSInteger


- uploadDay : NSString \*


- startTime : NSInteger


- endTime : NSInteger


#### Method

- \- initWithDict:

  - Return : instancetype

  - dict : NSDictionary \*


- \+ modelsWithDictArray:

  - Return : NSArray\<TuyaSmartCloudDayModel \*\> \*

  - dictArray : NSArray\<NSDictionary \*\> \*


### Class TuyaSmartCloudTimeEventModel : NSObject

#### Property

- describe : NSString \*


- startTime : NSInteger


- endTime : NSInteger


- snapshotUrl : NSString \*


- isEncypt : BOOL


#### Method

- \- initWithDict:

  - Return : instancetype

  - dict : NSDictionary \*


- \+ modelsWithDictArray:

  - Return : NSArray\<TuyaSmartCloudTimeEventModel \*\> \*

  - dictArray : NSArray\<NSDictionary \*\> \*


### Class TuyaSmartCloudTimePieceModel : NSObject

#### Property

- startTime : NSInteger


- startDate : NSDate \*


- endTime : NSInteger


- endDate : NSDate \*


- playTime : NSInteger


- prefix : id


#### Method

- \- initWithDict:

  - Return : instancetype

  - dict : NSDictionary \*


- \+ modelsWithDictArray:

  - Return : NSArray\<TuyaSmartCloudTimePieceModel \*\> \*

  - dictArray : NSArray\<NSDictionary \*\> \*





## TuyaSmartCameraMessage.h
### Class TuyaSmartCameraMessage : NSObject

#### Property

- timeZone : NSTimeZone \*

  > 默认为 \[NSTimeZone systemTimeZone\]


#### Method

- \- initWithDeviceId:timeZone:

  - Return : instancetype

  - devId : NSString \*

  - timeZone : NSTimeZone \*


- \- messageDaysForYear:month:success:failure:

  > 获取有消息的日期

  - year : NSInteger

    > 年 

  - month : NSInteger

    > 月 

  - success : void (^)(NSArray\<NSString \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 


- \- messagesWithMessageCodes:Offset:limit:startTime:endTime:success:failure:

  > 获取消息列表

  - msgCodes : NSArray \*

    > msgCodes 

  - offset : NSInteger

    > 偏移量 

  - limit : NSInteger

    > 页大小 

  - startTime : NSInteger

    > 传入0代表没有时间限制 

  - endTime : NSInteger

    > 结束时间 

  - success : void (^)(NSArray\<TuyaSmartCameraMessageModel \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 


- \- getMessageSchemes:failure:

  > 获取消息分类列表

  - success : void (^)(NSArray\<TuyaSmartCameraMessageSchemeModel \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 





## TuyaSmartCameraMessageModel.h
### Class TuyaSmartCameraMessageModel : NSObject

#### Property

- dateTime : NSString \*

  > 日期


- msgTypeContent : NSString \*

  > 消息类型描述


- attachPic : NSString \*

  > 图片地址


- attachVideos : NSArray \*

  > 视频地址


- msgSrcId : NSString \*

  > 消息来源


- msgContent : NSString \*

  > 消息内容


- msgTitle : NSString \*

  > 消息标题


- msgId : NSString \*

  > 消息id


- msgCode : NSString \*

  > 消息类型码


- time : NSInteger

  > 日期的时间戳


#### Method

- \- initWithDictionary:

  - Return : instancetype

  - dict : NSDictionary \*


- \+ messageModesWithDataArray:

  - Return : NSArray\<TuyaSmartCameraMessageModel \*\> \*

  - datas : NSArray\<NSDictionary \*\> \*


### Class TuyaSmartCameraMessageSchemeModel : NSObject

#### Property

- describe : NSString \*

  > 类型描述


- msgCodes : NSArray \*

  > 包含的消息类型


#### Method

- \- initWithDictionary:

  - Return : instancetype

  - dict : NSDictionary \*


- \+ messageSchemeModesWithDataArray:

  - Return : NSArray\<TuyaSmartCameraMessageSchemeModel \*\> \*

  - datas : NSArray\<NSDictionary \*\> \*





## TuyaSmartPlaybackDate.h
### Class TuyaSmartPlaybackDate : NSObject

#### Property

- year : NSInteger


- month : NSInteger


- day : NSInteger


#### Method

- \+ playbackDateWithYear:month:day:

  - Return : instancetype

  - year : NSInteger

  - month : NSInteger

  - day : NSInteger


- \+ playbackDateWithDate:

  - Return : instancetype

  - date : NSDate \*


- \+ isToday:

  - Return : BOOL

  - date : TuyaSmartPlaybackDate \*







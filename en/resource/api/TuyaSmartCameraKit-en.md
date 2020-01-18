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

  > config cloud data tags. if you want reqeust cloud data for service api by yourself instead of use "loadCloudData:", you shoud call this method before play cloud video.

  - jsonCloudDataTags : NSString \*

    > cloud data tags 


- \- configCloudDataTagsV2:

  - Return : NSString \*

  - jsonCloudDataTags : NSString \*


- \- loadCloudData:

  > load cloud data

  - complete : void(^)(TuyaSmartCloudState state)

    > callback 


- \- reLoadCloudDays:

  > reload cloud days

  - complete : void(^)(NSArray\<TuyaSmartCloudDayModel \*\> \*cloudDays)

    > callback 


- \- timeEventsWithCloudDay:offset:limit:success:failure:

  > get time event of the day

  - cloudDay : TuyaSmartCloudDayModel \*

    > target day 

  - offset : int

    > event index offset 

  - limit : int

    > event count limit, \-1 is get all 

  - success : void(^)(NSArray\<TuyaSmartCloudTimeEventModel \*\> \* timeEvents)

    > success callback 

  - failure : void(^)(NSError \* error)

    > failure callback 


- \- timeLineWithCloudDay:success:failure:

  > get time line data of the day

  - cloudDay : TuyaSmartCloudDayModel \*

    > target day 

  - success : void(^)(NSArray\<TuyaSmartCloudTimePieceModel \*\> \* timePieces)

    > success callback 

  - failure : void(^)(NSError \* error)

    > failure callback 


- \- playCloudVideoWithStartTime:endTime:isEvent:onResponse:onFinished:

  > play cloud video

  - startTime : long

    > start time 

  - endTime : long

    > end time. cloud video will auto continue util the last video finished in the day 

  - isEvent : BOOL

    > YES is the time event, NO is the time line video. 

  - responseCallback : void(^)(int errCode)

    > errCode 0 is success, negative is failed. 

  - finishedCallback : void(^)(int errCode)

    > video is finished 


- \- pausePlayCloudVideo

  > pause play

  - Return : int

    > errCode 0 is success


- \- resumePlayCloudVideo

  > resume play

  - Return : int

    > errCode 0 is success


- \- stopPlayCloudVideo

  > stop play

  - Return : int

    > errCode 0 is success


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

  > start record.


- \- startRecordAtPath:

  > start record and the video will save to the file path.

  - filePath : NSString \*

    > save path 


- \- stopRecord

  > stop record. video will save to photo album. photo asset collection name is bundle name, "\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]".

  - Return : int

    > error code. return 0 as record success.


- \- snapShoot

  > get a screenshot of th video and save it to photo album. photo asset collection name is bundle name, "\[NSBundle mainBundle\].infoDictionary\[(\_\_bridge NSString \*)kCFBundleNameKey\]".

  - Return : UIImage \*


- \- snapShootAtPath:thumbnilPath:

  > get a screenshot of the video and save it to file path. if you do not need a thumbnil, set thumbnilPath nil.

  - Return : UIImage \*

  - filePath : NSString \*

  - thumbnilPath : NSString \*


- \- destroy

  > destory resource


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

  > device dps data did update

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

  > does the device support this dp

  - Return : BOOL

    > BOOL

  - dpName : TuyaSmartCameraDPKey

    > dpName 


- \- valueForDP:

  > get the value of dp, if support the dp, else return nil.

  - Return : id

    > dp value

  - dpName : TuyaSmartCameraDPKey

    > dpName 


- \- valueForDP:success:failure:

  > will publish query command to device for the dp,

  - dpName : TuyaSmartCameraDPKey

    > dpName 

  - success : TYSuccessID

    > success callback 

  - failure : TYFailureError

    > failure callback 


- \- setValue:forDP:success:failure:

  > set the value of dp, if support the dp, else return nil

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

  > default is \[NSTimeZone systemTimeZone\]


#### Method

- \- initWithDeviceId:timeZone:

  - Return : instancetype

  - devId : NSString \*

  - timeZone : NSTimeZone \*


- \- messageDaysForYear:month:success:failure:

  > get day list which has messages

  - year : NSInteger

    > year 

  - month : NSInteger

    > month 

  - success : void (^)(NSArray\<NSString \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 


- \- messagesWithMessageCodes:Offset:limit:startTime:endTime:success:failure:

  > get message list

  - msgCodes : NSArray \*

    > msgCodes 

  - offset : NSInteger

    > offset 

  - limit : NSInteger

    > page size 

  - startTime : NSInteger

    > pass 0 for no time limit 

  - endTime : NSInteger

    > end time 

  - success : void (^)(NSArray\<TuyaSmartCameraMessageModel \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 


- \- getMessageSchemes:failure:

  > get message schemes list

  - success : void (^)(NSArray\<TuyaSmartCameraMessageSchemeModel \*\> \*result)

    > callback 

  - failure : void (^)(NSError \*error)

    > callback 





## TuyaSmartCameraMessageModel.h
### Class TuyaSmartCameraMessageModel : NSObject

#### Property

- dateTime : NSString \*

  > date string


- msgTypeContent : NSString \*

  > message type description


- attachPic : NSString \*

  > picture path


- attachVideos : NSArray \*

  > video list


- msgSrcId : NSString \*

  > message source id (device id)


- msgContent : NSString \*

  > message content text


- msgTitle : NSString \*

  > message title text


- msgId : NSString \*

  > message id


- msgCode : NSString \*

  > message code


- time : NSInteger

  > date unix stamp


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

  > description text


- msgCodes : NSArray \*

  > message codes


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







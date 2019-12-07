# Change Log

## TuyaSmartCameraBase 4.2.5、TuyaSmartCameraM 4.2.5、TuyaSmartCameraKit 4.3.2、TuyaCamera 3.13.3

* The P2P and audio and video codec libraries are independent TuyaCamera components, and they are changed to dynamic library  to resolve the conflict of different library versions.
* Add interfaces for recording, screenshots, and saving to a specified file path.

## 4.0.2

* Fix occasional recording crash.
* [Debug Log File](https://tuyainc.github.io/tuyasmart_camera_ios_sdk_doc/zh-hans/resource/log_file.html)

## 4.0.1

* Add echo cancellation.
* Add cloud video play ability.

## 3.2.0

* Split SDK to TuyaSmartCameraKit，TuyaSmartCameraBase, TuyaSmartCameraM，TuyaSmartCameraT components. If you not support p2p 1.0, just use ```pod "TuyaSmartCameraKit"```.
* TuyaSmartCameraDPManager add interface ```-(id)valueForDP:(TuyaSmartCameraDPKey)dpName```. It will get the value from app caches, most of dp could get the latest value, because device will report automatically. but TuyaSmartCameraSDCardStorageDPName, TuyaSmartCameraSDCardFormatStateDPName need publish the query command.
* Delete TuyaSmartCameraDefault from sdk, but you can get it form Example in the repo.
* "tuya.m.ipc.config.get" need upgrade to 2.0.
* TuyaSmartCameraFactory add some property, and interface: 

     __Objective-C__
        
    ``` objectivec
    + (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data; 
    ```

    __Swift__
    
    ```swift
    func ipcConfig(withUid: String!, localKey: String!, configData: [AnyHashable : Any]!)
    ```

 uid could pass nill. configData pass the responder of `"tuya.m.ipc.config.get"` with 2.0 version, you can get the config object.
* TuyaSmartVideoViewType add property scaleToFill. set it YES, the video will scaled to fill the view.
* add cloud video feature.

## 3.1.1

* Provide more simple method for camera basic bussiness.
* Camera device DP interface.
* P2P 2.0 provide interface to get the decoded YUV data of video frame.
* Split the SDK for p2p 1.0 and p2p 2.0.

## 3.1.0

* Create different version camera object according to the p2ptype.
* Screen shot and video record saved in custom album which named as app bundle name.
* Support receive the original video frame data.


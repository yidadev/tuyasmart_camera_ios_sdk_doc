# Initiate SDK

## TuyaSmartHomeKit general interface introduction

After integrate with TuyaSmartHomeKit， and initialize the SDK， need to use general interface to call service  api to get camera configurations parameter.

__Objective-C__

```objective-c
- (void)load {
//self.request = [TuyaSmartRequest alloc] init];

/**
*   Request interface of server
*
*  @param apiName  api name of interface
*  @param postData postData parameter value assigned from service
*  @param version  version of interface
*  @param success  callback of successful
*  @param failure  callback of failed

*/
[self.request requestWithApiName:@"api_name" postData:@"post_data" version:@"api_version" success:^(id result) {

} failure:^(NSError *error) {

}];    
}
```

__Swift__

``` swift
func load() {
    self.request = TuyaSmartRequest()
    /**
*   Request interface of server
*
*  @param apiName  api name of interface
*  @param postData postData parameter value assigned from service
*  @param version  version of interface
*  @param success  callback of successful
*  @param failure  callback of failed

*/
    self.request.request(withApiName: "api_name", postData: "post_data", version: "api_version", success: { (result) in
                
    }) { (error) in         
    }
}
```

### Camera configurations parameter interface

* Interface introduction

| Name                  | Version | Description           |
| --------------------- | ------- | --------------------- |
| tuya.m.ipc.config.get | 2.0     | camera configurations |


* Request parameters

| Name  | Type     | Description      |
| ----- | -------- | ---------------- |
| devId | NSString | camera device id |

__Objective-C__

```objective-c
NSDictionary *params = @{
@"devId": @"XXXXXXXX" 
};
```

__Swift__

``` swift
let params = ["devId": "XXXXXXXX"]
```

## Create camera object

* Camera configuration object

  you can use this method to get config object after the request respond.

  __Objective-C__

  ```objective-c
  #define kTuyaSmartIPCConfigAPI @"tuya.m.ipc.config.get"
  #define kTuyaSmartIPCConfigAPIVersion @"2.0"
  __weak typeof(self) weakSelf = self;
  [[TuyaSmartRequest new] requestWithApiName:kTuyaSmartIPCConfigAPI postData:@{@"devId": self.devId} version:kTuyaSmartIPCConfigAPIVersion success:^(id result) {
  	__strong typeof(weakself) self = weakself;
    // SDK initialized may be blocking thread, so it is best to create camera in a sub-thread

  	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    // create config object according to the responder of the reqeust

  		TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:nil localKey:self.device.deviceModel.localKey configData:result];
  		self.camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType config:config delegate:self];
  	});
  } failure:^(NSError *error) {
  	NSLog(@"error: %@", error);
  }];
  ```

    __Swift__
    
    ``` swift
    let kTuyaSmartIPCConfigAPI = "tuya.m.ipc.config.get"
    let kTuyaSmartIPCConfigAPIVersion = "2.0"
    TuyaSmartRequest().request(withApiName: kTuyaSmartIPCConfigAPI, postData: ["devId" : self.devId], version: kTuyaSmartIPCConfigAPIVersion, success: { [weak self] (result) in
        DispatchQueue.global().async {
            let config = TuyaSmartCameraFactory.ipcConfig(withUid: nil, localKey: "your localKey", configData: result)
            self.camera = TuyaSmartCameraFactory.camera(withP2PType: p2pType, config: config, delegate: self)        
        }
    }) { (error) in
    
    }
    ```

* When create camera，add below code to input head files：

__Objective-C__

``` obejctivec
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```

__Swift__

``` swift
import TuyaSmartCameraKit
```

* Add below code to create configurations parameter and create camera object:

__Objective-C__

```objective-c
#define kTuyaSmartIPCConfigAPI @"tuya.m.ipc.config.get"
#define kTuyaSmartIPCConfigAPIVersion @"2.0"

//self.devId = @"your_device_id";
//self.request = [TuyaSmartRequest alloc] init];
//self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];

//p2p type，the SDK creates different versions of the camera object based on the p2p type

id p2pType = [self.device.deviceModel.skills objectForKey:@"p2pType"];

__weak typeof(self) weakSelf = self;
[self.request requestWithApiName:kTuyaSmartIPCConfigAPI postData:@{@"devId":self.devId} version:kTuyaSmartIPCConfigAPIVersion success:^(id result) {
    __strong typeof(weakself) self = weakself;
	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
		TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:nil localKey:self.device.deviceModel.localKey configData:result];
		self.camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType config:config delegate:self];
	});
} failure:failure];
```

__Swift__

``` swift

let kTuyaSmartIPCConfigAPI = "tuya.m.ipc.config.get"
let kTuyaSmartIPCConfigAPIVersion = "2.0"
let p2pType = self.device.deviceModel.skills["p2pType"]
self.requeset.request(withApiName: kTuyaSmartIPCConfigAPI, postData: ["devId" : self.devId], version: kTuyaSmartIPCConfigAPIVersion, success: { [weak self] (result) in
        DispatchQueue.global().async {
        let config = TuyaSmartCameraFactory.ipcConfig(withUid: nil, localKey: self.device.deviceModel.localKey , configData: result)
        self.camera = TuyaSmartCameraFactory.camera(withP2PType: p2pType, config: config, delegate: self)
            }
}) { (error) in
}
```

until now, we build the camera successfully , then we can begin to use the function of the camera

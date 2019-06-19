# Initiate SDK

## TuyaSmartHomeKit general interface introduction

After integrate with TuyaSmartHomeKit， and initialize the SDK， need to use general interface to call service  api to get camera configurations parameter.

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

### Camera configurations parameter interface

* Interface introduction

| Name                  | Version | Description           |
| --------------------- | ------- | --------------------- |
| tuya.m.ipc.config.get | 2.0     | camera configurations |


* Request parameters

| Name  | Type     | Description      |
| ----- | -------- | ---------------- |
| devId | NSString | camera device id |

```objective-c
NSDictionary *params = @{
@"devId": @"XXXXXXXX" 
};
```

## Create camera object

* Camera configuration object

  you can use this method to get config object after the request respond.

  ```
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

  ​


* When create camera，add below code to input head files：

```
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```


* Add below code to create configurations parameter and create camera object:

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

until now, we build the camera successfully , then we can begin to use the function of the camera

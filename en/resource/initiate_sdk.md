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
| tuya.m.ipc.config.get | 1.0     | camera configurations |


* Request parameters

| Name  | Type     | Description      |
| ----- | -------- | ---------------- |
| devId | NSString | camera device id |

```objective-c
NSDictionary *params = @{
@"devId": @"XXXXXXXX" 
};
```

* Response parameter

| Name              | Type         | Description                         |
| ----------------- | ------------ | ----------------------------------- |
| result            | NSDictionary | result data                         |
| result.id         | NSString     | device id                           |
| result.p2pConfig  | NSDictionary | expand configurations               |
| result.p2pId      | NSString     | p2p connect id                      |
| result.password   | NSString     | p2p connect password                |
| result.timeZoneId | NSString     | device time zone                    |
| status            | NSString     | status（success：ok； fail：error） |
| success           | BOOL         | whether success                     |

```json
{
result =     {
password = "xxxxxx",
id = "xxxxxxxxxxxxxxxxxx",
p2pConfig =     {
},
p2pId = "XXXXXXXXXXXXXXXXXXX",
timeZoneId = "Asia/Shanghai",
},
success = 1,
status = "ok",
}
```

## Create camera object

* Camera configuration parameter property introduction

| Name      | Type         | Description                       |
| --------- | ------------ | --------------------------------- |
| p2pId     | NSString     | p2p connect id                    |
| password  | NSString     | p2p connect password              |
| localKey  | NSString     | LAN password                      |
| p2pConfig | NSDictionary | camera p2p channel configurations |

* When create camera，add below code to input head files：

```
#import <TuyaSmartCamera/TuyaSmartCameraFactory.h>
```


* Add below code to create configurations parameter and create camera object:

```objective-c
//self.devId = @"your_device_id";
//self.request = [TuyaSmartRequest alloc] init];
//self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];

//p2p type，the SDK creates different versions of the camera object based on the p2p type
id p2pType = [self.device.deviceModel.skills objectForKey:@"p2pType"];

__weak typeof(self) weakSelf = self;
[self.request requestWithApiName:@"tuya.m.ipc.config.get"   
postData:@{@"devId":self.devId} 
version:@"1.0" 
success:^(id result) {
TuyaSmartCameraConfig *config = [TuyaSmartCameraConfig new];
config.p2pId = [result objectForKey:@"p2pId"];
config.password = [result objectForKey:@"password"];
config.localKey = self.device.deviceModel.localKey;
config.p2pConfig = [result objectForKey:@"p2pConfig"];
weakSelf.camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType 
config:config
delegate:self];
} failure:failure];
```

until now, we build the camera successfully , then we can begin to use the function of the camera

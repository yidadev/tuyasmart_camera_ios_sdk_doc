# 初始化

## TuyaSmartHomeKit 通用接口介绍

在接入TuyaSmartHomeKit，并初始化其SDK后，需要使用其通用接口调用服务端的API接口来获取摄像头的配置参数。

__Objective-C__

```objective-c
- (void)load {
//self.request = [TuyaSmartRequest alloc] init];

/**
*  请求服务端接口
*
*  @param apiName    接口名称
*  @param postData 业务入参
*  @param version  接口版本号
*  @param success  操作成功回调
*  @param failure  操作失败回调
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
    *  请求服务端接口
    *
    *  @param apiName    接口名称
    *  @param postData 业务入参
    *  @param version  接口版本号
    *  @param success  操作成功回调
    *  @param failure  操作失败回调
    */
    self.request.request(withApiName: "api_name", postData: "post_data", version: "api_version", success: { (result) in
                
    }) { (error) in         
    }
}
```


### 摄像头配置参数接口

* 接口说明

| 名称                  | 版本 | 描述       |
| --------------------- | ---- | ---------- |
| tuya.m.ipc.config.get | 2.0  | 摄像头配置 |


* 请求参数

| 名称  | 类型   | 描述               |
| ----- | ------ | ------------------ |
| devId | 字符串 | 摄像头对应的设备id |

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

## 创建摄像头对象

* 摄像头配置对象

  在最 3.2.0 版本中，不再需要关注 TuyaSmartCameraConfig 类的属性。在上面的接口调用成功后，可通过下面的方法来获取配置对象。

  ```objective-c
  // TuyaSmartCameraFactory.h

  /**
   create TuyaSmartCameraConfig instance according to api request responder and user uid, device localKey
   
   @param uid [TuyaSmartUser sharedInstance].uid
   @param localKey TuyaSmartDeviceModel.localKey
   @param data api request responder of "tuya.m.ipc.config.get"
   @return TuyaSmartCameraConfig instance
   */
  + (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data;
  ```

  ​


* 在创建摄像头的文件中，添加以下代码导入头文件：

__Objective-C__

``` obejctivec
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```

__Swift__

``` swift
import TuyaSmartCameraKit
```

* 添加以下代码创建配置参数对象并创建摄像头对象：

__Objective-C__

```objective-c
#define kTuyaSmartIPCConfigAPI @"tuya.m.ipc.config.get"
#define kTuyaSmartIPCConfigAPIVersion @"2.0"

//self.devId = @"your_device_id";
//self.request = [TuyaSmartRequest alloc] init];
//self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];

//p2p通道类型，SDK会根据p2pType创建不同版本的摄像头对象。
id p2pType = [self.device.deviceModel.skills objectForKey:@"p2pType"];

__weak typeof(self) weakSelf = self;
[self.request requestWithApiName:kTuyaSmartIPCConfigAPI postData:@{@"devId":self.devId} version:kTuyaSmartIPCConfigAPIVersion success:^(id result) {
    __strong typeof(weakself) self = weakself;
	dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        // 根据接口返回的数据创建Config对象
		TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:[TuyaSmartUser sharedInstance].uid localKey:self.device.deviceModel.localKey configData:result];
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
                // 根据接口返回的数据创建Config对象
        let config = TuyaSmartCameraFactory.ipcConfig(withUid: TuyaSmartUser.sharedInstance()?.uid, localKey: self.device.deviceModel.localKey , configData: result)
        self.camera = TuyaSmartCameraFactory.camera(withP2PType: p2pType, config: config, delegate: self)
            }
}) { (error) in
}
```

至此，已经成功创建出了摄像头对象，接下来可以开始使用摄像头的功能了。

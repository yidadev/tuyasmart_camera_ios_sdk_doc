## 家庭与设备管理

### 家庭管理

用户登录成功后需要通过`TuyaSmartHomeManager`去获取整个家庭列表的信息,然后初始化其中的一个家庭`TuyaSmartHome`，获取家庭详情信息，对家庭中的设备进行管理，控制。参考文档[家庭管理]([https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Home.html#%E5%AE%B6%E5%BA%AD%E7%AE%A1%E7%90%86](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Home.html#家庭管理))。

### 设备管理

设备管理相关的所有功能对应`TuyaSmartDevice`类，需要使用设备Id进行初始化。错误的设备Id可能会导致初始化失败，返回`nil`。参考文档[设备管理]([https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Device.html#%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Device.html#设备管理))。

```
请始终通过家庭来获取设备列表，并从家庭的设备列表里获取设备 id 来初始化 TuyaSmartDevice 类，以避免未同步家庭数据，TuyaSmartDevice 初始化失败的情况。
```

### 初始化摄像机

在获取到设备列表后，就可以根据设备的类型来判断是否是智能摄像机设备，如果是智能摄像机设备，则可以根据`TuyaSmartDeviceModel`中的信息来创建摄像机对象。

#### 判断是否是智能摄像机

可以根据`TuyaSmartDeviceModel`的`category`属性来判断设备的类型，智能摄像机的类型是`sp`。

__Objective-C__

```objective-c
[[TuyaSmartHomeManager new] getHomeListWithSuccess:^(NSArray<TuyaSmartHomeModel *> *homes) {
		[homes enumerateObjectsUsingBlock:^(TuyaSmartHomeModel * _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
				TuyaSmartHome *home = [TuyaSmartHome homeWithHomeId:obj.homeId];
				[home getHomeDetailWithSuccess:^(TuyaSmartHomeModel *homeModel) {
						[home.deviceList enumerateObjectsUsingBlock:^(TuyaSmartDeviceModel * _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            		if ([obj.category isEqualToString:@"sp"]) {
                		NSLog(@"%@ 是一个智能摄像机设备", obj.name);
            		}
        }];
        } failure:^(NSError *error) {

        }];
    }];
} failure:^(NSError *error) {

}];
```

__Swift__

```swift
let homeManager = TuyaSmartHomeManager()
homeManager.getHomeList(success: { homeList in
    homeList?.forEach({ homeModel in
        let home = TuyaSmartHome(homeId: homeModel.homeId)
        home?.getDetailWithSuccess({ _ in
            home?.deviceList.forEach({ deviceModel in
                if deviceModel.category == "sp" {
                    print(deviceModel.name!, "是一个智能摄像机设备")
                }
            })
        }, failure: { error in

        })
    })
}) { error in

}
```

```
注意，这里的示例代码只是展示筛选出摄像头设备的最简单流程，实际开发中，应该根据 UI 交互的逻辑来展示和管理设备。
```

#### 摄像头配置信息

筛选出智能摄像机设备后，就可以根据设备ID，即`TuyaSmartDeviceModel`的`devId`属性来获取摄像机的配置信息。

配置信息需要通过涂鸦云端 API 来获取，接口信息如下：

| 接口名                | 版本 | 描述                        |
| --------------------- | ---- | --------------------------- |
| tuya.m.ipc.config.get | 2.0  | 获取摄像机设备的P2P配置信息 |

接口参数：

| 参数名 | 类型   | 说明   | 是否必须 |
| ------ | ------ | ------ | -------- |
| devId  | String | 设备id | 是       |

需要使用`TuyaSmartRequest`类来调用涂鸦云端的 API，参考文档[通用接口](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/CommonInterface.html)。

根据接口返回的数据，使用`TuyaSmartCameraFactory`工具类创建`TuyaSmartCameraConfig`对象，以作为创建摄像机对象的参数。`TuyaSmartCameraFactory`相关接口如下：

```objective-c
/**
 创建一个camera配置实例

 @param uid 使用 [TuyaSmartUser sharedInstance].uid
 @param localKey 使用 TuyaSmartDeviceModel.localKey
 @param data 请求"tuya.m.ipc.config.get" API 返回的数据
 @return TuyaSmartCameraConfig 对象
*/
+ (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data;

/**
 创建一个camera实例

 @param type p2p 类型，"TuyaSmartDeviceModel" 对象的 "skills" 属性中的 "p2pType" 字段
 
 @param ipcConfig "TuyaSmartCameraConfig" 对象
 @param delegate delgate
 @return Camera 对象
 */
+ (id<TuyaSmartCameraType>)cameraWithP2PType:(id)type config:(TuyaSmartCameraConfig *)ipcConfig delegate:(id<TuyaSmartCameraDelegate>)delegate;
```

#### P2P 类型

涂鸦 IPC SDK 支持三种 p2p 通道实现方案，SDK 会根据 p2p 类型来初始化不同的摄像头对象，通过下面的方式获取设备的 p2p 类型。

__Objective-C__

```objective-c
id p2pType = [deviceModel.skills objectForKey:@"p2pType"];
```

__Swift__

```swift
let p2pType = deviceModel.skills["p2pType"]
```

#### 示例代码

__Objective-C__

```objective-c
// deviceModel 为设备列表中的摄像机设备
id p2pType = [deviceModel.skills objectForKey:@"p2pType"];
[[TuyaSmartRequest new] requestWithApiName:@"tuya.m.ipc.config.get" postData:@{@"devId": deviceModel.devId} version:@"2.0" success:^(id result) {
    TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:[TuyaSmartUser sharedInstance].uid localKey:deviceModel.localKey configData:result];
    id<TuyaSmartCameraType> camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType config:config delegate:self];
} failure:^(NSError *error) {
    
}];
```

__Swift__

```swift
let p2pType = deviceModel.skills["p2pType"]
TuyaSmartRequest().request(withApiName: "tuya.m.ipc.config.get", postData: ["devId" : deviceModel.devId], version: "2.0", success: { result in
    let config = TuyaSmartCameraFactory.ipcConfig(withUid: TuyaSmartUser.sharedInstance().uid, localKey: deviceModel.localKey, configData: result as? [AnyHashable : Any])
    let camera = TuyaSmartCameraFactory.camera(withP2PType: p2pType!, config: config!, delegate: self)
}) { error in
    
}
```

```
注意，上面代码中，delegate 参数传入的 `self` 需要实现 TuyaSmartCameraDelegate 协议。
```
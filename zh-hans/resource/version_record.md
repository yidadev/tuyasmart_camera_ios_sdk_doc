# 版本更新记录

### 3.2.0

* SDK 拆分为 TuyaSmartCameraKit，TuyaSmartCameraBase, TuyaSmartCameraM，TuyaSmartCameraT。如果您不需要支持 p2p 1.0 ，则直接使用 ```pod "TuyaSmartCameraKit"```即可。
* TuyaSmartCameraDPManager 中增加获取 DP value 的同步方法，从 APP 端的缓存中获取。大多数 DP 都可以通过此方法获取到最新值，设备会主动上报。如 SD 卡容量，格式化进度等则需要下发查询。
* SDK 中不在提供 TuyaSmartCameraDefault 类，但可以从仓库中的 Example 程序中获取，也可参照其实现。
* "tuya.m.ipc.config.get" 接口需要升级到 2.0。
* TuyaSmartCameraFactory 增加几个属性，并增加 ```+ (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data; ``` 方法。uid 可暂时传入空字符串。configData 参数传入 "tuya.m.ipc.config.get" 2.0 版本接口返回的数据即可，可以获取到对应的 TuyaSmartCameraConfig 对象。
* 视频渲染视图增加 scaleToFill，设置为 YES，视频图像会自动拉伸铺满整个视图。
* 增加云存储视频播放功能。

### 3.1.1

* 提供摄像头功能的基础封装，接入更简单。
* 摄像头功能点封装。
* p2p 2.0 开放视频解码后的视频帧数据接口。
* 摄像头SDK拆分，可以根据需要选择接入 p2p 1.0 和 p2p 2.0 SDK。

### 3.1.0

* 根据 p2pType 创建不同版本的摄像头对象。
* 截图和录像保存在与APP同名的自定义相册中。
* 增加获取视频裸流接口。

# 版本更新记录

### TuyaSmartCameraBase 4.2.5、TuyaSmartCameraM 4.2.5、TuyaSmartCameraKit 4.3.2、TuyaCamera 3.13.3

* P2P 和 音视频解码库独立为 TuyaCamera 组件，改为动态库方式，以解决相关库版本冲突问题。
* 增加录制、截图，保存到指定文件路径的接口。

### TuyaSmartCameraM 4.0.4、TuyaSmartLogger 0.1.0

* 修复一些音视频相关问题。

* 增加本地日志输出文件功能，开发中遇到阻塞问题，可以接入 TuyaSmartLogger 库，将日志文件导出并在涂鸦工单平台提问，附件日志文件，详情参考[调试日志文件](https://tuyainc.github.io/tuyasmart_camera_ios_sdk_doc/zh-hans/resource/log_file.html)

### 4.0.2

* 修复偶现录制崩溃问题

### 4.0.1

* 增加回声消除功能
* 增加云存储视频播放功能

### 3.2.0

* SDK 拆分为 TuyaSmartCameraKit，TuyaSmartCameraBase, TuyaSmartCameraM，TuyaSmartCameraT。如果您不需要支持 p2p 1.0 ，则直接使用 ```pod "TuyaSmartCameraKit"```即可。
* TuyaSmartCameraDPManager 中增加获取 DP value 的同步方法，从 APP 端的缓存中获取。大多数 DP 都可以通过此方法获取到最新值，设备会主动上报。如 SD 卡容量，格式化进度等则需要下发查询。
* SDK 中不在提供 TuyaSmartCameraDefault 类，但可以从仓库中的 Example 程序中获取，也可参照其实现。
* "tuya.m.ipc.config.get" 接口需要升级到 2.0。
* TuyaSmartCameraFactory 增加几个属性，并增加工厂方法： 

    __Objective-C__
    
    ``` objectivec
    + (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data; 
    ```

    __Swift__
    
    ```swift
    func ipcConfig(withUid: String!, localKey: String!, configData: [AnyHashable : Any]!)
    ```
    

uid 可暂时传入空字符串。configData 参数传入 `"tuya.m.ipc.config.get"`  2.0 版本接口返回的数据即可，可以获取到对应的 TuyaSmartCameraConfig 对象。
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

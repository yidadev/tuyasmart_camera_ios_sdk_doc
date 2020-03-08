# 版本更新记录

## 文档更新记录

| 版本号 | 编写/修订说明 | 修订人 | 修订事件   |
| ------ | ------------- | ------ | ---------- |
| v1.0.0 | 创建          | 傅浪   | 2020/03/01 |



## SDK 更新记录

| 模块                | 版本   |
| ------------------- | ------ |
| TuyaCameraSDK       | 3.15.3 |
| TuyaSmartCameraBase | 4.4.2  |
| TuyaSmartCameraM    | 4.3.4  |
| TuyaSmartCameraKit  | 4.4.4  |

**更新说明**

* TuyaCamera 更名为 TuyaCameraSDK，如果有在 podfile 中指定 TuyaCamera 的版本号，需要改一下库名和版本。
* 去除掉了部分尚未标准化（文档中未介绍过）的类和接口。
* 报警消息、云存储事件中的图片改为不加密的状态。
* 修复一些稳定性问题。



| 模块                | 版本号 |
| ------------------- | ------ |
| TuyaSmartCameraBase | 4.2.6  |
| TuyaSmartCameraM    | 4.2.6  |
| TuyaCamera          | 3.13.4 |

**更新说明**

* 增加获取对讲时所采集的音频数据的接口，以供开发者对音频数据二次处理



| 模块                | 版本   |
| ------------------- | ------ |
| TuyaSmartCameraBase | 4.2.5  |
| TuyaSmartCameraM    | 4.2.5  |
| TuyaSmartCameraKit  | 4.3.2  |
| TuyaCamera          | 3.13.3 |

**更新说明**

* P2P 和 音视频解码库独立为 TuyaCamera 组件，改为动态库方式，以解决相关库版本冲突问题。
* 增加录制、截图，保存到指定文件路径的接口。



| 模块             | 版本  |
| ---------------- | ----- |
| TuyaSmartCameraM | 4.0.4 |
| TuyaSmartLogger  | 0.1.0 |

**更新说明**

* 修复一些音视频相关问题。
* 增加本地日志输出文件功能，开发中遇到阻塞问题，可以接入 TuyaSmartLogger 库，将日志文件导出并在涂鸦工单平台提问，附件日志文件，详情参考[调试日志文件](https://tuyainc.github.io/tuyasmart_camera_ios_sdk_doc/zh-hans/resource/log_file.html)



| 模块                | 版本  |
| ------------------- | ----- |
| TuyaSmartCameraBase | 4.0.2 |
| TuyaSmartCameraM    | 4.0.2 |

**更新说明**

* 修复偶现录制崩溃问题



| 模块                         | 版本  |
| ---------------------------- | ----- |
| TuyaSmartCameraBase          | 4.0.1 |
| TuyaSmartCameraM             | 4.0.1 |
| TuyaSmartCameraKit           | 4.0.1 |
| TYCameraCloudServicePanelSDK | 0.2.1 |

**更新说明**

* 增加回声消除功能
* 增加云存储视频播放功能
* 增加云存储服务购买页面 SDK 



| 模块                | 版本  |
| ------------------- | ----- |
| TuyaSmartCameraBase | 3.2.0 |
| TuyaSmartCameraM    | 3.2.0 |
| TuyaSmartCameraT    | 3.2.0 |
| TuyaSmartCameraKit  | 3.2.0 |

**更新说明**

* SDK 拆分为 TuyaSmartCameraKit，TuyaSmartCameraBase, TuyaSmartCameraM，TuyaSmartCameraT。如果您不需要支持 p2p 1.0 ，则直接使用 ```pod "TuyaSmartCameraKit"```即可。
* TuyaSmartCameraDPManager 中增加获取 DP value 的同步方法，从 APP 端的缓存中获取。大多数 DP 都可以通过此方法获取到最新值，设备会主动上报。如 SD 卡容量，格式化进度等则需要下发查询。
* SDK 中不在提供 TuyaSmartCameraDefault 类，但可以从仓库中的 Example 程序中获取，也可参照其实现。
* "tuya.m.ipc.config.get" 接口需要升级到 2.0。
* TuyaSmartCameraFactory 增加几个属性，并增加工厂方法： 

    __ObjC__
    
    ``` objc
    + (TuyaSmartCameraConfig *)ipcConfigWithUid:(NSString *)uid localKey:(NSString *)localKey configData:(NSDictionary *)data; 
    ```

    __Swift__
    
    ```swift
    func ipcConfig(withUid: String!, localKey: String!, configData: [AnyHashable : Any]!)
    ```
    

uid 可暂时传入空字符串。configData 参数传入 `"tuya.m.ipc.config.get"`  2.0 版本接口返回的数据即可，可以获取到对应的 TuyaSmartCameraConfig 对象。
* 视频渲染视图增加 `scaleToFill`属性，设置为 `YES`，视频图像会自动拉伸铺满整个视图。
* 增加云存储视频播放功能。

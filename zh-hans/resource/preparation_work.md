# 准备工作

TuyaSmartCamera 依赖于涂鸦智能iOS SDK，在接入此SDK时，需要先接入TuyaSmartHomeKit（点击[这里](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/)查看接入文档）。

在AppDelegate.m文件顶部，添加以下内容：

```objective-c
#import <TuyaSmartCamera/TuyaSmartCameraFactory.h>
```

在 [AppDelegate application:didFinishLaunchingWithOptions:] 方法中添加以下代码初始化SDK：（3.0.0版本废弃）

```objective-c
// 3.0.0 版本后废弃该方法。SDK 会在第一次创建摄像头的时候自动初始化。
[TuyaSmartCameraFactory initSDKs];
```

## 示例代码约定

在以下示例代码中，如果没有特殊说明，所有实例均位于`ViewController`类的实现文件中

```
@interface ViewController : UIViewController

@end

@implementation ViewController

// 所有示例代码均位于此处

@end
```

对于像`self.prop`这样的引用，我们约定`prop`属性在`ViewController`类中已经有了正确的实现。例如：

```
self.device = [[TuyaSmartDevice alloc] initWithDeviceId:@"your_device_id"];
```

若想让它正确执行，需要在当前的`ViewController`类中添加一个 `TuyaSmartDevice`属性：

```
@property (nonatomic, strong) TuyaSmartDevice *device;
```

以此类推。

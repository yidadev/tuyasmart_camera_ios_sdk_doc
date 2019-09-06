# 准备工作

TuyaSmartCamera 依赖于涂鸦智能iOS SDK，在接入此SDK时，需要先接入TuyaSmartHomeKit（点击[这里](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/)查看接入文档）。

在使用Camera的文件顶部，添加以下内容：

__Objective-C__

```objective-c
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```

__Swift__

``` swift
import TuyaSmartBaseKit
```

## 示例代码约定

在以下示例代码中，如果没有特殊说明，所有实例均位于`ViewController`类的实现文件中

__Objective-C__

```
@interface ViewController : UIViewController

@end

@implementation ViewController

// 所有示例代码均位于此处

@end
```

__Swift__

```swift
class ViewController: UIViewController {

}
```

对于像`self.prop`这样的引用，我们约定`prop`属性在`ViewController`类中已经有了正确的实现。例如：

__Objective-C__

``` objectivec
self.device = [[TuyaSmartDevice alloc] initWithDeviceId:@"your_device_id"];
```

__Swift__

``` swift
self.device = TuyaSmartDevice(deviceId: "your_device_id")d
```

若想让它正确执行，需要在当前的`ViewController`类中添加一个 `TuyaSmartDevice`属性：

__Objective-C__

``` objectivec
@property (nonatomic, strong) TuyaSmartDevice *device;
```

__Swift__

``` swift
var device: TuyaSmartDevice?
```

以此类推。

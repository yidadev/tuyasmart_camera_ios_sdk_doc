# Preparation Work

TuyaSmartCamera relies on the Tuya Smart iOS SDK, when accessing this SDK, you need to access TuyaSmartHomeKit (Click [Here](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/en/) to view the document) .

TuyaSmartCameraKit only contains the functional package for the camera device. The distribution function needs to be connected to TuyaSmartHomeKit. The camera p2p configuration information also needs to be obtained through the functions in TuyaSmartBaseKit.

Add blow code to AppDelegate.m top :

__Objective-C__

```objective-c
#import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
```

__Swift__

``` swift
import TuyaSmartCameraKit
```

## Example Code Appointment

Under below example code，if there is no special statement， all are under ViewController： 


__Objective-C__

```
@interface ViewController : UIViewController

@end

@implementation ViewController

// All sample code is located here

@end
```

__Swift__

```swift
class ViewController: UIViewController {

}
```

Like self.prop， we appoint prop property has right realization under ViewController，like：

__Objective-C__

``` objectivec
self.device = [[TuyaSmartDevice alloc] initWithDeviceId:@"your_device_id"];
```

__Swift__

``` swift
self.device = TuyaSmartDevice(deviceId: "your_device_id")
```

if we want it excute correctly，need to add one TuyaSmartDevice property in ViewController：


__Objective-C__

``` objectivec
@property (nonatomic, strong) TuyaSmartDevice *device;
```

__Swift__

``` swift
var device: TuyaSmartDevice?
```

The rest can be done in the same manner

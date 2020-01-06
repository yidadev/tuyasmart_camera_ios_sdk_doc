# Low-power Doorbell

A low-power doorbell will sleep after a few seconds without a P2P connection. It cannot be directly connected to P2P after hibernation. It needs to wake up first, and then connect to the P2P channel after waking up. 

__Objective-C__

```objective-c
self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];
// Add observer for DP 
[self.dpManager addObserver:self];
// if it is a low-power doorbell
- (BOOL)isDoorbell {
    return [self.dpManager isSupportDP:TuyaSmartCameraWirelessAwakeDPName];
}

- (void)start {
    if ([self isDoorbell]) {
        __weak typeof(self) weakSelf = self;
        // Get the status of the device, if it is awaking up.
		BOOL isAwaking = [[self.camera.dpManager valueForDP:TuyaSmartCameraWirelessAwakeDPName] boolValue];
        if (isAwaking) {// Get the status of the device, if it is awaking up.

            if (self.isConnected) {
                [self.videoContainer addSubview:self.camera.videoView];
                self.camera.videoView.frame = self.videoContainer.bounds;
                [self.camera startPreview];
            }else {
                [self.camera connect];
            }
		}else { // Is sleeping, send wake up command.
            [self.device awakeDeviceWithSuccess:nil failure:nil];
        }
    }
}
// TuyaSmartCameraDPObserver will call this method when dp is updated
- (void)cameraDPDidUpdate:(TuyaSmartCameraDPManager *)manager dps:(NSDictionary *)dpsData {
    // if the value of TuyaSmartCameraWirelessAwakeDPName is 'YES'，the device is waking up.
    if ([[dpsData objectForKey:TuyaSmartCameraWirelessAwakeDPName] boolValue]) {
        [self start];
    }
}

```

__Swift__

``` swift
self.dpManager = TuyaSmartCameraDPManager(deviceId: self.devId)
self.device = TuyaSmartDevice(deviceId: self.devId)
self.dpManager?.addObserver(self)

func isDoorbell() -> Bool {
    return self.dpManager?.isSupportDP(TuyaSmartCameraWirelessAwakeDPName)
}

func start() {
    if isDoorbell() {
            let isAwaking = self.camera.dpManager.valueForDP(TuyaSmartCameraWirelessAwakeDPName)
            guard isAwaking else {
                self.device?.awake(success: nil, failure: nil)
            }
            guard self.isConnected else {
                self.camera.connect()
            }
            self.videoContainer.addSubView(self.camera.videoView)
            self.camera.videoView.frame = self.videoContainer.bounds
            self.camera.startPreview()        
    }
}

// TuyaSmartCameraDPObserver will call this method when dp is updated
func cameraDPDidUpdate(_ manager: TuyaSmartCameraDPManager!, dps dpsData: [AnyHashable : Any]!) {
     // if the value of TuyaSmartCameraWirelessAwakeDPName is 'YES'，the device is waking up.
    if let aweak = dpsData[TuyaSmartCameraWirelessAwakeDPName] as? Bool, aweak == true {
        self.start()
    }
} 
    
```

### Doorbell call

When doorbell bind to home success, and doorbell is online, app will receive notification when someone rings the doorbell.

* Notification Name: **kNotificationMQTTMessageNotification**
* Params: 
  * **devId**: device id of doorbell
  * **etype**: Event identifier, ```doorbell``` indicates doorbell call

__Objective-C__

```objective-c
#define kTuyaDoorbellNotification @"kNotificationMQTTMessageNotification"

- (void)observeDoorbellCall:(void(^)(NSString *devId, NSString *type))callback {
    if (!callback) {
        return;
    }
    [[NSNotificationCenter defaultCenter] addObserverForName:kTuyaDoorbellNotification object:nil queue:nil usingBlock:^(NSNotification *note) {
        NSDictionary *eventInfo = note.object;
        NSString *devId = eventInfo[@"devId"];
        NSString *eType = [eventInfo objectForKey:@"etype"];
        if ([eType isEqualToString:@"doorbell"]) {
            callback(devId, eType);
        }
    }];
}
```

__Swift__

```swift
func obserDoorbellCall(_ callBack: @escaping (String, String) -> Void) {
    NotificationCenter.default.addObserver(forName: NSNotification.Name(rawValue: "kNotificationMQTTMessageNotification"), object: nil, queue: nil) { (noti) in
        if let eventInfo = noti.object as? [String: Any?] {
            let devId = eventInfo["devId"] as! String
            let eType = eventInfo["etype"] as! String
            if eType == "doorbell" {
                callBack(devId, eType)
            }
        }
    }
}
```


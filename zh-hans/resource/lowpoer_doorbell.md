# 低功耗门铃

低功耗门铃在一定时间内没有p2p连接会休眠，休眠后无法直接连接p2p，需要先唤醒，唤醒后再连接p2p通道。具体流程如下：

__Objective-C__

```objective-c
self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];
// 添加 DP 监听
[self.dpManager addObserver:self];
// 判断是否是低功耗门铃
- (BOOL)isDoorbell {
    return [self.dpManager isSupportDP:TuyaSmartCameraWirelessAwakeDPName];
}

- (void)start {
    if ([self isDoorbell]) {
        __weak typeof(self) weakSelf = self;
        // 获取设备的状态
		BOOL isAwaking = [[self.camera.dpManager valueForDP:TuyaSmartCameraWirelessAwakeDPName] boolValue];
        if (isAwaking) { // 唤醒状态下，直接连接p2p 或者 开始预览
            if (self.isConnected) {
                [self.videoContainer addSubview:self.camera.videoView];
                self.camera.videoView.frame = self.videoContainer.bounds;
                [self.camera startPreview];
            }else {
                [self.camera connect];
            }
        }else { // 休眠状态下，发送唤醒命令
            [self.device awakeDeviceWithSuccess:nil failure:nil];
        }
    }
}
// TuyaSmartCameraDPObserver 当设备 DP 有更新的时候，会触发这个监听回调
- (void)cameraDPDidUpdate:(TuyaSmartCameraDPManager *)manager dps:(NSDictionary *)dpsData {
    // 如果收到 TuyaSmartCameraWirelessAwakeDPName 的更新，并且值为 YES，表示设备已唤醒
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

// TuyaSmartCameraDPObserver 当设备 DP 有更新的时候，会触发这个监听回调
func cameraDPDidUpdate(_ manager: TuyaSmartCameraDPManager!, dps dpsData: [AnyHashable : Any]!) {
     // 如果收到 TuyaSmartCameraWirelessAwakeDPName 的更新，并且值为 YES，表示设备已唤醒
    if let aweak = dpsData[TuyaSmartCameraWirelessAwakeDPName] as? Bool, aweak == true {
        self.start()
    }
} 
    
```
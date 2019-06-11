# 低功耗门铃

低功耗门铃在一定时间内没有p2p连接会休眠，休眠后无法直接连接p2p，需要先唤醒，唤醒后再连接p2p通道。具体流程如下：

```objective-c
self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
// 添加 DP 监听
[self.dpManager addObserver:self];
// 判断是否是低功耗门铃
- (BOOL)isDoorbell {
    return [self.dpManager isSurpportDP:TuyaSmartCameraWirelessAwakeDPName];
}

- (BOOL)start {
    if ([self isDoorbell]) {
        __weak typeof(self) weakSelf = self;
        // 获取设备的状态
        [self.camera.dpManager valueForDP:TuyaSmartCameraWirelessAwakeDPName success:^(id result) {
            if ([result boolValue]) {
                // 未休眠，直接连接
                if (weakSelf.isConnected) {
                    [weakSelf.camera.videoView tuya_clear];
                    [weakSelf.videoContainer addSubview:weakSelf.camera.videoView];
                    weakSelf.camera.videoView.frame = weakSelf.videoContainer.bounds;
                    [weakSelf.camera startPreview];
                }else {
                    [weakSelf.camera connect];
                }
            }else {
                // 已经休眠，发送唤醒命令
                [weakSelf.device awakeDeviceWithSuccess:nil failure:nil];
            }
        } failure:nil];
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


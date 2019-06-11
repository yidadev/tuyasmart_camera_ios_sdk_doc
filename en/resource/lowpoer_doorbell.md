# Low-power Doorbell

A low-power doorbell will sleep after a few seconds without a P2P connection. It cannot be directly connected to P2P after hibernation. It needs to wake up first, and then connect to the P2P channel after waking up. 

```objective-c
self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
// Add observer for DP 
[self.dpManager addObserver:self];
// if it is a low-power doorbell
- (BOOL)isDoorbell {
    return [self.dpManager isSurpportDP:TuyaSmartCameraWirelessAwakeDPName];
}

- (BOOL)start {
    if ([self isDoorbell]) {
        __weak typeof(self) weakSelf = self;
        // Get the status of the device, if it is sleeping.
        [self.camera.dpManager valueForDP:TuyaSmartCameraWirelessAwakeDPName success:^(id result) {
            if ([result boolValue]) {
                // Is waking up, can connect.
                if (weakSelf.isConnected) {
                    [weakSelf.camera.videoView tuya_clear];
                    [weakSelf.videoContainer addSubview:weakSelf.camera.videoView];
                    weakSelf.camera.videoView.frame = weakSelf.videoContainer.bounds;
                    [weakSelf.camera startPreview];
                }else {
                    [weakSelf.camera connect];
                }
            }else {
                // Is sleeping, send wake up command.
                [weakSelf.device awakeDeviceWithSuccess:nil failure:nil];
            }
        } failure:nil];
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

